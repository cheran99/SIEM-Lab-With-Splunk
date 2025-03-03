# SIEM-Lab-With-Splunk

## Table of Contents
- [Introduction](#introduction)
  - [Objectives](#objectives)
- [Tools Used](#tools-used)
- [Prerequisites](#prerequisites)
  - [System Requirements](#system-requirements)
    - [Recommended Hardware](#recommended-hardware)
    - [Recommended Virtual Machine Requirements](#recommended-virtual-machine-requirements)
  - [Software Requirements](#software-requirements)
- [Project Setup](#project-setup)
  - [Downloading and Installing Splunk Enterprise on Ubuntu VM](#downloading-and-installing-splunk-enterprise-on-ubuntu-vm)
  - [Downloading and Installing the Universal Forwarder](#downloading-and-installing-the-universal-forwarder)
  - [Configuring The Receiver](#configuring-the-receiver)
  - [Configuring The Deployment Client](#configuring-the-deployment-client)
  - [Collecting Windows Logs](#collecting-windows-logs)
- [Simulate Brute-Force Log In Attempts](#simulate-brute-force-log-in-attempts)
- [Creating Alerts](#creating-alerts)
- [Triggering Alerts](#triggering-alerts)
- [Building Basic Dashboards](#building-basic-dashboards)
  - [Statistics Table](#statistics-table)
  - [Pie Chart](#pie-chart)
  - [Column Chart](#column-chart)
- [Conclusion](#conclusion)
- [References](#references)

## Introduction

Detection and response to suspicious activities, such as brute-force attacks, are crucial to protecting the digital landscape. Because of this, a SIEM solution is essential to streamline log collection, enhance threat detection, and support incident response. This project highlights the setup of a SIEM lab using Splunk Enterprise to collect Windows logs and detect suspicious activity such as brute-force logins.

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

## Prerequisites

### System Requirements

#### Recommended Hardware
- CPU: Minimum 4 cores (Intel i5/i7 or AMD Ryzen 5/7)
- RAM: Minimum 16GB especially if you are running multiple virtual machines
- Storage: Minimum 50GB free disk space
- OS: Windows 10 or later/Ubuntu 20.04 or later/macOS 11 or later

#### Recommended Virtual Machine Requirements

- Ubuntu VM (Splunk Server)
  - CPU: 2 cores
  - Memory: 4GB RAM
  - Storage: 25GB

- Windows 10 VM (Splunk Universal Forwarder)
  - CPU: 2 cores
  - Memory: 4GB RAM
  - Storage: 25GB

### Software Requirements

- Virtualisation Software:
  - The platform to run the virtual machines can be one of the following:
    - VirtualBox
    - VMWare 
- Splunk Enterprise (Free Trial or Paid Version)
- Splunk Universal Forwarder 



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
  - Throttle: Yes
  - Suppress results containing field value: *
  - Suppress triggering for: 30 minutes
  - Trigger Actions:
    - Send email
      - Priority: High
      - Include:

        ![image](https://github.com/user-attachments/assets/b630fc11-8def-4c30-acd6-1bf77c7e9651)
    - Add to Triggered Alerts:
      - Severity: High

    ![image](https://github.com/user-attachments/assets/88498222-e45e-4736-8178-c16f52e6c576)


- Save the alert.

## Triggering Alerts

The next step is to attempt more brute-force logins on the Windows VM. After each attempt, an alert will trigger, and relevant recipients will receive an automated email. After several failed attempts, Splunk sent an automated email to the relevant recipients:

![image](https://github.com/user-attachments/assets/9f8313e0-13e2-4c2d-8f6d-385a4949bdff)
![image](https://github.com/user-attachments/assets/660dd424-7ed4-4c03-8bd2-f42df9aa1298)

On the Splunk Web, when you go to "Activity", then to "Triggered Alerts", the results show the number of times a failed login triggered an alert:

![image](https://github.com/user-attachments/assets/9e71ead0-1553-4968-b1a9-6fe075dfaff6)

The triggered alerts also show the severity level for this specific alert. 

## Building Basic Dashboards

The next step is to create dashboards. To do this:
- Go to "Search & Reporting" on the Splunk Web.
- Go to the "Dashboards" section:

  ![image](https://github.com/user-attachments/assets/c2595acb-7208-42c2-b3d9-0d5fc9158151)
- Click "Create New Dashboard":
- Add the following information:
  - Dashboard Title: Brute Force Activity
  - Classic Dashboard

  ![image](https://github.com/user-attachments/assets/05a13ab8-ee38-476e-9d7c-6d3dc9feb1da)
- Click "Create".
- This will create a blank page for this specific dashboard:

  ![image](https://github.com/user-attachments/assets/07ca6f1d-6d6e-45e3-b776-2d3e4fb82b50)

### Statistics Table

To create a statistics table that shows brute-force login activities in the "Brute Force Activity" dashboard:
- Click "Add Panel", then "New", and then "Statistics Table":
  - Add the following information:
    - Content Title: Table of Brute-Force Logins
    - Search String:
      ```
      index=* sourcetype=WinEventLog:Security EventCode=4625
      | table _time, host, EventCode, Failure_Reason
      | sort -_time
      ```
    - Click "Add to Dashboard".
- This is what the dashboard looks like for the table of brute-force logins:

  ![image](https://github.com/user-attachments/assets/df1b319b-2ea1-4d5e-827b-5866b059b4ea)

### Pie Chart

To create a pie chart that shows successful and unsuccessful logins:
- Click "Add Panel", then "New", and then "Pie Chart":
  - Add the following information:
    - Content Title: Successful & Unsuccessful Logins
    - Search String:
      ```
      index=* sourcetype=WinEventLog:Security (EventCode=4625 OR EventCode=4624)
      | eval Status=if(EventCode=4624, "Successful Login", "Failed Login")
      | stats count by Status
      ```
    - Click "Add to Dashboard".
- Here is the pie chart for successful and unsuccessful logins:

  ![image](https://github.com/user-attachments/assets/41720e7c-1d1a-4929-a6ae-eb58f8514404)

### Column Chart

In order to create a column chart that illustrates the timeline of successful and unsuccessful logins:
- Click "Add Panel", then "New", and then "Column Chart":
  - Add the following information:
    - Content Title: Timeline of Successful & Unsuccessful Logins
    - Search String:
       ```
       index=* sourcetype=WinEventLog:Security (EventCode=4625 OR EventCode=4624)
       | eval Status=if(EventCode=4624, "Successful Login", "Failed Login")
       | timechart span=1h count by Status
    - Click "Add to Dashboard".
- Here is the column chart that shows the timeline of successful and failed logins:

  ![image](https://github.com/user-attachments/assets/2c19470c-ee27-4444-ba99-3e116a32c6bb)


## Conclusion

A SIEM lab using Splunk was successfully implemented to detect and analyse brute-force login activity from the Windows VM. The Universal Forwarder was installed and configured on the Windows VM thereby allowing logs from the said machine to be remotely sent to the Splunk server. 

A series of brute-force logins were simulated against the Windows VM. The search query on the Splunk Web was used to streamline logs relating to brute-force activity and create an alert rule that gets triggered when another brute-force login happens. As a result, and automated email is sent to relevant recipients. 

Furthermore, a dashboard was implemented to visualise the logs relating to brute-force activities.
















  





  







  



  



  







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
- <a href="https://youtu.be/8jvEmAmQNug?si=tCZR3tHukvwUm7-N"> Creating Alerts in Splunk Enterprise</a>
- <a href="https://youtu.be/tkSZws7vBEo?si=1ZHbG-NZ1V-7gzfo"> How to create Splunk Dashboard | Cybersecurity</a>
- <a href="https://docs.splunk.com/Documentation/Splunk/9.4.0/SearchTutorial/Startsearching"> Basic searches and search results</a>
- <a href="https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4624"> 4624(S): An account was successfully logged on.</a>
- <a href="https://kinneygroup.com/blog/splunk-search-command-of-the-week-stats/"> Using the stats Command</a>
- <a href="https://docs.splunk.com/Documentation/SCS/current/SearchReference/EvalCommandOverview"> SPL2 Search Reference: eval command overview</a>
- <a href="https://docs.splunk.com/Documentation/Splunk/9.4.0/Viz/PieChart"> Dashboards and Visualizations: Pie chart</a>
- <a href="https://docs.splunk.com/Documentation/Splunk/9.4.0/DashStudio/chartsPie"> Splunk Dashboard Studio: Pie charts</a>
- <a href="https://docs.splunk.com/Documentation/Splunk/9.4.0/DashStudio/chartsBar"> Splunk Dashboard Studio: Bar and column charts</a>
- <a href="https://docs.splunk.com/Documentation/SCS/current/SearchReference/TimechartCommandExamples"> SPL2 Search Reference: timechart command examples</a>


  
