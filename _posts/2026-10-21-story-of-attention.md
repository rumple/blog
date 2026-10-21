---
title: "Story of Attention"
---

Attention in a transformer is usually explained with math: queries, keys, values, a dot product, and a softmax, as in the paper that introduced the transformer, [Attention Is All You Need](https://arxiv.org/abs/1706.03762). The math is short, but I found that it didn't stay with me. I could follow the equations, and a week later I couldn't say what they were for. So I made up this story for myself. It lets me understand the intuition behind attention, without any math. Here is the story.

## The plain cloth

The bazaar woke before the sun did. Lanterns swung from wooden poles, chai bubbled in dented brass pots, and the air was already thick with cardamom, wet earth, and the sweet rot of overripe mangoes. Somewhere a bell rang. Somewhere a goat complained. Hundreds of voices rose together into the bazaar's one great hum.

Into this noise walked a trader named Kiran, a bolt of plain white cloth folded over one shoulder. It was good cloth, tightly woven and soft as milk. But in a market that blazed with every color under the sun, it was invisible. People's eyes slid over it the way they slide over a blank wall.

Kiran climbed onto an upturned crate in the middle of the square, cupped their hands, and called out over the din:

*"I need color! Deep color, color that lasts!"*

For a moment the nearest stalls went quiet. Then the bazaar answered.

From a corner shaded by a tattered awning, where great clay vats steamed and the ground was stained blue-black, the dyer raised a dripping arm: *"Indigo! Deep as the night sky, and it never fades!"*

From a stall heaped with orange and crimson powder, the spice seller laughed and held up a fistful of threads: *"Saffron! Bright as a wedding morning, though the sun steals it by summer!"*

The rope maker, sitting cross-legged among coils of rough jute, shouted without looking up: *"Strong rope! Pulls a cart, ties a boat!"*

And the potter, hands grey to the elbow, slapped a wet lump onto the wheel: *"Clay pots! Cool water all day!"*

Kiran stood on the crate and listened, weighing each offer against what they needed the way a jeweler weighs gold. The dyer's call rang true; it was almost exactly what Kiran had asked for. The saffron was color, beautiful color, but it wouldn't last. The rope and the pots were honest goods, but they had nothing to do with color.

So Kiran opened their purse and spent the coins to match. Most of them went into the dyer's stained palm. A few went to the spice seller for a small packet of saffron. A single copper each went to the rope maker and the potter, mostly out of politeness.

By the time the sun stood straight overhead, Kiran's cloth hung drying between two poles. It had turned a deep, shimmering indigo, and a thin saffron border ran along its edge like a line of fire. It was the same cloth, with the same weave and the same softness, but now people stopped to look at it. A child reached out to touch it. An old woman asked where Kiran had found such a blue.

Kiran wasn't the only one who had traded that morning. All across the bazaar, while Kiran had been calling out, every other trader had been doing the same. The dyer needed finer thread, the potter needed rope to bind the kiln wood, and the spice seller wanted pots to keep the saffron dry. Demands rose and supplies answered, back and forth across the square, all at once.

When the lanterns were lit again that evening, nobody left the bazaar the way they came in. Everyone went home carrying a little of someone else, and each of them was a little more themselves for it.

---

## What just happened

Think of each trader, together with what they carry, as a token, and you have just watched an attention head at work. Attention heads are the core building block of the transformer, the architecture behind today's large language models.

A transformer reads text as a list of tokens. Each token starts as a vector that describes the token on its own, without any context. That vector is Kiran's plain cloth. Before a token can be useful, it has to gather context from the tokens around it.

The attention head is the bazaar where that happens:

| Bazaar | Attention head |
|---|---|
| A trader and what they carry | A token |
| Kiran's plain cloth | The token's vector before attention |
| Kiran's demand | The query |
| Each trader's supply | The key |
| How well a supply matches the demand | The score: the dot product of the query and the key |
| Splitting the coins according to the match | The softmax, which turns the scores into weights that add up to 1 |
| The goods that Kiran takes home | The values, added together using the weights |
| The dyed cloth, still the same cloth | The residual connection: the token keeps what it was, and adds what it gathered |
| Everyone trading at once | Every token runs its own query against every other token, in parallel |

A few details in the story are doing real work:

- **Kiran buys from everyone.** No trader gets zero coins, but most of them get very little. Attention works the same way. The softmax never picks just one token. It gives most of the weight to the best matches.
- **The cloth stays the same cloth.** Attention does not replace a token's vector. It adds to it. After this step, the token is still the same token, but it also carries some of the tokens that it matched best.
- **Everyone is a buyer and a seller.** Every token sends out a query, and every token answers the others' queries with its key and value. One attention step is the whole bazaar trading at once, not one trader at a time.

## Who teaches the traders what to say

In the story, Kiran knew what to ask for, and each trader knew how to describe their goods. In a transformer, nobody writes those calls. Each attention head has three learned matrices that turn a token's vector into its query, its key, and its value. Training adjusts these matrices over billions of examples, until the questions that tokens ask are answered by the tokens that actually help.

A real model also has more than one bazaar. Each layer has several attention heads that run side by side, and each head learns to trade in something different. Each one looks for a different kind of context. A token visits all of them at once, and goes home with goods from each.

## What the story leaves out

The story glosses over a lot. It says nothing about how the scores are scaled, how the heads' outputs are combined, how a model stops tokens from looking at the tokens that come after them, or how the vectors are shaped in the first place. That is on purpose. The story is a mnemonic device for the intuition, not a description of the details. When I forget how attention works, I think of the bazaar, and the pieces fall back into place. For the details, you should still read [the paper](https://arxiv.org/abs/1706.03762).
