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
git clone https://github.com/kit-66/dockhand-qi.git && cd dockhand-qi && docker-compose up -d
```
или

```bash
docker compose up -d
# или, при использовании старого клиента:
# docker-compose up -d
```

---

3) Загрузка на сервер и запуск (через SCP + SSH)

Пример (локально в папке с docker-compose.yml):

```bash
scp docker-compose.yml user@server:/home/user/dockhand-qi/docker-compose.yml
ssh user@server 'cd /home/user/dockhand && docker compose pull && docker compose up -d'
```

---

4) Однострочная запись файла на сервер и запуск (если нет SCP)

```bash
ssh user@server 'mkdir -p /home/user/dockhand-qi&& cat > /home/user/dockhand-qi/docker-compose.yml' < docker-compose.yml
ssh user@server 'cd /home/user/dockhand-qi&& docker compose pull && docker compose up -d'
```

---

5) Инициализация репозитория (локально) и push на GitHub

```bash
git init
git add README.md docker-compose.yml
git commit -m "Add Dockhand quick deploy"
git branch -M main
git remote add origin git@github.com:kit-66/dockhand-qi.git
git push -u origin main
```

---

6) Скачивание файлов напрямую с GitHub (wget / curl)

Если ваш репозиторий публичный, можно скачать готовый `docker-compose.yml` прямо с raw-ссылки и запустить на сервере:

Пример — скачать и запустить в текущей директории:

```bash
wget -O docker-compose.yml https://raw.githubusercontent.com/kit-66/dockhand-qi/main/docker-compose.yml
# или
curl -fsSL -o docker-compose.yml https://raw.githubusercontent.com/kit-66/dockhand-qi/main/docker-compose.yml

docker compose pull
docker compose up -d
```

Однострочная команда для удалённого сервера (через SSH):

```bash
ssh user@server "mkdir -p /home/user/dockhand && cd /home/user/dockhand-qi && wget -O docker-compose.yml https://raw.githubusercontent.com/kit-66/dockhand-qi/main/docker-compose.yml && docker compose pull && docker compose up -d"
```

Примечание: замените `main` на фактическую ветку, если она отличается.

Если репозиторий приватный, используйте authenticated curl через GitHub API и токен (задать в переменной окружения):

```bash
export GITHUB_TOKEN=your_token_here
curl -H "Authorization: token $GITHUB_TOKEN" -H "Accept: application/vnd.github.v3.raw" -o docker-compose.yml "https://api.github.com/repos/kit-66/dockhand-qi/contents/docker-compose.yml?ref=main"
```

---

Примечания и безопасность:
- Проверьте актуальный тег образа в официальном репозитории; возможно, есть специфичный релизный тег.
- Dockhand управляет Docker через доступ к `/var/run/docker.sock`. Доступ к сокету даёт процессам в контейнере привилегии управления Docker и требует осторожности.
- Перед запуском скачанных файлов всегда просматривайте их содержимое (например, `cat docker-compose.yml`).
- При необходимости добавьте `.env.example` и секцию `environment` в `docker-compose.yml` для конфиденциальных настроек.
