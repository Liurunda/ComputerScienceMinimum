# 练习 — 渐进 Token 复杂度：LLM 时代的"时间复杂度"

Exercise — Asymptotic Token Complexity: "Time Complexity" in the Age of LLMs

第零章让你亲手发明了渐进时间复杂度——用 T(n) 在 n 翻倍时的行为，来预测大规模运行的成本。在那个世界里，n 是输入数据的规模，"运行时间"是你要预估的代价。

Chapter 0 had you invent asymptotic time complexity — using T(n)'s behavior as n doubles to predict large-scale cost. In that world, n is the input data size, and "runtime" is the cost you need to estimate.

现在换一个世界。你要用一个大语言模型（LLM）解决一个任务。你消耗的不是 CPU 周期——是 **token**。每一次调用模型，你发送的 prompt token 和收到的 completion token，加起来就是这次交互的成本。在这个世界里——**是否存在一个 "渐进 Token 复杂度" 的概念？** 如果存在，它的 n 是什么？它的"增长阶"由什么决定？

Now switch worlds. You need to solve a task using a large language model (LLM). What you consume is not CPU cycles — it's **tokens**. Each model call, your prompt tokens plus the completion tokens you receive — that's the cost of this interaction. In this world — **does a concept of "asymptotic token complexity" exist?** If it does, what is its n? What determines its "order of growth"?

本练习没有标准答案——因为这个概念尚不存在。你要做的，是用第零章教给你的渐进思维，去**试着定义它**。

This exercise has no standard answer — because the concept doesn't exist yet. What you'll do is use the asymptotic thinking Chapter 0 taught you to **try to define it.**

---

**(1)** 先从最简单的情况开始。你有一个长度为 n 的英文文本。你要让 LLM 做以下三件事之一：

Start with the simplest case. You have an English text of length n. You ask the LLM to do one of three things:

- **任务 A**：用一句话总结这篇文本（summarization）。
- **任务 B**：将这篇文本从英文翻译成中文（translation）。
- **任务 C**：回答"在这篇文本中，角色 X 对角色 Y 的情感经历了怎样的变化？请引用原文中的具体段落支撑你的分析。"

对于任务 A 和 B，输入 token 数量大致和 n 成正比——你几乎要把全文放进 prompt。输出 token 数量呢？对于任务 A，输出（一句话总结）和 n 是什么关系？任务 B（翻译后的中文文本）呢？任务 C 呢？

For tasks A and B, input tokens are roughly proportional to n — you must put nearly the full text in the prompt. What about output tokens? For task A, what is the relationship between the output (a one-sentence summary) and n? Task B (the translated Chinese text)? Task C?

**(a)** 对三个任务分别画出"你猜测的 token 总量（输入+输出）随 n 增长"的草图。不要算精确数字——用手画三条曲线，标注每条曲线的"形状直觉"（线性？次线性？超线性？）。

**(b)** 任务 C 中有一个循环：你需要从文本中找到相关段落 → 引用 → 分析 → 可能需要返回文本再找更多段落。这种"可能需要回去再看一眼"的行为，在传统的渐进时间复杂度中对应什么？在第零章的 smoke test 场景中——任务 C 的 token 消耗是否比任务 A 更难从"小 n 实验"外推到大 n？

**(a)** For each of the three tasks, sketch "your guessed total token count (input + output) as n grows." Don't compute exact numbers — hand-draw three curves, labeling each with its "shape intuition" (linear? sublinear? superlinear?).

**(b)** Task C contains a loop: you need to find relevant passages → quote → analyze → possibly return to the text for more passages. In traditional asymptotic time complexity, what does this "might need to go back and look again" behavior correspond to? In the Chapter 0 smoke test scenario — is Task C's token consumption harder to extrapolate from "small n experiments" to large n than Task A's?

*提示：回忆练习-渐进时间复杂度第 (4) 问——缓存断崖。LLM 的上下文窗口是否也有类似的"断崖"？当 n 超过模型的上下文窗口长度时——会发生什么？Hint: recall question (4) from the asymptotics exercise — the cache cliff. Does the LLM's context window have a similar "cliff"? When n exceeds the model's context window length — what happens?*

---

**(2)** 在传统时间复杂度中，n 是"输入数据的规模"。但在 LLM 任务中——考虑以下三种情况：

In traditional time complexity, n is "the size of the input data." But in LLM tasks — consider these three cases:

- **情况 1**：给模型一段 10 页的法律合同，让它找出所有包含"赔偿责任上限超过 $1,000,000"的条款。n = 页数。
- **情况 2**：给模型同样的 10 页合同，但这次让它判断"这份合同的整体风险倾向对客户是否公平，并列举至少三个理由"。n = 页数——但难度不同。
- **情况 3**：你收到一个陌生的 Python 代码仓库，共 50 个文件，总计约 10,000 行代码。你让模型"找出其中可能导致 SQL 注入的所有代码位置"。

**(a)** 在情况 1 和情况 2 中，它们的 n（输入长度）相同。它们的 token 消耗可能相差多大？导致差异的那个因素——如果它不是 n——应该叫什么？

**(b)** 情况 3 有一个隐藏的放大因子：50 个文件。每多一个文件，不仅增加了代码行数（n ↑），而且需要额外的 token 来理解"这个文件在这个仓库中的角色"（文件之间的调用关系、import 依赖等）。**当 n 既代表代码行数、又隐含着文件间的依赖复杂度时——"渐进 Token 复杂度"的 O 表达式中是否应该出现不只一个变量？** 如果是，直觉上哪些变量是主导项？哪些在 n 变大时被洗掉？

**(a)** In Case 1 and Case 2, their n (input length) is the same. How much could their token consumption differ? The factor causing that difference — if it's not n — what should it be called?

**(b)** Case 3 has a hidden amplification factor: 50 files. Each additional file not only increases the lines of code (n ↑), but requires extra tokens to understand "that file's role in the repository" (call relationships, import dependencies, etc.). **When n represents both lines of code AND implicitly encodes inter-file dependency complexity — should the O expression of "asymptotic token complexity" contain more than one variable?** If so, intuitively, which variables dominate? Which get washed out as n grows large?

*提示：回顾第三章——"语义"。一个文件的含义，不是它的函数名和行数的总和。它在仓库中的**位置**——"谁调用它"和"它调用谁"——本身就是信息。这个信息和代码行数之间是否有某种——类似第六章层级结构中的——比例关系？Hint: recall Chapter 3 — "semantics." The meaning of a file is not the sum of its function names and line count. Its **position** in the repository — "who calls it" and "whom it calls" — is itself information. Between this information and line count — is there some proportional relationship — similar to the hierarchical structures in Chapter 6?*

---

**(3)** 现在把"模型本身"放进去。同一个任务，同一个 n，不同的模型——token 消耗是否不同？

Now put "the model itself" into the equation. Same task, same n, different models — does token consumption differ?

**(a)** 请实际做一个小实验（可以用 ChatGPT、Claude 或任何你能访问的模型）。用完全相同的 prompt 问两个不同的模型："请用 Python 实现一个红黑树，包含插入和删除操作。代码写完后，请用 3 个测试用例验证正确性。" **记录每个模型消耗的 completion token 数量**（在 ChatGPT 的 playground 或 Claude 的 API response 中可以直接看到 `usage.completion_tokens`）。两个模型的 completion token 数量是否相同？如果不同——可能的原因是什么？

**(a)** Do a small real experiment (use ChatGPT, Claude, or any model you can access). Use the exact same prompt on two different models: "Please implement a red-black tree in Python, including insertion and deletion operations. After writing the code, verify correctness with 3 test cases." **Record the completion token count for each model** (visible as `usage.completion_tokens` in ChatGPT's playground or Claude's API response). Are the two models' completion token counts the same? If different — what might explain the difference?

**(b)** 以下三个因素中，哪些可能影响同一个任务在不同模型之间的 token 消耗差异？请对每一个给出你的直觉判断（是/否/不确定），并用一句话解释：

Among the following three factors, which could affect token consumption differences across models for the same task? For each, give your intuitive judgment (yes/no/uncertain) with a one-sentence explanation:

- **Tokenizer 不同**："红黑树"在模型 A 的 tokenizer 里可能被切成 3 个 token，在模型 B 里可能因为中文词汇表不同而被切成 5 个 token。
- **模型的"啰嗦程度"不同**：模型 A 被训练成"简洁回答"，模型 B 被训练成"在回答前后加上礼貌用语和安全声明"。
- **模型的推理链长度不同**：模型 A 在生成代码前先"在脑中过一遍算法逻辑"（隐式或显式的 chain-of-thought），模型 B 直接生成代码。

**(c)** 如果你要为"渐进 Token 复杂度"定义一个通用的 O 表达式——对于给定任务 T、给定模型 M，**你认为应该写成 T(n, M) 还是 T(n)？** 理由是什么？如果 T(n, M)——那么 M 作为参数应该是什么类型？（字符串"gpt-4"？模型的参数量？模型的某种能力分数？）

**(c)** If you were to define a universal O expression for "asymptotic token complexity" — for a given task T and a given model M, **should it be written as T(n, M) or T(n)?** Why? If T(n, M) — what type should the parameter M be? (The string "gpt-4"? The model's parameter count? Some kind of capability score?)

---

**(4)** 在软件开发中，你已经不再"只调用一次模型"了。以下是两类目前被广泛使用的方式：

In software development today, you no longer "call the model just once." Here are two currently widespread patterns:

- **Agent 循环**：模型生成一个方案 → 执行方案（写代码、shell 命令等）→ 读取执行结果 → 发现错误 → 修正方案 → 再次执行 → ... 直到成功或达到最大轮次。
- **多 Agent 辩论**：两个或多个模型实例各自独立生成答案，然后互看对方的答案，交替提出批评和修改，直到达成共识（或达到最大轮次）。

**(a)** 对于 Agent 循环——假设每次迭代失败的修复会让下一次迭代的 prompt 变得更长（因为包含了前一次的错误信息和代码）。如果每一次失败的概率是 p（0 < p < 1），且系统设定了最大 k 次循环。请用 **最坏情况** 和 **期望情况** 两种方式，分别写下 token 消耗随 k 和 p 变化的表达式。不需要精确——用 O 记号或量级估计即可。

**(a)** For the Agent loop — assume each failed iteration makes the next iteration's prompt longer (because it includes the previous error info and code). If the probability of failure per attempt is p (0 < p < 1), and the system sets a maximum of k loops. Write down — for both **worst-case** and **expected-case** — the token consumption as a function of k and p. No need for precision — O notation or order-of-magnitude estimates are sufficient.

**(b)** 如果多 Agent 辩论中每轮的 token 消耗 = 所有 Agent 的输出之和 × 辩论轮数——这个 O 表达式暗示了一种什么风险？在真实产品中，这个风险通常如何被控制？

**(b)** If in multi-agent debate, per-round token consumption = sum of all agents' outputs × number of debate rounds — what risk does this O expression imply? In real products, how is this risk typically controlled?

**(c)** 回到第零章开篇——Fermi 在三位一体核试验的冲击波中估算出 1 万吨 TNT。他现在不会用手撕纸片了——他会用一个 Agent 循环。**如果 Fermi 的 Agent 在主循环中每次错误之后，token 消耗都翻倍（因为错误日志全部塞回 prompt），这个 Agent 的 Token 复杂度是什么？** 这个问题——在 Fermi 按下"Run"之前——靠什么来决定他愿不愿意付这个 token 账单？

**(c)** Return to Chapter 0's opening — Fermi at the Trinity test, estimating 10 kilotons from torn paper. He wouldn't use paper now — he'd use an Agent loop. **If Fermi's Agent doubles its token consumption after each error in the main loop (because all error logs get stuffed back into the prompt), what is this Agent's Token Complexity?** Before Fermi presses "Run" — what determines whether he's willing to pay that token bill?

*提示：这和1970年的主机账单在结构上有没有区别？当"计算"的计费单位从 CPU 秒变成 token，Fermi 问题的核心——"用你知道的少量东西推算你不知道的东西的量级"——依然成立。改变的只是"你知道的东西"变成了什么。Hint: is this structurally different from the 1970 mainframe bill? When the billing unit of "compute" shifts from CPU-seconds to tokens, the core of the Fermi problem — "using the little you know to deduce the order of magnitude of what you don't" — still holds. What changed is only what "the little you know" consists of.*

---

**(5)** 最后——那个坐在键盘前面用 LLM 的人。你——或者说任何一个使用 AI 的人——你的技能，是否会影响"渐进 Token 复杂度"？

Finally — the person sitting at the keyboard using the LLM. You — or anyone using AI — does your skill affect "asymptotic token complexity"?

**(a)** 以下三个用户，面对"用 Python 写一个网页爬虫抓取新闻标题"这个任务：

- **用户 X**：把任务描述写成一段 500 字的自然语言，贴进 ChatGPT。
- **用户 Y**：把任务拆成 5 个子任务，每个子任务用 50 字的精确指令分 5 轮完成，每一轮的输出被整理后送入下一轮作为上下文。
- **用户 Z**：已经知道了大多数网页的 HTML 结构。他直接写出一个包含 CSS 选择器伪代码的 prompt，一次性让模型填完代码。

这三个用户完成**同一个最终目标**所消耗的 token，可能会有怎样的 O 阶差异？谁的 token 消耗和 n（要抓取的页面数量）的关系最低？谁的"人脑+模型"的联合复杂度最高？

**(a)** Three users face the task "write a Python web scraper to crawl news headlines":

- **User X**: writes the task description as a 500-word natural language paragraph and pastes it into ChatGPT.
- **User Y**: decomposes the task into 5 subtasks, each with a 50-word precise instruction, completed across 5 rounds, each round's output being organized and fed into the next round as context.
- **User Z**: already knows the HTML structure of most websites. They directly write a prompt containing CSS selector pseudocode, asking the model to fill in the code in one shot.

What O-order differences might there be in the tokens consumed by these three users to achieve **the same final goal**? Whose token consumption has the lowest relationship with n (number of pages to crawl)? Whose "human brain + model" joint complexity is the highest?

**(b)** 现在回到第零章的核心论点——渐进和估计的联手。假如你是 2026 年的一个技术主管，你要决定：**一个由 LLM 驱动的代码审查系统的月度 token 预算。** 你做了 3 次实验，每次审查一个不同规模的 PR（小 100 行，中 500 行，大 2000 行），记录了每次的 token 消耗。你需要据此估计"如果每个月要审查 500 个 PR，token 预算大概是多少"。你的手下告诉你："直接用这三个数据点除以代码行数、乘以 500 再乘以平均行数——线性外推呗。" **你会相信这个线性外推吗？在你回答之前，请列出至少三种可能导致实际 token 消耗偏离线性外推的因素，并标注哪些因素在 n 变大时会被放大（类似 O 中的高阶项），哪些因素是固定的常数开销。你不需要给他们一个精确预算——但你需要告诉他们"我们的估计大概在哪个量级，以及最好和最坏的情况可能差多少"。**

**(b)** Now return to Chapter 0's core thesis — the alliance of asymptotics and estimation. Suppose you're a tech lead in 2026, deciding: **the monthly token budget for an LLM-driven code review system.** You ran 3 experiments, each reviewing a PR of different size (small 100 lines, medium 500 lines, large 2000 lines), recording token consumption each time. You need to estimate: "if we review 500 PRs per month, roughly what's the token budget?" Your colleague tells you: "just divide these three data points by lines of code, multiply by 500, then multiply by average line count — linear extrapolation." **Would you trust this linear extrapolation?** Before answering, list at least three factors that could cause actual token consumption to deviate from linear extrapolation, and label which factors amplify as n grows (like higher-order terms in O) and which are fixed constant overhead. You don't need to give them a precise budget — but you need to tell them "roughly what order of magnitude our estimate is at, and how much best and worst case could differ."

---

**(6)** 最后一问——一个反思题，不需要数学。2026 年初，部分科技公司出现了一个名为 **"Tokenmaxxing"** 的内部文化：部门鼓励员工在自己的日常工作中**使用尽可能多的 token**——向 LLM 发送更长、更多轮的 prompt；在 prompt 里附上更多的"背景上下文"；在 Agent 循环中允许更多的重试次数。公司内部甚至建立了**个人 token 消耗排行榜**，将周 token 消耗量作为"AI 采用率"（AI adoption）的代理指标进行公开排名。

**(6)** Final question — a reflection, no math needed. In early 2026, some tech companies developed an internal culture called **"Tokenmaxxing"**: departments encouraged employees to **use as many tokens as possible** in their daily work — sending longer, multi-turn prompts to LLMs; appending more "background context" to prompts; allowing more retry attempts in Agent loops. Companies even set up **personal token consumption leaderboards**, publicly ranking employees by weekly token consumption as a proxy metric for "AI adoption."

**(a)** 在本练习的前五个问题中，你花了很大的力气去分析**token 消耗和问题规模/难度/模型/Agent 框架/使用者的技能之间的关系**。在这些分析的基础上——**"用 token 消耗量作为 AI 采用率的代理指标"在测量学上犯了什么错误？** 请用第零章的语言来回答：token 消耗量和 AI 采用率之间的关系，是线性、超线性、次线性——还是这两者根本没有稳定的函数关系？如果它们之间没有一个可外推的渐进关系——排行榜上的那个人是第 1 名还是第 50 名，你从中获得了什么信息？

**(a)** In the first five questions of this exercise, you spent considerable effort analyzing **the relationship between token consumption and problem size / difficulty / model / Agent framework / user skill.** Based on this analysis — **what measurement error does "using token consumption as a proxy for AI adoption" commit?** Answer in the language of Chapter 0: is the relationship between token consumption and AI adoption linear, superlinear, sublinear — or do the two have no stable functional relationship at all? If there is no extrapolatable asymptotic relationship between them — what information do you actually gain from knowing whether that person on the leaderboard is #1 or #50?

**(b)** Tokenmaxxing 的激励结构可能会导致一种"反向优化"：如果奖金和 token 消耗量挂钩，员工是否有动机——不一定是有意识的——去做以下这些事？请对每一项，用一个词判断（是/否），并举一个极简的例子：

- 本来一个 50 字的 prompt 就能让模型生成正确的函数，现在改成 800 字的 prompt，附带一段 300 字的"你是一个世界级的资深工程师，请仔细思考..."的系统指令。
- 本来可以直接复制粘贴一段代码到 prompt 里让模型审查，现在多加 20 行"这是我上个月的相关的代码变更的 git diff，请参考"——即使这些额外的上下文和这次审查无关。
- 本来第 3 轮 Agent 循环已经得到了正确结果，现在让它跑到第 10 轮——"再检查一下，确保万无一失"。
- 本来可以用一个 token 效率更高的模型（比如一个在 50 token 内就能给出正确输出的新版本），但因为公司用的是"token 消耗量"来评估——这个新模型在排行榜上永远不会出现。为什么？

**(b)** Tokenmaxxing's incentive structure may induce a kind of "inverse optimization": if bonuses are tied to token consumption, do employees have an incentive — not necessarily conscious — to do the following? For each, answer with one word (yes/no) and give a minimal example:

- A 50-word prompt would have gotten the model to generate the correct function — now it's replaced with an 800-word prompt, plus a 300-word system instruction saying "You are a world-class senior engineer, please think carefully..."
- You could directly copy-paste a code snippet into the prompt for the model to review — now you append an extra 20 lines of "here's the git diff of my related code changes from last month, for reference" — even though this extra context is irrelevant to the review.
- The Agent loop already got the correct result at round 3 — now you let it run to round 10 — "check one more time, just to be absolutely sure."
- You could use a more token-efficient model (e.g., a new version that gives the correct output within 50 tokens), but because the company evaluates by "token consumption" — this new model would never appear on the leaderboard. Why?

**(c)** Tokenmaxxing 的排行榜，在结构上和第十章"课程评分把高维能力向量映射到一维分数"是否同构？在这两个案例中——"被测量的那个数字"（考试分数 / token 消耗量）和"你想要但测不到的那个东西"（学生能力 / AI 采用率）之间的关系——是否可以写成一个渐进复杂度的 O 表达式？如果可以——这两个案例中，哪一个的"高阶项"更混乱？哪一个的"常数因子"更像是被被测量制度自身的人为扭曲？

**(c)** Is the Tokenmaxxing leaderboard structurally isomorphic to Chapter 10's "course grading maps a high-dimensional ability vector onto a one-dimensional score"? In both cases — the relationship between "the number being measured" (exam score / token consumption) and "the thing you want but can't measure" (student ability / AI adoption) — can this relationship be written as an O expression of asymptotic complexity? If so — in which of the two cases are the "higher-order terms" more chaotic? In which case is the "constant factor" more likely to be artificially distorted by the measurement regime itself?

**(d)** 最后——如果让你为你的"AI 采用率"设计一个更好的度量，它应该包含哪些维度？**你的度量中是否包含 token 消耗量？如果不包含——请给出理由；如果包含——请回答：它在你的度量公式里是放在分子上、分母上、还是作为一个"低权重项"出现——以及，为什么你给 token 消耗这个位置？**

**(d)** Finally — if you were to design a better metric for "AI adoption," what dimensions should it include? **Does your metric include token consumption? If not — give your reasoning; if yes — answer: does it appear in the numerator, the denominator, or as a "low-weight term" in your formula — and why did you give token consumption that position?**
