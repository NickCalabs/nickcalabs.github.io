---
layout: post
categories: Rails
author: Nick Calabro
image: railswelcome.jpg
excerpt: "Now your blog is created. You're writing posts, you're sharing your work, but wait - it's not live on the web! Until now."
---

<meta name="twitter:card" content="summary" />
<meta name="twitter:site" content="@NickCalabs" />
<meta name="twitter:title" content="{{ page.title }}" />
<meta name="twitter:description" content="Nick Calabro's Blog" />

<div class="message">This is the final part of the six part series covering, in detail, how to <a href="http://nameless-dusk-8821.herokuapp.com">create this blog with Ruby on Rails</a> from backend to frontend. 
</div>

##Heroku Create

Deploying your app can be the hardest part. I went through many different attempts to try and get this app running as smooth as I can, since I wasn’t prepared to handle sysadmin stuff at this point in time, I ended up making it a separate little app on Heroku instead.

Heroku makes things so effortless anyone can have an app running on the live web in a matter of minutes. There are a couple tiny adjustments we have to make in our app first. Heroku and SQLite don’t play nicely together, it prefers Postgresql. In your Gemfile, comment out the sqlite3 gem and at the bottom of the file add this:

```ruby
group :production do
    gem 'pg'
end
```

Be sure to commit this, and `bundle install`. There, now your app is ready to run on Heroku’s servers. For the next part, just make a free account over at <a href="https://dashboard.heroku.com/">Heroku</a> and follow their instructions for getting the tool belt installed. Once you’ve done that, run `heroku create`. If you refresh your dashboard on Heroku you’ll see a new app appear. At this point, you can try running `heroku open` which will load your app live in your browser, however, nothing is really there yet except a little note assuring us everything is okay. We still have to run `git push heroku master` - this will actually push your app onto the server, it might take a while. Finally, we run a migration through heroku with `heroku run rake db:migrate`.

heroku open and you should see your site live! Share the link with pride. Go back to your dashboard and connect it with the Github repo so when you push changes Heroku can detect them straight from there.

Now we’re going to get a little hack-y. I’m not sure of the most efficient way to go about this, if I’m doing this totally wrong, well, at least I had a good time. Go to your app users/sign_up and create an account. Don’t forget these credentials and make sure they’re fairly secure. git clone your app again or however you have it set up at the moment and navigate to the `app/models/user.rb` file and just remove the `:registerable` tag. This will allow no one to make an account on your blog and start messing with your posts.

Commit it, deploy the latest branch in the Heroku dashboard, and you should be fully set to go.

<div class="message">
  If you liked this or found it useful please let me know. You can check out the code on my <a href="http://github.com/nickcalabs">GitHub</a> and I'd love to connect on <a href="http://twitter.com/nickcalabs">Twitter</a>, <a href="http://linkedin.com/in/nickcalabro">Linkedin</a>, or just chatting in <a href="mailto:calabro.nick@gmail.com">emails</a>.
</div>