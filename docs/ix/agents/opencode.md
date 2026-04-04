在了解opencode之前，先理解什么是AI Agent 

AI Agent = 大模型（思考） + 工具（执行） + 记忆（上下文） + 目标（驱动）


> 人们常说的大语言模型/大模型（chagpt/gemini/qwen……），其实就是Large Language Model (LLM) 
> 
> 在执行任务的时候，AI Agent 会调用os的API来触发行为动作
> 
> 就像用户在与llm对话的时候，Human-LLM Context是非常关键的边界和范围，而human与AI Agent 交互也存在Human-Agent Context
> 
> 而人类input的目标需求就是驱动整个AI Agent 完成任务的触发器。

当前讨论火热的claude code, LangChain, openclaw都是基于 AI Agent 概念的泛化产品。AI Agent 代表了人工智能从“内容生成”向“任务执行”的关键转变，它正成为连接人类意图与数字世界行动的桥梁。

简而言之，从最开始的human-llm交互，变成了human-agent交互。

> 本身我是打算使用claude code & openrouter提供的免费llm key来演示，但是2026-04-04发布更新，似乎禁止了第三方llm model的接入，打算要求用户使用其claude llm或其规定范围内的llm model，因此使用opencode演示

*website* https://opencode.ai/

local machine是Windows os，因此我选择了npm下载

![opencode](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/intelligence/opencode.png)

通过`opencode`启动即可，其默认配置了Big Pickle（免费）的llm，也可通过`/model`自己选择

![opencode](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/intelligence/oc-model.png)