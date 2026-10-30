# EAQECC family Lean certificate

The formalization is split into three independent layers:

- `Definitions.lean`: shared symplectic vocabulary and the normalizer-side certificate. It imports
  only Mathlib and contains no named theorem result.
- `Theorem1.lean`: the complete proof of Theorem 1. It imports `Definitions.lean`, but not
  `Lemma2.lean`.
- `Lemma2.lean`: the complete proof of Lemma 2. It imports `Definitions.lean`, but not
  `Theorem1.lean`.

The `Comparator` directory contains a separate Challenge, Solution, and JSON configuration for
each result. The Challenge imports only `Definitions.lean`; the Solution imports exactly the proof
module that it submits.

## Exact goals

`EAQECCFamily.theorem1_exact` constructs `z₂` and nonzero `λ` under the paper's two field-size branches.
Its `NormalizerSideCertificate` proves generator injectivity, the restricted Gram formula, the exact
radical, actual finranks `q+2` and `q`, exact non-radical distance `n-1`, the resulting EAQECC
parameters, and BDH excess `q-2`.

`EAQECCFamily.lemma2_coordinate_normalization` assumes `R ≤ L`, `dim L = dim R + 2`,
`dim R > 2`, and
non-radical symplectic weight at least `n-1`. It constructs coordinatewise local symplectic
equivalences that send all of `R` to Z-type.

## Local kernel builds

From the archive root:

```sh
lake exe cache get
lake build \
  CodingTheoryLib.Research.EAQECCFamily.Definitions \
  CodingTheoryLib.Research.EAQECCFamily.Theorem1 \
  CodingTheoryLib.Research.EAQECCFamily.Lemma2

lake build \
  CodingTheoryLib.Research.EAQECCFamily.Comparator.Theorem1Challenge \
  CodingTheoryLib.Research.EAQECCFamily.Comparator.Theorem1Solution \
  CodingTheoryLib.Research.EAQECCFamily.Comparator.Lemma2Challenge \
  CodingTheoryLib.Research.EAQECCFamily.Comparator.Lemma2Solution
```

The only `sorry` declarations are the intentionally unproved statements in the two Challenge
files. The proof and Solution files contain no `private`, `sorry`, `admit`, or user-defined axiom.

## Comparator checks

With compatible `comparator`, `lean4export`, and `landrun` binaries in `PATH`:

```sh
lake env /path/to/comparator \
  CodingTheoryLib/Research/EAQECCFamily/Comparator/Theorem1.json

lake env /path/to/comparator \
  CodingTheoryLib/Research/EAQECCFamily/Comparator/Lemma2.json
```

Both configurations permit only `propext`, `Quot.sound`, and `Classical.choice`.

## Verification

The checks of these files are recorded in `../../../verification/`: a kernel build on macOS and a
Comparator run on Linux x86_64 for both configurations, on 30 October 2026. Earlier runs on
16 and 21 September 2026 checked the same proofs under a previous module name; only module and
namespace names differ.

`landrun v0.1.17` consumes the child command's `--` separator. The Comparator used here carries the
one-line fix in `../../../verification/linux-2026-10-30/comparator-landrun-separator.patch`. No
export, declaration, axiom-checking, or kernel logic was changed.

## Source hashes

```text
26a962bf6811020fd138eafadd80f1049e68a1091ba469afe87ca95fafb6a64e  Definitions.lean
8cb009084ecc66dc8a8da4efc89c190aab291a3afbe9f364b92e33a8ae17615f  Theorem1.lean
d94f57798ba16f4827372d69fcca380815638c051e765cd99304e52fffaadcfb  Lemma2.lean
5fd725e4561fb8f3e04d3e99d8d31e414c964cce723d07a843c05cacb951c585  Comparator/Theorem1Challenge.lean
693b33e153b45ef2b356e5fe752c7d9e6acc4152d094821adaeccb5a19e2af37  Comparator/Theorem1Solution.lean
081ac674ff49690d56c7d7ae9c5536d082f588e541b1d18b66afe8685b8bf8aa  Comparator/Theorem1.json
a67896703a2ceec1f5ea2c4c5950c47756cd78e54c38d5f817f9e5c2d6f336da  Comparator/Lemma2Challenge.lean
57a89a53b344f4303682f64eff5cbbd4fe82fd4471fd7e9054088af7986ad617  Comparator/Lemma2Solution.lean
80e215efa9aaf84da794de13e0988f2b0979b4aa93098dc44f5b78e9a2a003fd  Comparator/Lemma2.json
```
