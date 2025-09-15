## 🔹 1. Basics of Apache HTTP Server

* **What is Apache HTTP Server?**
  A widely used open-source web server that serves web pages over HTTP/HTTPS.

* **Default port numbers:**

  * HTTP → 80
  * HTTPS → 443

* **Main configuration file paths:**

  * `/etc/httpd/conf/httpd.conf` (RHEL/CentOS)
  * `/etc/apache2/apache2.conf` (Ubuntu/Debian)

* **Command to control Apache:**

  ```bash
  systemctl start httpd
  systemctl stop httpd
  systemctl restart httpd
  systemctl status httpd
  apachectl -t   # test config
  ```

---

## 🔹 2. Virtual Hosts

* **Definition:** Allows hosting multiple websites on one Apache server.
* **Types:**

  * **Name-based:** Uses host header (most common).
  * **IP-based:** Different IPs.
  * **Port-based:** Different ports.

**Example (Name-based vhost):**

```apache
<VirtualHost *:80>
    ServerName example.com
    DocumentRoot /var/www/example
    ErrorLog /var/log/httpd/example_error.log
    CustomLog /var/log/httpd/example_access.log combined
</VirtualHost>
```

---

## 🔹 3. SSL/TLS (HTTPS)

* **Enable SSL module:**

  ```bash
  a2enmod ssl   # Debian/Ubuntu
  ```
* **Typical SSL vhost:**

```apache
<VirtualHost *:443>
    ServerName secure.example.com
    DocumentRoot /var/www/secure
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/example.crt
    SSLCertificateKeyFile /etc/ssl/private/example.key
</VirtualHost>
```

* **Let’s Encrypt setup:** via `certbot`.

---

## 🔹 4. Proxy & Load Balancing

* **Forward Proxy:** Acts on behalf of client.
* **Reverse Proxy:** Acts on behalf of server (commonly used).

**Example: Reverse Proxy with mod\_proxy:**

```apache
<VirtualHost *:80>
    ServerName app.example.com
    ProxyPass / http://127.0.0.1:8080/
    ProxyPassReverse / http://127.0.0.1:8080/
</VirtualHost>
```

**Load Balancer example:**

```apache
<Proxy balancer://mycluster>
    BalancerMember http://192.168.1.101:8080
    BalancerMember http://192.168.1.102:8080
</Proxy>

<VirtualHost *:80>
    ServerName lb.example.com
    ProxyPass / balancer://mycluster/
    ProxyPassReverse / balancer://mycluster/
</VirtualHost>
```

---

## 🔹 5. Authentication & Security

* **Basic Auth Example:**

```apache
<VirtualHost *:80>
    DocumentRoot /var/www/private
    <Directory "/var/www/private">
        AuthType Basic
        AuthName "Restricted Area"
        AuthUserFile /etc/httpd/.htpasswd
        Require valid-user
    </Directory>
</VirtualHost>
```

* Create user:

  ```bash
  htpasswd -c /etc/httpd/.htpasswd user1
  ```

* **Security Best Practices:**

  * Hide Apache version: `ServerTokens Prod`, `ServerSignature Off`
  * Disable directory listing: `Options -Indexes`
  * Use firewall + SELinux/AppArmor rules
  * Enable HTTPS with strong ciphers

---

## 🔹 6. Performance & Optimization

* **MPMs (Multi-Processing Modules):**

  * `prefork`: process-based, good for PHP apps.
  * `worker`: threaded, better performance.
  * `event`: modern, scalable.

* **Caching (mod\_cache, mod\_expires):**

```apache
<IfModule mod_expires.c>
    ExpiresActive On
    ExpiresByType text/html "access plus 1 hour"
    ExpiresByType image/jpg "access plus 1 month"
</IfModule>
```

---

## 🔹 7. Logs & Troubleshooting

* **Log files:**

  * Access log: `/var/log/httpd/access_log`
  * Error log: `/var/log/httpd/error_log`

* **Check syntax before restart:**

  ```bash
  apachectl configtest
  ```

* **Useful debugging commands:**

  ```bash
  curl -I http://localhost
  tail -f /var/log/httpd/error_log
  ```

---

## 🔹 8. Common Interview Questions

1. Explain difference between **Apache** and **Nginx**.
2. How to configure **multiple websites** on a single server?
3. What is the use of **mod\_proxy**?
4. What is the difference between **prefork, worker, and event MPM**?
5. How to **redirect HTTP to HTTPS**?

   ```apache
   RewriteEngine On
   RewriteCond %{HTTPS} off
   RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
   ```
6. How to **enable Gzip compression**?

   ```apache
   AddOutputFilterByType DEFLATE text/html text/plain text/xml text/css text/javascript application/javascript
   ```

---
**Apache HTTP Server configuration** into the three key parts you asked about:

* **Virtual Hosts**
* **Proxy**
* **SSL (HTTPS)**

---

## 🔹 Apache Configuration File Structure

* **Main config file**

  * Red Hat: `/etc/httpd/conf/httpd.conf`
  * Debian: `/etc/apache2/apache2.conf`

* **Virtual host configs**

  * Red Hat: `/etc/httpd/conf.d/*.conf`
  * Debian: `/etc/apache2/sites-available/*.conf` and symlinked to `/etc/apache2/sites-enabled/`

* **Modules configs**

  * Red Hat: `/etc/httpd/conf.modules.d/*.conf`
  * Debian: `/etc/apache2/mods-available/` and `/etc/apache2/mods-enabled/`

---

## 1️⃣ Virtual Hosts (vhost)

Virtual hosts let Apache serve multiple websites (domains) from one server.

### Example: HTTP virtual host

```apache
<VirtualHost *:80>
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/example

    <Directory /var/www/example>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog /var/log/httpd/example_error.log
    CustomLog /var/log/httpd/example_access.log combined
</VirtualHost>
```

🔹 Key points:

* `ServerName` → main domain.
* `ServerAlias` → alternate domains.
* `DocumentRoot` → path to site files.
* Logs help debugging per site.

---

## 2️⃣ Proxy (Reverse Proxy)

Apache can act as a reverse proxy to forward requests to another backend app (e.g., Django, Node.js, Tomcat).

### Example: Reverse proxy

```apache
<VirtualHost *:80>
    ServerName app.example.com

    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:8080/
    ProxyPassReverse / http://127.0.0.1:8080/

    ErrorLog /var/log/httpd/app_error.log
    CustomLog /var/log/httpd/app_access.log combined
</VirtualHost>
```

🔹 Key modules needed:

```bash
# RHEL/CentOS
dnf install mod_proxy mod_ssl

# Ubuntu/Debian
a2enmod proxy proxy_http proxy_balancer ssl
```

---

## 3️⃣ SSL (HTTPS)

To enable HTTPS, you need:

1. SSL/TLS module enabled (`mod_ssl`).
2. A valid SSL certificate (`.crt`) and private key (`.key`).
3. Optional: chain certificate from CA.

### Example: SSL vhost

```apache
<VirtualHost *:443>
    ServerName example.com
    DocumentRoot /var/www/example

    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/example.crt
    SSLCertificateKeyFile /etc/ssl/private/example.key
    SSLCertificateChainFile /etc/ssl/certs/example-ca-bundle.crt

    <Directory /var/www/example>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog /var/log/httpd/example_ssl_error.log
    CustomLog /var/log/httpd/example_ssl_access.log combined
</VirtualHost>
```

🔹 Key notes:

* Port `443` is standard for HTTPS.
* You must **open firewall ports**:

  ```bash
  firewall-cmd --add-service=https --permanent
  firewall-cmd --reload
  ```

---

## 🛠️ Common Workflow

1. **Create vhost config** (`/etc/httpd/conf.d/example.conf` or `/etc/apache2/sites-available/example.conf`).
2. **Enable site** (on Debian: `a2ensite example.conf`).
3. **Check syntax**:

   ```bash
   apachectl configtest
   ```
4. **Restart Apache**:

   ```bash
   systemctl restart httpd   # RHEL/CentOS
   systemctl restart apache2 # Debian/Ubuntu
   ```

---
# **real-world combined Apache vhost configuration** that does all three:

* Serves a static **website** (HTML, CSS, JS)
* Proxies API requests to a **backend app** (running on `127.0.0.1:8000`)
* Secures everything with **SSL**

---

## 🌍 Example: `example.com` with static site + API proxy + SSL

Create file:

* **Red Hat** → `/etc/httpd/conf.d/example.conf`
* **Debian** → `/etc/apache2/sites-available/example.conf`

```apache
# Redirect all HTTP (port 80) to HTTPS (port 443)
<VirtualHost *:80>
    ServerName example.com
    ServerAlias www.example.com
    Redirect permanent / https://example.com/
</VirtualHost>

# Main HTTPS Virtual Host
<VirtualHost *:443>
    ServerName example.com
    ServerAlias www.example.com

    # Enable SSL
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/example.crt
    SSLCertificateKeyFile /etc/ssl/private/example.key
    SSLCertificateChainFile /etc/ssl/certs/example-ca-bundle.crt

    # Serve static site files
    DocumentRoot /var/www/example

    <Directory /var/www/example>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    # Proxy API requests to backend app
    ProxyPreserveHost On
    ProxyPass /api http://127.0.0.1:8000/api
    ProxyPassReverse /api http://127.0.0.1:8000/api

    # Logs
    ErrorLog /var/log/httpd/example_ssl_error.log
    CustomLog /var/log/httpd/example_ssl_access.log combined
</VirtualHost>
```

---

## 🔹 Explanation

* **HTTP to HTTPS redirect**
  First block catches all `http://example.com` requests and redirects to `https://example.com`.

* **SSL setup**
  Certificates (`.crt`, `.key`, `.ca-bundle`) must match your domain.
  If using **Let’s Encrypt**, paths usually are:

  ```
  /etc/letsencrypt/live/example.com/fullchain.pem
  /etc/letsencrypt/live/example.com/privkey.pem
  ```

* **Static site**
  `/var/www/example` → where you put `index.html`, CSS, JS.

* **API proxy**
  `/api` on Apache → forwards to backend app (`http://127.0.0.1:8000/api`).
  This means:

  * `https://example.com/api/users` → hits backend app.
  * `https://example.com/` → serves static frontend.

* **Logs**
  Errors and access logs are separate from default Apache logs for easier debugging.

---

## 🛠️ Enabling the Config

**Red Hat / CentOS / Rocky / Alma:**

```bash
# Save file in /etc/httpd/conf.d/example.conf
apachectl configtest
systemctl restart httpd
```

**Ubuntu / Debian:**

```bash
# Save file in /etc/apache2/sites-available/example.conf
a2ensite example.conf
a2enmod ssl proxy proxy_http
apachectl configtest
systemctl restart apache2
```

---
# **Apache config with multiple proxy VirtualHosts** — for example, one server hosting different apps on different subdomains.

Here’s a **real-world example**:

---

## 🌍 Example: Multiple Proxy VirtualHosts

We’ll assume:

* `app1.example.com` → Django app (running on `127.0.0.1:8000`)
* `app2.example.com` → Node.js app (running on `127.0.0.1:3000`)
* `app3.example.com` → Java app (Tomcat on `127.0.0.1:8080`)

File:

* **RHEL** → `/etc/httpd/conf.d/proxyapps.conf`
* **Debian** → `/etc/apache2/sites-available/proxyapps.conf`

```apache
# Redirect HTTP to HTTPS
<VirtualHost *:80>
    ServerName app1.example.com
    Redirect permanent / https://app1.example.com/
</VirtualHost>

<VirtualHost *:80>
    ServerName app2.example.com
    Redirect permanent / https://app2.example.com/
</VirtualHost>

<VirtualHost *:80>
    ServerName app3.example.com
    Redirect permanent / https://app3.example.com/
</VirtualHost>

# ----------------------
# HTTPS VirtualHosts
# ----------------------

<VirtualHost *:443>
    ServerName app1.example.com

    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/app1.example.com/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/app1.example.com/privkey.pem

    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:8000/
    ProxyPassReverse / http://127.0.0.1:8000/

    ErrorLog /var/log/httpd/app1_error.log
    CustomLog /var/log/httpd/app1_access.log combined
</VirtualHost>

<VirtualHost *:443>
    ServerName app2.example.com

    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/app2.example.com/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/app2.example.com/privkey.pem

    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:3000/
    ProxyPassReverse / http://127.0.0.1:3000/

    ErrorLog /var/log/httpd/app2_error.log
    CustomLog /var/log/httpd/app2_access.log combined
</VirtualHost>

<VirtualHost *:443>
    ServerName app3.example.com

    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/app3.example.com/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/app3.example.com/privkey.pem

    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:8080/
    ProxyPassReverse / http://127.0.0.1:8080/

    ErrorLog /var/log/httpd/app3_error.log
    CustomLog /var/log/httpd/app3_access.log combined
</VirtualHost>
```

---

## 🔹 How it works

* Requests to `https://app1.example.com` → sent to Django backend on port **8000**
* Requests to `https://app2.example.com` → sent to Node.js backend on port **3000**
* Requests to `https://app3.example.com` → sent to Tomcat backend on port **8080**
* Each app has its own SSL certificate & log files.
* All `http://` traffic is redirected to `https://`.

---

## 🔧 Required Modules

Enable required Apache modules:

```bash
# RHEL
dnf install mod_ssl
# already included: mod_proxy, mod_proxy_http

# Debian
a2enmod ssl proxy proxy_http
```

---
Perfect 👍 now I understand — you want **one vhost** (one domain like `example.com`), but inside that vhost Apache should **proxy different paths** to different backend services.

---

## 🌍 Example: One vhost, multiple proxy paths

File:

* **RHEL** → `/etc/httpd/conf.d/multiproxy.conf`
* **Debian** → `/etc/apache2/sites-available/multiproxy.conf`

```apache
<VirtualHost *:80>
    ServerName example.com
    Redirect permanent / https://example.com/
</VirtualHost>

<VirtualHost *:443>
    ServerName example.com

    # Enable SSL
    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/example.com/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/example.com/privkey.pem

    # -------------------------
    # Proxy paths
    # -------------------------

    # Django app (backend 1)
    ProxyPass /app1 http://127.0.0.1:8000/
    ProxyPassReverse /app1 http://127.0.0.1:8000/

    # Node.js app (backend 2)
    ProxyPass /app2 http://127.0.0.1:3000/
    ProxyPassReverse /app2 http://127.0.0.1:3000/

    # Tomcat app (backend 3)
    ProxyPass /app3 http://127.0.0.1:8080/
    ProxyPassReverse /app3 http://127.0.0.1:8080/

    # -------------------------
    # Logs
    # -------------------------
    ErrorLog /var/log/httpd/multiproxy_error.log
    CustomLog /var/log/httpd/multiproxy_access.log combined
</VirtualHost>
```

---

## 🔹 How it works

* `https://example.com/app1` → Proxied to Django (`127.0.0.1:8000`)
* `https://example.com/app2` → Proxied to Node.js (`127.0.0.1:3000`)
* `https://example.com/app3` → Proxied to Tomcat (`127.0.0.1:8080`)

So, **one SSL certificate, one domain, multiple apps behind paths.**

---

## 🔧 Required Modules

Enable required modules:

**RHEL:**

```bash
dnf install mod_ssl
# mod_proxy, mod_proxy_http are included by default
```

**Debian:**

```bash
a2enmod ssl proxy proxy_http
```

---
# **Apache HTTP Server interview prep topics** very well. Let me expand each point with **interview-style answers**, so you’ll have both **conceptual clarity** and **ready-to-speak responses**.

---

# ✅ Apache HTTP Server Interview Prep

---

## 1. Core Concepts and Architecture

**Q: What is Apache HTTP Server?**
Apache is an **open-source web server** that serves static and dynamic content over the **HTTP and HTTPS protocols**. It’s one of the most widely used web servers globally, powering millions of websites. Its role is to **listen for client requests (browsers, API clients), process them, and respond with web content** such as HTML, CSS, images, or dynamic responses via backends like PHP, Python, or Java.

**Main components:**

* **Server Core** → Handles connection management and request processing.
* **Modules** → Extend Apache’s functionality. Examples:

  * `mod_ssl` (SSL/TLS support)
  * `mod_rewrite` (URL rewriting)
  * `mod_proxy` (proxying and load balancing)
* **Configuration Files** → Main: `httpd.conf`, plus `conf.d/`, `sites-available/`, and `sites-enabled/` in Debian-based systems.
* **Request Processing Engine** → Uses **MPMs (Multi-Processing Modules)** such as:

  * `prefork` (process-based, stable for non-thread-safe modules like mod\_php)
  * `worker` (multi-threaded, scalable)
  * `event` (optimized for keep-alive connections)

**Protocols supported:** Primarily **HTTP/1.1, HTTP/2 (with mod\_http2), and HTTPS**.

**Default ports:**

* **80** → HTTP
* **443** → HTTPS

---

## 2. Configuration and Management

**Main configuration file:**

* RHEL/CentOS: `/etc/httpd/conf/httpd.conf`
* Ubuntu/Debian: `/etc/apache2/apache2.conf`

**Service management:**

```bash
systemctl start httpd
systemctl stop httpd
systemctl restart httpd
systemctl status httpd
apachectl configtest   # check config syntax
```

**Check if running:**

```bash
ps aux | grep httpd
ss -tulnp | grep :80
```

**Virtual Hosting:**

* **Name-based vhost (most common):**

```apache
<VirtualHost *:80>
    ServerName site1.com
    DocumentRoot /var/www/site1
</VirtualHost>

<VirtualHost *:80>
    ServerName site2.com
    DocumentRoot /var/www/site2
</VirtualHost>
```

* **IP-based vhost:** Assigns separate IPs.

```apache
<VirtualHost 192.168.1.100:80>
    ServerName app.local
    DocumentRoot /var/www/app
</VirtualHost>
```

**Changing ports:**
In `httpd.conf` → `Listen 8080`

**DirectoryIndex:** Defines default file when a directory is requested:

```apache
DirectoryIndex index.html index.php
```

**Alias and AliasMatch:**

```apache
Alias /images/ /var/www/media/images/
AliasMatch ^/icons/(.*) /var/www/icons/$1
```

**Enable/Disable modules:**

* Debian/Ubuntu: `a2enmod rewrite`, `a2dismod ssl`
* RHEL: Edit `httpd.conf` → `LoadModule mod_rewrite.so`

**Enabling SSL:**

1. Install `mod_ssl`.
2. Generate or install certificate (`.crt`, `.key`).
3. Add SSL vhost.

---

## 3. Functionality and Features

**mod\_rewrite (URL rewriting):**

* Used for SEO-friendly URLs, redirects, enforcing HTTPS.
  Example:

```apache
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

**Directory Indexing:**

* Enable listing: `Options +Indexes`
* Disable listing (best practice): `Options -Indexes`

**Log files:**

* Access log: `/var/log/httpd/access_log`
* Error log: `/var/log/httpd/error_log`
  Use `LogFormat` directive to customize.

**Security best practices:**

* Run Apache as a **non-root user**.
* Disable unused modules.
* Hide server version:

  ```apache
  ServerTokens Prod
  ServerSignature Off
  ```
* Use `Options -Indexes` to prevent file listing.
* Apply WAF (e.g., `mod_security`).

**Proxying with Apache:**
Apache can act as a **reverse proxy**:

```apache
<VirtualHost *:80>
    ServerName api.example.com
    ProxyPass / http://127.0.0.1:8080/
    ProxyPassReverse / http://127.0.0.1:8080/
</VirtualHost>
```

---

## 4. Troubleshooting and Error Handling

**Common HTTP error codes:**

* **403 Forbidden** → Permission denied, bad `Options`/`Allow` config.
* **404 Not Found** → Missing file or wrong `DocumentRoot`.
* **500 Internal Server Error** → Misconfigured `.htaccess`, script errors.

**“Connection reset by peer”:**

* SSL mismatch
* Proxy timeout
* Firewall/SELinux restrictions

**Debugging techniques:**

* Check logs with `tail -f /var/log/httpd/error_log`
* Run `apachectl configtest`
* Use `curl -I http://localhost` to test headers
* Enable `LogLevel debug` temporarily

---

## 5. Advanced Topics

**Apache Tomcat Integration:**

* Use **AJP connector** (`mod_proxy_ajp`) to connect Apache and Tomcat:

```apache
<VirtualHost *:80>
    ProxyPass / ajp://localhost:8009/
</VirtualHost>
```

**Performance optimization:**

* Enable caching (`mod_cache`, `mod_expires`)
* Use `worker` or `event` MPM
* Enable Gzip compression:

  ```apache
  AddOutputFilterByType DEFLATE text/html text/css application/javascript
  ```
* Use KeepAlive connections with tuning

**Security Hardening:**

* Use TLS 1.2/1.3 only → disable SSLv2/v3, TLS 1.0
* Enable `mod_security` + `mod_evasive`
* Chroot/jail Apache if possible
* Regular patching

---
