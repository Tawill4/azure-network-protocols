<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Video Demonstration</h2>

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/lAb7Nz2.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/ODQt6Hc.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a Resource Group, where both of the VM's will go. Then create a new Virtual Machine running windows 10, and make sure it is in the newly created resource group. Create a username and password, click the check box at the very bottom. Click next until you reach "Networking" and note that it created a Virtual Network for the VM. Press review and create to finish.
</p>
<br />

<p>
<img src="https://i.imgur.com/F1oWCOG.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/rTBOdAC.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/3MFE8DP.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next create a second VM running Ubuntu / linux. Change the authentication to Username/Password from SSH. Anything around 2vcpus is good. Go to the networking tab and make sure the V-net and subnet are the same as the first VM.. also make sure they're in the same region. (I have both in East US 2).
</p>
<br />

<p>
<img src="https://i.imgur.com/O2Eyo53.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/qQ98bT1.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/xnWwJop.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now connect to the Windows 10 VM using Remote Desktop by getting the public IP address of the Vm and pasting it in the windows app and logging in. After loggining into the VM go to the browser and download wireshark.
</p>
<br />

<p>
<img src="https://i.imgur.com/kTLlNv5.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/DWWYTZN.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/AVsoxCt.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open wireshark and filter for ICMP traffic only. Then grab the Ubuntu VM's private IP address and ping it inside of command prompt.. then go to wireshark and observe the traffic (in purple).
</p>
<br />

<p>
<img src="https://i.imgur.com/kTLlNv5.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/DWWYTZN.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/AVsoxCt.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/AVsoxCt.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open wireshark and filter for ICMP traffic only. Then grab the Ubuntu VM's private IP address and ping it inside of command prompt.. then go to wireshark and observe the traffic (in purple). Take it a step further and also ping a website like (www.google.com) and observe the same type of traffic.. In wireshark you should see requests AND replies if it is working.
</p>
<br />

<p>
<img src="https://i.imgur.com/LkYNKZx.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/ewW2lYO.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/5xQ1VRY.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/wlKJsnl.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/JpMQESi.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
Initiate a constant ping of the Ubuntu VM's private IP address, then go back into azure -> Network Security Groups -> Add -> Deny incoming traffic from ICMP traffic. Make sure you put the priority as 200 or anything below the lowest one... this makes the VM aknowledge that rule before anything else. After adding the security rule, go back to the Windows 10 VM and observe the ping saying "request timed out" and wireshark only sending requests with no replies. From here go back and delete the security rule, and once again return to the windows VM to see that the ping is working again, as well as wireshark getting replies.
</p>
<br />

<p>
<img src="https://i.imgur.com/zkwTQR9.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/HYgxLnB.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
Back in the Windows 10 VM, change the Wireshark filter to only show DHCP traffic. Then open Powershell as an administrator to try and renew the IP address.. do this by running this command; "ipconfig /renew", note the request and ack traffic in wireshark.
</p>
<br />
