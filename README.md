# ✅ Настройка Nginx (на хостинге)

Создай файл:

/etc/nginx/sites-available/bezzze.ru

server {
    listen 80;
    server_name bezzze.ru www.bezzze.ru;

    location / {
        proxy_pass         http://127.0.0.1:5678;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection 'upgrade';
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}


# Потом:

sudo ln -s /etc/nginx/sites-available/bezzze.ru /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx

# 🔥 Как запустить
docker compose up -d
