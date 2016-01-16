---
layout: post
title: "Build a Blog with Rails"
categories: Rails
author: Nick Calabro
image: railswelcome.jpg
---

<meta name="twitter:card" content="summary" />
<meta name="twitter:site" content="@NickCalabs" />
<meta name="twitter:title" content="{{ page.title }}" />
<meta name="twitter:description" content="Nick Calabro's Blog" />

##Rails New

The website you are on right now is a fairly basic blog written in Ruby on Rails. It allows users to create posts and involves some pretty exciting syntax highlighting and Bootstrap integration. In a series of articles, I will go through, step by step, how to create this very site - from rails new all the way to deployment.

This is designed for someone who is comfortable with code in general, however, I’ll be doing my best to describe what each step does and why we’re doing it. Hopefully, you and I will learn more about the intricacies of a site like this.

First we have to open up Terminal. Navigate to the directory you’d like this project to live in and create a new Rails app. In this case I’ll be going to the Desktop.

```console
cd Desktop
rails new blog
```

You'll see everything being created in the terminal. Now, enter the blog directory and start the Rails server.

```console
cd blog
rails s
```

From here, navigate to `http://localhost:3000`. You should see this screen letting you know everything is running smoothly.

<img src="/img/{{ page.image }}">

##Git Init

Open up your project in your favorite text editor. You should see a whole file tree including app, bin, config, and whatnot. We’re going to touch nearly everything here so be patient. While we’re here, now might be a good time to create our git repository. In your terminal, type:

```console
git init
git status
git add .
git status
git commit -m "Initial commit"
```

This is going to create the repository. Then, with status we can see all the untracked files in red. Add the entire directory, check the status again, and commit it all with a little message.

From here, we need to add some functionality to our app. First we need to create a controller for our posts.

```console
rails generate controller posts
```

You’ll see your terminal spit out some stuff after this one. To see what the generate did, navigate to `app/controllers/` and you’ll find the `posts_controller.rb` was born. We’re going to want the posts to be at the root of our site and we need there to be some functionality to create a new post. The first step for this is to give it the index and the new actions. Here’s your `posts_controller`:

```ruby
class PostsController < ApplicationController
	def index
	end 

	def new
	end
end
```

Going to the routes file (`config/routes.rb`). We can start by deleting all the comments in this file. It’s very cluttered and can be daunting. At this point, you should be left with only two lines. Inside, you’re going to add some routes. We need a route for our posts, and we also want that posts page to be our root. At the end, your `routes.rb` should look like this:

```ruby
Rails.application.routes.draw do
	resources :posts
	root "posts#index"
end
```

At this point, if you refresh your browser (be sure your server is still running [rails s]) you’ll find an error telling you you’re missing a template. To fix this, head over to `app/views/posts/` and add a new file called `index.html.erb`. In this file, feel free to add some dummy HTML and just make sure it shows up in your browser. We also have the new action to worry about. Create another file inside `app/views/posts/` called `new.html.erb`. Now, if we navigate to `http://localhost:3000/posts/new`, we’ll get that file. In your `new.html.erb` file, we’ll need a form to actually let the user create a post with. It’ll look like this.

```html
<%= form_for :post, url: posts_path do |f| %>
    <p>
        <%= f.label :title %><br>
        <%= f.text_field :title %>
    </p>

    <p>
        <%= f.label :body %><br>
        <%= f.text_area :body %>
    </p>

    <p>
        <%= f.submit %>
    </p>

<% end %>
```

##Rake

Navigate to `localhost:3000/posts/new` and enjoy the view of the wonderfully boring form you just created. Now, if you hit submit on this page, your data isn’t going to really go anywhere. The next step is to generate a model. Head over to your terminal and repeat after me:

```console
rails g model Post title:string body:text
rake db:migrate
```

You should see under `db/migrate` and `app/models` the newest files just created. In `posts_controller.rb` we’re going to create our create method and our private method to permit the parameters. We’re also going to add the show method for when we want to display an individual post. `posts_controller.rb` will look like this:

```ruby
class PostsController < ApplicationController
    def index
    end

    def new
    end

    def create
        @post = Post.new(post_params)
        @post.save

        redirect_to @post
    end

     def show
        @post = Post.find(params[:id])
    end

    private
        def post_params
            params.require(:post).permit(:title, :body)
        end
end
```

##Show
Before we create a post, we have to create the show page. Again, under `app/views/posts`, create a new file called `show.html.erb`. Here, we’re going to want to display the title and body of our posts. Also, I threw in some interesting code to get the date of the post to show up as well.

```html
<h1><%= @post.title %></h1>

<p>Submitted <%= time_ago_in_words(@post.created_at) %> ago</p>
<p><%= @post.body %></p>
```

If you go to `localhost:3000/posts/new`, create your post, and navigate to `localhost:3000/posts/1`, you should see your first ever blog post. The final step for now is to be sure we can access all the posts currently in our database. In `posts_controller.rb`, inside the index action add

```ruby
@posts = Post.all.order(‘created_at DESC’)
```

Then, in `index.html.erb` we need to create a loop to display all our posts.

```ruby
<% @posts.each do |post| %>
    <div>
        <h2><%= link_to post.title, post %></h2>
        <p><%= post.created_at.strftime("%B, %d, %Y") %></p>
    </div>
<% end %>
```

Go ahead and commit have you have with git.


<div class="message">
  Hi, this was originally written and posted on that <a href="https://nameless-dusk-8821.herokuapp.com/">very blog's site</a> which is still being hosted up via Heroku. 
</div>