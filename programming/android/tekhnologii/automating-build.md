# Automating build

Есть отличный докер образ thyrlian/android-sdk-vnc и thyrlian/android-sdk \(без поддержки VNC; нужно для подключения к интерфейсу\)

Пример Docker файла \(размещаем в корень проекта Android-приложения\)

```yaml
FROM thyrlian/android-sdk-vnc

WORKDIR /home
COPY . .
RUN ./gradlew build :app:assembleRelease

```

И соотв docker compose файл \(docker-compose -f build.yaml build\)

```yaml
version: '3'
services:
  builder:
    container_name: android-builder
    build:
      context: ./
      dockerfile: Dockerfile
    restart: always
```

