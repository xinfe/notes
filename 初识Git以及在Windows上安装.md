# 初识Git以及在Windows上安装
### 一 、 简单了解Git
  Git是分布式版本控制系统，是Linus为了便于管理开源代码、帮助Linux开发而创造的（大神用了俩礼拜用C写的），大家所熟知的GitHub代码托管平台便是使用的Git。
  
  Linus为什么不使用CVS、SVN这些免费的集中式版本控制系统来管理代码呢？这就扯到集中式与分布式的区别。如下图：

![图片.png](http://upload-images.jianshu.io/upload_images/4860205-5461ddc2897df980.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

  所以，Linus宁愿自己写也不愿使用集中式版本控制系统。而Git的优点还远远不止于此，它免费开源，还拥有强大的分支管理，当我们熟练使用时便能体会到。
### 二 、  [Git版本下载](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

  点击上方小标题，前往下载。由于本人的电脑是Windows系统，所以下载相应安装包。
### 三 、 在Windows上安装Git
  1. 打开安装包，默认安装。

  2. 安装完成后，点击运行Git Bash，弹出小黑框“Welcome to Git”，说明Git安装成功.

  3. 在命令行中输入以下内容对你的机器进行标志（双引号中的内容需修改为你自己的）：
```
	$ git config --global user.name "Your Name"
	$ git config --global user.email "email@example.com"
```
### 四 、 参考资料
 - [廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)      
 - [Git官网](https://git-scm.com/)