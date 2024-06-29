# MULTI-TIER-WEB-APP-PROJECT SETUP![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.001.png)
This project is an __educational tool__ designed to demonstrate the integration and deployment of a multi-tier application using Vagrant. 

It helps to understand how to set up and manage a complex application stack, __mimicking a real-world production environment__. 

In Production, we use __Docker, Kubernetes, Ansible, Chef, Puppet, SaltStack, Terraform and Cloud Provider Tools__.


## Example Scenario

In a real-world production environment, you might have:

- A dedicated database server running MySQL.
- A separate server running Memcached for caching.
- Another server for RabbitMQ handling messaging.
- An application server running a Tomcat instance with your application.
- A web server running Nginx serving as a reverse proxy.

The Vagrantfile sets up a similar structure with :

- __Nginx as Web Server :__ that acts as a reverse proxy to route requests to the application server and Serves static content.
- __Tomcat as Application Server :__ that hosts the core application logic, typically a Java-based web application.
- __MySQL as Database Server :__ that manages data storage and retrieval for the application.
- __Memcached as Cache Server :__ that provides caching to improve performance by storing frequently accessed data in memory.
- __RabbitMQ as Message Broker :__ that handles messaging and task queues, enabling asynchronous communication between different parts of the application.


## Setup and Run the Project
To set up and run the vprofile-project, you would typically:

- [x] Install VirtualBox and Vagrant on your development machine. [Prereqs_doc.pdf](https://github.com/user-attachments/files/16040138/Prereqs_doc.pdf)

- [x] Execute below command in your computer to install hostmanager plugin
```
vagrant plugin install vagrant-hostmanager
```
- [x] Clone the project repository from GitHub to your local machine.
- [x] Open a terminal and navigate to the directory containing the Vagrantfile.
- [x] Start the VMs : Run
```
vagrant up
```
to start and provision the VMs as defined in the Vagrantfile.
- [x] Access the VMs: Use
```
vagrant ssh <vm-name>
```
to access individual VMs for further configuration or troubleshooting.
- [ ] Test the Setup: Ensure that all components are running and communicating correctly by accessing the web server and checking if the application is working as expected.

## Services Launched :
* Host Machine (with Vagrant)
* ├── VM1: web01 (Nginx, IP: 192.168.56.11)
* ├── VM2: app01 (Tomcat, IP: 192.168.56.12)
* ├── VM3: db01 (MySQL, IP: 192.168.56.15)
* ├── VM4: mc01 (Memcached, IP: 192.168.56.14)
* └── VM5: rmq01 (RabbitMQ, IP: 192.168.56.13)



## Setting up each stack :
Follow the rest of the tutorial here [__VprofileProjectSetupWindowsAndMacIntel.pdf__](./VprofileProjectSetupWindowsAndMacIntel.pdf).

## Vagrant commands summarized:
```
Start the VM: vagrant up
Stop the VM: vagrant halt
Suspend the VM: vagrant suspend
Resume the VM: vagrant resume
Destroy the VM: vagrant destroy
```
