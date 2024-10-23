# Phase 1: AWS VM and Tool Setup

## Introduction
This document outlines the steps to set up an Ubuntu EC2 instance, configure a Kubernetes cluster, and install Jenkins, Nexus, and SonarQube.

---

## 1. Creating an Ubuntu EC2 Instance on AWS

### 1.1 Sign in to the AWS Management Console
- **Step 1:** Open your web browser and navigate to the [AWS Management Console](https://aws.amazon.com/console/).
  - ![AWS console login page](https://github.com/morinsola01/Secure-CI-CD-Pipeline-With-Monitoring-Tools/issues/1#issue-2609689763)
- **Step 2:** Enter your AWS account credentials and click "Sign In".

### 1.2 Launching an EC2 Instance
1. **Navigate to the EC2 Dashboard:**
   - **Step 1:** In the AWS Management Console, search for "EC2" and select it.
   - ![AWS console showing EC2 service selection](link-to-screenshot)

2. **Create a New Instance:**
   - **Step 2:** Click on "Instances" in the left sidebar.
   - **Step 3:** Click the "Launch Instance" button.
   - ![EC2 dashboard with "Launch Instance" highlighted](link-to-screenshot)

3. **Select an Amazon Machine Image (AMI):**
   - **Step 4:** Choose "Ubuntu" (e.g., Ubuntu Server 20.04 LTS) and click "Select".
   - ![AMI selection screen showing Ubuntu options](link-to-screenshot)

4. **Choose an Instance Type:**
   - **Step 5:** Select the instance type (e.g., `t2.micro` for free tier).
   - ![Instance type selection with `t2.micro` highlighted](link-to-screenshot)

5. **Configure Instance Details:**
   - **Step 6:** Configure network settings as needed and click "Next: Add Storage".
   - ![Instance configuration screen](link-to-screenshot)

6. **Add Storage:**
   - **Step 7:** Adjust the root volume size if necessary.
   - ![Storage configuration page](link-to-screenshot)

7. **Add Tags:**
   - **Step 8:** Optionally add tags for organizational purposes.
   - ![Tagging screen](link-to-screenshot)

8. **Configure Security Group:**
   - **Step 9:** Allow SSH access (port 22) from your IP address, and add HTTP (port 80) and HTTPS (port 443) if required.
   - ![Security group configuration with SSH, HTTP, and HTTPS rules](link-to-screenshot)

9. **Review and Launch:**
   - **Step 10:** Check the configuration and click "Launch". Select or create a key pair for SSH access.
   - ![Review and launch instance page](link-to-screenshot)

10. **Access the EC2 Instance:**
    - **Step 11:** Use SSH to connect to your instance using the key pair.
    - ![Terminal window showing SSH command](link-to-screenshot)

---

## 2. Setting Up Kubernetes Cluster

### 2.1 Update System Packages
- **Step 1:** Run the following command on both master and worker nodes:
  ```bash
  sudo apt-get update
