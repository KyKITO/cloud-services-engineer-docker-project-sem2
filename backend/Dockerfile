# Используем образ для сборки
FROM golang:1.21-alpine AS builder

# Устанавливаем git для приватных модулей
RUN apk add --no-cache git 

WORKDIR /app

# Копируем только файлы зависимостей для лучшего кэширования
COPY go.mod go.sum ./
RUN go mod download

# Копируем исходный код
COPY . .

# Компилируем с дополнительными оптимизациями
RUN CGO_ENABLED=0 GOOS=linux go build \
    -a -installsuffix cgo \
    -ldflags '-w -s -extldflags "-static"' \
    -o /app/bin/app ./cmd/api

# Финальный образ - используем distroless для безопасности
FROM gcr.io/distroless/static-debian11:nonroot

# Копируем бинарник
COPY --from=builder /app/bin/app /app

# distroless уже содержит nonroot пользователя
USER 65532:65532

EXPOSE 8081

# Используем exec форму для корректной обработки сигналов
CMD ["/app"]