# Let's Encrypt Overview:
Let's Encrypt is a free, automated, and open certificate authority (CA) that provides SSL/TLS certificates for securing websites. It aims to make the process of enabling HTTPS (SSL/TLS) easy and accessible to everyone. Let's Encrypt certificates are valid for 90 days and can be renewed automatically.

## End-to-End Steps for Let's Encrypt SSL Certificate with Nginx on GCP Ubuntu 22.04 VM

### SSH into your VM:
```bash
ssh username@your-vm-ip
```

### Update packages:
```bash
sudo apt update
```

### Install Certbot:
 ```bash
sudo apt install certbot
```

### Stop Nginx:
 ```bash
sudo service nginx stop
```

### Obtain SSL Certificate:
 ```bash
sudo certbot certonly --standalone -d yourdomain.com
```

### Configure Nginx:
Edit Nginx configuration file:

 ```bash
sudo nano /etc/nginx/sites-available/default
```

Update the file with SSL certificate paths.
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

### Set up Automatic Renewal:

 ```bash
sudo crontab -e
```

### Add the following line:
 ```bash
0 */12 * * * certbot renew
```

### Verify Automatic Renewal:
 ```bash
sudo certbot renew --dry-run
```

### Verify HTTPS on your Nginx Page:
Visit your website in a browser using https://yourdomain.com. Ensure the browser shows a secure connection (a padlock icon).

Congratulations! You have successfully configured Let's Encrypt SSL for Nginx on your GCP Ubuntu 22.04 VM. The provided steps should help you secure your website with HTTPS.


