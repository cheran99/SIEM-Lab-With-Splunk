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

On the Splunk web interface, when you go to "Settings", then to "Add Data", and when you choose "Forward" as the data source, the Universal Forwarder for the Windows VM does not show up on the list. This may be because the deployment server is not configured. The deployment server needs to be specified on the host where the Universal Forwarder is installed so that the host itself can be classified as a deployment client. 

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








  



  



  







## References
- <a href="https://www.splunk.com/en_us/blog/learn/splunk-universal-forwarder.html">Splunk Universal Forwarder</a> 
- <a href="https://docs.splunk.com/Documentation/Forwarder/9.4.0/Forwarder/InstallaWindowsuniversalforwarderfromaninstaller"> Install a Windows universal forwarder</a>
- <a href="https://docs.splunk.com/Documentation/Forwarder/9.4.0/Forwarder/Startorstoptheuniversalforwarder"> Start or stop the universal forwarder</a>
- <a href="https://docs.splunk.com/Documentation/Forwarder/9.4.0/Forwarder/Enableareceiver"> Enable a receiver for Splunk Enterprise</a>
- <a href="https://docs.splunk.com/Documentation/Forwarder/9.4.0/Forwarder/Configuretheuniversalforwarder"> Configure the universal forwarder using configuration files</a>
- <a href="https://docs.splunk.com/Documentation/Splunk/9.4.0/Updating/Configuredeploymentclients"> Configure deployment clients</a>

  
