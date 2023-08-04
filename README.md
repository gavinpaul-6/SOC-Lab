# SOC-Lab
Cybersecurity Detection &amp; Monitoring Lab

When I was pursuing a career in IT back in 2022 I realized that the best way to learn is to lab.

For aspiring and professional security analysts that are already in the cybersecurity field, having a home lab is crucial for gaining familiarity with tools, configurations, and simulating attacks.

The home lab I configured was inspired by Day Cyberwox's Cybersecurity and Detection Monitoring lab. I also made additional modifications to simulate a hybrid environment in the cloud using Microsoft Azure.

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

https://github.com/Security-Onion-Solutions/securityonion/blob/master/VERIFY_ISO.md 

