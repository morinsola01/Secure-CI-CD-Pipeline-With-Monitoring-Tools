<h1><strong>Phase 1: AWS VM and Tool Setup</strong></h1>

In this phase, I provisioned and configured the core infrastructure for the CI/CD pipeline on AWS. This involved setting up virtual machines, Kubernetes, Jenkins, Nexus, and SonarQube, all while ensuring a secure and scalable environment for the pipeline.


**1. Provisioning AWS EC2 Instances**
I created Ubuntu-based EC2 instances using the AWS Management Console. Security groups were configured to restrict access to essential ports (SSH, HTTP) to ensure secure access.

**AWS EC2 Setup**

<img width="1434" alt="Screenshot 2024-10-23 at 9 18 31 PM" src="https://github.com/user-attachments/assets/0365ddfd-8eb6-483a-999f-2f8c75550b22">

**AWS Security Group Setup**

<img width="1081" alt="Screenshot 2024-10-23 at 9 04 45 PM" src="https://github.com/user-attachments/assets/b519bc04-8837-4bc7-b3e8-1b61d44e68b9">



**2. Kubernetes Cluster Setup (v1.28.1)**
I set up a Kubernetes cluster by creating three Ubuntu EC2 instances—one master node and two worker nodes. After initializing the Kubernetes master node, the two worker nodes were joined to form a functional cluster using `kubeadm`.

- **Master Node Initialization**: The master node was initialized with **Calico** for networking, and NGINX was deployed as the Ingress controller to handle external access to services.

- **Node Joining**: Both worker nodes were successfully joined to the master node, creating a scalable environment ready for application deployment.

**Kubernetes Cluster Nodes**

<img width="559" alt="Screenshot 2024-10-23 at 9 22 35 PM" src="https://github.com/user-attachments/assets/10185f23-de97-4c88-80d2-8e62c4fe5588">


**3. Jenkins Setup**
I deployed **Jenkins** using a Docker container to manage the CI/CD pipeline. By containerizing Jenkins, I ensured easy portability and scalability.

- **Deploying Jenkins**: I pulled the official Jenkins Docker image and ran the container, exposing it on port 8080.

    ```bash
    docker run -d --name jenkins -p 8080:8080 jenkins/jenkins:lts
    ```

- **Retrieving Jenkins Initial Password**: Once Jenkins was up and running, I accessed the initial admin password stored in the Docker container. This password is required for the first-time setup.

    To get the password, I executed the following commands:
    
    ```bash
    docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword
    ```

    This command retrieved the password from Jenkins' secret directory inside the Docker container.

- **Changing the Password**: After logging into Jenkins using the initial password, I went ahead and set up a new, secure admin password to customize the instance for my pipeline needs.

**4. Nexus Repository Manager Setup**
Using Docker, I deployed Nexus to manage dependencies and build artifacts, which is critical for version control and reliable software delivery. I accessed Nexus via port 8081 and ensured secure access controls were in place.
- Retrieved the admin password from the container for the initial configuration.

**Nexus Setup**

<img width="1440" alt="Screenshot 2024-10-23 at 9 32 33 PM" src="https://github.com/user-attachments/assets/5f208ec0-4272-4650-bd5a-e64881326ac5">


**5. SonarQube Setup**
SonarQube was deployed via Docker to enable static code analysis and maintain code quality. This integration with Jenkins provided real-time feedback on code vulnerabilities and quality checks within the pipeline.

**SonarQube**

<img width="1435" alt="Screenshot 2024-10-23 at 9 33 44 PM" src="https://github.com/user-attachments/assets/9a4a190f-c445-4e57-9852-12d73647294e">


---
**Next Steps: Application Setup and CI/CD Pipeline Configuration**

This setup provided a solid foundation for the upcoming phases, which involve linking the application repository to Jenkins, automating CI/CD processes, and ensuring system observability through integrated monitoring tools.
