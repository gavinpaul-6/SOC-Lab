# Setting up my Cybersecurity Detection &amp; Monitoring Lab


When I was pursuing a career in IT back in 2022 I realized that the best way to learn is to lab.

For aspiring and professional security analysts that are already in the cybersecurity field, having a home lab is crucial for gaining familiarity with tools, configurations, and simulating attacks.

The home lab I configured was inspired by Day Cyberwox's Cybersecurity and Detection Monitoring lab. I also made additional modifications to simulate a hybrid environment in the cloud using Microsoft Azure. This is me detailing my experience setting up this lab and less so a walkthrough on how to configure the lab. Check out Day's Cyberwoxacademy website for more details on how to complete this setup: https://cyberwoxacademy.com/building-a-cybersecurity-homelab-for-detection-monitoring/

<h2>Hardware Specs for the Host PC:</h2>

- CPU: AMD Octa-Core Ryzen 7 5800X
- RAM: 64 GB
- Storage: 2 TB SSD
- Graphics Card:  GeForce RTX 3070 8 GB
- Motherboard: Asus G15DK
- Host Operating System: Windows 11 Pro


In late June, I decided to invest in a gaming PC that I could (obviously) play games with but also to build out the labs for my cybersecurity career. I could have used my laptop which had 16GB of RAM, but I wanted a powerhouse to build out beefier labs in the future. If you have the funds I highly recommend investing in one. Unfortunately, the link I used to purchase it from Amazon is no longer available.

<p align="center">
<img src="https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/94dc0435-8ff5-4edd-b1b8-d8059460cca1" width="400" height="800" />
</p>

<h2>Part 1: Configuring pfSense</h2>

I chose VMWare Workstation Pro as my hypervisor of choice. VirtualBox is another option.

Next, was to install and configure <b>pfSense</b> as the firewall to segment the network and route traffic which will only be accessible from the Kali Linux machine.

I configured pfSense as shown in the screenshot:

![2023-07-02_14-05-40](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/8ed26011-2362-415c-ba4d-470d97409ea2)

I added 5 network adapters and correspond them with VMnet interfaces.

- NAT - This is the connection for the virtual environment to the host PC and Internet.
- VMNet2 - This is the connection to the Kali VM which will be the attack machine and management interface for pfSense.
- VMNet3 - This is the connection to the Victim Domain Active Directory network which will simulate a corporate environment.
- VMNet4 - This is the connection to the all-in-one IDS Security Onion instance.
- VMNet5 - This is the span port connection that receives the frames from the interface connected to the victim domain network.
- VMNet6 - This is the connection to the Splunk instance which is our SIEM that will be ingesting Windows logs from the domain controller on the victim network.


<h2>Part 2: Configuring SecurityOnion and SOC Analyst Machine</h2>

The next step was to install Security Onion, the all-in-one IDS, Security Monitoring, and Log Management solution.

I installed the Security Onion ISO from here: https://github.com/Security-Onion-Solutions/securityonion/blob/master/VERIFY_ISO.md 

![2023-07-02_14-46-01](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/efb885b3-9625-456e-8bff-a544ae79d591)

I configured SecurityOnion as shown in the screenshot:


![2023-08-04_10-56-23](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/d62b059f-27a6-44a1-a3b8-8aedf6443807)

After finishing the Security Onion installation, I needed a web interface to connect to Security Onion using the Management IP given.

![2023-07-02_15-43-59](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/239d6c7e-a948-4a7a-ab2b-ebcecc7e18e2)


I downloaded and installed Ubuntu Desktop as my Security Analyst machine to simulate a SOC/Security Analyst accessing a SIEM. In this scenario to access Security Onion's web interface.

![2023-07-02_16-11-19](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/3ad77626-8685-4d84-b78d-99fe4105cb5d)

![2023-07-03_10-18-40](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/5d590acc-acec-4cbf-95ca-1905f2dc37f9)

![2023-07-03_10-17-27](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/9a672f3c-1838-4029-8270-5295a5e1247c)


<h2>Part 3: Configuring Kali Linux as the Attack Machine</h2>

Next, I installed Kali Linux and before powering it on I changed the network adapter to VMNet2 and changed the memory to 4GB, then power it on and use default credentials. After powering the VM, I changed the default password in the terminal.

![2023-07-02_16-27-14](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/2cc51854-b589-4963-8d7b-c0595bb1c33b)

I navigated to Firefox web browser and entered 192.168.1.1 and clicked 'Advanced' to access the pfSense WebConfigurator to make changes to the pfSense interface and firewall rules.

![2023-07-02_23-29-20](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/6dcd7264-fbaa-4efd-a5af-71dd3a7d83ec)
![2023-07-02_23-42-34](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/17167177-1a7e-406a-a1a5-1d818cc3129f)

<h2>Part 4: Configuring Windows Server 2019 as a Domain Controller</h2>

This portion of the lab is setting up an Active Directory domain with Windows Server 2019 as the Domain Controller and a Windows 10 machine.
After completing the setup, I renamed the Domain Controller and restarted. After rebooting I installed Active Directory Domain Services and configured Active Directory Certificate Services on the destination server from the Server Manager dashboard.


<h2>Part 5: Creating Users with Powershell</h2>

For this part of the lab, I wanted to replicate a small-medium business Active Directory environment with a bunch of users. This wasn't necessary for this lab, but I wanted to make it as realistic with 1000 users instead of 2 or 3. I utilized a Powershell script from Josh Madakor's AD Lab on YouTube to generate users. I also had invested in two Powershell books last year to help me learn more. 

The two book resources I recommend for this:
- Learn Powershell in a Month of Lunches by Travis Plunk and James Petty
- Learn Powershell Scripting in a Month of Lunches by Dom Jones and Jeffery Hicks

![2023-07-16_18-29-39](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/dc8252d2-8177-47c4-a68e-ba97cf1e5e17)

![2023-07-16_18-37-43](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/af26d541-6eb6-49e5-a08b-a2bcd349caf0)


<h2>Part 6: Setting up Windows 10 PC and Adding to AD Domain </h2>

After generating the users, I installed a Windows 10 VM and added it to VMNet3 (Victim Domain).

I navigated to Network Adapter settings, right clicked on Ethernet, selected Properties.

Selected IPv4 and added IP Address <b>192.168.2.1</b> as the default gateway and <b>192.168.2.10 (Victim Network)</b> as the DNS Server.

Search "domain" in the search bar and select Access work or school option.

Selected 'Connect' > 'Join this device to local Active Directory Domain

Enter domain name.local (GAVINPAUL.local for me)
![2023-07-03_01-07-55](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/d5ffa57b-9fc5-40e5-a70e-995551d06fa5)

![2023-07-03_01-08-20](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/bee07866-5f59-4c8d-82a1-e771fdb71a2e)


<h2>Part 7: Installing Splunk on Ubuntu Server </h2>

The next step was setting up my Splunk instance. This was pretty straightforward. I installed Ubuntu Server from Ubuntu.com and followed the steps. To access the GUI, I needed to install tasksel and reboot.

Once I was able to access the GUI and get past the initial setup, I navigated to Firefox to install Splunk from their website.

![2023-07-03_01-31-52](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/8305afe5-0fa5-4c3b-bb57-e13141c8740d)

![2023-07-03_01-52-12](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/0065797d-4d89-4a61-a7bd-ac977a63079b)



![2023-07-03_02-08-25](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/3393b2e2-01a6-48e8-9dca-f54b34a5c096)

After installing Splunk to my Downloads folder, I unzipped it using the tar command using the following:

tar -xvzf splunk

I navigated to splunk > bin and accepted the terms and created admin credentials to log into Splunk.

![2023-07-03_02-11-39](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/1aa052fb-b9f6-4c3f-a314-a410cc25a1e6)

Once Splunk is up in running, I navigated to web server URL using Firefox and logged into Splunk.

![2023-07-03_02-14-46](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/21c92a8d-d2ab-4e1d-b858-e9ac41a9447e)


![2023-07-03_02-21-09](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/2651ed82-621a-413b-8fc9-bf8945ec4675)



<h2>Part 8: Installing Universal Forwarder on Windows Server </h2>

Now that Splunk was installed, the next step was to install the Universal Forwarder to log the activities from my Domain Controller to the Splunk SIEM.

I navigated back to Windows Server VM and installed Google Chrome before downloading the Splunk Universal Forwarder. 

![2023-07-03_02-37-34](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/184e0109-1312-44ca-a176-4c4d2a48fa3d)

![2023-07-03_02-40-16](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/d72335b3-fe9d-4ad9-98f3-d90815b8256b)

- Once downloaded, I logged back into my Splunk machine and logged into Splunk and navigated to Settings > Forward and Receiving > Configure receiving. 

- Clicked the top right green 'New Receiving Port' button.

- Enter 9997 and then click save so Splunk will listen for inbound connections from the Universal Forwarder on port 9997.

The next step was to create an index for the Windows Event Logs. A Splunk index is a repository for Splunk data. Data that has not been previously added to Splunk is referred to as raw data. When the data is added to Splunk, it indexes the data (uses the data to update its indexes), creating event data. Individual units of this data are called events.

![2023-07-03_02-21-09](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/2651ed82-621a-413b-8fc9-bf8945ec4675)

