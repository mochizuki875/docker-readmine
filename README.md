# Docker Redmine

## 概要
`sameersbn/docker-redmine`をベースとしてdocker環境にRedmine、Nginx R-Proxy、監視コンポーネントを構築する。
https://github.com/sameersbn/docker-redmine

## クイックスタート

~~~
cd redmine
docker-compose up -d
cd nginx
docker-compose up -d
cd monitor
docker-compose up -d
~~~