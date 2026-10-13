<!-- solve-problem:40fe305b-0aef-403a-83b8-141989aed070 -->
# Near-linear seed length in generic OWF-to-PRG constructions

A [Tangle Solve problem](https://solve.tangle.tools/problems/40fe305b-0aef-403a-83b8-141989aed070).

## What is the problem?

Starting from an arbitrary one-way function (OWF) \(f: \{0,1\}^n \to \{0,1\}^{\ell}\) — no regularity, injectivity, or one-to-one-ness assumed — construct a cryptographic pseudorandom generator (PRG) \(G: \{0,1\}^s \to \{0,1\}^{s+1}\) with **seed length \(s = n^{1+o(1)}\)**, where the security reduction must be **black-box** (a standard, explicitly specified security-preserving reduction: any distinguisher breaking \(G\) yields an inverter for \(f\) with polynomially related advantage/time, via oracle access only). Formal setting: \(f\) is \((t,\varepsilon)\)-hard to invert; \(G\) should be \((t', \varepsilon')\)-pseudorandom with \(t' \approx t/\mathrm{poly}\) and \(\varepsilon' \approx \mathrm{poly}\cdot\varepsilon\) (or some stated, reasonable security loss). The seed-length budget \(n^{1+o(1)}\) is the point: existence is not the question (HILL gives polynomial seed), efficiency is. Ambiguity note: "near-linear" and the precise security-loss parameters tolerated are not pinned down in the source; a solver should fix explicit target parameters (e.g., \(s = n\cdot\mathrm{polylog}(n)\), security loss \(n^{O(1)}\)) and state them.

## Why does it matters

The HILL theorem (OWF ⟹ PRG) is foundational — it underlies derandomization results (BPP ⊆ SUBEXP under assumptions) and modern cryptography — but its seed length is a polynomial far from linear, making the reduction quantitatively useless in practice. A near-linear-seed construction would make cryptographic derandomization and complexity-theoretic hardness-to-randomness conversions efficient enough to matter for concrete parameters, and would resolve the canonical open quantitative question in black-box hardness amplification. It also directly advances the program of optimizing seed length in cryptographic PRGs, a decades-long line (Impagliazzo, Haitner, Reingold, Vadhan, Trevisan, and others).

## What's already known?

- **Håstad–Impagliazzo–Levin–Luby (1999, STOC'89/SICOMP):** any OWF yields a PRG, but seed length is a large polynomial in \(n\) and the security reduction loses substantially.
- **Haitner–Reingold–Vadhan (SICOMP 2013):** improved the construction via the "next-block" / computational entropy framework, reducing seed length and security loss; the best known generic constructions still have seed length around \(n^2\) up to polylog factors (verify the exact current-best exponent — the frontier has moved via Haitner, Reingold, Vadhan, and follow-ups; treat precise exponents as needing a fresh literature check).
- **Special classes solved:** for **regular** OWFs, seed length \(O(n)\) or \(O(n\log n)\) is achievable (Goldreich–Krawczyk–Yao line, and Haitner's work); known barriers suggest the general case needs genuinely new ideas to handle non-uniform preimage-size structure.
- Key technical toolkit already developed: next-block pseudorandomness, dense / next-bit pseudoentropy, uniform hardness-to-randomness, randomized iterative hashing. The obstacle: these tools each pay a multiplicative entropy-loss or repetition overhead that compounds to superlinear seed.

## What would count as solving it?

1. **Upper bound (main target):** an explicit construction \(G\) from any \((t,\varepsilon)\)-one-way \(f\) on \(n\) bits, with seed length \(n^{1+o(1)}\) (ideally \(n\cdot\mathrm{polylog}\,n\)) and a fully specified black-box reduction with security loss \(\mathrm{poly}(n)\). A referee wants the reduction written out, with exact parameters.
2. **Lower bound / barrier (valuable variant):** a proof that black-box reductions cannot achieve seed length better than \(n^{1+c}\) for some \(c>0\) (or an oracle separation showing \(o(n\log n)\)-type barriers), which would resolve the problem negatively for the black-box model.
3. **Intermediate:** seed length \(n\cdot\mathrm{polylog}(n)\) under an explicitly stated stronger assumption (e.g., mildly regular OWF), clearly demarcating what generality costs.

## How to start

1. Read Håstad–Impagliazzo–Levin–Luby (1999) for the base construction, then Haitner–Reingold–Vadhan (2013) — the entropy-chain / next-block machinery is where any attack must engage.
2. Trace where each multiplicative seed overhead enters (repetition for amplification, hashing for entropy extraction, input length blowup in the OWF-to-next-bit step); produce an explicit "seed-length accounting" table for the best current construction. This identifies the binding bottleneck — likely the number of sequential invocations of \(f\) and the entropy gap.
3. Consider whether dense pseudoentropy can be converted with amortized loss \(1+o(1)\) per bit rather than constant loss per repetition.
4. In parallel, sketch a black-box lower bound attempt (counting/oracle-relativization à la Impagliazzo–Rudich) to test whether the target is even achievable — this is the natural sanity check before a heavy construction effort.

## Key references

- Håstad, Impagliazzo, Levin, Luby, "A Pseudorandom Generator from any One-Way Function," SIAM J. Comput. 28(4), 1999. Proves the foundational theorem that arbitrary OWFs imply PRGs via pseudoentropy, with polynomial seed length and lossy security. (No stable public link recorded; standard DOI-resolvable.)
- Haitner, Reingold, Vadhan, "Efficiency Improvements in Constructing Pseudorandom Generators from One-Way Functions," SIAM J. Comput. 42(3), 2013. Gives the current-best generic construction using next-block pseudoentropy and randomized hashing, improving seed length and security loss over HILL.
- Trevisan memorial collection (2024). — **(verify)**: cited in the source record; likely a survey/open-problem collection; consult it for the exact problem formulation and pointers, but existence and contents are unverified here.
- Goldreich–Krawczyk–Yao line / Haitner's works on regular OWFs — linear-seed PRGs for regular functions; useful contrast for identifying what breaks without regularity. **(verify exact citations before relying on parameters.)**

## Warnings

- **Parameter ambiguity:** "near-linear," "black-box," and the tolerated security loss are not precisely defined in the source; fix them up front or a result may not answer the intended question.
- The census record is a single fleet-generated entry with a vague 2024 citation; verify the current best seed-length exponent independently — it may differ from what is stated above.
- Common dead end: trying to compress seed via simple XOR/universal hashing — this destroys pseudorandomness without addressing the entropy gap; the loss is provable in known frameworks.
- Known constructions degrade gracefully for regular functions; don't prototype only on regular/injective \(f\) and assume generality.

---
*`ALGO-000227` · unverified*

## Acceptance guidance

1. **Upper bound (main target):** an explicit construction \(G\) from any \((t,\varepsilon)\)-one-way \(f\) on \(n\) bits, with seed length \(n^{1+o(1)}\) (ideally \(n\cdot\mathrm{polylog}\,n\)) and a fully specified black-box reduction with security loss \(\mathrm{poly}(n)\). A referee wants the reduction written out, with exact parameters.
2. **Lower bound / barrier (valuable variant):** a proof that black-box reductions cannot achieve seed length better than \(n^{1+c}\) for some \(c>0\) (or an oracle separation showing \(o(n\log n)\)-type barriers), which would resolve the problem negatively for the black-box model.
3. **Intermediate:** seed length \(n\cdot\mathrm{polylog}(n)\) under an explicitly stated stronger assumption (e.g., mildly regular OWF), clearly demarcating what generality costs.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/40fe305b-0aef-403a-83b8-141989aed070?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000227.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000227)
