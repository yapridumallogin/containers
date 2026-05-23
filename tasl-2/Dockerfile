# Stage 1: Build
FROM node:18-alpine AS builder

WORKDIR /app

# Копируем манифесты для кэширования зависимостей
COPY package*.json ./

# Устанавливаем зависимости
RUN npm ci --only=production && npm cache clean --force

# Копируем исходники
COPY . .

# Сборка приложения
RUN npm run build

# Stage 2: Runtime
FROM nginx:alpine

# Копируем собранные статические файлы из stage builder
COPY --from=builder /app/dist /usr/share/nginx/html

# Открываем порт, который использует nginx
EXPOSE 80

# Запускаем nginx в foreground режиме
CMD ["nginx", "-g", "daemon off;"]