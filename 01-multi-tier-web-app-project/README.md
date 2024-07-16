
# Multi-Tier Web Application Setup Readme

## About the Project
This project demonstrates the setup of a multi-tier web application stack on a local machine. It aims to simplify the process of setting up various services required for a web application, such as databases and web servers, making it repeatable and automated.

## Scenario
In many development projects, a variety of services power the application runtime, including databases like MySQL or PostgreSQL, and web services like Apache Tomcat or JBoss. Setting up these services locally can be complex, time-consuming, and hard to replicate consistently across different environments.

## Problem
Setting up a local development environment is often:
- **Complex**: Involving multiple configurations and dependencies.
- **Time-consuming**: Requiring manual installation and configuration.
- **Non-repeatable**: Difficult to replicate the exact setup across different machines or environments.

## Solution
This project provides a solution to create a local setup that is:
- **Repeatable**: Ensures consistent environments across different setups.
- **Automated**: Reduces the manual effort required for setup.
- **Infrastructure as Code (IaC)**: Uses code to define and manage the infrastructure.

## Tools Used
- **Hypervisor**: Oracle Virtual Machine (VirtualBox)
- **Automation**: Vagrant
- **CLI**: Git Bash

## Architecture
The architecture diagram outlines a typical multi-tier application setup with the following components:
1. **Load Balancer (NGINX)**: Distributes incoming traffic to the application servers.
2. **Application Server (Apache Tomcat)**: Hosts the web application.
3. **Message Queue (RabbitMQ)**: Handles asynchronous communication between services.
4. **Database (MySQL)**: Stores application data.
5. **Caching Layer (Memcached)**: Provides fast in-memory caching to improve performance.
![image](https://github.com/user-attachments/assets/df52f605-01d2-430d-a6b0-c9168b8ca53d)




## Getting Started
To set up the environment locally, follow these steps:

1. **Install VirtualBox**: Download and install Oracle VirtualBox from the official website.
2. **Install Vagrant**: Download and install Vagrant from the official website.
3. **Set up Git Bash**: Ensure you have Git Bash installed for command line operations.

Guide to install VirtualBox and Vagrant on your development machine. [Prereqs_doc.pdf](https://github.com/user-attachments/files/16040138/Prereqs_doc.pdf)

## Steps to Set Up
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/EtienneBel/devops-projects/tree/master/01-multi-tier-web-app-project
   cd 01-multi-tier-web-app-project
   ```
2. **Initialize Vagrant**:

   This command will install hostmanager plugin.
   ```bash
   vagrant plugin install vagrant-hostmanager
   ```
   This command will spin up the virtual machines as defined in the Vagrantfile.
   
   ```bash
   vagrant up
   ```

3. **Setting up each stack**:

* Follow the rest of the tutorial here [__VprofileProjectSetupWindowsAndMacIntel.pdf__](./VprofileProjectSetupWindowsAndMacIntel.pdf).
* Once the setup is complete, you can access the application through the load balancer's IP address or domain.


## Future Improvements
- Integrate additional services as required by the project.
- Enhance automation scripts for more complex setups.
- Explore containerization solutions like Docker for an even more portable setup.

---

This readme provides an overview of setting up a multi-tier web application stack locally, focusing on automation and repeatability. For detailed instructions and additional configurations, refer to the project documentation.
