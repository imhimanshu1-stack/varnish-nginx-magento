# Magento with Varnish and HTTPS + Nginx

This repository contains the necessary configuration files to set up **Varnish with HTTPS support** for a Magento installation. Since Varnish does not support SSL/TLS natively, we use **Nginx as an SSL termination proxy** to forward requests to Varnish.

## 📁 Included Files

- **default.vcl** - Varnish configuration file for Magento.
- **domain.conf** - Nginx server block configuration for handling SSL termination.
- **nginx.conf.sample** - Sample Nginx configuration file.
- **varnish** - Script for managing Varnish service.
- **varnish.service** - Systemd service file for Varnish.

## 🚀 Setup Instructions

### 1️⃣ Install Dependencies
Ensure you have Nginx, Varnish, and Certbot (for SSL certificates) installed on your server.
```sh
sudo apt update
sudo apt install nginx varnish certbot python3-certbot-nginx -y
```

### 2️⃣ Configure Varnish
Replace the default Varnish configuration:
```sh
sudo cp default.vcl /etc/varnish/default.vcl
```
Restart Varnish:
```sh
sudo systemctl restart varnish
```

### 3️⃣ Configure Nginx for SSL Termination
Copy the Nginx configuration:
```sh
sudo cp domain.conf /etc/nginx/sites-available/magento
sudo ln -s /etc/nginx/sites-available/magento /etc/nginx/sites-enabled/
```
Get an SSL certificate (optional if using an existing cert):
```sh
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
```
Restart Nginx:
```sh
sudo systemctl restart nginx
```

### 4️⃣ Configure Systemd for Varnish
Copy the Varnish service file:
```sh
sudo cp varnish.service /etc/systemd/system/varnish.service
sudo systemctl daemon-reload
sudo systemctl restart varnish
```

## 🔄 How It Works
1. **Nginx listens on port 443 (HTTPS) and terminates SSL**.
2. **Nginx forwards requests to Varnish on port 80**.
3. **Varnish caches and processes Magento requests, forwarding them to Apache/PHP-FPM as needed**.

## ✅ Verification
Check if everything is working:
```sh
systemctl status varnish nginx
```
You should see both services **running** successfully.

## 📌 Notes
- Ensure your Magento base URL is set to **https://yourdomain.com**.
- You may need to clear Magento cache:
```sh
php bin/magento cache:flush
```
- If using Cloudflare, disable "Automatic HTTPS Rewrites".

## 📜 License
This setup is open-source and can be modified as needed. 🚀

