---
title: Configure Blog with Hexo + Git + Duoshuo
category: Others
---

This is my first blog after switching to [Hexo](https://hexo.io/). After hours hard work I configurate it successfully. Now It's time to collect and record the steps and issues I met.

<!--more-->

## Setup
```
$ hexo init <folder>
$ cd <folder>
$ npm install 
```
**NOTE**   
* First in first, you need to create a repository in you github, whose name should be `xxx.github.io`, `xxx` is your username;

* First time you use Hexo, you need to install `hexo-deployer-git`, you can run the command below:  

 	```
 	npm install hexo-deployer-git --save
 	```

* Then config `deploy` in the _config file, after this, whenever you run `hexo deploy`, it will push the change to the github automatically:

```
deploy:
   type: git
   repo:     //this is path for your repo `xxx.github.io`
   branch: master
```

## Quick Start

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Run server
You can preview your generated posts locally after run local node server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)



### Deploy to remote sites

``` bash
$ hexo deploy
```
After you run the `deploy` command, the code will be merged to your deploy repo automatically. 

More info: [Deployment](https://hexo.io/docs/deployment.html)


## Setup Duoshuo
*  首先需要到[多说home page](http://duoshuo.com/)注册账号，目前只支持用微博，微信等账号等入，然后在home page 有个`我要安装`，点击后可以创建站点，其中有一个是多说域名，例如，http://abc.duoshuo.com， `abc`将是接下来要用到的`duoshuo_shortname`;
*  接下来按照[这里](http://dev.duoshuo.com/threads/541d3b2b40b5abcd2e4df0e9)配置即可。

