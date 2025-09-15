Port numbers are numerical identifiers (0-65535) that network protocols like TCP and UDP use to direct data 
to the correct application or service on a device. While an IP address identifies a specific device, a 
port number is like an "extension" that specifies the application on that device, ensuring data reaches 
the intended destination. You use them implicitly when using applications (like web browsers using port 
80/443 for HTTP/HTTPS) or when configuring firewalls and network services to open specific ports for 
communication.   

# What are port numbers? 
- Like an extension number: An IP address gets data to the correct computer, but a port number directs that 
  data to the correct program or service on that computer.
- Standardized: Each port number is reserved for specific functions or services.
- Range: Port numbers are 16-bit numbers, meaning they range from 0 to 65535. 

# How do they work? 

1. Client initiates connection: When your computer (the client) wants to connect to a web server, it sends a request to the server's IP address. 
2. IP address directs to server: The request travels across the internet to the server's specific IP address. 
3. Port number directs to service: The request also includes a port number (e.g., 80 for HTTP) that tells the server's operating system which application to give the data to. 
4. Service responds: The web server's HTTP service, listening on port 80, receives the request and sends 
   back the webpage data to your computer's specific, temporarily assigned ephemeral port.   

# Common port numbers: 
Port 80: Used for Hypertext Transfer Protocol (HTTP) for unencrypted web traffic.
Port 443: Used for secure, encrypted HTTPS traffic.
Port 25: Used for sending emails via the Simple Mail Transfer Protocol (SMTP). 

# How do you use them? 
Implicitly: Most users interact with ports without direct input, as applications and operating systems handle them automatically.
Manually:
Port Forwarding: You might need to configure your router to forward incoming traffic on a specific port to a particular device on your local network, according to an application's requirements.
Firewall Configuration: To allow specific network services to run or connect, you may need to open their required port numbers in your firewall settings
Troubleshooting: If an application isn't connecting, you might use a command like netstat to see which ports are open and the services using them, then check if the necessary port is blocked.

---
A port number is a 16-bit number that serves as a communication endpoint for processes on a networked device. It identifies specific applications or services running on a host that are sending or receiving data over a network. When data arrives at a server, the port number tells the device which specific service or application should handle that data, working in conjunction with an IP address to direct network traffic. [1]  
Examples of Linux Services and their Associated Port Numbers: 
Here are some common Linux services and their default port numbers, typically using TCP or UDP protocols: 

• SSH (Secure Shell): Port 22 (TCP) - Used for secure remote access to the server, allowing for command-line access, file transfers, and secure tunneling. 
• FTP (File Transfer Protocol): Ports 20 (TCP for data) and 21 (TCP for control) - Used for transferring files between a client and a server. 
• Telnet: Port 23 (TCP) - An older protocol for remote access, less secure than SSH as it transmits data in plaintext. 
• SMTP (Simple Mail Transfer Protocol): Port 25 (TCP) - Used for sending email messages between mail servers. 
• DNS (Domain Name System): Port 53 (TCP/UDP) - Translates human-readable domain names into machine-readable IP addresses. [2]  
• HTTP (Hypertext Transfer Protocol): Port 80 (TCP) - The fundamental protocol for the World Wide Web, used for serving web pages. 
• POP3 (Post Office Protocol version 3): Port 110 (TCP) - Used by email clients to retrieve email from a mail server. 
• IMAP (Internet Message Access Protocol): Port 143 (TCP) - Another protocol for retrieving email, offering more advanced features than POP3. 
• SNMP (Simple Network Management Protocol): Ports 161 (UDP for agent) and 162 (UDP for manager) - Used for monitoring and managing network devices. 
• HTTPS (Hypertext Transfer Protocol Secure): Port 443 (TCP) - A secure version of HTTP, using SSL/TLS encryption for secure communication over the web. 
