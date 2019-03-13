---
layout: post
title:  "Deploy Jekyll with Mina on DigitalOcean"
date:   2019-03-13 13:51:26
categories: tools deploy
tags: fedora linux deploy mina digital-ocean
image: /assets/article_images/2019-02-13-asdf/haven1-i_see_stars-01-1024x768.png
---

Let's deploy [Jekyll](http://jekyllrb.com) blog on [DigitalOcean](https://m.do.co/c/51b036646dd1) or any other dedicated server.
> My favorite distro is [Fedora](https://getfedora.org/), so all example will work only on Fedora/CentOS.

---
### Mina
The simplest way to use [Mina](https://github.com/mina-deploy/mina) gem.

Add mina to your **Gemfile** and run ```bundle install```

{% highlight ruby %}
gem 'mina'
{% endhighlight %}

Init configuration with command ```mina init```

Open created file ```config/deploy.rb``` in your favorite editor and edit.

{% highlight ruby %}
require 'mina/rails'
require 'mina/git'

set :application_name, 'YOUR_BLOG_NAME'
set :domain, 'YOUR_DOMAIN_IP'
set :deploy_to, '/var/www/YOUR_BLOG_NAME'
set :repository, 'git@github.com:USER/REPOSITORY_NAME.git'
set :branch, 'master'

set :user, 'root'
set :term_mode, nil
set :forward_agent, true

# Using with Asdf
task :remote_environment do
  comment 'Initialize'
  command %{source ~/.asdf/asdf.sh}
  command %{export PATH=$HOME/.asdf/bin:$HOME/.asdf/shims:$PATH}
end

task :setup do
  # command %{rbenv install 2.3.0 --skip-existing}
end

desc "Deploys the current version to the server."
task :deploy do
  deploy do
    invoke :'git:clone'
    invoke :'deploy:link_shared_paths'
    invoke :'bundle:install'
    invoke :'deploy:cleanup'

    on :launch do
      in_path(fetch(:current_path)) do
        command %{mkdir -p tmp/}
        command "bundle exec jekyll build"
      end
    end
  end
end
{% endhighlight %}


> Don't forget add your public ssh key to github.com

Now setup project structure and deploy:
{% highlight bash %}
# Creates structure on server
$ mina setup

# Deploys
$ mina deploy
{% endhighlight %}

---

### Nginx

Create config file ```/etc/nginx/conf.d/my_blog.conf``` and set blog root:

{% highlight bash %}
server {
    listen       PORT;
    server_name  HOSTNAME;

    # Set root to your blog current _site version
    root         /var/www/my_blog/current/_site;


    location / {
    }

    error_page 404 /404.html;
        location = /40x.html {
    }

    error_page 500 502 503 504 /50x.html;
        location = /50x.html {
    }
}

{% endhighlight %}

Then restart Nginx ```systemclt restart nginx```
