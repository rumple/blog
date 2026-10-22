---
title: "The Game ChatGPT Plays"
---

If you don't work with computers, AI tools like ChatGPT can seem like magic. In a way, they are: what they can do is remarkable. But the ideas underneath them are simpler than they look, and you don't need a technical background to follow them. This post explains one of those ideas, using a guessing game.

Here is the game. I have a sentence in mind with fourteen words in it. Can you guess the fifteenth word?

> \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ **?**

You can't, because you don't know the first fourteen words. The next word depends on the words before it. This is also where tools like ChatGPT and Claude start when they produce text.

## A word on its own means very little

Suppose I show you just one of the fourteen words, the last one:

> \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ \_\_\_\_ of **?**

That doesn't help much. "Of" can be followed by almost anything: of course, of mine, of water, of the year.

This isn't only true of small words like "of". Take the word "bank". In "I sat on the bank and watched the river", it means the edge of a river. In "I went to the bank to deposit a cheque", it means a place that keeps your money. The word is the same; the words around it decide what it means.

Or take "it". In "The dog chased the ball until it was tired", "it" is the dog. In "The dog chased the ball until it rolled under the car", "it" is the ball. Nothing about the word "it" tells you which one. Only the other words do.

## Now with all the words

Here is the whole sentence:

> I stopped at the bakery on my way home and bought a loaf of **?**

Now most people would guess "bread".

## Which words helped you?

Which of these words helped you guess "bread"?

Mainly two words: **bakery** and **loaf**. "Bought" helps a little, because it tells you that something was bought. The rest, "I", "stopped", "at", "the", "on my way home", make little difference.

One way to check this is to change a word and see whether the guess changes.

- Change **bakery** to **wine shop**: *"I stopped at the wine shop on my way home and bought a bottle of..."* The guess is now something like "wine". (I also changed "loaf" to "bottle", because "a loaf" does not fit a wine shop.)
- Change **on my way home** to **after work**: *"I stopped at the bakery after work and bought a loaf of..."* The guess is still "bread".

So the words in a sentence do not count equally. Some carry a lot of weight for the next word, and some carry very little.

## Putting numbers on it

Suppose you gave each of the fourteen words a score for how much it helped you guess "bread", with all the scores adding up to 100. The scores might look something like this. These are numbers I made up to show the idea, not numbers from a real model.

| Word | Score |
|---|---|
| bakery | 40 |
| loaf | 35 |
| bought | 9 |
| a | 3 |
| stopped, home, of | 2 each |
| I, at, the, on, my, way, and | 1 each |

Two words get three quarters of the total score. That is what "carry more weight" means: when you guess the next word, you rely mostly on a few words.

These scores are not fixed. They depend on which word you are trying to guess. If the same sentence stops earlier:

> I stopped at the bakery on my way **?**

Now the next word is "home", and the words that help are "on my way". "Bakery" does not help here. Each time you guess a new word, the earlier words get new scores.

## How ChatGPT uses this

ChatGPT produces text by playing the same guessing game. It writes one word at a time, and each word is a guess about what should come next, given everything before it: your question, plus the words of the answer it has written so far. For each word, it follows the same steps:

1. Look back at all the words so far.
2. Give each of them a score for how much it matters to the next word.
3. Guess the next word, leaning mostly on the words with high scores.
4. Add that word to the end, and go back to step 1.

If ChatGPT were writing our sentence, it would reach "bought a loaf of", give "bakery" and "loaf" high scores, and guess "bread". Then it would add "bread" and start the loop again for the word after that. Repeating these steps produces the whole answer.

## How does it know which words matter?

You knew that "bakery" mattered because you have been to bakeries, and you have heard "a loaf of bread" many times. Nobody gave you a rule. You picked it up from experience.

A model starts without that experience, and at first every word looks equally important to it. It learns by practice. While a model like ChatGPT is being built, it is shown a very large amount of text, such as books, articles and websites. It hides the next word, guesses it, and checks the guess against the real word. When the guess is wrong, it adjusts how it scores words slightly, so that the words that help will score higher next time. After billions of these small corrections, its scores become similar to yours: "bakery" and "loaf" score high for "bread", and "on my way" scores high for "home". Nobody writes the scores by hand.

## What this leaves out

Real models are more complicated than this. A real model works with pieces of words rather than whole words, and it scores the words many times over, in many layers, each looking for something different. But the basic idea is the same as with the bakery sentence: look at all the words so far, decide which ones matter, and guess the next word.

I described how the scoring works in more detail, still without math, in [Story of Attention]({% post_url 2026-10-21-story-of-attention %}).
