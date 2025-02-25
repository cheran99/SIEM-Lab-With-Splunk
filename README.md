# SIEM-Lab-With-Splunk

## Introduction

Detection and response to suspicious activities, such as brute-force attacks, are crucial to protecting the digital landscape. Because of this, an SIEM lab is essential to streamline threat detection and mitigate potential threats. This project highlights the setup of an SIEM lab using Splunk Enterprise.

### Objectives
1. Download and install Splunk on your local machine/virtual machine
2. Configure Splunk to collect and ingest log data from host operating systems
3. Create custom threat detection rules
4. Simulate suspicious activities such as brute force logins
5. Build dashboards to show the contents of monitored logs

## Tools Used
- Splunk Enterprise
- Ubuntu VM (Splunk Server)
- Windows 10 VM (Splunk Universal Forwarder)

## Project Setup

### Downloading and Installing Splunk Enterprise on Ubuntu VM

To download and install Splunk Enterprise:
- Go to the following link: <a href="https://www.splunk.com/en_us/download/splunk-enterprise.html">Download Splunk Enterprise</a>
- Create your account.
- Verify your email to activate your Splunk account.
- The verification link will take you to the download page where you must choose a platform to download Splunk.
- Choose Linux as the platform since it is being downloaded on the Ubuntu VM and you can choose the .deb installation package.
- Open Terminal in the Ubuntu VM and paste and run the wget command to download the selected installation package:
  ![image](https://github.com/user-attachments/assets/7e333cc9-6bc7-41f0-8b32-0f57be9e25b3)
- Install Splunk using the command:

  `sudo dpkg -i splunk-9.4.0-6b4ebe426ca6-linux-amd64.deb`
- To check the status of the installation, run the following command:

  `sudo dpkg --status splunk`
- Next, start Splunk using the following command:

  `sudo /opt/splunk/bin/splunk start`
- Accept the license and create an administrator username and password:
  ![image](https://github.com/user-attachments/assets/0db65ea8-7186-4372-a5e1-09f89b6c681f)
- Once you have successfully created a username and password, you will be provided with the Splunk web interface:
  ![image](https://github.com/user-attachments/assets/d26d2b33-d5c5-4d25-8c0c-a74ec02d3c3a)
- Open the link on the web browser in the Ubuntu VM. This will take you to the login page where you will be asked to use the credentials that you created earlier:
  ![image](https://github.com/user-attachments/assets/1df131f9-9885-4e75-8002-ef3b25ca8b4a)

### Downloading and Installing the Universal Forwarder

The purpose of Universal Forwarders is to remotely collect data from the host operating system and send it to the Splunk Enterprise instance. The Splunk server will ingest this data and create monitoring logs. The Universal Forwarder will be downloaded and installed on the Windows VM. 

To download and install the Universal Forwarder on the Windows VM:
- Go to the following link: <a href="https://www.splunk.com/en_us/download/universal-forwarder.html">Download Universal Forwarder for Remote Data Collection</a>.
- Choose the Windows 64-bit package to download and install:
  ![image](https://github.com/user-attachments/assets/bdf3292b-0124-4aec-9679-e4905714c487)
- Accept the agreement and click "Access Program".
- This will download the package.
- Once the download is complete, click on the file to set up the Universal Forwarder.
- Tick the box to accept the License Agreement and select "An on-premises Splunk Enterprise instance" to use this Universal Forwarder with:
  ![image](https://github.com/user-attachments/assets/9cd1f555-225a-4057-889c-a24c2512debe)
- You will then be asked to create the credentials for the administrator account. Use the same credentials that you have made for the Splunk Enterprise instance:

  ![image](https://github.com/user-attachments/assets/10284a29-d77a-4345-bce5-dcbc2a6d0477)
- Skip the "Deployment Server" page and this will take you to the "Receiver Indexer" page. You can use the IP address for the Ubuntu VM and the default port number 9997. This will remotely send logs to the Splunk Enterprise instance:

  ![image](https://github.com/user-attachments/assets/e243dbef-5d92-4404-9198-72ecec7f5a5e)
- Complete the installation.
- Once it is installed, go to the Control Panel to verify the installation:
  ![image](https://github.com/user-attachments/assets/aca0abef-cb16-4a6e-b8b3-4ffc2717849f)
- Open Powershell and run it as administrator.
- Change the directory using the "cd" command to the directory where the Universal Forwarder is which is `C:\Program Files\SplunkUniversalForwarder\bin`.
- Start the Universal Forwarder using the following command:

  `.\splunk start`

  ![image](https://github.com/user-attachments/assets/b641d0cc-68b5-4b0c-ba64-59d48cc46b83)

- Connect the Universal Forwarder to the receiving indexer using the following command:

  `./splunk add forward-server <host name or ip address>:<listening port>`
- For example:

  `./splunk add forward-server <Ubuntu-VM-IP>:9997`


- Restart the Universal Forwarder using the following command:

  `.\splunk restart`

### Configuring The Receiver

- In the Ubuntu VM, go to the Splunk Web on the web browser using the Splunk Web interface: `http://ubuntu-1-VirtualBox:8000`:
  ![image](https://github.com/user-attachments/assets/0cf3709e-ed67-4df3-919c-ea3bb11a1cf2)
- Go to "Settings", then to "Forwarding and receiving":
  ![image](https://github.com/user-attachments/assets/b692029b-233b-4265-af3e-69df581b5ff3)
- Click "Configure receiving", and then click "New receiving port":
  ![image](https://github.com/user-attachments/assets/3962cb88-ba0e-449b-88a6-5208c4e6cfeb)
- Add port 9997 as the receiving port and then click save. This will be the port the Receiver listens on to collect and ingest data from the host.
- Restart Splunk for the changes to take effect using the following command:

  `sudo /opt/splunk/bin/splunk restart`
- Once restarted, log back into the Splunk Web.
- To verify that Splunk is listening to logs, run the following command in the Terminal:

  `sudo netstat -tulnp | grep 9997`
  ![image](https://github.com/user-attachments/assets/9833eaea-7e90-46af-b33d-a3fc791587dc)

### Configuring The Deployment Client

On the Splunk web interface, when you go to "Settings", then to "Add Data", and when you choose "Forward" as the data source, the Universal Forwarder for the Windows VM does not show up on the list. This may be because the deployment server is not configured. The deployment server needs to be specified on the host where the Universal Forwarder is installed so that the said host can be classified as a deployment client. 

To fix this issue:
- On the Windows VM, go to Powershell and run it as administrator.
- Change the directory to `C:\Program Files\SplunkUniversalForwarder\bin`.
- The IP address of the deployment server is the Ubuntu VM because the Splunk Web server is running on that virtual machine. The default management port is 8089. To connect the Universal Forwarder to the deployment server, run the following command:

  `./splunk set deploy-poll <Ubuntu-VM-IP>:8089`
- Once the configuration is set, restart the Universal Forwarder using the following command:

  `.\splunk restart`
- On the Ubuntu VM, run the following command on the Terminal to verify that the Windows Universal Forwarder has now been set as the deployment client:

  `sudo /opt/splunk/bin/splunk list deploy-clients`
  ![image](https://github.com/user-attachments/assets/2dc91a04-5179-4738-ad6f-a4058e230b25)
- As shown above, the Windows Universal Forwarder has now been set as the deployment client.
- When you return to the Splunk Web to add the Windows Universal Forwarder as the data source, it now comes up on the list:
  ![image](https://github.com/user-attachments/assets/256fdae5-65e4-4c6d-bc6b-b618c79c4f60)

### Collecting Windows Logs

- Go to the Splunk Web in the Ubuntu VM.
- Go to "Settings", then to "Indexes":

  ![image](https://github.com/user-attachments/assets/99d4bfe7-275f-4529-8fe4-fa2c67fb775d)
- On the "Indexes" page, click "New Index".
- The index name should be "wineventlog":
  ![image](https://github.com/user-attachments/assets/53784c6a-20d4-4e7a-b0dc-ca4ef0af84c1)
- The rest of the configurations should remain as they are. Click "Save".
- Next, go to "Settings", then to "Add Data", and then select "Forward" as the data source.
- Select the Windows Universal Forwarder that was created earlier. Create a new server class name:
  ![image](https://github.com/user-attachments/assets/e2ffffcb-37e2-4d78-9d4a-a7256e3f00f7)
- On the "Select Source" page, choose "Local Event Logs" as the source, and then select "Application", "ForwardedEvents", "Security", and "System":
  ![image](https://github.com/user-attachments/assets/d8c96405-0a18-459e-85a9-c8431e5d6aea)
- On the "Input Settings" section, select "wineventlog" as the index:
  ![image](https://github.com/user-attachments/assets/92db18ef-7388-42e8-9502-8debfb20250d)
- Review the following before submitting it:
  ![image](https://github.com/user-attachments/assets/fe56b789-3c6b-41f4-b714-4cfad47617e3)
- The data for the local event logs from the Windows VM has been successfully added:
  ![image](https://github.com/user-attachments/assets/8980ee27-90c9-41f1-b84b-08a617619c84)
- You can now search for data, build dashboards etc.
- When you search for Event Log data from the Windows VM, this is what comes up:
  ![image](https://github.com/user-attachments/assets/d1a3a060-e266-4ad5-be07-6e2ae84d5b7f)
- This shows that the Splunk Web server is listening to the Universal Forwarder on the Windows VM. 


## Simulate Brute-Force Log In Attempts

The next step is to simulate brute-force login attempts. To do this:
- Log out of the Windows VM.
- Log in using different random passwords until you use the right password.
- On the Windows VM, go to Event Viewer, then to Windows logs > Security.
- The Event ID for failed logons is 4625. In the Security event logs, click "Find" under the "Actions" tab. Type in "4625" to search for logs relating to Event ID 4625:
  ![image](https://github.com/user-attachments/assets/6546cf27-fe9f-4fb5-90b3-baad9088c503)
- Four failed login attempts were simulated therefore the Event Viewer has four Event ID 4625 logs shown:
  ![image](https://github.com/user-attachments/assets/a6e5d6b9-7447-46f0-a24f-1ca420f6b50c)
- When you click on each Event ID 4625 log for more details, the log describes that the account failed to log on, indicating that it was a failed login attempt:

  ![image](https://github.com/user-attachments/assets/18fc587c-c787-4ca7-9713-0498873493af)
- To verify if the Event ID 4625 logs appear on the Splunk Web server, go the search and type in the following query:

  `index=* sourcetype=WinEventLog:Security EventCode=4625`
- The search results on Splunk also show that there are four Event ID 4625 logs:
  ![image](https://github.com/user-attachments/assets/3e0a7f1b-276a-40b2-939a-f22de7d72b19)
- Here are more details about each event:
  ![image](https://github.com/user-attachments/assets/4f892438-d91c-4f6f-a108-e0cb89a0021a)

## Creating Alerts

On the Ubuntu VM:
- Go to the Splunk Web server.
- Go to "Search & Reporting".
- Type in the following query in the search bar and then click the search button:

  `index=* sourcetype=WinEventLog:Security EventCode=4625`
- On the top right corner, click "Save As" and then select "Alert":
  ![image](https://github.com/user-attachments/assets/27911f5e-c86c-43b5-bdff-4730497cf2a6)
- You will be asked to name this alert along with selecting the alert type, expiry, trigger conditions, and trigger actions:
  ![image](https://github.com/user-attachments/assets/fe72a0b8-83ef-41ca-a727-2d57646b0066)
- Add the following information to this Alert:
  - Title: Brute Force Login Attempt
  - Alert type: Real-time
  - Expires: 24 hours
  - Trigger alert when: Per-Result
  - Trigger Actions:
    - Send email
      - Priority: High
      - Include:

        ![image](https://github.com/user-attachments/assets/b630fc11-8def-4c30-acd6-1bf77c7e9651)
    - Add to Triggered Alerts:
      - Severity: High

    ![image](https://github.com/user-attachments/assets/c336b182-55c1-455a-bb19-86602a34a21e)

- Save the alert.

## Triggering Alerts

The next step is to attempt another brute-force login on the Windows VM. After each attempt, an alert will trigger, and relevant recipients will receive an automated email.







  





  







  



  



  







## References
- <a href="https://www.splunk.com/en_us/blog/learn/splunk-universal-forwarder.html">Splunk Universal Forwarder</a> 
- <a href="https://docs.splunk.com/Documentation/Forwarder/9.4.0/Forwarder/InstallaWindowsuniversalforwarderfromaninstaller"> Install a Windows universal forwarder</a>
- <a href="https://docs.splunk.com/Documentation/Forwarder/9.4.0/Forwarder/Startorstoptheuniversalforwarder"> Start or stop the universal forwarder</a>
- <a href="https://docs.splunk.com/Documentation/Forwarder/9.4.0/Forwarder/Enableareceiver"> Enable a receiver for Splunk Enterprise</a>
- <a href="https://docs.splunk.com/Documentation/Forwarder/9.4.0/Forwarder/Configuretheuniversalforwarder"> Configure the universal forwarder using configuration files</a>
- <a href="https://docs.splunk.com/Documentation/Splunk/9.4.0/Updating/Configuredeploymentclients"> Configure deployment clients</a>
- <a href="https://youtu.be/yP_PFRy-pdA?si=8MUBPv_FlrtNuVg0"> Cybersecurity Detection Lab: Forwarding Windows Event Logs to Splunk Using Universal Forwarder</a>
- <a href="https://www.manageengine.com/products/active-directory-audit/kb/windows-security-log-event-id-4625.html"> Windows Event ID 4625 – Failed logon</a>
- <a href="https://docs.splunk.com/Documentation/Splunk/9.4.0/Alert/DefineRealTimeAlerts"> Create real-time alerts</a>


  
