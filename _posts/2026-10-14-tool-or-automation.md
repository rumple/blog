---
title: "Tool or automation? You make the decision each time."
---

Rich Hickey [says that an LLM is automation, not a tool](https://www.linkedin.com/pulse/ai-tool-rich-hickey-kjl8e). Huahai [says that an LLM is a tool, and that you must have skill to use it correctly](https://yyhh.org/blog/2026/10/ai-is-a-tool). I think that both are correct. Each of them describes a different use of an LLM.

Tools and automation are different in how much they abstract. An abstraction does a part of the work for you, and it removes the details of that part from your control. For example, a compiler always abstracts the same part of the work: the generation of machine code. A saw abstracts almost nothing. An LLM is different. **Each time that you use an LLM, you select how much it abstracts.** For example, if you tell it to examine your understanding of a scientific paper, it abstracts almost nothing, because you still do the reading. If you tell it to read the paper for you, it abstracts all of the work.

## The cost of abstraction

All abstractions have a cost, but the cost is not the same for each abstraction. The cost has two parts.

**You lose control of the details.** When a tool abstracts more, you control fewer details. But you get control of a higher level of the work. For example, with a compiler, you do not control the registers. Instead, you control the structure of the program. This is the reason that we use abstractions.

**You stop learning the details.** You learn a skill when you do the work yourself. When a tool does the work for you, you do not learn that skill.

## Two axes for cost

The size of the cost depends on two properties of a tool.

**How much does it abstract?** When a tool abstracts more, you lose control of more details, and you learn fewer of them.

**If it fails, will you see the failure?** For example, compiler failures are easy to see: an error message, a crash, or a failed test. Because of this, the lost control has a low cost. LLM failures are not easy to see. For example, an incorrect summary of a paper looks the same as a correct summary.

The table shows the cost for each combination of these two properties.

|  | Failures are easy to see | Failures are difficult to see |
|---|---|---|
| Low abstraction | **Low cost.** For example, a saw. | **Low cost.** You are near the work, so you see problems. |
| High abstraction | **Low cost.** For example, a compiler. | **High cost.** |

The high-cost area has a special problem. When the tool fails, you must repair the failure yourself. To do this, you must understand the work that the tool did. But the tool did that work for you, so you did not learn the skill that is necessary to understand it.

## Is the cost acceptable?

A high cost is a problem only when it is necessary for you to know the removed details. Sometimes the details are the purpose of the work. Sometimes they are not important.

The same LLM can be in different areas of the table. This changes with how you use it. For example:

- {: .ex} **You read the paper. Then you tell the LLM to examine your understanding of it.** **Low cost:** low abstraction, and failures are easy to see because you know the paper. You keep the learning, because you did the reading.
- {: .ex} **You tell it to change the format of a table that you understand.** **Low cost:** high abstraction, but failures are easy to see. It was not necessary for you to learn that step.
- {: .ex .is-high} **You tell it to read the paper for you.** **High cost:** high abstraction, and failures are difficult to see. The removed details were the purpose of the work. Also, you cannot examine its summary unless you read the paper yourself.

To decide if the cost is acceptable, do not ask "Is it a tool?" Ask these questions:

1. Which part of the work does it abstract?
2. Is it necessary for me to know those details?
3. If it fails, will I see the failure? Can I repair it?

If the answers are "a large part," "yes," and "no," you have a problem. It does not matter if you call it a tool or automation.

---

Links:

- Hickey: [linkedin.com/pulse/ai-tool-rich-hickey-kjl8e](https://www.linkedin.com/pulse/ai-tool-rich-hickey-kjl8e)
- Huahai: [yyhh.org/blog/2026/10/ai-is-a-tool](https://yyhh.org/blog/2026/10/ai-is-a-tool)
