# Linux Comparator run — 2026-10-30

Theorem 1 and the supporting Lemma 2 both passed the exact-goal comparison and the Lean kernel
check, with exit code 0 and "Your solution is okay!". The source manifest passed before and after
the run, and the kernel build of all seven modules succeeded.

This run checks the sources after the module and namespace were renamed to `EAQECCFamily`. The
proofs are those of the runs on 16 and 21 September 2026; only names and doc comments differ.

The host is Linux x86_64 with Lean 4.29.0-rc6 and the ten package revisions of
`lake-manifest.json`. Comparator is a4f696825c583ed8a5b4060d9a0faa5b882d365b with the one-line
`comparator-landrun-separator.patch`, and lean4export is 048394e1afeeb52b0fa27bcf3f1ade2ff0f0ab6d.
The binaries are the ones built for the run of 21 September 2026; their SHA-256 hashes are in
`binaries.sha256` and `environment.json`. Optional nanoda is disabled in both configurations, so
this is a Lean-kernel check, not an independent-kernel validation.

`sha256sum` ran under a Korean locale and prints "성공" for OK in the integrity logs.

| file | contents |
|---|---|
| `Theorem1-comparator.log`, `Lemma2-comparator.log` and `.exit` | Comparator output and exit code |
| `kernel-build.log` | `lake build` of the seven modules |
| `source-integrity.log`, `source-integrity-after.log` | `sha256sum -c SOURCE_MANIFEST.sha256` before and after |
| `binaries.sha256`, `lean-version.log`, `landrun-version.log`, `package-revs.txt`, `platform.txt` | environment |
| `RESULTS.json`, `environment.json` | summary |
