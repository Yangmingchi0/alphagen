# AlphaGen Community Maintenance Guide

> **Status:** community-maintained documentation and reproducibility notes for the personal fork [Yangmingchi0/alphagen](https://github.com/Yangmingchi0/alphagen). The canonical research repository remains [ICT-FinD-Lab/alphagen](https://github.com/ICT-FinD-Lab/alphagen).

## Scope and boundaries

This fork exists to organize reproducibility work without changing the authors' repository. It does **not** replace the upstream project, speak for the paper authors, or change the upstream licensing status. No explicit LICENSE file was visible in the upstream repository during the September 2026 audit; reuse and redistribution must therefore be handled conservatively until the copyright holders publish licensing terms.

The first maintenance pass is documentation-only. It records the highest-impact reproduction problems reported by users and defines a test plan. It does not claim that the full KDD 2023 or HARLA/FCS 2026 experiments have already been reproduced.

## Why this project is prioritized

AlphaGen is FinD Lab's most visible public project, with more than one thousand GitHub stars and hundreds of forks. Its open issue queue is dominated by environment setup, Qlib compatibility, data preparation, runtime failures, and result reproduction. Improving the path from clone to a verified smoke test should therefore have the largest immediate community impact.

## Reproducibility audit

| Area | Current upstream signal | Maintenance action |
| --- | --- | --- |
| Python dependencies | requirements.txt exists, but users report outdated or conflicting versions | Build a tested environment matrix before changing pins |
| Qlib integration | Multiple reports concern the Qlib package/version and REG_CN imports | Record the exact package source, version, and initialization path used by each verified run |
| NumPy and compiled packages | Users report NumPy and segmentation-fault failures | Test versions together; avoid isolated upgrades without a complete run |
| Market data | The README describes Qlib metadata plus Baostock data | Document data dates, adjustment method, storage path, and redistribution limits |
| Experiment entry points | Several scripts and baselines are available | Add one minimal smoke test before attempting full paper reproduction |
| Results and checkpoints | Outputs are written to configurable paths | Publish only logs or artifacts that the original license and data terms permit |

## High-priority upstream issues

These links point to the original repository so discussion remains attributable to the project authors:

- [#62 — Qlib dependency is incorrect](https://github.com/ICT-FinD-Lab/alphagen/issues/62)
- [#59 — NumPy dependency issues](https://github.com/ICT-FinD-Lab/alphagen/issues/59)
- [#57 — Dependency versions are too old](https://github.com/ICT-FinD-Lab/alphagen/issues/57)
- [#56 — REG_CN import failure](https://github.com/ICT-FinD-Lab/alphagen/issues/56)
- [#60 — Reproduction failure and requested alpha factors](https://github.com/ICT-FinD-Lab/alphagen/issues/60)
- [#47 — How to reproduce the paper results](https://github.com/ICT-FinD-Lab/alphagen/issues/47)
- [#53 — Calendar/index boundary error](https://github.com/ICT-FinD-Lab/alphagen/issues/53)

## Verification protocol

A result should be marked **verified** only when the following information is recorded:

1. Upstream commit SHA and fork commit SHA.
2. Operating system, Python, CUDA, GPU/CPU, PyTorch, Qlib, NumPy, and Stable-Baselines3 versions.
3. Data source, retrieval date, adjustment convention, universe, and train/validation/test periods.
4. Exact command, configuration, random seed, and expected runtime class.
5. Exit status plus a small set of sanity checks, such as dataset shape and non-empty output files.
6. A short note explaining any difference from the paper or upstream README.

## Planned maintenance sequence

### P0 — Environment and smoke test

- Establish one conservative environment from the upstream dependency set.
- Verify imports and data-path initialization before running training.
- Run a short, explicitly non-benchmark smoke test.
- Record failures without silently changing algorithmic behavior.

### P1 — Reproduction documentation

- Add a tested quick-start command and expected output shape.
- Separate data preparation, training, evaluation, and backtesting instructions.
- Add troubleshooting entries for recurring Qlib, NumPy, calendar, and memory problems.

### P2 — Compatibility work

- Evaluate newer supported dependency combinations in separate branches.
- Keep compatibility fixes isolated from research-method changes.
- Add lightweight automated checks only after the minimal run is understood.

## Contribution policy for this fork

Documentation corrections, reproducibility logs, and narrowly scoped compatibility patches are welcome. Please link the related upstream issue when one exists. Do not upload proprietary market data, private credentials, commercial model outputs, or artifacts whose redistribution terms are unclear.

For questions about the original method or official results, use the [upstream issue tracker](https://github.com/ICT-FinD-Lab/alphagen/issues). For this fork's maintenance notes, use the personal fork's issue tracker.

---

Audit date: **2026-09-28** · Maintainer: [Yangmingchi0](https://github.com/Yangmingchi0)
