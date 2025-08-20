# Day 16: Install and Configure Nginx as an LBR

## Question

Day by day traffic is increasing on one of the websites managed by the `Nautilus` production support team. Therefore, the team has observed a degradation in website performance. Following discussions about this issue, the team has decided to deploy this application on a high availability stack i.e on `Nautilus` infra in `Stratos DC`. They started the migration last month and it is almost done, as only the LBR server configuration is pending. Configure LBR server as per the information given below:

**Task:**  

a. Install `nginx` on `LBR` (load balancer) server.

b. Configure load-balancing with the an `http` context making use of all `App Servers`. Ensure that you update only the main Nginx configuration file located at `/etc/nginx/nginx.conf`.

c. Make sure you do not update the apache port that is already defined in the apache configuration on all app servers, also make sure apache service is up and running on all app servers.

d. Once done, you can access the website using `StaticApp` button on the top bar.

---

## Solution

1. **Switch to the Load Balancer (LBR) Server**

```bash
ssh loki@stlb01
```

---

2. **Install Nginx**

```bash
sudo yum install nginx -y   # (if CentOS/RHEL)
# OR
sudo apt-get install nginx -y   # (for Ubuntu/Debian based)
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

3. **Edit the Main Configuration File**

Open `/etc/nginx/nginx.conf`:

```bash
sudo vi /etc/nginx/nginx.conf
```

Inside the `http` **block**, add an **upstream** section pointing to the app servers (usually `stapp01`, `stapp02`, `stapp03` in KodeKloud labs).
Each app server runs Apache on port 3000.

Example:

```nginx
http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    upstream app_servers {
        server stapp01:3000;
        server stapp02:3000;
        server stapp03:3000;
    }

    server {
        listen       80;
        server_name  localhost;

        location / {
            proxy_pass http://app_servers;
        }
    }

}
```

⚠️ Important: You must put the `upstream` and `server` blocks inside the main `http {}` block of `nginx.conf`.

---

4. **Test Configuration**

Check for syntax errors:

```bash
sudo nginx -t
```

If syntax is ok → reload:

```bash
sudo systemctl restart nginx
```

---

5. **Verify Apache on App Servers**

Log in to each app server (`stapp01`, `stapp02`, `stapp03`) and check Apache:

```bash
systemctl status httpd   # (CentOS/RHEL)
# or
systemctl status apache2 # (Ubuntu/Debian)
```

If not running:

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

5. **Validate**

Go back to the KodeKloud UI and click the **StaticApp button** (top bar).
You should see the application being served, load balanced across all app servers.

---

