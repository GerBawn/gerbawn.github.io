---
title: 使用GitHub Pages和Hexo快速搭建个人博客
---
## 前言
为了记录自己的学习过程，决定开始写个人博客，经过一晚上的折腾，最终决定使用github pages和hexo的个人博客系统，特将整个搭建过程记录下来，同时也作为博客的第一篇文章发表。
注：本文已默认读者拥有一个github账号，并了解git的常用操作。

## 上传ssh key到github
这里有两种方式可以上传ssh key到github，一种是使用github的[windows客户端][1]。
下载后按照指示安装，打开客户端
![ssh1]点击红框内的设置按钮，然后选择setting

使用自己的github账号登陆，然后打开git bash，输入
```
ssh -T git@github.com
```
出现下图就说明成功了。
另外一种方法是使用命令行
```
ssh-keygen
```
## 在github上建立博客仓库
github pages默认使用用户名.github.io仓库的master分支做为内容，所以首先需要建立一个仓库，像我就需要建立一个gerbawn.github.io的仓库，建立仓库的步骤在此就不详述了。

## 安装hexo并初始化
首先需要安装node.js，node.js有windows客户端，一键安装就好了。然后打开git bash。
```
npm install hexo-cli -g  //全局安装hexo-cli
cd /e/blog/   //这里输入你想创建博客的目录
npm install hexo-deployer-git --save
hexo init //hexo初始化
```
最后出现![succ]代表初始化成功。
然后修改`_config.yml`文件将目录与github上建立的仓库绑定。
![config]
注意这里的冒号后面一定要有一个空格。
接下来就可以开始编写博客了，首先来看一下hexo自带的一个例子
```
hexo generate  //每次使用写完博客使用这个命令生成html页面
hexo server //启动一个本地server，用于预览博客
```
输入上面的命令应该可以看到
![hexo_server]
这时候就可以输浏览器中输入`localhost:4000`看到默认输入的博文了![hexo_default]如果你打不开这个页面，说明你的端口冲突了，可以在命令行中输入`netstat -ano|grep 4000`找出对应的程序将其关闭。
接下来就可以生成自己的博文了，使用`hexo new test`或者自己在`/e/blog/source/_post/`目录下新建一个test.md，开始编写博文。写完之后输入
```
hexo clean
hexo generate
hexo server
```
打开浏览器应该能看见![hexo_test]，至此我们的本地博客系统就建立好了，下面要做的就是上传到github上了。使用`hexo deploy`将博客推送到github上，然后打开`http://gerbawn.github.io`就能看到刚刚写的博客了。

## 使用github管理博客
为了在多台机器上使用博客，我们需要将整个博客使用git管理起来，之前的`hexo deploy`只是将hexo生成的目标文件上传到了github上。
首先在`/e/blog`目录下初始化一个git仓库，这个仓库是用于管理博客的源码的，方便在多机间进行操作。
```
git init
git remote add origin https://github.com/gerbawn/gerbawn.github.io.git //这里的链接填你希望保存博客源码的仓库地址，推荐直接使用github pages使用的仓库
git checkout -b hexo //新建一个hexo用于保存源码，因为在之前我们将hexo生成的博客推送到了master分支，所以这里需要新建一个分支
git ci -m 'init'
git push origin hexo:hexo
```
现在我们就能在gerbawn.github.io仓库下看到新的分支了，如果你在其他地方需要写博客，只需要将分支clone下来就行了，方便又快捷。

## 使用自己的域名访问博客
如果需要使用自己的域名来访问博客，就需要将域名绑定到github上。
在域名管理中添加一个CNAME解析指向github pages，像我就是`blog.gerbawn.com`指向`gerbawn.github.io`。
然后在source目录下新建一个CNAME文件，里面写上自己的域名，再使用`hexo deploy`将这个文件推送到github上。打开浏览器输入blog.gerbawn.com就可以看到自己的博客了。

至此整个博客就搭建成功了，总的来说还是比较简单的。 接下来可以学习一些hexo的相关知识，美化博客的页面和增加功能，详见[hexo文档][2]。到目前为止，博客还只是一个纯静态页面，连评论功能都没有，在网上搜索了一下，评论功能现在主流的有[Disqus][3]，[多说][3]等等，因为我也还没有加上评论功能，在这里就不多说了。




[1]: https://github-windows.s3.amazonaws.com/GitHubSetup.exe
[2]: https://hexo.io/zh-cn/docs/
[3]: https://disqus.com
[4]: http://duoshuo.com/
[ssh1]: http://oczidrtiz.bkt.clouddn.com/ssh1.png
[succ]: http://oczidrtiz.bkt.clouddn.com/hexo1.png
[config]: http://oczidrtiz.bkt.clouddn.com/hexo_config1.png
[hexo_server]: http://oczidrtiz.bkt.clouddn.com/hexo_server.png
[hexo_default]: http://oczidrtiz.bkt.clouddn.com/hexo_default.png
[hexo_test]: http://oczidrtiz.bkt.clouddn.com/hexo_test.png
