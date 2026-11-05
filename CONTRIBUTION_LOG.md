# Contribution and learning log

## 2026-11-04 — Memory-bounded expanded-shard checksum verification

- **Draft PR:** [landsat-cloud-reconstruction #5](https://github.com/uniqueqamar/landsat-cloud-reconstruction/pull/5)
- **Commits:** [hash helper](https://github.com/uniqueqamar/landsat-cloud-reconstruction/commit/2a8e0740f37faffded3fcef114db5153141c3702), [validator integration](https://github.com/uniqueqamar/landsat-cloud-reconstruction/commit/69c58c6d075e755fbee16649442c90d7678e8389), [tests](https://github.com/uniqueqamar/landsat-cloud-reconstruction/commit/be9522d03258051fa43dca6f5c0491e454f8452f).
- **Change:** Replace whole-file `read_bytes()` SHA-256 hashing with incremental 1 MiB reads in expanded-shard integrity checks; preserve exact digest results.
- **Verification:** Five isolated local pytest cases passed (0.07 s), including bounded-read guard. Full repository integration tests, actual expanded data and CI not yet verified.
- **Concepts:** Streaming algorithms, memory complexity, file integrity, regression tests.
- **Follow-up:** Run `python -m pytest -q tests/test_file_hash.py` and `python validate_data.py` with real expanded data before review. Existing draft PRs #1–#4 remain separate; demo unchanged.

This log records published work and its verification status. Draft pull requests are not merged contributions.

## 2026-11-03 — Mask-aware evaluation safeguards

- **Draft PR:** [landsat-cloud-reconstruction #4](https://github.com/uniqueqamar/landsat-cloud-reconstruction/pull/4)
- **Commits:** [implementation](https://github.com/uniqueqamar/landsat-cloud-reconstruction/commit/177c7548b3dea85e7b2c971cfb1608c4a464720f), [tests](https://github.com/uniqueqamar/landsat-cloud-reconstruction/commit/6f35678450b5409bc9e7fd3146fdcf1d42c18d1a)
- **Change:** Handle validation splits with zero masked pixels explicitly, skip empty-mask batches, and report fully unmasked samples. Four regression cases added.
- **Verification:** Four equivalent isolated CPU PyTorch tests passed locally. The actual committed test module and full repository suite remain **unverified** because the local checkout could not be fetched.
- **Concepts:** Metric denominators, empty-set edge cases, batch-level versus sample-level accounting, regression tests.
- **Follow-up:** Run `python -m pytest -q tests/test_train_evaluate.py` in a complete checkout and inspect CI before requesting review.

## Earlier open draft work

- [2026-11-02: #3 — bounded decoded-shard cache](https://github.com/uniqueqamar/landsat-cloud-reconstruction/pull/3). Four isolated tests previously reported; full checkout testing pending.
- [2026-11-01: #2 — temporal split integrity](https://github.com/uniqueqamar/landsat-cloud-reconstruction/pull/2). Ten isolated tests previously reported; full raster verification pending.
- [2026-10-31: #1 — paired validation diagnostics](https://github.com/uniqueqamar/landsat-cloud-reconstruction/pull/1). Full suite pending.

## Learning exercises

1. Why is dividing masked error by zero invalid even when the model predicts the clear image perfectly?
2. For a batch with two entirely clear samples and one masked sample, what should the masked MAE denominator count?
3. Why should a validation error message identify the split rather than silently returning a score of zero?

## 2026-11-05 — Expanded shard validation performance and integrity

- **Draft PR:** [landsat-cloud-reconstruction #6](https://github.com/uniqueqamar/landsat-cloud-reconstruction/pull/6)
- **Commits:** [helper](https://github.com/uniqueqamar/landsat-cloud-reconstruction/commit/92d101670f3810d89e7ceaf48d138c9dc035fe27), [tests](https://github.com/uniqueqamar/landsat-cloud-reconstruction/commit/ca3f13069a9e13502b750f31a1e909b5da654d45), [integration](https://github.com/uniqueqamar/landsat-cloud-reconstruction/commit/a3767beec9f33240e41a44b2a788b161c32ef1d6)
- **Change:** Group expanded-index records by compressed shard, decode each array once per shard, and validate every indexed patch. Reject negative/out-of-range indices, inconsistent array dimensions, invalid reflectance, and invalid QA flags. Geographic split and checksum logic retained.
- **Verification:** Five synthetic pytest tests passed in isolated directory (0.07 seconds). Full checkout suite, actual expanded data and CI not verified; no benchmark claimed.
- **Concepts:** I/O amortization, compressed array loading, index bounds, data-integrity assertions, regression testing.
- **Follow-up:** Run committed tests and real `validate_data.py` on a complete checkout, measure runtime and peak RAM; inspect interaction with draft PR #5 before merging. Browser demo unchanged; no review requested.

## 2026-11-05 — Spectral reconstruction diagnostics (draft PR #7)

- **Draft PR:** https://github.com/uniqueqamar/landsat-cloud-reconstruction/pull/7
- **Commits:** [metrics](https://github.com/uniqueqamar/landsat-cloud-reconstruction/commit/56f9e59d780dd6762ea23527e170e80d80507582), [audit CLI](https://github.com/uniqueqamar/landsat-cloud-reconstruction/commit/e2ca3a7a840e830bdb9e12519f1d37f22c71f1f7), [tests](https://github.com/uniqueqamar/landsat-cloud-reconstruction/commit/9e55f04912f07780f7f20efd5d6e1cce11ec82b8).
- **Change:** Added masked reflectance MAE/RMSE/PSNR, per-band error, spectral angle mapper, NDVI error, and mean-fill comparison on identical masks. Complements open PR #1 without changing the evaluation CLI.
- **Verification:** Six isolated synthetic pytest tests passed (0.08 s), Python compilation passed. Full repo, real validation checkpoint, CI, unseen-scene evaluation and training **not performed**. No improved model accuracy claimed.
- **Next:** Run spectral audit on held-out validation patches, inspect worst bands/land cover, obtain genuinely geographically distinct scenes and temporal inputs, train and benchmark candidate v3 on validation only. Existing demo and access unchanged.
- **Learning:** Spectral angle, NDVI band indexing, masked denominators, geographic distribution shift.
