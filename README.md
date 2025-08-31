## Локальный запуск

### Установите Docker:
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

### Запуск docker compose
```bash
docker compose --profile production build
docker compose --profile production up -d
```