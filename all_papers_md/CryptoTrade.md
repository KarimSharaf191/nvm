# **CryptoTrade: A Reflective LLM-based Agent to Guide** **Zero-shot Cryptocurrency Trading**

**Yuan Li** _[∗]_ **, Bingqiao Luo** _[∗]_ **, Qian Wang** _[∗]_ **, Nuo Chen, Xu Liu, Bingsheng He**
National University of Singapore
li.yuan@u.nus.edu, luo.bingqiao@u.nus.edu, qiansoc@nus.edu.sg
nuochen@comp.nus.edu.sg, liuxu@comp.nus.edu.sg
hebs@comp.nus.edu.sg



**Abstract**


The utilization of Large Language Models
(LLMs) in financial trading has primarily been
concentrated within the stock market, aiding
in economic and financial decisions. Yet, the
unique opportunities presented by the cryptocurrency market, noted for its on-chain data’s
transparency and the critical influence of offchain signals like news, remain largely untapped by LLMs. This work aims to bridge
the gap by developing an LLM-based trading agent, CryptoTrade, which uniquely combines the analysis of on-chain and off-chain
data. This approach leverages the transparency
and immutability of on-chain data, as well
as the timeliness and influence of off-chain
signals, providing a comprehensive overview
of the cryptocurrency market. CryptoTrade
incorporates a reflective mechanism specifically engineered to refine its daily trading decisions by analyzing the outcomes of prior
trading decisions. This research makes two
significant contributions. Firstly, it broadens
the applicability of LLMs to the domain of
cryptocurrency trading. Secondly, it establishes a benchmark for cryptocurrency trading strategies. Through extensive experiments,
CryptoTrade has demonstrated superior performance in maximizing returns compared to
time-series baselines, but not compared to traditional trading signals, across various cryptocurrencies and market conditions. Our code
[and data are available at https://github.](https://github.com/Xtra-Computing/CryptoTrade)
[com/Xtra-Computing/CryptoTrade.](https://github.com/Xtra-Computing/CryptoTrade)


**1** **Introduction**

Large Language Models (LLMs) have revolutionized
financial decision-making and stock market prediction
by excelling in tasks such as sentiment analysis (Liang
et al., 2022) and explanation generation (Pu et al., 2023).
Specialized models like FinGPT and BloombergGPT
(Liu et al., 2023; Wu et al., 2023) demonstrate this capability. Recent research highlights their ability to interpret financial time-series and enhance cross-sequence


_∗_ Equal Contribution, Ordered Alphabetically



reasoning (Wei et al., 2022; Yu et al., 2023; Zhang et al.,
2023; Zhao et al., 2023; Yang et al., 2024). Furthermore, the development of LLM-based trading agents
like Sociodojo (Cheng and Chin, 2024) underscores the
potential for innovating investment strategies.
However, the application of LLMs in the cryptocurrency market remains underexplored, yet this field holds
great potential for future development for three main reasons. First, the cryptocurrency market is characterized
by high market value, volatility, and uncertainty, which
challenge traditional trading signals (Dro˙zd˙z et al., 2023;
Wei et al., 2023; Wang et al., 2024). Second, LLMs have
demonstrated their ability to understand and analyze
financial markets by leveraging large volumes of multimodal data, such as news and price information (Wu
et al., 2023). Third, the cryptocurrency market includes
open-sourced on-chain data, such as gas prices and total
transaction values, providing insights beyond just price
movements (Feichtinger et al., 2023; Ren et al., 2023;
Luo et al., 2023). To bridge this gap, we introduce CryptoTrade. By integrating on-chain data, including market
data and transaction records, with off-chain information like financial news, CryptoTrade leverages both
dimensions to execute daily trading strategies, taking
full advantage of the transparency of on-chain data and
the immediacy of off-chain information. We detail the
structure of CryptoTrade in Figure 1.
CryptoTrade consists of a three-part framework. Initially, we collect data where on-chain details such as
transactions and broader market data are aggregated
alongside off-chain data from established financial news
outlets like Bloomberg and Yahoo Finance. After data
collection, we perform statistical analyses to calculate
indicators such as moving averages, and apply text processing techniques for news summarization using the
same GPT models that will later be employed for analysis: GPT-3.5-turbo [1], GPT-4 [2], and GPT-4o [3] . Finally,
we enhance day-to-day decision-making with specialized analytical agents: market analyst agent evaluates
market trends, news analyst agent interprets recent news
impacts, and trading agent deliberates on investment


[1https://platform.openai.com/docs/](https://platform.openai.com/docs/models/gpt-3-5-turbo)
[models/gpt-3-5-turbo](https://platform.openai.com/docs/models/gpt-3-5-turbo)

[2https://platform.openai.com/docs/](https://platform.openai.com/docs/models/gpt-4)
[models/gpt-4](https://platform.openai.com/docs/models/gpt-4)

[3https://platform.openai.com/docs/](https://platform.openai.com/docs/models/gpt-4o)
[models/gpt-4o](https://platform.openai.com/docs/models/gpt-4o)



1094

_Proceedings of the 2024 Conference on Empirical Methods in Natural Language Processing_, pages 1094–1106
November 12-16, 2024 ©2024 Association for Computational Linguistics


CryptoTrade makes day-to-day trading decisions.



On-chain Data


Transactions


Market Data


Off-chain Data


News













Statistics


News Summary



Agent



Market Analyst

Agent



Trading Agent Reflection Agent







Figure 1: CryptoTrade Framework. Our framework begins with the collection of various types of data, including
on-chain transactions, market data, and off-chain data from multiple financial news sources. We extract on-chain
statistics while summarizing off-chain news to provide comprehensive inputs for our agents’ analysis. We then
deploy several LLM-based agents to make day-to-day trading decisions, utilizing a reflective mechanism to maximize
total returns over different time periods.



actions. Concurrently, reflection agent reviews past performance, allowing CryptoTrade to refine its strategies
to maximize returns.
Then, we conduct comprehensive experiments with
CryptoTrade using GPT-3.5-turbo, GPT-4, and GPT-4o,
evaluating its proficiency in making daily trading decisions for Bitcoin (BTC), Ethereum (ETH), and Solana
(SOL). These three cryptocurrencies were selected for
their prominence and market values of $134.14, $45.59,
and $7.61 billion, respectively, as of June 2nd, 2024 [4] .
CryptoTrade significantly outperforms time-series baselines such as Informer (Zhou et al., 2021) and PatchTST
(Nie et al., 2022), and achieves comparable performance
to trading signals like Moving Average Convergence Divergence (MACD) (Gencay, 1996) in both return and
sharpe ratio across bull, sideways, and bear market conditions. Notably, CryptoTrade operates in a zero-shot
manner without fine-tuning based on validation sets,
highlighting its potential for future applications. For
instance, during the ETH bullish test period, the Buy
and Hold strategy secured a 22.59% return, while CryptoTrade exceeded this by a remarkable 3%.
To summarize, we make the following three contributions:


  - We introduce CryptoTrade, an innovative trading
agent in the cryptocurrency domain, driven by
LLMs. CryptoTrade is designed to generate op

[4https://coinmarketcap.com/currencies/](https://coinmarketcap.com/currencies/)



timized trading decisions specifically for the cryptocurrency market, setting a new benchmark in this
field.


  - We develop a comprehensive framework for cryptocurrency trading agents that encompasses the collection of both on-chain and off-chain data, along
with the integration of a self-reflective component
to enhance decision-making processes. This approach aggregates diverse information sources and
establishes a new standard for data-driven trading
strategies within the cryptocurrency domain.


  - Through rigorous experiments, we present empirical evidence showcasing the efficacy of CryptoTrade compared to other baselines. CryptoTrade
advances the frontier of cryptocurrency trading
technologies and offers valuable insights for financial decision-making.


**2** **CryptoTrade Framework**


This section details the components employed to develop the CryptoTrade agent, including data collection,
market dynamics analysis, and agents development. Figure 1 shows an overview of CryptoTrade.


**2.1** **Data Collection**

The foundation of our methodology relies on a comprehensive collection of data from on-chain and off-chain



1095


sources, which is essential for making informed trading decisions in the cryptocurrency market. The data
license is detailed in Appendix A. The data ethics are
explained in Appendix B. The data collection strategy
is illustrated in Figure 1(a) and further detailed below:


  - **On-chain Data:** We leverage historical data from
CoinMarketCap [5], which provides daily insights
into prices, trading volumes, and market capitalization of various cryptocurrencies: BTC, ETH, SOL.
This dataset forms the backbone of our market
trend analysis, enabling us to decipher long-term
trends and identify cycles in cryptocurrency valuations and investor behavior.

Additionally, we incorporate detailed transaction statistics from on-chain activities. All
blockchain transactions are transparent, traceable, and publicly accessible, achieved through
securely linked blocks using cryptographic techniques (Narayanan et al., 2016). As numerous
prominent blockchain explorers provide tools for
easy access to blockchain transaction data, we retrieve on-chain transaction data from the Dune
Database [6], a crypto analytics platform, and construct comprehensive statistics related to these
transactions to include information on market dynamics. This includes comprehensive metrics such
as daily number of transactions, number of active
wallet, total value transferred, average gas price,
and total gas consumed. These features are crucial for understanding the operational aspects of
blockchains, such as network congestion times and
cost efficiency, which directly impact trading strategies. Our daily collection of these metrics facilitates a nuanced analysis of market dynamics and
liquidity, allowing for real-time adjustments to our
trading algorithms based on current market conditions.

  - **Off-chain Data:** We employ the Gnews API [7] to
systematically gather news articles related to each
cryptocurrency. This tool enables us to access a
wide array of sources through Google News, providing a comprehensive daily snapshot of market sentiment. Moreover, we particularly focus
on filtering news from reputable financial and
cryptocurrency-specific outlets such as Bloomberg,
Yahoo Finance, and crypto.news [8] to ensure the reliability and relevance of the information. For each
day, relevant articles were searched using the name
of each cryptocurrency as a keyword to ensure all
collected news articles were directly related to that
cryptocurrency. This approach helped exclude a
large amount of unrelated news. In this way, on
average, we collected 47.1 news articles related


[5https://coinmarketcap.com](https://coinmarketcap.com)
[6https://dune.com/home](https://dune.com/home)
[7https://pypi.org/project/gnews/](https://pypi.org/project/gnews/)
[8https://crypto.news/](https://crypto.news/)



to Bitcoin, 42.6 news articles related to Ethereum,
and 15.7 news articles related to Solana. Then,
we filtered the news by their source to further enhance relevance and reliability. Finally we will
use no more than 5 news every day for each cryptocurrency. The integration of analysis from these
articles allows us to capture the market’s sentiment
and response to developments, which is often a
precursor to significant market movements.


By merging both on-chain data and off-chain news
insights, our methodology offers a holistic view of the
cryptocurrency market. This integration not only enhances our analytical capabilities but also significantly
improves the precision of our trading decisions.


**2.2** **Market and News Analyst Agents**

Upon collecting extensive on-chain and off-chain data,
we analyze it through two key components of our CryptoTrade agent: (1) market analyst agent, (2) news analyst. By leveraging the capabilities of GPT models,
these analysts provide deep insights into the crypto market, enabling informed and strategic trading decisions.
**Market Analyst Agent.** The market analyst agent
plays a crucial role in deciphering market dynamics through the statistical analysis of key trading signals from on-chain data, such as MA (Gencay, 1996),
MACD (Wang and Kim, 2018), and Bollinger Bands
(Day et al., 2023). Details of these trading signals are
provided in Appendix E. Armed with this information,
the market analyst agent compiles reports on the market’s direction and momentum. An example is shown in
Figure 4.
**News** **Analyst** **Agent.** The news analyst agent is
tasked with extracting and analyzing critical information from the latest news to assess the potential market
impact of off-chain social hype. By sourcing news summaries from various trusted sources, the news analyst
agent pinpoints relevant recent events and assesses the
significance and implications of key topics, thus adding
an extra dimension of insight. An example is provided
in Figure 5.


**2.3** **Trading Agent**

Each day, the trading agent offers an investment suggestion based on reports from the market and news analyst
agents. After analyzing the reports, the trading agent
provides a concise rationale for its decisions. It also recommends allocating a certain portion of remaining cash
to purchase cryptocurrency (with a range from (0 to 1]),
selling a certain portion of owned cryptocurrency (with
a range from [ _−_ 1 to 0)), or holding (neither buying nor
selling). When a trading decision is made, a transaction
fee is charged in proportion to the traded value. Figure 6
illustrates an example of our trading agent’s operations.


**2.4** **Reflection Agent**

The reflection agent reviews the trading agent’s recent
activities to enhance future strategies. By analyzing the



1096


previous week’s prompts, decisions, and returns, the
reflection agent identifies the most impactful information and the reasons behind its significance, providing
feedback to the trading agent for future decisions. Consequently, CryptoTrade learns to focus on the most influential information for upcoming decisions. An example
is illustrated in Figure 7.


**3** **Experiments**


In this section, we detail the experiments designed to
evaluate the efficacy of our proprietary CryptoTrade
agent in comparison to established baseline strategies
within the trading domain.


**3.1** **Experimental Setup**

**Experiment** **Environments.** We conduct all experiments using PyTorch on an NVIDIA GeForce RTX
3090 GPU. More details are in Appendix C.

**Datasets.** To ensure our experiments are robust across
different cryptocurrencies and market conditions, we
base our study on a dataset covering several months,
detailed in Table 1. This dataset reflects the recent market performance of BTC, ETH, and SOL, presenting
challenges in capturing market trends and volatility. We
divide the dataset into validation and test sets, using the
former to select model hyperparameters and the latter
to evaluate model performance. We carefully select the
test period after September 2021, the GPT-3.5’s knowledge cutoff date, to prevent data leakage. The dataset
encompasses three market conditions: bull, sideways,
and bear, allowing us to test the effectiveness of both
the baselines and our model (Baroiu et al., 2023; Cagan,
2024), ensuring reliable and robust experimental results.

**Evaluation** **Scheme.** We initialize the trading agent
with 1 million US dollars, split equally between cash
and BTC/ETH/SOL, to enable potential profits from
both buying and selling cryptocurrencies. At the end
of the trading session, we use the following widelyaccepted metrics: **Return**, **Sharpe Ratio**, **Daily Return**
**Mean**, and **Daily Return Std** . This evaluation scheme
ensures a rigorous and unbiased assessment of both
baseline strategies and our CryptoTrade agent.
**(1)** **Return** measures the overall performance of
the trading strategy, calculated using the formula
_w_ _[end]_ _w_ _[start]_

_−_ _[start]_, where _w_ _[start]_ and _w_ _[end]_ represent the start


_w_ _[end]_ _w_ _[start]_

_w−_ _[start]_, where _w_ _[start]_ and _w_ _[end]_ represent the starting and ending net worth, respectively.
**(2) Sharpe Ratio** assesses the risk-adjusted return,
using the formula _[r]_ [¯] _[−][r][f]_, where _r_ ¯ is the mean of daily



using the formula _[r]_ [¯] _[−]_ _σ_ _[r][f]_, where _r_ ¯ is the mean of daily

returns, _σ_ is the standard deviation of daily returns, and
_rf_ is the risk-free return, set to 0 following SocioDojo
(Cheng and Chin, 2024).
**(3) Daily Return Mean** is the average of the daily
returns over the trading period, providing insight into
the typical daily performance of the trading strategy.
**(4) Daily Return Std** is the standard deviation of the
daily returns, indicating the volatility and risk associated
with the daily performance of the trading strategy.



**Baseline Strategies.** To benchmark the performance of
our CryptoTrade agent, we compare it against widely
recognized baseline strategies in the trading domain.
We present these baselines and hyperparameters in Appendix E.


**3.2** **Experimental Results**

The performance comparison presented in Table 2, Table 3, Table 4 between various trading strategies and our
proposed CryptoTrade agent reveals significant insights
into the efficacy of incorporating advanced data analysis
techniques for cryptocurrency trading. The table highlights the returns and Sharpe Ratios for each method,
where our CryptoTrade agent performs with outstanding percentage return and Sharpe Ratio compared with
time-series baselines but not superior than traditional
trading signals: Buy and Hold and SLMA. We outline
the superiority of CryptoTrade in the following two key
aspects:

**Superior Performance under Different Market Con-**
**ditions.** Remarkably, even without fine-tuning, CryptoTrade outperforms Transformer-based time-series baselines in most bases, demonstrating the robust capabilities of LLMs. Additionally, its performance is comparable to traditional trading signals like Buy and Hold and
MACD, further validating the potential of LLM-based
approaches. For instance, CryptoTrade (GPT-4o) excels
in all metrics under ETH’s bull market by 3% in total
return and sharpe ratio. While CryptoTrade (GPT-4o)
may not always be the top performer in every scenario,
it consistently surpasses more than half of the trading
signals across different market conditions, even without
fine-tuning. This highlights the effectiveness and versatility of CryptoTrade in leveraging LLMs to navigate
the complexities of the cryptocurrency market.
**Successful** **Trend** **Predictions.** We draw Figure 2 to
demonstrate the correlation between Ethereum’s opening prices and the positions held by the CryptoTrade
agent, with the yellow and blue lines representing daily
opening prices and Ethereum positions, respectively.
The observed fluctuations highlight the market’s volatility, while the alignment between position adjustments
and price movements showcases the agent’s proficiency
in anticipating market trends. Unlike the static Buy and
Hold strategy, CryptoTrade adopts a dynamic approach,
optimizing trades based on market analysis—purchasing
at lower prices and selling at peaks. This strategic
adaptability, especially evident during shaded periods
of preemptive position changes in anticipation of price
shifts, underscores the agent’s capacity for risk management and its adeptness at leveraging market volatility
for profit, marking a significant advancement over traditional trading strategies.


**3.3** **Ablation Study**

The ablation study presented in Table 5 critically examines the individual components of the prompt used
by the CryptoTrade (GPT-4o) agent. By systematically



1097


Type Split Start End Open Close Trend



BTC


ETH


SOL



Validation 2023-01-19 2023-03-13 20977.48 20628.03 -1.67%
Test Bearish 2023-04-12 2023-06-16 30462.48 25575.28 -15.61%
Test Sideways 2023-06-17 2023-08-25 26328.68 26163.68 -0.83%
Test Bullish 2023-10-01 2023-12-01 26967.40 37718.01 39.66%

Validation 2023-01-13 2023-03-12 1417.13 1429.60 0.88%
Test Bearish 2023-04-12 2023-06-16 1892.94 1664.98 -12.24%
Test Sideways 2023-06-20 2023-08-31 1734.79 1705.11 -1.91%
Test Bullish 2023-10-01 2023-12-01 1671.00 2051.76 22.59%

Validation 2023-01-14 2023-03-12 18.29 18.24 -0.27%
Test Bearish 2023-04-12 2023-06-16 23.02 14.76 -36.08%
Test Sideways 2023-07-08 2023-08-31 21.49 20.83 -3.23%
Test Bullish 2023-10-01 2023-12-01 21.39 59.25 176.72%



Table 1: Dataset splits. Prices are in US dollars. In each split, the transaction days include the start date and exclude
the end date. We evaluate the total profit on the end date.


**Strategy** **Total Return** **Daily Return** **Sharpe Ratio**

**Bull** **Sideways** **Bear** **Bull** **Sideways** **Bear** **Bull** **Sideways** **Bear**

Buy and hold **39.66** -0.83 -15.61 **0.56** _±_ 2.23 0.00 _±_ 1.74 -0.24 _±_ 2.07 **0.25** 0.00 -0.11
SMA 22.58 **3.65** -21.74 0.35 _±_ 1.89 **0.06** _±_ 1.21 -0.36 _±_ 1.25 0.18 **0.05** -0.29
SLMA 38.53 -3.14 -7.68 0.55 _±_ 2.21 -0.04 _±_ 0.83 -0.11 _±_ 1.23 **0.25** -0.05 -0.09
MACD 13.57 -6.71 -9.51 0.22 1.45 -0.09 1.01 -0.14 1.56 0.15 -0.09 -0.09
_±_ _±_ _±_
Bollinger Bands 2.97 -3.19 **-1.17** 0.05 _±_ 0.32 -0.04 _±_ 0.87 **-0.02** _±_ 0.51 0.15 -0.05 **-0.03**

LSTM 31.67 -4.13 -17.20 0.47 2.11 -0.05 1.62 -0.28 1.27 0.22 -0.03 -0.22
_±_ _±_ _±_
Informer 0.34 -2.33 -13.38 0.01 0.82 -0.03 0.54 -0.21 1.02 0.01 -0.06 -0.21
_±_ _±_ _±_
AutoFormer 14.73 -4.90 -12.72 0.24 1.65 -0.07 1.15 -0.20 1.13 0.14 -0.06 -0.18
_±_ _±_ _±_
TimesNet 2.84 -5.12 -13.64 0.05 1.06 -0.07 1.10 -0.22 1.04 0.05 -0.06 -0.21
_±_ _±_ _±_
PatchTST 1.79 -5.02 -21.94 0.03 0.71 -0.07 0.57 -0.37 1.05 0.04 -0.13 -0.35
_±_ _±_ _±_

Ours(GPT-3.5-turbo) 18.84 0.33 -9.12 0.30 1.69 0.01 1.19 -0.14 1.52 0.18 0.01 -0.09
_±_ _±_ _±_
Ours(GPT-4) 26.35 -4.07 -11.72 0.40 1.76 -0.05 1.43 -0.18 1.67 0.23 -0.04 -0.11
_±_ _±_ _±_
Ours(GPT-4o) 28.47 -5.08 -13.71 0.43 1.89 -0.07 1.14 -0.21 1.71 0.23 -0.06 -0.12
_±_ _±_ _±_


Table 2: Performance of each strategy on BTC under Bull, Sideways, and Bear market conditions. For each market
condition and each metric, the best result is highlighted in bold text and the runner-up result is underlined.


**Strategy** **Total Return (%)** **Daily Return (%)** **Sharpe Ratio**

**Bull** **Sideways** **Bear** **Bull** **Sideways** **Bear** **Bull** **Sideways** **Bear**

Buy and Hold 22.59 -1.91 -12.24 0.36 2.62 -0.01 1.94 -0.17 2.39 0.14 -0.00 -0.07
_±_ _±_ _±_
SMA 10.17 -5.45 -10.12 0.18 2.29 -0.15 1.64 -0.15 1.64 0.08 -0.07 -0.09
_±_ _±_ _±_
SLMA 5.20 -2.62 -15.90 0.11 2.37 -0.03 1.08 -0.24 1.86 0.05 -0.03 -0.13
_±_ _±_ _±_
MACD 7.72 0.77 -12.15 0.13 1.22 0.02 1.43 -0.18 1.56 0.10 0.01 -0.12
_±_ _±_ _±_
Bollinger Bands 2.59 **4.47** **-0.41** 0.04 _±_ 0.40 **0.07** _±_ 1.02 **0.00** _±_ 0.58 0.11 0.06 **-0.01**

LSTM 22.12 1.27 -13.22 0.36 _±_ 2.59 0.02 _±_ 1.11 -0.19 _±_ 2.36 0.14 **0.15** -0.08
Informer 14.55 -4.74 -11.49 0.23 1.54 -0.06 1.45 -0.17 1.65 0.15 -0.04 -0.10
_±_ _±_ _±_
AutoFormer 7.77 -10.06 -19.44 0.13 1.81 -0.14 1.33 -0.31 1.61 0.08 -0.10 -0.20
_±_ _±_ _±_
TimesNet 13.31 -8.08 -10.64 0.21 1.50 -0.11 1.08 -0.16 1.04 0.14 -0.10 -0.16
_±_ _±_ _±_
PatchTST 8.95 -9.64 -13.76 0.15 1.37 -0.13 1.66 -0.21 1.39 0.11 -0.11 -0.15
_±_ _±_ _±_

Ours(GPT-3.5-turbo) 18.91 -5.02 -14.40 0.30 2.01 -0.06 1.56 -0.22 2.08 0.15 -0.04 -0.10
_±_ _±_ _±_
Ours(GPT-4) **25.72** 0.72 -13.72 **0.41** _±_ 2.45 0.03 _±_ 1.67 -0.21 _±_ 2.02 0.17 0.02 -0.10
Ours(GPT-4o) 25.47 -6.59 -15.35 0.40 _±_ 2.25 -0.07 _±_ 1.81 -0.23 _±_ 2.16 **0.18** -0.04 -0.11


Table 3: Performance of each strategy on ETH under Bull, Sideways, and Bear market conditions.



removing key elements from the full prompt and observing the impact on percentage return and Sharpe ratio
during a bull market for ETH, we can identify the contribution of each component to the overall performance



of the trading strategy.We highlight the following two
insights from the results:

**Superiority of the Full Prompt.** The full prompt significantly outshines all other configurations with reduced



1098


**Strategy** **Total Return (%)** **Daily Return (%)** **Sharpe Ratio**

**Bull** **Sideways** **Bear** **Bull** **Sideways** **Bear** **Bull** **Sideways** **Bear**

Buy and Hold **176.72** -3.23 -36.08 **1.83** _±_ 6.00 0.01 _±_ 3.92 -0.61 _±_ 3.45 0.30 0.00 -0.18
SMA 119.37 -0.62 **1.04** 1.43 _±_ 5.67 0.03 _±_ 3.06 **0.02** _±_ 0.10 0.25 0.01 **0.16**
SLMA 169.98 **6.22** -8.11 1.78 _±_ 5.93 **0.16** _±_ 3.23 -0.11 _±_ 1.88 0.30 **0.05** -0.06
MACD 23.25 -9.78 -21.07 0.35 1.76 -0.16 2.38 -0.33 2.44 0.20 -0.07 -0.13
_±_ _±_ _±_
Bollinger Bands 2.92 -0.46 -21.69 0.05 0.35 0.00 1.23 -0.35 1.75 0.13 -0.00 -0.20
_±_ _±_ _±_

LSTM 144.69 -3.56 -36.75 1.61 5.69 0.01 3.90 -0.63 3.43 0.28 0.00 -0.18
_±_ _±_ _±_
Informer 41.85 -6.55 -26.13 0.58 _±_ 1.90 -0.10 _±_ 2.00 -0.43 _±_ 2.36 **0.31** -0.05 -0.18
AutoFormer 35.86 -6.17 -23.56 0.51 1.97 -0.10 1.90 -0.38 2.35 0.26 -0.05 -0.16
_±_ _±_ _±_
TimesNet 45.28 -10.63 -21.60 0.64 2.66 -0.18 2.01 -0.35 1.75 0.24 -0.09 -0.20
_±_ _±_ _±_
PatchTST 18.45 -7.10 -27.86 0.29 1.57 -0.11 1.98 -0.46 2.49 0.18 -0.06 -0.19
_±_ _±_ _±_

Ours(GPT-3.5-turbo) 102.45 -13.05 -24.08 1.26 4.54 -0.23 2.42 -0.39 2.60 0.28 -0.15 -0.10
_±_ _±_ _±_
Ours(GPT-4) 99.84 -2.16 -19.55 1.24 4.53 0.01 3.33 -0.31 2.35 0.27 0.00 -0.13
_±_ _±_ _±_
Ours(GPT-4o) 115.18 3.09 -16.32 1.38 4.98 0.11 3.31 -0.25 2.35 0.28 0.03 -0.10
_±_ _±_ _±_


Table 4: Performance of each strategy on SOL under Bull, Sideways, and Bear market conditions.










|Col1|Col2|Col3|Col4|Col5|Col6|Col7|
|---|---|---|---|---|---|---|
||||||||
|1600<br>1800<br>2000<br>2200<br>2400<br>2600<br>Price|Pric|||||0<br>100<br>200<br>300<br>400<br>500<br><br>Position|
|1600<br>1800<br>2000<br>2200<br>2400<br>2600<br>Price|||||||
|1600<br>1800<br>2000<br>2200<br>2400<br>2600<br>Price|||||Position|Position|
|1600<br>1800<br>2000<br>2200<br>2400<br>2600<br>Price|||||||



2023-08-31 2023-09-30 2023-10-30 2023-11-29 2023-12-29


Figure 2: Significant profitable periods exploited by the CryptoTrade agent. The yellow line shows the daily opening
prices of Ethereum in US dollars. The blue line tracks the daily positions, indicating the amount of Ethereum
possessed on each day. The blue dots denote trading decisions when the agent largely alters its position by trading
Ethereum. The red dots represent the corresponding trading prices. The agent successfully forecasts price changes,
securing substantial profits through low-price purchases and high-price sales.



Prompt Components Return (%) Sharpe Ratio
Full **28.47** **0.23**
w/o Reflection 17.14 0.06
w/o News 19.69 0.06
w/o TxnStats 12.70 0.05
w/o Technical 17.27 0.05
Base 8.40 0.03


Table 5: Ablation study on prompt components of the
CryptoTrade agent. Base prompt encompasses necessary context including trading rules, valid action space,
current cash and ETH holdings, and recent ETH prices.


components. The advantage of employing a full prompt
over all deducted variants is rooted in the integration
of diverse data sources. The full prompt encompasses
the comprehensive price data, news analysis, technical



indicators, on-chain transaction statistics, and reflective
analysis to offer a holistic view of the market. This
comprehensive approach allows the CryptoTrade agent
to leverage a wide array of information, enabling it to
navigate the complexities of the cryptocurrency market
with more nuanced and informed trading decisions.


**Advantage** **of** **Crypto** **Transaction** **Statistics.** The
omission of Ethereum transaction statistics results in
a significant decrease of the outcome by around 16%,
underscoring the indispensable role of on-chain statistics in enhancing trading strategies. This observation
highlights the necessity of integrating on-chain transaction data, revealing its unique value in enriching the
decision-making process in the cryptocurrency trading
tasks.



1099


2600

2500

2400

2300

2200

2100

2000

1900

1800





500


400


300


200



100



Figure 3: Case study of CryptoTrade’s actions in response to news reports on early rumor and the actual event of
Bitcoin ETF approval, which takes place on Jan 11, 2024. The red circles denote the trading prices. The agent
successfully benefits from a "buy the rumor, sell the news" strategy.



**3.4** **Case Study**

To assess the adaptability and responsiveness of the
CryptoTrade agent, we conduct a case study focusing
on its responsive actions in the context of the cryptocurrency market’s major events, illustrated in Figure 3. It
reveals that CryptoTrade’s strategy aligns with the "buy
the rumor, sell the news" principle, effectively capitalizing on early signs of the Bitcoin ETF approval event, a
scenario known to trigger market rallies due to speculative trading. By entering the market early, CryptoTrade
secures positions at lower costs ahead of the rally.
As the approval of the Bitcoin ETF becomes a reality,
the sentiment reaches a crescendo, resulting in inflated
asset prices due to heightened demand. CryptoTrade,
adhering to its strategic motivation, takes this peak as
an optimal point to sell, which is validated in the subsequent decline in the Ethereum price. This strategic exit
allows CryptoTrade to realize gains before the market
adjusted to the new equilibrium, which results in a price
pullback as early speculators take profits and the market
sentiment normalizes.
To sum up, CryptoTrade’s provident actions underscore the delicate balance between foresight and timing
in trading strategies. This case study demonstrates that
an informed and timely response to market signals —
both rumors and confirmed news - can yield advantageous outcomes. It also highlights the CryptoTrade
agent’s understanding of market psychology and its ability to translate this into profitable trading decisions.
**4** **Related Work**


**LLMs for Economics and Financial Decisions** Recent
advancements in LLMs have significantly influenced
economics and financial decision-making. Specialized



LLMs like FinGPT, BloombergGPT, FinMA (Liu et al.,
2023; Wu et al., 2023; Xie et al., 2023) are tailored
for finance, handling tasks such as sentiment analysis,
entity recognition, and question-answering. Another
research direction uses LLMs for financial time-series
forecasting. A notable contribution by (Yu et al., 2023)
employed zero-shot or few-shot inference with GPT-4
and instruction-based fine-tuning with LlaMA to enhance cross-sequence reasoning and multi-modal signal
integration. Additionally, the development of LLMbased agents for financial trading has gained attention.
Sociodojo (Cheng and Chin, 2024) created analytical
agents for stock portfolio management, showing the potential for generating "hyperportfolios." Despite these
advancements, the focus has largely been on the stock
market (Koa et al., 2024; Chen et al., 2023), leaving
a gap in the exploration of the cryptocurrency market
where the on-chain data is approachable and with much
information. Our work aims to address this gap by leveraging both on-chain and off-chain data to navigate the
dynamic cryptocurrency market.


**Time-Series** **Forecasting** **for** **Financial** **Markets**
Time-series forecasting has long been a cornerstone
of research in economics and financial markets. Early
studies focused on predicting stock market prices using methodologies such as machine learning (Leung
et al., 2021; Patel and Yalamalle, 2014), reinforcement
learning (Lee, 2001), and traditional time-series models (Herwartz, 2017). The Long Short-Term Memory
(LSTM) model has emerged as particularly influential
(Sunny et al., 2020) for its capability to process and
analyze time-series data. With the rise of blockchain
technology and cryptocurrencies, these techniques have
been extended to crypto assets (Khedr et al., 2021). Re


1100


cent research has evaluated the impact of various predictors on cryptocurrency pricing and returns, using both
on-chain data—such as historical transactions and market volume (Ferdiansyah et al., 2019)—and off-chain
factors like social media trends and news sentiment
(Abraham et al., 2018; Pang et al., 2019). These studies underscore the effectiveness of integrating diverse
data sources for forecasting the volatile dynamics of the
cryptocurrency market. Apart from above, Transformerbased models have shown particular promise in this area,
with state-of-the-art models like Informer (Zhou et al.,
2021), AutoFormer (Wu et al., 2021), PatchTST (Nie
et al., 2022), and TimesNet (Wu et al., 2022) further
advancing time-series forecasting.

**Self-Reflective Language Agents** The Self-Refine
framework introduces an advanced approach for autonomous advancement through self-evaluation and iterative self-improvement (Madaan et al., 2024). This
approach, along with efforts to automatically refine
prompts (Pryzant et al., 2023; Ye et al., 2024) and provide automated feedback to enhance reasoning capabilities (Paul et al., 2023), marks significant progress
in the field. Notably, the "Reflexion" framework by
(Shinn et al., 2024) revolutionizes the reinforcement of
language agents by utilizing linguistic feedback and reflective text within an episodic memory buffer, diverging
from traditional weight update methods. These advancements highlight the potential for LLMs to learn from
their errors and evolve through self-reflection. Despite
these developments, there is still untapped potential
in applying self-reflective language agents to financial
decision-making, particularly in cryptocurrency markets. This work aims to bridge that gap by investigating
the application of self-reflective mechanisms to enhance
financial decision-making processes in cryptocurrency
trading.


**5** **Conclusion**


We propose the CryptoTrade agent, an innovative approach to cryptocurrency trading that leverages advanced data analysis and LLMs. By integrating both
on-chain and off-chain data, along with a self-reflective
component, the CryptoTrade agent demonstrates a
sophisticated understanding of market dynamics and
achieves relatively high returns in cryptocurrency trading. Our comprehensive experiments comparing the
CryptoTrade agent to traditional trading strategies and
time-series models reveal its superior ability to navigate
the volatile cryptocurrency market, consistently achieving relatively high returns on investment under different
market conditions over time-series models while not
superior than traditional trading signals: Buy and Hold
and SLMA. This research underscores the significant
potential of LLM-driven strategies in enhancing trading
performance and sets a new benchmark for cryptocurrency trading with LLMs.



**Limitations**


One limitation of the current CryptoTrade framework
is the reliance on a relatively limited dataset. To address this, we plan to enrich the dataset with additional
off-chain data. Another limitation is the frequency of
trading actions, which is currently set to day-to-day. We
aim to refine this to hour-to-hour or minute-to-minute
intervals to further optimize returns in the cryptocurrency market. Additionally, we have identified that the
lack of fine-tuning for the LLMs using the validation
set may be a significant factor behind the LLM-based
agents’ underperformance compared to traditional trading signals. To improve the reliability of our forecasts,
we intend to fine-tune the LLMs with the validation set.


**Broader Impact**


One potential broader impact of our research is the risk
that individuals may follow the trading strategies we
provide and subsequently incur financial losses. It is
important to emphasize that these strategies are intended
for academic research only. CryptoTrade is not for
investment recommendations.


**Acknowledgements**


This research is supported by the National Research
Foundation, Singapore under its Industry Alignment
Fund–Pre-positioning (IAF-PP) Funding Initiative. Any
opinions, findings and conclusions or recommendations
expressed in this material are those of the author(s) and
do not reflect the views of National Research Foundation, Singapore.


**References**

Jethin Abraham, Daniel Higdon, John Nelson, and Juan
Ibarra. 2018. Cryptocurrency price prediction using
tweet volumes and sentiment analysis. _SMU_ _Data_
_Science Review_, 1(3):1.


Alexandru Costin Baroiu, Vlad Diaconita, and Simona Vasilica Oprea. 2023. Bitcoin volatility in
bull vs. bear market-insights from analyzing on-chain
metrics and twitter posts. _PeerJ Computer Science_,
9:e1750.


Michele Cagan. 2024. _Stock_ _Market_ _101:_ _From_ _Bull_
_and_ _Bear_ _Markets_ _to_ _Dividends,_ _Shares,_ _and_ _Mar-_
_gins—Your Essential Guide to the Stock Market_ . Simon and Schuster.


Zihan Chen, Lei Nico Zheng, Cheng Lu, Jialu Yuan, and
Di Zhu. 2023. Chatgpt informed graph neural network for stock movement prediction. _arXiv preprint_
_arXiv:2306.03763_ .


Junyan Cheng and Peter Chin. 2024. [Sociodojo:](https://openreview.net/forum?id=s9z0HzWJJp) Build[ing lifelong analytical agents with real-world text and](https://openreview.net/forum?id=s9z0HzWJJp)
[time series.](https://openreview.net/forum?id=s9z0HzWJJp) In _The Twelfth International Conference_
_on Learning Representations_ .



1101


Min-Yuh Day, Yirung Cheng, Paoyu Huang, and Yensen
Ni. 2023. The profitability of bollinger bands trading bitcoin futures. _Applied_ _Economics_ _Letters_,
30(11):1437–1443.


Stanisław Dro˙zd˙z, Jarosław Kwapie´n, and Marcin W˛atorek. 2023. What is mature and what is still
emerging in the cryptocurrency market? _Entropy_,
25(5):772.


Rainer Feichtinger, Robin Fritsch, Yann Vonlanthen,
and Roger Wattenhofer. 2023. The hidden shortcomings of (d) aos–an empirical study of on-chain
governance. In _International Conference on Finan-_
_cial Cryptography and Data Security_, pages 165–185.
Springer.


Ferdiansyah Ferdiansyah, Siti Hajar Othman, Raja Zahilah Raja Md Radzi, Deris Stiawan, Yoppy Sazaki,
and Usman Ependi. 2019. A lstm-method for bitcoin
price prediction: A case study yahoo finance stock
market. In _2019_ _international_ _conference_ _on_ _elec-_
_trical engineering and computer science (ICECOS)_,
pages 206–210. IEEE.


Ramazan Gencay. 1996. Non-linear prediction of security returns with moving average rules. _Journal of_
_Forecasting_, 15(3):165–174.


Helmut Herwartz. 2017. Stock return prediction under garch—an empirical assessment. _International_
_Journal of Forecasting_, 33(3):569–580.


Ahmed M Khedr, Ifra Arif, Magdi El-Bannany, Saadat M Alhashmi, and Meenu Sreedharan. 2021. Cryptocurrency price prediction using traditional statistical and machine-learning techniques: A survey. _Intel-_
_ligent Systems in Accounting, Finance and Manage-_
_ment_, 28(1):3–34.


Kelvin JL Koa, Yunshan Ma, Ritchie Ng, and Tat-Seng
Chua. 2024. Learning to generate explainable stock
predictions using self-reflective large language models. In _Proceedings of the ACM on Web Conference_
_2024_, pages 4304–4315.


Jae Won Lee. 2001. Stock price prediction using reinforcement learning. In _ISIE 2001. 2001 IEEE Interna-_
_tional Symposium on Industrial Electronics Proceed-_
_ings (Cat. No. 01TH8570)_, volume 1, pages 690–695.
IEEE.


Edward Leung, Harald Lohre, David Mischlich, Yifei
Shea, and Maximilian Stroh. 2021. The promises
and pitfalls of machine learning for predicting stock
returns. _The Journal of Financial Data Science_ .


Percy Liang, Rishi Bommasani, Tony Lee, Dimitris
Tsipras, Dilara Soylu, Michihiro Yasunaga, Yian
Zhang, Deepak Narayanan, Yuhuai Wu, Ananya Kumar, et al. 2022. Holistic evaluation of language
models. _arXiv preprint arXiv:2211.09110_ .


Xiao-Yang Liu, Guoxuan Wang, and Daochen Zha.
2023. Fingpt: Democratizing internet-scale data
for financial large language models. _arXiv preprint_
_arXiv:2307.10485_ .



Bingqiao Luo, Zhen Zhang, Qian Wang, Anli Ke,
Shengliang Lu, and Bingsheng He. 2023. Aipowered fraud detection in decentralized finance:
A project life cycle perspective. _arXiv_ _preprint_
_arXiv:2308.15992_ .


Aman Madaan, Niket Tandon, Prakhar Gupta, Skyler
Hallinan, Luyu Gao, Sarah Wiegreffe, Uri Alon,
Nouha Dziri, Shrimai Prabhumoye, Yiming Yang,
et al. 2024. Self-refine: Iterative refinement with
self-feedback. _Advances in Neural Information Pro-_
_cessing Systems_, 36.


Arvind Narayanan, Joseph Bonneau, Edward Felten,
Andrew Miller, and Steven Goldfeder. 2016. _Bitcoin_
_and cryptocurrency technologies:_ _a comprehensive_
_introduction_ . Princeton University Press.


Yuqi Nie, Nam H Nguyen, Phanwadee Sinthong, and
Jayant Kalagnanam. 2022. A time series is worth
64 words: Long-term forecasting with transformers.
_arXiv preprint arXiv:2211.14730_ .


Yan Pang, Ganeshkumar Sundararaj, and Jiewen Ren.
2019. Cryptocurrency price prediction using time series and social sentiment data. In _Proceedings of the_
_6th IEEE/ACM International Conference on Big Data_
_Computing, Applications and Technologies_, pages 35–
41.


Mayankkumar B Patel and Sunil R Yalamalle. 2014.
Stock price prediction using artificial neural network. _International Journal of Innovative Research_
_in Science, Engineering and Technology_, 3(6):13755–
13762.


Debjit Paul, Mete Ismayilzada, Maxime Peyrard, Beatriz Borges, Antoine Bosselut, Robert West, and
Boi Faltings. 2023. Refiner: Reasoning feedback
on intermediate representations. _arXiv_ _preprint_
_arXiv:2304.01904_ .


Reid Pryzant, Dan Iter, Jerry Li, Yin Tat Lee, Chenguang Zhu, and Michael Zeng. 2023. Automatic
prompt optimization with" gradient descent" and
beam search. _arXiv preprint arXiv:2305.03495_ .


Xiao Pu, Mingqi Gao, and Xiaojun Wan. 2023.
Summarization is (almost) dead. _arXiv_ _preprint_
_arXiv:2309.09558_ .


Kunpeng Ren, Nhut-Minh Ho, Dumitrel Loghin, ThanhToan Nguyen, Beng Chin Ooi, Quang-Trung Ta, and
Feida Zhu. 2023. Interoperability in blockchain: A
survey. _IEEE Transactions on Knowledge and Data_
_Engineering_ .


Noah Shinn, Federico Cassano, Ashwin Gopinath,
Karthik Narasimhan, and Shunyu Yao. 2024. Reflexion: Language agents with verbal reinforcement
learning. _Advances in Neural Information Processing_
_Systems_, 36.


Md Arif Istiake Sunny, Mirza Mohd Shahriar Maswood,
and Abdullah G Alharbi. 2020. Deep learning-based
stock price prediction using lstm and bi-directional
lstm model. In _2020 2nd novel intelligent and leading_



1102


_emerging sciences conference (NILES)_, pages 87–92.
IEEE.


Jian Wang and Junseok Kim. 2018. Predicting stock
price trend using macd optimized by historical volatility. _Mathematical Problems in Engineering_, 2018:1–
12.


Qian Wang, Zhen Zhang, Zemin Liu, Shengliang Lu,
Bingqiao Luo, and Bingsheng He. 2024. Ex-graph:
A pioneering dataset bridging ethereum and x. In
_The_ _Twelfth_ _International_ _Conference_ _on_ _Learning_
_Representations_ .


Jason Wei, Xuezhi Wang, Dale Schuurmans, Maarten
Bosma, Fei Xia, Ed Chi, Quoc V Le, Denny Zhou,
et al. 2022. Chain-of-thought prompting elicits reasoning in large language models. _Advances in neural_
_information processing systems_, 35:24824–24837.


Yu Wei, Yizhi Wang, Brian M Lucey, and Samuel A Vigne. 2023. Cryptocurrency uncertainty and volatility
forecasting of precious metal futures markets. _Jour-_
_nal of Commodity Markets_, 29:100305.


Haixu Wu, Tengge Hu, Yong Liu, Hang Zhou, Jianmin
Wang, and Mingsheng Long. 2022. Timesnet: Temporal 2d-variation modeling for general time series
analysis. In _The eleventh international conference on_
_learning representations_ .


Haixu Wu, Jiehui Xu, Jianmin Wang, and Mingsheng
Long. 2021. Autoformer: Decomposition transformers with auto-correlation for long-term series forecasting. _Advances in neural information processing_
_systems_, 34:22419–22430.


Shijie Wu, Ozan Irsoy, Steven Lu, Vadim Dabravolski,
Mark Dredze, Sebastian Gehrmann, Prabhanjan Kambadur, David Rosenberg, and Gideon Mann. 2023.
Bloomberggpt: A large language model for finance.
_arXiv preprint arXiv:2303.17564_ .


Qianqian Xie, Weiguang Han, Xiao Zhang, Yanzhao
Lai, Min Peng, Alejandro Lopez-Lira, and Jimin
Huang. 2023. Pixiu: A large language model, instruction data and evaluation benchmark for finance.
_arXiv preprint arXiv:2306.05443_ .


Jingfeng Yang, Hongye Jin, Ruixiang Tang, Xiaotian Han, Qizhang Feng, Haoming Jiang, Shaochen
Zhong, Bing Yin, and Xia Hu. 2024. Harnessing the
power of llms in practice: A survey on chatgpt and
beyond. _ACM Transactions on Knowledge Discovery_
_from Data_, 18(6):1–32.


Seonghyeon Ye, Hyeonbin Hwang, Sohee Yang,
Hyeongu Yun, Yireun Kim, and Minjoon Seo. 2024.
Investigating the effectiveness of task-agnostic prefix
prompt for instruction following. In _Proceedings of_
_the AAAI Conference on Artificial Intelligence_, volume 38, pages 19386–19394.


Kun Yi, Qi Zhang, Wei Fan, Shoujin Wang, Pengyang
Wang, Hui He, Ning An, Defu Lian, Longbing Cao,
and Zhendong Niu. 2024. Frequency-domain mlps
are more effective learners in time series forecasting.



_Advances in Neural Information Processing Systems_,
36.


Xinli Yu, Zheng Chen, Yuan Ling, Shujing Dong,
Zongyi Liu, and Yanbin Lu. 2023. Temporal data
meets llm–explainable financial time series forecasting. _arXiv preprint arXiv:2306.11025_ .


Zhuosheng Zhang, Aston Zhang, Mu Li, Hai Zhao,
George Karypis, and Alex Smola. 2023. Multimodal chain-of-thought reasoning in language models. _arXiv preprint arXiv:2302.00923_ .


Wayne Xin Zhao, Kun Zhou, Junyi Li, Tianyi Tang,
Xiaolei Wang, Yupeng Hou, Yingqian Min, Beichen
Zhang, Junjie Zhang, Zican Dong, et al. 2023. A
survey of large language models. _arXiv_ _preprint_
_arXiv:2303.18223_ .


Haoyi Zhou, Shanghang Zhang, Jieqi Peng, Shuai
Zhang, Jianxin Li, Hui Xiong, and Wancai Zhang.
2021. Informer: Beyond efficient transformer for
long sequence time-series forecasting. In _Proceed-_
_ings of the AAAI conference on artificial intelligence_,
volume 35, pages 11106–11115.



1103


**Appendix**

**A** **License**


The CryptoTrade’s dataset is released under the Creative
Commons Attribution-NonCommercial-ShareAlike
(CC BY-NC-SA) license. This means that anyone can
use, distribute, and modify the data for non-commercial
purposes as long as they give proper attribution and
share the derivative works under the same license terms.


**B** **Data Ethics**


**B.1** **On-chain Data**

We collect on-chain data from CoinMarketCap [9] and
Dune [10] . According to CoinMarketCap’s Terms of Service [11], we are granted a limited, personal, non-exclusive,
non-sub-licensable, and non-transferable license to use
the content and service solely for personal use. We agree
not to use the service or any of the content for any commercial purpose, and we adhere to these requirements.
Regarding Dune’s Terms of Service [12], we are permitted
to access Dune’s application programming interfaces
(the “API”) to perform SQL queries on blockchain data.


**B.2** **Off-chain News**

We employ the Gnews [13] to systematically gather news
articles related to each cryptocurrency. According to
Gnews’ Terms of Service [14], we can download the news
for non-commercial transitory viewing only, and we
cannot modify or copy the materials, use the materials for any commercial purpose or any public display,
attempt to reverse engineer any software contained on
Gnews API’s website, remove any copyright or other
proprietary notations from the materials, or transfer the
materials to another person or "mirror" the materials on
any other server. We adhere to these conditions in our
CryptoTrade dataset.


**C** **Experimental Environment**


All models in our experiments were implemented using
Pytorch 2.0.0 in Python 3.9.16, and run on a robust
Linux workstation. This system is equipped with two
Intel(R) Xeon(R) Gold 6226R CPUs, each operating at a
base frequency of 2.90 GHz and a max turbo frequency
of 3.90 GHz. With 16 cores each, capable of supporting
32 threads, these CPUs offer a total of 64 logical CPUs
for efficient multitasking and parallel computing. The
workstation is further complemented by a potent GPU
setup, comprising eight NVIDIA GeForce RTX 3090
GPUs, each providing 24.576 GB of memory. The


[9https://coinmarketcap.com](https://coinmarketcap.com)
[10https://dune.com/home](https://dune.com/home)
[11https://coinmarketcap.com/terms/](https://coinmarketcap.com/terms/)
[12https://dune.com/terms](https://dune.com/terms)
[13https://pypi.org/project/gnews/](https://pypi.org/project/gnews/)
[14https://gnews.io/terms/](https://gnews.io/terms/)



operation of these GPUs is managed by the NVIDIASMI 525.60.13 driver and CUDA 12.0, ensuring optimal
computational performance for our tasks.


**D** **Analysts Examples**


In this section, we provide some examples of News Analyst, Market Analyst, Reflection Analyst, and Trading
Analyst.


You are an ETH cryptocurrency trading analyst. The recent price and auxiliary information is given in chronological
order below:


Open price: 1838.01, num_transactions: 1042149, unique_addresses: 503796, value_transferred: 1.12746e+24,
average_gas_price: 29177145259.0, total_gas_used: 107993000000.0, successful_txns: 1013494,
short_long_ma_signal: buy, macd_signal: buy, bollinger_bands_signal: hold


Open price: 1835.70, num_transactions: 1049539, unique_addresses: 525502, value_transferred: 8.91311e+23,
average_gas_price: 27012243725.0, total_gas_used: 107794000000.0, successful_txns: 1021103,
short_long_ma_signal: buy, macd_signal: buy, bollinger_bands_signal: holdx
…


Analyze the recent information and estimate the market trend accordingly.


Based on the recent information provided, it appears that the Ethereum (ETH) market trend is bullish. The open prices
have been relatively stable and there has been a consistent increase in the number of transactions, unique addresses,
and value transferred. The average gas price has also been fluctuating but overall remains at a relatively high level.


The technical indicators such as the short-long moving average signal, MACD signal, and Bollinger Bands signal all
suggest a "buy" sentiment. Additionally, the majority of transactions have been successful, indicating a strong level of
activity and interest in the market.


Overall, the data points towards a positive market trend for Ethereum, with potential for further growth in the near
future. Investors may consider taking advantage of the current bullish momentum in their trading strategies.


Figure 4: A sample of the Market Analyst.


You are a cryptocurrency trading analyst. You are required to analyze the following news:


Title: Ethereum Is Turning Deflationary, But Price Remains Stagnant - Watcher Guru,
Content: According to Ultra Sound Money data, …,


Title: Lido adoption surges with 10000 new stakers despite Ethereum's price struggles – CryptoSlate,
Content: “More than 10,000 new stakers joined …,
…


Analyze the news and estimate the market trend accordingly.


Based on the news articles provided, here is an analysis of the current market trend for Ethereum (ETH)
cryptocurrency:


1. Ethereum Is Turning Deflationary, But Price Remains Stagnant:

        - The Ethereum network has burned over half the coins issued since the merge, making it somewhat deflationary.

        - Despite the burns, ETH prices have not heavily responded, possibly due to less demand at the moment.

         - The end goal is for the burn rate to outpace the rate of issuing new coins, potentially making ETH truly deflationary.

         - Ethereum was trading at $1,833.89, down by 0.2% in the last 24 hours.


2. Lido adoption surges with 10,000 new stakers despite Ethereum's price struggles:

        - More than 10,000 new stakers joined Lido (LDO) in July, indicating continued adoption and growth.

         - Ethereum's price struggles have impacted Lido's Total Value Locked (TVL), but it remains a leading DeFi protocol.

         - Predictions suggest staked Ethereum via Lido will surpass eight million in August.

         - Despite challenges like the Curve Finance exploit, Lido's adoption and staked ETH continue to grow.


Figure 5: A sample of the News Analyst.


You are an experienced ETH cryptocurrency trader and you are trying to maximize your overall profit by trading ETH.
In each day, you will make an action to buy or sell ETH. You will start with 1 million dollars, half in cash and half in
ETH. You are assisted by a few analysts below and need to decide the final action.


News Analyst Report: {Reports from the News Analyst}
Market Analyst Report: {Reports from the Market Analyst}
Reflection Analyst Report: {Reports from the Reflection Analyst}


Now, start your response with your brief reasoning over the given reports. Wisely select the useful information,
conclude a clear market trend, pay more attention to long-term trends instead of short-term benefits, and avoid either
being too conservative or changing strategies too rapidly. When the trend is upwards, it is profitable to buy ETH, and
vice versa. Finally, suggest a 1-decimal float action in the range of [-1,1]. When the trend is bullish and upwards, you
need to suggest a positive value to buy ETH. When the trend is bearish and downwards, you need to suggest a negative
value to sell ETH. Your action is based on your prediction and confidence on the market trend, where a larger absolute
value indicates a higher confidence on your trend prediction.


Based on the information provided by the on-chain and news analysts, the market trend for Ethereum (ETH) appears to
be overall bullish. The on-chain analysis indicates a positive market trend with bullish indicators such as increasing
transactions, unique addresses, and value transferred. The technical indicators also suggest a "buy" sentiment, further
supporting the bullish trend.


Additionally, the news analysis highlights the deflationary nature of Ethereum due to coin burns, which could
potentially lead to further price appreciation in the future. Despite some price struggles, the adoption of protocols like
Lido continues to grow, indicating a strong interest in Ethereum.


Considering the fluctuating but overall bullish market trend, I would recommend buying ETH with a confidence level
of -0.8. This action aligns with the positive market sentiment and potential for further growth in the near future.


Figure 6: A sample of the Trading Analyst.


**E** **Baselines**


1. **Buy and Hold:** A straightforward strategy where
an asset is purchased at the beginning of the period
and held until its end.



1104


You are an ETH cryptocurrency trading analyst. Your analysis and action history is given in chronological order:


PROMPT: You are an experienced ETH cryptocurrency trader and you are trying to maximize your over…
ON-CHAIN ANALYST REPORT: Based on the recent information provided, it appears that the Ethereum…
NEWS ANALYST REPORT: Based on the news articles provided, there are several key developments…
REFLECTION ANALYST REPORT: Based on the information provided by the on-chain…
Start your response with your reasoning over the given context. Wisely select the information, …
ACTION: -0.8
DAILY RETURN: -0.016945891751062203


PROMPT: You are an experienced …
ON-CHAIN ANALYST REPORT: Based on the recent information provided, …
NEWS ANALYST REPORT: Based on the news provided, …
…


Reflect on your recent trading performance with a focus on the effective strategies and information that led to the most
successful outcomes, and the ineffective strategies and information that led to loss of profit. Identify key trends and
indicators in the current cryptocurrency market that are likely to influence future trades. Also assess whether a more
aggressive or conservative trading approach is warranted.


Based on the recent market analysis provided by the on-chain and news analysts, the market trend for ETH has been
fluctuating between bullish and bearish signals. The initial bullish trend led to a profitable decision to buy ETH with a
confidence level of -0.7, resulting in a positive daily return. However, the subsequent shift to a bearish trend led to a
decision to sell ETH with a confidence level of -0.8, resulting in a negative daily return. The following action of buying
ETH with a confidence level of -0.6 also resulted in a negative daily return.


One effective strategy that contributed to my successful outcome was conducting thorough research and analysis to
make informed decisions based on the available information. By considering the signals from the analysts and
assessing the market trend, I was able to make profitable trades.


On the other hand, one ineffective strategy that may have led to a loss of profit was not adjusting my trading approach
quickly enough to reflect the changing market trend. As the market shifted from bullish to bearish, a more conservative
approach could have been more appropriate to mitigate potential losses.


Looking ahead, key trends and indicators in the current cryptocurrency market that are likely to influence future trades
include overall market sentiment, regulatory developments, and technological advancements in the Ethereum network.


Figure 7: A sample of the Reflection Analyst.


2. **SMA (Gencay, 1996):** SMA triggers buy or sell
decisions based on the asset’s price relative to its
moving average. We finetune the SMA period by
testing different window sizes [5 _,_ 10 _,_ 15 _,_ 20 _,_ 30].
The optimal period is selected based on the best
performance on the validation set.


3. **SLMA (Wang and Kim, 2018):** SLMA involves
two moving averages of different lengths, with
trading signals generated at their crossover points.
We use different combinations of short and long
SMA periods, selecting the optimal ones based on
validation set performance.


4. **MACD (Wang and Kim, 2018):** A strategy that
uses the MACD indicator to identify potential buy
and sell opportunities based on the momentum of
the asset. The MACD is calculated as the difference between the 12-day EMA and the 26-day
EMA, with a 9-day EMA of the MACD line serving as the signal line. EMA stands for Exponential
Moving Average. It is a type of moving average
that places a greater weight and significance on the
most recent data points.


5. **Bollinger** **Bands** **(Day** **et** **al.,** **2023):** This strategy generates trading signals based on price movements relative to the middle, lower, and upper
Bollinger Bands. Bollinger Bands are constructed
using a 20-day SMA and a multiplier (commonly
set to 2) for the standard deviation. We use the
recommended period and multiplier settings for
this strategy.


6. **LSTM** **(Ferdiansyah** **et** **al.,** **2019)):** This strategy involves comparing today’s price with the
predicted price for tomorrow to identify potential buying and selling opportunities. We finetune the look-back window size using values in

[1 _,_ 3 _,_ 5 _,_ 10 _,_ 20 _,_ 30] and select the parameters that
perform best on the validation set.



7. **Informer (Zhou et al., 2021):** Informer utilizes
an efficient self-attention mechanism to capture
dependencies among variables. We adopt the recommended configuration for our experimental settings: a dropout rate of 0.05, two encoder layers,
one decoder layer, a learning rate of 0.0001, and the
Adam optimizer (Yi et al., 2024). The look-back
window size is selected using the same procedure
as for the LSTM.


8. **AutoFormer (Wu et al., 2021):** AutoFormer introduces a decomposition architecture by embedding
the series decomposition block as an inner operator, allowing for the progressive aggregation of the
long-term trend from intermediate predictions. We
use the recommended configuration for our experimental settings (Yi et al., 2024). The look-back
window size is selected using the same procedure
as for the LSTM.


9. **TimesNet (Wu et al., 2022):** TimesNet provides
a general framework for various time-series forecasting tasks. We adopt the recommended configurations for our experimental settings (Wu et al.,
2022). The look-back window size is selected using the same procedure as for the LSTM.


10. **PatchTST (Nie et al., 2022):** PatchTST proposes
an effective design for Transformer-based models
in time series forecasting by introducing two key
components: patching and a channel-independent
structure (Yi et al., 2024). The recommended configurations are used for our experimental settings.
The look-back window size is selected using the
same procedure as for the LSTM.


**F** **Author Statement**


As authors of the CryptoTrade, we hereby declare that
we assume full responsibility for any liability or infringement of third-party rights that may come up from the
use of our data. We confirm that we have obtained all
necessary permissions and/or licenses needed to share
this data with others for their own use. In doing so,
we agree to indemnify and hold harmless any person
or entity that may suffer damages resulting from our
actions.
Furthermore, we confirm that our CryptoTrade
dataset is released under the Creative Commons
Attribution-NonCommercial-ShareAlike (CC BY-NCSA) license. This license allows anyone to use, distribute, and modify our data for non-commercial purposes as long as they give proper attribution and share
the derivative works under the same license terms. We
believe that this licensing model aligns with our goal
of promoting open access to high-quality data while
respecting the intellectual property rights of all parties
involved.



1105


**G** **Hosting Plan**

We have chosen to host our code and data on GitHub
at [https://github.com/Xtra-Computing/](https://github.com/Xtra-Computing/CryptoTrade)
[CryptoTrade.](https://github.com/Xtra-Computing/CryptoTrade) Our decision is based on various
factors, including the platform’s ease of use, costeffectiveness, and scalability. We understand that accessibility is key when it comes to data management,
which is why we will ensure that our data is easily accessible through a curated interface. We also recognize
the importance of maintaining the platform’s stability
and functionality, and as such, we will provide the necessary maintenance to ensure that it remains up-to-date,
bug-free, and running smoothly.
At the heart of our project is the belief in open access
to data, and we are committed to making our data available to those who need it. As part of this commitment,
we will be updating our GitHub repository regularly, so
that users can rely on timely access to the most current
information. We hope that by using GitHub as our hosting platform, we can provide a user-friendly and reliable
solution for sharing our data with others.


1106


