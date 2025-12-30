**Rationally Turbulent Expectations Model**

Kent Osband

The underlying model is described in Chapter 7 of my book Rationally Turbulent Expectations, which is freely available on [SSRN](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5202486), and was not designed with any attention to Q-variance. I assume only that:

- Equities discount continuous dividend streams that have constant volatility and Markov-switching drifts.
- Traders are Perfect Learners, who know the underlying parameters and infer drifts optimally from evidence on dividends.
- Traders are also fractional Kelly gamblers, whose Kelly fraction of 0.75 is close to optimal given their uncertainties.
- Equity prices include just enough risk premia to ensure that supply equals trader demand. 

Like in the book, the drifts and volatility are fitted to Shiller’s data on real returns of the S&P, using dividends only through 1950 and mixing in 25% earnings since then to mitigate the well-documented dividend smoothing. Unlike in the book, this model fits three drift states, with estimated log rates of -0.3245, 0.0374, and 0.4572. The instantaneous generator matrix is [-1.139.1.1186,0.0307; 0.1054,-0.1485,0.0432; 0.998,1.5143,-1.6141], with implied stationary probabilities of 8.39%, 89.07%, and 2.54%. 

Fair market prices were estimated by simulating daily trading 252 days a year for a million years, sorting beliefs by mean and variance into 5,000 equal-sized bins, and iterating to convergence given a risk-free yearly discount rate of 0.01. How real-life traders would grope toward this solution is an open question, perhaps better modeled using David Orrell’s quantum methodology.

<img src="Figure_1_3State.png" width="800">
