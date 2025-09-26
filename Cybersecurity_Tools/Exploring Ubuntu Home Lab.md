1) Identifying Network Interfaces and IP Addresses
We use the command ip a or ipconfig:
![[Pasted image 20250925203524.png]](/Cybersecurity_Tools/Images/Pasted%20image%2020250925203524.png)
By using this command, we can see all network interfaces and their associated IP addresses. We can also view their status such as what is up or down. This is important information to have, and will allow us to better help understand the server's network configuration. From the screenshot we can see a number of IP addresses, each belonging to a different network interface such as inet, netmask, and broadcast. We can also see other information like MAC addresses 

2) Check Open Ports:
We use the command: sudo netstat -tuln or ss -tuln
![[Pasted image 20250925210619.png]](/Cybersecurity_Tools/Images/Pasted%20image%2020250925210619.png)
This command lists all open ports on the server along with the services listening on them. The -tuln restricts the output to show only TCP(t) and UDP(u) ports in listening (l) state without resolving names (n). We can see from the screenshot that there a variety of tcp and udp ports as well as some ports in the listening state.

3) Analyze Network Connections
We use the command: sudo lsof -i -P -n
![[Pasted image 20250925223446.png]](/Cybersecurity_Tools/Images/Pasted%20image%2020250925223446.png)
This command lists all open network connections. lsof means "list open files". -i lists all network files along with their associated processes. -P and -n prevent the resolution of port numbers and IP addresses. From the screenshot, we can see a number of UDP and TCP connections, as well as the type of network connections (IPv4 or IPv6). We can also see that some of the connections are in the listening state. 

4) Perform Network Scanning with Nmap
We use the command: sudo map -SS -O localhost
![[Pasted image 20250925230052.png]](/Cybersecurity_Tools/Images/Pasted%20image%2020250925230052.png)
This command scans the server to identify open ports, running services, and the operating system. Nmap (Network Mapper) is a network scanning tool used to find hosts and services on a network. -sS performs a stealth TCP SYN scan, and -O attempts to determine the OS of the target. From the screenshot, we can the results of an Nmap scan. We can see that the host is up, the port is 631/tcp, and the OS is Linux 2.9.X. Something of note is that the network distance is 0 hops, meaning the number of devices (usually routers) that the data travels through was zero. This makes sense since we're referring to the localhost, so no travelling is needed. 

5) Check for Open Ports on the Server's Network
We use the command: sudo -sP 192.168.1.0/24
![[Pasted image 20250925232715.png]](/Cybersecurity_Tools/Images/Pasted%20image%2020250925232715.png)
This command identifies all live hosts on the network. This is useful for finding any unauthorized devices and keeping track of authorized devices. -sP in Nmap is a Ping scan, which hosts on a network are up without performing a port scan. You can see from the screenshot that a bunch of IPs are being scanned. The list of IPs ranges from 192.168.1.1 to 192.168.1.255.

6) Check for Services and Versions
We use the command: sudo nmap -sV localhost
![[Pasted image 20250926004414.png]](/Cybersecurity_Tools/Images/Pasted%20image%2020250926004414.png)
This command scans for open ports and attempts to determine the service and version running on each port. This is useful for finding out of date software that needs to be upgraded. -sV in Nmap enables version detection. Based on the screenshot, we can see that this port is running on CUPS 2.4.

7) Identify Potential Vulnerabilities
We use the command: sudo nmap --script vuln localhost
![[Pasted image 20250926010307.png]](/Cybersecurity_Tools/Images/Pasted%20image%2020250926010307.png)
This command uses Nmap's vulnerability scanning scripts to identify known vulnerabilities. --script vuln runs a script that checks for various vulnerabilities. In the screenshot we can see a large list of possible vulnerabilities in the server. This list points out various vulnerabilities such as possible admin folders, possible backups, etc.

8) Inspect Network Traffic 
We use the command: sudo tcdump -i eth0
![[Pasted image 20250926011702.png]](/Cybersecurity_Tools/Images/Pasted%20image%2020250926011702.png)
This command monitors network traffic on a specific interface, in this case, eth0. This is useful for observing real-time traffic to detect suspicious traffic. The screenshot doesn't provide much, but if you were monitoring, you could stop the monitoring process by hitting ctrl + c. 

9) Monitor Network Connections in Real-Time
We use the command: sudo watch -n 1 netstat -tulnp
![[Pasted image 20250926012616.png]](/Cybersecurity_Tools/Images/Pasted%20image%2020250926012616.png)
This command continuously monitors network connections, updating every second. This is useful for real-time observation of network activities, such as when a new connection or service is starting. "watch" runs a specified command at regular intervals, this command in this case is netstat, which is used to keep us updated about the network connections in real time. The real time observations can't be captured by the screenshot, but the screenshot itself provides insight on the type of information we're keeping an eye on, such as the IP address of the connections, the state, and the type of protocol. 

10) Check Firewall Rules
We use the command: sudo ufw status verbose 
![[Pasted image 20250926014654.png]](/Cybersecurity_Tools/Images/Pasted%20image%2020250926014654.png)
This command displays the current firewall rules configured on the server. This is useful for making sure only the necessary ports are open. ufw stands for Uncomplicated Firewall, which is a front-end for managing iptables. Verbose provides a detailed view of the current firewall configuration. As seen in the screenshot, a firewall isn't setup yet. 

