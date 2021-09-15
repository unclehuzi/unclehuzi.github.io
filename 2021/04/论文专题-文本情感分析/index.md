# 【论文专题】文本情感分析


这篇是文本情感分析的应用篇

<!--more-->

**运用情感分析技术，让我们的研究更丰富**

本文所讨论的情感分析聚焦在 **二分类** 情况，即判断一句话（短文本）的情感倾向是正面还是负面

我们可以简单的理解为：

存在一个很牛逼的箱子，这个箱子有进口处和出口处，我们需要做的便是把某句话通过进口处放入这个箱子，之后这个箱子从出口处吐出结果

其中情感倾向的结果方面，大多数箱子是会告诉我们情感倾向为正的概率是多少（假设是 $a$ ），显然情感倾向为负的概率便是 $1-a$

若 $a>1-a$ ，即 $a>0.5$ ，“箱子”认为这句话的情感倾向是正面的

总之，我们可以根据研究的需要，根据箱子吐出的结果构造变量用于下游的任务。

那么，本文就系统的整理了好用的“箱子”


## ERNIE[^1]

谷歌的BERT问世之后，预训练模型那是备受关注啊，算是自然语言处理领域的里程碑事件了。

我为了蹭热度，琢磨百度的这个ERNIE，也是废了点时间

在本地装好各种环境之后，在我这个小破电脑上训练了一天一夜吧

但最终的方案是蹭了百度的 AI Studo

注册账号，运行项目还送算力卡，这样就能用GPU训练了

小破电脑一天一夜的活，几分钟就搞定了

> 本地装包，没梯子的话，一定要用清华的镜像

```bash
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/Paddle/
```

Python代码

```python
#需要更改训练集、验证集、测试集的文件位置

import paddlehub as hub

!hub install ernie

module = hub.Module(name="ernie")

class DemoDataset(BaseNLPDataset):
    """DemoDataset"""
    def __init__(self):
        # 数据集存放位置
        self.dataset_dir = "work"
        super(DemoDataset, self).__init__(
            base_path=self.dataset_dir,
            train_file="train.tsv",
            dev_file="dev.tsv",
            test_file="test.tsv",
            # 如果还有待预测数据，可以放在predict.tsv
            predict_file="predict.tsv",
            train_file_with_header=True,
            dev_file_with_header=True,
            test_file_with_header=True,
            predict_file_with_header=True,
            # 数据集类别
            label_list=["0", "1"])
dataset = DemoDataset()

reader = hub.reader.ClassifyReader(
    dataset=dataset,
    vocab_path=module.get_vocab_path(),
    sp_model_path=module.get_spm_path(),
    word_dict_path=module.get_word_dict_path(),
    max_seq_len=128)
    
strategy = hub.AdamWeightDecayStrategy(
    weight_decay=0.01,
    warmup_proportion=0.1,
    learning_rate=5e-5)
    
config = hub.RunConfig(
    use_cuda=True,
    num_epoch=1,
    checkpoint_dir="ernie_txt_cls_turtorial_demo",
    batch_size=128,     #一般为2^n
    eval_interval=10,
    strategy=strategy)
    
inputs, outputs, program = module.context(
    trainable=True, max_seq_len=128)

# Use "pooled_output" for classification tasks on an entire sentence.
pooled_output = outputs["pooled_output"]

feed_list = [
    inputs["input_ids"].name,
    inputs["position_ids"].name,
    inputs["segment_ids"].name,
    inputs["input_mask"].name,
]

cls_task = hub.TextClassifierTask(
    data_reader=reader,
    feature=pooled_output,
    feed_list=feed_list,
    num_classes=dataset.num_labels,
    config=config)
    
run_states = cls_task.finetune_and_eval()

# 预测
data = [[d.text_a] for d in dataset.get_predict_examples()]

run_states = cls_task.predict(data=data)
results = [run_state.run_results for run_state in run_states]
```

## senta[^2]

百度的senta模型

有种模型即软件的味道

对于我这种小白很友好，比ERNIE方便

加载好模型就能用

> CNN, GRU, LSTM, BiLSTM

```python
import paddlehub as hub

# 加载模型
cnn = hub.Module(name='senta_cnn')

# 预测
data_dict = {"text":["你怎么那么好看"]}
results = cnn.sentiment_classify(data = data_dict)
```

## snownlp[^3]

snownlp比较老了，但他的训练集都是电商评论的数据

契合我的主题，所以我也用了

这个用起来也方便

```python
from snownlp import SnowNLP

s = SnowNLP(u'这个东西真心很赞')

s.sentiments    # 输出情感倾向为正面的概率
```

当然，snownlp还能干别的，如分词、繁体转简体、提取关键词...


## 参考资料

[^1]: https://aistudio.baidu.com/aistudio/index
[^2]: https://www.paddlepaddle.org.cn/modelbasedetail/senta
[^3]: https://github.com/isnowfy/snownlp



<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
