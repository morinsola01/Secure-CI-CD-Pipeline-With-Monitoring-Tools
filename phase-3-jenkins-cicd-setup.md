### Phase 3: Jenkins CI/CD Setup


In this phase, Jenkins was configured to automate the CI/CD process, including plugin installations, environment configurations, and building, testing, scanning, and deploying the application.


#### Jenkins Plugin Setup

The following plugins were installed to extend Jenkins capabilities:

- **Eclipse Temurin Installer**: For automated JDK installation and configuration.
- **Pipeline Maven Integration**: Enabled Maven commands within Jenkins Pipeline.
- **Config File Provider**: Allowed centralized management of configuration files.
- **SonarQube Scanner**: Integrated SonarQube for code quality and security scanning.
- **Kubernetes CLI & Kubernetes**: Facilitated interactions with Kubernetes clusters.
- **Docker & Docker Pipeline Step**: Managed Docker builds, containers, and integrated with Docker registries.

![Installed Plugins](<img width="1440" alt="Screenshot 2024-10-23 at 10 12 39 PM" src="https://github.com/user-attachments/assets/b4f89338-86d0-44ce-9d81-1d91998195e7">
)


#### Jenkins Pipeline Configuration

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

![Pipeline](<img width="1440" alt="Screenshot 2024-10-23 at 10 07 42 PM" src="https://github.com/user-attachments/assets/e634c0fd-9be0-4d45-86e5-3037300f89e6">
)


#### Jenkins Pipeline Script

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
