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
* You have run **ssh-keygen** to create a new SSH key pair for authentication
	* Run **cat ~/.ssh/id_rsa.pub** to display your id_rsa.pub key

## Environment Setup
### Setting up the Resource Group
1. On the home screen, search for "Resource Group". Choose the result for **Resource groups** 
2. Click on the **+ Add** button and then on **Create resource group**
3. Create a name for your resource group and choose a region.
4. Click on **Review + Create.** to finalize your settings and create the group. 

### Setting up the Virtual Network
1. On the home screen, search for "Virtual Network". Choose the result for **Virtual networks** 
2. Click on the **+ Add** button and then on **Create virtual network**
3. Configure your network with these settigns.
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
6. 

### Creating the Jump Box Provisioner Virtual Machine


### Creating Virtual Machines
1. Ensure you are logged into your personal Azure account.
2. Create a new virtual machine by navigating to **Resource > Add Virtual Machine**
3. Set Virtual Machine Name to "Web-1"
4. Set Availability set to "Web-Set"
5. Set the size to **1-CPU, 2 GB memory**
6. Set Virtual Machine Username to "azureuser"
7. Set Virtual Network to "Red-Team-net" in Azure
8. Set Public IP to **none**
