---
{"dg-publish":true,"permalink":"/digital-garden/setting-up-nginx-as-a-reverse-proxy-with-load-balancing-and-https/","noteIcon":""}
---

#load-balancer #linux #proxy
NGINX is a powerful web server that can be used as a reverse proxy and load balancer with HTTPS support. This guide will walk you through setting up an NGINX proxy with SSL termination and multiple backend services.

---

## Generating Self-Signed SSL Certificates

To enable HTTPS, you first need to create an SSL certificate and Diffie-Hellman parameters. If generating the `dhparam.pem` file takes too long, use `sudo` if you're not running as root, but avoid `sudo` if already root.

```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt

sudo openssl dhparam -out /etc/nginx/dhparam.pem 4096
```

---

## Configuring SSL in NGINX

### Create a Self-Signed SSL Configuration Snippet

```
sudo nano /etc/nginx/snippets/self-signed.conf
```

Add the following lines:

```
ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;
```

### Configure SSL Security Parameters

```
sudo nano /etc/nginx/snippets/ssl-params.conf
```

Fill it with:

```
ssl_protocols TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_dhparam /etc/nginx/dhparam.pem;
ssl_ciphers ECDHE-RSA-AES256-GCM-SHA512:DHE-RSA-AES256-GCM-SHA512:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384;
ssl_ecdh_curve secp384r1;
ssl_session_timeout  10m;
ssl_session_cache shared:SSL:10m;
ssl_session_tickets off;
ssl_stapling on;
ssl_stapling_verify on;
resolver 8.8.8.8 8.8.4.4 valid=300s;
resolver_timeout 5s;
add_header X-Frame-Options DENY;
add_header X-Content-Type-Options nosniff;
add_header X-XSS-Protection "1; mode=block";
```

---

## Configuring NGINX Reverse Proxy with SSL

### Edit the Default Site Configuration

Backup the original file and edit:

```
sudo nano /etc/nginx/sites-available/default
```

Example configuration for a basic proxy setup:

```
server {
    listen 443 ssl;
    listen [::]:443 ssl;
    include snippets/self-signed.conf;
    include snippets/ssl-params.conf;

    server_name _;

    location / {
        proxy_pass         http://172.17.0.2:80;
        proxy_redirect     off;
        proxy_set_header   Host             $host;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        proxy_max_temp_file_size 0;
    }
}

server {
    listen 80;
    listen [::]:80;
    server_name 10.172.46.16;
    return 302 https://$server_name$request_uri;
}
```

---

## Configuring Custom Ports and Multiple Proxy Passes

For handling multiple services on different ports:

```
server {
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name _;
    include snippets/self-signed.conf;
    include snippets/ssl-params.conf;
    return 301 $scheme://202.77.99.56:9000$request_uri;
}

server {
    listen 9000 ssl;
    listen [::]:9000 ssl;
    include snippets/self-signed.conf;
    include snippets/ssl-params.conf;
    server_name _;
    location / {
        proxy_pass         http://172.17.0.2:9000;
        proxy_redirect     off;
        proxy_set_header   Host             $host;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        proxy_max_temp_file_size 0;
    }
}

server {
    listen 9300 ssl;
    listen [::]:9300 ssl;
    include snippets/self-signed.conf;
    include snippets/ssl-params.conf;
    server_name _;
    location / {
        proxy_pass         http://172.17.0.6:80;
        proxy_redirect     off;
        proxy_set_header   Host             $host;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        proxy_max_temp_file_size 0;
    }
}

server {
    listen 80;
    listen [::]:80;
    server_name _;
    return 302 https://$server_name$request_uri;
}
```

---

## Testing and Restarting NGINX

Once configurations are complete, test and restart NGINX:

```
sudo nginx -t
sudo service nginx restart
```

---

## Conclusion

By following this guide, you have set up NGINX as a secure reverse proxy with HTTPS support, custom ports, and multiple backend services. This configuration enhances security and performance while efficiently managing traffic across services.
```


