# Dockhand — Быстрое развёртывание

Краткая шпаргалка для быстрого развёртывания Dockhand с помощью Docker / Docker Compose.

Официальный репозиторий:
- https://github.com/Finsys/dockhand
- Официальная документация: https://dockhand.pro/

---

1) Запуск одиночным контейнером (docker run)

```bash
docker run -d \
  --name dockhand \
  --restart unless-stopped \
  -p 3000:3000 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v dockhand_data:/app/data \
  fnsys/dockhand:latest
```

Откройте в браузере: http://<HOST>:3000

---

2) Docker Compose (рекомендуется для сервера)

Создайте файл `docker-compose.yml` (пример ниже) и выполните:

```bash
docker compose up -d
# или, при использовании старого клиента:
# docker-compose up -d
```

---

3) Загрузка на сервер и запуск (через SCP + SSH)

Пример (локально в папке с docker-compose.yml):

```bash
scp docker-compose.yml user@server:/home/user/dockhand/docker-compose.yml
ssh user@server 'cd /home/user/dockhand && docker compose pull && docker compose up -d'
```

---

4) Однострочная запись файла на сервер и запуск (если нет SCP)

```bash
ssh user@server 'mkdir -p /home/user/dockhand && cat > /home/user/dockhand/docker-compose.yml' < docker-compose.yml
ssh user@server 'cd /home/user/dockhand && docker compose pull && docker compose up -d'
```

---

5) Инициализация репозитория (локально) и push на GitHub

```bash
git init
git add README.md docker-compose.yml
git commit -m "Add Dockhand quick deploy"
git branch -M main
git remote add origin git@github.com:kit-66/dockhand.git
git push -u origin main
```

---

Примечания:
- Проверьте актуальный тег образа в официальном репозитории; возможно, есть специфичный релизный тег.
- Для управления Docker-контейнерами на сервере Dockhand использует доступ к /var/run/docker.sock — убедитесь, что это безопасно в вашем окружении.
- При желании добавьте `.env.example` и секцию `environment` в docker-compose.yml для конфиденциальных настроек.
