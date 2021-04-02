# Setup Guide

Before beginning you want to make sure to update -- sudo apt update -- and upgrade -- sudo apt upgrade -- all virtual machines first! This includes the VMs hosting DVWA and ELK containers. This should be the first step in VM creation and maintenance in general, but it is particularly vital in this case as it will interfere with package installation and playbook execution should the systems be out of date.

1. Initial Azure Configurations

a) Create a Resource Group

  By having all or most of our resources in one Virtual Network, the webservers and jumpbox virtual machines will be a part of the same subnet. Placing all resources in one Resource Group is simply good housekeeping and easier to manage. It is best to keep the names consist to avoid confusion (e.g. EXAMPLE-resource-group and EXAMPLE-vnet). 
  
  Simply type in the type of resource intended to create -- for exmaple, "resource group" -- in the search bar. It will most likely appear as the top search result will be the right resource. Clink on the link and then clikc "+ Add".

 It is necessary to enter a Resource Group name and to select a Region. From there you may select "Review + create" at the bottom of the page, or click "Next: Tags >" to label the Resource Group for further organization (in the case there are multiple Resource Groups). 

b) Setting up the VNet

  Before you can deploy servers and services, there must be a network where these items can be accessed.

  - This network should have the capacity to hold any resource that the Red Team needs, now and in the future.

  - Return to the home screen and search for "net." Choose the search result for Virtual networks.

  - Click on the + Add button on the top-left of the page or the Create virtual network button on the bottom of the page.

  Fill in the network settings:

  - Subscription: Your free subscription should be the only option here. 

  - Resource group: This should be the resource group you created in step two.

  - Name: A descriptive name so it will not get confused with other cloud networks in the same account.

  - Region: Make sure to choose the same region you chose for your resource group.

    - Carefully configuring the region of your resources is important for ensuring  low latency and high availability. Resources should be located as close as possible to those who will be consuming them.

 ![Create_VN](./Images/Create_VN.png)
 
  - IP Addresses: Azure requires you to define a network and subnet. 
    - Use the defaults on this tab.
  - Security: Leave the default settings. 
    - In order to avoid recurring charges, do NOT enable DDoS Protection Standard. 
    - Tags: No tags are needed. 

  Click Create
c) Setting Up Network Security Groups

  Now we need to setup a firewall in front of the VNet to block specific traffic

  To create a network security group:

  - On your Azure portal home screen, search "net" and choose **Network security groups**. 
  ![NSG](./Images/NSG.png)
- Create a new security group.

  - Add this security group to your resource group.

  - Give the group a recognizable name that is easy to remember.

  - Make sure the security group is in the same region that you chose during the previous activity.
  
    To create an inbound rule to block traffic:

  - Once the security group is created, click on the group to configure it.

  - Choose **Inbound security rules** on the left.

  - Click on the **+ Add** button to add a rule.

  Configure the inbound rule as follows:

  - Source: Choose **Any** source to block all traffic.

  - Source port ranges: Source ports are always random, even with common services like HTTP. Therefore, keep the wildcard (*) to match all source ports.

  - Destination: Select **Any** to block any and all traffic associated with this security group.

  - Destination port ranges: Usually, you would specify a specific port or a range of ports for the destination. In this case, you can use the wildcard (*) to block all destination ports. You can also block all ports using a range like `0-65535`.

  - Protocol: Block **Any** protocol that is used.

  - Action: Use the **Block** action to stop all of the traffic that matches this rule.

  - Priority: This rule will always be the last rule, so it should have the highest possible number for the priority. Other rules will always come before this rule. The highest number Azure allows is 4,096.

  - Name: Give your rule a name like "Default-Deny."

  - Description: Write a quick description similar to "Deny all inbound traffic."

  - Save the rule.
 
  ![A Rule Example](./Images/JumpboxRule.png)
