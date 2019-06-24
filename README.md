# Vue単体をDockerで動かす

## 起動方法

$ docker-compose build
$ docker-compose up -d
$ docker-compose exec web /bin/ash
$ cd vue-project/
$ npm run serve

## 消去

$ docker-compose down -v

## やったこと

Dockerfileを作成
$ vim Dockerfile
```
FROM node:12.4.0-alpine

WORKDIR /app

RUN apk update&& \
    npm install -g npm && \
    npm install -g @vue/cli

CMD ["/bin/ash"]
```

docker-compose.ymlを作成
$ vim docker-compose.yml
```
version: '3'
services:
  web:
    build: .
    ports:
      - 1234:8080
    volumes:
      - .:/app
    stdin_open: true
    tty: true
```
※ stdin_open:コンテナの標準入力をオープンしたままにする
※ tty:コンテナに疑似TTYを割り当てる

$ docker-compose build
$ docker-compose up -d
$ docker-compose exec web /bin/ash
以下、docker内部
※ volumesでマウントされたルートディレクトリに作成される
$ vue create vue-project
$ cd vue-project
$ npm run serve
http://localhost:1234/ にアクセスするとVueが開く