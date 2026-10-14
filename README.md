# Composite Sampler

Code, numerical results, and figures accompanying
[A proximal gradient algorithm for composite log-concave sampling](https://arxiv.org/abs/2605.12461)
by Linghai Liu and Sinho Chewi.

## Citation

The paper is currently in submission. Our work can be cited as:

> Linghai Liu and Sinho Chewi. *A proximal gradient algorithm for composite
> log-concave sampling*. arXiv:2605.12461, 2026.

```bibtex
@misc{liu2026proximalgradient,
  title         = {A proximal gradient algorithm for composite log-concave sampling},
  author        = {Linghai Liu and Sinho Chewi},
  year          = {2026},
  eprint        = {2605.12461},
  archivePrefix = {arXiv},
  url           = {https://arxiv.org/abs/2605.12461}
}
```

## Repository structure

```text
composite_sampler/
├── composite_logconcave_sampler_experiments.ipynb
├── README.md
├── experiment1/    # Sparse logistic regression
├── experiment2/    # Box-constrained logistic regression
└── experiment3/    # Dimension scaling
```

## Code

| File | Description |
| --- | --- |
| `composite_logconcave_sampler_experiments.ipynb` | Jupyter notebook containing the sampler implementations, synthetic-data generation, numerical experiments, diagnostics, and plotting routines. |

Run the notebook from the repository root. The three experiment directories
contain the saved results accompanying the paper. New notebook runs save
figures in `composite_sampler_figures/` and JSON summaries in the working
directory.

## Experiment 1: Sparse logistic regression

The `experiment1/` directory contains results for Bayesian logistic regression
with an $\ell_1$ prior.

| File | Description |
| --- | --- |
| `logistic_l1_running_rmse.csv` | Running posterior-mean RMSE versus gradient evaluations for the three samplers. |
| `logistic_l1_marginal_intervals.csv` | Per-coordinate posterior means and 5th/95th percentiles, comparing the Composite Sampler with the Prox-MALA reference. |
| `logistic_l1_mean_rmse.pdf` | Plot of the running posterior-mean RMSE. |
| `logistic_l1_intervals.pdf` | Plot comparing marginal posterior intervals. |

**`logistic_l1_running_rmse.csv`** contains one row per method and recorded iteration:

- `method`: sampler name.
- `grad_evals`: number of gradient evaluations.
- `running_rmse`: RMSE of the running posterior mean relative to the reference.

**`logistic_l1_marginal_intervals.csv`** contains one row per coefficient:

- `coefficient_index`: coefficient index.
- `reference_q05`, `reference_mean`, `reference_q95`: reference posterior summaries.
- `composite_q05`, `composite_mean`, `composite_q95`: Composite Sampler posterior summaries.

## Experiment 2: Box-constrained logistic regression

The `experiment2/` directory contains results for Bayesian logistic regression
with a box constraint on the coefficients.

| File | Description |
| --- | --- |
| `box_logistic_running_rmse.csv` | Running posterior-mean RMSE versus gradient evaluations for the three samplers. |
| `box_logistic_mean_rmse.pdf` | Plot of the running posterior-mean RMSE. |

The CSV uses the same columns as `logistic_l1_running_rmse.csv`:
`method`, `grad_evals`, and `running_rmse`.

## Experiment 3: Dimension scaling

The `experiment3/` directory contains results for Gaussian sampling with box
constraints, evaluated at dimensions
$d \in \{4, 8, 16, 32, 64, 128, 256, 512\}$.

| File | Description |
| --- | --- |
| `scaling_curves.csv` | Sliced Wasserstein-2 distance versus gradient evaluations, indexed by dimension, method, and seed. |
| `scaling_costs.csv` | Gradient evaluations needed to first reach the prescribed distance threshold, together with the final distance for each run. |
| `scaling_summary.txt` | Two summary tables: median cost to reach the threshold and median final distance, grouped by dimension and method. |
| `scaling_cost_vs_d.pdf` | Plot of cost to reach the threshold versus dimension. |
| `scaling_running_w2_d{d}.pdf` | Eight plots of sliced Wasserstein-2 distance versus gradient evaluations, one for each dimension listed above. |

**`scaling_curves.csv`** contains the columns `d`, `method`, `seed`,
`grad_evals`, and `sliced_w2`. Each row records a distance measurement for
one dimension, method, seed, and evaluation count.

**`scaling_costs.csv`** contains the columns `d`, `method`, `seed`,
`cost_to_threshold`, and `final_w2`. Each row summarizes one run.
A missing `cost_to_threshold` means that the method did not reach the
threshold within that run's computational budget.
