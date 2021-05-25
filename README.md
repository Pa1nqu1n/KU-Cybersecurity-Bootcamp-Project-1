# KU-Cybersecurity-Bootcamp-Project-1

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/Pa1nqu1n/KU-Cybersecurity-Bootcamp-Project-1/blob/8b6ecc30e732926e25e1161dcfd036efc4466e11/Diagrams/Elk%20stack%20diagram.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the ansible-playbook file may be used to install only certain pieces of it, such as Filebeat.

Filebeat.yml
---
- name: installing and launching filebeat
  hosts: webservers
  become: yes
  tasks:
  - name: download filebeat deb
	command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb
  - name: install filebeat deb
	command: dpkg -i filebeat-7.6.1-amd64.deb
  - name: drop in filebeat.yml
	copy:
  	src: /etc/ansible/filebeat-config.yml
  	dest: /etc/filebeat/filebeat.yml
  - name: enable and configure system module
	command: filebeat modules enable system
  - name: setup filebeat
	command: filebeat setup
  - name: start filebeat service
	command: service filebeat start
  - name: enable service filebeat on boot
	systemd:
  	name: filebeat
  	enabled: yes

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

Load balancers help prevent ddos attacks by spreading out the requests for severs by sending them out to other servers so that one server is not completely bogged down to point no can access by using balancers allows clients accessing it to go different servers of the same website so its less like for server to complete crash.

The advantage of jumpbox is that it provides a private gateway to your private network that allows all machines that are added into the network will not be dealing with the internet directly thus adding another level of security for it.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system logs.

Filebeat- It montaiers the log files and locations that are specified and it also collects log events it forwards the information to Elasticsearch or Logstash. For this project I used Elasticsearch.

Metricbeat- Collects the metrics and staticstic’s form your machine and will send them to output that is specified for it to go to whether that's Elasticsearch or Logstash. For this project I used Elasticsearch.

The configuration details of each machine may be found below.


| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump-Box-Provisioner | Gateway  | 10.0.0.7 | Linux |
| Web-1 | Web Server  | 10.0.0.10 | Linux |             
| Web-2 | Web Server  | 10.0.0.9 | Linux |
| Elkserver | Monitoring | 10.2.0.8 | Linux |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

104.181.106.116

Machines within the network can only be accessed byJump-Box-Provisioner.

The Elk server can access Jump-Box-Provisioner IP-10.0.0.7, Web-1 IP 10.0.0.10, Web-2 IP 10.0.0.9

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump-Box-Provisioner | No |104.181.106.116 |
| Web-1 | No | 10.0.0.7 |
| Web-2 | No | 10.0.0.7 |
| Elkserver | No | 10.0.0.7 |
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

instead of having to go in each day and manually update it taking away time form the work day that could be used to focus on 

The playbook implements the following tasks:

Install docker.io

Install pip3

Vm.max_map_count value: "262144"

Download and launch a docker elk container


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/Pa1nqu1n/KU-Cybersecurity-Bootcamp-Project-1/blob/a953b78eaf5779bc753b349f3b107722ea960977/Images/docker


### Target Machines & Beats
This ELK server is configured to monitor the following machines:

Web-1 IP-10.0.0.10

Web-2 IP-10.0.0.9 

We have installed the following Beats on these machines:

Filebeats

Metricbeats


These Beats allow us to collect the following information from each machine:

The filebeat monitors log files and locations that you specify which is used to see changes and messages from the log files that received commands such as the kill command.

The metricbeat collects and records the metrics and statistics of the operating system and also pulls from the services that you are running on the server, which allows you to be able to tell the RAM and CPU being used on the web servers at a particular time. 




### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

Copy the yaml file to /etc/ansible directory.

Update the /etc/ansible/hosts file to include hosts groups, IP address, with the following line ansible_python_interpreter=/usr/bin/python3

Run the playbook, and navigate to curl localhost/setup.php to check that the installation worked as expected.


The list of playbooks used for the project are Filbeat.yml, Docker.yml, Install-elk.yml, Pentest.yml, and are to copied into the /etc/ansible directory 

When it comes to updating, use the /etc/ansible/hosts directory by using the hosts group name with the IP address underneath to specify the machines the playbooks are supposed to be running on.


The URL that is used to see if the Elk server is up and running is http://40.83.222.230:5601/app/kibana







