# 远程管理hugo源文件以自动更新blog


将博客源文件托管在某个地方，换台电脑继续编辑。再加上触发机制更好，即一上传⏫就同步更新博客～

<!--more-->

在之前[快速搭建个人博客]({{< ref "posts/20210528-搭建个人博客/index.md" >}})的文章中有提到

> 将 `public` 文件夹下的文件推送至GitHub仓库

每次这么操作还是有点繁琐的，所以就想如果更新完hugo源文件，博客自动更新就好了。此外，换台电脑💻，依然正常操作就更更更好了～

终于，是找到了GitHub的 ***Actions*** 中 ***Workflows*** 功能

将Hugo源文件维护在GitHub上，只要源文件的仓库更新，自动更新存有 `public` 文件的仓库，那么博客也就随之更新了。


在此记录下实现过程 👇

## 1. 生成SSH key （Windows 环境）

参考[这个](https://www.jianshu.com/p/95262f5eba7a)

1. 通过git bash cd 至 `.ssh` 文件夹

  ```shell
  cd ~/.ssh/
  ```

  如果提示 `No such file or directory`，可以手动的创建一个 `.ssh`文件夹，***BY***  `mkdir ~/.ssh`

2. 配置全局 name 和 email


  ```shell
  git config --global user.name "你的用户名"
  git config --global user.email "你的公司或个人邮箱"
  ```

4. 生成 key

  ```git
  ssh-keygen -t rsa -C "你的公司或个人邮箱"
  ```
  
  连续按 **3** 次回车

  最后得到俩文件： `id_rsa` 和 `id_rsa.pub`

## 2. 创建并配置仓库

参考[这个](https://jinnzy.github.io/shi-yong-hugolai-da-jian-ge-ren-blog/#%E5%88%A9%E7%94%A8github-pages%E9%83%A8%E7%BD%B2blog)

### 2.1 配置hugo源文件的仓库

**仓库名称不限**，在此以 `unclehuzi.github.io.source` 为例

进入`unclehuzi.github.io.source`仓库，添加`Secrets`，名称为`ACTIONS_DEPLOY_KEY`，将 `id_rsa` 文件的内容粘过去，得到内容如下所示

![Secrets](https://gitee.com/unclehu/pic/raw/master/img/image-20210608140032371.png)



**上传 hugo源文件**

> 把 `themes` 主题文件夹中的 `.git` 文件删除

不然`Github` 会检测到是别的仓库，上传后文件夹是灰色的



### 2.2 配置 unclehuzi.github.io 仓库

仓库名称有讲究，得是这个 `github_user_name.github.io`

进入`unclehuzi.github.io` 仓库，添加`Deploy keys` ，名称不限制，将`id_rsa.pub`文件的内容粘过去。

![Deploy keys](https://gitee.com/unclehu/pic/raw/master/img/image-20210608140646451.png)

![result](https://gitee.com/unclehu/pic/raw/master/img/image-20210608140754762.png)



### 2.3 配置工作流（Workflows）

进入`unclehuzi.github.io.source`仓库，创建 **Actions**

![Actions](https://gitee.com/unclehu/pic/raw/master/img/image-20210608140939363.png)

代码如下 👇

```yaml
name: Deploy Hugo Site to Github Pages on Master Branch

on:
  push:
    branches:
      - master  # Attention 1

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.83.1'
          extended: true  # 使用扩展版
          # Attention 2

      - name: Build
        run: hugo #--minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY }} # 这里的 ACTIONS_DEPLOY_KEY 则是上面设置 Private Key的变量名
          external_repository: unclehuzi/unclehuzi.github.io # Pages 远程仓库 
          publish_dir: ./public
          keep_files: false # remove existing files
          publish_branch: master  # deploying branch
          # Attention 3
          commit_message: ${{ github.event.head_commit.message }}
```


{{< admonition type=note title="备注" open=true >}}
* Attention 1

  `source` 仓库的分支名称为 `master`

* Attention 2

  `hugo` 的版本

* Attention 3

  `unclehuzi.github.io` 仓库的分支名称
{{< /admonition >}}




## 3. Finished

![result](https://gitee.com/unclehu/pic/raw/master/img/image-20210608141817601.png)

以后维护好`source`这个仓库就能实现

1. 触发机制以自动更新blog
2. 换个电脑 💻 继续写blog

## Reference

1. https://jinnzy.github.io/shi-yong-hugolai-da-jian-ge-ren-blog/#%E5%88%A9%E7%94%A8github-pages%E9%83%A8%E7%BD%B2blog
2. https://www.jianshu.com/p/95262f5eba7a







<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
