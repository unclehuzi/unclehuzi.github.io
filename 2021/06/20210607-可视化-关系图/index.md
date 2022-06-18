# pyecharts-关系图


以官方例子：[微博转发关系图](https://gallery.pyecharts.org/#/Graph/graph_weibo)为例，说明所需要的数据格式

<!--more-->

## 数据样例

```python
import json

from pyecharts import options as opts
from pyecharts.charts import Graph

with open("weibo.json", "r", encoding="utf-8") as f:
    j = json.load(f)
    nodes, links, categories, cont, mid, userl = j
c = (
    Graph(init_opts=opts.InitOpts(width="1920px"
                                 ,height="1080px"))
    .add(
        "",
        nodes,
        links,
        categories,
        repulsion=900,  # 节点之间的斥力因子。值越大则斥力越大
    	# 支持设置成数组表达斥力的范围，此时不同大小的值会线性映射到不同的斥力。
        gravity=0.01,  # 节点受到的向中心的引力因子。该值越大节点越往中心点靠拢。
        linestyle_opts=opts.LineStyleOpts(curve=0.2),
        label_opts=opts.LabelOpts(is_show=False),
    )
    .set_global_opts(
        legend_opts=opts.LegendOpts(is_show=False),
        title_opts=opts.TitleOpts(title="Graph-微博转发关系图"),
    )
    .render("graph_weibo.html")
)
```

`weibo.json` 文件，可从 [这儿](https://github.com/pyecharts/pyecharts-gallery/blob/master/Graph/weibo.json) 获取

主要由5部分组成

![image-20210607171146263](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20210607171146263.png)

* nodes
* links
* categories
* cont
* mid
* userl

## 格式说明

{{< admonition type=note title="说明" open=true >}}
本文聚焦在前三个的数据格式说明。每一部分是个`list`，每个`list` 又由多个`dict`组成
{{< /admonition >}}


以“转发微博”作为场景简单阐述关系图所展示的信息：某位具有影响力的微博用户A 发了条微博，被用户B、C、D看到并转发了；之后，用户E、F、G也转发了B所转发的这篇文章，以此类推

那么，这个过程中涉及到的每个用户便是一个`node`。为此，也以 `nodes`作为切入点展开说明

### nodes

样例如下所示，

![node-example](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20210608150552783.png)

记录着节点的信息[^1][^2]，

```json
{
    "name": "Camel3942",            # 节点名称，即博主昵称
    "symbolSize": 5,        		# 图中标志大小     
    "draggable": "False",        	# 是否可拖动
    "value": 1,        				# 被再次转发次数
    "category": "Camel3942",          # From Where
    "label": {	                    # 此博主被再次转发后，含有此标签，否则不含
    	"normal": {
    		"show": "True"
            	}
            }
}
```


{{< admonition type=warning title="警告" open=true >}}
节点名称（`name`）不能重复
{{< /admonition >}}

### links

个人理解作用是将每个`node` 连接起来

```json
{
    "source": "新浪体育",
    "target": "Beijingold4"
},
{
    "source": "Camel3942",
    "target": "xiaoA"
}
```



### categories

而这部分则记录着**有被他人转发**的用户名称（`name`），即 `links` 中 `source` 所对应的内容

```json
{
    "name": "新浪体育"
},
{
    "name": "Camel3942"
}
```



## Reference

[^1]: https://blog.csdn.net/qq_35006861/article/details/116721589
[^2]: https://blog.csdn.net/Kevin_HZH/article/details/91043392




<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
