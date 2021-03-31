## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

(Images/Azure_Network_Diagram_Project.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the ansible playbook file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: Enter the playbook file._

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting traffic to the network. BY distributing HTTP traffics between webservers, it allows for traffic control and a reduced chance of the network crashing and being overwhelmed by cybersecurity attacks such as DoS. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system metrics. Such as Filebeat which watches for activity within the network and Metricbeats which monitors the system health and utilization. 

The configuration details of each machine may be found below.

| Name     | Function   | IP Address   | Operating System |   
|----------|------------|--------------|------------------|
| Jump Box | Gateway    | 52.183.67.96 | Linux            |   
| Web-1    | Webserver  | 10.0.0.6     | Linux            |   
| Web-2    | Webserver  | 10.0.0.7     | Linux            |   
| Web-3    | Webserver  | 10.0.0.9     | Linux            |   
| Elk      | Monitoring | 10.1.04      | Linux            |   
### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- the users home ip address
Machines within the network can only be accessed by SSH connection via Jumpbox.
The machine that has access to the Elk VM is the jumpbox, who’s IP is 52.183.67.96.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Address |
|----------|---------------------|--------------------|
| Jump Box | Yes                 | home/user ip       |
| Web-1    | No                  | 10.0.0.1-.254      |
| Web-2    | No                  | 10.0.0.1-.254      |
| Web-3    | No                  | 10.0.0.1-.254      |
| Elk      | No                  | 10.0.0.1-.254      |
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it streamlines the configuration of multiple servers in a single deployment which increases the ease of maintenance, reducing the chance of internal errors. Patches can be updated easily in the case of misconfigurations with ansible-playbooks. 

The playbook implements the following tasks:
- Installs docker.io – it references the IP address listed under elk in ansible’s hosts file to install docker on the target VM
- Increase virtual memory – a standard container does not have enough virtual memory to run an ELK container 
- Install pip3
- Install Docker python module
- Launch Docker and enable service Docker on boot – downloads and launches the ELK container, and lists the ports needed to access applications 

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
(Images/Docker_PS.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
-10.0.0.6
-10.0.0.7
-10.0.0.8

We have installed the following Beats on these machines:
- Filebeats
- Metricbeats

These Beats allow us to collect the following information from each machine:
- Filebeats is collecting syslogs. 
- Metricbeats is collecting metrics from the operating and services that are running on the server.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the config file to the /etc/filebeat.
- Update the config file to include port and ip address of the service you’re using it on. For example, Kibana is port 5601.
- Run the playbook, and navigate to module status to check that the installation worked as expected.

The filebeat-playbook.yml is copied over to the /etc/ansible where the hosts file is located. This is prevalent since it is the host file that needs to be updated in order to ensure Ansible to run the playbook on specific machines. In order to specify which machines install Elk versus the ones that install Filebeat, we edit the configuration file for that. In order to check if the ELK server is running properly we navigate to the URL: <elk server IP>:5601/app/kibana. If the page loads then the server is loaded properly. 

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
Download the playbook config:
Curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > filebeat-config.yml

Refer to filebeat-playbook.yml for commands needed to update the proper files

To run the playbook utilize the command: ansible-playbook filebeat-playbook.yml
