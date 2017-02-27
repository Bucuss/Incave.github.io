---
layout: post
cover: 'assets/images/cover1.jpg'
title: Github Page Begin
date: 2017-02-26 20:55:00
navigation: True
tags: inCode
subclass: 'post tag-inCode'
categories: 'Bucuss'
logo: 'assets/images/ghost.png'
---

---

###懒癌晚期

在经历了超长时间的拖延后，这个星期终于把这个 *github page* 给部署好了。弄这个 *Blog* 的初衷是学习一下 *git* 和 *github* 的用法以及顺手发一些拙劣的文章 >3< ,趁今天刚刚部署好对一些奇奇怪怪的问题还比较深刻，赶紧记一波笔记。

要利用 *github* 来搭建一个 *Blog* 需要先了解一下 *git* ，*github* 还有一些 *jekyll* 的知识。对于git的知识可以从[Git](https://git-scm.com/book/zh/v2)这里学习，是很值得去反复学习的官方文档.jekyll的资料可以在[jekyllcn](http://jekyllcn.com/docs/home/)了解.github的使用就可以在实战中慢慢学会。这次 *Blog* 的搭建是基于 *github* 的 *github page* 功能。在[Github-page](https://pages.github.com/)有简单的入门使用方法，但我要弄一个比较符合我审美的 *Blog* 。所以依靠强大的搜索引擎慢慢摸索.了解到可以利用模板来制作自己的页面。对于对 *Ruby* 还一脸懵逼的我来说，这是个捷径。

###大麻烦

本以为省去了繁杂的基础代码的 *coding* 会很快搞定，但是配置文件的设置纠结了我好久。主要是这3个：

>* _config.yml
* RakeFile
* .travis.yml

---

* 第一个大头 *_config.yml* 是jekyll的主要配置文件，网站的名字和html文件里的一些引用的预设置都在这个文件。

		# Website info

		name: '栗'
		description: "记录文字 ，分享生活"

		# edit baseurl to simply '/' if using as your personal page
		# (instead of a project page)

		baseurl: /Incave.github.io/

		# Settings for deploy rake task
		# Username and repo of Github repo, e.g.
		# https://github.com/USERNAME/REPO.git
		# username defaults to ENV['GIT_NAME'] used by Travis
		# repo defaults to USERNAME.github.io
		# Branch defaults to "source" for USERNAME.github.io
		# or "master" otherwise

		username: Bucuss
		repo: Incave.github.io
		branch: master
		relative_source: ../Incave.github.io/
		destination: ../Incave.github.io-page/

	以上是本 *Blog* 的一些设置，其中 *baseurl* 在本地预览时可以使用'/'但是部署在 *github*  上就会出错，得设置成项目名的目录下。*username* 和 *repo* 的设置是以后通过 *travis ic* 来 *build* 是要使用的，*repo* 是项目名（ 一开始不懂瞎 *JB* 起名 *TAT* ）。

* 第二个 *Rakefile* 其实下载好模板后使用不用修改，只是我在还没弄明白 *Rake* 是干嘛的时在哪里瞎操作（蠢哭）。
我的理解是 *rakefile* 相当于 *travis* 在 *build* 时执行的脚本。

* 第三个 *.travis.yml* 花了我的时间最长（明明是最少的一个文件，我愣是搞了一下午）。由于 *github page* 不支持这个模板的 *jekyll* 插件，显示不了 *tag* 的页面，一直 404。寻求原作者的[文档](https://github.com/biomadeira/jasper)得知可以通过第三方的 *travis_ic* 来 *build* 好页面再重新 *push* 到 *gh-pages* 分支上，在分支上部署 *github page* ，这样就可以曲线救国。

	其实还有折中的办法就是每次 *push* 本地 *build* 好的页面( *XX-page* )到 *github* 上，不过比较麻烦。(注意：新建分支时 *gh-pages* 的 *s* 不要少了，不要问我为什么知道 QAQ )

		# Generate your secure token with the travis gem
		# get Github token from your Travis CI profile page
		# gem install travis
		# GH_TOKEN from https://github.com/settings/tokens
		# travis encrypt
		# GIT_NAME="USERNAME" GIT_EMAIL="EMAIL" GH_TOKEN="TOKEN"


		env:
		  	global:
		    	secure: XXX...
		branches:
		  	only:
		    - master

	首先要在[github_setting](https://github.com/settings/tokens)设置自己的 *token* (类似密钥),生成的 *token* 只有第一次可以看到，需要的话要记下来。然后安装好 *travis* 使用 *travis* 的命令设置 *GIT_NAME* , *GIT_EMAIL* , *GH_TOKEN* 这3个参数，*secure key* 是获得的 *token* 在 *travis* 加密生成的 *key* 。

		# 安装 Travis CI 命令行工具
		$gem install travis

		#设置参数
		$travis env set [argname] [value]

		$travis env set GIT_NAME bucuss
		...

		#加密获得密钥
		$travis encrypt GH_TOKEN=YOUR_TOKEN

	一开始在原作者的文档的指导下理解不能，不知道得自己在 *github* 上设置自己的 *token* ，后来在[使用 Travis CI 自动更新 GitHub Pages](http://notes.iissnan.com/2016/publishing-github-pages-with-travis-ci/) 这里比较清晰的了解了过程。(毕竟英语苦手，还是得多学习)，一切设置好后可以在自己的 *Travis* 里 *build* 一下，成功则是绿色的 *pass* ，页面就应该 *ok* 了。

---

###后记

这里还有一些不解，就是 *Travis* 提供的 *token* 在哪里使用，一开始误以为是这个 *token*，结果一直 *build* 失败，以后再在好好查查。墙内环境的影响，导致 *github* 的 *push* 和 *pull* 都极其缓慢，这里可以通过更改 *host* 缓解：

		151.101.72.249 global-ssl.fastly.Net
		192.30.253.112 github.com

>上記の









