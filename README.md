# Deviceを使ってみる

1. Gemfileに`gem 'device'`を追加して`bundle install`する

```ruby
gem 'devise'
```

2. `rails g devise:install`を実行する

```shell
bundle install
```

3. インストールと設定をする。
```shell
rails g devise:install
```

4. Deviceを生成する。
「account」はDevice用のモデル。

```shell
rails g devise account
```

5. モデルを生成したらマイグレーションをする。

```shell
rails db:migrate
```