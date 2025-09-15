## 1. **ifconfig** (deprecated → replaced by `ip addr`)

Displays or configures network interfaces.

```bash
# Show all network interfaces and IP addresses
ifconfig  

# Bring an interface up
sudo ifconfig eth0 up  

# Assign an IP address to an interface
sudo ifconfig eth0 192.168.1.100 netmask 255.255.255.0
```

👉 Modern replacement:

```bash
ip addr show
ip link set eth0 up
```

---

## 2. **traceroute**

Traces the path packets take to reach a destination.

```bash
# Trace the route to google.com
traceroute google.com  

# Use ICMP instead of UDP
traceroute -I google.com
```

👉 Helps diagnose where delays happen between your system and a remote server.

---

## 3. **dig** (Domain Information Groper)

Performs DNS lookups.

```bash
# Basic A record lookup
dig google.com  

# Query MX (mail) records
dig google.com MX  

# Query specific DNS server
dig @8.8.8.8 google.com
```

👉 More powerful and modern than `nslookup`.

---

## 4. **telnet** (legacy, often replaced by `nc`)

Tests connectivity to a port.

```bash
# Check if port 80 (HTTP) is open
telnet google.com 80  

# Quit telnet session
CTRL + ] then type quit
```

👉 Nowadays, **netcat (`nc`)** is preferred:

```bash
nc -zv google.com 80
```

---

## 5. **nslookup**

Another DNS lookup tool (older than `dig`).

```bash
# Lookup IP of a domain
nslookup google.com  

# Query specific DNS server
nslookup google.com 8.8.8.8
```

👉 `dig` is more feature-rich, but `nslookup` is still common.

---

## 6. **netstat** (deprecated → replaced by `ss`)

Displays network connections, routing tables, interfaces, and stats.

```bash
# Show all active connections
netstat -tulnp  

# Show listening ports
netstat -lntp
```

👉 Modern replacement:

```bash
ss -tulnp
```

---

# ✅ Extra Useful Network Commands

### 🔹 **ping** – Check connectivity

```bash
ping google.com
ping -c 4 8.8.8.8    # Send 4 packets only
```

---

### 🔹 **curl / wget** – Test HTTP requests

```bash
curl -I https://google.com   # Fetch HTTP headers
wget https://example.com/file.zip
```

---

### 🔹 **scp / rsync** – Copy files over SSH

```bash
scp file.txt user@192.168.1.50:/tmp/
rsync -avz file.txt user@192.168.1.50:/tmp/
```

---

### 🔹 **nmap** – Network scanner

```bash
# Scan open ports
nmap 192.168.1.1  

# Scan a range of IPs
nmap 192.168.1.0/24
```

---

### 🔹 **arp** – Show ARP cache (IP ↔ MAC mapping)

```bash
arp -a
```

---

### 🔹 **route / ip route** – View routing table

```bash
route -n
ip route show
```

---

👉 So the **modern equivalents** you should focus on are:

* `ip` (instead of ifconfig)
* `ss` (instead of netstat)
* `dig` (instead of nslookup)
* `nc` (instead of telnet)

---

Perfect 👍 Here’s a **Linux Network Commands Cheat Sheet** you can keep as a quick reference.

---

# 🖥️ Linux Network Commands Cheat Sheet

| Command              | Description                                               | Example                                             |
| -------------------- | --------------------------------------------------------- | --------------------------------------------------- |
| **ifconfig** *(old)* | Show/assign IPs, bring interfaces up/down                 | `ifconfig eth0 192.168.1.100 netmask 255.255.255.0` |
| **ip addr**          | Show IP addresses and interfaces (modern)                 | `ip addr show`                                      |
| **ip link**          | Enable/disable interfaces                                 | `ip link set eth0 up`                               |
| **ping**             | Test connectivity to a host                               | `ping -c 4 google.com`                              |
| **traceroute**       | Trace packet path to destination                          | `traceroute google.com`                             |
| **dig**              | Query DNS records                                         | `dig google.com MX`                                 |
| **nslookup** *(old)* | Query DNS (basic)                                         | `nslookup google.com 8.8.8.8`                       |
| **telnet** *(old)*   | Test connectivity to a port                               | `telnet google.com 80`                              |
| **nc (netcat)**      | Test ports, transfer data (modern replacement for telnet) | `nc -zv google.com 80`                              |
| **netstat** *(old)*  | Show connections, listening ports                         | `netstat -tulnp`                                    |
| **ss**               | Show socket statistics (modern netstat)                   | `ss -tulnp`                                         |
| **curl**             | Fetch URL, test HTTP/HTTPS                                | `curl -I https://google.com`                        |
| **wget**             | Download files from the web                               | `wget https://example.com/file.zip`                 |
| **scp**              | Copy files via SSH                                        | `scp file.txt user@192.168.1.50:/tmp/`              |
| **rsync**            | Sync files over SSH (efficient)                           | `rsync -avz file.txt user@192.168.1.50:/tmp/`       |
| **nmap**             | Network/port scanner                                      | `nmap 192.168.1.1`                                  |
| **arp**              | Show ARP table (IP ↔ MAC)                                 | `arp -a`                                            |
| **route** *(old)*    | Show routing table                                        | `route -n`                                          |
| **ip route**         | Show routing info (modern)                                | `ip route show`                                     |
| **hostname -I**      | Show host’s IP addresses                                  | `hostname -I`                                       |
| **whois**            | Get domain registration details                           | `whois google.com`                                  |

---

👉 You can practice by:

1. Checking your own IPs → `ip addr show`
2. Testing DNS resolution → `dig google.com`
3. Checking open ports on your machine → `ss -tulnp`
4. Testing connectivity to a server → `ping` + `traceroute`

---

