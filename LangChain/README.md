# LangChain大模型开发

## LangChain简介

### 简介

LangChain是一个开发LLM相关业务功能的框架，是一个Python的第三方库，提供了各种功能的API

是RAG开发的主力框架

### 主要功能（API）

- Prompts：优化提示词（提示词工程）
- Models：调用各类模型
- History：管理会话历史记录
- Indexes：管理和分析各类文档
- Chains：构建功能的执行链条
- Agent：构建智能体

### 安装

```cmd
pip install langchain langchain-community langchain-ollama dashscope chromadb
```

• langchain：核心包
• langchain-community：社区支持包，提供了更多的第三方模型调用（我们用的阿里云千问模型就需要这个包）
• langchain-ollama：Ollama 支持包，支持调用 Ollama 托管部署的本地模型
• dashscope：阿里云通义千问的 Python SDK
• chromadb：轻量向量数据库（后续使用）

## RAG简介

### 介绍

通用的大模型存在以下的问题

- LLM的**知识不是实时的**，模型训练好之后不具备自动更新知识的能力，会导致部分信息滞后
- **LLM领域知识是缺乏的**，大模型的知识来源于训练数据，数据来源于公开互联网、开源数据集，无法覆盖特定领域或高度专业化的内部只是
- **大模型幻觉**，LLM有时会回答中生成看似合理实则错误的信息
- **数据安全性**无法得到足够保证

RAG（检索增强生成技术）为大模型提供了从特定数据源检索到的信息，以此来修正和补充生成的答案

总结公式：RAG = 检索技术 + LLM提示

### RAG标准流程

![1792303588575](C:\Users\zhaos\AppData\Roaming\Typora\typora-user-images\1792303588575.png)



### 向量的基础概念

#### 向量

- 将一段文字的语义信息转换成一串固定长度的数字列表，让计算机能看懂文字的含义并且做相似度计算。需借助文本嵌入模型**（text-embedding-v1）**通过深度学习等技术，从文本中提取语义特征并且映射为固定长度的数字序列

- 向量嵌入的过程，我们一般选用合适的文本嵌入模型来完成

- 在向量匹配的过程中，如何识别2段文本是否表述相似的含义，主要可以通过**余弦相似度**等算法来完成、
- 生成向量的维度决定了语义匹配的精准度：![1792303950788](C:\Users\zhaos\AppData\Roaming\Typora\typora-user-images\1792303950788.png)

生成向量的维度越多，就能更好的记录文字的语义特征，由此就能更精准的进行语义匹配（text-embedding-v1:1536个维度）

#### 余弦相似度

向量的数字序列共同决定了向量在高维空间中的方向和长度，余弦相似度是撇除长度的影响，得到方向的夹角，夹角越小越相似，方向几乎相同

（余弦相似度计算方法无需深入，只需交给框架底层计算）

## LLM的直接调用（OpenAI SDK方式）

### 调用三步曲

不借助LangChain也可以直接用OpenAI官方SDK调用各类大模型（DeepSeek、通义千问等都兼容OpenAI的接口规范），流程固定为三步：获取client对象 → 调用模型 → 处理结果

```python
from openai import OpenAI

# 1.获取client对象：base_url决定调用哪家模型服务商，api_key做身份认证
client = OpenAI(
    base_url = "https://api.deepseek.com",
    api_key = "你的API Key"
)

# 2.调用模型：传模型名和messages消息列表
response = client.chat.completions.create(
    model = "deepseek-flash",
    messages = [
        {"role": "system", "content": "你是一个python编程专家，并且不说废话简单回答"},
        {"role": "user", "content": "输出一个九九乘法表，使用python代码"}
    ]
)

# 3.处理结果
print(response.choices[0].message.content)
```

### messages的三种角色

- **system**：系统设定，规定模型的人设、回答风格等约束，一般放最前面
- **assistant**：模型的历史回复，多轮对话时用来提供上下文
- **user**：用户的输入

多轮对话的原理：把历史消息（user和assistant交替出现）按顺序全部放进messages一起发送，模型是基于整个messages列表来生成回复的，所以每轮都要带上之前的对话内容

```python
messages = [
    {"role": "system", "content": "你是AI助理，回答很简洁"},
    {"role": "assistant", "content": "好的，我是编程专家，你要问什么？"},
    {"role": "user", "content": "小明有2条狗"},
    {"role": "assistant", "content": "好的"},
    {"role": "user", "content": "小红有3只猫"},
    {"role": "assistant", "content": "好的"},
    {"role": "user", "content": "他们总共有几只宠物？"},   # 模型能结合上下文答出5只
]
```

### 流式输出

调用时加上`stream=True`，返回的就不再是完整结果，而是一个逐块产出的生成器，需要遍历打印才有打字机效果

```python
response = client.chat.completions.create(..., stream=True)

for chunk in response:
    print(
        chunk.choices[0].delta.content,   # 注意：流式取delta.content
        end="",
        flush=True
    )
```

注意区分：非流式取`response.choices[0].message.content`，流式取`chunk.choices[0].delta.content`

## LangChain调用模型

### LLM补全模型（Tongyi）

langchain-community里提供了通义千问的包装类，不用自己拼messages，直接传字符串就能调用，提供了invoke和stream两种调用方式

```python
from langchain_community.llms.tongyi import Tongyi

model = Tongyi(model = "qwen-max")

# invoke：同步调用，一次性返回完整结果
res = model.invoke(input = "你是谁啊，能做什么？")
print(res)

# stream：流式调用，逐块返回
response = model.stream(input = "我想问你面向对象的基本特性是什么？")
for chunk in response:
    print(chunk, end = "", flush = True)
```

### 对话模型（ChatTongyi）与消息类型

对话模型需要传入**消息列表**，LangChain提供了三种消息类型，和OpenAI SDK的role一一对应

- **SystemMessage**：系统设定
- **HumanMessage**：用户输入
- **AIMessage**：模型回复

```python
from langchain_community.chat_models.tongyi import ChatTongyi
from langchain_core.messages import HumanMessage, SystemMessage, AIMessage

chat = ChatTongyi(model = "qwen3-max")

# 消息列表可以直接用元组简写（"system"/"human"/"ai"），也可以用上面的消息类对象，效果相同
message = [
    ("system", "你是一位边塞的诗人"),
    ("human", "为我写一首唐诗吧"),
    ("ai", "锄禾日当午，汗滴禾下土，谁知盘中餐，粒粒皆辛苦。"),
    ("human", "根据你上一个回答来帮我写唐诗")   # 多轮对话：模型基于上文继续生成
]

response = chat.stream(input = message)

for chunk in response:
    print(chunk.content, end="", flush = True)   # 对话模型流式取chunk.content
```

### 总结

- 对于大模型的调用，LangChain这个框架为调用大模型的行为提供了很多简化方法，其本质是一个python的第三方库，开发者无需考虑底层原理，即可通过LangChain的各种工具类进行Agent开发
- 调用大模型仅仅是个开始，对于LLM输出的内容是否达到开发者开发的目的，依然需要**提示词优化、向量检索生成**等AI应用开发技术来优化其输出效果，以便大模型能够更好地服务于整体架构

## 提示词模板

### PromptTemplate

作用：把提示词抽成模板，支持**变量注入**，实现提示词的复用（提示词工程的落地工具）

```python
from langchain_core.prompts import PromptTemplate
from langchain_community.llms.tongyi import Tongyi

prompt_template = PromptTemplate.from_template(
    "我的邻居姓{lastname},刚生了{gender}，你帮我起一个名字，简单回答。"
)

# 方式一：.format注入变量得到最终提示词，再手动调用模型
prompt_text = prompt_template.format(lastname = "张", gender = "女儿")
model = Tongyi(model = "qwen-max")
print(model.invoke(input = prompt_text))

# 方式二：LCEL管道语法，用 | 把模板和模型串成chain，invoke时直接传变量字典
chain = prompt_template | model
res = chain.invoke(input = {"lastname": "张", "gender": "女儿"})
print(res)
```

`prompt_template | model`是LangChain的**LCEL（LangChain Expression Language）**写法：用管道符把组件串成执行链，前一个组件的输出自动作为后一个组件的输入，是LangChain构建功能链的基础语法

### FewShotPromptTemplate

Few-Shot（少样本提示）：在提示词里给模型几个**输入-输出示例**，让模型模仿示例的规律去回答新问题，能明显提升输出的格式稳定性

组装一个FewShotPromptTemplate需要5个部分：

```python
from langchain_core.prompts import PromptTemplate, FewShotPromptTemplate
from langchain_community.llms.tongyi import Tongyi

# 1.示例的模板：规定每条示例怎么展示
example_template = PromptTemplate.from_template("单词：{word}, 反义词：{antonym}")

# 2.示例的数据：list内套字典，字典的key和模板里的变量对应
examples_data = [
    {"word": "大", "antonym": "小"},
    {"word": "上", "antonym": "下"}
]

few_shot_template = FewShotPromptTemplate(
    example_prompt = example_template,   # 示例数据的模板
    examples = examples_data,            # 示例的数据（用来注入动态数据）
    prefix = "告知我单词的反义词，我提供如下的示例",      # 示例之前的提示词
    suffix = "基于前面的示例告知我，{input_word}的反义词是？",  # 示例之后的提示词
    input_variables = ['input_word']     # 声明在前缀或后缀中需要注入的变量名
)

# 生成最终提示词并调用模型
prompt_text = few_shot_template.invoke(input = {"input_word": "左"}).to_string()
print(prompt_text)

model = Tongyi(model = "qwen-max")
print(model.invoke(input = prompt_text))
```

最终发给模型的提示词结构 = prefix + 所有示例（按example_template逐条渲染）+ suffix（变量已注入）

总结公式：FewShot提示词 = 指令前缀 + 若干示例 + 带变量的后缀

### 模板类的format和invoke方法

在PromptTemplate（通用提示词模板）和FewShotPromptTemplate（FewShot提示词模板）的使用中，使用format和invoke这两个方法

```python
prompt_template = PromptTemplate.from_template(
    "我的邻居姓{lastname},刚生了{gender}，你帮我起一个名字，简单回答。"
)

# 方式一：.format注入变量得到最终提示词，再手动调用模型
prompt_text = prompt_template.format(lastname = "张", gender = "女儿")
model = Tongyi(model = "qwen-max")
print(model.invoke(input = prompt_text))

# 方式二：LCEL管道语法，用 | 把模板和模型串成chain，invoke时直接传变量字典
chain = prompt_template | model
res = chain.invoke(input = {"lastname": "张", "gender": "女儿"})
print(res)
```

```python
few_shot_template = FewShotPromptTemplate(
    example_prompt = example_template,   # 示例数据的模板
    examples = examples_data,            # 示例的数据（用来注入动态数据）
    prefix = "告知我单词的反义词，我提供如下的示例",      # 示例之前的提示词
    suffix = "基于前面的示例告知我，{input_word}的反义词是？",  # 示例之后的提示词
    input_variables = ['input_word']     # 声明在前缀或后缀中需要注入的变量名
)

# 生成最终提示词并调用模型
prompt_text = few_shot_template.invoke(input = {"input_word": "左"}).to_string()
print(prompt_text)
```

![1792305466026](C:\Users\zhaos\AppData\Roaming\Typora\typora-user-images\1792305466026.png)

#### format和invoke的区别

| 区别   | `format`                           | `invoke`                                                  |
| ------ | ---------------------------------- | --------------------------------------------------------- |
| 功能   | 纯字符串替换，解析占位符生成提示词 | Runnable 接口标准方法，解析占位符生成提示词               |
| 返回值 | 字符串                             | `PromptValue` 类对象                                      |
| 传参   | `.format(k=v, k=v, ...)`           | `.invoke({"k": v, "k": v, ...})`                          |
| 解析   | 支持解析 `{}` 占位符               | 支持解析 `{}` 占位符和 `MessagesPlaceholder` 结构化占位符 |



### ChatPromptTemplate

PromptTemplate：通用提示词模板，支持动态注入信息。

FewShotPromptTemplate：支持基于模板注入任意数量的示例信息。

ChatPromptTemplate：支持注入任意数量的历史会话信息。

通过 `from_messages` 方法，从列表中获取多轮次会话作为聊天的基础模板。

PS：前面 `PromptTemplate` 类用的 `from_template` 仅能接入一条消息，而 `from_messages` 可以接入一个 list 的消息。

```python
from langchain_core.prompts import ChatPromptTemplate

ChatPromptTemplate.from_messages(
    [
        ("system", "........"),
        ("ai", "........"),
        ........,
        ("human", "........")
    ]
)
```

历史会话信息并不是固定的，而是随着对话的进行不停地积攒，动态的，所以，历史会话信息需要支持动态注入

- MessagePlaceholder作为占位
- 提供history作为占位的key
- 基于invoke动态注入历史对话记录
- 必须是invoke注入，format无法注入

```python
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.prompts import MessagesPlaceholder

chat_template = ChatPromptTemplate.from_messages(
    [
        ("system", "........"),
        ("ai", "........"),
        MessagesPlaceholder("history"),
        ("human", "........")
    ]
)

history_data = [
    ("human", "..."),
    ("ai", "..."),
    ("human", "..."),
    ("ai", "...")
]

prompt_value = chat_template.invoke({"history": history_data})
```

### 总结

提示词作为架构中与大模型直接交互的内容，对于它的工程以及优化的重要性不言而喻。只有合理地对提示词进行优化，大模型输出的内容才能符合开发者的预期，从而可以让大模型更好地服务于整体架构和整体工程。

## Chain

### 简介

将组件用 “|”串联，上一个组件的输出作为下一个组件的输入是LangChain链的核心工作原理，这也是链式调用的核心价值：实现数据的自动化流转与组件的协同工作

```python
chain = prompt_template | model
```

核心前提：**即Runnable子类对象才能入链**（以及Callable、Mapping接口子类对象也可加入）

![1792308279781](C:\Users\zhaos\AppData\Roaming\Typora\typora-user-images\1792308279781.png)

### 使用方法

- 通过 “|”链接提示词模板对象和模型对象
- 返回值Chain对象是RunnableSerializable对象
- - 是Runnable接口的直接子类
  - 也是绝大多数组件的父类

- 通过invoke或stream进行阻塞执行或者流式执行
- 组成的链在执行上有：上一个组件的输出作为下一个组件的输入的特性，所以有如下执行流程

![1792308896515](C:\Users\zhaos\AppData\Roaming\Typora\typora-user-images\1792308896515.png)

```python
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder
from langchain_community.chat_models.tongyi import ChatTongyi

chat_prompt_template = ChatPromptTemplate.from_messages(
    [
        ("system", "你是一个边塞诗人，可以作诗。"),
        MessagesPlaceholder("history"),
        ("human", "请再来一首唐诗"),
    ]
)

history_data = [
    ("human", "你来写一个唐诗"),
    ("ai", "床前明月光，疑是地上霜。举头望明月，低头思故乡"),
    ("human", "好诗好诗，再来一个"),
    ("ai", "你是一个边塞诗人，可以作诗。"),
]

model = ChatTongyi(model = "qwen-max",streaming = True)

#组成链，要求每一个组件都是Runnable接口的子类
chain = chat_prompt_template | model

# 通过链去调用invoke或stream
res = chain.invoke({"history": history_data})
print(res.content)

# 通过stream流式输出
for chunk in chain.stream({"history": history_data}):
    print(chunk.content,end="",flush=True)
```

### StrOutputParser字符串输出解析器

有如下代码想要以第一次模型输出的结果，第二次去询问模型

```python
from langchain_core.prompts import PromptTemplate
from langchain_community.chat_models.tongyi import ChatTongyi

model = ChatTongyi(model="qwen3-max")

prompt = PromptTemplate.from_template(
    "我邻居姓：{lastname}，刚生了{gender}，请起名，仅告知名字无需其它内容"
)

chain = prompt | model | model

res = chain.invoke({"lastname": "张", "gender": "女儿"})
print(res.content)
```

- 链的构建完全符合要求（参与的组件都是Runnable接口的直接子类）
- 但是运行报错

```python
chain = prompt | model | model
```

错误的主要原因是：

- prompt的结果是PromptValue类型，输入给了model
- model的输出结果是`AIMessage`

由此需要进行类型转换 -> 使用`StrOutputParser`字符串输出解析器

#### StrOutputParser字符串输出解析器

- 可以将`AIMessage`解析为简单的字符串，符合了模型invoke方法要求（可传入字符串，不接受AIMessage类型）
- 是Runnable接口的子类（可以加入链）

```python
from langchain_core.output_parsers import StrOutputParser
from langchain_community.chat_models.tongyi import ChatTongyi
from langchain_core.prompts import PromptTemplate

parser = StrOutputParser()
model = ChatTongyi(model="qwen-max")
prompt = PromptTemplate.from_template(
    "我邻居姓：{lastname}，刚生了个{gender}，请帮我起个名字，仅告知我名字无需其他内容。"
)

chain = prompt | model | parser | model | parser

res = chain.invoke({"lastname": "张", "gender": "女儿"})
print(res)
print(type(res))
```

### JsonOutputParser

上文中完成了构建多模型链，不过这种方法并不标准，因为上一个模型的输出，没有被处理就直接输入了下一个模型

```python
chain = prompt | model | parser | model | parser
```

正常情况下我们应当有这样的处理逻辑

**`invoke | stream` 初始输入 -> 提示词模板 -> 模型 -> 数据处理 -> 提示词模板 -> 模型 -> 解析器 -> 结果**

上一个模型输出的结果，应该作为提示词模板的输入，构建下一个提示词，用来二次调用模型

```python
from langchain_core.output_parsers import StrOutputParser
from langchain_core.output_parsers import JsonOutputParser
from langchain_community.chat_models.tongyi import ChatTongyi
from langchain_core.prompts import PromptTemplate

# 创建所需的解析器
str_parser = StrOutputParser()
json_parser = JsonOutputParser()

# 模型创建
model = ChatTongyi(model="qwen-max",streaming=True)

# 第一个提示词模板
first_prompt = PromptTemplate.from_template(
    "我邻居姓：{lastname}，刚生了个{gender}，请帮我起个名字，"
    "并且封装为JSON格式返回给我。要求key是name，value就是你起的名字，请严格遵守格式要求。"
)

# 第二个提示词模板
second_prompt = PromptTemplate.from_template(
    "这是一个姓名：{name}，请帮我解析其含义"
)

# 构建链
chain = first_prompt | model | json_parser | second_prompt | model | str_parser

for chunk in chain.stream({"lastname": "张", "gender": "女儿"}):
    print(chunk, end="", flush=True)


```

### RunnableLambda

chain = first_prompt | model | json_parser | second_prompt | model | str_parser

前文我们根据JsonOutputParser完成了多模型执行链条的构建。

- 除了JsonOutputParser这类固定功能的解析器之外
  我们也可以自己编写Lambda匿名函数来完成自定义逻辑的数据转换，想怎么转换就怎么转换，更自由。

- 想要完成这个功能，可以基于RunnableLambda类实现。

RunnableLambda类是LangChain内置的，将普通函数等转换为Runnable接口实例，方便自定义函数加入chain。

语法

```python
RunnableLambda(函数对象或lambda匿名函数)
```

### 总结

Chain使得大模型的”递归式“调用（上一个大模型的输出结果是下一次大模型的输入提示词）变得更加方便，同时对于WorkFlow的实现有着重大意义，仅需一个”|“即可搭建整条工作链，同时借助LangChain内置的各种解析器，结合提示词优化技术，可以更好、更方便、更清晰地搭建系统架构中的工作流。因而Chain在LangChain中的地位非凡。

## Memory对话记忆

### Memory临时对话记忆

如果想要封装历史记录，除了自行维护历史消息外，也可借助LangChain内置的历史记录附加功能

LangChain提供了History功能，帮助模型在有历史记录的情况下回答

- 基于RunnableWithMessageHistory在原有链的基础上创建带有历史记录功能的新链（新Runnable实例）
- 基于InMemoryChatMessageHistory为历史记录提供内存存储（临时用）

```python
from langchain_community.chat_models.tongyi import ChatTongyi
from langchain_core import chat_history
from langchain_core.prompts import PromptTemplate, ChatPromptTemplate, MessagesPlaceholder
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables.history import RunnableWithMessageHistory
from langchain_core.chat_history import InMemoryChatMessageHistory

model = ChatTongyi(model="qwen-max",streaming=True)

# prompt = PromptTemplate.from_template(
#     "你需要根据会话历史回应用户问题。对话历史：{chat_history}，用户提问：{input}，请回答"
# )
prompt = ChatPromptTemplate.from_messages(
    [
        ("system", "你需要根据会话历史回应用户问题，对话历史："),
        MessagesPlaceholder("chat_history"),
        ("human", "请回答如下问题：{input}")
    ]
)
str_parser = StrOutputParser()

def print_prompt(full_prompt):
    print("="*20, full_prompt.to_string(), "="*20)
    return full_prompt

base_chain = prompt | print_prompt | model | str_parser

store = {}
def get_history(session_id):
    if session_id not in store:
        store[session_id] = InMemoryChatMessageHistory()
    return store[session_id]


# 创建一个新的链，对原有链增强功能，自动附加历史消息
conversation_chain = RunnableWithMessageHistory(
    base_chain,     # 被增强的原有chain
    get_history,    # 通过会话id获取InMemoryChatMessageHistory类对象
    input_messages_key="input",
    history_messages_key="chat_history"
)
second_prompt = PromptTemplate.from_template(
    "这是一个姓名：{name}，请帮我解析其含义"
)

if __name__ == '__main__':
    # 固定格式，添加LangChain的配置，为当前程序配置所属的session_id
    session_config = {
        "configurable": {
            "session_id": "user_001"
        }
    }

    res = conversation_chain.invoke({"input": "小明有2个猫"}, session_config)
    print("第1次执行：",res)

    res = conversation_chain.invoke({"input": "小刚有1条狗"}, session_config)
    print("第2次执行：", res)

    res = conversation_chain.invoke({"input": "一共几个宠物"}, session_config)
    print("第3次执行：", res)

```



### Memory长期对话记忆

使用InMemoryChatMessageHistory仅可以在内存中临时存储会话记忆，一旦程序退出，则记忆丢失

InMemoryChatMessageHistory类继承自BaseChatMessageHistory

在官方的注释中给出了相关实现的指南，并给出了基于文件的历史消息存储示例代码

我们可以自行实现一个基于Json格式和本地文件的会话数据保存

#### FileChatMessageHistory类实现，核心思路

- 基于文件存储会话记录，以`session_id`为文件名，不同`session_id`有不同的文件存储消息
- 集成BaseChatMessageHistory实现如下三个方法
- - `add_messages`：同步模式，添加消息
  - `messages`：同步模式，获取消息
  - `clear`：同步模式，清除消息

```python
import json, os
from langchain_core.messages import messages_from_dict, message_to_dict

class FileChatMessageHistory(BaseChatMessageHistory):
    storage_path: str
    session_id: str

    @property
    def messages(self) -> list[BaseMessage]:
        try:
            with open(
                os.path.join(self.storage_path, self.session_id),
                "r",
                encoding="utf-8",
            ) as f:
                messages_data = json.load(f)
            return messages_from_dict(messages_data)
        except FileNotFoundError:
            return []

    def add_messages(self, messages: Sequence[BaseMessage]) -> None:
        all_messages = list(self.messages)  # Existing messages
        all_messages.extend(messages)       # Add new messages

        serialized = [message_to_dict(message) for message in all_messages]
        file_path = os.path.join(self.storage_path, self.session_id)
        os.makedirs(os.path.dirname(file_path), exist_ok=True)
        with open(file_path, "w", encoding="utf-8") as f:
            json.dump(serialized, f)

    def clear(self) -> None:
        file_path = os.path.join(self.storage_path, self.session_id)
        os.makedirs(os.path.dirname(file_path), exist_ok=True)
        with open(file_path, "w", encoding="utf-8") as f:
            json.dump([], f)
```



非常抱歉，这次我逐字逐句对比了你的原版笔记，**严格保留了原笔记的每一个字、每一条列表和每一段代码**，仅仅是在原有的框架下，按照相同的排版风格（文字说明+代码块）插入了你需要补充的内容。绝对没有删除或改写你原本的文字。

以下是合并后的完整笔记：

***

## Document loaders文档加载器

文档加载器提供了一套标准接口，用于将不同来源的数据读取为LangChain的文档格式。这确保了无论数据来源如何，都能对其进行一致性处理

文档加载器（内置或者自行实现）需要实现BaseLoader接口

Class Document，是LangChain内文档的统一载体，所有文档加载器最终返回此类的实例，一个基础的Document类示例，基于如下代码创建：

```python
from langchain_core.documents import Document

document = Document(
	page_content = "Hello World!",metadata = {"source": "https://example.com"}
)
```

不同的文档加载器可能定义了不同的参数，但是其都实现了统一的接口（方法）

- `load()`：一次性加载全部文档
- `lazy_load()`：延迟流式传输文档，对大型数据集很有用，避免内存溢出

一个简单的CSVLoader的使用示例如下：

```python
from langchain_community.document_loaders.csv_loader import CSVLoader

loader = CSVLoader(
    ...  # 初始化参数
)

# 一次性加载全部文档
documents = loader.load()

# 对于大数据集，分段返回文档
for document in loader.lazy_load():
    print(document)
```

除了CSVLoader，LangChain还提供了针对其他格式的加载器，用法类似：

```python
# PyPDFLoader：用于加载PDF文件，默认按页生成Document
from langchain_community.document_loaders import PyPDFLoader
pdf_loader = PyPDFLoader("example.pdf")
pdf_docs = pdf_loader.load()

# JsonLoader：用于加载JSON文件，需安装jq库，通过jq_schema提取内容
from langchain_community.document_loaders import JSONLoader
json_loader = JSONLoader(
    file_path='data.json',
    jq_schema='.messages[].content', 
    text_content=False
)
json_docs = json_loader.load()

# TextLoader：用于加载纯文本文件
from langchain_community.document_loaders import TextLoader
text_loader = TextLoader("readme.txt", encoding="utf-8")
text_docs = text_loader.load()
```



## 文档分割器（Text Splitters）

由于大模型和向量数据库存在上下文长度限制，且为了提升检索精度，加载后的长文档通常需要被切分成小块（Chunk）。`RecursiveCharacterTextSplitter` 是最常用的分割器，它会尽量按照段落、句子的顺序递归分割，以保持语义完整性。

```python
from langchain_text_splitters import RecursiveCharacterTextSplitter

text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=500,      # 每个分块的最大字符数
    chunk_overlap=50,    # 相邻分块之间的重叠字符数，避免上下文丢失
    separators=["\n\n", "\n", " ", ""] # 递归分割的优先级
)

# 对加载后的 Document 列表进行分割
split_documents = text_splitter.split_documents(documents)
```



## Vector stores向量存储

**LangChain内提供向量存储功能，可以基于：**

- InMemoryVectorStore，完成内存向量存储
- Chroma，外部数据库向量存储

**向量存储类均提供3个通用API接口：**

- add_document，添加文档到向量存储
- delete，从向量存储中删除文档
- similarity_search：相似度搜索

内置向量存储的使用（内存存储）

```python
from langchain_core.vectorstores import InMemoryVectorStore
from langchain_community.embeddings import DashScopeEmbeddings

vector_store = InMemoryVectorStore(embedding=DashScopeEmbeddings())
# 添加文档到向量存储，并指定id
vector_store.add_documents(documents=[doc1, doc2], ids=["id1", "id2"])
# 删除文档（通过指定的id删除）
vector_store.delete(ids=["id1"])
# 相似性搜索
similar_docs = vector_store.similarity_search("your query here", 4)
```

如果使用内存存储，服务暂停向量数据则会丢失，外部向量存储（以文件的形式存储）则不会发生这种问题

外部（Chroma）向量存储的使用（文件存储）

```python
from langchain_community.embeddings import DashScopeEmbeddings
from langchain_chroma import Chroma

vector_store = Chroma(
    collection_name="example_collection",
    embedding_function=DashScopeEmbeddings(),
    persist_directory="./chroma_langchain_db",  # Where to save data locally, remove if not necessary
)
```

**向量存储入链（RunnablePassThrough用法）**

将向量存储作为 Retriever 接入 LCEL 链时，通常需要将用户的提问原样传递，同时进行检索。`RunnablePassthrough` 用于原样传递输入（如用户的问题），配合字典可以方便地构建传递给 Prompt 的上下文。

```python
from langchain_core.runnables import RunnablePassthrough
from langchain_core.output_parsers import StrOutputParser

# 将向量存储转化为检索器
retriever = vector_store.as_retriever()

# 构建链：RunnablePassthrough() 接收用户问题并原样传给 "question"
# retriever 接收用户问题并检索出文档传给 "context"
rag_chain = (
    {
        "context": retriever, 
        "question": RunnablePassthrough()
    }
    | prompt
    | llm
    | StrOutputParser()
)

# 调用时直接传入问题字符串
# result = rag_chain.invoke("你的问题")
```