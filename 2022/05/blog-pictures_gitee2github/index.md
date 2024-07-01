# 将博客中的图片从Gitee迁移至Github






Gitee这波仓库开源需审核的这波操作是有点秀的

<!--more-->

到底是啥原因？政府？公司盈利问题？

[如何看待2022年 5 月 18 日 Gitee 仓库开源须审核，已开源部分仓库暂时关闭，审核通过后再次公开？](https://www.zhihu.com/question/533388365/answer/2492508717)

我用Gitee主要是两个原因：

1. 解决Github下载慢的问题
2. 图床

我是希望基于Markdown写作并且能一文多发的，有一天我发现存在Gitee的图片微信公众号能加载成功，此后我便依赖Gitee搞了个图床，博客的很多图片也存在Gitee上面。

发生这件事儿后，我本以为实名认证下，提交下审核就没事儿，仓库依然能正常开源的了，结果...万万没想到

![image-20220528164021689](https://raw.githubusercontent.com/unclehuzi/pic/master/PicGoimage-20220528164021689.png)

这就直接导致了此博客基于Gitee的图片都无法加载...

和朋友吐槽了这事儿，他提供了一个很好的方案。先把Gitee的图片挪到GitHub，再替换下图片链接

![image-20220528165501884](https://raw.githubusercontent.com/unclehuzi/pic/master/PicGoimage-20220528165501884.png)

```python
from pathlib import Path

p = Path("folderPath") # e.g. /content/posts/
FileList=list(p.glob("**/*.md")) 

def alter(file,old_str,new_str):
    """
    替换文件中的字符串
    :param file:文件名
    :param old_str:就字符串
    :param new_str:新字符串

    Reference: https://blog.csdn.net/qq_30068487/article/details/90297814
    """
    file_data = ""
    with open(file, "r", encoding="utf-8") as f:
        for line in f:
            if old_str in line:
                line = line.replace(old_str,new_str)
            file_data += line
    with open(file,"w",encoding="utf-8") as f:
        f.write(file_data)


o_s = "https://gitee.com/unclehuzi/picture/raw/master/img"
n_s = "https://raw.githubusercontent.com/unclehuzi/pic/master/img"

for file in FileList:
    alter(file, o_s, n_s)
```



同时，我也向Gitee提交了注销账号的申请...这么一搞普通老百姓本身也不是目标用户了...

关于图床的 PlanB 就是七牛云了，但需要有个域名



<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
