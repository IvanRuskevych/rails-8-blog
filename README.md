# Simple rails app

```bash
  rails new blog
```
```bash
  rails g scaffold post title:string body:text
  rails db:migrate
  bin/dev
```
 - create new post & http://localhost:3000/posts
 - api http://localhost:3000/posts.json
 - add to `app/views/layouts/application.html.erb` next code:
```rb
  <%= stylesheet_link_tag "https://cdn.simplecss.org/simple.css" %>
```
```bash
  rails console
  3.4.1 :001 > Post.first
  3.4.1 :003 > Post.first.update! title: "Update from CLI"
```
```bash
  rails action_text:install
  rails db:migrate
  bin/dev
```
- add to `app/models/post.rb` next code:
```
  has_rich_text :body
```
- add to `app/views/posts/_form.html.erb` next code:
```
  <%= form.rich_textarea :body %>
```
```bash
  bin/importmap pin local-time
```
- check file `config/importmap.rb` & fined `pin "local-time"`.
This files are shipped directly to the browser over HTTP2, so its nice & fast!!
- add to `app/javascript/application.js` next code:
```
  import LocalTime from "local-time"
  LocalTime.start();
```
```bash
  rails g resource comment post:references content:text
  rails db:migrate
  bin/dev
```
- add to `app/controllers/comments_controller.rb` next code:
```
  import LocalTime from "local-time"
  LocalTime.start();
```
- add to `app/models/post.rb` next code:
```
  class CommentsController < ApplicationController
  before_action :set_post
  def create
    @post.comments.create! params.expect(comment: [ :content ])
    redirect_to @post
  end

  private
    def set_post
      @post = Post.find(params[:post_id])
    end
end
```
- add to `app/models/post.rb` next code:
```
  has_many :comments
```
- create file `app/views/comments/_comments.html.erb`
```
<h2>Comments</h2>
<div id="comments">
  <%= render post.comments %>
</div>

<%= render "comments/new", post: post %>
```
- create file `app/views/comments/_comment.html.erb`
```
<div id="<%= dom_id(comment) %>">
  <%= comment.content %>
  <%= time_tag comment.updated_at, "data-local": "time-ago" %>
</div>
```
- create file `app/views/comments/_new.html.erb`
```
<%= form_with model: [ post, Comment.new ] do |form| %>
  Your comment: <br>
  <%= form.text_area :content, size: "20x5" %> <br>
  <%= form.submit %>
<% end %>
```
- add to `app/models/comment.rb` next code:
```
  broadcasts_to :post
  !! TODO: socet doesnt work - chack docs
```
## PRODUCTION-part time: 20m
- `config/routes.rb`
```
  TODO: uncomment root "posts#index" for production!!!
  root "posts#index"
```
## Authentication
```bash
    rails g authentication
    rails db:migrate
    bin/dev
```
- add to `db/seeds.rb` next code:
```
  User.create! email_address: "dhh@hey.com", password: "123"
```
```bash
  rails db:seed
```
- add to `app/views/layouts/application.html.erb` next code:
```
  <body>
    <%= button_to "Sign out", session_path, method: :delete if authenticated? %>
    <%= yield %>
  </body>
```

## PWA
- in file `app/views/layouts/application.html.erb` uncomment:
```
<%= tag.link rel: "manifest", href: pwa_manifest_path(format: :json) %>
```
- in file `config/routes.rb` uncomment:
```
  get "manifest" => "rails/pwa#manifest", as: :pwa_manifest
  get "service-worker" => "rails/pwa#service_worker", as: :pwa_service_worker
```
- you find manifest in the file `app/views/pwa/manifest.json.erb`. Also look to `app/views/pwa/service-worker.js`

# README

This README would normally document whatever steps are necessary to get the
application up and running.

Things you may want to cover:

* Ruby version

* System dependencies

* Configuration

* Database creation

* Database initialization

* How to run the test suite

* Services (job queues, cache servers, search engines, etc.)

* Deployment instructions

* ...
