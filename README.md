# Proxy-linux

# Configure a Proxy on a Linux Server

This document provides instructions for configuring a proxy server on a Linux system. It covers the setup of both a **forward proxy** using Squid and a **reverse proxy** using Nginx.

## 1. Setting Up a Forward Proxy (Squid)

### Step 1: Install Squid
- On **Debian/Ubuntu-based systems**:
  ```bash
  sudo apt update
  sudo apt install squid
  ```
- On **Red Hat/CentOS-based systems**:
  ```bash
  sudo yum install squid
  ```

### Step 2: Configure Squid
- Edit the Squid configuration file located at `/etc/squid/squid.conf`:
  ```bash
  sudo nano /etc/squid/squid.conf
  ```
- Add the following settings to allow your local network and specify the listening port:
  ```
  # Define ACL for allowed network range (Local network)
  acl localnet src 192.168.1.0/24

  # HTTP access rules
  http_access allow localnet
  http_access deny all

  # Squid listening port (default is 3128)
  http_port 3128
  ```

### Step 3: Restart the Squid Service
- Apply the changes by restarting Squid:
  ```bash
  sudo systemctl restart squid
  ```

---

## 2. Setting Up a Reverse Proxy (Nginx)

### Step 1: Install Nginx
- On **Debian/Ubuntu-based systems**:
  ```bash
  sudo apt update
  sudo apt install nginx
  ```
- On **Red Hat/CentOS-based systems**:
  ```bash
  sudo yum install nginx
  ```

### Step 2: Configure Nginx for Reverse Proxy
- Edit the Nginx configuration file located at `/etc/nginx/sites-available/default`:
  ```bash
  sudo nano /etc/nginx/sites-available/default
  ```
- Replace the content with the following configuration:
  ```nginx
  server {
      listen 80;
      server_name yourdomain.com;

      location / {
          proxy_pass http://127.0.0.1:8080;
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;
      }
  }
  ```

### Step 3: Test and Restart Nginx
- Test the Nginx configuration:
  ```bash
  sudo nginx -t
  ```
- If successful, restart the Nginx service:
  ```bash
  sudo systemctl restart nginx
  ```

---

### Notes:
- Replace `192.168.1.0/24` with your network's IP range as needed.
- Replace `yourdomain.com` with your domain name in the Nginx configuration.

---

This formatted document is ready for a GitHub commit. It uses clean markdown with proper code blocks for improved readability.

### config proxy for apt
```bash
sudo nano /etc/apt/apt.conf
```
Add this line to your /etc/apt/apt.conf file (substitute your details for yourproxyaddress and proxyport).
```bash
Acquire::http::Proxy "http://yourproxyaddress:proxyport";
```
Save the apt.conf file.
