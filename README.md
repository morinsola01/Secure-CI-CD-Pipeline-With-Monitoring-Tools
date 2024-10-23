<h1><strong>Secure CI/CD Pipeline with Monitoring Tools</strong></h1>
**Project Overview**
This project demonstrates the setup of a secure CI/CD pipeline using Kubernetes, Jenkins, SonarQube, and Nexus, hosted on AWS virtual machines. It also integrates monitoring tools like Grafana, Prometheus, and Blackbox Exporter for real-time infrastructure and service monitoring.

Table of Contents
Project Phases
Tools Used
Pipeline Architecture
Screenshots
Conclusion
Project Phases
Phase 1: AWS VM and Tool Setup
Deployed AWS Ubuntu virtual machines.
Configured security groups for secure VM access.
Installed and configured Kubernetes for container orchestration.
Set up Jenkins for continuous integration and continuous delivery (CI/CD).
Configured Nexus as a repository manager for dependencies and artifacts.
Integrated SonarQube for static code analysis and security scanning.
Phase 2: Application Setup
Created a GitHub repository for the application code.
Linked the repository to Jenkins for automatic pipeline integration.
Phase 3: Jenkins CI/CD Setup
Configured Jenkins pipelines to automate CI/CD processes.
Implemented security features within the pipeline, including:
Static Application Security Testing (SAST)
Dynamic Application Security Testing (DAST)
Linked Jenkins to GitHub for repository automation.
Phase 4: Monitoring Tools Setup
Set up Prometheus and Grafana for system and application monitoring.
Configured Blackbox Exporter to monitor the availability of services.
Created Grafana dashboards for real-time monitoring and alerts.
Tools Used
AWS EC2: Infrastructure setup for virtual machines.
Kubernetes: Container orchestration for application management.
Jenkins: CI/CD automation tool for building and deploying the application.
Nexus: Repository manager for storing dependencies and artifacts.
SonarQube: Tool for analyzing code quality and security.
Prometheus: Monitoring tool for metrics collection.
Grafana: Visualization and dashboard tool for real-time monitoring.
Blackbox Exporter: Service monitoring tool for HTTP, TCP, and DNS endpoints.
Pipeline Architecture

The CI/CD pipeline follows these steps:

Code pushed to GitHub triggers the Jenkins pipeline.
Jenkins runs code quality checks using SonarQube.
Nexus manages build artifacts.
Deployment of applications via Kubernetes.
Prometheus and Grafana monitor the infrastructure and application health.
Screenshots
Here are some screenshots of key steps and configurations:

AWS VM Setup: Screenshot
Jenkins Pipeline: Screenshot
SonarQube Code Analysis: Screenshot
Grafana Monitoring Dashboard: Screenshot
Conclusion
This project showcases how to create a robust, secure CI/CD pipeline with comprehensive monitoring. The integration of security tools and monitoring solutions ensures the pipeline is not only automated but also secure and observable.
