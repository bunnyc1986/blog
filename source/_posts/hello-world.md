title: Hello World
date: 2015/10/30
categories:
- Web
tags:
- Heroku
- Node.js
---
I have been planning to start a blog for a very long time (about 2 years). The first option is to use well known Wordpress, but I am not a PHP expert, and I don't want to setup a database somewhere. Then I start to think about building a blog engine in Node.js with Express, then project paused mostly because I am lazy and could not focus on the blog project. :P

Untill recently, I start to work on a hardward project and found many trick is interesting to be shared. I finally get this blog started and registered a new domain for it. So, this time I am serious about it. I will share my diccovery through the project and daily work. And also, I might put on some interesting personal story too. :)

So today, my first post is about how was this blog setup.

# Thank you, Hexo

[Hexo](https://hexo.io/) is a very light weight blog engine written in Node.js. I like it because it works perfectlly with a simple hosting service like [Heroku](https://www.heroku.com/), and not database is needed. There are many pre-created themes to choose from. The writing and posting are executed from command line tool. The posts are just files on file system. So if you use service like heroku, or other source control service, you will have the full commit log as your activity log. The styling and formatting is very simplified and unified, just [Markdown](https://en.wikipedia.org/wiki/Markdown). Basically, it has every thing I want to use on my blog.

## Get started!

### Create a new Heroku application

If you don't have a Heroku account, create one. It's free to [sign up](https://signup.heroku.com/login). Then the second step is to be familiar with its Node.js development process. Heroku provides very initiative step-by-step [guide](https://devcenter.heroku.com/articles/getting-started-with-nodejs#introduction).

You could use the Heroku command line tool to create the new app. I did it on Web UI. Once the app is created. You need to link it to your local workspace.

``` bash
$ heroku login
$ heroku git:clone -a <application name>
```

### Install Hexo

Installing Hexo is very simple task. The detailed [install guide](https://hexo.io/docs/index.html) is on its website. Here, I just layout some key steps:

* Install Node.js, see details on [its website](https://nodejs.org).
* Install Hexo command line tool: (You might need root permission.)
  ``` bash
  $ npm install -g hexo-cli
  ```
* Create the Hexo application into the application folder
  ``` bash
  $ hexo init <application name>
  $ cd <application name>
  $ npm install
  ```
* Update the \_config.yml file. The document is [here](https://hexo.io/docs/configuration.html). Most likely, you only need to modify couple things below for now:
  ```
  # Site
  title: <name of the blog>
  author: <your name>
  language: default
  timezone: <the timezone you are in>
  ```
  ```
  # URL
  ## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
  url: <your blog's url>
  root: /
  ```
* Now you are ready to test the blog using Hexo command
 ``` bash
 $ hexo server -p 8000
 ```
* Open your browser and visit [http://localhost:8000](http://localhost:8000). You should be able to see the blog site. Give yourself a tour!

### Deploy your blog to Heroku

If the site looks good to you. You can start deploying it to Heroku and make is public.

* First thing, you need to create a `Procfile` file as following:
  ``` bash
  web: hexo generate; hexo server -p $PORT
  ```
  Heroku doesn't allow you to use random port number. `$PORT` is the assigned port number. Don't worry, it will be mapped to port 80 externally.
* One last time check. Run the app locally through the Heroku command:
  ``` bash
  $ heroku local web
  ```
  Then you will find a message:
  ```
  web.1  | INFO  Hexo is running at http://0.0.0.0:5000/. Press Ctrl+C to stop.
  ```
  `5000` is a usual test port number assigned to `$PORT`. Now double check the site.
* Commit all the files. Hexo has already created the `.gitignore` file for you. Mine looks like:
  ```
  .DS_Store
  Thumbs.db
  db.json
  *.log
  node_modules/
  public/
  ```
  If it looks good to you, commit and push all the files.
  ``` bash
  $ git add .
  $ git commit -am "initial commit"
  $ git push heroku master
  ```
  Once the files are pushed, you will see message indicating node application detected. And the message shows you Heroku is deploying the code.
* Go to your Heroku app settings. Find and visit the 'Heroku Domain'. Your blog should be live now. Woohoo~!

### Write a post

Unlike other blog apps, Hexo uses command line to create static files for posts, pages and so on. For example:
``` bash
$ hexo new post "My first post"
```
Then you can edit the created file with Markdown. I personally recommend [Atom](https://atom.io) for editor. It provides Markdown helper and preview plugins. And it's great for other files too like javascript, css, node.js e.g.

If you have doubts and questions, you will find help at [Hexo document](https://hexo.io/docs/writing.html). Or you could also reference my posts on [Github](https://raw.githubusercontent.com/bunnyc1986/blog/master/source/_posts/hello-world.md).

When you've done editing the new post. Just commit and push the change. Heroku will automatically deploy the new post.
