---
layout: post
tags:
- jekyll
- code
categories: Rails
author: Nick Calabro
image: railswelcome.jpg
---

<meta name="twitter:card" content="summary" />
<meta name="twitter:site" content="@NickCalabs" />
<meta name="twitter:title" content="{{ page.title }}" />
<meta name="twitter:description" content="Nick Calabro's Blog" />

##Styling

Our app have very little functionality - which is good. At least it has some functionality. Before we continue, though, I think we should add some styling so we’re not looking at any gross basic HTML structuring. The first thing we’re going to do here is add Bootstrap. In your terminal, create a new branch. This way, if we screw something up, we can always revert back to where we are now and start breathing at a normal rate again.

```console
git co -b style
```

Here, `co` is checkout, `-b` is a shortcut to switch directly to that branch, and `style` is the name of the branch. Now if you enter `git branch`, you’ll see the master branch, and that we’re on the style branch. Now since we’re safe, let’s get stylish. In your Gemfile, near the middle, before the `group :development, :test do`, add:

```ruby
#bootstrap
gem 'therubyracer'
gem 'less-rails'
gem 'twitter-bootstrap-rails', '~> 3.2.1.rc1'
```

You can follow along with how I went about this in the <a href="https://github.com/seyhunak/twitter-bootstrap-rails">Twitter Bootstrap Rails</a> github repo, by the way. Now give your terminal a little `rails g bootstrap:install less`; this will install the proper stylesheets and whatnot. Then, `rails g bootstrap:layout` will create the `application.html.erb`. It’ll probably ask you if you’re sure you want to overwrite your current file, just confirm that you do. Now if you reload your browser you’ll see a much more pleasing site - still showing our posts, and now with a sidebar as well. For the site you’re currently viewing, I got rid of the sidebar. I’m sure you can figure out how to cut that out, if not, here’s the snippet to delete. You can even play around with some of <a href="http://getbootstrap.com/">Bootstrap</a>’s features and make an even cooler sidebar.

```html
<div class="col-lg-3">
  <div class="well sidebar-nav">
    <h3>Sidebar</h3>
    <ul class="nav nav-list">
      <li class="nav-header">Sidebar</li>
      <li><%= link_to "Link1", "/path1"  %></li>
      <li><%= link_to "Link2", "/path2"  %></li>
      <li><%= link_to "Link3", "/path3"  %></li>
    </ul>
  </div><!--/.well -->
</div><!--/span-->
```

##Navbar

For the sake of making what we have a little more presentable and to give us some instant gratification we’re going to add some Bootstrap markup to this. Right now, we see Bootstrap generated a nice nav bar for us. It’s cute, but it’s not what I have here, and we’re building this very site. To achieve the transparent nav with links floating on the right while maintaining a nice dropdown toggle for mobile I had to do a few things. First fix your `navbar-brand`, because that’s probably not how you want that displayed. I did it like this:

```html
<h1><%= link_to "Nick Calabro", root_path, class: "navbar-brand" %></h1>
```

This way, we’re able to put it in an `h1` tag, and it’s also still a link with the bootstrap class `navbar-brand` that keeps it in place. There are also some divs we want to throw in there, and we’re going to do some magic to get links to active or not if you’re on the corresponding page. When it’s all said and done, it’ll look like this.

```html
<div class="navbar">
  <div class="container">
    <div class="navbar-header page-scroll">
      <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-responsive-collapse">
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
      <h1><%= link_to "Nick Calabro", root_path, class: "navbar-brand" %></h1>
    </div>
    <div class="navbar-collapse collapse navbar-responsive-collapse">
      <ul class="nav navbar-nav navbar-right">
        <li class="<%= 'active' if current_page?(root_path) %>"><%= link_to "Articles", root_path  %></li>
      </ul>
    </div>
  </div>
</div>
```

In your `ul`, we’re embedding some ruby to only add the active class if the current page is that `li`’s path. Pretty cool, if you ask me - right, you didn’t ask me. Another way we could’ve done this looks something like this `<li class="<%= 'active' if params[:controller] == 'root' %>"><%= link_to “Articles", root_path  %></li>`. On to terminal.

```console
git add status
git add .
git commit -m “Add Bootstrap"

git co master
git merge style
git branch -d style
```

Now that you have something that looks pretty, we can commit these changes, merge them, delete the style branch and add some crazy functionality in the next post.