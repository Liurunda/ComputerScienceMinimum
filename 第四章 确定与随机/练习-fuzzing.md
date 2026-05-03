# 练习 4 — 随机测试 vs 确定性验证：fuzzing 的边界

Exercise 4 — Random Testing vs. Deterministic Verification: The Boundaries of Fuzzing

---

**(a) 实现一个简单的 fuzzer**

**(a) Implement a simple fuzzer**

编写一个简单的"随机测试"工具：给定一个目标函数（如一个除法函数 `divide(a, b)` 或一个解析日期字符串的函数），随机生成大量输入，调用目标函数，记录是否崩溃（异常、panic、或返回明显错误的值）。

Write a simple "random testing" tool: given a target function (e.g., a division function `divide(a, b)` or a date-string parser), randomly generate a large number of inputs, call the target function, and record whether it crashes (throws an exception, panics, or returns obviously wrong values).

在以下三个目标函数上测试你的 fuzzer：

Test your fuzzer on the following three target functions:

1. `divide(a, b)` — 返回 a / b（没有除零检查的版本）
2. `parse_date(s)` — 解析 "YYYY-MM-DD" 格式的字符串（没有错误处理的版本）
3. 你自己写的一个有隐蔽 bug 的函数（你知道 bug 是什么但 fuzzer 不知道）

1. `divide(a, b)` — returns a / b (version without divide-by-zero check); 2. `parse_date(s)` — parses strings in "YYYY-MM-DD" format (version without error handling); 3. A function you wrote yourself with a hidden bug (you know what the bug is but the fuzzer doesn't).

你的 fuzzer 需要多少次测试才能找到第一个崩溃？它对每个函数的 bug 发现效率如何？

How many test cases does your fuzzer need to find the first crash? How efficient is it at finding bugs for each function?

---

**(b) Fuzzing 的盲区**

**(b) The blind spots of fuzzing**

给其中一个目标函数注入一个**在极窄条件下才会触发**的 bug（如：只有当输入是一个特定的质数年份、并且月份是 2 月 29 日时，parse_date 才会返回错误的日期）。你的 fuzzer 是否能找到这个 bug？如果不能，需要什么样的输入分布设计才能让它被找到？

Inject a bug into one of the target functions that triggers **only under extremely narrow conditions** (e.g., `parse_date` returns a wrong date only when the input year is a specific prime number and the month+day is February 29). Can your fuzzer find this bug? If not, what input distribution design would it take for the bug to be found?

这个实验如何体现"随机测试提供概率信心而非确定性保证"这一论断？你需要多少次测试才能对这个函数"没有 bug"有 99% 的信心？

How does this experiment illustrate the claim that "random testing provides probabilistic confidence, not deterministic guarantees"? How many test cases would you need to be 99% confident that this function "has no bugs"?

---

**(c) 形式化验证 vs 随机测试：一个辩论**

**(c) Formal verification vs. random testing: a debate**

读一读关于 fuzzing 和形式化验证（formal verification）的一个简短介绍（可以让 LLM 帮你总结两者的核心差异）。然后，写一段简短的辩论稿，从两个立场分别论述：

Read a brief introduction to fuzzing and formal verification (ask an LLM to summarize the core differences for you). Then, write a short debate, arguing from two positions:

- **立场 A（fuzzing 支持者）**：形式化验证太慢、太贵，永远只能覆盖极小一部分程序。随机测试虽然不完美，但它是我们在工程现实中最有效的质量保障手段。
- **立场 B（形式化验证支持者）**：随机测试的本质是"我们没有能力证明程序正确"，从而默认接受了"可能有 bug"的状态。对于安全关键的软件（飞行控制、医疗设备、加密货币合约），概率信心是不够的。

Position A (pro-fuzzing): Formal verification is too slow and expensive, and can only ever cover a tiny fraction of programs. Random testing is imperfect, but in engineering reality it is our most effective quality assurance tool.; Position B (pro-formal-verification): The essence of random testing is "we are unable to prove the program correct," so we default to accepting the state of "there might be bugs." For safety-critical software (flight control, medical devices, cryptocurrency contracts), probabilistic confidence is not enough.

你的辩论稿应该在最后给出一个综合判断：在什么条件下，随机测试"足够好了"？在什么条件下，我们必须付出形式化验证的代价？这个"条件"，是否可以用本章"随机性是一种货币"的框架来重新表述？如果你在决定花多少"随机性货币"和多少"确定性货币"，你的预算约束是什么？

Your debate should conclude with a synthesis: under what conditions is random testing "good enough"? Under what conditions must we pay the cost of formal verification? Can this "condition" be reformulated using this chapter's framework of "randomness as a currency"? If you're deciding how much "randomness currency" versus "determinism currency" to spend, what is your budget constraint?
