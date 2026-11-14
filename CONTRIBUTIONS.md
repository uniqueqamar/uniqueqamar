# Engineering contributions and learning log

This log records verified repository activity, distinguishes code changes from upstream acceptance, and avoids claiming unrun tests or unmeasured model improvements.

## 2026-11-14 — hls4ml softmax fixture probability bug

- External upstream [draft PR #1617](https://github.com/fastmachinelearning/hls4ml/pull/1617) from [fix/softmax-test-sampling-20261114](https://github.com/uniqueqamar/hls4ml/tree/fix/softmax-test-sampling-20261114).
- Commits [da7a650](https://github.com/uniqueqamar/hls4ml/commit/da7a65077afd29efea7144770fea842099dec04d) and [d13803b](https://github.com/uniqueqamar/hls4ml/commit/d13803b36d7f86c10684315f255166ca1cc00626).
- Root cause: NumPy's `randint(0, 1)` upper bound is exclusive, so the prior `randint(0, 1, shape) < 0.05` selected **all** inputs for perturbation, not approximately 5%. The patch uses `np.random.random(shape) < 0.05` and adds a deterministic regression test.
- **2026-11-14 verification:** extracted the actual fixture and regression test logic into an isolated local pytest module (without importing TensorFlow/hls4ml): **1 passed in 0.05 s** (NumPy 2.3.5, pytest 9.0.2). A 40,000-sample seeded comparison yielded old mask rate 1.0 and corrected mask rate 0.0493. This is not a full hls4ml suite run.
- GitHub Actions on PR head: manifest **success**, packaging **success**, Sphinx **failure**. Sphinx log reports missing `get_fixed_quantizer_np` in installed `quantizers` while importing XLS backend; whether this also fails on upstream main is unverified. No human review recorded at the time of inspection. **Keep draft**; no review requested.
- Learning: exclusive upper bounds, Bernoulli sampling, deterministic regression tests, and distinguishing unrelated CI dependency failures from a patch regression.
- Next: run full softmax tests in a compatible HLS/TensorFlow environment; compare Sphinx CI with main; address any review comments before marking ready.

## 2026-11-11 — Flask cooperative subclass initialization

- Upstream issue: [pallets/flask#6186](https://github.com/pallets/flask/issues/6186).
- Fork branch: [uniqueqamar/flask — fix/cooperative-init-subclass-6186](https://github.com/uniqueqamar/flask/tree/fix/cooperative-init-subclass-6186).
- Implementation: [40373def](https://github.com/uniqueqamar/flask/commit/40373defd1399f23d24ebcf5ba13379aad01dfd9) forwards class keyword arguments through `super().__init_subclass__(**kwargs)`.
- Regression tests: [824ce4c](https://github.com/uniqueqamar/flask/commit/824ce4c3d943eebf907eaa42e885b4ed96442616) covers cooperative mixin hooks and unknown class keywords.
- **Status:** two commits pushed to fork; **no upstream PR**. GitHub restricts new Flask PRs to collaborators; attempted PR creation returned HTTP 403. Tests were not run in the connected GitHub-only environment. Do not claim this contribution was submitted upstream.
- Learning: Python cooperative multiple inheritance uses the MRO; subclass hooks should forward remaining keyword arguments. A conflict-free Git comparison does not imply permission to open a PR.
- Next: run `python -m pytest tests/test_basic.py -k flask_init_subclass -q` in a full checkout; find a community-accessible upstream repository before investing in another patch.

## 2026-10-31 onward — Landsat reconstruction validation work

- [Draft PR #1](https://github.com/uniqueqamar/landsat-cloud-reconstruction/pull/1): one-pass paired masked evaluation and per-patch diagnostics; original PR records three synthetic tests passing and explicitly excludes real validation and unseen-region claims.
- [Draft PR #8](https://github.com/uniqueqamar/landsat-cloud-reconstruction/pull/8): spatial interpolation baseline, separate from the trained model; eight synthetic tests reported passing in the PR.
- [Draft PR #9](https://github.com/uniqueqamar/landsat-cloud-reconstruction/pull/9): reject unsupported fully cloudy or clear inputs before optical-only inference; six synthetic tests reported passing in the PR.
- **2026-11-11 review:** PR #1 remains open and draft, mergeable; no review comments. No Landsat model weights, geographic splits, deployed demo, or inference code changed in this review. No new accuracy numbers were measured.
- Learning: paired evaluation uses the same samples/masks for each method; a later-date optical image is only a temporal proxy, not true simultaneous cloud-free ground truth. Reserve held-out geographic test scenes for final evaluation.

## Prior — hls4ml streaming line buffer

- [Merged PR #1596](https://github.com/fastmachinelearning/hls4ml/pull/1596): Vitis HLS 2025.1 streaming line-buffer compatibility fix. Maintainer verification and merge were recorded previously; this log entry is retrospective, not new work on 2026-11-11.
