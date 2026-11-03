# Contribution and learning log

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
