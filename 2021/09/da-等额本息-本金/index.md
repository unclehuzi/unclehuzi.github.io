# 等额本息-等额本金



test
<!--more-->

hhhhhh

{{< mermaid >}}
journey
    title My working day
    section Go to work
      Make tea: 5: Me
      Go upstairs: 3: Me
      Do work: 1: Me, Cat
    section Go home
      Go downstairs: 5: Me
      Sit down: 5: Me

{{< /mermaid >}}


{{< echarts >}}
title:
    text: 折线统计图
    top: 2%
    left: center
tooltip:
    trigger: axis
legend:
    data:
        - 邮件营销
        - 联盟广告
        - 视频广告
        - 直接访问
        - 搜索引擎
    top: 10%
grid:
    left: 5%
    right: 5%
    bottom: 5%
    top: 20%
    containLabel: true
toolbox:
    feature:
        saveAsImage:
            title: 保存为图片
xAxis:
    type: category
    boundaryGap: false
    data:
        - 周一
        - 周二
        - 周三
        - 周四
        - 周五
        - 周六
        - 周日
yAxis:
    type: value
series:
    - name: 邮件营销
      type: line
      stack: 总量
      data:
          - 120
          - 132
          - 101
          - 134
          - 90
          - 230
          - 210
    - name: 联盟广告
      type: line
      stack: 总量
      data:
          - 220
          - 182
          - 191
          - 234
          - 290
          - 330
          - 310
    - name: 视频广告
      type: line
      stack: 总量
      data:
          - 150
          - 232
          - 201
          - 154
          - 190
          - 330
          - 410
    - name: 直接访问
      type: line
      stack: 总量
      data:
          - 320
          - 332
          - 301
          - 334
          - 390
          - 330
          - 320
    - name: 搜索引擎
      type: line
      stack: 总量
      data:
          - 820
          - 932
          - 901
          - 934
          - 1290
          - 1330
          - 1320
{{< /echarts >}}





<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">

