# GitHub+Hugo快速搭建个人博客


详述Hugo+Github搭建个人博客
<!--more-->


受我司同期大佬的影响，我也便搭建个人博客折腾下，记录点什么~

在这demo成型之际，以小白视角记录下基于 [HuGo](https://gohugo.io/) 和 [Github](https://github.com/unclehuzi/unclehuzi.github.io) 完成搭建的历程

所以，最初的准备工作便是（假设已安装[`Git`](https://git-scm.com/)）


## 安装 Hugo

可以参考[官方文档](https://gohugo.io/getting-started/installing/)完成在Windows, Mac等平台的安装

> 虽然HuGo是基于`Go`语言编写的，但并不是一定要安装`Go`才能使用HuGo。可跨平台

在此，以 Windows 系统为例记录安装过程

1. 在[这儿](https://github.com/gohugoio/hugo/releases)下载对应的压缩包

   ![示例](https://i.loli.net/2021/05/28/9LIpGlriVdAHaCz.png)

2. 解压

   `hugo_0.83.1_Windows-64bit.zip`解压后得到的文件如下图所示

   ![解压后的文件](https://i.loli.net/2021/05/28/7drVlwEK4sFxyL9.png)

3. 添加至环境变量

   将`hugo.exe`文件放入`bin`文件夹（若无可新建）

   ![示例-环境变量](https://i.loli.net/2021/05/28/tGvqagUWm7uVPOD.png)

4. 验证是否安装成功

   ```shell
   hugo version
   
   # hugo v0.83.1-5AFE0A57 windows/amd64 BuildDate=2021-05-02T14:38:05Z VendorInfo=gohugoio
   ```

5. 本地创建博客

   终端 `cd` 至某个文件下

   ```shell
   hugo new site unclehuzi  # blog目录就是创建的博客目录
   
   # Congratulations! Your new Hugo site is created in D:\unclehuzi.
   # Just a few more steps and you're ready to go:
   # 1. Download a theme into the same-named folder.
   #    Choose a theme from https://themes.gohugo.io/ or
   #    create your own with the "hugo new theme <THEMENAME>" command.
   # 2. Perhaps you want to add some content. You can add single files
   #    with "hugo new <SECTIONNAME>\<FILENAME>.<FORMAT>".
   # 3. Start the built-in live server via "hugo server".
   # Visit https://gohugo.io/ for quickstart guide and full documentation.
   
   tree unclehuzi/
   #####
   D:\unclehuzi
   ├─archetypes
   ├─content			    # 博客文章存放目录（markdown文件）
   ├─data
   ├─layouts
   ├─static			    # 静态文件/图片/CSS/JS文件
   ├─themes			    # 博客主题模板存放目录
   └─config.toml            # 博客的配置文件           
   ```

6. 选择主题

   我选择的是 [Maupassant](https://github.com/flysnow-org/maupassant-hugo/)，通过`Git` 的方式获取

   ```shell
   # cd 至 unclehuzi 文件夹
   
   git clone https://github.com/flysnow-org/maupassant-hugo themes/maupassant
   
   cp themes/maupassant/exampleSite/config.toml .  # 使用模板自带的配置文件替换默认配置文件
   
   mkdir content/post	# 创建博客文章md文件存放路径（该主题模板要求放在content/post目录下）
   ```

7. 根据需要调整 `D:\unclehuzi\config.toml` 文件

   ```shell
   baseURL = "your_github_name.github.io"  # 修改为博客的网址，此处使用github pages地址，后面具体介绍
   languageCode = "zh-CN"
   title = "胡子叔叔的小站"                  # 博客的名字
   theme = "maupassant"
   ```

   具体可以参考 [官方说明](https://github.com/flysnow-org/maupassant-hugo/) 进行配置

8. 本地测试

   ```shell
   # cd D:\unclehuzi
   hugo new post/my-first-blog.md
    
   vim content/post/my-first-blog.md
   
   # 以下为md文件内容
   +++
   title="My First Blog"
   tags=["blog"]
   categories=["博客相关"]
   date=2020-01-16T10:37:32+08:00
   draft=false	# 此处要改为false，否则在首页不会显示！
   +++
   #### Hello World!
   ```

   通过 `hugo server -D` 命令启动，访问 [http://localhost:1313/](http://localhost:1313/) 即可

## 建立博客专属的仓库

**主要的事情说三遍**：

**Repository name 填 github名字.github.io**，如 `unclehuzi.github.io`

**Repository name 填 github名字.github.io**，如 `unclehuzi.github.io`

**Repository name 填 github名字.github.io**，如 `unclehuzi.github.io`

仓库创建完成之后记得修改 `D:\unclehuzi\config.toml` 文件的 `baseURL`

### 生成 public 文件

创建完成之后，通过 `hugo` 生成 `public` 文件

```bash
hugo  --theme=maupassant --baseUrl="https://your_github_name.github.io" 
# theme为主题模板名称，url为上一步创建的github仓库名称
```

### 通过 git 命令将 public 文件推送至仓库

```bash
cd D:\unclehuzi\public

# 1. 初始化
git init

# 2. 将文件夹下所有文件加入本地仓库
git add .

# 3. 添加注释
git commit -m "comments"	# git commit -m "updates $(date)"

# 4. 建立连接  BY HTTPS
git remote add origin https://github.com/unclehuzi/unclehuzi.github.io.git

# 5. 远程仓库拉到本地（如果远程仓库内没有文件可跳过步骤5）
git pull --rebase origin master
# 之后更新本地：
git pull origin master

# 6. 上传 push
git push -u origin master

# git push -u origin master -f # 强制上传

# 备注
# 和远程建立连接之后，执行 2-3-6 步骤即可。
```

最后，访问 https://your_github_name.github.io 即可

## 总结

本文基于 Github Pages 服务，使用成熟的框架及主题快速搭建个人博客demo。

更新网站两步走：

1. 在博客文件夹（`D:\unclehuzi`）下生成 `public` 文件夹
2. 将 `public` 文件夹内的所有文件推送至GitHub仓库

后续在网站设计方面还需要了解下Web开发相关知识，针对性的修改主题的源码以实现自己的需求~


