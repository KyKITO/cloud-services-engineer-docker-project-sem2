# Используем образ для сборки 
FROM node:18-alpine AS builder

# Рабочая директория
WORKDIR /app

# Копируем package файлы первыми для кэширования
COPY package*.json ./

# Устанавливаем зависимости
RUN npm ci && npm cache clean --force

# Используем аргументы и переменные окружения 
ARG VUE_APP_API_URL=/api
ENV VUE_APP_API_URL=$VUE_APP_API_URL \
    NODE_OPTIONS=--openssl-legacy-provider \
    NODE_ENV=production

# Копируем код приложения 
COPY . .
RUN npm run build

# Используем основной образ
FROM nginx:1.25-alpine

# Создаем пользователя с фиксированными UID/GID
RUN addgroup -S -g 1001 webapp && \
    adduser -S -u 1001 -G webapp -s /sbin/nologin webapp

# Настройка nginx одной командой
RUN mkdir -p /tmp /var/cache/nginx/client_temp /usr/share/nginx/html/momo-store && \
    sed -i -e 's|pid[[:space:]].*;|pid /tmp/nginx.pid;|' \
           -e '/user[[:space:]]*nginx;/c\user webapp;' /etc/nginx/nginx.conf && \
    rm -f /etc/nginx/conf.d/default.conf && \
    chown -R webapp:webapp /usr/share/nginx/html /var/cache/nginx /var/log/nginx /etc/nginx/conf.d /tmp

# Копируем nginx.conf и статику
COPY ../frontend-nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=builder --chown=webapp:webapp /app/dist /usr/share/nginx/html/momo-store

# Используем noroot пользователя
USER 1001:1001

# Пробрасываем порт 
EXPOSE 80

# Команда запуска 
CMD ["nginx", "-g", "daemon off;"]