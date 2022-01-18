# About

This is my Vaultwarden setup

## Nginx setting

```
server {
        location ~ ^(.*)$ {
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
            proxy_pass http://localhost:8089;
        }
        
        client_max_body_size 50M;
        server_name vaultwarden.HOSTNAME;

        listen [::]:443 ssl ipv6only=on; # managed by Certbot
        listen 443 ssl; # managed by Certbot
        
        ssl_certificate /etc/letsencrypt/live/vaultwarden.HOSTNAME/fullchain.pem; # managed by Certbot
        ssl_certificate_key /etc/letsencrypt/live/vaultwarden.HOSTNAME/privkey.pem; # managed by Certbot
        include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
        ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}
server {
        if ($host = vaultwarden.HOSTNAME) {
            return 301 https://$host$request_uri;
        } # managed by Certbot


        listen 80;
        listen [::]:80;

        server_name vaultwarden.HOSTNAME;
        return 404; # managed by Certbot
}
```
