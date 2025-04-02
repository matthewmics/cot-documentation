# NGINX

:::info
In our server. The default nginx configuration can be found here

```
cd /etc/nginx/sites-available
nano default
```

Using nano you can modify the configuration
:::

## Overview

This Nginx configuration is set up to serve multiple applications under different locations. It handles API requests, WebSocket connections, static file hosting, and reverse proxying for both production and test environments. Additionally, it includes SSL settings managed by Certbot.

## Configuration Details

### Server Block (HTTPS - Port 443)

-   Domain: `recruitment.connextglobal.com`
-   SSL Enabled: Managed by Certbot
-   Reverse Proxy for APIs:
    -   `/api` → http://127.0.0.1:3001
    -   `/apiex` → http://127.0.0.1:3002
-   WebSocket Handling: - /socket.io → http://127.0.0.1:3001 - /socketex → http://127.0.0.1:3002
    :::info
    We don't use websockets anymore but just to note we can also proxy it using nginx
    :::
-   Static File Hosting: - `/directory` serves files from /home/ubuntu/directory/recruitment-system

        - `/` serves files from /home/ubuntu/recruitment-system/client/dist

        - `/experimental` serves files from /home/ubuntu/test/recruitment-system/client/dist

        - `/community` serves files from /home/ubuntu/test/recruitment-system/client-community/dist

    :::info
    We don't use `/community` anymore. This was used for applicant login but the development of this has been halted
    :::

### API Reverse Proxy Settings

    - `Each API endpoint is proxied with:`

    - `proxy_http_version 1.1`

    - `proxy_set_header Upgrade $http_upgrade`

    - `proxy_set_header Connection 'upgrade'`

    - `proxy_set_header Host $host`

    - `proxy_cache_bypass $http_upgrade`

    - `proxy_set_header X-Forwarded-Proto $scheme`

    - `proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for`

### Static File Caching

    - All static assets (`.js, .css, .png, .jpg, .jpeg, .gif, .ico, .svg`) have a cache expiration of 30 days.

    - Cache-control headers ensure efficient browser caching while preventing unnecessary reloads.

### SSL Configuration (Managed by Certbot)

    - SSL certificates are stored at:

        - `/etc/letsencrypt/live/recruitment.connextglobal.com/fullchain.pem`

        - `/etc/letsencrypt/live/recruitment.connextglobal.com/privkey.pem`

    - Uses recommended SSL settings from Certbot (`/etc/letsencrypt/options-ssl-nginx.conf` and `ssl-dhparams.pem`).

### HTTP to HTTPS Redirect (Port 80)

    - Redirects all traffic from HTTP to HTTPS (`return 301 https://$host$request_uri`).

    - If the request does not match the domain, it returns a `404` response.

## Usage

### Starting Nginx

To apply the configuration, restart Nginx:

```
sudo systemctl restart nginx
```

### Checking Configuration Validity

Before restarting, test the configuration for errors:

```
sudo nginx -t
```

### Updating SSL Certificates

To renew SSL certificates manually:

```
sudo certbot renew --dry-run
```

:::info
In our server we have a cron for this just follow these steps:

```
sudo su
crontab -e
```

:::

## Current NGINX Configuration

```


server {
	server_name recruitment.connextglobal.com;
	client_max_body_size 500M;
	location /api {
		proxy_pass http://127.0.0.1:3001;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
    	        proxy_set_header Connection 'upgrade';
    		proxy_set_header Host $host;
    		proxy_cache_bypass $http_upgrade;
    		proxy_set_header X-Forwarded-Proto $scheme;
    		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    		client_max_body_size 10g;
	}

        location /apiex {
                proxy_pass http://127.0.0.1:3002;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
                proxy_set_header X-Forwarded-Proto $scheme;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                client_max_body_size 10g;
        }

	location /directory {
		alias /home/ubuntu/directory/recruitment-system;
		autoindex on;
		index off;
		try_files $uri $uri/ =404;
		default_type text/plain;
	}


	location /socketex {
        proxy_pass http://127.0.0.1:3002;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        }


	location /socket.io {
        proxy_pass http://127.0.0.1:3001;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        }

	root /home/ubuntu/recruitment-system/client/dist;
	index index.html;

	location / {
		try_files $uri $uri/ /index.html;

		location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg)$ {
                 expires 30d;
                 access_log off;
                 add_header Cache-Control "public, max-age=2592000";
                }

		add_header Cache-Control "no-store, no-cache, must-revalidate";
		expires 0;
	}

	location /experimental {

		alias /home/ubuntu/test/recruitment-system/client/dist;
	        try_files $uri $uri/ /experimental/index.html;

		location ~* ^/experimental/(.+\.(js|css|png|jpg|jpeg|gif|ico|svg))$ {
                  expires 30d;
                  access_log off;
                  add_header Cache-Control "public, max-age=2592000";
                  alias /home/ubuntu/test/recruitment-system/client/dist/$1;  # Ensure this points to the correct directory
                }

                add_header Cache-Control "no-store, no-cache, must-revalidate";
	}


        location /community {

                alias /home/ubuntu/test/recruitment-system/client-community/dist;
                try_files $uri $uri/ /community/index.html;

                location ~* ^/community/(.+\.(js|css|png|jpg|jpeg|gif|ico|svg))$ {
                  expires 30d;
                  access_log off;
                  add_header Cache-Control "public, max-age=2592000";
                  alias /home/ubuntu/test/recruitment-system/client-community/dist/$1;  # Ensure this points to the correct directory
                }

                add_header Cache-Control "no-store, no-cache, must-revalidate";
        }


    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/recruitment.connextglobal.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/recruitment.connextglobal.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}
server {
    if ($host = recruitment.connextglobal.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


	listen 80;
	server_name recruitment.connextglobal.com;
    return 404; # managed by Certbot


}

```

## Conclusion

This Nginx configuration is structured to serve multiple applications efficiently, ensuring secure API handling, WebSocket support, static file caching, and automatic SSL management. Modifications should be tested with `nginx -t` before applying changes.
