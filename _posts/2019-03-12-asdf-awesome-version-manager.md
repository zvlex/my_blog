---
layout: post
title:  "Asdf - awesome version manager for multiple runtime envirotments"
date:   2019-02-12 21:13:26
categories: asdf ruby golang elixir
tags: none
image: /assets/article_images/2019-02-13-asdf/haven1-i_see_stars-01-1024x768.png
---

Real world programming you have many projects with different programming language versions. While managing this zoo, you use tools like ```rbenv```, ```rvm```, ```nvm```, ```gvm``` and etc. You spend hours for setup them. This is boring and not efficient...

> And what the solution? Asdf!

[**Asdf**](https://asdf-vm.com/) - is version manager for multiple runtime envirotments.

CLI tool supports a lot of plugins for installing different versions of programming languages, tools and databases. It also supports existing config files such like ```.node-version```, ```.nvmrc```, ```.ruby-version```. Just append ```legacy_version_file = yes``` to ```~/.asdfrc``` file. Automatically switches runtime versions and many many other cool things.
You can see full list of plugins [here](https://asdf-vm.com/#/plugins-all?id=plugin-list).

### Setup
{% highlight bash %}
git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch v0.7.0

### For zsh
echo -e '\n. $HOME/.asdf/asdf.sh' >> ~/.zshrc
echo -e '\n. $HOME/.asdf/completions/asdf.bash' >> ~/.zshrc

### For bash
echo -e '\n. $HOME/.asdf/asdf.sh' >> ~/.bashrc
echo -e '\n. $HOME/.asdf/completions/asdf.bash' >> ~/.bashrc

### For Fish
echo 'source ~/.asdf/asdf.fish' >> ~/.config/fish/config.fish
mkdir -p ~/.config/fish/completions; and cp ~/.asdf/completions/asdf.fish ~/.config/fish/completions
{% endhighlight %}


### Install Ruby and Golang:

{% highlight bash %}
### Add Ruby plugin
asdf plugin-add ruby

### Add Golang plugin
asdf plugin-add golang

### Install Ruby 2.6.1 and Golang 1.12 versions
asdf install ruby 2.6.1
asdf install golang 1.12

### Use as global in system
asdf global ruby 2.6.1
asdf global golang 1.12

### Or you can use as local version only in current folder
### It creates .tool-versions file with "ruby 2.6.1" and "golang 1.12" version
asdf local ruby 2.6.1
asdf local golang 1.12

{% endhighlight %}

---
