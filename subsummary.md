# Summary of submissions as of end-2025

We have now had a number of excellent entries to the competition (not all of whom posted to GitHub) and wish to thank all the participants. This note summarises again the question, and the answers received so far. Although the competition was originally framed as an end-of-year challenge, it remains open.

Q-variance is all about the coefficient of $z^2$. The model showed it must be 0.5. Some common models may show a quadratic, and some may even show the coefficient is independent of period $T$. But that coefficient is a parameter in the models, it can be chosen (for example by adjusting a shape factor). None have to have 0.5. 

This competition goes further, by asking – not if a continuous-time model can predict q-variance – but whether any such model can even **produce** q-variance, in a reliable fashion, using up to three parameters: a base volatility, a horizontal offset, and an extra parameter. The last might be something that is fit to data, or it could be a factor which is selected with the sole justification of matching q-variance.

The full list of submissions is given below. Four submissions (numbers 2 to 5 in the list) drew on the idea of sampling variance $v$ from an inverse-gamma distribution with shape factor set to $\alpha = 3/2$ and rate equal to a base variance $\sigma^2$. While this distribution does agree in principle with q-variance, you still have to find a time series which matches it over all periods $T$. 

[Entry 2](https://github.com/q-variance/challenge/tree/main/submissions/simu.ai) (since withdrawn) tackled this in an ingenious way by adding an extra layer of regime switching, however this put the number of tuneable parameters over the limit. It also couldn’t address the other problem, which is that the inverse-gamma distribution is extremely noisy, with huge spikes, and the variance of $v$ (i.e. the variance-of-variance) is infinite. The result is that the model (and its $R^2$ score) only converges over simulations of thousands of years, and even then is sensitive to whether the selected time period contains a certain spike. 

Entries 6 to 8 used a stochastic volatility approach, however the models either introduced extra parameters or did not agree with q-variance to the required standard. Entry 9 used an optimized [GARCH(1,1) model](https://github.com/q-variance/challenge/tree/main/submissions/tingjun2) to obtain a good fit, but required four main parameters, plus a fifth to keep the model stable (the parameters were in a region where the variance-of-variance was again infinite). 

Most entries have tackled the challenge by attempting to reverse-engineer the figure. A different approach was taken in [entry 10](https://github.com/q-variance/challenge/tree/main/submissions/Kent) which showed that a preexisting model produced an approximate version of q-variance. While the model had more than three parameters, it was a rare example of a model which naturally produces the desired kind of behaviour without special recalibration.

One thing raised by the competition is that entries which, on paper at least, should satisfy q-variance, often also turn out to be impossible to calibrate, without adding extra parameters to stabilize the model. For example, in both the inverse-gamma and GARCH(1,1) entries, the variance density has a particular power-law scaling in the wings which is compatible with q-variance. However the same property means that the variance-of-variance is infinite. It turns out that, given reasonable assumptions, any continuous-time model which satisfies q-variance also has infinite variance-of-variance, so is highly unstable (see technical explanation below). Also the resulting log price change distribution is unrealistically fat-tailed. This doesn't conclusively rule out such a model, but it helps explain why finding one is so difficult, and obviously raises a host of issues for the whole continuous-time approach.

Entries are evaluated on the number of free parameters (see the README) and the $R^2$ score (which has to converge instead of just holding for a particular submission period). In the end, none of the submissions managed to achieve accuracy using the competition limit of three parameters, but some came near using more. We therefore have no firm winner, but a number of close entries (Wilmott magazine to announce later). 

---

**List of entries**

*Target: global r² ≥ 0.995 using ≤3 free parameters (continuous-time models only)*

| # | Submission        | Author        | Parameters                                                                 | Status     | Date       | Notes |
|---|-------------------|---------------|----------------------------------------------------------------------------|------------|------------|-------|
| 1 | stochastic vol    | Grok          | roughness, leverage, vol-of-vol, initial variance, offset                  | rejected   | 2025-11-30 | Thank you for playing. |
| 2 | simu.ai           | Thijs         | σ, drift, shape factor 3/2, sampling rate                                  | withdrawn  | 2025-12-11 | Variance drawn from inverse-Gamma (shape 3/2) with regime switching. Q-variance only appears over very long time horizons; score drops below threshold for reasonable changes in sampling parameters. |
| 3 | tags              | Edouard       | σ, drift, shape factor 3/2                                                  | TBD        | 2025-12-14 | Inverse-Gamma entry. Variance is undefined, leading to instability and lack of reliable convergence. |
| 4 | —                 | Aleh          | σ, drift, shape factor 3/2                                                  | TBD        | 2025-12-14 | Inverse-Gamma model. Converges in theory over very long times, but q-variance would only be observable over centuries of data. |
| 5 | —                 | ShengQuan     | σ, drift, shape factor 3/2                                                  | TBD        | 2025-12-14 | Inverse-Gamma model with the same sampling limitations. |
| 6 | Luke              | Luke          | drift, base volatility, jump probability per day, jump standard deviation  | TBD        | 2025-12-15 | Uses stochastic jumps; exceeds parameter limit. |
| 7 | equitquant.dev    | Dragon        | sigma, mu, kappa, c_int, a_shape                                            | TBD        | 2025-12-24 | Uses stochastic shocks; exceeds parameter limit; result depends on simulation time. |
| 8 | tingjun           | tingjun       | drift, base volatility, b                                                   | withdrawn  | 2025-12-24 | Uses stochastic volatility; result depends on simulation time. |
| 9 | tingjun           | tingjun       | drift, base volatility, alpha, beta, volatility cap                        | TBD        | 2025-12-27 | GARCH(1,1), requires extra parameters to stabilize.|
|10 | Kent              | Kent Osband   | multiple (>3)                                                               | TBD        | 2025-12-30 | Based on *Rationally Turbulent Expectations*. |

---

**Q-variance makes classical models unstable**

As seen with a number of entries, models which approximate q-variance are often quite unstable, so require extra parameters to limit fluctuations. One reason for this behaviour is that if we impose q-variance exactly, then the variance-of-variance for a classical model is undefined.

To see this, suppose a classical continuous-time model represents the variance $$V$$ over a period $$T$$ as a random variable. Assume that $$z | V$$ is conditionally Gaussian, and apply Bayes' formula to obtain

$$
\mathbb{E}[V \mid z] = \frac{\int_0^\infty V  p(z \mid V) p(V) dV}{\int_0^\infty p(z \mid V) p(V) dV} = \frac{\int_0^\infty V^{1/2} e^{-z^2/(2V)} p(V) dV}{\int_0^\infty V^{-1/2} e^{-z^2/(2V)} p(V) dV}.
$$

If the density $$p(V)$$ decays exponentially or faster for large $$V$$ (as in classical diffusive stochastic volatility models), then $$\mathbb{E}[V\mid z]$$ grows subquadratically in $$z$$, so exact q-variance cannot hold. We therefore suppose instead that the density of $$V$$ decays with a regularly varying tail, so $$p(V)$$ varies with $$C V^{-1-\alpha}$$ as $$V \to \infty$$ for some $$C>0$$ and $$\alpha>0$$. This is the case for example for a GARCH(1,1) model, or when the variance is given by an inverse-gamma distribution.

For $$z \neq 0$$, make the change of variables $$V=z^2 u$$, yielding

$$
\mathbb{E}[V \mid z] = z^2 \frac{\int_0^\infty u^{1/2} e^{-1/(2u)} p(z^2 u) du}{\int_0^\infty u^{-1/2} e^{-1/(2u)} p(z^2 u) du} \sim k(\alpha) z^2
$$

where

$$
k(\alpha) = \frac{\int_0^\infty u^{-\alpha-1/2} e^{-1/(2u)} du}{\int_0^\infty u^{-\alpha-3/2} e^{-1/(2u)} du} = \frac{1}{2\left(\alpha - \frac{1}{2}\right)}.
$$

Exact q-variance requires the quadratic coefficient in $$z^2$$ to equal $$1/2$$, so $$k(\alpha)=1/2$$ and $$\alpha = 3/2$$. However, for a tail $$p(V)\sim C V^{-1-\alpha}$$ one has $$\mathbb{E}[V^2]<\infty$$ if and only if $$\alpha>2$$. Q-variance therefore places the model in a zone where the variance-of-variance diverges. 
