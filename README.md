
<p align="center">
<img src="https://github.com/user-attachments/assets/96f87482-0053-4788-8888-aa93a4400811" />

</p>

<h1>Active Directory Infrastructure Setup in Azure</h1>
This project demonstrates setting up the foundational infrastructure required for an Active Directory environment in the cloud using Microsoft Azure by deploying a Windows Server 2022 virtual machine as a Domain Controller and a Windows 10 machine as a client. The Domain Controller is configured with a static private IP and serves as the internal DNS server, while the client is set to use the Domain Controller for all DNS queries. This setup establishes secure communication, validates connectivity, and prepares the environment for future domain deployment and user access management.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop Connection
- Windows PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022 Datacenter: Azure Edition - x64 Gen2
- Windows 10 Pro, version 22H2 - x64 Gen2

<h2>Deployment and Configuration Steps</h2>

<h3>0️⃣ Overview of Azure Resources </h3>

We will create and work with the following resources in Microsoft Azure:
- Virtual Machines:
  - one for the domain controller (dc-1 running Windows Server 2022)
  - one for the client machine (client-1 running Windows 10)
- Resource Groups: To organize and manage related resources.
- Virtual Networks: To allow secure communication between the virtual machines. 

By the end, your Azure portal should look similar to this: 

<img width="801" alt="azure" src="https://github.com/user-attachments/assets/f81c6800-45ef-4c8d-be3c-1f2bde5a0e9c" /> 

<h3>1️⃣ Create Resource Group</h3>
<p> Go to the Azure Portal and navigate to Resource Groups. </p>
<p> Click + Create, and create a resource group. </p>

<img width="539" alt="resourcegroup" src="https://github.com/user-attachments/assets/fe39fb40-c24b-4cd9-898c-d6225b75d3cd" />

<h3> 2️⃣ Create Virtual Network </h3>

<p> Go to the Azure Portal and navigate to Virtual Networks. </p>
<p> Click + Create, and create a virtual network. </p>

<img width="557" alt="vnet" src="https://github.com/user-attachments/assets/23e03380-e422-4080-85b6-0c52e51fc28f" />

<h3> 3️⃣ Create Client VM </h3>

<p>In an Active Directory environment, a client machine is needed to later join to the domain, test authentication, and demonstrate communication with the Domain Controller.</p>
Create a Virtual Machine with the following configuration:

- Resource Group: Choose the Resource Group we created earlier
- Name: set to your choice 
- Image: Windows 10 Pro, version 22H2
- Size: 2 vCPUs, 8 GiB memory
- Username/Password: set to your choice 
- Licensing: Check the box to confirm Windows licensing
- Network: Choose the Virtual Network we created earlier

Click Review + Create, then Create.

<img width="454" alt="client-1" src="https://github.com/user-attachments/assets/ccb69a2a-feaa-498b-85ab-29d5cd941232" />

<img width="475" alt="vmsize,password,licensing" src="https://github.com/user-attachments/assets/d5637f58-db61-47a7-adc8-29ce88bf8d13" />

<h3> 4️⃣ Create Domain Controller VM </h3>
<p>In an Active Directory environment, the Domain Controller (DC) also serves as the DNS server, handling all internal name resolution needed for domain authentication and resource access.</p>
Create a new Virtual Machine with the following configuration:

- Resource Group: Choose the Resource Group we created earlier
- Name: set to your choice 
- Image: Windows Server 2022 Datacenter
- Size: 2 vCPUs, 8 GiB memory
- Username/Password: Use the same username/password as before.
- Virtual Network: Choose the Virtual Network we created earlier

Click Review + Create, then Create.

<img width="457" alt="dc-1" src="https://github.com/user-attachments/assets/b60ef10f-8580-45a2-ad27-1d0e797c7ff8" />

<h3> 5️⃣ Set Domain Controller's NIC Private IP address to static </h3>

When configuring a DC, it’s essential that its IP address remains constant so that all domain-joined devices can reliably find it for authentication, DNS, and other directory services. 

This is critical because if the DC’s IP changes, clients relying on it for DNS and domain services would fail to locate it.

Virtual Machine -> DC VM -> Network Settings -> Network Interface/IP Configuration

Change Private IP address settings from Dynamic to Static.

Click Save.

<img width="622" alt="dc-1static" src="https://github.com/user-attachments/assets/f5c7c345-c953-45e0-9470-95ac16c70518" />

<h3> 6️⃣ Set Client's DNS settings to Domain Controller's Private IP address </h3>

This ensures that the Client directs all domain-related queries to the Domain Controller. 

This is necessary so that when the Client later attempts to join the domain, it can correctly locate the domain controller using DNS.

Virtual Machine -> Client VM -> Network Settings -> Network Interface/IP Configuration -> DNS Servers

Select Custom, and enter DC's private IP address.

Click Save.

<img width="530" alt="client-1dnsserver" src="https://github.com/user-attachments/assets/4b613a8d-545f-480e-92b2-9780598352fd" />

<h3> 7️⃣ Restart Client VM </h3>

<img width="818" alt="client-1restart" src="https://github.com/user-attachments/assets/affe18e4-5e96-4acb-8801-271db9f81adc" />

<h3> 8️⃣ Login to Domain Controller VM </h3>

Open Remote Desktop Connection.

Enter DC’s public IP address and log in using the username/password you set before.

<img width="807" alt="RDC-dc-1" src="https://github.com/user-attachments/assets/3ae1e24f-31ff-4da4-a332-04beb9d23077" />

<h3> 9️⃣ Disable Firewall (for testing connectivity) </h3>

Inside the DC VM, open Windows Defender Firewall with Advanced Security.

Click Windows Defender Firewall Properties.

For each profile (Domain, Private, Public), set Firewall state: Off

<img width="782" alt="firewalloff" src="https://github.com/user-attachments/assets/41578574-1bb8-40d8-a212-bf55b87fa839" />

<h3> 🔟 Login to Client VM </h3>

Open Remote Desktop Connection.

Enter Client’s public IP address and log in using the username/password you set before.

<img width="806" alt="RDC-client-1" src="https://github.com/user-attachments/assets/1d221b42-b5d9-4b76-bfcf-542027fcd485" />

<h3> 1️⃣1️⃣ Attempt to ping DC's private IP address (to ensure network connectivity)</h3>

Inside the Client VM, open Windows Powershell.

Ping DC's private IP address.

Run ipconfig /all

The output for the DNS settings should display DC’s private IP address, confirming that the client is using the Domain Controller for name resolution.

<img width="610" alt="powershell" src="https://github.com/user-attachments/assets/8cc2bbcc-4fa3-46a3-9bdb-0694ed83e82a" />
