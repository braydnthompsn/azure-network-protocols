<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this lab, we'll check out different types of network traffic to and from Azure Virtual Machines using Wireshark. We'll also play around with Network Security Groups. <br />


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

- Create a Resource Group
- Create a Virtual Machine
- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Actions and Observations</h2>
</br>
</br>
<h3 align="center">
  Set up your virtual environment
</h3>
</br>
<p>
  First, we'll create a Resource Group in the Azure Portal.
</p>
<p>
  <img src="https://i.imgur.com/dOAeXqs.png" height="75%" width="100%" alt="Resource Group"/>
</p>
<p>
  Create a Windows virtual machine. In this lab, I did mine in East US.
</p>
<p>
  When you make your VM, choose the Resource Group that was just created and let it make a new vNet and Subnet. Be sure to use the password option under the Administrator Account section:
</p>
<p>
  <img src="https://i.imgur.com/PHOwjLh.png" height="75%" width="100%" alt="Windows VM"/>
</p>
<p>
  Now create an Ubuntu virtual machine.
</p>
<p>
  When you make this VM, pick the Resource Group that we just created and let it create a new vNet and Subnet. Use the password option under the Administrator Account section:
</p>
<p>
  <img src="https://i.imgur.com/N5zwQUH.png" height="75%" width="100%" alt="Ubuntu VM"/>
</p>
<p>
  Check out the Virtual Network using Network Watcher:
</p>
<p>
  <img src="https://i.imgur.com/Pn02GXF.png" height="75%" width="100%" alt="Network Watcher"/>
</p>
<br />
<br />
<h3 align="center">
  Lets look at ICMP traffic
</h3>
<br />
<p>
  -Remote into your Windows 10 Virtual Machine
</p>
  -Install Wireshark, open it
</p>
  -Filter for ICMP traffic only.
</p>
<p>
  <img src="https://i.imgur.com/0BsfNiS.jpg" height="75%" width="100%" alt="Microsoft Remote Desktop - Mac"/>
</p>
<p>
  Get the private IP address of the Ubuntu VM and ping it from within the Windows 10 VM. Look at ping requests and replies within WireShark:
</p>
<p>
  <img src="https://i.imgur.com/yYGKuAy.png" height="75%" width="100%" alt="Ubuntu private IP"/>
  <img src="https://i.imgur.com/3h9QSEY.png" height="75%" width="100%" alt="ICMP traffic - private IP"/>
</p>
<p>
  Ping a public website and observe the traffic in WireShark:
</p>
<p>
  <img src="https://i.imgur.com/YduMvc7.png" height="75%" width="100%" alt="ICMP traffic - public IP"/>
</p>
<p>
  Initiate a perpetual ping from your Windows 10 VM to your Ubuntu VM:
</p>
<p>
  <img src="https://i.imgur.com/bihftKK.png" height="75%" width="100%" alt="ICMP traffic - perpetual ping"/>
</p>
<p>
  Open the NSG that your Ubuntu VM is using and disable inbound ICMP traffic. Head back to the Windows 10 VM and observe the ICMP traffic in WireShark and the command line Ping activity:
</p>
<p>
  <img src="https://i.imgur.com/ovGk5dq.png" height="75%" width="100%" alt="ICMP traffic - perpetual ping"/>
  <img src="https://i.imgur.com/NjuUANI.png" height="75%" width="100%" alt="ICMP traffic - ICMP denied"/>
</p>
<p>
  Re-enable ICMP traffic for the Network Security Group in your Ubuntu VM. In the Windows 10 VM, observe the ICMP traffic in WireShark and the command line ping activity. It should be working again. Finally, stop the ping.
</p>
<p>
  <img src="https://i.imgur.com/nZbl2sA.png" height="75%" width="100%" alt="ICMP traffic - ICMP re-enabled"/>
</p>
<br />
<br />
<h3 align="center">
  SSH traffic
</h3>
<br />
<p>
  In Wireshark, filter for SSH traffic only. In your Windows 10 VM, SSH into your Ubuntu virtual machine using its private IP address. Type commands into the linux SSH connection and observe SSH traffic in WireShark.
</p>
</p>
  Exit the SSH connection by typing ‘exit’ and pressing [return]:
</p>
  <img src="https://i.imgur.com/6YEDJKu.png" height="75%" width="100%" alt="SSH traffic"/>
<p>
<br />
<br />
<h3 align="center">
  DHCP Traffic
</h3>
<br />
<p>
  In Wireshark, filter for DHCP traffic only. From your Windows 10 VM, issue your VM a new IP address from the command line with ipconfig /renew.
</p>
Observe the DHCP traffic appearing in WireShark:
</p>
<p>
  <img src="https://i.imgur.com/mKyAHFr.png" height="75%" width="100%" alt="DHCP traffic"/>
</p>
<br />
<br />
<h3 align="center">
  DNS Traffic
</h3>
<br />
<p>
  Back in Wireshark, filter for DNS traffic only.
</p>
<p>
  From your Windows 10 VM within a command line, use nslookup to learn what google.com's IP address is and check out the DNS traffic being shown in WireShark:
</p>
<p>
  <img src="https://i.imgur.com/mYZ8CAK.png" height="75%" width="100%" alt="DNS traffic"/>
</p>
<br />
<br />
<h3 align="center">
  Lastly, RDP traffic
</h3>
<br />
<p>
  In Wireshark, filter for RDP traffic only using tcp.port==3389.
</p>
<p>
  Here, you'll see a constant stream of traffic since RDP is constantly showing you a live stream from one computer to another. 
</p>
<p>
  <img src="https://i.imgur.com/hNlhTVp.png" height="75%" width="100%" alt="RDP traffic"/>
</p>
<p>
  Now that we've finished our observations, we'll clean up our Azure environment to prevent additional charges $$.
</p>
</p>
