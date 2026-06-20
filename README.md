# Fundamental Sequence Ordinal Encoding

*Construction of this project*

*Original idea by @solarzone, explained by @rngdelak*

---

## Overview

This document introduces a uniform method to encode ordinals using three core functions:

- **FS(α, n)** → Returns the n-th element of the fundamental sequence of ordinal α  
- **isSuccessor(α)** → True if α is a successor ordinal, False if limit ordinal  
- **cmp(α, β)** → Comparison function (-1 if α < β, 0 if equal, 1 if α > β)

**Prerequisite:** Familiarity with ordinal numbers up to Γ₀.

---

## I. Key Concepts

A **fundamental sequence** of a limit ordinal α is a sequence that approaches α from below.

**Notation:**
- fs(α): the fundamental sequence of α
- α[n]: the n-th element (0-indexed)

**Symbol used:**
- |A| : number of element in set A
- [α;β] : The set contain all ordinals x that α ≤ x ≤ β

### Examples

- fs(ε₀) = {1, ω, ω^ω, ω^(ω^ω), ...}  
  → ε₀[3] = ω^(ω^ω)

- fs(ω^ω) = {1, ω, ω², ω³, ...}  
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

```
For 0 < x < 1: h(x) =
"0" + h(x/k)               if x < k
"" (empty string)          if x = k
"1" + h((x - k)/(1 - k))   if x > k
```
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

```
for |[α;β]| > 0:
g([α;β];s)=
g([α;f(α,β)];t)    if s[0]="0" 
g([f(α,β);β];t)    if s[0]="1"
f(α,β)             if β is a limit and s=""
α                  if β is a successor

as s = s[0] + t (t is string s without the first element)

given that α,β are ordinals, s is binary string
```

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
| 9 | 1 | [ω^(ω³ + ω²), ω^(ω³·2)] |
| End | "" | Final Result : ω^(ω³ + ω²·2) |



*You may also analyse some yourself too!*
*If you can analyse these correct example correctly, you have understand it!*


Bound ordinal : e0
| Binary      | Ordinal        |
|-------------|----------------|
| 0           | 0              |
| ""          | 1              |
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

```
for |[α;β]| > 0 and 0 ≤ x < 1
g⁻¹([α;β];x) =
"0" + g⁻¹([α;f(α,β)];t)      if x < f(α,β)
"1" + g⁻¹([f(α,β);β];t)      if x > f(α,β)
""                           if x = f(α,β)
""                           if β is a successor ordinal

as s = s[0] + t (t is string s without the first element)
```

## 2. Function h⁻¹(s)

We define h⁻¹ as follows, for fixed constant k ∈ (0,1)

```
for any binary string s and aspect ratio 0 < k < 1
h⁻¹(s) = 
k if                s = ""
k*h⁻¹(t)            if s[0] = "0"
k+(1-k)*h⁻¹(t)      if s[0] = "1"

as s = s[0] + t (t is string s without first element)
```

## Extend this idea

### Conversion bettween 2 symetrical ordinal system

It able to convert any ordinal between 2 symetrical ordinal system

- Let two system A and B such that lim(A)<lim(B)

- Express lim(A) in B (call X)

- then for any a belong to A

**We have the conversion to B**

```
Conv(a) = gB(hB(h⁻¹A(g⁻¹A(a))*(g⁻¹B(h⁻¹B(X))))
for gB,hB,h⁻¹B,g⁻¹B are function implemented for system B, and h⁻¹A,g⁻¹A is implemented for system A
```

**Requirement : symetric between 2 fs**

```
fsA(k)[n] = fsB(k)[n]
this mean fs of 2 equivalent ordinal is equivalent
```

Like ω can be represented in many ways

- Such as Y(1,2) has fs of Y(),Y(1),Y(1,1),Y(1,1,1),... in wY-sequence

- or (0)(1) has fs of "",(0),(0)(0),(0)(0)(0),... in BMS

- Or psi0(psi0(0)) is 0,psi0(0),psi0(0)+psi0(0),psi0(0)+psi0(0)+psi0(0),... BOcf

**But they are fundamentally fs of ω which is 0,1,2,3,4,...**

---
### Fundamental sequence α[n] with rational n and reversal

```
α[n] = g([0;α];1-(1-k)^n)
```

for aspect ratio 0 < k < 1

**Explaination**

- 1-(1-k)^n maps n = 0,1,2,3,4,... into g⁻¹(α[0]), g⁻¹(α[1]), g⁻¹(α[2]), g⁻¹(α[3]),...

- g([0;α];1-(1-k)^n) maps n = 0,1,2,3,4,... into α[0], α[1], α[2], α[3],... which is exactly fs(α)

**We also define the reverse of this process**

```
α{β} = ln(1-h⁻¹(g⁻¹([0;α];β)))/ln(1-k)
```

**Explaination**

- h⁻¹(g⁻¹([0;α];β)) is the position of β inside hierachical space α

- ln(1-h⁻¹(g⁻¹([0;α];β)))/ln(1-k) maps them back into 0,1,2,3,4,...


## Global javascript implement

```js
/********************************************************
 * REQUIRED GLOBAL FUNCTIONS
 ********************************************************/

//This function are ordinal sysrem dependences
//System for the example : LPrSS

function cmp(a, b) {
    for (let i = 0; i < Math.min(a.length, b.length); i++) {
        if (a[i] < b[i]) return -1;
        if (a[i] > b[i]) return 1;
    }

    if (a.length < b.length) return -1;
    if (a.length > b.length) return 1;
    return 0;
}

function fs(a, n) {
	let out = [...a];
	let cutNode = out.pop();
	let root = out.length - 1;
	while (out[root] >= cutNode && root > 0) root--;
	let increment = cutNode - out[root] - 1;
	let badPart = out.slice(root);
	for (let i = 1; i < n; i++) {
		out = out.concat(badPart.map(v => v + increment * i));
	}
	return out;
}

function isSuccessor(array) {
	return array.length === 0 || array.at(-1) === 0;
}

let ZERO = [] // 0 = [] in LPrSS

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

/********************************************************
 * Extension of Fundamental sequence to rational
 ********************************************************/

function fsR(alpha, n, k = 0.5) {

    const x = 1 - Math.pow(1 - k, n);

    return g(ZERO, alpha, h(x, k));
}

/********************************************************
 * fsR⁻¹
 ********************************************************/

function fsRInv(alpha, beta, k = 0.5) {

    const s = gInv(ZERO, alpha, beta);

    const x = hInv(s, k);

    return Math.log(1 - x) /
           Math.log(1 - k);
}
```


## Summary

- Real numbers in (0,1) are encoded into binary strings via **h(x)**  
- Binary strings define a path through ordinal intervals via **g(X, α)**  
- Fundamental sequences guide interval refinement  
- The process yields a unique ordinal below α

## Implement
- Check ordinal.js for the Implementation of this construction with bound ordinal **Lim(BMS)**
