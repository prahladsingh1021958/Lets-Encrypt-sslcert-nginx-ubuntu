# Let's Encrypt Overview:
Let's Encrypt is a free, automated, and open certificate authority (CA) that provides SSL/TLS certificates for securing websites. It aims to make the process of enabling HTTPS (SSL/TLS) easy and accessible to everyone. Let's Encrypt certificates are valid for 90 days and can be renewed automatically.

## End-to-End Steps for Let's Encrypt SSL Certificate with Nginx on GCP Ubuntu 22.04 VM

### SSH into your VM:
Connect to your Ubuntu VM on GCP using SSH. You can use the following command, replacing [your-vm-ip] with your actual VM's IP address:
```bash
ssh username@your-vm-ip
```

### Update packages:
```bash
sudo apt update
```

### Install Certbot:
Certbot is a tool that simplifies the process of obtaining and renewing SSL certificates.
 ```bash
sudo apt install certbot
```

### Stop Nginx:
Before obtaining the certificate, you might need to stop Nginx temporarily:
 ```bash
sudo service nginx stop
```

### Obtain SSL Certificate:
Run Certbot to obtain the SSL certificate. Make sure to replace yourdomain.com with your actual domain:
 ```bash
sudo certbot certonly --standalone -d yourdomain.com
```
Follow Certbot prompts:
Certbot will ask for your email address and will also ask if you want to receive emails from the Electronic Frontier Foundation. Choose according to your preference.

### Configure Nginx to use the SSL certificate:
Update your Nginx configuration file to use the obtained SSL certificate. Open the Nginx configuration file for editing. This file is typically located at /etc/nginx/sites-available/default or in a similar location.

 ```bash
sudo nano /etc/nginx/sites-available/default
```

Add or update the following lines to include SSL certificate paths:
 ```bash
server {
    listen 443 ssl;
    server_name yourdomain.com;

    ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;
    
    # Other SSL settings...

    # Rest of your server configuration...
}
```

### Restart Nginx:
After updating the configuration, restart Nginx:
 ```bash
sudo service nginx restart
```

### Verify Nginx Configuration:
Ensure there are no syntax errors:
 ```bash
sudo nginx -t
```

### Restart Nginx Again:
 ```bash
sudo service nginx restart
```

### Automate Certificate Renewal:
Certificates obtained from Let's Encrypt are valid for 90 days. It's recommended to set up automatic renewal. You can do this by adding a cron job:
 ```bash
sudo crontab -e
```
Add the following line to check for renewals twice a day:
 ```bash
0 */12 * * * certbot renew
```
Save and exit the editor.

### Verify Automatic Renewal:
You can test the renewal process by running:
 ```bash
sudo certbot renew --dry-run
```

### Verify HTTPS on your Nginx Page:
Visit your website in a browser using https://yourdomain.com. Ensure the browser shows a secure connection (a padlock icon).

Congratulations! You have successfully configured Let's Encrypt SSL for Nginx on your GCP Ubuntu 22.04 VM. The provided steps should help you secure your website with HTTPS.


