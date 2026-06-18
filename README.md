# Fundamental Sequence Ordinal Encoding

*Construction of this project*

*Original idea by @solarzone, explained by @rngdelak*

---

## Overview

This document introduces a uniform method to encode ordinals using three core functions:

- **FS(α, n)** → Returns the n-th element of the fundamental sequence of ordinal α  
- **is_successor(α)** → True if α is a successor ordinal, False if limit ordinal  
- **cmp(α, β)** → Comparison function (-1 if α < β, 0 if equal, 1 if α > β)

**Prerequisite:** Familiarity with ordinal numbers up to Γ₀.

---

## I. Key Concepts

A **fundamental sequence** of a limit ordinal α is a sequence that approaches α from below.

**Notation:**
- FS(α): the sequence  
- α[n]: the n-th element (0-indexed)

### Examples

- FS(ε₀) = {1, ω, ω^ω, ω^(ω^ω), ...}  
  → ε₀[3] = ω^(ω^ω)

- FS(ω^ω) = {1, ω, ω², ω³, ...}  
  → ω^ω[3] = ω³

---

## II. Core Definitions

### 1. Function f(α, β)

```
f(α, β) = min { β[n] | β[n] > α }
```

Returns the smallest element in β’s fundamental sequence which is greater than α.

---

### 2. Encoding Real Numbers into Binary Strings (h(x))

For 0 < x < 1:

- If x < k → `"0" + h(x/k)`
- If x = k → `""` (empty string)
- If x > k → `"1" + h((x - k)/(1 - k))`
this works for all k that 0 < k < 1
These are **symbolic binary strings**, not standard binary expansions.

#### Examples (k = 1/2)

| x   | h(x) |
|-----|------|
| 1/8 | "00" |
| 1/4 | "0"  |
| 3/8 | "01" |
| 1/2 | ""   |
| 5/8 | "10" |
| 3/4 | "1"  |
| 7/8 | "11" |

---

### 3. Function g([α;β];s)

we can define g as folow

g([α;β];s)=

g([α;f(α,β)];t) if s[0]="0" 

g([f(α,β);β];t) if s[0]="1"

f(α,β) if β is a limit and s=""

α if β is a successor

as s = s[0] + t (t is string s without the first element)

---

## III. Example Evaluation

**Binary string:** `111011001`  
**Ordinal bound:** ε₀

### Steps


Intialize : [0, ε₀]
| Step | Bit | Interval |
|------|-----|----------|
| 1 | 1 | [1, ε₀] |
| 2 | 1 | [ω, ε₀] |
| 3 | 1 | [ω^ω, ε₀] |
| 4 | 0 | [ω^ω, ω^(ω^ω)] |
| 5 | 1 | [ω^(ω²), ω^(ω^ω)] |
| 6 | 1 | [ω^(ω³), ω^(ω^ω)] |
| 7 | 0 | [ω^(ω³), ω^(ω⁴)] |
| 8 | 0 | [ω^(ω³), ω^(ω³·2)] |
| 9 | 1 | [ω^(ω³ + ω²), ω^(ω³·2)] |Final Result : ω^(ω³ + ω²·2)



*You may also analyse some yourself too!*
*If you can analyse these correct example correctly, you have understand it!*


Bound ordinal : e0
| Binary      | Ordinal        |
|-------------|----------------|
| 0           | 0              |
| ''          | 1              |
| 10          | 2              |
| 101         | 3              |
| 1011        | 4              |
| 1           | ω              |
| 11000       | ω+1            |
| 110001      | ω+2            |
| 1100011     | ω+3            |
| 1100        | ω2             |
| 110010      | ω2+1           |
| 1100101     | ω2+2           |
| 11001011    | ω2+3           |
| 110010111   | ω2+4           |
| 11001       | ω3             |
| 110011      | ω4             |
| 1100111     | ω5             |
| 110         | ω^2            |
| 1101000     | ω^2 + 1        |
| 11010001    | ω^2 + 2        |
| 110100011   | ω^2 + 3        |
| 110100      | ω^2 + ω        |
| 11010010    | ω^2 + ω + 1    |
| 110100101   | ω^2 + ω + 2    |
| 1101001     | ω^2 + ω2       |

---

## IV. Reverse functions

## 1. Function g⁻¹([α;β];x)

We define the inverse of g as follows:

g⁻¹([α;β];x) =

"0" + g⁻¹([α;f(α,β)];t) if x < f(α,β)

"1" + g⁻¹([f(α,β);β];t) if x > f(α,β)

"" if x = f(α,β)

"" if β is a successor ordinal


---

## 2. Function h⁻¹(s)

We define h⁻¹ as follows, for fixed constant k ∈ (0,1)

---

h⁻¹("") = k

h⁻¹(s) = k * h⁻¹(t) if s[0] = "0"

h⁻¹(s) = k + (1 - k) * h⁻¹(t) if s[0] = "1"

as s = s[0] + t (t is string s without first element)

---

## Summary intuition

- "0" = left refinement
- "1" = right refinement
- f(α,β) = split point
- recursion reconstructs path

## Global javascript implement

```js
/********************************************************
 * REQUIRED GLOBAL FUNCTIONS
 ********************************************************/

//This function are ordinal sysrem dependences

function fs(alpha, n) {}

function isSuccessor(alpha) {}

function cmp(a, b) {}

/********************************************************
 * f(alpha,beta)
 ********************************************************/

function f(alpha, beta) {

    let n = 0;

    while (true) {

        const x = fs(beta, n);

        if (cmp(x, alpha) > 0) {
            return x;
        }

        n++;
    }
}

/********************************************************
 * g([α,β], s)
 ********************************************************/

function g(alpha, beta, s) {

    // terminal
    if (isSuccessor(beta)) {
        return alpha;
    }

    const split = f(alpha, beta);

    // empty string
    if (s === "") {
        return split;
    }

    const bit = s[0];
    const rest = s.slice(1);

    if (bit === "0") {
        return g(alpha, split, rest);
    }

    return g(split, beta, rest);
}

/********************************************************
 * g⁻¹
 ********************************************************/

function gInv(alpha, beta, target) {

    if (isSuccessor(beta)) {
        return "";
    }

    const split = f(alpha, beta);

    const c = cmp(target, split);

    if (c === 0) {
        return "";
    }

    if (c < 0) {
        return "0" +
            gInv(alpha, split, target);
    }

    return "1" +
        gInv(split, beta, target);
}

/********************************************************
 * h(x)
 ********************************************************/

function h(x, k = 0.5) {

    if (x === k) {
        return "";
    }

    if (x < k) {
        return "0" + h(x / k, k);
    }

    return "1" +
        h((x - k) / (1 - k), k);
}

/********************************************************
 * h⁻¹
 ********************************************************/

function hInv(s, k = 0.5) {

    if (s === "") {
        return k;
    }

    const bit = s[0];
    const rest = s.slice(1);

    if (bit === "0") {
        return k * hInv(rest, k);
    }

    return k +
        (1 - k) * hInv(rest, k);
}
```

## Summary

- Real numbers in (0,1) are encoded into binary strings via **h(x)**  
- Binary strings define a path through ordinal intervals via **g(X, α)**  
- Fundamental sequences guide interval refinement  
- The process yields a unique ordinal below α

## Extends this idea

## Conversion bettween 2 symetrical ordinal system

It able to convert any ordinal between 2 system

Let two system A and B such that lim(A)<lim(B)

Express lim(A) in B (call X)

then for any a belong to A

We have the conversion to B

---
Conv(a) = gB(hB(h-1A(g-1A(a))*(g-1B(h-1B(X))))
---

for gB,hB,h-1B,g-1B are function implemented for system B, and h-1A,g-1A is implemented for system A

Requirement : symetric between 2 fs
---
fsA(k)[n] = fsB(k)[n]
"fs of 2 equivalent ordinal is equivalent"
---

Like w can be represented in many ways

Such as Y(1,2) has fs of Y(),Y(1),Y(1,1),Y(1,1,1),... in wY-sequence

or (0)(1) has fs of (0),(0)(0),(0)(0)(0),... in BMS

Or psi0(psi0(0)) is 0,psi0(0),psi0(0)+psi0(0),psi0(0)+psi0(0)+psi0(0),... BOcf

But they are fundamentally fs of w which is 0,1,2,3,4,...


## Implement
- Check ordinal.js for the Implementation of this construction with bound ordinal ω^ω
