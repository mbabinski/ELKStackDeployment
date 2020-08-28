# ELKStackDeployment
Deploy ELK Stack in Azure
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![ELKStackDeployment](https://github.com/mbabinski/ELKStackDeployment/blob/master/Images/Network%20Diagram%20(Full).png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.
- install-elk.yml
- files/filebeat-config.yml
- files/metricbeat-config.yml
- roles/filebeat-playbook.yml
- roles/metricbeat-playbook.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting public access to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file system logs and system metrics.

The configuration details of each machine may be found below.

| Name               | Function  | IP Address | Operating System   |
|--------------------|-----------|------------|--------------------|
| JumpboxProvisioner | Gateway   | 10.0.0.4   | Linux Ubuntu 18.04 |
| Web-1              | DVWA      | 10.0.0.7   | Linux Ubuntu 18.04 |
| Web-2              | DVWA      | 10.0.0.6   | Linux Ubuntu 18.04 |
| web-3              | DVWA      | 10.0.0.8   | Linux Ubuntu 18.04 |
| ElkVM              | ELK Stack | 10.1.0.4   | Linux Ubuntu 18.04 |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpboxProvisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 24.21.164.120

Machines within the network can only be accessed by the admin host (IP address 24.21.164.120) via port 22 (SSH) and 5601 (Kibana).

A summary of the access policies in place can be found in the table below.

| Name               | Publicly Accessible             | Allowed IP Addresses |
|--------------------|---------------------------------|----------------------|
| JumpboxProvisioner | No (SSH from admin only)        | 24.21.164.120        |
| Web-1              | Yes (via load balancer only)    | 13.90.31.252         |
| Web-2              | Yes (via load balancer only)    | 13.90.31.252         |
| web-3              | Yes (via load balancer only)    | 13.90.31.252         |
| ElkVM              | No (SSH/Kibana from admin only) | 24.21.164.120        |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because this approach is faster, more reliable, repeatable, and less prone to erroneous configuration.

The playbook implements the following tasks:
- Installs docker.io using apt-get
- Installs python3-pip module
- Installs python docker module
- Increases virtual memory to support ELK stack
- Downloads and launch ELK docker container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

Images/confirm_sebpelk_running.PNG

### Target Machines & Beats
This ELK server is configured to monitor the following machines: Web-1, Web-2, web-3
- 10.0.0.6
- 10.0.0.7
- 10.0.0.8

We have installed the following Beats on these machines:
- Metricbeat
- Filebeat

These Beats allow us to collect the following information from each machine:
- Filebeat allows you to quickly parse logs, visualize trends, and get maximum value from the ELK stack. 
- Metricbeat allows fast collection and dissemintation of system metrics like CPU usage, memory allocation, and network IO statistics.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install-elk.yml file to /etc/ansible.
- Copy the filebeat-config.yml file to /etc/ansible/files
- Copy the metricbeat-config.yml file to /etc/ansible/files
- Copy the filebeat-playbook.yml file to /etc/ansible/roles
- Copy the metricbeat-playbook.yml file to /etc/ansible/roles
- Update the /etc/ansible/hosts file to include the following lines (where x is the IP address of the web or ELK server):
```
[webservers]
10.0.0.x ansible_python_interpreter=/usr/bin/python3
10.0.0.x ansible_python_interpreter=/usr/bin/python3
10.0.0.x ansible_python_interpreter=/usr/bin/python3
[elk]
10.0.0.x ansible_python_interpreter=/usr/bin/python3
```

- Update filebeat-config.yml to include:
```
output.elasticsearch:
  hosts: ["10.1.0.4:9200"]
  username: "elastic"
  password: "changeme" # will require reset
```
- Update metricbeat-config.yml to include:
```
setup.kibana:
  host: "10.1.0.4:5601
```
- Run the install-elk.yml playbook, and navigate to http://<ELK.VM.External.IP>:5601/app/kibana to check that the installation worked as expected.
