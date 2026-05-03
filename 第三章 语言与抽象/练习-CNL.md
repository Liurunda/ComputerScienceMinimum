# 练习 1 — 设计一种用于 AI 编程的"受控自然语言"

Exercise 1 — Design a "Restricted Natural Language" for AI Programming

本章结尾提出了一个核心命题：如果"好的自然语言用法能够被总结成一些规则"，我们就发明了一种新的"用于 AI 编程的语言"。这个练习就是让你亲手试一试。

The chapter ends with a core proposition: if "good natural language usage patterns can be summarized into a set of rules," we have invented a new kind of "language for AI programming." This exercise lets you try it yourself.

---

**(a) 收集好用法与坏用法**

**(a) Collect good usage and bad usage**

用一个支持自然语言编程的 AI 工具（如 Claude Code、Cursor、GitHub Copilot 等），完成以下三个编程任务各两次——一次用你直觉认为"好"的描述方式，一次用你直觉认为"差"的描述方式，记录两次生成代码的质量差异。

Use an AI tool that supports natural language programming (e.g., Claude Code, Cursor, GitHub Copilot, etc.). Complete each of the following three programming tasks twice — once using what you intuitively feel is a "good" description, once using what you feel is "bad." Record the quality difference between the two generated outputs.

三个任务（任选或全部）：

Three tasks (choose any or all):

- 实现一个具有特定行为的数据结构（如"一个支持 O(1) 查找最近插入的 100 个元素的缓存"）
- 为一个函数写完整的单元测试
- 重构一段已有的、可读性较差的代码

Implement a data structure with specific behavior (e.g., "a cache that supports O(1) lookup of the 100 most recently inserted elements"); Write complete unit tests for a function; Refactor an existing piece of poorly readable code.

对于每次尝试，记录：你的 prompt 原文；AI 生成的代码是否一次就正确？如果不是，哪里出错了？你如何修改 prompt 来修正错误？

For each attempt, record: Your original prompt text; Was the AI-generated code correct on the first try? If not, what went wrong?; How did you modify the prompt to fix the error?

---

**(b) 提炼规则**

**(b) Extract rules**

基于 (a) 中的实验，总结出至少 5 条"让 AI 生成好代码"的语言规则。每条规则应该是一个可操作的约束——是对自然语言用法的限制（restriction），而不是泛泛的建议。

Based on your experiments in (a), extract at least 5 language rules that "make AI generate good code." Each rule should be an actionable constraint — a restriction on natural language usage, not a vague suggestion.

*示例（供参考，不要照抄）：规则 1：每个函数需求的描述必须以"给定...当...则..."的结构书写；规则 2：涉及数据结构的描述必须显式声明期望的时间复杂度。*

*Example (for reference, do not copy): Rule 1: Every function requirement description must be written in the "Given... When... Then..." structure; Rule 2: Descriptions involving data structures must explicitly declare the expected time complexity.*

对每条规则，解释它**约束了什么**（它防止了哪种自然语言的模糊性？）以及**牺牲了什么**（它让你不能说什么？）。

For each rule, explain what it constrains (what kind of natural language ambiguity does it prevent?) and what it sacrifices (what can you no longer say?).

---

**(c) 测试你的 CNL**

**(c) Test your CNL**

用你设计的那套规则（你的"受控自然语言"），重新完成 (a) 中的三个任务。这一次，严格遵守你自己制定的每一条规则。记录：与第一次自由描述相比，代码质量的提升（或下降）；是否出现了规则"不够用"的情况——你受限于规则无法精确表达某个需求？是否有规则实际上阻碍了 AI 生成更好的代码？

Using your set of rules (your "Restricted Natural Language" or CNL), redo the three tasks from (a). This time, strictly follow every rule you've established. Record: Compared to the first free-form description, the improvement (or decline) in code quality; Were there situations where the rules were "insufficient" — you couldn't precisely express a requirement due to the constraints?; Were there rules that actually hindered the AI from generating better code?

---

**(d) 反思：CNL 的边界**

**(d) Reflection: the boundaries of CNL**

你的 CNL 必然在某些任务上表现好，在某些任务上表现差。回答：你的 CNL 最适合哪类编程任务？最不适合哪类？如果将你的 CNL 与另一个同学的 CNL 做"互操作"——用你的 CNL 写的 prompt 放到ta的 AI 上执行——会出什么问题？这和你本章学到的"黑话"现象有什么关系？你的 CNL 是有损压缩还是无损压缩？即：经过你的 CNL 约束后，是否有些合法的、值得实现的需求被"压掉了"？

Your CNL inevitably performs well on some tasks and poorly on others. Answer: What type of programming tasks is your CNL best suited for? Worst suited for?; If you "interoperate" your CNL with another student's CNL — running a prompt written in your CNL on their AI — what problems would arise? How does this relate to the "jargon" phenomenon discussed in this chapter?; Is your CNL lossy or lossless compression? That is, after being constrained by your CNL, are there legitimate, worthwhile requirements that have been "compressed away"?
