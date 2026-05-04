# 计算科学简论

Computing Science Minimum

## 动机 Motivation

如果整个计算机科学的知识体系，都能够通过少量的基本原则，被一个大语言模型填充出来，那么，究竟哪些内容，是必不可少的、删无可删的？

If most computer science knowledge could be generated from some basic principles by an LLM, then what is the minimum / the necessary?

## 定位 Position

本书帮助读者进行至少80年的、关于计算的思考和玩耍，从而在一生中不感到无聊。

This book helps readers think about and play with computing for at least 80 years, thus saving their life from boredness.

深入掌握本书内容的读者，将能胜任各种和计算相关的工程与研究工作。但这并非本书的优先目的，而是任何追寻智识乐趣和深入思考的活动，都具有的一种附带好处。

Readers who understand this book would be competent for all kinds of engineer and research jobs related to computing. However, that is not the primary goal of this book. Instead, that is a collateral benefit of any activity pursuing intelligent joyfulness and deep thinking.

## 前置条件 Prerequisites

闲暇 Leisure

好奇心 Curiosity

基础的中文或英文 Basic Chinese or English Literacy

中学数学和物理 High School Math and Physics


## 风格 Style

我对标准英语的语法做了优化, 使其在认知负载和信息密度上更接近中文。我也建议，使用“中文-中式英语”双语写作，作为未来的标准学术语言。

About the style and syntax: I optimized the standard English in the sense of cognitive load and information density, making it more similar to Chinese. I also suggest duolingual Chinese-Chinglish writing as the future standard academic language.

如果你想要一份“标准英语”的版本，让大语言模型帮你生成一份就可以了。我没有义务为你做这个事情。

If you want a "Standard English" version, just ask any LLM to generate it for you. I bear no responsibility to do that for you.

## 工具 Tools

推荐使用 VS Code - claude code - DeepSeek-V4 作为学习本讲义的工具链。

最好还有一个Unix-like的终端（linux或Mac OS的原生终端，或Windows系统下的WSL），因为claude code通常在Unix-like的终端下表现更好。

这也是写作本讲义时主要使用的工具链(除了少量的 Gemini-3.1-pro)。

一个判断：使用AI辅助学习时，“世界知识”是模型能力最重要的维度。但Gemini-3.1-pro的价格昂贵，难以用于AI辅助学习时所消耗的大量token。而DeepSeek-V4的价格足够亲民，使得大多数人能够负担“借助AI消耗大量token辅助学习”这件事情。

对于没有相关基础的同学来说，配置环境需要解决很多具体的技术问题。

建议和一个对话框形态的AI助手共同解决这些问题，阅读文档，在自己的电脑上尝试，告诉对话框形态的AI助手自己尝试了什么、发现了什么现象，然后执行下一步。这样，你和对话框形态的AI助手共同组成了一个agentic AI系统。

一些配置环境相关的文档：

[为Claude Code配置DeepSeek模型](https://api-docs.deepseek.com/zh-cn/quick_start/agent_integrations/claude_code )

[WSL和VS Code](https://learn.microsoft.com/zh-cn/windows/wsl/tutorials/wsl-vscode)

