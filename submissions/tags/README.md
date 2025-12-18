# Team: tags (Edouard Tallent)
# December 2025
# First shot

## Model summary

We simulate a continuous-time GBM with **daily stochastic variance** drawn from an
Inverse-Gamma distribution with shape 3/2:

- Instantaneous variance per year on day t:  
  $\( V_t \sim \mathrm{InvGamma}(\alpha = 3/2, \beta = \sigma_0^2) \)$
- Daily time-step: $\( dt = 1/252 \)$
- Conditional on $\(V_t\)$, the daily log-return is
  $\[
  \Delta \log S_t = (\mu - 0.5 V_t) dt + \sqrt{V_t dt} \, \varepsilon_t,\quad
  \varepsilon_t \sim \mathcal{N}(0,1).
  \]$
- Price path:
  $\[
  S_{t+1} = S_t \exp(\Delta \log S_t).
  \]$

This is equivalent to a continuous-time GBM
$\(
  dS_t = S_t (\mu dt + \sqrt{V_t} dW_t)
\)$
with piecewise-constant volatility on each day.

## Parameters

- Shape of IG: **α = 3/2** (fixed by theory; not tuned).
- Scale of IG: **σ₀ = 0.259** (one free parameter).
- Drift: **μ = 0.0** (kept fixed; does not materially affect q-variance).
- Other settings (S₀, horizon length, RNG seed) are fixed and not tuned for R².

## Result

Using `code/data_loader_csv.py` followed by `code/score_submission.py`,
we obtain:

- Windows: ~3.09 million
- Canonical q-variance curve (σ₀ = 0.259, z₀ = 0.021):  
  **R² ≈ 0.9988**

