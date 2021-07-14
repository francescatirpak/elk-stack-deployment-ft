# elk-stack-deployment-ft
Deployment of Various VMs running Docker, DVWA, and ELK containers

Most scripts are used to configure cloud servers with docker containers, resulting in an architecture including 4 servers running DVWA containers supported by a jump box, and a separate cloud server running ELK stack.

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[https://github.com/francescatirpak/elk-stack-deployment-ft/blob/main/Diagrams/Project1_NetworkDiagram.png]

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Ansible file may be used to install only certain pieces of it, such as Filebeat.

  [Install ELK](elk-stack-deployment/Ansible/roles/install-elk.yml "Install ELK")

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly fault tolerant, in addition to restricting access to the network.
- Load balancing protects the availability aspect of security, ensuring at least one server is available to use should others fail.
- Using a jump box continues hardening security on the aspect of access to the servers in the internal network, ensuring only users given express permission to enter these servers do so.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _____ and system _____.
- Filebeat gathers log files from specific files (e.g. Apache, Microsoft Azure tools, Nginx web server, MySQL databases, etc) to be analyzed in Logstash and Elasticsearch for changes to said files.
- Alternatively, Metricbeat collects metrics about the machine in question, such as uptime and CPU usage to analyze for suspicious changes.

The configuration details of each machine may be found below.

| Name     | Function        | IP Address | Operating System |
|----------|-----------------|------------|------------------|
| Jump Box | Gateway         | 10.0.0.4   | Linux            |
| Web-1    | Internet Access | 10.0.0.5   | Linux            |
| Web-2    | Internet Access | 10.0.0.6   | Linux            |
| Web-3    | Internet Access | 10.0.0.7   | Linux            |
| DVWA-3   | DVWA            | 10.0.0.10  | Linux            |
| DVWA-4   | DVWA            | 10.0.0.9   | Linux            |
| ELK-VM-1 | ELK stack       | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 74.90.85.33

Machines within the network can only be accessed by SSH using an encrypted key pair via Ansible containers.

A summary of the access policies in place can be found in the table below.

| Name         | Publicly Accessible | Allowed IP Addresses |
|--------------|---------------------|----------------------|
| Jump Box     | Yes                 | 74.90.85.33          |
| Web-1        | No                  | 10.0.0.4             |
| Web-2        | No                  | 10.0.0.4             |
| Web-3        | No                  | 10.0.0.4             |
| DVWA-3       | No                  | 10.0.0.4             |
| DVWA-4       | No                  | 10.0.0.4             |
| ELK-VM-1     | No                  | 10.0.0.4             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because Ansible allows accelerated automation without direct access to servers, allowing configuration to be easy and fast for any user.

The playbook implements the following tasks:
- install Docker
- increase virtual memory
- download and launch ELK docker container
  - implement in specified published ports
- enable Docker service

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

[https://github.com/francescatirpak/elk-stack-deployment-ft/blob/main/Images/docker_ps_output.png]

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6
- 10.0.0.7

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat generates and organizes log files regarding the servers specified to it in order to visualize and analyze in Logstash and Elasticsearch. This can be used to identify if any files have been changed and when.
- Metricbeat, similarly, collects machine metrics to identify changes and analyze how healthy said machine is. This can used to identify when suspicious activity may occur, such as high CPU usage and uptime.
- Both are used to idenfity potentially suspicious behavior in different areas of the machines.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-playbook.yml file to /etc/ansible/roles within the Ansible container under the Jump Box.
- Update the filebeat-config.yml file to include the host IP (ELK VM internal IP) and port (9200) under "Elasticsearch output" (line 1106). Navigate to line #1106 and add the host IP and port (5601).
- Run the playbook, and navigate to the Filebeat installation page on the ELK server GUI to check that the installation worked as expected. 

- Copy 'install-elk.yml' playbook into the /etc/ansibile/roles directory within the Ansible node.
- The filebeat-config.yml and metricbeat-config.yml files allow customization of this process, including to specify which machine to install an instance of these Beats to. Change the IP address of the machine you wish to configure in these files in order to customize the process.
- The hosts file configures which servers are in which group to specify configuration through an Ansible playbook. Change the IP addresses and group headers here to specify which machines to install instances of Filebeat, Metricbeat, or ELK servers on specific machines.
- Navigate to http://[your.VM.IP]:5601/app/kibana, using the public IP address of the ELK server. Follow the links 'Add Log Data', 'System Logs', 'DEB' (tab), scroll to the bottom of the webpage, and select 'Check data' under the Module status section. If the playbook succeeded, the metric data will be available here.
