# SOC Lab
Cybersecurity Detection &amp; Monitoring Lab

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


<h2>Part 2: Configuring SecurityOnion</h2>

The next step was to install Security Onion, the all-in-one IDS, Security Monitoring, and Log Management solution.

I installed the Security Onion ISO from here: https://github.com/Security-Onion-Solutions/securityonion/blob/master/VERIFY_ISO.md 

![2023-07-02_14-46-01](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/efb885b3-9625-456e-8bff-a544ae79d591)

I configured SecurityOnion as shown in the screenshot:


![2023-08-04_10-56-23](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/d62b059f-27a6-44a1-a3b8-8aedf6443807)

After finishing the Security Onion installation, I needed a web interface to connect to Security Onion using the Management IP given.

![2023-07-02_15-43-59](https://github.com/gavinpaul-6/SOC-Lab/assets/98987388/239d6c7e-a948-4a7a-ab2b-ebcecc7e18e2)


COMING SOON...






