---
title: An interesting Math problem
summary:
date: 2025-03-22
authors:
  - admin
tags:
  - math
---

## 📘 Solve the equation:

\[
\sqrt{5 - x} = 5 - x^2
\]

---

### ✏️ Step 1: Eliminate the square root

Square both sides:

\[
(\sqrt{5 - x})^2 = (5 - x^2)^2
\]
\[
5 - x = 25 + x^4 - 10x^2
\]

---

### ✏️ Step 2: Rearrange into a standard polynomial form

Move all terms to one side:

\[
0 = x^4 - 5 - 10x^2 + x + 25
\]

---

### ✏️ Step 3: Let’s use the quadratic formula

We write this as a quadratic in terms of \(5\):  
Let:

- \(a = 1\)
- \(b = -(2x^2 + 1)\)
- \(c = x^4 + x\)

Use the quadratic formula:

\[
5 = \frac{(2x^2 + 1) \pm \sqrt{(2x^2 + 1)^2 - 4(x^4 + x)}}{2}
\]

---

### ✏️ Step 4: Apply the Quadratic Formula and Simplify

We are solving:

\[
5 = \frac{2x^2 + 1 \pm \sqrt{(-2x^2 - 1)^2 - 4(x^4 + x)}}{2}
\]

Now simplify the discriminant:

\[
= \frac{2x^2 + 1 \pm \sqrt{4x^2 - 4x + 1}}{2}
\]

Factor the square root:

\[
= \frac{2x^2 + 1 \pm (2x - 1)}{2}
\]

Now evaluate the two cases from here, which can be solve using standard quard formula.

### ✏️ Step 5: Solve the two quadratic equations

Using the quadratic formula:

#### Equation 1:

\[
x^2 + x - 5 = 0 \Rightarrow x = \frac{-1 \pm \sqrt{1^2 + 4 \cdot 5}}{2} = \frac{-1 \pm \sqrt{21}}{2}
\]

#### Equation 2:

\[
x^2 - x - 4 = 0 \Rightarrow x = \frac{1 \pm \sqrt{1^2 + 4 \cdot 4}}{2} = \frac{1 \pm \sqrt{17}}{2}
\]

---

### ✅ Final Answer:

Check all four roots against the original equation. Only two satisfy it:

\[
\boxed{\frac{1}{2}(1 - \sqrt{17}) \quad \text{and} \quad \frac{1}{2}(\sqrt{21} - 1)}
\]
