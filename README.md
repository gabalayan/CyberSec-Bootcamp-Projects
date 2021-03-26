# ELK-Stack-Project
#### Creating a live ELK stack server in Microsoft Azure in order to monitor traffic over a cloud network.
## Technologies Used
* Elastic Stack
* Elastic Filebeat
* Microsoft Azure
* Docker
* Ansible

## Prerequisites
Before you continue, ensure you have met the following requirements:
* You have an account with Microsoft Azure
* You have run **ssh-keygen** in your device's terminal to create a new SSH key pair for authentication
	* Run **cat ~/.ssh/id_rsa.pub** to display your id_rsa.pub key

## Environment Setup
### Setting up the Resource Group
1. On the home screen, search for "Resource Group". Choose the result for **Resource groups** 
2. Click on the **+ Add** button and then on **Create resource group**
3. Create a name for your resource group and choose a region.
	* Ex. "RED-TEAM"
4. Click on **Review + Create.** to finalize your settings and create the group. 

### Setting up the Virtual Network
1. On the home screen, search for "Virtual Network". Choose the result for **Virtual networks** 
2. Click on the **+ Add** button and then on **Create virtual network**
3. Configure your network with these settings.
	* Subscription: Your free, trial subscription will be used here.
	* Resource group: The resource group you created in the previous step
	* Name: RedTeamNet
	* Region: Make sure to choose the same region you choose for your resource group.
	* IP Addresses: Use the defaults on this tab.
	* Security: Leave the default settings.
		* In order to avoid recurring charges, do **not** enable DDoS Protection Standard.
	* Tags: No tags are necesarry
4. Click **Create.**

### Setting up a Network Security Groups
1. On the home screen, search for "Network Security". Choose the result for **Network security groups.**
2. Click on the **+ Add** button and then on **Create network security group**
3. Add this security group to the resource group you created earlier.
4. Give the security group a recognizeable name.
5. Make sure the security group is in the same region as the virtual network. 
6. Click **Create.**
7. Add inbound security rules by going to **Settings** and selecting **Inbound security rules**
	* Configure your inbound security rules with these settings
		**Allow-SSH** Priority: 100, Port 22, Protocol: TCP, Source: <Host IP Address>, Destination: <Jump Box Provisioner VM Private IP>, Allow
		**SSH-From-Jump-Box** Priority: 200, Port 22, Protocol: TCP, Source: <Jump Box Provisioner VM Private IP>, Destination: VirtualNetwork, Allow
		**Port_80** Priority: 250, Port 80, Protocol: Any, Source: Any, Destination: VirtualNetwork, Allow
		**Allow-Port-9200** Priority: 251, Port 9200, Protocol: Any, Source: <Host IP Address>, Destination: Any, Allow
		**Default-Deny** Priority: 4096, Port: Any, Protocol: Any, Source: Any, Destination: Any, Deny

### Creating the Jump Box Provisioner Virtual Machine
1. On the home screen, search for "Virtual Machine". Choose the result for **Virtual Machines**
2. Click on the **+ Add** button and then on **Create a virtual machine**
3. Add the following settings for this VM:
	* Resource group: The resource group you specified for your red team
	* Virtual machine name: "Jump Box Provisioner"
	* Region: Same region that you used for your other resources
	* Availablility options: Default. We will change this setting on other machines. However, for our Jump Box we will keep this at Default. 
	* Image: Ubuntu Server 18.04
	* Size: Standard - B1s - 1 vcpus, 1 GiB memory
4. For SSH, use the following settings
	* Authentication type: SSH public key
	* Username: Create a unique username for this machine
		* Ex. RedAdmin
	* SSH public key: Paste the public key string that you generated from **cat ~/.ssh/id_rsa.pub** in the pre-requisites
	* Public inbound ports: Ignore. This will be overwritten by your security group
	* Select inbound ports: Ignore. This will also be overwritten by your security group
5. On the **Networking** tab for your VM, set the following settings.
	* Virtual network: Choose the virtual network you created earlier for Red-Team
	* Subnet: Choose the subnet created earlier
	* Public IP: Chooose **Create New** and choose **Static** in the side panel. Give the IP address a unique name.
	* NIC network security group: Choose **Advanced** and specifiy the custom security group
	* Accelerated networking: Default
	* Load Balancing: Default (No)
6. Click on **Review + Create**
7. Finalize all your settings and create the VM by clicking the **Create** button again


### Creating VM's 2 and 3 - Web VM's
1. On the home screen, search for "Virtual Machine". Choose the result for **Virtual Machines**
2. Click on the **+ Add** button and then on **Create a virtual machine**
3. Add the following settings for this VM:
	* Resource group: The resource group you specified for your red team
	* Virtual machine name: "Web-1" and "Web-2"
		* You can add more VM's, just keep this naming convension for simplicity.
	* Region: Same region that you used for your other resources
	* Availability options: Make sure all of the Web VM's are in the same availability set. 
		* Availability Options > 'Availability Set' > **Create New**
			* Give the set an appropriate name and choose this set when creating the other Web VM's.
