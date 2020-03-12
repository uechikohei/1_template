# 1_template
2020/3/12 作成

(nginxリバース)

# 操作

`docker-compose build`

`docker-compose run app bash`

`rails new . --force --database=mysql --skip-test`

## database.yml変更
```
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch(“RAILS_MAX_THREADS”) { 5 } %>
  username: root
  password: root
  host: db

development:
  <<: *default
  database: app_development

test:
  <<: *default
  database: app_test

production:
  <<: *default
  database: app_production
  username: root
  password: root

```

`docker-compose up -d`

`docker-compose exec app bash`

`rails db:create`

`rails db:migrate`

=> yay!rails!になる。

![yay](https://user-images.githubusercontent.com/52321565/76489424-a7570e00-646b-11ea-9100-3d4beb80e03d.png)


# webpacker(bootstrapとsass)導入

`rails g controller home index`

### frontsディレクトリの作成

```
>
　├ app/
　 　└ fronts/
        ├ packs/
        |  └ application.js
        └ src/
           └ stylesheets/
              ├_custom.scss
              └ application.scss
```

fronts/packs/application.jsに記述
```
import "bootstrap"
import '../src/stylesheets/application'
```

fronts/src/stylesheets/application.scssに記述
```
@import "./custom";
@import "~bootstrap/scss/bootstrap";
```

config/routes.rbに記述
`root 'home#top'`

config/webpacker.yml内を変更
`check_yarn_integrity: false` - defaultもdevelopmentも変更する。

app/views/application.html.erbに記述
```
<%= stylesheet_pack_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
<%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
```

app/views/home/index.html.erbに記述
```
<h1>ここが赤になるよ</h1>
<p>Bootstrapと.scss適用されているとね！</p>
```

fronts/src/stylesheets/_custom.scss
```
h1{
    color:red;
}
```


# ブラウザで確認

`docker-compose build`

`dokcer-compose up`


<img width="503" alt="スクリーンショット 2020-03-12 14 06 51" src="https://user-images.githubusercontent.com/52321565/76489337-6828bd00-646b-11ea-94df-ca606c4f5b1e.png">
