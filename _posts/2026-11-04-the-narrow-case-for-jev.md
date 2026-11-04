---
title: "The Narrow Case for Jev"
---

In my [last post]({{ site.baseurl }}{% post_url 2026-10-20-asking-llm-for-confidence %}), I wrote that we cannot rely on an LLM to give a confidence score. When you ask an LLM "How confident are you?", it writes an answer in the same way that it writes any other text, and that answer was never checked against how often the model is correct. Though that post was about confidence scores, the same arguments apply when an LLM gives probabilities. For a few weeks, I have been [playing with decision models](https://github.com/rumple/jev-snake-python) to see if they are any better. The one that I used is Jev, from TypeSafe.

## What a decision model is

A decision model takes a question and a list of options that you write in advance, and it gives back a probability for each option. An LLM, on the other hand, takes text and gives back text. For example, a support-ticket router can ask "Which team should handle this ticket?" with the options billing, technical, and sales. The answer is a probability for each team, such as billing 0.08, technical 0.85, and sales 0.07.

Unlike LLMs, a decision model is not autoregressive. An LLM produces text one token at a time, and each token is chosen based on the tokens that came before it. A decision model, by contrast, does not write its answer token by token. It reads the question and the options, and it gives the probabilities for all of the options in a single pass. Because of this, it is very fast and costs much less than an LLM.

Jev also has questions that return the probability of a yes-or-no statement, or a score on a scale that you define. In each case, you define the possible answers, and the model gives a probability to each of them. It does not produce text.

## What TypeSafe claims

TypeSafe says that it trained Jev so that its probabilities are calibrated. That is, across many answers, when Jev says 0.8, it should be correct about 80% of the time. This is the same check as comparing a weather forecaster's predictions with how often it actually rained, but it is done while the model is trained. An LLM was scored on the next token of text. Jev, in contrast, was scored on whether its probabilities match how often it is correct.

But calibration training helps only partly: Jev's probabilities were checked against TypeSafe's questions, not yours. And training cannot make up for missing information. Jev can use only the information in your question and your options, so whether its probabilities are based on enough information depends on what you give it.

This is a difference in design. The next section shows what this difference does and does not give you in practice.

## What works and what doesn't

### What works

**It is fast and cheap.** Because it gives all of the probabilities in a single pass, it is fast. In my experiments, a call took about a third of a second. This makes it practical to ask for a decision very often, for example, on every incoming message. The same decisions made by an LLM would be much slower and would cost much more, because an LLM writes its answer token by token.

**The answer is always one of your options.** Jev can choose only from the options that you list. An LLM, however, can answer in the wrong format, or suggest an option that you did not list. For example, if the options are billing, technical, and sales, the answer is always one of those three, so your code does not need to read and check free text.

**You get a probability for every option.** For example, a ticket with billing 0.45 and technical 0.50 is different from a ticket with technical 0.95, even though technical is the top choice for both. You can use this difference to send unclear cases to a person.

**It works well when the hard part is already done.** For example, before you ask Jev to route a ticket, code can look up the customer's account and add facts to the question, such as "The last payment failed" or "The customer has no open orders." When the facts are clear and the options are clearly described, Jev has much less to guess. Choosing among options that are clearly described is the task that it does well.

### What doesn't work

**The same input does not always give the same answer.** You might expect Jev to be different from an LLM here, but it is not. With an LLM, there are two problems that you can see for yourself: if you ask again, the answer changes, and if you change the wording, the answer changes. Jev has both problems. For example, suppose that you send Jev the same ticket twice, and a ticket goes to a person when its probability is below 0.9. The first time, technical gets 0.91, and the ticket is routed automatically. The second time, technical gets 0.89, and the ticket goes to a person. The difference is small, but the decision changed. Now suppose that you change the question from "Which team should handle this ticket?" to "Which team is responsible for this problem?" The ticket is the same, but technical might now get 0.70. If the information is the same, the answer should be the same. A confidence score that changes when nothing about the problem has changed cannot be relied on.

**It was calibrated on TypeSafe's questions.** TypeSafe checked the probabilities against its own data. A confidence score must be checked on the same kind of question. Your questions, your options, and your wording are different from TypeSafe's, so the calibration may or may not be true for them. For example, if your tickets use the names of your own products and features, Jev has probably never seen those names, and nobody has checked whether its 0.8 still means 80% for your tickets. Also, the calibration results have not been checked independently on a wide range of real workloads. The guides for Jev themselves recommend that you label a few hundred of your own examples before you choose a threshold.

**It is calibrated on average.** "0.8 means correct 80% of the time" is a statement about many answers. It does not tell you whether one particular answer is correct. For example, if Jev routes 1,000 tickets with a probability of 0.8, about 200 of them are expected to go to the wrong team, and the probabilities do not tell you which 200.

**It can use only the information that you give it.** A decision model can use only the information in your question and your options. If the options are not clear, or if the input does not contain the information that is necessary to decide, the probabilities will not be reliable. For example, a ticket that says only "It doesn't work" does not say whether the problem is a payment or a bug. Jev still returns a probability for billing and for technical, and those probabilities look the same as the ones for a clear ticket.

**It gives no reason for its answer.** Jev gives you only the probabilities. There is no signal about why it chose one option over another. With an LLM, by comparison, you can at least read what it writes, even though it is also a black box. You can ask it to explain its answer, and you can see the steps that led to it. The explanation is not always accurate, but it gives you a signal to work with. So Jev is a bigger black box than an LLM. This makes it impossible to steer. When an answer is wrong, you cannot see which part of the input or the criteria caused it. For example, if Jev says that a harmless post breaks a content policy, you cannot tell whether it reacted to a word in the post or to a sentence in the policy. All that you can do is change the wording and try again, and, as described above, small changes to the wording change the answer in ways that you cannot predict.

**Its confidence value measures preference, not correctness.** Jev also returns a value called "confidence" next to the probabilities. Unlike the confidence that an LLM gives, which is just more text that the model writes, this value is calculated by a fixed rule from the shape of the probabilities. The more of the probability goes to one option, the higher the confidence. So it shows how strongly the model prefers one option over the others, but it does not show whether that option is correct. For example, Jev can give technical 0.95 and report a high confidence, and the ticket can still belong to billing. A low confidence is not clear either. It can mean that the input does not have enough information to decide, or that more than one option really fits, and the value does not tell you which. For example, a ticket that says only "It doesn't work" and a ticket about a payment that failed because of a bug can both get billing 0.50 and technical 0.50. The first ticket needs more information from the customer. The second ticket needs both teams.

## When to use it

Before you choose between Jev and an LLM, ask whether you need a model at all. Code is better than any model when two things are true: you can collect the input in a structured form, such as fields, numbers, and choices from a list, and the criteria are specific enough to write as rules. Code always gives the same answer for the same input, and you can see why it gave that answer. A model is necessary only when one of these is not true. Either the input is text that you cannot change, or the criteria are vague and written by someone else, so you cannot turn them into rules. If the criteria are vague but yours, you can make them specific. If they are someone else's but specific, a person can read them once and write them as code. But a content policy that bans "harassment" or "misleading claims" does not say exactly which posts break it, and you cannot rewrite it, because it is not yours.

- {: .ex .is-high} **Approving expense claims.** You design the expense form, and you write the expense policy. Ask for the amount, the category, and the receipt in separate fields, and write the policy as rules, such as "Meals up to $50 per day." Code can check every claim in the same way.
- {: .ex .is-high} **Checking a customer's age against a regulation.** The regulator's rule is not yours, but it is specific: customers must be at least 18 years old. A person can read it once and write it as code. Ask for the date of birth in a date field, and compare it with the date in code.

When a model must read the text, the choice is between Jev and an LLM. The difference is in how each one reaches an answer. Jev reads the question and the options once and gives the probabilities. It cannot look anything up, and it cannot work through the problem in steps. An LLM can do both. It can call tools to find information that it does not have, such as a customer's account or a related document, and it can reason through a decision that has several steps. It also writes out what it did, so when it is wrong, you can see where it went wrong. But each of these steps takes time and costs money, and when an LLM gives a probability, that probability has not been checked against anything.

So Jev fits a decision that meets three conditions. First, you know in advance what information is needed, and your code can add it to the question. Second, the decision is a single choice among options that you can describe clearly. Third, the question is similar to the kind of question that Jev was calibrated on, in general language, without your own product names or terms. When all three are true, there is a reasonable basis to expect that Jev's 0.8 means about 80%, even before you check it on your own data. It fits best when you make the decision many times, so its speed and cost matter. An LLM fits better when the model must find the information itself, when deciding takes several steps, or when someone needs to see how the answer was reached.

- {: .ex} **Sorting customer feedback.** Thousands of comments arrive every week, and each one is a complaint, a question, or praise. The comments are in everyday language, the options are clear, and nothing needs to be looked up. A wrong label costs little. This is a case for Jev.
- {: .ex} **Reviewing contract clauses against a legal policy.** A clause can refer to definitions elsewhere in the contract, and the policy has exceptions that depend on the type of contract. You cannot know in advance which parts are needed, so the model must look them up and check them one at a time. When a clause is flagged, a lawyer needs to see the reasoning. This is a case for an LLM.

## Even then, check it

Even when Jev fits, do not assume that its probabilities are correct for your questions. A question that is similar to the ones Jev was calibrated on is a reason to expect calibration, not proof of it. So I would use Jev only when all of these are also true:

- {: .ex} **You make the same decision many times.** Calibration is a property of many answers, so it can be checked only when there are many answers.
- {: .ex} **You can find out afterward whether the answer was correct.** For example, a reviewer checks a sample of the decisions. Without this information, you cannot check the probabilities.
- {: .ex} **The probability is used by a rule that you check with your own records.** For example, "Accept the post above 0.9, and send it to a person below 0.9." Your records, not Jev, show where the threshold should be, and answers near the threshold go to a person.
{: .examples}

The confidence comes from a rule that is built from your own records. Jev can be one of the inputs to that rule, but it does not replace the rule or the record.

Finally, if the cost of a wrong answer is very high, do not use Jev at all, even when all of these conditions are true. A calibrated 0.95 still means that about 1 answer in 20 is wrong, and Jev gives no reason that would help you find that answer. For example, do not use it to decide whether to close a customer's account, approve a large payment, or report a post to the police. In these cases, a wrong answer costs much more than the time that a person needs to make the decision.

## Conclusion

Use Jev only when all of these are true:

- A model must read text that you cannot change, or apply criteria that are vague and that you cannot rewrite.
- The information that is needed is in the question, or your code can add it.
- The decision is a single choice among options that you can describe clearly.
- The question is in general language, like the questions that Jev was calibrated on.
- You make the decision many times, and you can check the answers against what actually happened.
- A wrong answer costs little.
