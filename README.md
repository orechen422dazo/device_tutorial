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

## Hello Controllerを作成する

```shell
rails g controller hello index login_check
```

views/hello/index.html.erbを作成する。

```html
<h1>Hello#index</h1>
<p><%= @msg %></p>
<hr>
<div class="link-container">
  <%= link_to 'Login Check Page &gt;&gt;'.html_safe, { action: 'login_check' }, class: 'styled-link' %>
</div>

<style>
    body {
        font-family: Arial, sans-serif;
        margin: 20px;
    }
    h1 {
        color: #333;
    }
    p {
        font-size: 18px;
    }
    .link-container {
        margin: 10px 0;
    }
    .styled-link {
        background-color: #4CAF50;
        color: white;
        padding: 10px 20px;
        text-decoration: none;
        display: inline-block;
        border-radius: 12px;
    }
    .styled-link:hover {
        background-color: #45a049;
    }
</style>
```

views/hello/login_check.html.erbを作成する。

```html
<h1>Hello#login_check</h1>
<p>Find me in app/views/hello/login_check.html.erb</p>

<p>Logged in as: <%= @account.email %></p>
<p>Account created at: <%= @account.created_at %></p>

<%= button_to "Logout", destroy_account_session_path, method: :delete, class: "logout-button" %>

<style>
    .logout-button {
        background-color: #ff4d4d;
        color: white;
        border: none;
        padding: 10px 20px;
        text-align: center;
        text-decoration: none;
        display: inline-block;
        font-size: 16px;
        margin: 4px 2px;
        cursor: pointer;
        border-radius: 12px;
    }
    .logout-button:hover {
        background-color: #ff1a1a;
    }
</style>
```

hello_controller.rbを編集する。

```ruby
class HelloController < ApplicationController
  layout "application"
  before_action :authenticate_account!, only: :login_check
  def index
    @msg = "this is sample page."
  end

  def login_check
    @account = current_account
    @msg = "account created at: " + @account.created_at.to_s
  end
end
```

routes.rbを編集する。

```ruby
Rails.application.routes.draw do
  get "hello/index"
  get "hello/login_check"
  devise_for :accounts
  # Define your application routes per the DSL in https://guides.rubyonrails.org/routing.html

  # Reveal health status on /up that returns 200 if the app boots with no exceptions, otherwise 500.
  # Can be used by load balancers and uptime monitors to verify that the app is live.
  get "up" => "rails/health#show", as: :rails_health_check

  # Render dynamic PWA files from app/views/pwa/* (remember to link manifest in application.html.erb)
  # get "manifest" => "rails/pwa#manifest", as: :pwa_manifest
  # get "service-worker" => "rails/pwa#service_worker", as: :pwa_service_worker

  # Defines the root path route ("/")
  # root "posts#index"
  # ルートを追加
  get 'hello', to: 'hello#index'
end
```
