# 笔记-ChatGPT Prompt Engineering for Developers


课程笔记- ***《ChatGPT Prompt Engineering for Developers》***

<!--more-->



## L1-Introduction

目的、定位：使用 API 调用到 LLM，以快速构建软件应用程序。

随着大型语言模型（LLM）的发展，LLM 大致可以分为两种类型，即基础LLM和指令微调LLM。基础LLM是基于文本训练数据，训练出预测下一个单词能力的模型，其通常是在互联网和其他来源的大量数据上训练的。例如，如果你以“从前有一只独角兽”作为提示，基础LLM可能会继续预测“生活在一个与所有独角兽朋友的神奇森林中”。但是，如果你以“法国的首都是什么”为提示，则基础LLM可能会根据互联网上的文章，将答案预测为“法国最大的城市是什么？法国的人口是多少？”，因为互联网上的文章很可能是有关法国国家的问答题目列表。

许多 LLMs 的研究和实践的动力正在指令调整的 LLMs 上。指令调整的 LLMs 已经被训练来遵循指令。因此，如果你问它，“法国的首都是什么？”，它更有可能输出“法国的首都是巴黎”。指令调整的 LLMs 的训练通常是从已经训练好的基本 LLMs 开始，该模型已经在大量文本数据上进行了训练。然后，使用输入是指令、输出是其应该返回的结果的数据集来对其进行微调，要求它遵循这些指令。然后通常使用一种称为 RLHF（reinforcement learning from human feedback，人类反馈强化学习）的技术进行进一步改进，使系统更能够有帮助地遵循指令。

![Untitled](ChatGPT%20Prompt%20Engineering%20for%20Developers%20c9153ee72958474f8788a56ad8d03ef0/Untitled.png)

## L2-Guidelines

### Setup

本教程使用 OpenAI 所开放的 ChatGPT API，因此你需要首先拥有一个 ChatGPT 的 API_KEY

openai: `pip install openai`

dotenv: `pip install -U python-dotenv`

### Prompting Principles

#### Principle 1: Write clear and specific instructions

- Tactic 1: Use delimiters to clearly indicate distinct parts of the input
    
    Delimiters can be anything like: ```, """, < >, `<tag> </tag>`, `:`
    
- Tactic 2: Ask for a structured output
    
    JSON, HTML
    
- Tactic 3: Ask the model to check whether conditions are satisfied
    - 如果任务做出的假设不一定满足，我们可以告诉模型先检查这些假设，如果不满足，指示并停止执行。你还可以考虑潜在的边缘情况以及模型应该如何处理它们，以避免意外的错误或结果。
        
        ![Untitled](ChatGPT%20Prompt%20Engineering%20for%20Developers%20c9153ee72958474f8788a56ad8d03ef0/Untitled%201.png)
        
- Tactic 4: "Few-shot" prompting
    - 即在要求模型执行实际任务之前，提供给它少量成功执行任务的示例。
        
        ![Untitled](ChatGPT%20Prompt%20Engineering%20for%20Developers%20c9153ee72958474f8788a56ad8d03ef0/Untitled%202.png)
        

#### Principle 2: Give the model time to “think”

- Tactic 1: Specify the steps required to complete a task
    
    相当于告诉小弟为了实现目标A，拆解到1、2、3分别是啥结果，最后汇总以实现目标A
    
    （这也衍生出**问题的拆解、设计能力**目前还算是人类的核心竞争力）
    
- Tactic 2: Instruct the model to work out its own solution before rushing to a conclusion
    
    在明确指导模型在做决策之前要思考解决方案时，我们会得到更好的结果。
    

### Model Limitations: Hallucinations

如果模型在训练过程中接触了大量的知识，它并没有完全记住所见的信息，因此它并不很清楚自己知识的边界。这意味着它可能会尝试回答有关晦涩主题的问题，并编造听起来合理但实际上并不正确的答案。我们称这些编造的想法为幻觉。

If the model is being exposed to a vast amount of knowledge during its training process, it has not perfectly memorised the information it's seen, and so it doesn't know **the boundary of its knowledge** very well. This means that it might try to answer questions about obscure topics and can make things up that sound plausible but are not actually true. And we call these fabricated ideas hallucinations.

要求模型找到文本中的任何相关引用，然后要求它使用这些引用来回答问题，这种追溯源文档的方法通常对减少幻觉非常有帮助。（比如现在的 New Bing）

## L3-Iterative

需要好的迭代过程来不断改进 Prompt。

类比机器学习开发的流程。通常是先有一个想法，然后再实现它：编写代码，获取数据，训练模型，这会给您一个实验结果。然后您可以查看输出结果，进行错误分析，找出它在哪里起作用或不起作用，甚至可以更改您想要解决的问题的确切思路或方法，然后更改实现并运行另一个实验等等，反复迭代，以获得有效的机器学习模型。

![Untitled](ChatGPT%20Prompt%20Engineering%20for%20Developers%20c9153ee72958474f8788a56ad8d03ef0/Untitled%203.png)

🌰 generate marketing copy from a product fact sheet

## L4-**Summarizing**

文本概括 + 关键信息提取

- Python调用接口
    
    ```python
    import openai
    import os
    OPENAI_API_KEY = os.environ.get("OPENAI_API_KEY")
    openai.api_key = OPENAI_API_KEY
    
    def get_completion(prompt, model="gpt-3.5-turbo"): 
        messages = [{"role": "user", "content": prompt}]
        response = openai.ChatCompletion.create(
            model=model,
            messages=messages,
            temperature=0, # 值越低则输出文本随机性越低
        )
        return response.choices[0].message["content"]
    ```
    

## L5-Inferring

「推断」infer sentiment and topics from product reviews and news articles.

- 🌰 情感分析、类型判断、信息提取等task
    
    ![Untitled](ChatGPT%20Prompt%20Engineering%20for%20Developers%20c9153ee72958474f8788a56ad8d03ef0/Untitled%204.png)
    

也举了个是否match特定主题的例子 ⇒ 想想咋变现，体现人类善于利用工具的点

## L6-Transforming

use Large Language Models for text transformation tasks such as language translation, spelling and grammar checking, tone adjustment, and format conversion.

- 🌰 **文本翻译+拼写纠正+风格调整+格式转换**
    
    ```
    对以下```之间的英文评论文本，进行以下操作：
    （1）针对拼写及语法纠错；
    （2）将其转化成中文；
    （3）将翻译后的中文文本转化成优质淘宝评论的风格，从各种角度出发，分别说明产品的优点与缺点，并进行总结。同时润色一下描述，使评论更具有吸引力。
    第3个任务输出结果格式为：
    【优点】xxx
    【缺点】xxx
    【总结】xxx
    注意，只需填写xxx部分，并分段以markdown格式输出。
    ```
    Got this for my daughter for her birthday cuz she keeps taking \
    mine from my room.  Yes, adults also like pandas too.  She takes \
    it everywhere with her, and it's super soft and cute.  One of the \
    ears is a bit lower than the other, and I don't think that was \
    designed to be asymmetrical. It's a bit small for what I paid for it \
    though. I think there might be other options that are bigger for \
    the same price.  It arrived a day earlier than expected, so I got \
    to play with it myself before I gave it to my daughter.
    ```
    ```
    
    ![Untitled](ChatGPT%20Prompt%20Engineering%20for%20Developers%20c9153ee72958474f8788a56ad8d03ef0/Untitled%205.png)
    

## L7-Expanding

调用接口的时候可以通过设置 `temperature` 参数以决定response的多样性

![Untitled](ChatGPT%20Prompt%20Engineering%20for%20Developers%20c9153ee72958474f8788a56ad8d03ef0/Untitled%206.png)

## L8-Chatbot

- 关于 **role** 的介绍与说明
    
    `system`、`assistant`、`user` 
    
    角色的设定更多是面向开发者。webUI交互时可以说扮演xxx角色
    
    ![Untitled](ChatGPT%20Prompt%20Engineering%20for%20Developers%20c9153ee72958474f8788a56ad8d03ef0/Untitled%207.png)
    

### OrderBot

We can automate the collection of user prompts and assistant responses to build a OrderBot. The OrderBot will take orders at a pizza restaurant.

- 基于notebook的交互代码
    
    ```python
    import os
    import openai
    from dotenv import load_dotenv, find_dotenv
    _ = load_dotenv(find_dotenv()) # read local .env file
    
    openai.api_key  = os.getenv('OPENAI_API_KEY')
    
    def get_completion(prompt, model="gpt-3.5-turbo"):
        messages = [{"role": "user", "content": prompt}]
        response = openai.ChatCompletion.create(
            model=model,
            messages=messages,
            temperature=0, # this is the degree of randomness of the model's output
        )
        return response.choices[0].message["content"]
    
    def get_completion_from_messages(messages, model="gpt-3.5-turbo", temperature=0):
        response = openai.ChatCompletion.create(
            model=model,
            messages=messages,
            temperature=temperature, # this is the degree of randomness of the model's output
        )
    #     print(str(response.choices[0].message))
        return response.choices[0].message["content"]
    
    import panel as pn  # GUI
    pn.extension()
    
    panels = [] # collect display 
    
    context = [ {'role':'system', 'content':"""
    You are OrderBot, an automated service to collect orders for a pizza restaurant. \
    You first greet the customer, then collects the order, \
    and then asks if it's a pickup or delivery. \
    You wait to collect the entire order, then summarize it and check for a final \
    time if the customer wants to add anything else. \
    If it's a delivery, you ask for an address. \
    Finally you collect the payment.\
    Make sure to clarify all options, extras and sizes to uniquely \
    identify the item from the menu.\
    You respond in a short, very conversational friendly style. \
    The menu includes \
    pepperoni pizza  12.95, 10.00, 7.00 \
    cheese pizza   10.95, 9.25, 6.50 \
    eggplant pizza   11.95, 9.75, 6.75 \
    fries 4.50, 3.50 \
    greek salad 7.25 \
    Toppings: \
    extra cheese 2.00, \
    mushrooms 1.50 \
    sausage 3.00 \
    canadian bacon 3.50 \
    AI sauce 1.50 \
    peppers 1.00 \
    Drinks: \
    coke 3.00, 2.00, 1.00 \
    sprite 3.00, 2.00, 1.00 \
    bottled water 5.00 \
    """} ]  # accumulate messages
    
    inp = pn.widgets.TextInput(value="Hi", placeholder='Enter text here…')
    button_conversation = pn.widgets.Button(name="Chat!")
    
    def collect_messages(_):
        prompt = inp.value_input
        inp.value = ''
        context.append({'role':'user', 'content':f"{prompt}"})
        response = get_completion_from_messages(context) 
        context.append({'role':'assistant', 'content':f"{response}"})
        panels.append(
            pn.Row('User:', pn.pane.Markdown(prompt, width=600)))
        panels.append(
            pn.Row('Assistant:', pn.pane.Markdown(response, width=600, style={'background-color': '#F6F6F6'})))
     
        return pn.Column(*panels)
    
    interactive_conversation = pn.bind(collect_messages, button_conversation)
    
    dashboard = pn.Column(
        inp,
        pn.Row(button_conversation),
        pn.panel(interactive_conversation, loading_indicator=True, height=300),
    )
    
    dashboard
    ```
    
    样式形如👇
    
    ![Untitled](ChatGPT%20Prompt%20Engineering%20for%20Developers%20c9153ee72958474f8788a56ad8d03ef0/Untitled%208.png)
    

## Reference

[ChatGPT Prompt Engineering for Developers - DeepLearning.AI](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/)

[https://github.com/datawhalechina/prompt-engineering-for-developers](https://github.com/datawhalechina/prompt-engineering-for-developers)



<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
