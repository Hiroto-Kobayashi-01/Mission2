# 課題2
---
## 課題内容
* Railsを使ったAPI経由でデータベース(MySQL or SQLite or MongoDB)に氏名/年齢/住所を登録してください
* 追加:データ入力&データ一覧機能も実装
  

## 課題実行方法
 ### 環境構築
 **1.Rubyのダウンロード**
 
  1-1.Homebrewのインストール(以下参照)
  > https://aiacademy.jp/media/?p=2817

  1-2.rbenvのインストール
 ターミナルで以下のコマンドを入力
 > brew install rbenv

 1-3.rubyのインストール
 * ターミナルで以下のコマンドを入力
 > rbenv install --list

 * Rubyのダウンロード可能なバージョンが書かれているので最新バージョンを選びダウンロードする
> rbenv install 3.3.0(←例)

 * ダウンロードしたrubyを利用するために以下のコマンドを入力
> rbenv global 3.3.0(←例)


**2.bundlerとrailsのインストール**

ターミナルで以下のコマンドを入力
> gem install bundler
> gem install rails

---

### 課題2の実行内容

**1.railsの雛形作成**


1-1. フォルダを作成しcdコマンドで移動する

1-2. 以下のコマンドを入力しrailsの雛形を作成する
> rails new (任意の名前)

1-3. 任意のエディタで1-2を立ち上げる

<br>

**2.ルーティング設定**

configフォルダ内のroutes.rbを開き以下のコードを入力する
```
Rails.application.routes.draw do

resources :posts, only: [:index, :new, :create, :show, :edit, :update]

end
```

<br>

**3.model設定**
   
3-1. 以下のコマンドを入力しモデルを作成する
> rails g model Posts name:string age:integer address:string

3-2. 以下のコマンドを入力しマイグレーションする
> rails db:migrate

<br>

**4.controller設定**

4-1. 以下のコマンドでファイルを作成する
> rails g controller Posts

4-2. 4-1のファイルに以下のコードを入力する
```
class PostsController < ApplicationController

    def index
        @posts = Post.all
    end

    def new
        @post = Post.new
    end

    def create
        @post = Post.new(post_params)

        if @post.save
            redirect_to posts_path
        else
            render :new
        end
    end

    def edit
        @post = Post.find(params[:id])
    end

    def update
        @post = Post.find(params[:id])
        if @post.update(post_params)
            redirect_to posts_path
        else
            render :edit
        end
    end
   
    def show
        @post = Post.find(params[:id])
    end

    
     private

    def post_params
        params.require(:post).permit(:name, :age, :address)
    end
end
```
<br>

**5.viewの設定**

5-1.appフォルダ内のviewsを開き、postsというファイルの中に、index.html.erb　new.html.erb　show.html.erb　edit.html.erb　を作成する

5-2. それぞれ以下のコードを入力する
```
#index.html.erb
<h1>一覧</h1>


 <% @posts.each do |post| %>
 <%= link_to post.name, post_path(post) %>

 <% end %>

 <br><%= link_to "新規登録", new_post_path %> </br>
```

```
#new.html.erb
<h1>情報登録フォーム</h1>

<%= form_with model: @post do |form| %>
 <%= form.label :name %><%= form.text_field :name %>

 <%= form.label :age %><%= form.number_field :age %>

 <%= form.label :address %><%= form.text_field :address %>

<br>
 <%= form.submit "登録" %>
</br>
 <% end %>
```

```
#show.html.erb
<h1>登録情報</h1>

<p>
  <strong>Name:</strong>
  <%= @post.name %>
</p>

<p>
  <strong>Age:</strong>
  <%= @post.age %>
</p>

<p>
  <strong>Address:</strong>
  <%= @post.address %>
</p>

<p>
 <%= link_to "一覧へ",  posts_path%>
 <%= link_to "編集", edit_post_path(@post) %> 
 
 </p>
```

```
#edit.html.erb

<h1>登録情報の変更</h1>

<%= form_with model: @post do |form| %>
 <%= form.label :name %><%= form.text_field :name %>

 <%= form.label :age %><%= form.number_field :age %>

 <%= form.label :address %><%= form.text_field :address %>

<br>
 <%= form.submit "更新" %>
</br>
 <% end %>
```
<br>

**6.サーバーの立ち上げと情報登録**

6-1. 以下のコマンドでサーバーを立ち上げる
> rails s

6-2. webページを開き以下のURLを読み込む
> localhost:3000/posts

6-3. 開いたwebページから登録されている情報の閲覧と情報の登録を行える








