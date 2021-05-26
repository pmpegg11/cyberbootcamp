# cyberbootcamp
This is the repository for different things that I have created during my Cyber Security Bootcamp

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/pmpegg11/cyberbootcamp/blob/b0ae271eeb5a4569622146711990bd109e738860/Diagrams/My%20first%20Azure%20Network.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Ansible file may be used to install only certain pieces of it, such as Filebeat.

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network.
The Load balancer is to ensure that the incoming traffic is shared by both vulnerable web servers. Access controls will ensure that only authorised users will be able to connect to this network. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems of the VMs on the network and system metrics, such as CPU usage, sudo escalation failures, etc. 

The configuration details of each machine may be found below.

| Name     | Function   | IP Address | Operating System |
|----------|------------|------------|------------------|
| Jump Box | Gateway    | 10.0.0.1   | Linux            |
| Web-1    | Web Server | 10.1.0.160 | Linux            |
| Web-2    | Web Server | 10.1.0.170 | Linux            |
| Elk      | Monitoring | 10.0.0.4   | Linux            |

Additionally, a Load Balancer server has been placed in front of all machines, except for the Jump Box. There are two availability zones targeted by the Load Balancer:
Availability Set 1: Web-1 + Web-2
Availability Set 2: Elk 

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:114.75.165.190

Machines within the network can only be accessed by each other. The two Web-Servers VMs send traffic to the Elk server (10.0.0.4).

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 114.75.165.190       |
| Elk      | No                  | 10.1.0.1-254         |
| Web-1    | No                  |  10.1.0.1-254        |
| Wev-2    | No                  |  10.1.0.1-254        |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows IT administrators to automate daily tasks and maintenance of a network. 
This allows them to focus time on other important tasks.

The playbook implements the following tasks:
- Firstly it installs docker.io onto the Elk VM.
- It then installs Python3 onto the VM and installs a module with it.
- The playbook then increases the memory of the system in order for it to function normally. 
- The elk container is then downloaded and lauched.
- An addition to the playbook allows the container to start when rebooted as well. 

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/pmpegg11/cyberbootcamp/blob/b0ae271eeb5a4569622146711990bd109e738860/Ansible/Images/Playbook%20completed%20and%20docker%20successD1.PNG

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 and Web-2, which are located at the IPs of 10.1.0.160 and 10.1.0.170 respectivefully.

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat
- Packetbeat

These Beats allow us to collect the following information from each machine:
- Filebeat: Filebeat is used to detect changes to the filesystem, which is specifically used to collect Apache logs. 
- Metricbeat: Metricbeat detects changes in system metrics, such as CPU usage. We also use it to detect SSH login attempts, failed sudo escalations, and CPU/RAM statistics. 
- Packetbeat: This collects packets that pass through the NIC, similar to Wireshark. We use it to generate a trace of all activity that takes place on the network, in case later forensic analysis should be warranted. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook (be it from a webpage, or somewhere else) file to the '/etc/ansible' directory. This will ensure that the playbooks are located in the correct place.
- Update the hosts file to include the VMs in which we intended the playbook to run on. Specifically this file will detail which VM we intended to run the ELK server, and what VMs are to be targeted.  
- Run the playbook, and navigate to the address of Kibana (In this case it is http://10.0.0.4:5601) to check that the installation worked as expected.

- The easiest way to perfrom these steps is actually using Git. 

- Firstly we use the following commands to ensure that the playbook is located in the right area:

 $ cd /etc/ansible
 $ mkdir files
 # Clone Repository + IaC Files
 $ git clone https://github.com/yourusername/project-1.git
 # Move Playbooks and hosts file Into `/etc/ansible`
 $ cp project-1/playbooks/* .
 $ cp project-1/files/* ./files

- The next step is to modify the hosts file to ensure that the correct VMs are targeted for specific reason. When we modify the hosts file, we are looking at the following section:
 
 [webservers]
 10.1.0.160
 10.1.0.170

 [elk]
 10.0.0.4

- The last step is to run the playbook itself:

 $ cd /etc/ansible
 $ ansible-playbook install_elk.yml elk
 $ ansible-playbook install_filebeat.yml webservers
 $ ansible-playbook install_metricbeat.yml webservers

- After this step, we will want to wait a few minutes for Kibana to start. Once we have waited we can run the following command: 

 curl http://10.0.0.4:5601

#End
