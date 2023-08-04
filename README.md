# Setting up my Cybersecurity Detection &amp; Monitoring Lab


When I was pursuing a career in IT back in 2022 I realized that the best way to learn is to lab.

For aspiring and professional security analysts that are already in the cybersecurity field, having a home lab is crucial for gaining familiarity with tools, configurations, and simulating attacks.

The home lab I configured was inspired by Day Cyberwox's Cybersecurity and Detection Monitoring lab. I also made additional modifications to simulate a hybrid environment in the cloud using Microsoft Azure. This doesn't go over all of the step-by-step details of how to configure the lab. Check out Day's Cyberwoxacademy website. https://cyberwoxacademy.com/building-a-cybersecurity-homelab-for-detection-monitoring/

<h2>Hardware Specs for the Host PC:</h2>

- CPU: AMD Octa-Core Ryzen 7 5800X
- RAM: 64 GB
- Storage: 2 TB SSD
- Graphics Card:  GeForce RTX 3070 8 GB
- Motherboard: Asus G15DK
- Host Operating System: Windows 11 Pro

In late June, I decided to invest in a gaming PC that I could (obviously) play games with but also to build out the labs for my cybersecurity career. I could have used my laptop which had 16GB of RAM, but I really wanted a powerhouse to build out beefier labs in the future. If you have the funds I highly recommend investing in one. I would link to where I purchased my PC but the link is no longer available as of August 3rd.

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
After completing the setup, I renamed the Domain Controller and restarted. After rebooting I installed Active Directory Domain Services from the Server Manager dashboard.



