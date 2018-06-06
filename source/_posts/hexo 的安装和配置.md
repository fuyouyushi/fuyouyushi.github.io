---
title: Hexo 的安装和配置
---

## hexo 的安装

### homebrew 的安装

终端安装命令:

<!-- more -->

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

安装结束后，检测安装结果。终端检测命令:

```
$ brew doctor   
//(输出 “Your system is ready to brew” 表示homebrew成功)
```

### 安装 Nodejs

使用homebrew 安装Nodejs. 终端安装命令:

```
$ brew install node

```

### 安装 git

mac 自带git, 此步跳过

### 安装hexo

终端安装命令:

```
$ sudo npm install -g hexo
```

初始化hexo 到一个指定的目录(我的自定义目录为myhexo)。

```
$ hexo init myhexo
```

进入myhexo， 进行安装npm

```
$ npm install
```

安装完成后，可以输入下面命令开启 hexo 服务，

```
$ hexo s
```

开启之后在 本地地址可以进行预览 [http://localhost:4000/](http://localhost:4000/).

## 关联github

在github 上面新建一个Repository，名字为${github用户名}.github.io, 例如: **fuyouyushi.github.io** 
<br>在刚才创建的myhexo 文件下面打开 _config.xml 文件，在最后进行编辑

```
deploy:
  type: git
  repository: https://github.com/fuyouyushi/fuyouyushi.github.io
  branch: master
```

在myhexo 目录下执行

```
$ hexo g
```

接下来执行

```
$ hexo d
```

如果遇到提示 **ERROR Deployer not found: git**, 则先执行以下命令再执行**hexo d**

```
$ npm install hexo-deployer-git --save
```

期间需要输入 github 的用户名和密码.
<br>至此，可以利用github pages + hexo建立一个初步的个人博客。

## 下载主题

### 下载Next 主题

hexo的主题网站[https://hexo.io/themes/index.html](https://hexo.io/themes/index.html). 里面列出了各种样式的主题。以**Next** 主题为例：
</br>进入到Hexo 站点目录下(之前我创建的为myhexo 目录)

```
$ cd myhexo
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```

下载完成后，打开配置文件***_config.yml***, 修改theme 为next

```
theme: next
```

之后命令行先进行clean, 再启动本地服务器可以在本地 [http://localhost:4000/](http://localhost:4000/), 预览next主题的效果

```
$ hexo clean
$ hexo s -g
```

### 配置Next 主题

针对于Next 主题的配置需要修改的是 themes/next 目录下的_config.yml 文件

#### Scheme 配置:
Scheme 是 NexT 提供的一种特性，借助于 Scheme，NexT 为你提供多种不同的外观.

- Muse - 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
- Mist - Muse 的紧凑版本，整洁有序的单栏外观
- Pisces - 双栏 Scheme，小家碧玉似的清新

搜索 scheme 关键字。 你会看到有三行 scheme 的配置，将你需用启用的 scheme 前面注释 # 去除即可。

```
 #scheme: Muse
 #scheme: Mist
 scheme: Pisces
```

#### 设置头像

修改字段 avatar， 值设置成头像的链接地址。其中，头像的链接地址可以是

- 完整的互联网 URI， 例如 http://example.com/avatar.png
- 站点内的地址. 将头像放置主题目录下的 source/uploads/ (新建 uploads 目录若不存在).配置为：avatar: /uploads/avatar.png; 或者 放置在 source/images/ 目录下, 配置为：avatar: /images/avatar.png






## TODO list:
- ssh 的添加
