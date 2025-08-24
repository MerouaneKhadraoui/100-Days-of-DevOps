# Day 20: Configure Nginx + PHP-FPM Using Unix Socket

## Question

The `Nautilus` application development team is planning to launch a new PHP-based application, which they want to deploy on `Nautilus` infra in `Stratos DC`. The development team had a meeting with the production support team and they have shared some requirements regarding the infrastructure. Below are the requirements they shared:

**Task:**  

a. Install `nginx` on `app server 3`, configure it to use port `8092` and its document root should be `/var/www/html`.

b. Install `php-fpm` version `8.3` on `app server 3`, it must use the unix socket `/var/run/php-fpm/default.sock` (create the parent directories if don't exist).

c. Configure php-fpm and nginx to work together.

d. Once configured correctly, you can test the website using `curl http://stapp03:8092/index.php` command from jump host.

---

## Solution

1. **Install Nginx**

```bash
sudo yum install -y nginx
sudo systemctl enable nginx
```

2. **Install PHP-FPM 8.3**

```bash
sudo dnf install epel-release -y
sudo dnf install https://rpms.remirepo.net/enterprise/remi-release-9.rpm -y
sudo dnf module list php
sudo dnf module enable php:remi-8.3 -y
sudo dnf install php-fpm php php-cli php-common php-mysqlnd php-gd php-xml php-mbstring php-pdo php-opcache
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
```

- Check version php-fpm

```bash
php-fpm -v
#PHP 8.3.29 (fpm-fcgi) 
```

3. **Configure PHP-FPM to use Unix Socket**

Edit pool config file

```bash
sudo vi /etc/php-fpm.d/www.conf
```

Change:

```nginx
user = nginx
group = nginx
listen = /var/run/php-fpm/default.sock
listen.owner = nginx
listen.group = nginx
listen.mode = 0660
```

Make sure socket directory exists:

```bash
sudo mkdir -p /var/run/php-fpm
sudo chown -R nginx:nginx /var/run/php-fpm
```

Restart PHP-FPM:

```bash
sudo systemctl enable --now php-fpm
sudo systemctl restart php-fpm
```

4. **Configure Nginx for PHP**

```bash
sudo vi /etc/php-fpm.d/www.conf
```

```nginx

server {
    listen       8092 ;
    listen       [::]:8092;
    server_name  stapp03;
    root /var/www/html;
    index index.php index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~\.php$ {
        include fastcgi_params;
        fastcgi_pass unix:/var/run/php-fpm/default.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }

    error_page 404 /404.html;
    error_page 500 502 503 504 /50x.html;
}

```
Test & reload Nginx:

```bash
sudo nginx -t
sudo systemctl enable nginx
sudo systemctl restart nginx
```

5. **Test From Jump Host**

On jump host, run:

```bash
curl http://stapp03:8092/index.php
```
You should see PHP output confirming everything works 🎉