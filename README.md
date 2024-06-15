# NIST800-53 SC-7 Compliance Hardening

<h2>Purpose</h2>

- Configure security settings for the honeynet so that NIST800-53, specifically SC-7 Boundary Protection, compliance is satisfied.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/Lab%20diagram%201.png"/>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/Lab%20diagram%202.png"/>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/1.png"/>

#
<h3>Enable Firewall for Key Vault</h3>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/2.png"/>

Navigate to your key vault blade.

Go to the Networking tab. Within "Firewalls and virtual networks" select "Disable public access" and "Allow trusted Microsoft services to bypass this firewall", then click "Apply".

<h3>Create a Private Endpoint Connection for Key Vault</h3>

A Private Endpoint Connection allows you to connect to a resource on Azure only through the private network/subnet. This gives the resource more security as it cannot be accessed through the public Internet.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/3.png"/>

Go to "Private endpoint connections" and select "+ Create".

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/4.png"/>

Select the same Resource Group used up till now and give the instance a name. The Network Interface name will be named automatically. Select the same region as the Resource Group.

Go to "Resources" next.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/5.png"/>

Select "Connect to an Azure resource in my directory". Select "Key Vault" for the Resource Type and Resource options.

Go to "Virtual Network".

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/6.png"/>

Select the subnet you want to deploy the private endpoint within, and choose to "Dynamically allocate IP address".

Go to "DNS".

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/7.png"/>

Select "Yes" to integrate with private DNS zone and choose the respective subscription and subnet.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/8.png"/>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/9.png"/>

Create the private endpoint.

#
<h3>Enable Firewall for Storage Accounts</h3>

Repeat the same steps as were followed for Key Vault.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/10.png"/>

<h3>Create a Private Endpoint Connection for Storage Account</h3>

Repeat the same steps as were followed for Key Vault.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/11.png"/>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/12.png"/>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/13.png"/>

Select "blob" (for blob storage) as the target sub-resource

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/14.png"/>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/15.png"/>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/16.png"/>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/17.png"/>

#
<h3>Observe the network configurations using Network Watcher Topology</h3>

From here, you can get a high-level overview of all the "stuff" that is in your subnet.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/18.png"/>

For Scope, select the respective subscription, resource group, and location you want to focus on.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/19.png"/>

From here, we can see everything that is within the subnet, including the private endpoints created for the Key Vault and Storage Account, respectively.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/20.png"/>

It's possible to see more detail about specific infrastructure elements. Just click the little "+" symbol on the corner of the icons. 

For example, selecting the Linux VM NIC allows us to see the resources that are connected to it such as the linux-vm itself, it's NSG, and the IP configurations associated with it. 

#
<h3>Test Key Vault Network Connectivity</h3>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/21.png"/>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/22.png"/>

Using the Key Vault FQDN, perform an nslookup through the windows-vm. The IP address resolved for the Key Vault should be a private address as the KV and windows-vm are in the same sunbet.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/23.png"/>

Conduct an nslookup for the Key Vault again but on your host computer. It should resolve to a public IP address as your host computer is making contact with the Key Vault from the public Internet.

#
<h3>Test Storage Account Network Connectivity</h3>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/24.png"/>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/25.png"/>

Conduct an nslookup for the Storage Account using its FQDN on windows-vm. As with the Key Vault, it should resolve to a private IP address also since, again, the SA and windows-vm are in the same subnet.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/26.png"/>

The same follows for conducting an nslookup via your host computer. It should resolve to a public IP address as contact is being made through the public Internet.

#
<h3>Enable and Configure NSG for the Subnet</h3>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/27.png"/>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/28.png"/>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/29.png"/>

Create an NSG and attach it to the subnet. It's fine if the NSG doesn't have an configurations associated with it as of now. It still meets NIST800-53: SC-7 compliance demands as is.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/30.png"/>

From Network Watcher, you can see a high level overview of the network including the newly attached subnet NSG.

#
<h3>Lock down VM NSGs</h3>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/31.png"/>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/32.png"/>

Remove the NSG inbound security rule allowing full access to the windows-vm. Also delete any inbound rule allowing RDP access.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/33.png"/>

Configure an inbound rule allowing all traffic only from the public IP address of your host device. This will make it so that only inbound traffic from your host computer will be allowed entry to windows-vm.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/nist80053-sc7-images/main/34.png"/>

Repeat the same process for linux-vm.
