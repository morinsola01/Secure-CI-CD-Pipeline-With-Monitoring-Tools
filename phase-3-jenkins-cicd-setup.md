<h1><strong>Phase 3: Jenkins CI/CD Setup</strong></h1>


In this phase, Jenkins was configured to automate the CI/CD process, including plugin installations, environment configurations, and building, testing, scanning, and deploying the application.

**Jenkins Plugin Setup**

The following plugins were installed to extend Jenkins capabilities:

- **Eclipse Temurin Installer**: For automated JDK installation and configuration.
- **Pipeline Maven Integration**: Enabled Maven commands within Jenkins Pipeline.
- **Config File Provider**: Allowed centralized management of configuration files.
- **SonarQube Scanner**: Integrated SonarQube for code quality and security scanning.
- **Kubernetes CLI & Kubernetes**: Facilitated interactions with Kubernetes clusters.
- **Docker & Docker Pipeline Step**: Managed Docker builds, containers, and integrated with Docker registries.

**Installed Plugins**
<img width="1440" alt="Screenshot 2024-10-23 at 10 12 39 PM" src="https://github.com/user-attachments/assets/0ba627b0-1cf4-4b39-9976-4226d42cb311">


**Jenkins Pipeline Configuration**

I configured the necessary credentials in Jenkins for GitHub, Docker, SonarQube, Nexus, and Kubernetes. These credentials were securely referenced in the pipeline using the **Pipeline Syntax** feature, ensuring a clean and secure integration with external services.

The Jenkins pipeline was scripted to handle the entire CI/CD workflow. The key stages included:

- **Git Checkout**: Automatically pulled code from the GitHub repository.
- **Compile**: Used Maven to compile the code.
- **Test**: Ran unit tests using Maven.
- **File System Scan**: Performed security scanning using Trivy.
- **SonarQube Analysis**: Integrated SonarQube for static code analysis and quality gate verification.
- **Build & Publish**: Packaged the application with Maven and published it to Nexus.
- **Docker Image Build & Scan**: Built and tagged Docker images, followed by Trivy image scans.
- **Push Docker Image**: Pushed the Docker image to DockerHub.
- **Deploy to Kubernetes**: Deployed the application to the Kubernetes cluster.
- **Verify Deployment**: Checked the status of the pods and services within Kubernetes.

**Pipeline**

<img width="1437" alt="Screenshot 2024-10-23 at 9 27 43 PM" src="https://github.com/user-attachments/assets/27e66d49-a3a7-41c7-a408-e0359a77da00">

<img width="1440" alt="Screenshot 2024-10-23 at 10 28 17 PM" src="https://github.com/user-attachments/assets/1a8ff0b9-8c5a-4f48-9292-0265ca7ea49e">



**Maven Releaser and Snapshot Configuration**

The pipeline was configured to support both **release** and **snapshot** builds using Nexus as the artifact repository. The process was managed through Maven with the following configurations:

- **Snapshots**: Builds that were not marked for release were published as snapshots to Nexus, using a snapshot repository. This allowed for incremental versions to be tested and reviewed without requiring a full release.
- **Releases**: When ready for production, the Maven build was configured to publish release versions to the Nexus release repository. This was triggered by the `mvn deploy` command in the pipeline, ensuring that only stable, tested builds were released.

The integration between Nexus and Jenkins ensured that each build, whether snapshot or release, was tracked and versioned properly, enabling efficient artifact management throughout the development cycle.

**Nexus Repository**

<img width="1440" alt="Screenshot 2024-10-23 at 9 32 33 PM" src="https://github.com/user-attachments/assets/fd32a8b6-b063-42f0-83b1-aac591de8eb0">

<img width="1440" alt="Screenshot 2024-10-23 at 10 29 37 PM" src="https://github.com/user-attachments/assets/0d2d257c-d9ff-4660-8412-d2cf29aeee93">


**SonarQube Analysis and Quality Gate**

During the pipeline, SonarQube was used to perform static code analysis, checking for potential vulnerabilities and ensuring code quality. The pipeline included a **Quality Gate** that blocked the process if the code did not meet predefined quality standards.

The stages were configured as follows:

- **SonarQube Scan**: Triggered after the `mvn compile` stage, SonarQube scanned the project and uploaded the results to the SonarQube server.
- **Quality Gate Check**: The pipeline awaited the result of the quality gate. If the code failed the check, the pipeline was aborted, preventing low-quality or insecure code from moving forward.

**SonarQube**
<img width="1440" alt="Sonarqube" src="https://github.com/user-attachments/assets/c3c7a50f-2db1-4ad1-b6f1-4ff3ba04cd91">
<img width="1440" alt="Screenshot 2024-10-23 at 11 15 11 PM" src="https://github.com/user-attachments/assets/c0d1a03c-e0e4-4bc8-971a-593c08b4103d">




**Jenkins Pipeline Script**

```groovy
pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'maven3'
    }

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'git-cred', url: 'https://github.com/morinsola01/secure-cicd.git'
            }
        }

        stage('Compile') {
            steps {
                sh "mvn compile"
            }
        }

        stage('Test') {
            steps {
                sh "mvn test"
            }
        }

        stage('File System Scan') {
            steps {
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner \
                            -Dsonar.projectName=secure-cicd \
                            -Dsonar.projectKey=secure-cicd \
                            -Dsonar.java.binaries=. '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }

        stage('Build') {
            steps {
                sh "mvn package"
            }
        }

        stage('Publish To Nexus') {
            steps {
                withMaven(globalMavenSettingsConfig: 'global-settings', jdk: 'jdk17', maven: 'maven3', mavenSettingsConfig: '', traceability: true) {
                    sh "mvn deploy"
                }
            }
        }

        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t momustapha/secure-cicd:latest ."
                    }
                }
            }
        }

        stage('Docker Image Scan') {
            steps {
                sh "trivy image --format table -o trivy-image-report.html momustapha/secure-cicd:latest"
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push momustapha/secure-cicd:latest"
                    }
                }
            }
        }

        stage('Deploy To Kubernetes') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: '', credentialsId: 'k8-cred', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://172.31.93.110:6443') {
                    sh "kubectl apply -f deployment-service.yaml"
                }
            }
        }

        stage('Verify the Deployment') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: '', credentialsId: 'k8-cred', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://172.31.93.110:6443') {
                    sh "kubectl get pods -n webapps"
                    sh "kubectl get svc -n webapps"
                }
            }
        }
    }

    post {
        always {
            script {
                def jobName = env.JOB_NAME
                def buildNumber = env.BUILD_NUMBER
                def pipelineStatus = currentBuild.result ?: 'UNKNOWN'
                def bannerColor = pipelineStatus.toUpperCase() == 'SUCCESS' ? 'green' : 'red'

                def body = """
                    <html>
                    <body>
                    <div style="border: 4px solid ${bannerColor}; padding: 10px;">
                    <h2>${jobName} - Build ${buildNumber}</h2>
                    <div style="background-color: ${bannerColor}; padding: 10px;">
                    <h3 style="color: white;">Pipeline Status: ${pipelineStatus.toUpperCase()}</h3>
                    </div>
                    <p>Check the <a href="${BUILD_URL}">console output</a>.</p>
                    </div>
                    </body>
                    </html>
                """

                emailext (
                    subject: "${jobName} - Build ${buildNumber} - ${pipelineStatus.toUpperCase()}",
                    body: body,
                    to: 'omorinsolamustapha@gmail.com',
                    from: 'jenkins@example.com',
                    replyTo: 'jenkins@example.com',
                    mimeType: 'text/html',
                    attachmentsPattern: 'trivy-image-report.html'
                )
            }
        }
    }
}
