# Day 15: Setup SSL for Nginx

## Question

The system admins team of `xFusionCorp Industries` needs to deploy a new application on `App Server 2` in `Stratos Datacenter`. They have some pre-requites to get ready that server for application deployment. Prepare the server as per requirements shared below:

**Task:**  

1. Install and configure `nginx` on `App Server 2`.

2. On `App Server 2` there is a self signed SSL certificate and key present at location `/tmp/nautilus.crt` and `/tmp/nautilus.key`. Move them to some appropriate location and deploy the same in Nginx.

3. Create an `index.html` file with content `Welcome!` under Nginx document root.

4. For final testing try to access the `App Server 2` link (either hostname or IP) from `jump host` using curl command. For example `curl -Ik https://<app-server-ip>/`.

---

## Solution

1. **Install Nginx**

```bash
sudo yum install -y nginx   # (if CentOS/RHEL)
# OR
sudo apt-get update && sudo apt-get install -y nginx   # (if Ubuntu/Debian)
```

Enable and start the service:

```bash
sudo systemctl enable nginx
sudo systemctl start nginx
```

Check:

```bash
systemctl status nginx
```

---

2. **Move SSL Certificate & Key**

Move the provided cert & key to a standard location:

```bash
sudo mkdir -p /etc/nginx/ssl
sudo mv /tmp/nautilus.crt /etc/nginx/ssl/
sudo mv /tmp/nautilus.key /etc/nginx/ssl/
sudo chmod 600 /etc/nginx/ssl/nautilus.key
```

---

3. **Configure Nginx for SSL**

Edit the default SSL configuration file:

```bash
sudo vi /etc/nginx/conf.d/nautilus.conf
```

Add this configuration:

```nginx
server {
    listen 443 ssl;
    server_name _;

    ssl_certificate /etc/nginx/ssl/nautilus.crt;
    ssl_certificate_key /etc/nginx/ssl/nautilus.key;

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

# Optional: Redirect HTTP → HTTPS
server {
    listen 80;
    server_name _;
    return 301 https://$host$request_uri;
}
```

---

4. **Create `index.html`**

```bash
echo "Welcome!" | sudo tee /usr/share/nginx/html/index.html
```

---

5. **Test Configuration**

Check for syntax errors:

```bash
sudo nginx -t
```

Reload Nginx:

```bash
sudo systemctl reload nginx
```

---

6. **Verify from Jump Host**

From the Jump Host (not App Server 2 itself), test with:

```bash
curl -Ik https://stapp02/
```

Expected output (HTTP 200 OK with SSL response):

```makefile
HTTP/1.1 200 OK
Server: nginx/...
Date: ...
Content-Type: text/html
Content-Length: 8
...
```

---

