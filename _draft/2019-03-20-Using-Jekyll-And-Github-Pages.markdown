---
layout: post
title:  "Using Jekyll And Github.io To Build And Host A Blog"
date:   2019-03-20 23:00:00 +0800
author: XiongJJ
comments: true
categories: git web-technology
---

For some reason I started to write this blog since last week (03/13) while I have just a little-little-little bit of website building experience. So, I'm intended to build the blog using `markdown` only (for me it is much more easier than building it from ground using `html` + `css` ). And the best solution so far I have found on the internet is to use [Jekyll](https://jekyllrb.com/) to compile and build the website and host it on [Github Pages](https://pages.github.com/).

However, while it is easy to host your website on github as the git actions necessary were none other than **add, commit and push**, things don't go easily on Jekyll side. I didn't find any tutorial on the internet that was simple and concise enough, but tutorials that was written intently for complete beginners who don't write any code before, or tutorials written without any graphical examples and super long paragraph explaining a simple action which the user don't really need to know about.

After struggling with the tutorials described above, I decided to write an article to give a simple guide using Jekyll for other **intermediate user** who wants to start writing a blog/personal website but having few/without experience like me. This guide assumes you to already have the skills listed below:
+   familiar with git and github repo
+   familiar with markdown language
+   basic shell command(not really needed if you works in Windows)
+   basic package manager actions(JS npm, Python's pip...)

If you do not meet the requirements above, I'd suggest that you start from learning them separately. If you are a experienced programmer I believe they shouldn't be problems to you.

Note: this guide was written in Ubuntu, but OSes don't really matters here as you know how to install the necessary packages on different OS.

## Installation
First, install `git` if you haven't do so. `Jekyll` is a package(called ruby gems) written in Ruby, so run the commands below to install a full Ruby development environment and then Jekyll. You also need the gem `bundler` when building the site.

```
# On ubuntu
# Installing ruby development environment
$ sudo apt install ruby-full build-essential zlib1g-dev

# Then the gems
$ gem install jekyll bundler
```

## Build a simple website and host on github
+   Create a repo on github.com, name it with this convention `[github-username].github.io` and do not initialize it with anything else including README.<br>
	```
	By naming your repo with the domain name XXX.github.io, this repo will be set up for the github-pages to serve content in your master branch automatically.
	```
+   After creating a repo, get the remote repo URL.
+   Run commands below to create a new Jekyll blog instance, and push to the remove repository.
	```
	# create new blog with same name as your remote reposotory
	$ jekyll new [github-username].github.io
	$ cd [github-username].github.io

	# initialize the generated folder into a git repository
	$ git init
	$ git add .
	$ git commit -m "[commit message]"

	# add a remote 
	$ git remote add origin [remote repo URL]
	$ git remote -v

	# push to github
	$ git push origin master
	```

