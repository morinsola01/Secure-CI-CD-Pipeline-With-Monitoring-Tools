# Phase 1: AWS VM and Tool Setup

## Overview
In this phase, I focused on setting up the necessary AWS infrastructure and tools to support the development and deployment my application. The goal was to establish a solid foundation for a Continuous Integration and Continuous Deployment (CI/CD) pipeline.

## Key Steps

### 1. AWS EC2 Instance Setup
- Launched an Amazon EC2 instance using Ubuntu Server 22.04 LTS.
- Configured security groups and VPC settings for secure access.
<img width="1081" alt="Screenshot 2024-10-23 at 9 04 45 PM" src="https://github.com/user-attachments/assets/8c878897-03ff-4475-8cb5-73229c4f0e03">

### 2. Networking Configuration
- Set up the Virtual Private Cloud (VPC) to manage the network environment.
- Established appropriate inbound and outbound rules in the security groups to ensure secure communication.

### 3. Docker Installation
- Installed Docker on the EC2 instance to facilitate containerization of the Flask application.

### 4. Flask Application Setup
- Developed a basic Flask application to serve as the backend for the project.

### 5. Testing the Setup
- Verified that the EC2 instance and Docker installation were functioning correctly.
- Successfully ran the Flask application within a Docker container.

## Screenshots
- ![EC2 Instance Setup](https://your-link-to-screenshot.com/ec2-setup.png)
- ![VPC Configuration](https://your-link-to-screenshot.com/vpc-setup.png)
- ![Docker Installation](https://your-link-to-screenshot.com/docker-installation.png)
- ![Flask App Running](https://your-link-to-screenshot.com/flask-app-running.png)

## Conclusion
Phase 1 laid the groundwork for the subsequent stages of the project by ensuring that all necessary components were in place and functioning as intended. This setup is crucial for the implementation of the CI/CD pipeline in the following phases.
