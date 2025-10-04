1) Enable UFW (Uncomplicated Firewall)

We check the status of UFW using the command sudo ufw status:
![[Pasted image 20251004154453.png]](/Cybersecurity_Tools/Images/Firewall/Pasted%20image%2020251004154453.png)
Before enabling UFW we make sure the connection to ssh is allowed using the command: sudo ufw allow 22/tcp:

![[Pasted image 20251004162953.png]](/Cybersecurity_Tools/Images/Firewall/Pasted%20image%2020251004162953.png)

The reason why it is important to allow traffic through port 22 (SSH access) before enabling UFW is because by activating UFW without any rule, UFW drops all incoming connections, essentially locking us out of our own server without any way back in since we can no longer connect via SSH.

We check the other ports that are open that we may need to allow specific connections such as 80, 443, etc.. We use the following command to do this: sudo ss -tuln

![[Pasted image 20251004165256.png]](/Cybersecurity_Tools/Images/Firewall/Pasted%20image%2020251004165256.png)

tuln is for checking TCP, UDP, and Listening. n displays addresses in numeric form. Socket Statistics (ss) is a command line utility for checking network statuses. From the screenshot we can see a number of different services with different ips and either a UDP or TCP protocol. We can also see the state of these services, with some of them being on the listening state. If there was a port that we were unsure of we could check a specific service of that port by using: sudo lsdof -i :portNumber, where portNumber is the number of the port you want to check. 

We enable UFW using the command: sudo ufw status 

![[Pasted image 20251004170848.png]](/Cybersecurity_Tools/Images/Firewall/Pasted%20image%2020251004170848.png)

We can check the status with the command: sudo ufw status

![[Pasted image 20251004171009.png]](/Cybersecurity_Tools/Images/Firewall/Pasted%20image%2020251004171009.png)

Now we can see from the status screen that our UFW is active and allowing traffic through port 22. 

If the server was a web server, the ports that should be allowed through UFW would be either port 80 for unencrypted web servers or port 443 for HTTPS which is what encrypted web servers use. We can allow these ports access using the previously mentioned command: sudo ufw allow (portnumber)/tcp

![[Pasted image 20251004174005.png]](/Cybersecurity_Tools/Images/Firewall/Pasted%20image%2020251004174005.png)

We can check the status of these newly added ports using the check status command from before, or the following verbose command: sudo ufw status verbose

![[Pasted image 20251004174729.png]](/Cybersecurity_Tools/Images/Firewall/Pasted%20image%2020251004174729.png)

What makes this status screen different from the last one is that it gives more information alongside the active status, specifically logging, default, and new profiles.  Logging shows the logging level, which is low in this case. Default stands for default policies which displays the default behavior for incoming, outgoing, and routed connections. New profiles shows how new application profiles are handled, which in this case, they are skipped. 

Let's say there is a disgruntled ex employee who is known to wreck havoc. We want to block his IP address, 10.0.0.0, from our server using UFW. We can do this via the command: sudo ufw deny from 10.0.0.0

![[Pasted image 20251004181453.png]](/Cybersecurity_Tools/Images/Firewall/Pasted%20image%2020251004181453.png)

Let's also say that we want to allow 192.168.1.50 access to port 587, which is a port used for secure email submission. We can do this via the command: sudo ufw allow from 192.168.1.50 to any port 587

![[Pasted image 20251004182007.png]](/Cybersecurity_Tools/Images/Firewall/Pasted%20image%2020251004182007.png)

We can check the status of these newly added rules once again by using the sudo ufw status command from before.

![[Pasted image 20251004182258.png]](/Cybersecurity_Tools/Images/Firewall/Pasted%20image%2020251004182258.png)

We can see the 10.0.0.0 IP being blocked and 192.168.1.50 being allowed through port 587.

2) Enable UFW Logging

We make sure that UFW logging is enabled. To do this we run: sudo ufw logging on

![[Pasted image 20251004182953.png]](/Cybersecurity_Tools/Images/Firewall/Pasted%20image%2020251004182953.png)

We can change the level of detail of the logs by specifying the following levels: low, medium, high, full.

For our purposes, we will set the logging info to high: sudo ufw logging high

![[Pasted image 20251004184002.png]](/Cybersecurity_Tools/Images/Firewall/Pasted%20image%2020251004184002.png)

UFW logs are stored in /var/log/ufw.log. We can check our logs in the VM using the following command: sudo tail -f /var/log/ufw.log. Note that the -f options allows us to monitor the logs in real time. 

![[Pasted image 20251004184538.png]](/Cybersecurity_Tools/Images/Firewall/Pasted%20image%2020251004184538.png)

Here we can see a large amount of information. This info includes MAC addresses (MAC), source IP addresses (SRC), destination IP addresses (DST), source ports (SPT), destination ports (DPT), protocol used (PROTO=), and an indication if a packet was blocked by UFW (UFW BLOCK).

This information is useful to us because allows us to monitor suspicious activity on the server and allow us to act in real time to any potential threats. Info such as the MAC address, IP addresses and ports of interest allow us to easily locate and respond to an attacker's connection if necessary.

To filter specific entries, we use the grep command. For denied traffic, we use: sudo grep 'DENY' /var/log/ufw.log. For allowed traffic, we use: sudo grep 'ALLOW' /var/log/ufw.log

![[Pasted image 20251004191309.png]](/Cybersecurity_Tools/Images/Firewall/Pasted%20image%2020251004191309.png)

As we can see, there is a lot of information for allowed traffic, but none for denied. No output for denied indicates that the UFW hasn't denied any connections, hence nothing to report. 