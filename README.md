# Automated-ELK-Stack-Deployment
The files in this repository were used to configure the network depicted below:

![topo](https://user-images.githubusercontent.com/74678784/120737420-0911f680-c4bc-11eb-9627-4e36d9af517f.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to recreate the entire deployment pictured above. The install-elk.yml file may be used to install ELK-stack on a machine, while the metricbeat.yml and filebeat.yml may be used to install these beat monitors on the desired machine.

Below are screenshots of the above-mentioned files:

![screencap1](https://user-images.githubusercontent.com/74678784/120737569-4fffec00-c4bc-11eb-88b4-8251a4e32d6d.png)
![screencap2](https://user-images.githubusercontent.com/74678784/120737613-63ab5280-c4bc-11eb-8339-11ae4d64d99d.png)
![screencap3](https://user-images.githubusercontent.com/74678784/120737663-7b82d680-c4bc-11eb-9b47-f0254aa7411c.png)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network. These two important roles maintain availability and confidentiality in the CIA Triad.

The Jump Box Provisioner must be accessed from a local machine via ssh using asymmetric encryption. The Jump Box Provisioner houses a container that uses the same method to access the other virtual machines on the network.

Integrating an ELK server allows users to easily monitor the VMs hosting the DVWA for changes to the metrics and system logs on the Web1 and Web2 virtual machines.

Beats are data shippers that monitor and send data to Elasticsearch. Filebeat centralizes logs and files whereas Metricbeat monitors system and service statistics. Both of these beats are installed on the Web1 and Web2 Virtual Machines, and then they forward the collected data to the ELK Server.

The configuration details of each machine may be found below:

| Name                 | Function  | IP Address | Operating System |
|----------------------|-----------|------------|------------------|
| Jump Box Provisioner | Gateway   | 10.0.0.4   | Ubuntu Linux     |
| Web1 Virtual Machine | DVWA Host | 10.0.0.5   | Ubuntu Linux     |
| Web2 Virtual Machine | DVWA Host | 10.0.0.6   | Ubuntu Linux     |
| ELK Server           | ELK       | 10.1.0.4   | Ubuntu Linux     |


### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
76.203.140.169

Machines within the network can only be accessed by the Jump Box Provisioner.
- The ELK VM can only be accessed via the ansible container within the Jump Box machine. The Jump Box’s internal IP address is the only IP allowed incoming to the virtual network.

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible | Allowed IP Addresses |
|----------------------|---------------------|----------------------|
| Jump Box Provisioner | Yes                 | 76.203.140.169       |
| Web1 Virtual Machine | No                  | 10.0.0.4             |
| Web2 Virtual Machine | No                  | 10.0.0.4             |
| ELK Server           | No                  | 10.0.0.4             |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because Ansible allows for a streamlined deployment across multiple virtual machines at once.
It also allows for a lightweight and consistent installation. 

The playbook implements the following tasks:
- The ‘hosts: elk’ in the beginning designates our target group of machines.
- The sysctl-ansible module allows us to raise the virtual memory to meet the requirements of the elk stack.
- The middle commands install docker, python, and the python-docker module.
- The last command pulls in the elk image and opens three ports. 

The following screenshot displays the result of running `docker ps` on the ELK Server after successfully configuring the ELK instance:
![screencap4](https://user-images.githubusercontent.com/74678784/120738035-25faf980-c4bd-11eb-875a-5719791b8c24.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
10.0.0.5 Web1 VM
10.0.0.6 Web2 VM

We have installed the following Beats on these machines:
Filebeat
Metricbeat

These Beats allow us to collect the following information from each machine:
Filebeat monitors logs and files whereas Metricbeat monitors system and service statistics. Both of these beats are installed on the Web1 and Web2 Virtual Machines, and then they forward the collected data to the ELK Server.
I used Metricbeat to gather the results of a CPU stress test, and I used Filebeat to gather results from failed ssh login attempts, then Kibana visualized the results of these tests.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install-elk.yml file to the /etc/ansible directory .
- Update the hosts file to include the IP addresses of the desired virtual machines and the group.
- Run the playbook, and navigate to the ELK Server’s public IP to check that the installation worked as expected.
