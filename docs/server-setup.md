# Подготовка сервера

Учебная VM из практической работы № 7: `devops-vm`. Подключение с хоста: `ssh devops` (пользователь `devops`, адрес VM в сети VirtualBox `192.168.56.101`). На VM установлен Docker.

Публичный пакет: [ghcr.io/artemkaobraz/devops-ci-app](https://github.com/artemkaobraz/devops-ci-app/pkgs/container/devops-ci-app). Для чтения пакета токен и `docker login` не нужны. Образ опубликован для `linux/amd64` и `linux/arm64`.

Для развёртывания выбран полный SHA проверенного коммита:

```text
78d1e1c87e35d260847742e6bd10d6ed2b6da7ff
```

Команды проверки на VM:

```bash
docker pull ghcr.io/artemkaobraz/devops-ci-app:78d1e1c87e35d260847742e6bd10d6ed2b6da7ff
docker run -d --name ci-app -p 127.0.0.1:5001:5000 ghcr.io/artemkaobraz/devops-ci-app:78d1e1c87e35d260847742e6bd10d6ed2b6da7ff
curl --fail http://127.0.0.1:5001/health
docker exec ci-app id
docker logs ci-app
docker rm -f ci-app
```

Ожидаемый ответ `/health`: `{"status":"ok"}`. Приложение внутри контейнера работает от пользователя `app`. Публикация порта ограничена адресом `127.0.0.1` VM.

Для будущего развёртывания через Ansible используйте полный SHA вместо `latest`: `latest` перемещается после новых публикаций, а SHA однозначно указывает на версию, прошедшую CI. Образ следует загружать из GHCR, а не пересобирать на VM.
