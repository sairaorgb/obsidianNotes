
## Quick glossary (one-liners)

* **Input size (`n`)** — a measure of problem input (length, number of elements, bits, etc.).
* **Time complexity** — how the number of steps grows with `n`.
* **Space complexity** — how memory usage grows with `n` (ignore program binary, focus on asymptotic extra memory).
* **Upper bound (Big‑O)** — `f(n) = O(g(n))`: `f` grows no faster than constant×`g` for large `n`.
* **Lower bound (Big‑Ω)** — `f(n) = Ω(g(n))`: `f` grows at least as fast as constant×`g` for large `n`.
* **Tight bound (Big‑Θ)** — `f(n) = Θ(g(n))`: both `O(g)` and `Ω(g)` hold.
* **Little‑o / little‑ω** — strict forms of upper/lower bounds (`f=o(g)` means `f/g→0`).
* **Asymptotic equivalence (`~`)** — `f(n) ~ g(n)` means `f/g → 1`.
* **Amortized complexity** — average cost per operation over a sequence (used for dynamic arrays, splay trees, etc.).

---

## Formal definitions (mathematical)

* **Big‑O:** `f(n) = O(g(n))` iff `∃ c>0, n0` s.t. `∀ n ≥ n0: 0 ≤ f(n) ≤ c·g(n)`.
* **Big‑Ω:** `f(n) = Ω(g(n))` iff `∃ c>0, n0` s.t. `∀ n ≥ n0: 0 ≤ c·g(n) ≤ f(n)`.
* **Big‑Θ:** `f(n) = Θ(g(n))` iff `f(n) = O(g(n))` and `f(n) = Ω(g(n))`.
* **Little‑o:** `f(n) = o(g(n))` iff `lim_{n→∞} f(n)/g(n) = 0`.
* **Little‑ω:** `f(n) = ω(g(n))` iff `lim_{n→∞} f(n)/g(n) = ∞`.
* **Asymptotic equivalence:** `f(n) ~ g(n)` iff `lim_{n→∞} f(n)/g(n) = 1`.

---

## Intuition / how to read bounds

* `O(g)` = **won't asymptotically exceed** `g` (upper ceiling).
* `Ω(g)` = **won't asymptotically go below** `g` (lower floor).
* `Θ(g)` = **gives the true growth class** (sandwich).
* **Loose vs tight:** `f(n)=O(n^2)` can be true for `f(n)=n` — formally correct, but not informative. Prefer the tightest classification (Θ) when possible.

---

## Worst / Best / Average case

* **Worst-case:** maximum cost over all inputs of size `n` → commonly used when guaranteeing performance.
* **Best-case:** minimum cost (rarely useful alone).
* **Average-case:** expected cost under a specified input distribution; needs a clear probability model.

**Examples:**

* Linear search: best `Ω(1)`, worst `O(n)`, average `Θ(n)` if target equally likely.
* QuickSort (classic): worst `O(n^2)`, average `Θ(n log n)` (random pivot or random input).

---

## Common complexity classes (small table)

* `O(1)` — constant
* `O(log n)` — logarithmic
* `O(n)` — linear
* `O(n log n)` — linearithmic
* `O(n^2)` — quadratic
* `O(n^k)` — polynomial (k constant)
* `O(2^n)` — exponential
* `O(n!)` — factorial

Use this order: `1 < log n < n < n log n < n^2 < n^3 < 2^n < n!`.

---

## How to derive complexity from code (practical rules)

1. **Count basic operations** (comparisons, assignments) and express as `f(n)`.
2. **Ignore constants** and lower-order terms when `n → ∞`.
3. **Loops:** nested loops multiply (`for i` inside `for j` → often `O(n·m)` or `O(n^2)`).
4. **Consecutive statements:** add costs, then keep highest-order term.
5. **Recurrences:** set up recurrence and solve (Master theorem, substitution, recursion tree).
6. **Amortized analysis:** use aggregate, accounting, or potential method.

**Quick patterns:**

* Single loop over `n` → `O(n)`.
* Two independent loops → `O(n+m)` (or `O(n)` if `m`∼`n`).
* Loop with half-size each iteration → `O(log n)` (binary search pattern).
* Divide-and-conquer with `T(n)=a T(n/b) + f(n)` → use Master theorem when applicable.

---

## Recurrences & Master theorem (cheat-sheet)

For `T(n) = a T(n/b) + f(n)` with `a≥1, b>1`:

* Let `n^{log_b a}` be the critical exponent.
* If `f(n) = O(n^{log_b a - ε})` → `T(n) = Θ(n^{log_b a})`.
* If `f(n) = Θ(n^{log_b a}·log^k n)` → `T(n) = Θ(n^{log_b a}·log^{k+1} n)`.
* If `f(n) = Ω(n^{log_b a + ε})` and regularity holds (`a f(n/b) ≤ c f(n)` for some `c<1`) → `T(n) = Θ(f(n))`.

(When Master doesn't apply, use recursion-tree or substitution.)

---

## Space complexity specifics

* **Auxiliary (extra) space:** memory beyond input.
* **In-place:** `O(1)` extra memory (e.g., in-place partitioning for quicksort).
* **Recursive calls:** add `O(depth)` to space (e.g., merge sort recursion `O(log n)` depth or `O(n)` depending on implementation).

Tip: report both total space and auxiliary space when relevant.

---

## Amortized complexity (short)

* Applies when an expensive operation is rare but others are cheap (e.g., dynamic array resizing).
* **Aggregate method:** total cost over `m` ops divided by `m`.
* **Accounting method:** give credits to cheap ops to pay for expensive ones.
* **Potential method:** use a potential function to relate states.

Example: `vector.push_back` average `O(1)` amortized even though occasional `O(n)` resize happens.

---

## Little-o and strictness — why useful

* `f(n) = o(g(n))` means `g` grows strictly faster: `f/g → 0`.
* Distinguishes loose `O` from truly negligible terms. Useful in proofs and approximations.

---

## Useful analytical tools

* **Limit test:** If `lim_{n→∞} f(n)/g(n) = c` (0<c<∞) then `f = Θ(g)` (if `c`>0 finite).
* **Compare polynomials:** higher degree dominates.
* **Exponentials vs polynomials:** exponentials dominate polynomials: `n^k = o(2^n)`.
* **Summations:** use closed forms or integral approximations. Common sums:

  * `\sum_{i=1}^n i = Θ(n^2)`
  * `\sum_{i=0}^{log n} 2^i = Θ(n)`
  * `\sum_{i=1}^n 1/i = Θ(log n)`

---

## Common pitfalls & interview traps

* Saying `O(n)` when you mean `Θ(n)` — prefer Θ if exact.
* Forgetting that `O(1)` does not mean zero cost — it means constant cost independent of `n`.
* Miscounting costs inside conditional branches — analyze each branch and take worst/expected case as needed.
* Confusing input-size parameterization (bits vs elements). For numeric algorithms on integers, cost may depend on bit-length.

---

## Worked examples (short)

**1. `for (i=0;i<n;i++) for (j=0;j<i;j++)`**

* Steps ≈ `\sum_{i=0}^{n-1} i = Θ(n^2)` → `O(n^2)` and `Ω(n^2)` → `Θ(n^2)`.

**2. `T(n)=2T(n/2)+n`**

* `a=2, b=2, n^{log_b a}=n` and `f(n)=n=Θ(n^{log_b a})` → `T(n)=Θ(n log n)` (merge sort).

**3. Dynamic array push with doubling**

* Sequence of `m` pushes costs `O(m)` total → amortized `O(1)` per push.

---

## Practical advice

* Use **Θ** when you can; use **O** for safe upper bounds in proofs.
* Focus on the bottleneck: find the dominating term.
* Note the hidden constants and lower-order terms matter for small `n` or tight performance needs.
* When in doubt, write a recurrence or count key operations and simplify.

---

## Short cheatsheet (one page)

* `If f(n) = a_k n^k + ... + a_0` with `a_k>0` → `f(n) = Θ(n^k)`.
* `log_a n = Θ(log_b n)` for any `a,b>1` (change of base constant).
* `n^c = o(a^n)` for any `a>1, c>0`.
* `sum_{i=1}^n i^p = Θ(n^{p+1})` for `p>-1`.

---


