<h1><strong>Phase 2: Application Repository Setup and Jenkins Integration</strong></h1>

In this phase, I set up a **private Git repository** and linked it to Jenkins to automate the CI/CD pipeline.

**1. Creating and Connecting the Private Git Repository**

- **Private Repository Creation**: I created a private Git repository on GitHub to securely host the application code. This ensures that the project source remains protected while allowing easy integration with other tools.
  
- **Personal Access Token (PAT) Setup**: To enable secure access from Jenkins, I generated a **Personal Access Token (PAT)** with appropriate permissions for repository access. This token acts as a secure way to authenticate Jenkins for automated tasks like pulling code and pushing changes.

- **Cloning the Repository**: Using Git Bash, I cloned the repository locally with:
  
    ```bash
    git clone <repository_URL>
    ```

    This created a local version of the repository where I added the source code and DevSecOps configurations.

**Repository Setup**

<img width="1440" alt="Screenshot 2024-10-23 at 9 53 04 PM" src="https://github.com/user-attachments/assets/686b5819-86a0-43cd-8295-d14bb82ef3a5">


**2. Connecting GitHub to Jenkins**

- **Jenkins Integration**: I connected the GitHub repository to Jenkins by configuring Jenkins to use the PAT for repository access. This enabled automatic triggers for the CI/CD pipeline whenever changes were pushed to the repository.

- **Jenkins Pipeline Configuration**: I created a Jenkins pipeline that automates the build, test, and deploy processes. The pipeline was set to trigger on every push to the GitHub repository, ensuring continuous integration and continuous delivery.

**Jenkins Pipeline**

<img width="1437" alt="Screenshot 2024-10-23 at 9 27 43 PM" src="https://github.com/user-attachments/assets/4144e69b-f3c7-4d29-ab40-d2ab65746840">


By establishing this setup, I ensured that the codebase was securely managed and continuously integrated into the CI/CD process, paving the way for automated testing, security scanning, and deployment in the later phases.

