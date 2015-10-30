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

## Thank you, Hexo

[Hexo](https://hexo.io/) is a very light weight blog engine written in Node.js. I like it because it works perfectlly with a simple hosting service like [Heroku](https://www.heroku.com/), and not database is needed. There are many pre-created themes to choose from. The writing and posting are executed from command line tool. The posts are just files on file system. So if you use service like heroku, or other source control service, you will have the full commit log as your acrivity log. The sytling and formating is very simplified and unified, just [Markdown](https://en.wikipedia.org/wiki/Markdown). Basically, it has every thing I want to use on my blog.

### Get started!

Installing Hexo is very simple task. The detailed [install guide](https://hexo.io/docs/index.html) is on its website. Here, I just layout some key steps:

* Install Node.js, see details on [its website](https://nodejs.org).
* Install Hexo command line tool.
  
  ''' bash
  $ npm install -g hexo-cli
  '''

