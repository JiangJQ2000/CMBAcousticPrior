# CMBAcousticPrior

A Gaussian prior on (θ<sub>*</sub>, ω<sub>b</sub>, ω<sub>bc</sub>) from the CMB that does not assume a late-time cosmology, where ω<sub>bc</sub> = ω<sub>b</sub> + ω<sub>c</sub>. Use it in place of the full CMB likelihood when you combine CMB with BAO, supernovae or other late-time data in non-standard late-time models.

**Config generator.** Pick a dataset and a code, then copy the config from [the webpage](https://JiangJQ2000.github.io/CMBAcousticPrior/).

## Quick start

Nothing to install beyond Cobaya and a Boltzmann code. Every config contains the full likelihood.

```bash
pip install cobaya camb      # or: pip install cobaya classy
cobaya-run configs/baseline_camb.yaml
cobaya-run configs/baseline_class.yaml
```

To add the prior to your own run, copy the `cmb_acoustic:` block from the `likelihood:` section of any config into your YAML. Use the CAMB or CLASS version to match your theory code.

## Measurements

| CMB data | 100θ<sub>*</sub> | ω<sub>b</sub> | ω<sub>bc</sub> | Configs |
|---|---|---|---|---|
| Planck PR4 + ACT DR6 + SPT-3G D1 (**baseline**) | 1.04100 ± 0.00021 | 0.022440 ± 0.000099 | 0.1410 ± 0.0011 | `configs/baseline_{camb,class}.yaml` |
| Planck PR4 | 1.04107 ± 0.00026 | 0.022263 ± 0.000158 | 0.1412 ± 0.0013 | `configs/PR4_*.yaml` |
| ACT DR6 | 1.04086 ± 0.00035 | 0.022588 ± 0.000166 | 0.1456 ± 0.0032 | `configs/ACTDR6_*.yaml` |
| SPT-3G D1 | 1.04087 ± 0.00068 | 0.022222 ± 0.000214 | 0.1453 ± 0.0053 | `configs/SPT3GD1_*.yaml` |
| WMAP + ACT DR6 + SPT-3G D1 | 1.04095 ± 0.00029 | 0.022495 ± 0.000111 | 0.1434 ± 0.0022 | `configs/WMAP_ACTDR6_SPT3GD1_*.yaml` |
| Planck PR4 + ACT DR6 + SPT-3G D1, without low-ℓ EE | 1.04100 ± 0.00022 | 0.022440 ± 0.000099 | 0.1409 ± 0.0011 | `configs/baseline_no_lowlEE_*.yaml` |

The means and covariances are in `data/<name>.txt`. The first row is the mean of (θ<sub>*</sub>, ω<sub>b</sub>, ω<sub>bc</sub>), with θ<sub>*</sub> in radians, not 100θ<sub>*</sub>. The next three rows are the covariance.

The raw chains for the baseline dataset are available in the [release](https://github.com/JiangJQ2000/CMBAcousticPrior/releases).

## CAMB and CLASS

The prior's θ<sub>*</sub> is CAMB's `thetastar`, computed with CosmoRec recombination.

- **CAMB:** if your CAMB build includes CosmoRec, uncomment `recombination_model: CosmoRec` in the config. With CAMB's default recombination, 100θ<sub>*</sub> shifts by −2.4×10<sup>−5</sup>, about 0.1σ of the baseline error.
- **CLASS:** keep the CLASS settings in the configs as they are.
  - **`reio_parametrization: reio_none` is required.** CLASS's `theta_star_100` otherwise includes reionization and comes out about 15σ higher. With `reio_none`, CLASS and CAMB with CosmoRec agree to Δχ² ≈ 0.005 near the best fit.
  - **List every parameter you sample in `input_params` under `classy`.** For example, add `w0_fld` and `wa_fld` if you sample them. CLASS only receives the sampled parameters listed there.

## Citation

If you use this prior, please cite:

K. Lodha, J.-Q. Jiang, P. Mukherjee, A. Shafieloo, *Acoustic priors from the latest cosmic microwave background data*, [arXiv:2611.33334](https://arxiv.org/abs/2611.33334).
