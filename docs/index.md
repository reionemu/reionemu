---
hide:
  - toc
---

<div class="hero">
  <img src="assets/reionemu-logo.png" alt="reionemu logo" class="hero-logo">
  <p class="hero-kicker">Machine-learning emulator for reionization-era kSZ science</p>
  <h1>A fast emulator for the kinetic Sunyaev-Zel'dovich power spectrum</h1>
  <p class="hero-copy">
    <code>reionemu</code> helps turn simulation outputs into trainable datasets, emulator models,
    and reusable workflows for exploring reionization parameter space without rerunning expensive simulations.
  </p>
  <div class="hero-actions">
    <a class="md-button md-button--primary" href="getting-started/">Get Started</a>
    <a class="md-button" href="api-overview/">Browse API</a>
  </div>
</div>

## What the package covers

<div class="feature-grid">
  <div class="feature-card">
    <h3>Simulation to dataset</h3>
    <p>Condense raw outputs, compute flat-sky power spectra, and assemble training-ready HDF5 datasets.</p>
  </div>
  <div class="feature-card">
    <h3>Training workflows</h3>
    <p>Build dataloaders, train deterministic or MC-dropout emulators, and evaluate validation performance with reusable utilities.</p>
  </div>
  <div class="feature-card">
    <h3>Search and tuning</h3>
    <p>Run Ray Tune experiments to explore architecture and optimizer choices for the deterministic four-parameter emulator.</p>
  </div>
  <div class="feature-card">
    <h3>Experiment artifacts</h3>
    <p>Save JSON manifests, configs, results, normalizers, and model checkpoints for reproducible emulator runs.</p>
  </div>
</div>

## Start here

- [Getting Started](getting-started.md) covers installation, a quick check that the package imports, and a first training run.
- [API Overview](api-overview.md) maps the public API onto the pipeline, from simulation output to saved experiment artifact.

## Repository layout

The [reionemu/reionemu](https://github.com/reionemu/reionemu) repository holds only the package and its documentation:

- Core Package: `src/reionemu/`
- Documentation Source: `docs/`

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
