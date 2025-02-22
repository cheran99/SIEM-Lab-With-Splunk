# SIEM-Lab-With-Splunk

## Introduction

Detection and response of suspicious activities such as brute force attacks is crucial towards protecting the digital landscape. Because of this, a SIEM lab is essential to streamline threat detection and take the necessary steps to mitigate potential threats. This projects highlights the set up of a SIEM lab using Splunk Enterprise.

### Objectives
1. Download and install Splunk on your local machine/virtual machine
2. Configure Splunk to collect and ingest log data from host operating systems
3. Create custom threat detection rules
4. Simulate suspicious activities such as brute force logins
5. Build dashboards to show contents of monitored logs

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

### Downloading and Installing Universal Forwarder

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
- Skip the "Deployment Server" page and this will take you to the Receiver Indexer page. Use the IP address for the Ubuntu VM and the default port number 9997:
  ![image](https://github.com/user-attachments/assets/e243dbef-5d92-4404-9198-72ecec7f5a5e)
- Complete the installation. 



  







## References
- https://www.splunk.com/en_us/blog/learn/splunk-universal-forwarder.html 


  
