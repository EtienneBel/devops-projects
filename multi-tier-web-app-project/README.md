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

The Vagrantfile sets up a similar structure with VMs named db01, mc01, rmq01, app01, and web01, each performing these specific roles :

- __Web Server (Nginx - web01) :__ Acts as a reverse proxy to route requests to the application server and Serves static content.
- __Application Server (Tomcat - app01) :__ Hosts the core application logic, typically a Java-based web application.
- __Database Server (MySQL - db01) :__ Manages data storage and retrieval for the application.
- __Cache Server (Memcached - rmq01) :__ Provides caching to improve performance by storing frequently accessed data in memory.
- __Message Broker (RabbitMQ - mc01) :__ Handles messaging and task queues, enabling asynchronous communication between different parts of the application.


## Network
![image](https://github.com/EtienneBel/devops-projects/assets/49534121/9e426a79-e1d8-4bda-b2b8-9b05d20801b5)


## Setup and Running the Project
To set up and run the vprofile-project, you would typically:

- [x] Install Vagrant and VirtualBox on your development machine.
- [x] Clone the project repository from GitHub to your local machine.
- [x] Open a terminal and navigate to the directory containing the Vagrantfile.
- [x] Start the VMs : Run `vagrant up` to start and provision the VMs as defined in the Vagrantfile.
- [x] Access the VMs: Use `vagrant ssh <vm-name>` to access individual VMs for further configuration or troubleshooting.
- [ ] Test the Setup: Ensure that all components are running and communicating correctly by accessing the web server and checking if the application is working as expected.

## Vagrant commands summarized:
```
Start the VM: vagrant up
Stop the VM: vagrant halt
Suspend the VM: vagrant suspend
Resume the VM: vagrant resume
Destroy the VM: vagrant destroy
```
