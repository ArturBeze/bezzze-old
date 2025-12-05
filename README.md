# ✅ Настройка Nginx (на хостинге)

Создай файл:

/etc/nginx/sites-available/bezzze.ru

    server {
        listen 80;
        server_name bezzze.ru www.bezzze.ru;
    
        # Автоматический редирект на HTTPS
        location / {
            return 301 https://$host$request_uri;
        }
    }
    
    server {
        listen 443 ssl;
        server_name bezzze.ru www.bezzze.ru;
    
        # Пути к сертификатам Let's Encrypt
        ssl_certificate /etc/letsencrypt/live/bezzze.ru/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/bezzze.ru/privkey.pem;
        ssl_protocols TLSv1.2 TLSv1.3;
        ssl_ciphers HIGH:!aNULL:!MD5;
    
        location / {
            proxy_pass         http://127.0.0.1:5678/;
            proxy_http_version 1.1;
            proxy_set_header   Upgrade $http_upgrade;
            proxy_set_header   Connection 'upgrade';
            proxy_set_header   Host $host;
            proxy_cache_bypass $http_upgrade;
    
            # Для корректной работы вебхуков
            proxy_set_header   X-Forwarded-Proto $scheme;
            proxy_set_header   X-Forwarded-Host $host;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }

Используем certbot:

    sudo apt update
    
    sudo apt install certbot python3-certbot-nginx -y
    
    sudo certbot --nginx -d bezzze.ru -d www.bezzze.ru


Certbot автоматически настроит сертификаты и редирект с HTTP на HTTPS.

После этого n8n будет доступен по https://bezzze.ru

# Потом:

    sudo ln -s /etc/nginx/sites-available/bezzze.ru /etc/nginx/sites-enabled/

    sudo nginx -t

    sudo systemctl reload nginx

# Получение сертификата Let's Encrypt

# 🔥 Как запустить

    docker compose up -d
