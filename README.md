<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />




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

- Virtual Machine
- Wireshark
- Administrator Poershell Terminal

<h2>Actions and Observations</h2>

<p>
<img width="1421" height="692" alt="azure services snapshot" src="https://github.com/user-attachments/assets/932df126-86a2-437c-8e05-1c11fc579180" />


</p>
<p>
Azure resources
</p>
<br />

<p>
<img width="1747" height="917" alt="image" src="https://github.com/user-attachments/assets/ce288c6d-f061-485a-8bc0-4649fd6c1773" />

</p>
<p>
Wireshark
  (Observe ICMP Traffic)

  If using Mac, install Microsoft Remote Desktop

Use Remote Desktop to connect to your Windows 10 Virtual Machine

Within your Windows 10 Virtual Machine, Install Wireshark

Open Wireshark and start packet capture

Within Wireshark, filter for ICMP traffic only

Retrieve the private IP address of the Ubuntu VM (linux-vm) and attempt to ping it from within the Windows 10 VM

Observe ping requests and replies within WireShark

From The Windows 10 VM, open command line or PowerShell and attempt to ping a public website (such as www.google.com) and observe the traffic in WireShark
</p>
<br />

<p>
<img width="1748" height="1011" alt="image" src="https://github.com/user-attachments/assets/38b1d640-a321-4c65-9744-4b4a134ae40c" />

</p>
<p>
(Configuring a Firewall [Network Security Group])

  Initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM

Open the Network Security Group your Ubuntu VM is using and disable incoming (inbound) ICMP traffic

Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity

Re-enable ICMP traffic for the Network Security Group your Ubuntu VM is

Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity (should start working)

Stop the ping activity

-- Next

(Observe SSH Traffic)

Log back into the windows-vm

Back in Wireshark, start a packet capture up

Filter for SSH traffic only

From your Windows 10 VM, “SSH into” your Ubuntu Virtual Machine (via its private IP address)

Open PowerShell, and type: ssh labuser@<private IP address>

Type commands (username, pwd, etc) into the linux SSH connection and observe SSH traffic spam in WireShark

Exit the SSH connection by typing ‘exit’ and pressing [Enter]


</p>
<img width="1657" height="673" alt="image" src="https://github.com/user-attachments/assets/c2fc9834-21fd-4141-9f55-246be83a661e" />
Network configuration


<img width="1437" height="803" alt="image" src="https://github.com/user-attachments/assets/852f1718-5492-4d05-87d0-b2ff07478a9d" />
icmpv4

(Observe DHCP Traffic)

Back in Wireshark, filter for DHCP traffic only

From your Windows 10 VM, attempt to issue your VM a new IP address from the command line

Open PowerShell as admin and run: ipconfig /renew

Observe the DHCP traffic appearing in WireShark

--Next

Observe DNS Traffic)

Back in Wireshark, filter for DNS traffic only

From your Windows 10 VM within a command line, use nslookup to see what google.com and disney.com’s IP addresses are

Observe the DNS traffic being show in WireShark

A-Record Exercise

Connect/log into DC-1 as your domain admin account (mydomain.com\jane_admin)

Connect/log into Client-1 as an admin (mydomain\jane_admin)

From Client-1 try to ping “mainframe” notice that it fails

Nslookup “mainframe” notice that it fails (no DNS record)

Create a DNS A-record on DC-1 for “mainframe” and have it point to DC-1’s Private IP address

Go back to Client-1 and try to ping it. Observe that it works

Local DNS Cache Exercise

Go back to DC-1 and change mainframe’s record address to 8.8.8.8

Go back to Client-1 and ping “mainframe” again. Observe that it still pings the old address

Observe the local dns cache (ipconfig /displaydns)

Flush the DNS cache (ipconfig /flushdns).

Observe that the cache is empty (ipconfig /displaydns)

Attempt to ping “mainframe” again. Observe the address of the new record is showing up

CNAME Record Exercise

Go back to DC-1 and create a CNAME record that points the host “search” to “www.google.com”

Go back to Client-1 and attempt to ping “search”, observe the results of the CNAME record

On Client-1, nslookup “search”, observe the results of the CNAME record



<img width="1652" height="900" alt="image" src="https://github.com/user-attachments/assets/92dee813-e4b0-4733-a571-71e9547a6273" />
ubuntu (linux)

<img width="1492" height="826" alt="image" src="https://github.com/user-attachments/assets/1da535bd-7f7d-4b7b-9de9-063da1e9ecb9" />
ubuntu linux to windows
TCP

<img width="1578" height="947" alt="image" src="https://github.com/user-attachments/assets/9aab450b-c231-47ec-8693-bdbbe85a6e5b" />
UDP


<img width="1505" height="980" alt="image" src="https://github.com/user-attachments/assets/98661362-2f89-4558-8f16-f0faa0634790" />

RDP

(Observe RDP Traffic)

Back in Wireshark, filter for RDP traffic only (tcp.port == 3389)

Observe the immediate non-stop spam of traffic? Why do you think it’s non-stop spamming vs only showing traffic when you do an activity?

Answer: because the RDP (protocol) is constantly showing you a live stream from one computer to another, therefor traffic is always being transmitted



------------------------------------------------------------------------------------------------------------------------------------------------------

VPN (Virtual Private Network)

(Create Virtual Machine in Azure)

Browse to https://whatismyipaddress.com/ FROM WITHIN YOUR OWN MACHINE and take note of this in a text file

Create a Resource Group

Create a Windows 10 Virtual Machine in another geographic location (try a different country)

Log into the VM with Remote Desktop

Browse to https://whatismyipaddress.com/ and take note of this in a text file

(Sign up for ProtonVPN and test the VPN connection)

On your actual computer, sign up for the free version of Proton VPN https://account.protonvpn.com/signup?plan=free&language=en  

Back within your VM, download the Proton VPN client

Login to the VPN (https://account.protonvpn.com/login) and choose a VPN server in yet another country (such as Japan)

Browse to https://whatismyipaddress.com/  and take note of this in a text file

Try browsing to Google, Disney, and/or Amazon and see if there is anything different about the sites in relation to the location of your VPN server. For example, the language or URL may be different

(Clean up Azure resources)
Delete the resource group you created in Step 2
Ensure the resources/Resource Group has been deleted.


<br />
