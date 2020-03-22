# Keycloak Service Block Configuration 

1. 

```markdown
$  export HOST_NAME=keycloak.petplannersoftware.com
```
2. Edit nginx site conf

```markdown
$   sudo nano /etc/nginx/sites-available/$HOST_NAME
```
petplanner.dev@gmail.com
sudo certbot --nginx -d $HOST_NAME 

   /etc/letsencrypt/live/keycloak.petplannersoftware.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/keycloak.petplannersoftware.com/privkey.pem


3. copy paste the below server (Note carefully overide the letsincrypt path)

```markdown
server {
    server_name keycloak.petplannersoftware.com;
  
    location /keycloak {
      proxy_set_header        Host $host;
      proxy_set_header        X-Real-IP $remote_addr;
      proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header        X-Forwarded-Proto $scheme;
  
      proxy_pass          http://localhost:8080;
      proxy_read_timeout  90;
  
   }
  
    location / {
      proxy_set_header        Host $host;
      proxy_set_header        X-Real-IP $remote_addr;
      proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header        X-Forwarded-Proto $scheme;
  
      proxy_pass          http://localhost:8081;
      proxy_read_timeout  90;
    }
  
  
    listen [::]:443 ssl ipv6only=on; # managed by Certbot
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/keycloak.petplannersoftware.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/keycloak.petplannersoftware.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
  
  }
  server {
      if ($host = keycloak.petplannersoftware.com) {
          return 301 https://$host$request_uri;
      } # managed by Certbot
  
  
    server_name keycloak.petplannersoftware.com;
      listen 80;
      return 404; # managed by Certbot
  }

```
4. check nginx syntax:

```bash
$ sudo nginx -t
```

5. Restart Nginx:

```bash
$ sudo systemctl restart nginx
```
