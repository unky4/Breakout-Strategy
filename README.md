# Udacity's Artificial Intelligence for Trading Nanodegree

## Project: Trading with Momentum

### Summary of What I Learned

I learned how to implement another strategy from a hypothesis and create short and long signals that we can use to trade stocks. Although our return showed some profit potential, we again saw how vital it is to do statistical tests on our results. First, I learned how to utilise histograms and QQ plots to conclude that outliers skewed our results, and then I learned to use the Kolmogorov-Smirnov test to find these outliers and filter them out for more realistic returns.

## Table of content

1. [Project Overview](#overview)
2. [Data Description](#data)
3. [The Alpha Research Process](#research_process)
4. [Hypothesis](#strategy)
5. [Signal Generation and Return](#signal)
6. [Statistical Tests](#test)
7. [Results and Conclusions](#conclusions)
8. [Packages and Files](#packages)

***

<a id='overview'></a>
### Project Overview

In this project, we implement the breakout strategy. On the one hand, we attempt to find and remove any outliers, and on the other hand, we aim to test to see if it has the potential to be profitable using a Histogram and P-value. For the dataset, we use the end of day data from Quotemedia.

<a id='data'></a>
### Data Description

To cover all vital elements of this project, Udacity has created a scenario where companies mining Terbium are making huge profits, expanding/modifying the actual market data this way. Therefore, all the companies in this market sector are made up, representing a sector with significant growth. This (artificial) database is based on the end of day data from [Quotemedia](https://www.quotemedia.com/).

Unfortunately, Udacity does not have a [licence](https://github.com/udacity/artificial-intelligence-for-trading) to redistribute these data; however, they are working on alternatives to this problem.

<a id='research_process'></a>
The Alpha Research Process

Understanding where the various steps fit in the alpha research workflow is essential. Since the signal-to-noise ratio in trading signals is meagre, it is very easy to fall into the trap of overfitting to noise, and so it is inadvisable to jump right into signal coding. To help mitigate these overfitting issues, we start with a general observation and hypothesis; i.e., before we touch the data, we should be able to answer the following question: 

> What feature of markets or investor behaviour would lead to a persistent anomaly that our signal will try to use?

<a id='hypothesis'></a>
### Hypothesis

Ideally, the assumptions behind the hypothesis are testable before coding and evaluating the signal itself. The workflow, therefore, is as follows:

![Workflow](/images/workflow.png)

In this project, we assume that the first three steps have already been performed ("observe & research", "form hypothesis", "validate hypothesis"). The hypothesis for this project is the following:

- In the absence of news or significant investor trading interest, stocks oscillate in a range.
- Traders seek to capitalise on this range-bound behaviour periodically by selling/shorting at the top of the range and buying/covering at the bottom of the range. This behaviour reinforces the existence of the range.
- When stocks break out of the range, due to, e.g., a significant news release or from market pressure from a large investor:
  - the liquidity traders who have been providing liquidity at the bounds of the range seek to cover their positions to mitigate losses, thus magnifying the move out of the range, and
  - the move out of the range attracts other investor interest; these investors, due to the behavioural bias of herding (e.g., [Herd Behavior](https://www.investopedia.com/terms/h/herdinstinct.asp)) build positions that favour continuation of the trend.

<a id='signal'></a>
### Signal Generation and Return

To generate an indicator for this strategy, we utilise the price highs and lows, enabling us to create long and short signals using a breakout strategy. However, when we are already shorting a stock, having an additional signal to short a stock is not helpful for this strategy (also valid for long positions). Hence, we also need to clear the generated signal.

Now we can calculate how many days to short or long the stocks and generate the log price return between the closing price and the lookahead price. Ultimately, using these prices, we can create the signal returns.

<a id='test'></a>
### Statistical Tests

To check whether our log-returns are normal, we can plot a histogram as well as a [QQ](https://towardsdatascience.com/q-q-plots-explained-5aa8495426c0) (quantile-quantile) plot.

Furthermore, we can perform a Kolmogorov-Smirnov test to find stocks that are causing suspicious returns (i.e. outliers). This test is run on a normal distribution against each ticker's signal returns (where a long or short signal exits and compare).

Then, with the ks and p-values calculated, we can find which symbols are the outliers and deal with them accordingly. Once we have done that, we can re-plot the histograms and QQ plots.

<a id='conclusion'></a>
### Results and Conclusions

Since we are dealing with log returns, all three cases should yield a slightly skewed normal distribution (a positively skewed one for profitability).

![Results Histogram](/images/results_histogram.png)

Although we see three skewed distributions to the right (positively skewed) (implying that we might found a profitable strategy due to the positiveness), a significant bump on the right exists in all three cases. This tells me that these skewnesses are due to outliers. Since these outliers appear on the right, we can safely say that they generate a substantial amount of (false) return for us. If we had removed these outliers from the returns, our profits would definitely be lower, and it might be the case that we even lose money. Therefore, we can confidently say that the amount of returns generated by the three models using the backtest does not reflect the amount of returns we would make if we had used these strategies in the future.

![QQ plots](/images/results_qq.png)

From the QQ plots, we can further see that all distributions have fat tails (both ends), which is another worrying sign that this strategy would not work if deployed.

Once we have removed the outliers detected by the KS test, we can compare the log-returns with normal distributions. As we can see, our returns are now closer to a normal distribution.

![Without Outliers](/images/results_without_outliers.png.png)

<a id='packages'></a>
### Packages and Files

Custom packages:
- `helper` and `project_helper` packages contain utility and graph functions.
- `project_tests` include all unit tests for this project
- `tests` generates various test cases for the unit tests

The necessary libraries defined in `requirements.txt` are the followings:
- [colour==0.1.5](https://github.com/vaab/colour)
- [cvxpy==1.0.3](https://github.com/cvxgrp/cvxpy/)
- [cycler==0.10.0](https://matplotlib.org/cycler/)
- [numpy==1.13.3](http://www.numpy.org/)
- [pandas==0.21.1](https://github.com/pandas-dev/pandas)
- [plotly==2.2.3](https://plot.ly/python/)
- [pyparsing==2.2.0](https://github.com/pyparsing/pyparsing/)
- [python-dateutil==2.6.1](https://dateutil.readthedocs.io/en/stable/)
- [pytz==2017.3](https://pythonhosted.org/pytz/)
- [requests==2.18.4](http://docs.python-requests.org/en/master/)
- [scipy==1.0.0](https://www.scipy.org/)
- [scikit-learn==0.19.1](https://scikit-learn.org/stable/)
- [six==1.11.0](https://github.com/benjaminp/six)
- [tqdm==4.19.5](https://tqdm.github.io/)
- [zipline==1.2.0](https://github.com/quantopian/zipline)