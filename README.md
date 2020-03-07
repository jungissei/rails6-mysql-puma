Ruby on Rails6 / MySQL5.7 / puma / Dockerのアプリケーション開発雛形

## Railsスケルトンアプリインストール

```
$ docker-compose run web rails new . --force --no-deps --database=mysql --skip-test --webpacker
```

## 各ファイル修正

### /config/puma.rb

```
# ・・・・省略

# Allow puma to be restarted by `rails restart` command.
plugin :tmp_restart

# ここから下を追記--------------------------------------------
app_root = File.expand_path("../..", __FILE__)
bind "unix://#{app_root}/tmp/sockets/puma.sock"

stdout_redirect "#{app_root}/log/puma.stdout.log", "#{app_root}/log/puma.stderr.log", true
```

### /config/database.yml

```
# ・・・省略

default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: <%= ENV['DB_USERNAME'] %>
  password: <%= ENV['DB_PASSWORD'] %>
  host: <%= ENV['DB_HOST'] %>

# ・・・省略
```

### /docker-compose.yml

commandとvolumsの部分のコメントアウトを外す

## イメージビルド

```
$ docker-compose build
```

## コンテナ立ち上げ
```
$ docker-compose up -d
```
