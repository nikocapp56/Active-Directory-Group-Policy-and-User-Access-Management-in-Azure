
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>

</p>

<h1>Active Directory Group Policy and User Access Management in Azure</h1>
This project demonstrates how to extend an Active Directory environment hosted in Microsoft Azure by implementing Group Policy and managing user access controls. Using Azure-based virtual machines, domain-wide security policies are enforced through Group Policy. User management tasks, such as unlocking accounts, resetting passwords, and enabling/disabling accounts, are performed through Active Directory Users and Computers. Event Viewer is utilized to audit authentication activities, like logon attempts and account lockouts, and ensure robust oversight of domain security. This setup provides a strong foundation for understanding account security in a real-world environment and establishes the groundwork for more advanced user and group policy administration.

<h2> Environments and Technologies Used </h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop Connection
- Active Directory Domain Services
- Group Policy Management
- Command Line

<h2> Operating Systems Used </h2>

- Windows Server 2022 Datacenter: Azure Edition - x64 Gen2
- Windows 10 Pro, version 22H2 - x64 Gen2

<h2> Prerequisites </h2>

- [Active Directory Infrastructure Setup in Azure](https://github.com/nikocapp56/Active-Directory-Infrastructure-Setup-in-Azure)
- [Active Directory Deployment in Azure](https://github.com/nikocapp56/Active-Directory-Deployment-in-Azure)

<h2>Deployment and Configuration Steps</h2>

<h3> 0️⃣ Overview of User Access Management </h3>

In this walkthrough, we will:

- Use Group Policy Management, which allows centralized management of user and computer settings across the domain, to configure account lockout policies under the Default Domain Policy.
- Force Group Policy updates on the Client VM, ensuring policies apply immediately across the domain.
- Simulate failed logins to trigger account lockouts, demonstrating the effectiveness of our security configuration.
- Manage user accounts in Active Directory Users and Computers by unlocking locked accounts and performing common administrative tasks like resetting passwords and disabling accounts.
- Review security logs in Event Viewer to track logon events and account activities for auditing and compliance.

<h3> 1️⃣ Open Group Policy Management and Edit Default Domain Policy </h3>

Open Remote Desktop Connection and log into DC as mydomain.com\jane_admin.
<img width="806" height="271" alt="RDC-jane-domain-dc-1" src="https://github.com/user-attachments/assets/48a441db-25a2-490e-b528-f9a767c4ae7c" />

Open Group Policy Management. Right-click on Default Domain Policy and click Edit.

<img width="813" height="544.5" alt="1" src="https://github.com/user-attachments/assets/869ed19c-e9c0-44c4-8875-7a6a05563957" />

<h3> 2️⃣ Configure Account Lockout Policy </h3>

Computer Configuration -> Policies -> Windows Settings -> Security Settings -> Account Policies -> Account Lockout Policy

<img width="719" height="403" alt="2" src="https://github.com/user-attachments/assets/2e754b54-e414-4170-bef9-6ae533a87625" />

Set Account lockout threshold to the number of invalid logon attempts you want.

Check Define this policy setting, which activates the policy and adjusts related settings to suggested values.

<img width="450" height="500" alt="4" src="https://github.com/user-attachments/assets/0181fa74-df3a-459f-8896-a9c06b811bac" />
</p>
The settings will now be reflected under the Default Domain Policy Settings.
</p>
<img width="650" alt="5" src="https://github.com/user-attachments/assets/d06b736c-aa27-4c8c-822f-e3a1b39e23a7" />

<h3> 3️⃣ Update Group Policy on the Client </h3>

Group Policy refreshes automatically every ~90 minutes.

To force it immediately:

Log into client-1 as mydomain.com\jane_admin.

<img width="802" height="272" alt="RDC-jane-domain-client-1" src="https://github.com/user-attachments/assets/dc0fa83a-a05b-4b91-b73a-196f7513e66d" />

Open Command Prompt as Administrator. 

<img width="350" alt="6" src="https://github.com/user-attachments/assets/a04c6d70-f99f-4c51-b36b-1570c96904f5" />

Run gpresult /r
</p>
This displays the resultant set of policies applied to the user and computer, confirming that your new lockout policy is active.
</p>
<img width="500" alt="7" src="https://github.com/user-attachments/assets/dcc8c45e-328b-43f9-84e2-663328b0bec8" />

<h3> 4️⃣ Test Account Lockout </h3>

Try logging into Client as one of your domain users with the wrong password more times than your lockout threshold.

There should be a message saying the account has been locked due to too many failed logon attempts.

<img width="1000" alt="8" src="https://github.com/user-attachments/assets/5906b097-010a-4b12-bb87-d00e9039b4bb" />

<h3> 5️⃣ Unlock the User Account </h3>

Return to DC as mydomain.com\jane_admin.

<img width="806" height="271" alt="RDC-jane-domain-dc-1" src="https://github.com/user-attachments/assets/18c317ce-ddb0-4a5d-97bb-de6fd0da82db" />

Open Active Directory Users and Computers.

<img width="400" alt="18" src="https://github.com/user-attachments/assets/56105a1e-5981-41a4-a721-eb6593453f33" />

Navigate to _EMPLOYEES, find the locked user.

Double-click their name, go to the Account tab, and check Unlock account.

<img width="650" alt="9" src="https://github.com/user-attachments/assets/725a770a-2644-48cc-9ca6-d058dabdc99a" />

Within Active Directory Users and Computers, passwords can be reset and user accounts can be enabled/disabled.

<img width="412" height="533" alt="10" src="https://github.com/user-attachments/assets/e31c120e-22f1-4b2b-9eb9-b48afc634ded" />

<h3> 6️⃣ Observe Event Viewer Logs </h3>

Open Event Viewer.

Windows Logs > Security

To search for the username, right-click on "Security" and click "Find"

This log records security-related events, including successful and failed logon attempts, account lockouts, and changes to user accounts. Reviewing these logs is essential for auditing and tracking authentication activity across the domain.

<img width="1280" height="769" alt="11" src="https://github.com/user-attachments/assets/c4abf921-05bf-4987-90f2-c99186fd44df" />
