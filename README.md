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
