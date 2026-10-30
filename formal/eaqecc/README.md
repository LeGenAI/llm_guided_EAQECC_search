# Lean certificates for the EAQECC paper

Theorem 1 is the paper's formalized discovery result. Its exact goal and solution
are presented in Appendix D. Lemma 2 is retained here as supporting verification
for the SAT normalization; it is not an additional pipeline-discovery claim.
The two proof modules independently import the shared definitions.

The package keeps its lockfile and its
[certificate README](CodingTheoryLib/Research/EAQECCFamily/README.md).
`SOURCE_MANIFEST.sha256` covers its 13 files. This guide and the `verification/`
records are outside the package. The proofs are unchanged from the versions
checked on 16 and 21 September 2026; only the module and namespace names, now
`EAQECCFamily`, and the doc comments differ.

## Files

| File under `CodingTheoryLib/Research/EAQECCFamily/` | Purpose |
|---|---|
| `Definitions.lean` | Pauli vectors, symplectic product/weight, the concrete construction and certificate type |
| `Theorem1.lean` | General construction proof, including `theorem1_exact` |
| `Lemma2.lean` | Independent coordinate-normalization proof retained for completeness |
| `Comparator/Theorem1Challenge.lean` | Statement-only trusted target |
| `Comparator/Theorem1Solution.lean` | Same declaration, proved by `theorem1_exact` |
| `Comparator/Theorem1.json` | Permitted axioms and comparator settings |
| Corresponding `Lemma2` files | Supporting result's independent goal and solution |

The `sorry` in each Challenge file deliberately leaves the target unproved.
Challenge modules are not imported by the proofs or submitted Solutions. Kernel
builds of the Solutions report only `propext`, `Classical.choice` and `Quot.sound`.

## Check the package without Lean

From the repository root:

```bash
python3 scripts/audit_lean_certificate.py
```

This checks source hashes, the two exact goal texts and the Comparator settings.
It does not execute Lean or Comparator.

## Rebuild with the pinned Lean toolchain

Install [elan](https://github.com/leanprover/elan), then run from this directory:

```bash
lake exe cache get
lake build \
  CodingTheoryLib.Research.EAQECCFamily.Definitions \
  CodingTheoryLib.Research.EAQECCFamily.Theorem1 \
  CodingTheoryLib.Research.EAQECCFamily.Lemma2 \
  CodingTheoryLib.Research.EAQECCFamily.Comparator.Theorem1Solution \
  CodingTheoryLib.Research.EAQECCFamily.Comparator.Lemma2Solution
```

Lean is pinned to `4.29.0-rc6`; Mathlib is locked to
`1ea3462986d06c85c98fb18c10082274043405e4`. Preserve `lake-manifest.json` and do not
run `lake update`: the supplied lakefile's tag differs from the manifest's
recorded input reference, and Lake may warn that the manifest is out of date.
The replay uses the lockfile's exact commit. Build caches remain in `.lake/`
and are excluded from Git.

## Verification status

- **Kernel build:** PASS on macOS arm64 with the pinned Lean and Mathlib, on
  30 October 2026. See the [JSON record](verification/kernel-build-macos.json)
  and the [build output](verification/kernel-build-macos.log). Both proof
  modules and both submitted Solutions build, and they depend only on
  `propext`, `Classical.choice` and `Quot.sound`.
- **Linux Comparator run:** PASS for Theorem 1 and the supporting Lemma 2 on
  Linux x86_64, with Lean 4.29.0-rc6 and all ten locked package revisions, on
  30 October 2026. See the [logs, environment and CLI
  patch](verification/linux-2026-10-30/README.md). Comparator is pinned to
  `a4f696825c583ed8a5b4060d9a0faa5b882d365b`, and lean4export to
  `048394e1afeeb52b0fa27bcf3f1ade2ff0f0ab6d`.

Comparator requires Linux `landrun` and compatible `comparator` and `lean4export`
binaries. landrun 0.1.17 consumes the exporter's argument separator. The
[one-line patch](verification/linux-2026-10-30/comparator-landrun-separator.patch)
adds `--` before the executable name in Comparator's `buildLandrunArgs`.
Apply it to the pinned Comparator source and rebuild. The exporter source,
statement comparison, axiom policy and kernel-checking logic are unchanged.
Both configurations disable optional nanoda, so these are Lean-kernel checks,
not independent-kernel validation. The pinned
[Comparator documentation](https://github.com/leanprover/comparator/blob/a4f696825c583ed8a5b4060d9a0faa5b882d365b/README.md)
separates its statement/dependency comparison, permitted-axiom checks and
Lean kernel check from the optional additional-kernel check. We do not claim
the latter.
