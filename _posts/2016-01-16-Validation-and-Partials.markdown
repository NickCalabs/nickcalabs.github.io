---
layout: post
categories: Rails
author: Nick Calabro
title: "Validation & Partials (Part III)"
image: railswelcome.jpg
excerpt: "Have you ever wanted to create one snippet of code and re-use it in different areas throughout your project? You need to learn how to integrate partials into your Rails app."
---

<meta name="twitter:card" content="summary" />
<meta name="twitter:site" content="@NickCalabs" />
<meta name="twitter:title" content="{{ page.title }}" />
<meta name="twitter:description" content="Nick Calabro's Blog" />

<div class="message">This is part three of the six part series covering, in detail, how to <a href="http://nameless-dusk-8821.herokuapp.com">create this blog with Ruby on Rails</a> from backend to frontend. 
</div>

##Validate

Right now, you can create a post with absolutely no content inside. In your `app/models/post.rb` file, we’re going to want to add some validations.

```ruby
class Post < ActiveRecord::Base
    validates :title, presence: true, length: { miniimum: 5 }
    validates :body, presence: true
end
```

It’ll look like this when you’re finished. It can be kind of self-explanatory, but all we’re doing it telling it to throw an error if the title isn’t present at all or has fewer than 5 characters. Same thing with the body minus the char count.

Next, we have to add some functionality to `posts_controller.rb`. In the `new` method add `@post = Post.new`. We’re also going to change up the create method by adding a little conditional. It should look like this.

```ruby
def create
    @post = Post.new(post_params)

    if @post.save
        redirect_to @post
    else
        render 'new'
    end
end
```

##Errors

Now, if you try to make a post at `localhost:3000/posts/new` and your title is not at least five characters, you won’t be able to post it. But, this isn’t very intuitive. Sure, you know about that rule now, but you might forget. A simple error message can really come in handy. So, near the form in `new.html.erb` add this right under the `form_for` and right before the first `<p>` tag:

```ruby
<% if @post.errors.any? %>
    <div id="error">
        <h2><%= pluralize(@post.errors.count, "error") %> prevented this post from saving</h2>
        <ul>
            <% @post.errors.full_messages.each do |msg| %>
                <li><%= msg %></li>
            <% end %>
        </ul>
    </div>
<% end %>
```

##Partial

While we’re here, cut everything from this file except the `h1` tag on top and create a new file in the same directory called `_form.html.erb`. This is a special type of file called a partial that’ll allow you to throw snippets like the form into. These types of coding practices will help keep our programs more DRY. Paste the whole form in there and just change the first line from `<%= form_for :post, url: posts_path do |f| %>` to `<%= form_for @post do |f| %>` - this will ensure our form is dynamic enough to be passed around the application. Now in `new.html.erb` just add `<%= render ‘form’ %>`. Go ahead and test out a new post to find nothing has changed on the front end.

I’m sure you know how to commit your work with git at this point.

<div class="message">
  If you liked this or found it useful please let me know. You can check out the code on my <a href="http://github.com/nickcalabs">GitHub</a> and I'd love to connect on <a href="http://twitter.com/nickcalabs">Twitter</a>, <a href="http://linkedin.com/in/nickcalabro">Linkedin</a>, or just chatting in <a href="mailto:calabro.nick@gmail.com">emails</a>.
</div>