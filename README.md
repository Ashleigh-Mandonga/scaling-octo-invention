# 🚀 Nginx SSL Deployment — Stratos Datacenter Lab

## 📌 Objective
Deploy Nginx with HTTPS (self-signed SSL) on App Server 3 
of the Stratos Datacenter environment.

---

## 🖥️ Environment
| Role       | Hostname | User   |
|------------|----------|--------|
| Jump Host  | thor     | thor   |
| App Server | stapp03  | banner |

---

## 🚀 Steps Performed

### 1️⃣ Install Nginx
```bash
yum install nginx -y
nginx -v
```

### 2️⃣ Find SSL Certificates
```bash
find / -name "nautilus.crt" 2>/dev/null
find / -name "nautilus.key" 2>/dev/null
```

### 3️⃣ Secure the Key
```bash
chmod 600 /etc/ssl/private/nautilus.key
```

### 4️⃣ Nginx SSL Configuration
```nginx
server {
    listen       443 ssl;
    listen       [::]:443 ssl;
    server_name  stapp03;
    root         /usr/share/nginx/html;

    ssl_certificate     /etc/pki/tls/certs/nautilus.crt;
    ssl_certificate_key /etc/ssl/private/nautilus.key;
    ssl_session_cache shared:SSL:1m;
    ssl_session_timeout  10m;
    ssl_ciphers PROFILE=SYSTEM;
    ssl_prefer_server_ciphers on;
}
```

### 5️⃣ Create Welcome Page
```bash
echo "Welcome!" > /usr/share/nginx/html/index.html
cat /usr/share/nginx/html/index.html
```

### 6️⃣ Start & Enable Nginx
```bash
nginx -t
systemctl start nginx
systemctl enable nginx
systemctl status nginx
```

### 7️⃣ Validate HTTPS from Jump Host
```bash
curl -Ik https://stapp03/
```

---

## ✅ Result
```
HTTP/1.1 200 OK
Server: nginx/1.x.x
```
HTTPS endpoint live and verified!

---

## 🛠️ Tech Stack
`Nginx` `SSL/TLS` `Linux CentOS` `Bash` `SSH` `curl`

---

## 💡 Key Learnings
- Self-signed SSL cert deployment in Nginx
- Linux file permission hardening (`chmod 600`)
- Nginx config validation (`nginx -t`)
- Troubleshooting missing directories & config errors

---

## 🏷️ Tags
`nginx` `ssl` `devops` `linux` `https` `kodeKloud` `stratos`
