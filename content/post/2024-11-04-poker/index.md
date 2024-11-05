---
title: Poker Strategy
summary: This article provides a brief overview of mathematics in poker.
date: 2024-11-04
authors:
  - admin
tags:
  - random-thoughts
image:
  caption: "Image credit: [**Unsplash**](https://unsplash.com)"
---

# MDF and BF

## MDF

If the pot is \(1\) and your opponent's bet is \(b\):

- you fold: \(1 \times (1 - p(\text{you-call}))\)
- you call: \(-b \times p(\text{you-call})\)

Adding both cases:

\[
1 \times (1 - p(\text{you-call})) + p(\text{you-call}) \times -b = 0
\]

Simplifying:

\[
1 = (1 + b) \times p(\text{you-call})
\]

Solving for \(p(\text{you-call})\):

\[
p(\text{you-call}) = \frac{1}{1 + b}
\]

## Bluffing Frequency

- 0.75 pot
  - flop 67%
  - turn 50%
  - river 33%

# How to bet

1. Size

- If you have nuts advantage, bet bigger.
- If you do not have nuts advantage, bet smaller.
- Nuts advantage: when you have more combinations of super strong hands (two-pair or better) than his opponent

2. Find Value Cards

The inequality \( \text{Equity} > 1 - \frac{1}{2(1+b)} \) suggests a threshold where a player’s equity (their share of the pot if the hand goes to showdown) should exceed a certain value in order for them to make a profitable move, such as calling or betting.

To understand this better, we can break it down:

- **Setting Up the Inequality**: The right side, \(1 - \frac{1}{2(1+b)}\), provides a boundary value. This boundary depends on the size of the bet \(b\) relative to the pot (normalized to 1).

- **Interpretation of the Threshold**: If a player’s equity in the hand is greater than \(1 - \frac{1}{2(1+b)}\), then their hand is likely profitable enough to proceed with aggressive action, such as betting or calling.

- **Breakdown of the Expression**:

  - For larger bets \(b\), \( \frac{1}{2(1+b)} \) becomes smaller, and therefore \(1 - \frac{1}{2(1+b)}\) approaches 1. This implies that when the bet is large, the player needs a higher equity to continue.
  - For smaller bets \(b\), \( \frac{1}{2(1+b)} \) is larger, meaning that \(1 - \frac{1}{2(1+b)}\) is lower. In this case, a player can proceed with lower equity since the risk is relatively lower.

3. Value bet

- If I check, the remaining streets < biggest that I can get, I 100% bet

4. What to bluff

- Backdoor Draw
- Frontdoor Draw

## exploitation

### Call

- If opponent bluff too much, call, otherwise not (BF to check)
- Only call one street.

### Bluff

- If your opponent is calling too much, do not bluff, otherwise, bluff (MDF to check)
