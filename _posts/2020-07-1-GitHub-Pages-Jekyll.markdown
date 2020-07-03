---
layout: post
title:  "jekyll搭建GitHub Pages"
date:   2020-07-1 00:58:06 +0800
color: rgb(255,90,90)
cover: '../assets/test.png'
categories: GitHub Pages
tags: [GitHub Pages,Jekyll]
subtitle: '【MAC】从0开始建博客'
---
# 一、GitHub
## 1. GitHub账号注册
进入[https://github.com](https://github.com)，然后注册一个账户就可以了。
## 2. GitHub仓库建立
登录 GitHub 之后，在页面右上角点击 + 加号按钮，点击 New repository。
<img src="/assets/GitHub_Pages/1.png" width = "300" height = "280" alt="新建库" align=center />

仓库名是必须的，但是分两种：  
  * 普通仓库名  
&emsp;&emsp;随意就可以了，就像是文件名  
  * 个人网站项目  
&emsp;&emsp;如果是要新建一个个人网站项目，仓库的名称需要按照 GitHub 个人网站项目的规定来写。  
&emsp;&emsp;`YOUR-GITHUB-USERNAME.github.io`  
  
Description描述部分可写可不写，通常都是选Public公共模式，Public和Private的区别大家应该都知道。  
选择初始化会直接创建一个README文件，通常是用来介绍仓库具体内容，描述功能之类的作用，不初始化可以之后再自己创建。
点击创建仓库按钮。
![Alt text](/assets/GitHub_Pages/2.png "新建库")
创建成功之后，进入了项目主页面。点击设置按钮。
![Alt text](/assets/GitHub_Pages/3.png "新建库")
如果只是普通仓库「普通仓库也是可以有网页的」那么source可以选择master分支或者其他文件夹分支。  
如果是个人网站项目「每个账号只能有一个」那么source固定为master分支。
![Alt text](/assets/GitHub_Pages/4.png "新建库")
![Alt text](/assets/GitHub_Pages/5.png "新建库")
###### PS: 自设域名此处不探讨
这里是第一个官方教程：[GitHub:Hello world](https://guides.github.com/activities/hello-world/)。可以通过按钮来理解一个库的构造和维护，不过这只能让你创建修改一个Readme文件。  
这里你会意识到每个库是可以创建分支(branch)的，实际操作中，如果对版本要求没那么严格（做个网页而已），你可以忽略branch的存在，只用master一个默认branch即可。
## 3. GitHub本地仓库建立
#### &diams; 环境
检查Git `git --version`
![Alt text](/assets/GitHub_Pages/6.png "git")
没有就下载[Git for MAC](https://git-scm.com/downloads)
#### &diams; 创建、选择本地仓库路径
创建或者选择一个文件夹作为仓库 `cd xxx/xxx` 进入所选文件夹
#### &diams; 初始化
`git init`  
进入你的Mac上指定的目录下，进行查看有何变化，发现多了个.git的文件夹（默认是隐藏的）  
###### PS: [MAC下查看隐藏文件][hide-show]
#### &diams; 配置个人信息
配置你的姓名（告诉git上的其他用户你是谁？），命令如下：  
`git config user.name xxx`  
配置你的电子邮箱（告诉git上的其他用户你的联系方式是？），命令如下：  
`git config user.email xxx@xx.com`  
配置完成后可在.git/branches/config文件中查看到  
  
采用上面的命令进行配置是一次性的配置， 只会配置到被管理文件的.git文件夹下，要想一劳永逸，请使用下面的配置方式进行配置：  
`git config --global user.name xxx`  
`git config --global user.email xxx@xx.com`  
## 4. 本地仓库与远程仓库链接
#### &diams; 检查SSH key
到根目录: `cd ~`  
进入.shh文件夹: `cd ./.ssh`  
查看.ssh下的文件: `ls`  
查看秘钥是否生成: `cat id_rsa.pub`  
选中秘钥进行复制  
###### PS: 以上任何一步出现问题则说明有可能SSH key没有生成，需要创建
#### &diams; 创建SSH key
到根目录: `cd ~`  
使用邮箱生成秘钥: `ssh-keygen -t rsa -C "你的邮箱"`  
回车，再回车，输入自己mac开机密码，生产以下信息证明已经成功.
![Alt text](/assets/GitHub_Pages/7.png "SSH key")
重复检查SSH key的步骤：找到.ssh文件打开 id_rsa.pub，复制秘钥
#### &diams; 配置GitHub
![Alt text](/assets/GitHub_Pages/8.png  "SSH key"){:height="50%" width="30%"}  

![Alt text](/assets/GitHub_Pages/9.png "SSH key")  

![Alt text](/assets/GitHub_Pages/10.png "SSH key"){:height="50%" width="80%"}
回到终端，在.shh文件夹下查看ssh key配置是否成功: `ssh -T git@github.com`
## 5. 测试
- 简单的GitHub Pages  
这个官方教程 [GitHub Pages](https://pages.github.com/) 写的十分好懂，按这个做完之后你就有了一个你的网址 username.github.io，里面有一句 Hello World！
- 本地文件上传远程仓库
   * cd到项目文件夹  
   * `git init`: 初始化git仓库【不必须】  
   * `git add .`: 添加目录下所有项目文件到暂存区  
   * `git commit -m "xxx"`: 添加提交说明，简短  
   * `git remote rm origin`: 清除源项目【当下面命令报错：fatal: remote origin already exists，否则跳过】  
   * `git remote add origin https://xxxxxxx`: 仓库的http地址
![Alt text](/assets/GitHub_Pages/11.png "http address"){:height="50%" width="50%"}
   * `git push -u origin master`: push到远程到仓库【执行这一步可能会要求输入git的账号和密码】

###### PS: [git从代码仓库克隆代码，修改并上传](https://www.cnblogs.com/sk-3/p/9266091.html)  
# 二、jekyll
## 1. 本地环境搭建
安装 Jekyll 需要 Ruby 环境支持，Mac 系统是自带了 Ruby 环境的。终端中使用 `ruby -v` 可以查看当前 Ruby 版本。  
除了 Ruby，还需要 RubyGems，RubyGems（简称 gems）是一个用于对 Ruby 组件进行打包的 Ruby 打包模块。 它提供一个分发 Ruby 程序和库的标准格式，还提供一个管理程序包安装的工具。如果已安装该模块，终端中输入 `gem -v` 可以查看其版本。  
另外，还需要 GCC 和 Make 支持，这一般是系统自带的，可通过 `gcc -v`、`g++ -v` 和 `make -v` 查看其版本。  
确认上述的环境都配置正确后，就可以开始使用 gem 安装 Jekyll 了，终端中执行 `gem install jekyll bundler` 开始安装。安装完成后可输入 `bundler -v` 和 `jekyll -v` 查看其版本。  

###### PS: sudo + 命令 赋予操作较高权限  
## 2. jekyll目录结构
一个标准的使用Jekyll工具生成的网站,其目录结构**一般**如下:  
>├── _config.yml  
>├── _drafts  
>├   ├── begin-with-the-crazy-ideas.textile  
>├   └── on-simplicity-in-technology.markdown  
>├── _includes  
>├   ├── footer.html  
>├   └── header.html  
>├── _layouts  
>├   ├── default.html  
>├   └── post.html  
>├── _posts  
>├   ├── 2020-06-28-welcome-to-jekyll.markdown  
>├   └── 2020-06-29-hello-jekyll.markdown  
>├── _data  
>├   └── members.yml  
>├── _site  
>└── index.html  
  

|文件/目录|描述|  
|:-----:|:--:|  
|_config.yml|保存配置数据。很多配置选项都会直接从命令行中进行设置，但是如果你把那些配置写在这儿，你就不用非要去记住那些命令了。| 
|_drafts|drafts 是未发布的文章。这些文件的格式中都没有 title.MARKUP 数据。学习如何使用 drafts. |
|_includes|你可以加载这些包含部分到你的布局或者文章中以方便重用。可以用这个标签 &#123;&#37; include file.ext &#37;&#125; 来把文件 _includes/file.ext 包含进来。|
|_layouts|layouts 是包裹在文章外部的模板。布局可以在 YAML 头信息中根据不同文章进行选择。 这将在下一个部分进行介绍。标签 &#123;&#123; content &#125;&#125; 可以将content插入页面中。|
|_posts|这里放的就是你的文章了。文件格式很重要，必须要符合: YEAR-MONTH-DAY-title.MARKUP。 The permalinks 可以在文章中自己定制，但是数据和标记语言都是根据文件名来确定的。|
|_data|放一些其他配置文件,必须是.yml或者.yaml格式的,比如有一个文件叫members.yml,如果想引用这个文件里的内容就通过site.data.membres来引用|
|_site|一旦 Jekyll 完成转换，就会将生成的页面放在这里（默认）。最好将这个目录放进你的 .gitignore 文件中。|

## 3. 本地测试
- 模板  
网上搜索会有非常多的模板，推荐一个网站：[Jekyll Theme](http://jekyllthemes.org)   
【jekyll版本偏老，使用时需要替换Gemfile和Gemfile.lock文件】  
- [官方手册](https://www.jekyll.com.cn/docs/)  
- 流程  
   1. 下载一个模板  
   2. 依照官方手册创建一个网站文件夹  
   3. 将文件夹内生成的Gemfile和Gemfile.lock替换模板内的  
   4. 依照官方手册进入模板文件夹进行运行  
   5. 模板文件夹内的README文件中非常详细的介绍了_config.yml
   6. 依照实际运行效果进行修改  
  
###### PS: 出现报错多为版本问题其次是路径问题  
## 4. 放上GitHub
#### &diams; 清空仓库【清除之前的测试】
移除历史记录:  
>`rm -rf .git`  

重新对当前仓库初始化:  
>`git init`  

#### &diams; push模板文件夹内的所有文件
>`git add .`  
>`git commit -m "Initial commit"`:  

推到github远程仓库，确保覆盖历史记录:  
>`git remote add origin git@github.com:<YOUR ACCOUNT>/<YOUR REPOS>.git`  
>`git push -u --force origin master`  

## 5. 调整名称/路径
{% highlight ruby %}
baseurl: "/" #定位到当前文件夹
url: "https://cguling.github.io/" # 访问网站的网址
{% endhighlight %}
![Alt text](/assets/GitHub_Pages/12.png "http address"){:height="80%" width="80%"}
## 6. 测试
尝试访问自己的网址，出现模板应有画面那么就此大功告成，剩下的就是自己对模板进行调整和测试，一边实践一边学习语法吧！
# 三、坑坑总结
1. git本地仓库与远程仓库关联出现 failed to push some refs to git的问题: [解决][result1]  
2. 出现! \[rejected] master -> master (fetch first)问题: [解决][result2]  
3. jekyll serve 出现...(Bundler::GemNotFound): [解决][result3]  
4. 出现Could not fetch specs from https://rails-assets.org错误: [换源][change-source1]  
5. Could not find a valid gem 'jekyll' 安装jekyll问题: [换源][change-source2]  

###### PS: 我使用的源：http://gems.ruby-china.com/  
# 四、参考链接
- https://blog.csdn.net/yuexianchang/article/details/53431703  
- https://www.jianshu.com/p/f82c76b90336  
- https://blog.csdn.net/lu1024188315/article/details/74080021  
- https://www.cnblogs.com/wei-dong/p/13069093.html  
- https://www.cnblogs.com/ranyihang/p/12289478.html  
- https://blog.csdn.net/u012168038/article/details/77715439  
- https://www.jianshu.com/p/25111a6002ec  
- https://www.cnblogs.com/guangzan/p/12567819.html  

>>推崇Hexo的人超级多，有机会搭建个Hexo的博客试试  
>>提前准备几个链接:  
>>[MAC安装配置nodeJS](https://www.jianshu.com/p/94f3c9f12e24)  
>>[MAC配置测试nodeJS](https://blog.csdn.net/yst19910702/article/details/89714544)  
>>[Hexo+GitHub Pages 搭建博客](https://blog.csdn.net/u011974987/article/details/51331822)  

[hide-show]: https://www.baidu.com/link?url=AWl6Kqlud80hSB0trhoUkF4Jrc_OeJDDsTfoHxa-eM9n5yd-ahpxWkkfZ3VzUITvIYJ1TscJEPIJVegXzsxo-_&wd=&eqid=a6c455330001040c000000025efda0a4
[result1]: https://blog.csdn.net/dou_being/article/details/52807232
[result2]: https://blog.csdn.net/weixin_44118318/article/details/85030461
[result3]: https://www.jianshu.com/p/c70dc6d3af14
[change-source1]: https://blog.csdn.net/liguangxianbin/article/details/79551798
[change-source2]: https://blog.csdn.net/sunxboy/article/details/84723116
