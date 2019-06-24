---
layout: post
title: 用Jekyll搭建的Github Pages个人博客
date: 2016-04-16 11:11:11.000000000 +09:00
categories: Jekyll Github
tags: Jekyll&emsp;Github

---

&emsp;&emsp;近半年一直忙于项目开发，不烙得空。近期慢慢清闲了，终于有机会可以写写属于自己的技术博客。

&emsp;&emsp;之前有打算在[CSDN](http://blog.csdn.net)、[cnblogs](http://www.cnblogs.com)等博客平台上写的，不过个人觉得界面设计、页面效果比较low，不符合我的审美观<(￣▽￣)>。也有考虑过在[简书](http://www.jianshu.com)上面写，因为界面看着简约大方上档次的赶脚，平时也看到挺多简书里的技术文章。不过，简书首页写着“一个基于内容分析的社区”，也就是啥啥啥文章都有，不只是技术博客，额，给我感觉在上面写自己的技术博客不是很正宗（~~No zuo，No die~~）。依稀记得之前访问[喵神](http://onevcat.com)的博客很有feel，如果能做一个他这样的个人博客网站就好了，于是就有了不拉不拉不拉的一通乱逛、乱转、乱撞，终于整出属于[自己的博客](http://joe-liu.com)了，不过在这里要感谢喵神git上的提供的主题[Vno-Jekyll](https://github.com/onevcat/vno-jekyll)。

&emsp;&emsp;好了，其他废话不多说，下面就说一下搭建过程，然后你也可以拥有和我一模一样的博客了。

**达到效果**：

* 1.自定义域名访问技术博客（joe-liu.com）
* 2.Github二级域名访问博客（joe-liu.github.io）


###一、Github Pages
&emsp;&emsp;GitHub Pages 可以为你或者你的项目提供介绍网页，它是由 GitHub 官方托管和发布的。你可以使用 GitHub 提供的页面自动生成器。也可以做个人博客，是个轻量级的博客系统，没有麻烦的配置。使用标记语言如Markdown，不需自己搭建服务器，还可以绑定自己的域名。

> [Github Pages - 官方配置指南](https://help.github.com/categories/github-pages-basics/)  
> [Github Pages - 自定义页面指南](https://help.github.com/categories/customizing-github-pages/)  
> [极客学院翻译 - 中文版本指南](http://wiki.jikexueyuan.com/project/github-pages-basics/)

###二、Github二级域名创建
&emsp;&emsp;这个步骤比较简单，[Github Pages](https://pages.github.com)官网首页就有图文说明。

* 1.打开创建代码仓库页面
![repo1 icon](/assets/2016-04-16-used-jekyll-to-create-my-github-blog/7515e75bjw1f2xvg9vqjfj214w0v0dmt.jpg)
&emsp;&emsp;在红框的位置输入->`(你的用户名).github.io`  点击`Create repository`。（用户名就是红框左边，这里千万要记住大小写，不然前方有坑等着）

* 2.下载github的[客户端](https://desktop.github.com)

* 3.将刚刚创建的代码仓库，克隆一份到电脑本地
![repo2 icon](/assets/2016-04-16-used-jekyll-to-create-my-github-blog/7515e75bjw1f2xw5d4a4aj20m805omya.jpg)

* 4.创建一个简单的Hello World静态页面
&emsp;&emsp;用终端在目录下创建`index.html`文件，输入文件内容：

```ojb
<!DOCTYPE html>
<html>
<body>
<h1>Hello World</h1>
<p>I'm hosted with GitHub Pages.</p>
</body>
</html>
```
![repo3 icon](/assets/2016-04-16-used-jekyll-to-create-my-github-blog/7515e75bjw1f2xwi1alwfj208c03fdfv.jpg)


* 5.将`index.html`文件上传到github上
![repo3 icon](/assets/2016-04-16-used-jekyll-to-create-my-github-blog/7515e75bjw1f2xwrym2soj21ak0ycgrx.jpg
)

* 6.浏览器输入`(你的用户名).github.io`，就初步看到效果啦
![repo4 icon](/assets/2016-04-16-used-jekyll-to-create-my-github-blog/7515e75bjw1f2xwmlhawmj20qu0a4wfe.jpg)

###三、Jekyll本地环境搭建（待完善）
> [Jekyll中文指导手册](http://jekyllcn.com)

&emsp;&emsp;GitHub Pages为了提供对HTML内容的支持，选择了[Jekyll](https://github.com/jekyll/jekyll)作为模板系统，Jekyll是一个强大的静态模板系统，作为个人博客使用，基本上可以满足要求，也能保持管理的方便。  

&emsp;&emsp;Jekyll是一种简单的、适用于博客的、静态网站生成引擎。它使用一个模板目录作为网站布局的基础框架，支持Markdown、Textile等标记语言的解析，提供了模板、变量、插件等功能，最终生成一个完整的静态Web站点。说白了就是，只要安装Jekyll的规范和结构，不用写html，就可以生成网站。

* Jekyll基本结构

&emsp;&emsp;Jekyll的核心其实就是一个文本的转换引擎，用你最喜欢的标记语言写文档，可以是Markdown、Textile或者HTML等等，再通过layout将文档拼装起来，根据你设置的URL规则来展现，这些都是通过严格的配置文件来定义，最终的产出就是web页面。

```
|-- _config.yml
|-- _includes
|-- _layouts
|   |-- default.html
|    -- post.html
|-- _posts
|   |-- 2007-10-29-why-every-programmer-should-play-nethack.textile
|    -- 2009-04-26-barcamp-boston-4-roundup.textile
|-- _site
 -- index.html
```

* 将主题下载到本地[Vno-Jekyll](https://github.com/onevcat/vno-jekyll)，解压到刚刚的代码仓库目录下，可以把文件夹里的文件都删了。

* 安装本地`Jekyll`环境步骤：

	1.安装`ruby`（去百度一下，有很多教程）

	2.打开终端，执行`sudo gem install jekyll`（用vpn比较快点）
	
	如果是在淘宝的镜像，可能找不到`Jekyll`
	
	```obj
	ERROR:  Could not find a valid gem 'jekyll' (>= 0), here is why:
          Unable to download data from http://ruby.taobao.org/ - bad response Not Found 404 (http://ruby.taobao.org/latest_specs.4.8.gz)
	```
	
	更换镜像：  
	`gem source -r https://ruby.taobao.org/`  (移除淘宝镜像)  
	`gem source -a https://rubygems.org/`  (添加新镜像)
	
	查看当前镜像：`gem source`，出现下面输出代表更换成功
	
	```
	*** CURRENT SOURCES ***
	https://rubygems.org/
	```
	
	再次执行`sudo gem install jekyll`，输入电脑密码，等待安装一会。
	
	3.进入到你刚刚下载的Jekyll主体文件目录`cd /Users/Joe/Documents/git/Joe-Liuyi.github.io`
	
	4.执行`bundle install`
	如果出现提示：
	
	```
	-bash: bundle: command not found
	```
	就先安装bundle（执行`sudo gem install bundle`），再执行此命令，见一堆绿色打印的时候说明执行命令成功。
	
	5.开启Jekyll环境`bundle exec jekyll serve`，看见下面输出代表开启成功。

	```
	liuyideMacBook-Pro:Joe-Liuyi.github.io Joe$ bundle exec jekyll serve
Configuration file: /Users/Joe/Documents/git/Joe-Liuyi.github.io/_config.yml
            Source: /Users/Joe/Documents/git/Joe-Liuyi.github.io
       Destination: /Users/Joe/Documents/git/Joe-Liuyi.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
                    done in 0.528 seconds.
 Auto-regeneration: enabled for '/Users/Joe/Documents/git/Joe-Liuyi.github.io'
Configuration file: /Users/Joe/Documents/git/Joe-Liuyi.github.io/_config.yml
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
	```
	
	6.在浏览器输入`http://127.0.0.1:4000/`，即可看见刚刚从网上下载的`vno-jekyll`主体技术博客了
	
	7.用git电脑终端将这些代码都上传到git代码仓库。浏览器输入`(你的用户名).github.io`，就可以看到你的成效了。
	
	

###四、绑定个人域名
* **1.创建CNAME文件**

	> 在仓库根目录的`master`分支上创建文件`CNAME`，不带后缀。并将不带协议名的裸域名写进去(`joe-liu.com`，而不是`http://joe-liu.com/`)  
	> 如果看到绿色打钩，说明配置文件成功了。  
	这一步也可以参考[官方文档](https://help.github.com/articles/setting-up-your-pages-site-repository/)
	
![cname1 icon](/assets/2016-04-16-used-jekyll-to-create-my-github-blog/7515e75bgw1f2yidhzl6aj205007njro.jpg)

![cname2 icon](/assets/2016-04-16-used-jekyll-to-create-my-github-blog/7515e75bgw1f2yidihvu6j20go0bujrz.jpg)

![cname3 icon](/assets/2016-04-16-used-jekyll-to-create-my-github-blog/7515e75bjw1f2yj0czpekj20dw02s0sw.jpg)

![cname4 icon](/assets/2016-04-16-used-jekyll-to-create-my-github-blog/7515e75bjw1f2yj3rkqgxj20go06xdgn.jpg)

&emsp;&emsp;&emsp;&emsp;好吧， 你直接在代码仓库里点击`New file`也很快

![cname4 icon](/assets/2016-04-16-used-jekyll-to-create-my-github-blog/7515e75bjw1f2yjq1j0k9j20b405t3z5.jpg)



* **2.购买域名**

	> 我是在[万网](https://wanwang.aliyun.com)上购买的域名，域名不一定要和github用户名一样。
	
* **3.添加A记录**

  	> 购买完，进入`管理控制台` -> `云解析` -> `解析` -> `添加解析` -> 添加`A`记录 :  
	`192.30.252.153`  
	`192.30.252.154`
	
![domain icon](/assets/2016-04-16-used-jekyll-to-create-my-github-blog/7515e75bjw1f2yim1c737j21d6068768.jpg)

* 4.等待DNS解析生效

	> 大概十分钟内吧，奇葩情况可能要0-72小时都有可能。  
	可以在终端输入：`dig joe-liu.com +nostats +nocomments +nocmd`，查看解析生效没。如果看到下面两条记录说明就解析好了。快输入你的域名看看效果吧。

```
liuyideMacBook-Pro:~ Joe$ dig joe-liu.com +nostats +nocomments +nocmd
; <<>> DiG 9.8.3-P1 <<>> joe-liu.com +nostats +nocomments +nocmd
;; global options: +cmd
;joe-liu.com.			IN	A
joe-liu.com.		355	IN	A	192.30.252.153
joe-liu.com.		355	IN	A	192.30.252.154
joe-liu.com.		20	IN	NS	f1g1ns2.dnspod.net.
joe-liu.com.		20	IN	NS	f1g1ns1.dnspod.net.
```

###五、评论功能设置
1. 登录[Disqus](https://disqus.com)网站注册一个账号（开vpn比较快点）
2. 点击Setting`图标`，选择`Add disqus to site`
3. 点击`Start Using Engage`
4. 设置自己的Disqus的URL
![disqus icon](/assets/2016-04-16-used-jekyll-to-create-my-github-blog/7515e75bjw1f2zxj6kl8cj20go0a4756.jpg)
5. 设置根目录下`_config.yml`文件的disqus的URL

```obj
# Comment
comment:
    disqus: joeliu
```

###六、主题细节修改
&emsp;&emsp;一切顺利的话，你讲看到下面这个界面。接下来就将网站信息都改成自己的吧。
![background icon](/assets/2016-04-16-used-jekyll-to-create-my-github-blog/7515e75bjw1f2yk2dvkomj20jg09xmz0.jpg)

* **1.修改个人信息**  
&emsp;&emsp;博客名、描述、跳转链接的修改的主要文件路径是在根目录下的文件：`_config.yml`

* **2.修改背景图片**  
&emsp;&emsp;头像和背景存放路径：`代码仓库根目录`->`assets`->`image`，直接替换文件就好了，不过要保持文件名一样。
![guide icon](/assets/2016-04-16-used-jekyll-to-create-my-github-blog/7515e75bjw1f2yk5epypnj20m807ojsm.jpg)



###七、写文章
* **1.存放文章位置**
![article icon](/assets/2016-04-16-used-jekyll-to-create-my-github-blog/7515e75bjw1f2ydn81e45j212a0jq0wr.jpg)

&emsp;&emsp;用Jekyll写博客，一篇文章就是一个文件，所有需要发表的文章都要放在`_posts`文件夹里。文件的命名要按`YYYY-MM-DD-文章标题.markdown`这种格式来，后缀使用`.md`也可以。文件名一旦确定下来，就不要轻易更改，因为Jekyll结合的评论功能，会根据文件名去查找这篇文章的评论内容。同一篇文章只是更改了文件名，如下：
![article icon](/assets/2016-04-16-used-jekyll-to-create-my-github-blog/7515e75bjw1f2yecz7lggj20xc0eqq78.jpg)

* **2.写文章工具**

&emsp;&emsp;推荐用[Mou](http://25.io/mou/)吧

* **3.书写格式**

&emsp;&emsp;在文章的开头，我们需要先设置头信息。头信息需要根据[YAML](http://yaml.org/)的格式写在两行三虚线之间。

```obj
---
layout: post
title: 这个是标题
date: 2016-04-16 11:11:11.000000000 +09:00
tags: Jekyll Github
---
```

> `layout`：指文章布局的类型，需要使用指定的模版文件，模版文件放在`_layouts`目录下，暂时用`post.html`。`page.html`模板比`post.html`模板少了`更早的文章`和`评论模块`。有能力的也可以自己写一个文章页面的布局模板文件。  
> `title`：文章的标题。  
> `date`：发布文章的时间。（后面的一串零零零好像不能省）  
> `tags`：标签，一篇文章可以设置多个标签，使用空格分割。   

&emsp;&emsp;基本上一篇文章只要用到以上一些信息就可以了，当然还有其它的变量可以设置，具体用法可以在[Jekyll网站](http://jekyllrb.com/docs/frontmatter/)上查看。

&emsp;&emsp;终于可以开始写文章了，一劳永逸，以后再也不用关心那些破事了，将心思放在写文章上。

&emsp;&emsp;为了写出漂亮排版的文章，需要多注意下Markdown的语法说明，第一次运行`Mou`软件的时候，会有弹出使用语法说明，如果不小心关了，也可以在软件的菜单栏里找到 `Help -> Mou Help` :
![mou icon](/assets/2016-04-16-used-jekyll-to-create-my-github-blog/7515e75bjw1f2yfgbcdwej20m80ettby.jpg)

&emsp;&emsp;在写文章的时候，在终端执行`bundle exec jekyll serve`，开启Jekyll本地环境，可以一边写博文，一边刷新`http://127.0.0.1:4000/`地址查看实时效果。写完提交git，就完事啦。


