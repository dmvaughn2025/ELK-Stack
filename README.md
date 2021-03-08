# ELK-stack-Project
The files in this repository were used to configure the network depicted below.
![alt text]https://github.gatech.edu/dvaughn34/ELK-stack-/blob/master/Diagrams/ELk%20Project%20Diagram.png

These files have been tested and were used to deploy an ELK deployment on Azure. The files in this repository can be used to recreate the deployment pictured above.

This document contains the following:
- Description of the topology
- Access Policies
- ELK Configuration
  - Beats used
  - Machines being monitored
-How to use the Ansible build

# Description of the Toplogy
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.
Load balancing ensures that the application will be highly avaialable, in addition to restricting access to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system metrics and service information.

|  Name  |    Function    | IP Address | Operating System |
|--------|----------------|------------|------------------|
|Jump Box|     Gateway    |  10.1.0.4  |      Linux       |
| Web-1  | DVWA container |  10.1.0.5  |      Linux       |
| Web-2  | DVWA container |  10.1.0.6  |      Linux       |
| Web-3  | DVWA container |  10.1.0.8  |      Linux       |

# Access Policies
The machines on the internal network are not exposed to the public Internet.
Only the jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 76.214.122.90
Machines within the network can only be accessed by the jumpbox 104.211.28.99

A summary of the access policies can be found below

|   Name    | Publicly Accessible | Allowed IP Addresses |
|-----------|---------------------|----------------------|
| Jump box  |        yes          |      76.214.122.90   |
| DVWA VM's |        No           |      10.1.0.4        |
| ELK server|        No           |      10.1.0.4        |

# Elk Configuration
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it reduces the amount of time it takes to stand up the ELK stack as well as making the process easily repeatable. 

To implement the ELK stack, the following steps were taken. 
1. A new virtual network was created for the ELK stack and was peered with the existing red team virtual network. 
2. Using our existing ansible container, a new ssh key was generated for the new ELK VM. [How to create and use an SSH key for linux VM's in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-portal)
3. Utilizing this new ssh key, a new VM is created to house the Elk stack. [How to create a Linux VM in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-portal)
4. Next, we add the new VM to the ansible hosts file so that it can be recognized by the playbook (hosts file in ansible directory).
5. After the ELK vm is added to the hosts files, we can run the ELK-playbook to configure the VM. (elk playbook in ansible directory).
6. Lastly, we create a new inbound rule so that we can view the ELK GUI in the browser. This is done by allowing traffic from your host's public IP to port 5601 on your vitual network.
7. To verify everything is working correctly. Naviagte to <mark>http://[your.VM.IP]:5601/app/kibana</mark>

# Filebeat
Once we have the ELK server up and running, we want to install a beat that will forward and centralize our log data for analysis. This particular beat is called filebeat. 
To install filebeat, we need to accomplish the following:
1. Download filebeat-config.yml to the ansible container (Can be found in the ansible directory)
2. Edit the file so that the hosts under "output.elastisearch & setup.kibana" are the Ip address of your ELK machine.
3. Create the filebeat playbook and run it with <mark>ansible-playbook filebeat-playbook.yml</mark> (located in ansible directory)
4. Lastly, navigate back to the ELK GUI and click "check data" to verify the ELK stack is recieving incoming data.
