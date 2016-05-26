### Lets Encrypt

Using [this](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-14-04) tutorial:

```
sudo apt-get update
sudo apt-get -y install git bc
sudo git clone https://github.com/letsencrypt/letsencrypt /opt/letsencrypt
sudo apt-get install nginx
sudo vim /etc/nginx/sites-available/default
```

```
location ~ /.well-known {
  allow all;
}
```

```
sudo service nginx reload
./letsencrypt-auto certonly -a webroot --webroot-path=/usr/share/nginx/html -d newsai.org -d www.newsai.org
sudo ls -l /etc/letsencrypt/live/newsai.org
sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
sudo vim /etc/nginx/sites-available/default
```

```

server {
  listen 80;
  server_name newsai.org www.newsai.org;
  return 301 https://$host$request_uri;
}

server {
  listen 443 ssl;
  server_name newsai.org www.newsai.org;
  ssl_certificate /etc/letsencrypt/live/newsai.org/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/newsai.org/privkey.pem;

  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  ssl_prefer_server_ciphers on;
  ssl_dhparam /etc/ssl/certs/dhparam.pem;
  ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA';
  ssl_session_timeout 1d;
  ssl_session_cache shared:SSL:50m;
  ssl_stapling on;
  ssl_stapling_verify on;
  add_header Strict-Transport-Security max-age=15768000;

  location / {
    proxy_pass http://app:8000;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }
}
```

```
sudo service nginx reload
```

Navigate [here](https://www.ssllabs.com/ssltest/analyze.html?d=newsai.org)

```
/opt/letsencrypt/letsencrypt-auto renew
sudo crontab -e
```

Add to crontab:

```
30 2 * * 1 /opt/letsencrypt/letsencrypt-auto renew >> /var/log/le-renew.log
35 2 * * 1 /etc/init.d/nginx reload
```
