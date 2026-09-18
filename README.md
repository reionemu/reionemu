[![CI](https://github.com/reionemu/reionemu/actions/workflows/ci.yml/badge.svg)](https://github.com/reionemu/reionemu/actions/workflows/ci.yml)
[![Docs](https://img.shields.io/badge/docs-online-blue)](https://reionemu.org/reionemu/)
[![PyPI](https://img.shields.io/pypi/v/reionemu)](https://pypi.org/project/reionemu/)
[![Python](https://img.shields.io/badge/python-3.10%20%7C%203.11%20%7C%203.12%20%7C%203.13-blue)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![DOI](https://img.shields.io/badge/DOI-10.5281%2Fzenodo.21766410-blue)](https://doi.org/10.5281/zenodo.21766410)

<p align="center">
    <img src="https://raw.githubusercontent.com/reionemu/reionemu/main/docs/assets/reionemu-logo.png" alt="reionemu logo" width="300">
</p>

# reionemu

Machine-learning emulators for the kinetic Sunyaev-Zel'dovich (kSZ) angular power spectrum from reionization simulations, with uncertainty quantification.

The kSZ effect arises when CMB photons scatter off free electrons with bulk motion, and its angular power spectrum carries information about the timing, duration, and structure of reionization. `reionemu` learns the mapping of reionization parameters to binned kSZ power spectra, so parameter space can be explored without rerunning expensive simulations.

## Installation

```bash
pip install reionemu
```

Requires Python 3.10+. Installs NumPy, h5py, PyTorch, and Ray Tune.

## Quick start

Train the MC-dropout emulator on a condensed HDF5 dataset, then predict with uncertainties:

```python
from pathlib import Path

import numpy as np
import torch

import reionemu

# An HDF5 file that already contains /training (X, Y, ell)
h5_path = Path("condensed.h5")

loaders, normalizers, ell = reionemu.make_dataloaders(
    h5_path,
    split={"train": 0.8, "val": 0.2},
    config=reionemu.DataLoaderConfig(batch_size=32, seed=42),
)

model = reionemu.MCDropoutEmulator(dropout_rate=0.1)
history = reionemu.fit(
    model,
    loaders["train"],
    loaders["val"],
    torch.optim.Adam(model.parameters(), lr=1e-3),
    torch.nn.MSELoss(),
    config=reionemu.FitConfig(epochs=10, device="cpu"),
)

# (zmean_zre, alpha_zre, kb_zre, b0_zre)
theta = np.array([[8.0, 0.5, 1.0, 0.45]], dtype=np.float32)

pred_mean, pred_std, _, _ = reionemu.predict_mc(
    theta,
    model,
    X_mean=normalizers["X"].mean,
    X_std=normalizers["X"].std,
    n_mc_samples=200,
)

print(pred_mean, pred_std)
```

## What's included

| Module | Covers                                                                                |
| --- |---------------------------------------------------------------------------------------|
| `simio` | Condense simulation outputs, compute flat-sky power spectra, build training arrays    |
| `data` | Dataloaders, train/validation splits, normalization                                   |
| `models` | Deterministic and MC-dropout emulator architectures                                   |
| `training` | Training loop, K-fold cross-validation, metrics, model builders                       |
| `tuning` | Ray Tune hyperparameter search                                                        |
| `artifact` | JSON experiment manifests, normalizers, checkpoints                                   |

Everything stable is re-exported at the top level. Experimental variants live under `reionemu.models.experimental`.

## Documentation

Full API reference, guides, and a worked pipeline example: **[reionemu.org/reionemu](https://reionemu.org/reionemu/)**

## Citation

If `reionemu` contributes to work you publish, please cite both the software and the relevant paper.

### Software

This entry uses the concept DOI, which always resolves to the latest release.

```bibtex
@software{pearce_reionemu,
  author    = {Pearce, Robert},
  title     = {{reionemu: Python package for emulating the kSZ angular power spectrum from reionization simulations}},
  year      = {2026},
  publisher = {Zenodo},
  doi       = {10.5281/zenodo.21766410},
  url       = {https://doi.org/10.5281/zenodo.21766410},
}
```

GitHub's **Cite this repository** button generates BibTeX and APA from [`CITATION.cff`](https://github.com/reionemu/reionemu/blob/main/CITATION.cff) automatically for the most current release.

### Papers

*An Uncertainty-Aware Machine Learning Emulator for the Reionisation kSZ Power Spectrum*, Robert Pearce and Paul La Plante, in preparation (2026).

- Notebooks, scripts, datasets, and figures for the paper live in [reionemu/reionemu-pasa-2026](https://github.com/reionemu/reionemu-pasa-2026), which installs `reionemu` from PyPI.

## Acknowledgments

Developed in the LEADS Lab at the University of Nevada, Las Vegas, under [Dr. Paul La Plante](https://plaplant.github.io/), with computing resources from the Pittsburgh Supercomputing Center (Bridges-2).

Released under the [MIT License](https://github.com/reionemu/reionemu/blob/main/LICENSE).
