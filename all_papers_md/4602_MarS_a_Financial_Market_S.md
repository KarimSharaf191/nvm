**000**

**001**


**002**

**003**

**004**

**005**

**006**

**007**


**008**

**009**

**010**

**011**

**012**

**013**


**014**

**015**

**016**

**017**

**018**


**019**

**020**

**021**

**022**

**023**

**024**


**025**

**026**

**027**

**028**

**029**

**030**


**031**

**032**

**033**

**034**

**035**

**036**


**037**

**038**

**039**

**040**

**041**


**042**

**043**

**044**

**045**

**046**

**047**


**048**

**049**

**050**

**051**

**052**

**053**



Under review as a conference paper at ICLR 2025

# MARS: A FINANCIAL MARKET SIMULATION ENGINE POWERED BY GENERATIVE FOUNDATION MODEL


**Anonymous authors**
Paper under double-blind review


ABSTRACT


Generative models aim to simulate realistic effects of various actions across different contexts, from text generation to visual effects. Despite significant efforts to
build real-world simulators, the application of generative models to virtual worlds,
like financial markets, remains under-explored. In financial markets, generative
models can simulate complex market effects of participants with various behaviors, enabling interaction under different market conditions, and training strategies without financial risk. This simulation relies on the finest structured data in
financial market like orders thus building the finest realistic simulation. We propose Large Market Model (LMM), an order-level generative foundation model,
for financial market simulation, akin to language modeling in the digital world.
Our financial Market Simulation engine (MarS), powered by LMM, addresses the
domain-specific need for realistic, interactive and controllable order generation.
Key observations include LMM’s strong scalability across data size and model
complexity, and MarS’s robust and practicable realism in controlled generation
with market impact. We showcase MarS as a forecast tool, detection system,
analysis platform, and agent training environment, thus demonstrating MarS’s
“paradigm shift” potential for a variety of financial applications.


1 INTRODUCTION


The primary aim of generative models is to simulate realistic effects of various actions across different contexts, such as text generation (Achiam et al., 2023) and visual effects (Brooks et al., 2024).
Real-world simulators enable human interaction with diverse scenes and objects (Mialon et al.,
2023), allow robots to learn from simulated experiences without physical risk (Du et al., 2023),
and generate vast amounts of realistic data for training other machine intelligence (Li et al., 2023).


While research on real-world simulators is extensive (Zhu et al., 2024; Yang et al., 2024), the application of generative models for virtual world simulation remains under-explored. The financial
market exemplifies such a virtual world where each action, from trade execution to strategy deployment, can have ripple effects across a complex network of market participants. The ability to model
and predict these effects in real time is crucial for traders, analysts, and regulators alike. Yet, current market simulation models – largely focused on statistical or agent-based approaches – lack the
resolution, interactivity, and realism needed to reflect the full complexity of order-level behaviors.


To address these gaps, it is crucial to integrate the vast amounts of structured financial data, such
as Limit Order Book (LOB) (Gould et al., 2013), that are essential for capturing market microstructures. We therefore propose the Large Market Model (LMM), a generative foundation model specifically designed for order-level financial market simulation. LMM builds on the successes of generative models in other domains but uniquely adapts them to the financial context, where the generation
of orders, order batches, and LOBs plays a critical role in understanding market dynamics. By
leveraging structured market data, LMM scales effectively with increasing data and model size, as
we will demonstrate through scaling law evaluation, revealing its potential for handling large-scale
financial markets. LMM’s design ensures that it can generate high-resolution market simulations,
capturing both fine-grained individual order actions and broader market trends.


Powered by LMM, we introduce MarS, a financial Market Simulation engine, unlocking new potential in financial market forecasting, risk detection, strategy analysis. MarS is designed to ensure
realism, producing simulated market trajectories that are robust enough for practical financial tasks


1


**054**

**055**


**056**

**057**

**058**

**059**

**060**

**061**


**062**

**063**

**064**

**065**

**066**

**067**


**068**

**069**

**070**

**071**

**072**


**073**

**074**

**075**

**076**

**077**

**078**


**079**

**080**

**081**

**082**

**083**

**084**


**085**

**086**

**087**

**088**

**089**

**090**


**091**

**092**

**093**

**094**

**095**


**096**

**097**

**098**

**099**

**100**

**101**


**102**

**103**

**104**

**105**

**106**

**107**



Under review as a conference paper at ICLR 2025


such as predictive modeling, risk management, and agent training. It is capable of providing controlled generation, blending users’ interactively injected orders into the generation of realistic market
behaviors, assessing the market impact of these actions. This feature ensures that MarS delivers not
only high-fidelity simulations but also controllable environments where financial strategies can be
safely tested and evaluated.


Among the broad adoption of AI techniques in finance (Zhang et al., 2024; Liu et al., 2023b; Kim
et al., 2019; Hou et al., 2021), MarS is the first to fully leverage the core elements of financial
markets, making it a powerful tool for a wide range of downstream applications. We posit that MarS
has the potential to bring paradigm shifts to a wide range of tasks related to the financial market. In
this work, we demonstrate its transformative potential in four specific use cases:


1. **Forecast** **Tool** : MarS generates subsequent orders based on recent orders and LOB, simulating future market trajectories. This enables precise forecasting by analyzing multiple
simulated trajectories.

2. **Detection System** : By generating multiple future market trajectories, MarS identifies potential risks not apparent from current observations. For example, a sudden drop in trajectory variance could indicate an impending significant event, providing early warnings and
enhancing risk management.

3. **Analysis Platform** : MarS answers a wide range of “what if” questions by providing a realistic simulation environment. For instance, it evaluates the market impact of large orders
by comparing existing market impact formulas to simulated results, identifying potential
improvements and gaining deeper insights into market dynamics.

4. **Agent Training Environment** : The realistic and responsive nature of MarS makes it ideal
for training reinforcement learning agents. This is demonstrated with an order execution
scenario, showcasing MarS’s potential for developing and refining trading strategies without real-world financial risks.


The main contributions of this paper are as follows:


    - We introduce the Large Market Model (LMM), a generative foundation model designed
specifically for financial market simulations, and demonstrate its scalability across data
size and model complexity. This establishes a new direction for domain-specific foundation
models in finance.

    - We develop MarS, a high-fidelity financial market simulation engine powered by LMM,
capable of generating realistic market scenarios and modeling the intricate impacts of orderlevel dynamics. This unlocks new possibilities for applying generative models in financial
markets.

    - We demonstrate the versatility of MarS through four key downstream applications: precise
market forecasting, risk detection, market impact analysis, and agent training for trading
strategies. These applications highlight the significant potential of MarS for transforming
financial industry practices.


2 MARS DESIGN


To create a truly realistic simulation system, MarS must excel in three key dimensions: highresolution, controllability, and interactivity.


High-resolution refers to the ability of MarS to faithfully replicate the intricate dynamics of financial
markets. This is why we leverage trading orders and order batches as the foundational elements
of the simulation system, since they encapsulate the investment behaviors of market participants.
These fine-grained data points are essential for accurately reproducing historical market trajectories,
ensuring that the simulation reflects real market conditions and behaviors with precision.


Controllability offers users the flexibility to simulate a wide range of market scenarios and circumstances. Under the scenarios of assessing market trends, monitoring potential risks, or optimizing
trading strategies, MarS provides the tools needed to explore any possible market condition. This
capability is particularly valuable for stress testing and strategy optimization, where diverse and
even rare extreme cases must be modeled accurately.


2


Under review as a conference paper at ICLR 2025



**108**

**109**


**110**

**111**

**112**

**113**

**114**

**115**


**116**

**117**

**118**

**119**

**120**

**121**


**122**

**123**

**124**

**125**

**126**


**127**

**128**

**129**

**130**

**131**

**132**


**133**

**134**

**135**

**136**

**137**

**138**


**139**

**140**

**141**

**142**

**143**

**144**


**145**

**146**

**147**

**148**

**149**


**150**

**151**

**152**

**153**

**154**

**155**


**156**

**157**

**158**

**159**

**160**

**161**


|Col1|Col2|Col3|
|---|---|---|
||||
||…|…|



Figure 1: High-Level Overview of MarS. MarS is powered by a generative foundation model (LMM)
trained on order-level historical financial market data. During real-time simulation, LMM dynamically generates order series in response to various conditions, including user-injected interactive
orders, vague target scenario descriptions, and current/recent market data. These generated order series, combined with user interactive orders, are matched in a simulated clearing house in real-time,
producing fine-grained simulated market trajectories. The flexibility of LMM’s order generation
enables MarS to support various downstream applications, such as forecasting, detection systems,
analysis platforms, and agent training environments.


Interactivity is crucial for enabling real-time user interaction with the simulated market. By allowing
users to inject their own orders into the system, it enable them to evaluate market impacts, including both first-order and second-order effects. This feature is vital for analyzing trading strategies,
managing systemic risks, and developing regulatory policies in a controlled, risk-free environment.


2.1 LARGE MARKET MODEL FOR FINANCIAL MARKET SIMULATION


**Problem Formulation.** To address the need for high-resolution, controllable, and interactive simulations, we propose the Large Market Model (LMM), a generative foundation model specifically designed for order-level financial market simulation. ~~This approach builds on the advanced generative~~
~~techniques originated from NLP-related domains but uniquely adapts them to the fnancial context,~~
~~where~~ ~~the~~ ~~generation~~ ~~of~~ ~~orders,~~ ~~and~~ ~~order~~ ~~batches,~~ ~~plays~~ ~~a~~ ~~critical~~ ~~role~~ ~~in~~ ~~capturing~~ ~~market~~
~~micro-structures.~~ The problem is formulated as a conditional generation task, where the generation of trading orders is conditioned on historical data, user-injected orders, and market matching
rules. LMM incorporates key features of the market microstructure such as Limit Order Books
(LOB), enabling it to capture both individual trading behaviors and systemic market dynamics.


~~LMM is designed to ensure that it can handle both fne-grained individual order actions and broader~~
~~market trends.~~ ~~By leveraging structured market data, LMM provides the foundation for generating~~
~~high-resolution simulations that refect real-world market dynamics.~~ ~~The conditional trading order~~
~~generation~~ ~~process~~ ~~is~~ ~~the~~ ~~cornerstone~~ ~~of~~ ~~LMM,~~ ~~enabling~~ ~~realistic~~ ~~simulations~~ ~~that~~ ~~incorporate~~
~~user-defned scenarios, historical data, and market-specifc matching rules.~~


**Tokenization of Order and Order-Batch.** LMM models the generation of trading orders as a conditional generation process, leveraging sequential modeling techniques to predict the evolution of
market states over time. This is achieved through a novel representation learning approach tailored
for financial industry’s structured data, particularly the order flows at two distinct scales: individual
orders and aggregated order-batches. The **Order Model**, using a causal transformer, tokenizes historical order sequences and Limit Order Book (LOB) information to ensure the realistic generation


3




















**162**

**163**


**164**

**165**

**166**

**167**

**168**

**169**


**170**

**171**

**172**

**173**

**174**

**175**


**176**

**177**

**178**

**179**

**180**


**181**

**182**

**183**

**184**

**185**

**186**


**187**

**188**

**189**

**190**

**191**

**192**


**193**

**194**

**195**

**196**

**197**

**198**


**199**

**200**

**201**

**202**

**203**


**204**

**205**

**206**

**207**

**208**

**209**


**210**

**211**

**212**

**213**

**214**

**215**



Under review as a conference paper at ICLR 2025


of individual trading orders. The tokenization procedure for the _i_ _[th]_ order is as follows:

_Embi_ = emb( _orderi_ ) + linear ~~p~~ roj( _LOBi_ [volumes] ) + emb( _LOBi_ [mid] ~~[p]~~ [rice] ) (1)


Here, _orderi_ denotes an index indicating its position in the tuple (type, price, volume, interval), with
type being one of [“Ask”, “Bid”, “Cancel”]. _LOBi_ [volumes] represents the 10-level volumes for asks
and bids in the LOB. The _LOBi_ [mid] [price] is the mid-price of the LOB, expressed as the number of
price tick changes since market opening.


In parallel, the **Order-Batch Model** coverts the order batches as the image-like format, and employs
VQ-VAE to represent and generate aggregated trading behaviors over discrete time intervals. In
practice, we convert one order-batch into an RGB image format. We refer to such images as “order
images”, demonstrated in Fig. 2.


|Col1|ID<br>Type<br>Price<br>Volume|Col3|Col4|ID<br>Type<br>Price<br>Volume|…|ID<br>Type<br>Price<br>Volume|Col8|
|---|---|---|---|---|---|---|---|
|||𝐦𝐢𝐧𝐮𝐭𝐞𝐭|𝐦𝐢𝐧𝐮𝐭𝐞𝐭|𝐦𝐢𝐧𝐮𝐭𝐞𝐭|𝐦𝐢𝐧𝐮𝐭𝐞𝐭|||



Figure 2: Order image converter. The orders we use contain three attributes: type, price slot, and
volume slot. There are three types of orders: Bid, Ask, and Cancel. The price slot refers to the slot
of the actual order price in relation to the mid-price at the beginning of the interval. The volume
slot is obtained by dividing the actual volume of the order into bins. The number of orders with the
same attributes is represented by the pixel values, where a higher pixel value at a certain position
indicates a higher number of orders with the same attributes.


These two components are combined in an ensemble framework, enabling LMM to use autoregressive modeling methods to construct a foundational generative model. This framework effectively integrates micro-level participant behaviors with macro-level market trends, making it suitable
for diverse market conditions and scenarios. By mapping market states into high-dimensional embeddings, LMM captures the complex, non-linear dependencies within historical data and temporal
patterns, providing a robust representation of market dynamics. This design bridges the gap between
theoretical generative modeling and practical financial simulations. For further details on the tokenization strategy and the architectural design of Order and Order-Batch Models, we refer the reader
to Appendix B and C.


2.1.1 CONDITIONAL TRADING ORDER GENERATION


In LMM, the generation of trading orders is modeled as a conditional generation process that adapts
to real-time market dynamics. An order clip is a sequence of trading orders **x** = ( _x_ 0 _, . . ., xn_ ),
generated based on the following four key conditions: **DES** ~~**T**~~ **EXT** : A general description of the
desired market scenario (e.g., “price bump” or “volatility crush”), ensuring controllability. **Inter-**
**active** **Orders** : ( ˙ _xi_ +1 _, . . .,_ _x_ ˙ _i_ + _j_ ) are user-injected orders after the _i_ -th generated order. If _j_ = 0,
there are no interactive orders between _xi_ and _xi_ +1. **Starting** **Sequence** : ( _x_ 0 _, . . ., xm−_ 1) are the
initial _m_ orders, often using recent real orders to forecast subsequent ones, enabling realistic simulations. **MTCH** ~~**R**~~ : Matching rules for trading orders, defining the feasible space for each order and
reflecting the specific financial market’s characteristics.


The conditional generation process: _p_ ( _xi_ + _j_ +1 _|{DES_ ~~_T_~~ _EXT,_ ( ˙ _xi_ +1 _, . . .,_ _x_ ˙ _i_ + _j_ ) _,_ ( _x_ 0 _, . . ., xm_ ) _, MTCH_ ~~_R_~~ _}_ )
ensures that generated orders are realistic and aligned with both the user-defined scenario and the
underlying market structure. They can be adjusted for various MarS scenarios, with different applications showcased in Sec. 4. We give a summarization of the input conditions and configurations
of different applications, along with the detailed introduction of **MTCH** ~~**R**~~ and **DES** ~~**T**~~ **EXT** in
Appendix F.


2.1.2 FRAMEWORK DESIGN OF LARGE MARKET MODEL


The LMM integrates two complementary approaches: Order Sequence Modeling and Order-Batch
Sequence Modeling, combined into an ensemble model to address financial market complexities.


4





**Max**


**Mid**


**Min**



**Bid**



**Ask**











**Price Slot**



**Price Slot**





**Price Slot**


**216**

**217**


**218**

**219**

**220**

**221**

**222**

**223**


**224**

**225**

**226**

**227**

**228**

**229**


**230**

**231**

**232**

**233**

**234**


**235**

**236**

**237**

**238**

**239**

**240**


**241**

**242**

**243**

**244**

**245**

**246**


**247**

**248**

**249**

**250**

**251**

**252**


**253**

**254**

**255**

**256**

**257**


**258**

**259**

**260**

**261**

**262**

**263**


**264**

**265**

**266**

**267**

**268**

**269**



Under review as a conference paper at ICLR 2025


**Order** **Sequence** **Modeling.** We use a causal transformer to encode each order and its preceding
Limit Order Book (LOB) information as a single token. This method captures the sequential nature
of orders, ensuring realistic order sequences that reflect market dynamics. **Order-Batch Sequence**
**Modeling.** To model structured patterns of dynamic market behavior over time intervals, we apply
an auto-regressive transformer to order-batch sequences. Orders within a time step are grouped into
batches, converted into a structured representation of market behavior for this time step, and modeled
to maintain coherence and continuity. **Ensemble** **Model.** Combining order sequence and orderbatch modeling, the ensemble model balances fine-grained control of individual orders with broader
market dynamics. This integration ensures detailed and contextually accurate market simulations.
**Fine-grained Signal Generation Interface.** We introduce an interface that maps vague descriptions
to fine-grained control signals using LLM-based historical market record retrieval. This guides the
ensemble model, ensuring simulations follow realistic market patterns and user-defined scenarios.


The bottom-left of Fig. 1 shows the framework of the Large Market Model. The detailed design of
its four parts can be found in Appendix B, C, D, E.


2.1.3 SCALING LAW IN LARGE MARKET MODEL


LMM’s scalability is a key perspective to assess its effectiveness in handling increasingly large-scale
financial markets. In our four-part foundation model design, we employ an auto-regressive transformer for order-batch sequences and a causal transformer for order sequences. These components
utilize standard pre-training techniques commonly applied in foundation models, including those
used in language modeling (Kaplan et al., 2020) and vision modeling (Zhai et al., 2022).


To assess the scalability of the LMM, we evaluated its performance across varying data scales and
model sizes. The scaling curves are shown in Fig. 3. Our findings indicate that as the size of the data
and the model increases, LMM’s performance improves significantly, consistent with the scaling
laws observed in other foundation models. This suggests that the potential of LMM can be further
unlocked by leveraging larger datasets and more extensive computational resources.


While the current implementation only taps into a fraction of the available order-level financial
market data due to resource constraints, the vast amount of data accessible within financial markets
holds tremendous promise for future enhancements. MarS, in this context, serves as the tool to
unearth this “gold mine” of data, indicating substantial opportunities for more comprehensive and
powerful market simulations.



(a) Order Model (b) Order-Batch Model

Figure 3: Scaling curves of Order Model and Order-Batch Model. ( **a** ) Order Model: Trained on 32 billion
tokens, with model sizes ranging from 2 million to 1.02 billion parameters. ( **b** ) Order-Batch Model: Trained on
10 billion tokens, with model sizes ranging from 150 million to 3 billion parameters. The results demonstrate
enhanced performance with increased data and model sizes.


2.2 MARS - ORDER GENERATION COMBINED WITH SIMULATED CLEARING HOUSE


Powered by LMM, the MarS engine is designed to generate highly realistic market trajectories that
are robust enough for practical financial tasks such as predictive modeling, risk management, and
agent training.


At the core of MarS is the simulated clearing house, which matches both generated and interactive
orders in real-time, providing extensive information (e.g., LOB) for subsequent order generation.
For each generated order _xi_, the clearing house processes it against any _j_ interactive orders ( _j_ _≥_ 0)


5



8.0


7.8


7.6


7.4


7.2


7.0









5.75

5.50

5.25

5.00

4.75

4.50

4.25





10 [9] 10 [10]

Number of Training Tokens



10 [9] 10 [10]

Number of Training Tokens


**270**

**271**


**272**

**273**

**274**

**275**

**276**

**277**


**278**

**279**

**280**

**281**

**282**

**283**


**284**

**285**

**286**

**287**

**288**


**289**

**290**

**291**

**292**

**293**

**294**


**295**

**296**

**297**

**298**

**299**

**300**


**301**

**302**

**303**

**304**

**305**

**306**


**307**

**308**

**309**

**310**

**311**


**312**

**313**

**314**

**315**

**316**

**317**


**318**

**319**

**320**

**321**

**322**

**323**



Under review as a conference paper at ICLR 2025


injected by the user. The results of this matching process, including the recent LOB, are then used
to generate the next order _xi_ + _j_ +1, creating a continuous and dynamic simulation.


MarS excels at providing controlled generation, blending users’ interactively injected orders into
the generation of realistic market behaviors. Users can inject their own orders into the system and
observe how these actions impact market dynamics in real-time. This capability allows users to
simulate various trading strategies, assess market impacts, and evaluate the performance of their
strategies under different conditions. The blending process is carefully managed in MarS by adhering to two guiding principles.


    - **“Shaping** **the** **Future** **Based** **on** **Realized** **Realities”.** At each time step, the order-batch
model generates the next order-batch based on recent orders and corresponding matching
results from the simulated clearing house. These information conclude the immediate market impact of users’ injected orders and determines the generated market behaviors in the
next order-batch.

    - **“Electing** **the** **Best** **from** **Every** **Possible** **Future”.** Multiple predicted order-batches are
generated at each time step and the best match to the fine-grained control signal is selected,
ensuring the simulation remains realistic while allowing for user control.


The order-level transformer, trained on historical orders, naturally learns immediate market impact
for subsequent order generation. Concurrently, the ensemble model influences order generation,
aligning with the generated next order-batch. Fig. 4 illustrates the generation process, balancing
injected orders’ market impact and control signals to form a realistic simulation.


3 EXPERIMENTS


This section evaluates the capabilities of MarS in providing realistic, interactive, and controllable
simulations. Note that throughout our experiments, the term “ **replay** ” refers to replaying real historical market data within MarS to validate the simulation against real-world events.


3.1 REALISTIC SIMULATIONS


To assess the realism of MarS’s market simulations, we compare simulated data against key stylized
facts derived from historical market data (Sherkar & Sen, 2023). These stylized facts serve as robust
benchmarks, ensuring market simulations accurately reflect real-world market behaviors (Vyetrenko
et al., 2020; Coletta et al., 2022; Stillman et al., 2023). Fig. 5 presents several prevalent stylized facts.
MarS successfully replicates these stylized facts, demonstrating its capability to produce highly
realistic market simulations suitable for practical applications. Beside current three stylized facts,
we provide a detailed evaluation of eleven stylized facts in Appendix I and a quantitative analysis in
Appendix J.


3.2 INTERACTIVE SIMULATIONS


Understanding market impacts, i.e., changes in financial markets caused by trading activity, is crucial. MarS simulates these impacts by generating orders from detailed order-level data. Fig. 6a
illustrates MarS interacting with a trading agent executing a TWAP (Time-Weighted Average Price)
strategy, showing price trajectories and trading order details. Synthetic market impact is defined as
the difference (or ”gap”) between the simulated price trajectory, influenced by the trading agents
actions, and the real historical price trajectory. This gap quantifies the impact of the agents trading
strategies on the market. A clear example can be seen in Fig. 6a: during the interval 9:459:50, the
agent executes a TWAP strategy, causing observable changes in the subsequent price trajectory. The
gap between the two curves represents the synthetic market impact generated by the agents trading
actions. More detailed exploration over market ipamct could be found in Sec.4.3.


We validated these simulations by collecting market impacts from agents with various configurations, confirming that the synthetic data adheres to the _Square-Root-Law_, as depicted in Fig. 6b. The
_Square-Root-Law_, ∆ _∝_ _σ_ - _Q/V_, is a widely used model for market impact (Moro et al., 2009;
Lillo et al., 2003; Almgren et al., 2005), where ∆ is the price change, _σ_ is the volatility, _Q_ is the
trading volume, and _V_ is the total market volume. These results illustrate that MarS can effectively


6


Under review as a conference paper at ICLR 2025



**324**

**325**


**326**

**327**

**328**

**329**

**330**

**331**


**332**

**333**

**334**

**335**

**336**

**337**


**338**

**339**

**340**

**341**

**342**


**343**

**344**

**345**

**346**

**347**

**348**


**349**

**350**

**351**

**352**

**353**

**354**


**355**

**356**

**357**

**358**

**359**

**360**


**361**

**362**

**363**

**364**

**365**


**366**

**367**

**368**

**369**

**370**

**371**


**372**

**373**

**374**

**375**

**376**

**377**



model the impact of trading strategies on market prices, providing valuable insights for market participants and aiding in the development of more robust trading strategies. Additional details and
results about the TWAP agent and market impact can be found in Appendix H and K.


7

































Figure 4: The process of MarS generation. There is a two-level order generation in MarS — minutelevel order-batch generation and individual order generation. Order-Batch Level: At the end of
_minutet_, all generated and interactive orders are counted as the realized order-batch distribution
and appended to a series. Following the two guiding principles in Sec. 2.2, the Order-batch model
generates _N_ possible distributions for _minutet_ +1 based on the series. Then, following the ”Electing
the Best from Every Possible Future” principle, the target distribution ( _⋆_ ) is selected based on the
consistency of the minute-level control signal. Order Level: The Order Model generates immediate
responses for recent and user-submitted interactive orders. A logits vector indicates the expected
distribution of the next order. To balance market impact and controlled generation, an ensemble
model refines the next order distribution based on the target order-batch distribution of _minutet_ +1 ( _⋆_ ).
By sampling from refined logits, an order is generated in _minutet_ +1 and becomes the latest market
order. Before the end of _minutet_ +1, the Order Model and Ensemble Model iteratively generate
orders. In summary, within a minute, the order-batch serves as a target distribution, refining the order
model, while between minutes, the inter-minute order-batch is influenced by the orders completed in
the previous minute. The iterative generation of orders and order-batches ensures a realistic balance
between market impact and controlled generation.



Log Return of Simulation



Auto-Correlation of Log Return



Auto-Correlation of Volatility





0.004 0.002 0.000 0.002 0.004
Log Return











0.3


0.2


0.1


0.0





0.25


0.20


0.15


0.10





1 2 3 4 5
Lag (Minutes)



1 2 3 4 5
Lag (Minutes)



(a) Aggregational Gaussianity (b) Absence of Autocorrelations (c) Volatility Clustering


Figure 5: Illustration of Stylized Facts in MarS. ( **a** ) Aggregational Gaussianity: as the interval increases from
1 to 5 minutes, the distribution of log returns becomes more similar to a normal distribution. ( **b** ) Absence of
Autocorrelations: the auto-correlation of log returns rapidly decreases with increasing intervals. ( **c** ) Volatility
Clustering: high volatility auto-correlation is observed over periods.



(a) Aggregational Gaussianity



(b) Absence of Autocorrelations


Under review as a conference paper at ICLR 2025



**378**

**379**


**380**

**381**

**382**

**383**

**384**

**385**


**386**

**387**

**388**

**389**

**390**

**391**


**392**

**393**

**394**

**395**

**396**


**397**

**398**

**399**

**400**

**401**

**402**


**403**

**404**

**405**

**406**

**407**

**408**


**409**

**410**

**411**

**412**

**413**

**414**


**415**

**416**

**417**

**418**

**419**


**420**

**421**

**422**

**423**

**424**

**425**


**426**

**427**

**428**

**429**

**430**

**431**



3.3 CONTROLLABLE SIMULATIONS


We demonstrate the controllability of MarS by replicating historical events. Specifically, MarS allows two types of control signals: _{_ **replay curve** _,_ **prompt** _}_ . For control with **replay curve**, we simulate a price change between 0.3% and 0.5% over 5 minutes. With control enabled, an order batch is
generated using minute-level guiding signals from the replay curve, integrated with the order model
within an ensemble model to produce trading orders. Fig. 6c depicts the correlation between simulated and replay price trajectories. The introduction of control signals significantly enhances the
correlation scores (0 _._ 23 _→_ 0 _._ 47), showcasing MarS’s effectiveness in generating controllable market simulations. Fig. 6c also illustrates the balance between control and interaction. Configurations
with control but no interaction achieve the highest correlation scores, while introducing interaction
reduces control precision (0 _._ 47 _→_ 0 _._ 33). This inherent balance allows for more realistic interactions in diverse applications. For control with **prompt**, MarS allows users to use natural language to
describe specific historical scenarios, then utilizes Large Language Models(LLMs) to guide the generation through the **fine-grained signal generation interface** . The details and results are provided
in Appendix E.


4 APPLICATIONS


In Sec.2 and 3, we demonstrated the formulation of diverse financial tasks as a conditional trading order generation problem. Our experiments confirmed that MarS is Realistic, Controllable, and
Interactive, establishing it as a robust financial market simulator. This section explores potential
downstream applications of MarS, further validating its foundational role in financial market simulation. We present practical financial tasks to illustrate: a) MarS’s capability to solve financial
problems independently, and b) its utility as a simulation platform for other tasks. For a), we showcase Forecast and Detection tasks, and for b), we provide examples of “What if” Analysis, and
Reinforcement Learning Environment.


Here, we highlight that, analogous to text generation vs. language modeling (Achiam et al., 2023;
Abdin et al., 2024; Dubey et al., 2024), and video generation vs. physical world decision making
(Liu et al., 2024; Yang et al., 2024; 2023a), we have constructed a unified task interface through conditional trading order generation for diverse financial downstream tasks with MarS. This interface
can transfer complex and diverse financial information into specific tasks.


We compare current methodologies with the new paradigm introduced by MarS to illustrate the
“paradigm shift” across various types of financial tasks, as shown in Table 1. The introduction and
details of each task can be found in the subsequent sections.


4.1 FORECASTING

Forecasting is crucial in many financial applications, with market trend forecasting being a prime
example. This task demands models that accurately capture and reflect market dynamics. Traditionally, direct forecasting models are used. In this section, we assess the effectiveness of our market
simulation in predicting trends.


Following Ntakaris et al. (2018), we define the price change from _t_ to _t_ + _k_ minute as: _l_ =
�� _n_ 1 - _ni_ =1 _[m][i]_ - _−_ _m_ 0� _/m_ 0, where _m_ 0 is the mid-price at time _t_, _n_ is the number of orders between _t_ and _t_ + _k_ minutes, and _mi_ is the mid-price after the _i_ th order event. The price change is


8



0.012

0.010

0.008

0.006

0.004

0.002

0.000





45.2


45.0


44.8


44.6



09:35 09:40 09:45 09:50 09:55 10:00
Time



0.00000.00050.00100.00150.0020



w/o Control w/ Control



(a) Illustration of market interaction



(b) Square-Root-Law Validation (c) Effects of control signals


Figure 6: Results of interactive and controllable simulations in MarS.



(b) Square-Root-Law Validation


**432**

**433**


**434**

**435**

**436**

**437**

**438**

**439**


**440**

**441**

**442**

**443**

**444**

**445**


**446**

**447**

**448**

**449**

**450**


**451**

**452**

**453**

**454**

**455**

**456**


**457**

**458**

**459**

**460**

**461**

**462**


**463**

**464**

**465**

**466**

**467**

**468**


**469**

**470**

**471**

**472**

**473**


**474**

**475**

**476**

**477**

**478**

**479**


**480**

**481**

**482**

**483**

**484**

**485**



Under review as a conference paper at ICLR 2025

|Applications|Current Methods|MarS|
|---|---|---|
|Forecasting<br>Detection<br>“What if” Analysis<br>RL Environment|sequence extrapolation<br>Dif(_marketnow, marketpast_)<br>online experiments, empirical formula<br>fnite data, fake_ P_(_st_+1_|st, at_)|conditional generation<br>Dif(_marketnow, simu-marketnow_)<br>offine data-driven pipeline<br>infnite data, real_ P_(_st_+1_|st, at_)|



Table 1: Summary of how MarS reshapes mainstream financial applications. Diff( _·, ·_ ) represents
the difference between two market states for anomaly detection. _P_ ( _st_ +1 _|st, at_ ) denotes the state
transition given the current state and action. Without an interactive environment, most existing
financial RL works cannot model the realistic impact of market state caused by agent actions.Further
details of RL-Environment are in Sec.4.4

.


categorized into three classesup, down, and flatbased on the value of _l_, ensuring similar probabilities for each class over the training period. We compare our model with DeepLOB by Zhang et al.
(2019), a well-known baseline. Fig. 7a illustrates that LMM-based simulations significantly outperform DeepLOB, highlighting its superior market dynamics understanding. Additionally, the 1.02
billion-parameter model outperforms the 0.22 billion-parameter model, indicating that improved
validation loss in scaling curve (Fig. 3) correlates with enhanced forecasting performance.


It is noteworthy that all forecasting targets can be calculated using simulated trajectories from MarS,
whereas traditional direct forecasting models require separate training for each target. This underscores the significant advantage of simulation-based forecasting by MarS, For more discussion
about the comparison between DeepLOB and MarS/LMM, please refer to Appendix L..


|Fulfillment Rate: 0.968|Col2|
|---|---|
|||
|Fulfillment Rate: 0.997|Fulfillment Rate: 0.997|
|0|500<br>1000<br>1500<br>Step|



(a) Trend Prediction Accuracy (b) Training performance of the trading agent

Figure 7: Results of forecasting and RL-agent training tasks. For forecasting task, MarS executes 128 simulations at each initial time point, and aggregate outcomes to determine the final predicted class. The ground truth
is obtained from historical replay. For RL-agent training, the x-axis represents the number of update batches,
and the y-axis is the price advantage over our best-configured TWAP agent (L1-P0.9), in basis points (BP).


4.2 DETECTION

Detecting the changing state of market is crucial in financial tasks, especially in the regulation of
market abuse, e.g., insider trading (Meulbroek, 1992) and market manipulation (Putnin¸ˇs, 2012). Traditional methods for detecting illegal activities are time-consuming and challenging, and abnormal
market states are often defined and detected based on the differences between current and historical
market patterns. In this section, we take market manipulation as an example, and demonstrate how
MarS could bring a new simulation-based paradigm to detection task.


As we evaluated MarS’s realism in a normal market in Sec.3, a straightforward principle for anomaly
detection is that a quick drop in simulation realism metrics can serve as an initial indicator of potential anomalies. To verify it, we collected several market manipulation cases from China Securities
Regulatory Commission (CSRC) [1] . For each stock, we collected replay samples before, during and
after the manipulation, and conducted simulations by MarS simultaneously. Through calculating
Distribution Similarity [2], we evaluate the similarity of spread distributions, which serves as a key
indicator of market liquidity. This metric is used for comparison between replay and simulation.
More details on the experimental settings are provided in AppendixG.


[1http://www.csrc.gov.cn](http://www.csrc.gov.cn)
[2https://en.wikipedia.org/wiki/Overlap_coefficient](https://en.wikipedia.org/wiki/Overlap_coefficient)


9





0.65


0.60


0.55


0.50



1 2 3 4 5
Number of Minutes



6

4

2

0

2

4

6


**486**

**487**


**488**

**489**

**490**

**491**

**492**

**493**


**494**

**495**

**496**

**497**

**498**

**499**


**500**

**501**

**502**

**503**

**504**


**505**

**506**

**507**

**508**

**509**

**510**


**511**

**512**

**513**

**514**

**515**

**516**


**517**

**518**

**519**

**520**

**521**

**522**


**523**

**524**

**525**

**526**

**527**


**528**

**529**

**530**

**531**

**532**

**533**


**534**

**535**

**536**

**537**

**538**

**539**



Under review as a conference paper at ICLR 2025


Fig. 8 shows the varying spread distributions in different periods around manipulation. While MarS
generally performs well to simulate the normal market, its performance drops during the manipulation, showing a heavier tail and a peak around _δ_ = 1000. These anomalies can be viewed
as signals likely corresponding to market manipulation, where manipulators significantly impact
liquidity, widening the spread. These anomalies, less frequent in normal markets, lead to a performance drop in MarS, suggesting a new detection approach by monitoring such similarity drops.
Consequently, MarS can help investors avoid anomalies and assist financial institutions in maintaining market stability. It is important to note that a single anomaly does not conclusively indicate
market manipulation. Instead, it serves as an initial signal that requires further holistic assessment,
combining multiple metrics to ensure robust conclusions. The example provided serves as a representative illustration of our approach. Our primary objective in this experiment was to demonstrate
the paradigm shift MarS offers in market manipulation detection.



(a) Resiliency (b) LOB ~~p~~ ressure (c) LOB ~~d~~ epth

Figure 9: Effects of new factors on market impact.

**Dynamics** **of** **Long-Term** **Market** **Impact:** The long-term market impact, also known as price
impact trajectory, typically manifests as a gradually decaying sequence of price fluctuation after a
trade. Traditional research relies on empirical formulas to model this dynamics (Gatheral et al.,



Distribution Similarity: 0.870



Distribution Similarity: 0.835



Distribution Similarity: 0.873



0.25

0.20

0.15

0.10

0.05

0.00



250 500 750 1000
Value





500 1000
Value





200 400 600 800
Value









0.25

0.20

0.15

0.10

0.05

0.00





0.20


0.15


0.10


0.05


0.00



(a) Pre-manipulation (b) Manipulation period (c) Post-manipulation

Figure 8: Spread distribution in different periods of market manipulation. During the manipulation period, the
distribution similarity drops, and a heavier tail along with a noticeable peak arises.



(a) Pre-manipulation



(b) Manipulation period



4.3 “WHAT IF” ANALYSIS ON MARKET IMPACT

One of the most important “What if” topics in finance is to analyze market impact, the change in
asset prices caused by trading activity. Due to complex mechanisms, most existing research in this
area relies heavily on strong assumptions and empirical formulas (Zarinelli et al., 2015; Almgren
et al., 2005; Gatheral, 2010; Gatheral et al., 2012; 2011), and is limited to costly and risky online
experiments. In this section, we take market impact as an example, showing how MarS can act as
a reliable and powerful platform and contribute to “what if” analysis. As we have validated the
reliability of synthetic market impact in Sec.3.2, we step to a more ambitious goal: leverage the
synthetic data to build data-driven pipeline to discover new laws to explain market impact and its
long-term dynamics. Due to the limited space, details of experiment settings, clarification, and more
results in this section are provided in Appendix K.


**New factors beyond Square-Root-Law:** To uncover new factors beyond Square-Root-Law influencing market impact, we first employed symbolic regression (de Silva et al., 2020), using classic
volume and price factors before trading as the base dictionary. By applying genetic algorithms in
multiple iterations, we sought to identify the most informative factors on synthetic market impact.
The preliminary results were reviewed and refined by domain experts, leading to the discovery of
three new factors that partially explain market impact: _{resiliency_, _LOB_ ~~_p_~~ _ressure_, _LOB_ ~~_d_~~ _epth}_ . In
Fig. 9, we plot the relationships between market impact and these three factors.



0.010


0.008


0.006


0.004


0.002









0.010


0.008


0.006


0.004


0.002





0.005


0.004


0.003


0.002







6 8



0.0 0.2 0.4 0.6



8 10 12



(a) Resiliency



(b) LOB ~~p~~ ressure



10


**540**

**541**


**542**

**543**

**544**

**545**

**546**

**547**


**548**

**549**

**550**

**551**

**552**

**553**


**554**

**555**

**556**

**557**

**558**


**559**

**560**

**561**

**562**

**563**

**564**


**565**

**566**

**567**

**568**

**569**

**570**


**571**

**572**

**573**

**574**

**575**

**576**


**577**

**578**

**579**

**580**

**581**


**582**

**583**

**584**

**585**

**586**

**587**


**588**

**589**

**590**

**591**

**592**

**593**



Under review as a conference paper at ICLR 2025


2011; Donier et al., 2015a; Bacry et al., 2015), but could struggle to capture the full complexity
of real-world scenarios. To address this, we leverage generated market impact to develop a more
accurate, data-driven approach. Our method models the decay of market impact using an ordinary
differential equation (ODE), which integrates both potential influencing factors and decay functions:



4.4 REINFORCEMENT LEARNING ENVIRONMENT

The MarS environment, being both realistic and interactive, is ideal for training reinforcement learning (RL) agents. This environment accurately reflects an agent’s impact, provides realistic rewards,
and facilitates training robust agents for the financial market. In this experiment, we aim to train a
trading agent from scratch using MarS. The trading agent’s goal is to purchase a large volume within
5 minutes, optimizing both fulfillment rate and price advantage.


The trading agent’s state includes features such as remaining time, remaining volume, LOB imbalance, and the period’s stage (passive or aggressive). The agent’s actions are based on a configurable
TWAP strategy (detailed in Appendix H), where it can adjust the maximum passive volume ratio
(PVR) from _{_ 0, 0.1, ..., 1 _}_ and aggressive price (AP) in _{_ 0, 1, 2, 3, 4, 5 _}_ . The reward function is
defined as follows:
Reward = _α ×_ FulfillmentRate + PriceAdvantage (3)


where _α_ = 1 when FulfillmentRate _≤_ 0 _._ 95 and decreases to 0 as FulfillmentRate approaches 1.


For training, we used a batch size of 8192 and a learning rate of 4 _×_ 10 _[−]_ [5] . The trading model was
updated using a simple policy gradient algorithm (Sutton & Barto, 2018). The performance metric is
the price advantage over our best-configured TWAP agent (L1-P0.9), measured in basis points (BP,
1/10000). Fig. 7b shows the training performance of the trading agent. The agent’s performance
improves from -6 BP to 2˜6 BP during training. The observed fluctuations between 2˜6 BP are
attributed to the agent exploring various strategies between high and low fulfillment rates, resulting
in corresponding variations in price advantage based on the current reward setting.


This demonstration highlights that MarS is capable of training trading agents from scratch by leveraging its realistic and interactive simulation capabilities. A more comprehensive training design
could yield even stronger performance.


11



_n_

- _Wi,jXiFj_ [decay] ( _t_ ) (2)

_j_ =1



_dY_ ( _t_ )

= sum( _W_ _◦_ ( _X_ _⊗_ _F_ [decay] ( _t_ ))) =
_dt_



_m_



_i_ =1



where _Y_ ( _t_ ) is the long-term market impact, _X_ _∈_ R _[m]_ is the factor group, such as volume, price, etc.,
_√_
and _F_ [decay] ( _t_ ) : _t_ _→_ R _[n]_ includes possible decay functions, e.g., [1 _/t, . . .,_ 1 _/_ _t_ ]. _X_ and _F_ [decay] ( _t_ )

can be customized based on domain knowledge. _⊗_ is the outer product, _◦_ is the Hadamard product,
_X_ _[T]_ _⊗_ _F_ [decay] ( _t_ ) is a matrix with size R _[n][×][m]_, representing interactions among factors and decay
patterns, and _W_ _∈_ R _[n][×][m]_ is the learnable interaction weight. Eq.2 can explain how factors with
different decay patterns contribute to the long-term market impact. Fig. 10a illustrates the autocorrelation of the synthetic market impact decay, and trajectories predicted by the learned ODE, and
the base ODE from empirical formulas (Gatheral et al., 2011; Curato et al., 2017). Fig. 10b shows
the learned weights _W_, demonstrating the importance of interaction pairs of two decay functions
and seven factors, which can help to deepen our understanding of the long-term market impact.



1.000

0.975

0.950

0.925

0.900

0.875

0.850

0.825



1 2 3 4 5
Lag(minutes)





1


0



(a) Auto-correlation of long-term market impact (b) Interaction weights of learned ODE


Figure 10: Learned ODE of second-order market impact and the interpretable interaction weights.


**594**

**595**


**596**

**597**

**598**

**599**

**600**

**601**


**602**

**603**

**604**

**605**

**606**

**607**


**608**

**609**

**610**

**611**

**612**


**613**

**614**

**615**

**616**

**617**

**618**


**619**

**620**

**621**

**622**

**623**

**624**


**625**

**626**

**627**

**628**

**629**

**630**


**631**

**632**

**633**

**634**

**635**


**636**

**637**

**638**

**639**

**640**

**641**


**642**

**643**

**644**

**645**

**646**

**647**



Under review as a conference paper at ICLR 2025


5 RELATED WORK


We give a detailed and comprehensive discussion of related work on financial market simulation and
generative foundation model in Appendix A.


6 CONCLUSION


We introduce MarS, an order-level, fine-grained realistic financial market simulation engine, powered by the generative foundation model, LMM. This paper is the first to evaluate scaling capabilities
in financial order generation. Our evaluation of LMM’s scaling law demonstrates the potential for
continuous improvement in future financial world models. We identify three essential characteristics of impactful market simulation: realism, controllability, and interactivity, crucial for various
financial tasks. Designed with these traits, MarS’s simulation framework proves robust for practical
applications. We present four representative financial applications developed using MarS, underscoring its potential to catalyze a paradigm shift across various financial applications.


12


**648**

**649**


**650**

**651**

**652**

**653**

**654**

**655**


**656**

**657**

**658**

**659**

**660**

**661**


**662**

**663**

**664**

**665**

**666**


**667**

**668**

**669**

**670**

**671**

**672**


**673**

**674**

**675**

**676**

**677**

**678**


**679**

**680**

**681**

**682**

**683**

**684**


**685**

**686**

**687**

**688**

**689**


**690**

**691**

**692**

**693**

**694**

**695**


**696**

**697**

**698**

**699**

**700**

**701**



Under review as a conference paper at ICLR 2025


REFERENCES


Marah Abdin, Sam Ade Jacobs, Ammar Ahmad Awan, Jyoti Aneja, Ahmed Awadallah, Hany
Awadalla, Nguyen Bach, Amit Bahree, Arash Bakhtiari, Harkirat Behl, et al. Phi-3 technical report: A highly capable language model locally on your phone. _arXiv preprint arXiv:2404.14219_,
2024.


Josh Achiam, Steven Adler, Sandhini Agarwal, Lama Ahmad, Ilge Akkaya, Florencia Leoni Aleman, Diogo Almeida, Janko Altenschmidt, Sam Altman, Shyamal Anadkat, et al. Gpt-4 technical
report. _arXiv preprint arXiv:2303.08774_, 2023.


John Affleck-Graves, Carolyn M Callahan, and Ramachandran Ramanan. Detecting abnormal bidask spread: a comparison of event study methods. _Review of Quantitative Finance and Account-_
_ing_, 14:45–65, 2000.


Robert Almgren, Chee Thum, Emmanuel Hauptmann, and Hong Li. Direct estimation of equity
market impact. _risk_, 2005.


Selim Amrouni, Aymeric Moulin, Jared Vann, Svitlana Vyetrenko, Tucker Balch, and Manuela
Veloso. Abides-gym: gym environments for multi-agent discrete event simulation and application to financial markets. In _Proceedings of the Second ACM International Conference on AI in_
_Finance_, pp. 1–9, 2021.


Emmanuel Bacry, Adrian Iuga, Matthieu Lasnier, and Charles-Albert Lehalle. Market impacts and
the life cycle of investors orders, 2014. [URL https://arxiv.org/abs/1412.0217.](https://arxiv.org/abs/1412.0217)


Emmanuel Bacry, Adrian Iuga, Matthieu Lasnier, and Charles-Albert Lehalle. Market impacts and
the life cycle of investors orders. _Market Microstructure and Liquidity_, 1(02):1550009, 2015.


Yutong Bai, Xinyang Geng, Karttikeya Mangalam, Amir Bar, Alan L Yuille, Trevor Darrell, Jitendra
Malik, and Alexei A Efros. Sequential modeling enables scalable learning for large vision models.
In _Proceedings_ _of_ _the_ _IEEE/CVF_ _Conference_ _on_ _Computer_ _Vision_ _and_ _Pattern_ _Recognition_, pp.
22861–22872, 2024.


Gagan Bhatia, El Moatez Billah Nagoudi, Hasan Cavusoglu, and Muhammad Abdul-Mageed.
Fintral: A family of gpt-4 level multimodal financial large language models. _arXiv_ _preprint_
_arXiv:2402.10986_, 2024.


Rishi Bommasani, Drew A Hudson, Ehsan Adeli, Russ Altman, Simran Arora, Sydney von Arx,
Michael S Bernstein, Jeannette Bohg, Antoine Bosselut, Emma Brunskill, et al. On the opportunities and risks of foundation models. _arXiv preprint arXiv:2108.07258_, 2021.


Tim Brooks, Bill Peebles, Connor Holmes, Will DePue, Yufei Guo, Li Jing, David Schnurr, Joe
Taylor, Troy Luhman, Eric Luhman, Clarence Ng, Ricky Wang, and Aditya Ramesh. Video
generation models as world simulators. 2024. URL [https://openai.com/research/](https://openai.com/research/video-generation-models-as-world-simulators)
[video-generation-models-as-world-simulators.](https://openai.com/research/video-generation-models-as-world-simulators)


Tom B Brown. Language models are few-shot learners. _arXiv preprint arXiv:2005.14165_, 2020.


David Byrd, Maria Hybinette, and Tucker Hybinette Balch. Abides: Towards high-fidelity multiagent market simulation. In _Proceedings of the 2020 ACM SIGSIM Conference on Principles of_
_Advanced Discrete Simulation_, pp. 11–22, 2020.


Ricky T. Q. Chen. torchdiffeq, 2018. URL [https://github.com/rtqichen/](https://github.com/rtqichen/torchdiffeq)
[torchdiffeq.](https://github.com/rtqichen/torchdiffeq)


Carl Chiarella, Giulia Iori, and Josep Perell´o. The impact of heterogeneous trading rules on the limit
order book and order flows. _Journal of Economic Dynamics and Control_, 33(3):525–537, 2009.


Andrea Coletta, Matteo Prata, Michele Conti, Emanuele Mercanti, Novella Bartolini, Aymeric
Moulin, Svitlana Vyetrenko, and Tucker Balch. Towards realistic market simulations: a generative adversarial networks approach. In _Proceedings of the Second ACM International Conference_
_on AI in Finance_, pp. 1–9, 2021.


13


**702**

**703**


**704**

**705**

**706**

**707**

**708**

**709**


**710**

**711**

**712**

**713**

**714**

**715**


**716**

**717**

**718**

**719**

**720**


**721**

**722**

**723**

**724**

**725**

**726**


**727**

**728**

**729**

**730**

**731**

**732**


**733**

**734**

**735**

**736**

**737**

**738**


**739**

**740**

**741**

**742**

**743**


**744**

**745**

**746**

**747**

**748**

**749**


**750**

**751**

**752**

**753**

**754**

**755**



Under review as a conference paper at ICLR 2025


Andrea Coletta, Aymeric Moulin, Svitlana Vyetrenko, and Tucker Balch. Learning to simulate
realistic limit order book markets from data as a world agent. In _Proceedings_ _of_ _the_ _third_ _acm_
_international conference on ai in finance_, pp. 428–436, 2022.


Andrea Coletta, Joseph Jerome, Rahul Savani, and Svitlana Vyetrenko. Conditional generators for
limit order book environments: Explainability, challenges, and robustness. In _Proceedings of the_
_Fourth ACM International Conference on AI in Finance_, pp. 27–35, 2023.


Rama Cont. Empirical properties of asset returns: stylized facts and statistical issues. _Quantitative_
_finance_, 1(2):223, 2001.


Gianbiagio Curato, Jim Gatheral, and Fabrizio Lillo. Optimal execution with non-linear transient
market impact. _Quantitative Finance_, 17(1):41–54, 2017.


Brian M de Silva, Kathleen Champion, Markus Quade, Jean-Christophe Loiseau, J Nathan Kutz, and
Steven L Brunton. Pysindy: a python package for the sparse identification of nonlinear dynamics
from data. _arXiv preprint arXiv:2004.08424_, 2020.


Jonathan Donier, Julius Bonart, Iacopo Mastromatteo, and J-P Bouchaud. A fully consistent, minimal model for non-linear market impact. _Quantitative finance_, 15(7):1109–1121, 2015a.


Jonathan Donier, Julius Bonart, Iacopo Mastromatteo, and Jean-Philippe Bouchaud. A fully consistent, minimal model for non-linear market impact, 2015b. [URL https://arxiv.org/abs/](https://arxiv.org/abs/1412.0141)
[1412.0141.](https://arxiv.org/abs/1412.0141)


Yilun Du, Sherry Yang, Bo Dai, Hanjun Dai, Ofir Nachum, Josh Tenenbaum, Dale Schuurmans, and Pieter Abbeel. Learning universal policies via text-guided video generation. In
A. Oh, T. Naumann, A. Globerson, K. Saenko, M. Hardt, and S. Levine (eds.), _Advances_ _in_
_Neural_ _Information_ _Processing_ _Systems_, volume 36, pp. 9156–9172. Curran Associates, Inc.,
2023. URL [https://proceedings.neurips.cc/paper_files/paper/2023/](https://proceedings.neurips.cc/paper_files/paper/2023/file/1d5b9233ad716a43be5c0d3023cb82d0-Paper-Conference.pdf)
[file/1d5b9233ad716a43be5c0d3023cb82d0-Paper-Conference.pdf.](https://proceedings.neurips.cc/paper_files/paper/2023/file/1d5b9233ad716a43be5c0d3023cb82d0-Paper-Conference.pdf)


Abhimanyu Dubey, Abhinav Jauhri, Abhinav Pandey, Abhishek Kadian, Ahmad Al-Dahle, Aiesha
Letman, Akhil Mathur, Alan Schelten, Amy Yang, Angela Fan, et al. The llama 3 herd of models.
_arXiv preprint arXiv:2407.21783_, 2024.


Patrick Esser, Robin Rombach, and Bjorn Ommer. Taming transformers for high-resolution image
synthesis. In _Proceedings of the IEEE/CVF conference on computer vision and pattern recogni-_
_tion_, pp. 12873–12883, 2021.


Jim Gatheral. No-dynamic-arbitrage and market impact. _Quantitative finance_, 10(7):749–759, 2010.


Jim Gatheral, Alexander Schied, and Alla Slynko. Exponential resilience and decay of market
impact. _Econophysics of Order-driven Markets:_ _Proceedings of Econophys-Kolkata V_, pp. 225–
236, 2011.


Jim Gatheral, Alexander Schied, and Alla Slynko. Transient linear price impact and fredholm integral equations. _Mathematical Finance:_ _An International Journal of Mathematics, Statistics and_
_Financial Economics_, 22(3):445–474, 2012.


Ian Goodfellow, Jean Pouget-Abadie, Mehdi Mirza, Bing Xu, David Warde-Farley, Sherjil Ozair,
Aaron Courville, and Yoshua Bengio. Generative adversarial networks. _Communications of the_
_ACM_, 63(11):139–144, 2020.


Martin D Gould, Mason A Porter, Stacy Williams, Mark McDonald, Daniel J Fenn, and Sam D
Howison. Limit order books. _Quantitative Finance_, 13(11):1709–1742, 2013.


Min Hou, Chang Xu, Yang Liu, Weiqing Liu, Jiang Bian, Le Wu, Zhi Li, Enhong Chen, and TieYan Liu. Stock trend prediction with multi-granularity data: A contrastive learning approach with
adaptive fusion. In _Proceedings_ _of_ _the_ _30th_ _ACM_ _International_ _Conference_ _on_ _Information_ _&_
_Knowledge Management_, pp. 700–709, 2021.


Quzhe Huang, Mingxu Tao, Chen Zhang, Zhenwei An, Cong Jiang, Zhibin Chen, Zirui Wu, and
Yansong Feng. Lawyer llama technical report. _arXiv preprint arXiv:2305.15062_, 2023.


14


**756**

**757**


**758**

**759**

**760**

**761**

**762**

**763**


**764**

**765**

**766**

**767**

**768**

**769**


**770**

**771**

**772**

**773**

**774**


**775**

**776**

**777**

**778**

**779**

**780**


**781**

**782**

**783**

**784**

**785**

**786**


**787**

**788**

**789**

**790**

**791**

**792**


**793**

**794**

**795**

**796**

**797**


**798**

**799**

**800**

**801**

**802**

**803**


**804**

**805**

**806**

**807**

**808**

**809**



Under review as a conference paper at ICLR 2025


Hanna Hultin, Henrik Hult, Alexandre Proutiere, Samuel Samama, and Ala Tarighati. A generative
model of a limit order book using recurrent neural networks. _Quantitative_ _Finance_, 23(6):931–
958, 2023.


Jared Kaplan, Sam McCandlish, Tom Henighan, Tom B Brown, Benjamin Chess, Rewon Child,
Scott Gray, Alec Radford, Jeffrey Wu, and Dario Amodei. Scaling laws for neural language
models. _arXiv preprint arXiv:2001.08361_, 2020.


Raehyun Kim, Chan Ho So, Minbyul Jeong, Sanghoon Lee, Jinkyu Kim, and Jaewoo Kang.
Hats: A hierarchical graph attention network for stock movement prediction. _arXiv_ _preprint_
_arXiv:1908.07999_, 2019.


Junyi Li, Xintong Wang, Yaoyang Lin, Arunesh Sinha, and Michael Wellman. Generating realistic
stock market order streams. In _Proceedings_ _of_ _the_ _AAAI_ _Conference_ _on_ _Artificial_ _Intelligence_,
volume 34, pp. 727–734, 2020.


Zhuoyan Li, Hangxiao Zhu, Zhuoran Lu, and Ming Yin. Synthetic data generation with large language models for text classification: Potential and limitations. _arXiv preprint arXiv:2310.07849_,
2023.


Fabrizio Lillo, J. Doyne Farmer, and Rosario N. Mantegna. Master curve for price-impact function.
_Nature_, 421(6919):129–130, 2003. doi: 10.1038/421129a. URL [https://doi.org/10.](https://doi.org/10.1038/421129a)
[1038/421129a.](https://doi.org/10.1038/421129a)


Haotian Liu, Chunyuan Li, Qingyang Wu, and Yong Jae Lee. Visual instruction tuning, 2023a.


Xiao-Yang Liu, Guoxuan Wang, Hongyang Yang, and Daochen Zha. Fingpt: Democratizing
internet-scale data for financial large language models. _arXiv preprint arXiv:2307.10485_, 2023b.


Yixin Liu, Kai Zhang, Yuan Li, Zhiling Yan, Chujie Gao, Ruoxi Chen, Zhengqing Yuan, Yue Huang,
Hanchi Sun, Jianfeng Gao, et al. Sora: A review on background, technology, limitations, and
opportunities of large vision models. _arXiv preprint arXiv:2402.17177_, 2024.


I Loshchilov. Decoupled weight decay regularization. _arXiv preprint arXiv:1711.05101_, 2017.


Lisa K Meulbroek. An empirical analysis of illegal insider trading. _The Journal of Finance_, 47(5):
1661–1699, 1992.


Gr´egoire Mialon, Roberto Dess`ı, Maria Lomeli, Christoforos Nalmpantis, Ram Pasunuru, Roberta
Raileanu, Baptiste Rozi`ere, Timo Schick, Jane Dwivedi-Yu, Asli Celikyilmaz, et al. Augmented
language models: a survey. _arXiv preprint arXiv:2302.07842_, 2023.


Michael Moor, Oishi Banerjee, Zahra Shakeri Hossein Abad, Harlan M Krumholz, Jure Leskovec,
Eric J Topol, and Pranav Rajpurkar. Foundation models for generalist medical artificial intelligence. _Nature_, 616(7956):259–265, 2023.


Esteban Moro, Javier Vicente, Luis G. Moyano, Austin Gerig, J. Doyne Farmer, Gabriella Vaglica,
Fabrizio Lillo, and Rosario N. Mantegna. Market impact and trading profile of hidden orders
in stock markets. _Physical_ _Review_ _E_, 80(6), December 2009. ISSN 1550-2376. doi: 10.1103/
physreve.80.066102. [URL http://dx.doi.org/10.1103/PhysRevE.80.066102.](http://dx.doi.org/10.1103/PhysRevE.80.066102)


Ulrich A. Mller, Michel M. Dacorogna, Rakhal D. Dav, Richard B. Olsen, Olivier V. Pictet, and
Jacob E. von Weizscker. Volatilities of different time resolutions analyzing the dynamics of market components. _Journal_ _of_ _Empirical_ _Finance_, 4(2):213–239, 1997. ISSN 0927-5398. doi:
https://doi.org/10.1016/S0927-5398(97)00007-8. URL [https://www.sciencedirect.](https://www.sciencedirect.com/science/article/pii/S0927539897000078)
[com/science/article/pii/S0927539897000078.](https://www.sciencedirect.com/science/article/pii/S0927539897000078) High Frequency Data in Finance,
Part 1.


Peer Nagy, Sascha Frey, Silvia Sapora, Kang Li, Anisoara Calinescu, Stefan Zohren, and Jakob
Foerster. Generative ai for end-to-end limit order book modelling: A token-level autoregressive
generative model of message flow using a deep state space network. In _Proceedings of the Fourth_
_ACM International Conference on AI in Finance_, pp. 91–99, 2023.


15


**810**

**811**


**812**

**813**

**814**

**815**

**816**

**817**


**818**

**819**

**820**

**821**

**822**

**823**


**824**

**825**

**826**

**827**

**828**


**829**

**830**

**831**

**832**

**833**

**834**


**835**

**836**

**837**

**838**

**839**

**840**


**841**

**842**

**843**

**844**

**845**

**846**


**847**

**848**

**849**

**850**

**851**


**852**

**853**

**854**

**855**

**856**

**857**


**858**

**859**

**860**

**861**

**862**

**863**



Under review as a conference paper at ICLR 2025


Adamantios Ntakaris, Martin Magris, Juho Kanniainen, Moncef Gabbouj, and Alexandros Iosifidis. Benchmark dataset for midprice forecasting of limit order book data with machine learning methods. _Journal_ _of_ _Forecasting_, 37(8):852866, August 2018. ISSN 1099-131X. doi:
10.1002/for.2543. [URL http://dx.doi.org/10.1002/for.2543.](http://dx.doi.org/10.1002/for.2543)


T¯alis J Putnin¸ˇs. Market manipulation: A survey. _Journal of economic surveys_, 26(5):952–967, 2012.


Samyam Rajbhandari, Jeff Rasley, Olatunji Ruwase, and Yuxiong He. Zero: Memory optimizations
toward training trillion parameter models. In _SC20:_ _International_ _Conference_ _for_ _High_ _Perfor-_
_mance Computing, Networking, Storage and Analysis_, pp. 1–16. IEEE, 2020.


Syama Sundar Rangapuram, Matthias W Seeger, Jan Gasthaus, Lorenzo Stella, Yuyang Wang, and
Tim Januschowski. Deep state space models for time series forecasting. _Advances_ _in_ _neural_
_information processing systems_, 31, 2018.


Ethan Ratliff-Crain, Colin M. Van Oort, James Bagrow, Matthew T. K. Koehler, and Brian F. Tivnan.
Revisiting stylized facts for modern stock markets. In _2023_ _IEEE_ _International_ _Conference_ _on_
_Big Data (BigData)_, pp. 1814–1823, 2023. doi: 10.1109/BigData59044.2023.10386957.


Robin Rombach, Andreas Blattmann, Dominik Lorenz, Patrick Esser, and Bjrn Ommer. Highresolution image synthesis with latent diffusion models, 2021.


Robin Rombach, Andreas Blattmann, Dominik Lorenz, Patrick Esser, and Bj¨orn Ommer. Highresolution image synthesis with latent diffusion models. In _Proceedings of the IEEE/CVF confer-_
_ence on computer vision and pattern recognition_, pp. 10684–10695, 2022.


Christoph Schuhmann, Richard Vencu, Romain Beaumont, Robert Kaczmarczyk, Clayton Mullis,
Aarush Katta, Theo Coombes, Jenia Jitsev, and Aran Komatsuzaki. Laion-400m: Open dataset of
clip-filtered 400 million image-text pairs. _arXiv preprint arXiv:2111.02114_, 2021.


Vaibhav Sherkar and Rituparna Sen. Study of stylized facts in stock market data, 2023. URL
[https://arxiv.org/abs/2310.00753.](https://arxiv.org/abs/2310.00753)


Namid R. Stillman, Rory Baggott, Justin Lyon, Jianfei Zhang, Dingqiu Zhu, Tao Chen, and Perukrishnen Vytelingum. Deep calibration of market simulations using neural density estimators and
embedding networks, 2023. [URL https://arxiv.org/abs/2311.11913.](https://arxiv.org/abs/2311.11913)


Richard S. Sutton and Andrew G. Barto. _Reinforcement_ _Learning:_ _An_ _Introduction_ . MIT Press,
second edition, 2018.


Shuntaro Takahashi, Yu Chen, and Kumiko Tanaka-Ishii. Modeling financial time-series with generative adversarial networks. _Physica A: Statistical Mechanics and its Applications_, 527:121261,
2019a. ISSN 0378-4371. doi: https://doi.org/10.1016/j.physa.2019.121261. URL [https:](https://www.sciencedirect.com/science/article/pii/S0378437119307277)
[//www.sciencedirect.com/science/article/pii/S0378437119307277.](https://www.sciencedirect.com/science/article/pii/S0378437119307277)


Shuntaro Takahashi, Yu Chen, and Kumiko Tanaka-Ishii. Modeling financial time-series with generative adversarial networks. _Physica A: Statistical Mechanics and its Applications_, 527:121261,
2019b.


Hugo Touvron, Louis Martin, Kevin Stone, Peter Albert, Amjad Almahairi, Yasmine Babaei, Nikolay Bashlykov, Soumya Batra, Prajjwal Bhargava, Shruti Bhosale, et al. Llama 2: Open foundation and fine-tuned chat models. _arXiv preprint arXiv:2307.09288_, 2023.


Svitlana Vyetrenko, David Byrd, Nick Petosa, Mahmoud Mahfouz, Danial Dervovic, Manuela
Veloso, and Tucker Balch. Get real: Realism metrics for robust limit order book market simulations. In _Proceedings_ _of_ _the_ _First_ _ACM_ _International_ _Conference_ _on_ _AI_ _in_ _Finance_, pp. 1–8,
2020.


Pedram Babaei William Todt, Ramtin Babaei. Fin-llama: Efficient finetuning of quantized llms for
finance. [https://github.com/Bavest/fin-llama, 2023.](https://github.com/Bavest/fin-llama)


Shijie Wu, Ozan Irsoy, Steven Lu, Vadim Dabravolski, Mark Dredze, Sebastian Gehrmann, Prabhanjan Kambadur, David Rosenberg, and Gideon Mann. Bloomberggpt: A large language model
for finance. _arXiv preprint arXiv:2303.17564_, 2023.


16


**864**

**865**


**866**

**867**

**868**

**869**

**870**

**871**


**872**

**873**

**874**

**875**

**876**

**877**


**878**

**879**

**880**

**881**

**882**


**883**

**884**

**885**

**886**

**887**

**888**


**889**

**890**

**891**

**892**

**893**

**894**


**895**

**896**

**897**

**898**

**899**

**900**


**901**

**902**

**903**

**904**

**905**


**906**

**907**

**908**

**909**

**910**

**911**


**912**

**913**

**914**

**915**

**916**

**917**



Under review as a conference paper at ICLR 2025


Qianqian Xie, Weiguang Han, Xiao Zhang, Yanzhao Lai, Min Peng, Alejandro Lopez-Lira, and
Jimin Huang. Pixiu: A comprehensive benchmark, instruction dataset and large language model
for finance. _Advances in Neural Information Processing Systems_, 36, 2024a.


Qianqian Xie, Dong Li, Mengxi Xiao, Zihao Jiang, Ruoyu Xiang, Xiao Zhang, Zhengyu Chen,
Yueru He, Weiguang Han, Yuzhe Yang, et al. Open-finllms: Open multimodal large language
models for financial applications. _arXiv preprint arXiv:2408.11878_, 2024b.


Mengjiao Yang, Yilun Du, Kamyar Ghasemipour, Jonathan Tompson, Dale Schuurmans, and Pieter
Abbeel. Learning interactive real-world simulators. _arXiv preprint arXiv:2310.06114_, 2023a.


Sherry Yang, Jacob Walker, Jack Parker-Holder, Yilun Du, Jake Bruce, Andre Barreto, Pieter
Abbeel, and Dale Schuurmans. Video as the new language for real-world decision making. _arXiv_
_preprint arXiv:2402.17139_, 2024.


Yi Yang, Yixuan Tang, and Kar Yan Tam. Investlm: A large language model for investment using
financial domain instruction tuning. _arXiv preprint arXiv:2309.13064_, 2023b.


Elia Zarinelli, Michele Treccani, J Doyne Farmer, and Fabrizio Lillo. Beyond the square root:
Evidence for logarithmic dependence of market impact on size and participation rate. _Market_
_Microstructure and Liquidity_, 1(02):1550004, 2015.


Xiaohua Zhai, Alexander Kolesnikov, Neil Houlsby, and Lucas Beyer. Scaling vision transformers.
In _Proceedings_ _of_ _the_ _IEEE/CVF_ _conference_ _on_ _computer_ _vision_ _and_ _pattern_ _recognition_, pp.
12104–12113, 2022.


Boyu Zhang, Hongyang Yang, and Xiao-Yang Liu. Instruct-fingpt: Financial sentiment analysis by
instruction tuning of general-purpose large language models. _arXiv preprint arXiv:2306.12659_,
2023.


Wentao Zhang, Yilei Zhao, Shuo Sun, Jie Ying, Yonggang Xie, Zitao Song, Xinrun Wang, and
Bo An. Reinforcement learning with maskable stock representation for portfolio management in
customizable stock pools. In _Proceedings_ _of_ _the_ _ACM_ _on_ _Web_ _Conference_ _2024_, pp. 187–198,
2024.


Xuanyu Zhang and Qing Yang. Xuanyuan 2.0: A large chinese financial chat model with hundreds
of billions parameters. In _Proceedings of the 32nd ACM international conference on information_
_and knowledge management_, pp. 4435–4439, 2023.


Zihao Zhang, Stefan Zohren, and Stephen Roberts. Deeplob: Deep convolutional neural networks
for limit order books. _IEEE_ _Transactions_ _on_ _Signal_ _Processing_, 67(11):30013012, June 2019.
ISSN 1941-0476. doi: 10.1109/tsp.2019.2907260. [URL http://dx.doi.org/10.1109/](http://dx.doi.org/10.1109/TSP.2019.2907260)
[TSP.2019.2907260.](http://dx.doi.org/10.1109/TSP.2019.2907260)


Zheng Zhu, Xiaofeng Wang, Wangbo Zhao, Chen Min, Nianchen Deng, Min Dou, Yuqi Wang,
Botian Shi, Kai Wang, Chi Zhang, et al. Is sora a world simulator? a comprehensive survey on
general world models and beyond. _arXiv preprint arXiv:2405.03520_, 2024.


17


**918**

**919**


**920**

**921**

**922**

**923**

**924**

**925**


**926**

**927**

**928**

**929**

**930**

**931**


**932**

**933**

**934**

**935**

**936**


**937**

**938**

**939**

**940**

**941**

**942**


**943**

**944**

**945**

**946**

**947**

**948**


**949**

**950**

**951**

**952**

**953**

**954**


**955**

**956**

**957**

**958**

**959**


**960**

**961**

**962**

**963**

**964**

**965**


**966**

**967**

**968**

**969**

**970**

**971**



Under review as a conference paper at ICLR 2025


A RELATED WORKS


**Financial Market Simulation.** Before the recent surge in generative foundation models, researchers
in the finance domain had already recognized the immense potential of powerful market simulations.
Early approaches often utilized agent-based modeling, particularly multi-agent systems, to simulate
order-driven markets (Chiarella et al., 2009; Byrd et al., 2020; Amrouni et al., 2021).


With the advancements in deep learning technologies, several works have emerged that adopt the
world model paradigm to simulate Limit Order Book (LOB) markets (Takahashi et al., 2019b; Li
et al., 2020; Coletta et al., 2021; 2022; 2023). These studies primarily leveraged Generative Adversarial Networks (GANs) (Goodfellow et al., 2020) to model the distribution of LOB time series.


Recently, some generators have begun incorporating market micro-structure data, such as those
presented in (Hultin et al., 2023; Nagy et al., 2023). Among these, Nagy et al. (2023) is most related
to our work, particularly regarding the order model. They employ an auto-regressive model based
on a Deep State Space Network (Rangapuram et al., 2018) to generate LOB and message flows.
However, their focus is primarily on LOB modeling. While they demonstrate some realistic stylized
facts of the generated sequences, they do not evaluate the model’s capability to address downstream
financial tasks.


Our work aims to push the boundaries of financial market simulation by introducing an innovative
approach that goes beyond generating realistic order flows. We introduce MarS, a pioneering financial market simulation engine driven by the Large Market Model (LMM). Designed to meet the
specific demands of the financial sector, MarS excels in modeling the market impact of orders and
achieving high levels of controllability and realism. By framing various financial market tasks as
conditional trading order generation problems, we demonstrate MarS’s transformative potential and
practical applications in real-world financial markets.


**Foundation Models.** Foundation models are trained on broad datasets and can be adapted to a wide
range of downstream tasks. The term was popularized by the Stanford Institute (Bommasani et al.,
2021). The release of GPT-3 (Brown, 2020) showcased the powerful benefits of training large autoregressive language models (LLMs) on extensive corpora (Abdin et al., 2024; Achiam et al., 2023;
Dubey et al., 2024).


In addition, numerous foundation models have emerged in the fields of computer vision (CV) and
multimodal areas (Rombach et al., 2021; Brooks et al., 2024; Liu et al., 2023a). Recently, realworld simulators and industry-specific large models have become popular research topics in this
field. Real-world simulators aim to achieve real-world simulation through the unified goal of video
generation, addressing various tasks in fields such as autonomous driving, robotics, and gaming
(Liu et al., 2024; Zhu et al., 2024; Yang et al., 2024; 2023a). However, they primarily focus on
simulating the physical world. The order-driven financial market is an exemplary virtual world with
different operating principles. To the best of our knowledge, we are the first to build a financial
world simulator.


Industry-specific large models primarily focus on fields such as biomedicine (Moor et al., 2023), law
(Huang et al., 2023), and finance. In the financial domain, most large models are Financial LLMs,
which either pre-train LLMs on financial corpora (Wu et al., 2023; Zhang & Yang, 2023) or finetune them (Xie et al., 2024a; Zhang et al., 2023; William Todt, 2023; Yang et al., 2023b) to tackle
financial NLP tasks or multimodal tasks (Bhatia et al., 2024; Xie et al., 2024b), including sentiment
analysis, text classification, and question answering.


Beyond text, there is an even larger and more information-rich corpus in the financial world: trading
orders. We propose a Large Market Model (LMM), which, for the first time, reveals the scaling law
on trading orders. We take the first step toward building a generative foundation model as a world
model for the financial market. We believe that, with MarS as the shovel, the extensive order-level
data undoubtedly represent a significant gold mine.


18


**972**

**973**


**974**

**975**

**976**

**977**

**978**

**979**


**980**

**981**

**982**

**983**

**984**

**985**


**986**

**987**

**988**

**989**

**990**


**991**

**992**

**993**

**994**

**995**

**996**


**997**

**998**

**999**

**1000**

**1001**

**1002**


**1003**

**1004**

**1005**

**1006**

**1007**

**1008**


**1009**

**1010**

**1011**

**1012**

**1013**


**1014**

**1015**

**1016**

**1017**

**1018**

**1019**


**1020**

**1021**

**1022**

**1023**

**1024**

**1025**



Under review as a conference paper at ICLR 2025


B ORDER SEQUENCE MODELING.


B.1 INTRODUCTION


The order model for financial markets shares similarities with the Language Model (LM) for text
in several respects. Both models strive to predict the subsequent event, whether it be a token in a
text corpus or a trade order in financial markets. Additionally, the datasets for both are typically
extensive, facilitating the training of robust models. Furthermore, data in both domains can be
generated autoregressively.


Nevertheless, substantial differences also exist between the two fields. Each order in the financial
market is associated with a complex set of market dynamics, including the Limit Order Book (LOB),
transactions, and potentially market news in natural language. Consequently, each order may be
influenced by a broader array of information beyond the order stream itself. It is therefore imperative
to encode this rich information compactly while preserving the autoregressive generation paradigm.
Moreover, the financial market operates on a rule-based order matching system, which processes
orders and generates new states, such as transactions and the updated LOB. This necessitates an
additional order matching step to obtain accurate market states.


**Order Model**



Figure 11: The framework of the order model. The model is trained on the order stream and the corresponding LOB information. It is autoregressive, generating the next order based on the preceding
order and LOB information. The order matching step is employed to produce the new LOB state.


B.2 APPROACH


B.2.1 TOKENIZATION


The objective of tokenization is to make it compact and efficient for encoding and decoding while
retaining the majority of useful information. To this end, we opt to encode each order and its
antecedent LOB as a single token. The LOB information functions analogously to an image in a
text, offering additional context for the order. The tokenization procedure for the _i_ _[th]_ order is as
follows:
_Embi_ = emb( _orderi_ ) + linear ~~p~~ roj( _LOBi_ [volumes] ) + emb( _LOBi_ [mid] ~~[p]~~ [rice] ) (4)


19



**54321**



**54321**



**12345**



**1** **23** **45**



**12345**







𝐎𝐈𝐃𝐭+𝟏 𝐎𝐈𝐃𝐭+𝟐 𝐎𝐈𝐃𝐭+𝟑


**Causal Transformer**


**Order/LOB Tokenizer**



**5432** **1**
















**1026**

**1027**


**1028**

**1029**

**1030**

**1031**

**1032**

**1033**


**1034**

**1035**

**1036**

**1037**

**1038**

**1039**


**1040**

**1041**

**1042**

**1043**

**1044**


**1045**

**1046**

**1047**

**1048**

**1049**

**1050**


**1051**

**1052**

**1053**

**1054**

**1055**

**1056**


**1057**

**1058**

**1059**

**1060**

**1061**

**1062**


**1063**

**1064**

**1065**

**1066**

**1067**


**1068**

**1069**

**1070**

**1071**

**1072**

**1073**


**1074**

**1075**

**1076**

**1077**

**1078**

**1079**



Under review as a conference paper at ICLR 2025


Here, _orderi_ denotes an index indicating its position in the tuple (type, price, volume, interval), with
type being one of [“Ask”, “Bid”, “Cancel”]. Both price and volume are discretized into the range [0,
32), and interval into [0, 16). An index within the range [0, 49152) can uniquely identify a position
for the (type, price, volume, interval) tuple. _LOBi_ [volumes] represents the 10-level volumes for asks
and bids in the LOB, also discretized into [0, 32). The _LOBi_ [mid] [price] is the mid-price of the LOB,
expressed as the number of price tick changes since market opening.


This formula computes the embedding for the _i_ _[th]_ token, which is a composite of the order, the linear
projection of the LOB volumes, and the embedding of the LOB mid-price.


While the input token includes LOB information, it is impractical and unnecessary to predict the
resultant LOB during the decoding process. Instead, the new LOB information can be derived using
a standard order matching algorithm, based on the preceding LOB and the newly generated order.
Given this consideration, we only output the order index and conduct an order matching during
simulation to obtain the subsequent accurate LOB state, as depicted in Fig. 11.


B.3 DATA AND MODEL TRAINING


Our dataset encompasses the top 500 liquidity stocks in the Chinese stock market, covering the
period from 2017 to 2023 and comprising 16 billion order tokens. Our model architecture is based
on LLaMA2 (Touvron et al., 2023), and AdamW optimizer (Loshchilov, 2017) is employed in all
experiments. We utilize fp16 precision with DeepSpeed ZERO stage 2 (Rajbhandari et al., 2020) to
optimize memory usage. The sequence length is set at 1024, with a batch size of 4096, equating to
4 million tokens per optimization step.


The inclusion of LOB information in the tokenization process is compared to determine its impact
on training performance. The evidence suggests that integrating the LOB information contributes to
an enhanced training curve, as shown in Fig. 12.


|Col1|Tokenization Method<br>Order<br>Order + LOB|Col3|Col4|Col5|Col6|Col7|Col8|Col9|
|---|---|---|---|---|---|---|---|---|
||||||||||
||||||||||
||||||||||
||||||||||
||||||||||



Figure 12: Tokenization of the Order Model. A comparative analysis of the tokenization process
with and without the Limit Order Book (LOB) information. Incorporating precise LOB information
leads to an improved training curve.


Furthermore, we examine the effects of varying data and model sizes on training performance. The
data suggest that augmenting both data and model sizes correlates with improved outcomes, as
shown in Fig. 3a.


C ORDER-BATCH SEQUENCE MODELING


C.1 INTRODUCTION


In this section, we introduce the order-batch model. Different from the order model, which focuses
on individual orders, the order-batch model concentrates on batches of orders to model structured
patterns of dynamic market behavior over time intervals. We innovatively organize batches of orders
into an RGB image format, which are then discretized into tokens for autoregressive training, aimed
at generating order-batch sequences.


20



7.8


7.6


7.4


7.2


7.0



Number of Training Tokens 1e10


**1080**

**1081**


**1082**

**1083**

**1084**

**1085**

**1086**

**1087**


**1088**

**1089**

**1090**

**1091**

**1092**

**1093**


**1094**

**1095**

**1096**

**1097**

**1098**


**1099**

**1100**

**1101**

**1102**

**1103**

**1104**


**1105**

**1106**

**1107**

**1108**

**1109**

**1110**


**1111**

**1112**

**1113**

**1114**

**1115**

**1116**


**1117**

**1118**

**1119**

**1120**

**1121**


**1122**

**1123**

**1124**

**1125**

**1126**

**1127**


**1128**

**1129**

**1130**

**1131**

**1132**

**1133**



Under review as a conference paper at ICLR 2025


Figure 13: The intraday distribution of the average number of orders per minute.


As we know, financial markets are comprised of diverse participants, each with a unique set of
information and trading frequency. Even in the domain of high-frequency trading, there are nuances:
some traders pay close attention to each order, while others may focus on signals in fixed time
intervals to guide their trading decisions. Through data analysis, we can easily discern the traces
by the latter type of high-frequency traders. We counted the number of orders per minute for each
stock in our dataset introduced in Sec. B.3 to create a chart shown in Fig. 13. From this chart,
we can observe the following patterns: 1. The intraday order distribution is U-shaped. 2. There
is a significant increase in order number at the market open in the morning and after the lunch
break. 3. There are spikes in order numbers nearly every 10 minutes, suggesting a periodic pattern.
With the above observations, we find that the distribution of orders within fixed intervals adheres to
consistent patterns, and such patterns can also be captured by the model. So we attempt to model
these structured patterns of dynamic market behavior.


Besides, modeling batches of orders facilitates the generation of specific financial scenarios. If
generating a specified market scenario through prompts, there will be significant information asymmetry between the brief text of prompts and the thousands of orders in an order flow. Imposing
such a signal directly onto each order through an order model is clearly intractable. Therefore, we
need an order-batch model to act as a bridge between the prompt and the order model to facilitate
this transition. The order-batch model corresponds to prompts by first generating minute-level order
batches, and then decoding them into an order flow in conjunction with the order model.


C.2 APPROACH


As observed in Fig. 13, orders within fixed time intervals vary in numbers, and these variations are
significant at different time throughout the day. In light of this, learning representations from the
sequences after padding is clearly not a sensible approach. To better represent orders of variable
numbers, we creatively convert the orders into an RGB image format. This approach allows us not
only to “visualize” the changes in orders over a period of time but also to draw on the experience
of the image generation field, transforming the problem of order-batch generation into one of image
generation. We present the framework of the order-batch model in Fig. 14.


C.3 ORDER IMAGE CONVERTER


Learning representations directly from order sequences at fixed time intervals is not an effective
and practical approach. On the one hand, stocks with different levels of liquidity have significantly
different order numbers. On the other hand, for the same stock, the distribution of order numbers
throughout the day can be extremely uneven (with a higher concentration during the opening and
closing periods, and sparser distribution during the mid-day). Within fixed time intervals (e.g.,
minute-level), we care more about the aggregate characteristics of the order sequence rather than the
details of individual orders. Under the assumption that the distribution of orders remains relatively
stable over short periods, we can disregard the precise arrival times of individual orders and structure
the order sequences in a cross-sectional view.


21


**1134**

**1135**


**1136**

**1137**

**1138**

**1139**

**1140**

**1141**


**1142**

**1143**

**1144**

**1145**

**1146**

**1147**


**1148**

**1149**

**1150**

**1151**

**1152**


**1153**

**1154**

**1155**

**1156**

**1157**

**1158**


**1159**

**1160**

**1161**

**1162**

**1163**

**1164**


**1165**

**1166**

**1167**

**1168**

**1169**

**1170**


**1171**

**1172**

**1173**

**1174**

**1175**


**1176**

**1177**

**1178**

**1179**

**1180**

**1181**


**1182**

**1183**

**1184**

**1185**

**1186**

**1187**



Under review as a conference paper at ICLR 2025


|C|Ord Cancel|
|---|---|
|**Bid**<br>**C**<br>**Ask**|**ancel**|
|**Bid**<br>**C**<br>**Ask**||
|**Bid**<br>**C**<br>**Ask**||
|||
|||


|C<br>Bid<br>Ask|ancel|
|---|---|
|**Bid**<br>**C**<br>**Ask**||
|**Bid**<br>**C**<br>**Ask**||
|||
|||



Figure 15: Order image converter. The orders we use contain three attributes: type, price slot, and
volume slot. There are three types of orders: Bid, Ask, and Cancel. The price slot refers to the slot
of the actual order price in relation to the mid-price at the beginning of the interval. The volume
slot is obtained by dividing the actual volume of the order into bins. The number of orders with the
same attributes is represented by the pixel values, where a higher pixel value at a certain position
indicates a higher number of orders with the same attributes.


22



**Fine-tuned**
**Image Decoder**





𝟏



𝟐𝐭 𝐭𝐤𝐭



𝟏𝐭$𝟏 𝐭𝐤𝟐𝐭$𝟏



𝟐𝐭$𝟏 𝐭𝐤𝟑𝐭$𝟏



𝟑𝐭$𝟏 𝐭𝐤𝟏𝐭$𝟐



𝐭𝐤𝟐𝐭



𝟑𝐭 𝐭𝐤𝟏𝐭$𝟏





𝐊



𝟏𝐭 𝐭𝐤𝐭



𝐭𝐤𝟏𝐭$𝟏



𝟏𝐭$𝟏 𝐭𝐤𝟐𝐭$𝟏



𝐭𝐤𝐭



𝟐𝐭 𝐭𝐤𝐭



𝐊



𝟐𝐭$𝟏 𝐭𝐤𝐊𝐭$𝟏



𝐦𝐢𝐧𝐮𝐭𝐞𝐭





𝐦𝐢𝐧𝐮𝐭𝐞𝐭$𝟏



Figure 14: The framework of order-batch model. We employ a two-stage training approach: in
Stage 1, we leverage a fine-tuned image encoder to transform “order images” from minute-level
orders into tokens; in Stage 2, we train an autoregressive transformer model to learn the distribution
of the tokens. Order images are decoded from tokens via fine-tuned image decoder.


In practice, we convert one order-batch into an RGB image format. We refer to such images as
“order images”. The working process of the order image converter is demonstrated in Fig. 15.
**Technical Details** : _C_ denotes the categories of orders, or the channels of an order image. _W_ and _H_
represent the width and height of the order image, indicating the number of price and volume slots,
respectively. _V_ signifies the count of identical orders, reflected as the pixel value. In our work, we
set _C_ = 3 _, H_ = _W_ = 32 _, V_ _∈_ [0 _,_ 100].





**Max**


**Mid**


**Min**



**Bid**



**Ask**











**Price Slot**



**Price Slot**





**Price Slot**


**1188**

**1189**


**1190**

**1191**

**1192**

**1193**

**1194**

**1195**


**1196**

**1197**

**1198**

**1199**

**1200**

**1201**


**1202**

**1203**

**1204**

**1205**

**1206**


**1207**

**1208**

**1209**

**1210**

**1211**

**1212**


**1213**

**1214**

**1215**

**1216**

**1217**

**1218**


**1219**

**1220**

**1221**

**1222**

**1223**

**1224**


**1225**

**1226**

**1227**

**1228**

**1229**


**1230**

**1231**

**1232**

**1233**

**1234**

**1235**


**1236**

**1237**

**1238**

**1239**

**1240**

**1241**



Under review as a conference paper at ICLR 2025


The order image converter allows us not only to “depict” the changes in orders over a past period
but also to leverage experience from image generation. We can utilize a pre-trained visual encoder
to obtain an order-batch embedding.


C.3.1 STAGE 1: ORDER IMAGE TOKENIZER


After converting the order-batch into an order image, we transform the problem of modelling orderbatches into an image generation problem. In this way, we can follow the successful path of Large
Vision Models (Bai et al., 2024), adopting a two-stage approach to generate intraday order-batch
sequences. The first stage of the image generation task typically involves using a pre-trained image
tokenizer to discretize individual images into a series of tokens.


Specifically, we leverage VQGAN (Esser et al., 2021) to accomplish the conversion of order images into discrete tokens, which learns a convolutional model consisting of an encoder and decoder,
allowing them to represent images using codes from a learned, discrete codebook. In particular, VQGAN incorporates a discriminator and perceptual loss to ensure high quality during the compression
process. In our implementation, both the encoder and decoder utilized the original structure. **Tech-**
**nical Details** : We use a pre-trained VQGAN from LDM (Rombach et al., 2022), which was trained
on the LAION-400M database (Schuhmann et al., 2021). We adopt the configuration and weights
from one of the models in the LDM model zoo, with a down-sampling factor _f_ = 4, vocabulary size
_Z_ = 8192, and codebook dimension _d_ = 3. This means that an RGB order image of size 32 _×_ 32
with 3 channels is discretized into 8 _×_ 8 = 64 tokens at this stage, each with a dimension of 3. In
practice, we find that the off-the-shelf model parameters did not represent order images well, so we
fine-tune it using order images to achieve a transition from natural images to order images.


C.3.2 STAGE 2: ORDER-BATCH SEQUENCE MODELLING


After the order image tokenizer converts individual order images into a sequence of discrete tokens,
we concatenate these tokens to form an order-batch sequence. In Stage 2, we train an autoregressive
transformer to learn the distribution of these tokens. It learns not only the distribution of tokens that
make up an order image but also the distribution of tokens between order images. Consequently, we
can generate intraday order-batch sequences.


Specifically, we employ a language model for next token prediction training. **Technical Details** : We
use LLaMA2(Touvron et al., 2023) as the implementation framework for our autoregressive transformer. We calculate the cross-entropy loss between prediction logits and labels. Implementation
Details: The token length for LLaMA2 is 4096, and we concatenate 16 order-batches to form an
order-batch sequence, with a total length of 16 _×_ 64 = 1024, which is well below the length limit.


D ENSEMBLE MODEL


D.1 INTRODUCTION


In sections above, we introduced the order model and order batch model, each with its advantages:


    - **Order** **model** : This model generates orders individually and is designed to reflect shortterm market impacts rapidly. However, it lacks the ability to generate target scenarios over
the long run.


    - **Order-batch** **model** : This model generates order channels (We do not distinguish ’order
channels’ and ’order images’ in this paper), representing the macro behavior of the market,
and can be used to follow control signals. However, it lacks the ability for interactive
market simulation.


In this section, we introduce the ensemble model, which aims to balance interaction and controllability in market simulation.


23


**1242**

**1243**


**1244**

**1245**

**1246**

**1247**

**1248**

**1249**


**1250**

**1251**

**1252**

**1253**

**1254**

**1255**


**1256**

**1257**

**1258**

**1259**

**1260**


**1261**

**1262**

**1263**

**1264**

**1265**

**1266**


**1267**

**1268**

**1269**

**1270**

**1271**

**1272**


**1273**

**1274**

**1275**

**1276**

**1277**

**1278**


**1279**

**1280**

**1281**

**1282**

**1283**


**1284**

**1285**

**1286**

**1287**

**1288**

**1289**


**1290**

**1291**

**1292**

**1293**

**1294**

**1295**



Under review as a conference paper at ICLR 2025


D.2 APPROACH


The order channels output by the order-batch model contain rich information about macro trends in
the financial market. It would be advantageous if the order model could utilize this information to
generate orders.


We propose using an ensemble model that takes the order logits and order channels as input and
generates the next order, as illustrated in Fig. 4


In our experiment, we found it challenging to train the ensemble model directly from order channels
predicted by the order-batch model. The reason is that the order channels predicted by the orderbatch model still exhibit high variance and may not accurately reflect replay order data. Realizing
this, during training, we use the order channels directly from replay data, which provides an accurate
description of the market trend. In this way, our ensemble model learns how to condition on the
order channels to generate the next order. During simulation, we use order channels predicted by
the order-batch model to generate orders, which provide more flexibility for controllable simulation.


The ensemble model is a simple cross-attention model that takes the order logits and real order
channels as input and generates the next order. The loss advantage over the order model is used as
the training metric. Fig. 16 shows the training process of the ensemble model. We can see that with
this design, the ensemble model can improve its performance on order generation, demonstrating its
conditioning on order batch data.


0.0


0.2


0.4


0.6


0.8

|Col1|Ensemble Model Training|Col3|Col4|Col5|Col6|Col7|
|---|---|---|---|---|---|---|
||||||||
||||||||
||||||||
||||||||
||||||||
||||||||



# Training States 1e7


Figure 16: Training process of the ensemble model. The x-axis represents the number of training
samples, and the y-axis represents the loss advantage over the order model.


E FINE-GRAINED SIGNAL GENERATION INTERFACE


We introduce an interface that maps vague descriptions to fine-grained control signals using LLMbased historical market record retrieval. This guides our order batch model, ensuring simulations
reflect realistic market patterns and user-defined scenarios. The process involves three main steps:


    - **Example Provision and Code Generation** : Provide a sample of minute-level return history to GPT-4o mini and prompt it to generate code that retrieves historical periods matching specified scenarios.

    - **Scenario Filtering** : Apply the generated code on the entire dataset to identify more minutelevel trajectories for each scenario.

    - **Scenario Generation** : Use the identified minute-level trajectories to guide the generation
of order batches according to principles outlined in Sec. 2.2, alongside the ensemble model
for scenario generation.


The minute-level return history is stored in a CSV file, formatted as shown in Table 2:


We demonstrate market simulations for scenarios including “Sharp Drop”, “Sharp Rise”, and “Trend
Reversal”. Below, we detail the process when TEXT ~~D~~ ES is “Sharp Drop”. First, a prompt is


24


**1296**

**1297**


**1298**

**1299**

**1300**

**1301**

**1302**

**1303**


**1304**

**1305**

**1306**

**1307**

**1308**

**1309**


**1310**

**1311**

**1312**

**1313**

**1314**


**1315**

**1316**

**1317**

**1318**

**1319**

**1320**


**1321**

**1322**

**1323**

**1324**

**1325**

**1326**


**1327**

**1328**

**1329**

**1330**

**1331**

**1332**


**1333**

**1334**

**1335**

**1336**

**1337**


**1338**

**1339**

**1340**

**1341**

**1342**

**1343**


**1344**

**1345**

**1346**

**1347**

**1348**

**1349**



Under review as a conference paper at ICLR 2025


**date** **minute** **SZ000001** **SZ000002** **...** **SZ003043** **SZ003816**
2023-01-03 09:31:00 -0.001520 0.001664 ... -0.005541 0.000000
2023-01-03 09:32:00 0.000761 0.000000 ... -0.004261 0.000000
... ... ... ... ... ... ...
2023-03-31 14:55:00 0.000797 0.000657 ... 0.000164 0.000000
2023-03-31 14:56:00 0.000000 -0.000656 ... 0.000164 0.000000


Table 2: Format of minute-level return history


provided to GPT-4o mini, which generates code to filter typical cases for the “Sharp Drop” scenario.
The prompt is shown in Table 3.


**Scenario:** **Sharp Drop**
**Data Description** : The input data is in CSV format with the following information.


        - The first column “date” represents the trading date.


        - The second column “minute” represents the time.


        - Each subsequent column corresponds to an instrument, with the value in each
cell representing the return of the instrument for the given minute compared
to the previous minute.


**Output** **Description** : Please identify and provide 30 samples where a stock drops
sharply within a 25-minute window. For each sample, include the following details:


1. Date.


2. Start and end minute of the 25-minute window.


3. Stock code.


4. The return of the 25-minute interval.


**Constraints on Output:**


1. Ensure that the 25-minute cases do not contain duplicate stock codes and
datetimes. Each sample should be selected from different trading days.


2. Ensure that each 25-minute interval is within the same trading day.


3. You can use groupby(’datetime’).rolling(25).sum() to convert 1-minute-level returns to 25-minute-level returns.


4. The begin and end times of the 25-minute interval should be within trading
hours, e.g., 9:30 AM - 11:30 AM and 1 PM - 3 PM.


Table 3: Prompt used for generating code in the “Sharp Drop” scenario


The code generated by GPT-4o mini, shown in Fig. 17, is then used to filter the “Sharp Drop”
scenario and applied to the entire dataset to identify additional cases.


Once the minute-level return trajectory is retrieved, it is used to guide the generation of order batches
along with the ensemble model for scenario generation. Detailed descriptions and visualizations of
the three scenarios are provided:


    - **Sharp Drops** : Simulating sharp declines to understand market reactions to negative events,
assess risk management strategies, and evaluate market liquidity.


    - **Sharp Rises** : Simulating sharp increases to capture market behavior during positive events,
allowing traders to test profit-taking strategies and analyze upward trends.


    - **Trend Reversals** : Simulating trend reversals to identify signals for entry or exit points and
understand market reactions to trend shifts.


25


**1350**

**1351**


**1352**

**1353**

**1354**

**1355**

**1356**

**1357**


**1358**

**1359**

**1360**

**1361**

**1362**

**1363**


**1364**

**1365**

**1366**

**1367**

**1368**


**1369**

**1370**

**1371**

**1372**

**1373**

**1374**


**1375**

**1376**

**1377**

**1378**

**1379**

**1380**


**1381**

**1382**

**1383**

**1384**

**1385**

**1386**


**1387**

**1388**

**1389**

**1390**

**1391**


**1392**

**1393**

**1394**

**1395**

**1396**

**1397**


**1398**

**1399**

**1400**

**1401**

**1402**

**1403**



Under review as a conference paper at ICLR 2025


**import** pandas as pd
**import** datetime


file_path = ’minute_return_data_all.csv’
data = pd.read_csv(file_path)


data[’datetime’] = pd.to_datetime(data[’date’] + ’ ’ + data[’minute’])
data.set_index(’datetime’, inplace=True)
data.drop(columns=[’date’, ’minute’], inplace=True)


_#_ _Apply_ _groupby_ _on_ _each_ _stock_ _code_ _to_ _calculate_ _rolling_ _25-minute_ _sum_
rolling_returns = data.groupby(data.index.date).rolling(25).sum().

reset_index(level=0, drop=True)


_#_ _Filter_ _based_ _on_ _sharp_ _drops_ _within_ _the_ _25-minute_ _windows_ _(arbitrary_

_threshold_ _for_ _"sharp_ _drop")_
threshold = -0.05 _#_ _Example_ _threshold_ _for_ _a_ _sharp_ _drop_ _over_ _25_ _minutes_
sample_nums = 30
sharp_drops = rolling_returns[rolling_returns <= threshold].dropna(how=’

all’)


_#_ _Reset_ _the_ _index_ _to_ _retain_ _datetime_ _information_
sharp_drops = sharp_drops.reset_index()


_#_ _Extract_ _30_ _samples_ _ensuring_ _unique_ _stock_ _codes_ _and_ _trading_ _dates,_ _with_

_varied_ _start_ _times_
result = []
seen_dates = set()
seen_stocks = set()
seen_start_times = set()


**for** _, row **in** sharp_drops.iterrows():

date = row[’datetime’].date()
start_time = (row[’datetime’]   - pd.Timedelta(minutes=24)).time()
**if** start_time <= datetime.time(9, 30) **or** start_time <= datetime.time(

13, 00) **and** start_time                     datetime.time(11, 30):
**continue**
stock_drops = row.drop(labels=[’datetime’])


**if** start_time **not** **in** seen_start_times:

**for** stock_code, value **in** stock_drops.items():

**if** **not** pd.isna(value) **and** date **not** **in** seen_dates **and**

stock_code **not** **in**
seen_stocks:
result.append( **{**

’Date’: date,
’Start Time’: start_time,
’End Time’: row[’datetime’].time(),
’Stock Code’: stock_code,
’25-Minute Return’: value
**}** )
seen_dates.add(date)
seen_stocks.add(stock_code)
seen_start_times.add(start_time)
**if** len(result) >= sample_nums:

**break**
**if** len(result) >= sample_nums:

**break**


result_df = pd.DataFrame(result)


Figure 17: Generated code to filter out the “Sharp Drop” case


26


**1404**

**1405**


**1406**

**1407**

**1408**

**1409**

**1410**

**1411**


**1412**

**1413**

**1414**

**1415**

**1416**

**1417**


**1418**

**1419**

**1420**

**1421**

**1422**


**1423**

**1424**

**1425**

**1426**

**1427**

**1428**


**1429**

**1430**

**1431**

**1432**

**1433**

**1434**


**1435**

**1436**

**1437**

**1438**

**1439**

**1440**


**1441**

**1442**

**1443**

**1444**

**1445**


**1446**

**1447**

**1448**

**1449**

**1450**

**1451**


**1452**

**1453**

**1454**

**1455**

**1456**

**1457**



Under review as a conference paper at ICLR 2025


Fig. 18 displays real stock trends over the first 15 minutes and the stock trends generated by MarS
for the last 10 minutes of a 25-minute period for these scenarios. Each row represents a scenario
with three cases. The x-axis denotes time, and the y-axis indicates price. The blue line shows
the replay price trajectory, and the orange line depicts the simulated price trajectory with confidence
intervals. The results demonstrate MarS’s capability to effectively generate diverse market scenarios,
providing valuable insights for market participants.


|Col1|Col2|
|---|---|
|||
|||


|Col1|Col2|Col3|
|---|---|---|
||||
||||
||||
||||


|Col1|Col2|Col3|
|---|---|---|
||||
||||


|Col1|Col2|
|---|---|
|||
|||


|2|2023-04-18-002719-scenario2|
|---|---|
|||
|||
|||


|Col1|Col2|Col3|
|---|---|---|
||||
||||
||||


|Col1|Col2|
|---|---|
|||
|||


|2|2023-01-30-000|0548-scenario3|
|---|---|---|
||||
||||
||||


|Col1|Col2|
|---|---|
|||
|||



Figure 18: Case study for different scenario generation.


F CONFIGURATIONS OF INPUT OVER DIFFERENT APPLICATIONS


As we abstract the mechanism of MarS as a conditional generation process in Sec.2, we summarize
their input conditions over different applications in Table 4, and given more detailed clarification.


**DES** ~~**T**~~ **EXT** is a key component in the Conditional Trading Order Generation task, acting as a
control mechanism for the ”Conditional” aspect. It is designed to describe different market states
under which we aim to generate trading orders. Examples of such market states include ”sharp
price decline” or ”high market volatility.” By incorporating **DES** ~~**T**~~ **EXT**, we enable the generation
process to adapt to varying market conditions, making the generated trading orders contextually
relevant. More details on **DES** ~~**T**~~ **EXT** are provided in Sec E.


As for **MTCH** ~~**R**~~, it represents a comprehensive set of order-matching rules, for example, the widely
used double auction mechanism. In real-world financial markets, the rules are specified and periodically adjusted by exchanges. In our simulation, these rules are governed by the Simulated Clearing
House. We formulated **MTCH** ~~**R**~~ as a hyperparameter to make the MarS framework adaptable to
different markets and conditions. In the proporsed paper, we set it as series of standard settings of
default double auction. Expanding **MTCH** ~~**R**~~ would reveal the full extent of an exchanges trading
rules, encompassing many details that we have implemented in our code for the Simulated Clearing
House.


Moreover, while the double auction mechanism is a common paradigm for the majority of global
financial markets, there are variations in trading rules that differ across markets and periods. These
include aspects such as price fluctuation limits, circuit breakers, and the distinction between call and


27



1.000


0.975


0.950


1.15


1.10


1.05


1.00


1.10


1.05


1.00



1.00


0.98


0.96


1.10


1.05


1.00


1.10


1.05


1.00



1.00


0.95


1.10


1.05


1.00


1.05


1.00



2023-02-28-002861-scenario1


2023-03-09-003039-scenario2


2023-02-06-000056-scenario3



2023-05-09-002116-scenario1


2023-04-28-000620-scenario2


2023-01-10-000532-scenario3



2023-06-05-002843-scenario1


**1458**

**1459**


**1460**

**1461**

**1462**

**1463**

**1464**

**1465**


**1466**

**1467**

**1468**

**1469**

**1470**

**1471**


**1472**

**1473**

**1474**

**1475**

**1476**


**1477**

**1478**

**1479**

**1480**

**1481**

**1482**


**1483**

**1484**

**1485**

**1486**

**1487**

**1488**


**1489**

**1490**

**1491**

**1492**

**1493**

**1494**


**1495**

**1496**

**1497**

**1498**

**1499**


**1500**

**1501**

**1502**

**1503**

**1504**

**1505**


**1506**

**1507**

**1508**

**1509**

**1510**

**1511**



Under review as a conference paper at ICLR 2025


continuous auction sessions. Our goal was to encapsulate these variations within the conditional
trading order generation framework, ensuring the approach remains broadly applicable and flexible
for different market scenarios.

|Applications|Input Conditions|
|---|---|
|Forecasting<br>Detection<br>“What if” Analysis<br>RL Environment|(_x_0_, . . ., xm_)_, MTCH_ ~~_R_~~<br>(_x_0_, . . ., xm_)_, MTCH_ ~~_R_~~<br>[_DES_ ~~_T_~~_EXT_]_,_ (_x_0_, . . ., xm_)_∗,_ [( ˙_xi_+1_, . . .,_ ˙_xi_+_j_)]_, MTCH_ ~~_R_~~<br>_DES_ ~~_T_~~_EXT∗,_ (_x_0_, . . ., xm_)_∗,_ ( ˙_xi_+1_, . . .,_ ˙_xi_+_j_)_, MTCH_ ~~_R_~~|



Table 4: The summary of input conditions for order generation of different applications. _∗_ means
the condition is optional and [ ] indicates that either of the specified conditions should be chosen.


G DATA AND TECHNICAL DETAILS OF DETECTION


Table 5: Market manipulation samples collected from CSRC.


**Instrument** **Start time** **End time** **Case Number**
300475 2017-03-07 2017-04-25 [2020]No.92
002321 2017-04-17 2018-01-30 [2024]No.44
300263 2017-05-17 2017-09-25 [2023]No.36
300658 2019-02-13 2019-05-10 [2023]No.25
300378 2019-03-14 2019-04-15 [2021]No.116
300119 2019-04-01 2019-05-22 [2021]No.116
002718 2020-06-04 2020-07-15 [2022]No.64
300313 2020-08-19 2020-08-24 [2021]No.76
002730 2020-12-15 2021-11-17 [2024]No.23
002713 2022-05-05 2022-05-18 [2024]No.47


Table 5 shows the market manipulation samples collected from China Securities Regulatory Commission (CSRC). The data encompass a total of 10 stocks, which have never been included in
datasets used for our model training. For each stock, we gathered samples from an equal number
of trading days before and after the manipulation occurred for comparison. There are 522 trading
days for each period. For each trading day, we conducted simulations every 25 minutes and then
calculated a series of stylized facts of the simulated and replay trajectories.


The spread is a key indicator of market liquidity, with a larger spread indicating poorer market
liquidity. At time _t_, the spread _δ_ is defined as: _δt_ = _at_ _−_ _bt_, where _at_ is the best ask price and _bt_
is the best bid price. The spread distribution is widely used in detection tasks in finance (AffleckGraves et al., 2000; Vyetrenko et al., 2020).


H CONFIGURABLE TWAP STRATEGY


The Time-Weighted Average Price (TWAP) algorithm executes large trade volumes while minimizing market impact over a specified time frame. The TWAP strategy divides the total volume to
be traded into equal parts that are executed at regular intervals. This strategy consists of two distinct phases within each interval: the passive period and the aggressive period. Key configurations
include:


    - **Maximum** **Passive** **Volume** **Ratio** **(PVR)** : During the passive period, the strategy places
orders at the current bid price (bid1) with a volume determined by the PVR, aiming to fill
orders without significantly altering the market price. A PVR of 0 indicates no passive
volume during the passive period.


    - **Aggressive Price (AP)** : If passive trading does not achieve the expected volume, the strategy enters an aggressive phase, placing additional orders at a more aggressive price (AP)


28


**1512**

**1513**


**1514**

**1515**

**1516**

**1517**

**1518**

**1519**


**1520**

**1521**

**1522**

**1523**

**1524**

**1525**


**1526**

**1527**

**1528**

**1529**

**1530**


**1531**

**1532**

**1533**

**1534**

**1535**

**1536**


**1537**

**1538**

**1539**

**1540**

**1541**

**1542**


**1543**

**1544**

**1545**

**1546**

**1547**

**1548**


**1549**

**1550**

**1551**

**1552**

**1553**


**1554**

**1555**

**1556**

**1557**

**1558**

**1559**


**1560**

**1561**

**1562**

**1563**

**1564**

**1565**



Under review as a conference paper at ICLR 2025


to ensure the desired volume is executed. An AP of 0 means no aggressive order during the
aggressive period.


By balancing passive and aggressive trading, the TWAP strategy aims to execute large orders efficiently while controlling market impact.


Taking the buying task as an example, our configurable TWAP strategy is shown as below:


**Algorithm 1** Confgurable Time-Weighted Average Price (TWAP) Strategy for Buying.
**Input:** Total Volume _V_, Execution Time _T_ = 5 minutes, Split Interval ∆ _t_ = 30 seconds, Maximum
Passive Volume Ratio _PV R_, Aggressive Price _AP_ (ask1, ask2, ..., ask5)
**Output:** Executed Orders


**Initialization:**

1. Split the total volume _V_ into 10 equal parts. Each part _K_ = _V/_ 10 is expected to be
executed in ∆ _t_ = 30 seconds.


**For each interval** _i_ **from** 1 **to** 10 **:**


1. **Passive Period:** (First 25 seconds of each interval)


(a) Cancel all non-bid1 volumes.
(b) Submit a passive order with max volume _PV R × V_ and price bid1.

(c) Wait for 25 seconds.


2. **Aggressive Period:** (Last 5 seconds of each interval)


(a) If the current executed volume lags behind the expected volume:

         - Calculate the extra volume _E_ to be executed.

         - If the available volume is insufficient, cancel existing passive orders as needed.

         - Submit an aggressive order with volume _E_ and price _AP_ .
(b) Wait for 5 seconds.


I EVALUATION OF CONT’S 11 STYLIZED FACTS


I.1 SUMMARY


Stylized facts are high-level summaries of empirical characteristics in financial markets, essential
for assessing the realism of market simulations. In this section, we evaluate the 11 stylized facts
identified by Cont (2001) using historical and simulated order sequences.


To rigorously test these facts, we simulated 11,591 trajectories for the top 500 liquid stocks in the
Chinese market, from March 9, 2023, to July 12, 2023. Table 6 compares the presence of these
facts in both historical and simulated data. The **Historical** column indicates observation in real data,
while the **Simulated** column assesses their presence in simulated data. Key findings include:


    - Nine out of the 11 stylized facts are observed in both historical and simulated data. However, _Gain/loss asymmetry_ and _Leverage effect_ are not present, possibly reflecting modern
market shifts. Studies such as Ratliff-Crain et al. (2023) note similar absences in the modern U.S. Dow 30 stocks.


    - All 11 facts show similar patterns between simulated and historical sequences, showcasing
the model’s strong capability in generating realistic order sequences.


Note that merely evaluating stylized facts does not fully assess financial market simulation quality.
Further evaluations for **in-context** generation, such as forecasting (Section 4.1) and quantitative
analysis of stylized facts (Section J), are crucial.


I.2 DEFINITIONS OF STYLIZED FACTS


The 11 stylized facts from Cont (2001) are:


29


**1566**

**1567**


**1568**

**1569**

**1570**

**1571**

**1572**

**1573**


**1574**

**1575**

**1576**

**1577**

**1578**

**1579**


**1580**

**1581**

**1582**

**1583**

**1584**


**1585**

**1586**

**1587**

**1588**

**1589**

**1590**


**1591**

**1592**

**1593**

**1594**

**1595**

**1596**


**1597**

**1598**

**1599**

**1600**

**1601**

**1602**


**1603**

**1604**

**1605**

**1606**

**1607**


**1608**

**1609**

**1610**

**1611**

**1612**

**1613**


**1614**

**1615**

**1616**

**1617**

**1618**

**1619**



Under review as a conference paper at ICLR 2025

|Fact #|Fact Name|Historical|Simulated|
|---|---|---|---|
|1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11|Absence of autocorrelations<br>Heavy tails<br>Gain/loss asymmetry<br>Aggregational Gaussianity<br>Intermittency<br>Volatility clustering<br>Conditional heavy tails<br>Slow decay of autocorrelation in absolute returns<br>Leverage effect<br>Volume/volatility correlation<br>Asymmetry in timescales|_×_<br>_×_<br>_×_<br>_×_<br>_×_<br>_×_<br>_×_<br>_×_<br>_×_|_×_<br>_×_<br>_×_<br>_×_<br>_×_<br>_×_<br>_×_<br>_×_<br>_×_|



Table 6: Presence of Stylized Facts in Historical and Simulated Order Sequences. All facts are
present in both historical and simulated data, except for _Gain/loss asymmetry_ and _Leverage effect_ .


1. **Absence of autocorrelations** : “(linear) autocorrelations of asset returns are often insignificant, except for very small intraday time scales (20 minutes) for which microstructure
effects come into play.”


2. **Heavy** **tails** : “the (unconditional) distribution of returns seems to display a power-law or
Pareto-like tail, with a tail index which is finite, higher than two and less than five for
most data sets studied. In particular this excludes stable laws with infinite variance and the
normal distribution. However the precise form of the tails is difficult to determine.”


3. **Gain/loss** **asymmetry** : “one observes large drawdowns in stock prices and stock index
values but not equally large upward movements.”


4. **Aggregational** **Gaussianity** : “as one increases the time scale _t_ over which returns are
calculated, their distribution looks more and more like a normal distribution. In particular,
the shape of the distribution is not the same at different time scales.”


5. **Intermittency** : “returns display, at any time scale, a high degree of variability. This is
quantified by the presence of irregular bursts in time series of a wide variety of volatility
estimators.”


6. **Volatility** **clustering** : “different measures of volatility display a positive autocorrelation
over several days, which quantifies the fact that high-volatility events tend to cluster in
time.”


7. **Conditional** **heavy** **tails** : “even after correcting returns for volatility clustering (e.g. via
GARCH-type models), the residual time series still exhibit heavy tails. However, the tails
are less heavy than in the unconditional distribution of returns.”


8. **Slow decay of autocorrelation in absolute returns** : “the autocorrelation function of absolute returns decays slowly as a function of the time lag, roughly as a power law with an
exponent _β_ _∈_ [0 _._ 2 _,_ 0 _._ 4]. This is sometimes interpreted as a sign of long-range dependence.”


9. **Leverage effect** : “most measures of volatility of an asset are negatively correlated with the
returns of that asset.”


10. **Volume/volatility correlation** : “trading volume is correlated with all measures of volatility.”


11. **Asymmetry** **in** **time** **scales** : “coarse-grained measures of volatility predict fine-scale
volatility better than the other way round.”


I.3 EVALUATION OF STYLIZED FACTS


This subsection summarizes the evaluation results for each stylized fact. Initially, each instrument is
assessed individually, and the results are then aggregated across all instruments to obtain an average.
A 95% confidence interval is shown for line plots, and quantiles are displayed for the box plot.


**Absence of autocorrelations** : We computed the autocorrelation of returns using both the last and
mean trade prices per minute. Fig. 19a and 19b illustrate that autocorrelations decay quickly after


30


**1620**

**1621**


**1622**

**1623**

**1624**

**1625**

**1626**

**1627**


**1628**

**1629**

**1630**

**1631**

**1632**

**1633**


**1634**

**1635**

**1636**

**1637**

**1638**


**1639**

**1640**

**1641**

**1642**

**1643**

**1644**


**1645**

**1646**

**1647**

**1648**

**1649**

**1650**


**1651**

**1652**

**1653**

**1654**

**1655**

**1656**


**1657**

**1658**

**1659**

**1660**

**1661**


**1662**

**1663**

**1664**

**1665**

**1666**

**1667**


**1668**

**1669**

**1670**

**1671**

**1672**

**1673**



Under review as a conference paper at ICLR 2025


one minute. Using the last trade price shows negative autocorrelation at lag 1 due to the “bid-ask
bounce”, as noted in Ratliff-Crain et al. (2023). Conversely, the mean trade price shows positive
autocorrelation, indicating short-term momentum. For consistency with Ratliff-Crain et al. (2023),
we use the last trade price for subsequent evaluations.


|source<br>Replay<br>Simulation|Col2|Col3|Col4|Col5|Col6|Col7|Col8|Col9|Col10|Col11|
|---|---|---|---|---|---|---|---|---|---|---|
|source<br>Replay<br>Simulation|source<br>Replay<br>Simulation|source<br>Replay<br>Simulation|source<br>Replay<br>Simulation|source<br>Replay<br>Simulation|source<br>Replay<br>Simulation|source<br>Replay<br>Simulation|source<br>Replay<br>Simulation|source<br>Replay<br>Simulation|source<br>Replay<br>Simulation|source<br>Replay<br>Simulation|
||||||||||||


|Col1|source<br>Replay<br>Simulation|Col3|Col4|Col5|Col6|Col7|Col8|Col9|Col10|Col11|Col12|
|---|---|---|---|---|---|---|---|---|---|---|---|
|||||||||||||
|||||||||||||
|||||||||||||
|||||||||||||
|||||||||||||


|Col1|source<br>Replay<br>Simulation|Col3|Col4|Col5|Col6|Col7|Col8|Col9|Col10|Col11|Col12|
|---|---|---|---|---|---|---|---|---|---|---|---|
|||||||||||||
|||||||||||||
|||||||||||||
|||||||||||||
|||||||||||||


|Col1|source<br>Replay<br>Simulation|Col3|Col4|Col5|Col6|Col7|Col8|Col9|Col10|Col11|Col12|
|---|---|---|---|---|---|---|---|---|---|---|---|
|||||||||||||
|||||||||||||
|||||||||||||
|||||||||||||
|||||||||||||



**Gain/loss** **asymmetry** : Positive skewness of returns (Fig. 21a) suggests a deviation from Cont’s
original description.


**Volatility clustering** and **Slow decay of autocorrelation in absolute returns** : Autocorrelation of
absolute returns for different intervals shows slow decay in Fig. 21b. Considering absolute returns
as volatility Mller et al. (1997), this also illustrates volatility clustering.


**Intermittency** : Following Ratliff-Crain et al. (2023), extreme returns are defined as the 99% quantile of absolute returns. The Fano factor, used to verify Poisson distribution adherence, exceeded
1, indicating higher variability (Fig. 22a). This, along with heavy tails and volatility clustering,
confirms Intermittency.


**Leverage effect** : Return and lagged volatility correlation is slightly positive (Fig. 22b), contrary to
Cont’s description.


31



0.00


0.05


0.10


0.15


0.20



Auto-Correlation of Returns (Last)


lag



0.20

0.15

0.10

0.05

0.00



Auto-Correlation of Returns (Mean)


lag



(a) Absence of autocorrelations (Last Price) (b) Absence of autocorrelations (Mean Price)

Figure 19: Absence of autocorrelations. ( **a** ) Using _last_ trade price. ( **b** ) Using _mean_ trade price. Both show
rapid decline after 1 minute.


**Heavy** **tails** and **Aggregational** **Gaussianity** : Kurtosis of returns for various intervals was calculated. Positive kurtosis indicates sharper peaks and heavier tails than normal distribution. Fig. 20a
shows that return distributions exhibit heavy tails. Distributions trend towards normality as intervals
extend from 1 to 20 minutes, aligning with Aggregational Gaussianity.


**Conditional** **heavy** **tails** : Volatility varies throughout the trading day, peaking at open and close.
After normalizing returns by minute-specific volatility and computing kurtosis, Fig. 20b shows that
normalized returns still exhibit heavy tails, though less pronounced than unconditional returns in
Fig. 20a, consistent with Conditional heavy tails.



8


6


4


2



Kurtosis of Returns


Period (Minutes)



8


6


4


2



Kurtosis of Returns (Normalized)


Period (Minutes)



(a) Heavy tails and Aggregational Gaussianity (b) Conditional heavy tails

Figure 20: ( **a** ) Heavy tails and Aggregational Gaussianity. ( **b** ) Conditional heavy tails.


**1674**

**1675**


**1676**

**1677**

**1678**

**1679**

**1680**

**1681**


**1682**

**1683**

**1684**

**1685**

**1686**

**1687**


**1688**

**1689**

**1690**

**1691**

**1692**


**1693**

**1694**

**1695**

**1696**

**1697**

**1698**


**1699**

**1700**

**1701**

**1702**

**1703**

**1704**


**1705**

**1706**

**1707**

**1708**

**1709**

**1710**


**1711**

**1712**

**1713**

**1714**

**1715**


**1716**

**1717**

**1718**

**1719**

**1720**

**1721**


**1722**

**1723**

**1724**

**1725**

**1726**

**1727**



Under review as a conference paper at ICLR 2025


Skewness of Returns


|source<br>Replay<br>Simulation|Col2|Col3|Col4|Col5|Col6|Col7|Col8|Col9|Col10|Col11|
|---|---|---|---|---|---|---|---|---|---|---|
||||||||||||


|source<br>Replay<br>Simulation|Col2|Col3|Col4|Col5|Col6|Col7|Col8|Col9|Col10|
|---|---|---|---|---|---|---|---|---|---|
|||||||||||


|Col1|Col2|source<br>Replay<br>Simulation|Col4|Col5|Col6|Col7|
|---|---|---|---|---|---|---|
||||||||
||||||||


|orrelation between Return and Lagged Volatili|Col2|Col3|Col4|Col5|Col6|Col7|Col8|Col9|
|---|---|---|---|---|---|---|---|---|
|source<br>Replay<br>Simulation|source<br>Replay<br>Simulation|source<br>Replay<br>Simulation|source<br>Replay<br>Simulation|source<br>Replay<br>Simulation|source<br>Replay<br>Simulation|source<br>Replay<br>Simulation|source<br>Replay<br>Simulation|source<br>Replay<br>Simulation|
||||||||||


|Col1|source<br>Replay|Col3|
|---|---|---|
||source<br>Replay<br>|source<br>Replay<br>|
||Simulation||



(a) Volume/volatility correlation (b) Asymmetry in timescales

Figure 23: ( **a** ) Volume/volatility correlation: positive correlation. ( **b** ) Asymmetry in timescales: significant
negative asymmetry observed.


J QUANTITATIVE ANALYSIS OF STYLIZED FACTS


To ensure experiments are comparable across runs, we quantify the stylized facts with two metrics:


32



Auto-Correaltion of Absolute Returns


lag



0.5


0.4


0.3


0.2


0.1



Period (Minutes)



0.15


0.10


0.05



(a) Gain/loss asymmetry (b) Volatility Clustering

Figure 21: ( **a** ) Gain/loss asymmetry: right-skewed distribution. ( **b** ) Volatility Clustering: slow decay of
absolute return autocorrelation.



Fano Factor of "Burst" Returns


fano_factor



0.04


0.02


0.00



lag



(a) Intermittency (b) Leverage effect

Figure 22: ( **a** ) Intermittency: Fano factor exceeds 1, indicating high variability. ( **b** ) Leverage effect: slightly
positive correlation between return and lagged volatility.


**Volume/volatility correlation** : Positive correlation between volume and lagged volatility is evident
(Fig. 23a).


**Asymmetry** **in** **timescales** : Following Takahashi et al. (2019a), we assessed correlation between
fine- and coarse-grained volatility across lags from -10 to 10 minutes. Fig. 23b shows significant
negative asymmetry, consistent with Takahashi et al. (2019a) and Mller et al. (1997).



Correlation between Volume and Lagged Volatility



Correlation between Coarse and Fine Grain Volatilities


10 8 6 4 2 0 2 4 6 8 10
lag



0.12

0.10

0.08

0.06

0.04







0.4


0.2


0.0



lag


**1728**

**1729**


**1730**

**1731**

**1732**

**1733**

**1734**

**1735**


**1736**

**1737**

**1738**

**1739**

**1740**

**1741**


**1742**

**1743**

**1744**

**1745**

**1746**


**1747**

**1748**

**1749**

**1750**

**1751**

**1752**


**1753**

**1754**

**1755**

**1756**

**1757**

**1758**


**1759**

**1760**

**1761**

**1762**

**1763**

**1764**


**1765**

**1766**

**1767**

**1768**

**1769**


**1770**

**1771**

**1772**

**1773**

**1774**

**1775**


**1776**

**1777**

**1778**

**1779**

**1780**

**1781**



Under review as a conference paper at ICLR 2025


    - **Distribution Similarity** : We calculate the overlap coefficient between the empirical distribution of the stylized fact and the simulated distribution. A higher score indicates a higher
similarity in the overall distribution.


    - **Accuracy** **(3-Class)** : We classify one stylized fact value into three classes based on replay data: low, medium, and high, ensuring similar probabilities for each class over the
simulation period. We then compare the stylized fact value between simulation and replay
and calculate the accuracy of the classification. This metric measures our capability for
in-context prediction.



Figure 24: Stylized Fact Analysis: Buy Order Ratio. This metric assesses the proportion of buy to
buy+sell orders, capturing market dynamics that may influence the market trend.


We show an example for the Buy Order Ratio in Fig. 24: we calculate the buy order ratio for
each minute and then compare the distribution of the ratio between simulation and replay data. In
summary, we achieve a high score for the overall distribution similarity and an acceptable 3-class
classification considering the nuances of market dynamics. We list the full quantitative results in
Table 7.


**Name** **Distribution Similarity** **Accuracy (3-Class)**
Volatility 0.872 0.516
Spread 0.970 0.729
Mean Order Volume 0.957 0.776
Aggressive Order Ratio 0.920 0.525
Buy Order Ratio 0.933 0.570
1-Min Return 0.956 0.684
2-Min Return 0.936 0.625
3-Min Return 0.924 0.583
4-Min Return 0.914 0.548
5-Min Return 0.908 0.531


Table 7: Summary of stylized facts. The prediction for 1 to 5-Min Return is aggregated from 128
rollouts for each initial time point.


K FORECASTING


We give a detailed introduction and discussion on interactive simulation and market impact analysis.


**Market Impact Generation:** We generate market impact data using the TWAP strategy with four
different configurations: L1-P0.1, L1-P0.9, L5-P0.1, and L5-P0.9. The configuration name LX-PY
indicates that the aggressive price (AP) is askX and the maximum passive volume ratio (PVR) is
Y. These agents are assigned to buy varying volumes over 5 minutes with different instructions
and starting times. We explored the market impact generated by these trading agents from 624k
simulated trading trajectories.


33



Accuracy(3-class): 0.5698



0.025


0.020


0.015


0.010


0.005


0.000



Distribution Similarity: 0.9331



0.2 0.4 0.6 0.8
Value



























0 1 2
Replay


**1782**

**1783**


**1784**

**1785**

**1786**

**1787**

**1788**

**1789**


**1790**

**1791**

**1792**

**1793**

**1794**

**1795**


**1796**

**1797**

**1798**

**1799**

**1800**


**1801**

**1802**

**1803**

**1804**

**1805**

**1806**


**1807**

**1808**

**1809**

**1810**

**1811**

**1812**


**1813**

**1814**

**1815**

**1816**

**1817**

**1818**


**1819**

**1820**

**1821**

**1822**

**1823**


**1824**

**1825**

**1826**

**1827**

**1828**

**1829**


**1830**

**1831**

**1832**

**1833**

**1834**

**1835**



Under review as a conference paper at ICLR 2025


**Further** **analysis** **of** **synthetic** **market** **impact:** Beyond the verification of the Square-Root-Law,
we apply further analysis on synthetic market impact data. The key findings are summarized as
follows:


    - Agents with more aggressive configurations (L5-P0.1 and L5-P0.9) are expected to exhibit
a larger market impact and achieve a higher fulfillment rate. Our simulations quantify their
differences and confirm these assumptions, as illustrated in Fig. 25a.

    - The agents generate both short-term and long-term market impacts in MarS, as shown in
Fig. 25b, similar to observations studied in previous empirical work (Bacry et al., 2014;
Donier et al., 2015b). We also observe that agents with a larger passive volume ratio generate less momentum after trading ends.


|Col1|agent_type<br>L1-P0.1<br>L5-P0.1<br>L1-P0.9<br>L5-P0.9|
|---|---|
|||


|Col1|Col2|agent_type<br>L1-P0.1<br>L5-P0.1<br>L1-P0.9<br>L5-P0.9|
|---|---|---|
||||
||||




~~_[a]_~~ _[sk]_ ~~_[v]_~~ _[olume][last-pre-min][ −]_ _[LOB]_ ~~_[b]_~~ _[id]_ ~~_[v]_~~ _[olume][last-pre-min][|]_
_LOB_ ~~_i_~~ _mblast-pre-min_ = _[|][LOB]_ _,_ (11)

_LOB_ ~~_a_~~ _sk_ ~~_v_~~ _olumelast-pre-min_ + _LOB_ ~~_b_~~ _id_ ~~_v_~~ _olumelast-pre-min_

and _α, β, {γt}_ are the hyper-parameters with constrain: _α_ _∈_ (0 _,_ 1), _β_ _∈_ (0 _,_ 1), _γt_ _∈_ (0 _,_ 1) for any
_t_, and [�] _[last-pre-min]_ _t_ 0 _[−]_ [1] _γt_ = 1. _last-pre-min_ means the last minute before the agent starts to trade.
_LOB_ ~~_a_~~ _sk_ ~~_v_~~ _olume_ and _LOB_ ~~_b_~~ _id_ ~~_v_~~ _olume_ are the ask and bid volumes of LOB. _agent_ ~~_t_~~ _rans_ ~~_v_~~ _olumet_ is
the transaction volume of the agent at time _t_ . _mid_ ~~_p_~~ _ricet_ is the mid-price at time _t_ .


We also investigate the correlation between three new factors and the Square-Root-Law factors:
_sqrt_ ( _Q/V_ ) and volatility _σ_ in Fig. 26. It’s clear to see the correlation scores of those factors are
relatively low.


**Dynamics** **of** **long-term** **Market** **Impact:** For equation 14 we used to model the longterm market impact, we set two decay functions: _F_ _[decay]_ ( _t_ ) = [ [1] _[,]_ ~~_√_~~ 1 []] [and] [seven] [factors:]




[1] ~~_√_~~ 1

_t_ _[,]_




[and] [seven] [factors:]
_t_ []]



1.00


0.98


0.96


0.94


0.92









1.0

0.8

0.6

0.4

0.2

0.0



Minute



(a) Fulfillment rate of different agents (b) Short-term and long-term market impact

Figure 25: Further investigation of synthetic market impact
These findings confirm the reliability and convenience of using synthetic data from MarS, allowing
for in-depth exploration of market dynamics without the cost, risk, and time constraints associated
with real-world experiments.


**New factors of Market Impact:** The new three factors _{resiliency_, _LOB_ ~~_p_~~ _ressure_, _LOB_ ~~_d_~~ _epth}_ are
defined as below:


_resiliency_ = 1 _−_ log( _|pre_ _trading_ ~~_m_~~ _oment|_ ) (5)
_LOB_ ~~_p_~~ _ressure_ = ( _α ∗_ _agent_ ~~_t_~~ _rans_ ~~_a_~~ _sk_ + (1 _−_ _α_ ) _∗_ _agent_ ~~_t_~~ _rans_ ~~_b_~~ _id_ ) _∗_ _LOB_ ~~_i_~~ _mblast-pre-min_ (6)
_LOB_ ~~_d_~~ _epth_ = log( _β ∗_ _LOB_ ~~_a_~~ _sk_ ~~_v_~~ _olumelast-pre-min_ + (1 _−_ _β_ ) _∗_ _LOB_ ~~_b_~~ _id_ ~~_v_~~ _olumelast-pre-min_ ) _,_ (7)


where:



_pre_ ~~_t_~~ _rading_ ~~_m_~~ _oment_ =




- _tlast-pre-min_ 0 _−_ 1 _γt_ _∗_ _mid_ ~~_p_~~ _ricet_ _−_ 1 (8)
_mid_ ~~_p_~~ _ricelast-pre-min_




       - _tradet_ = _trade_ ~~_e_~~ _ndstart_ _[agent]_ ~~_[t]_~~ _[rans]_ ~~_[v]_~~ _[olume]_ _t_
_agent_ ~~_t_~~ _rans_ ~~_a_~~ _sk_ = - _ttrade_ = _trade_ ~~_e_~~ _nd_ ~~_s_~~ _tart_ _[agent]_ ~~_[t]_~~ _[rans]_ ~~_[v]_~~ _[olume]_ _t_ [+] _[ LOB]_ ~~_[a]_~~ _[sk]_ ~~_[v]_~~ _[olume][last-pre-min]_ (9)

       - _tradetrade_ ~~_es_~~ _tartnd_ _[agent]_ ~~_[t]_~~ _[rans]_ ~~_[v]_~~ _[olume]_ _t_
_agent_ ~~_t_~~ _rans_ ~~_b_~~ _id_ = - _tradetrade_ ~~_es_~~ _tartnd_ _[agent]_ ~~_[t]_~~ _[rans]_ ~~_[v]_~~ _[olume]_ _t_ [+] _[ LOB]_ ~~_[b]_~~ _[id]_ ~~_[v]_~~ _[olume][last-pre-min]_ (10)



34


Under review as a conference paper at ICLR 2025



**1836**

**1837**


**1838**

**1839**

**1840**

**1841**

**1842**

**1843**


**1844**

**1845**

**1846**

**1847**

**1848**

**1849**


**1850**

**1851**

**1852**

**1853**

**1854**


**1855**

**1856**

**1857**

**1858**

**1859**

**1860**


**1861**

**1862**

**1863**

**1864**

**1865**

**1866**


**1867**

**1868**

**1869**

**1870**

**1871**

**1872**


**1873**

**1874**

**1875**

**1876**

**1877**


**1878**

**1879**

**1880**

**1881**

**1882**

**1883**


**1884**

**1885**

**1886**

**1887**

**1888**

**1889**



where _σ_ is the volatility, _Q_ is the trading volume, and _V_ is the total market volume.


L COMPARISON OF DEEPLOB AND MARS/LMM IN FORECASTING TASKS


**Aspect** **DeepLOB** **MarS/LMM**
**Applicable Tasks** Task specific forecasting. General forecasting through simulation.
**Input Features** Limit order book (LOB) data. High-frequency order-level data.
**Model** Small, handcrafted, and not scalable Large-scale foundation model.
**Prediction** Single-step or fixed-length. Multi-step, sequence generation.


Table 8: Comparison of DeepLOB and MarS/LMM in forecasting tasks.


Table 8 compares DeepLOB and MarS/LMM in forecasting tasks, emphasizing their distinct approaches and capabilities. DeepLOB is designed for specific forecasting tasks, trained on fixed step
forecasting, and uses Limit Order Book (LOB) data as input. It features a relatively small, handcrafted model for LOB forecasting, which is hard to scale up, and provides single-step predictions
for fixed-length forecasting, such as price changes after 100 orders or 1 minute. In contrast, MarS is
designed for market simulation, capable of performing general forecasting through simulation, and
uses fine-grained order sequence data as input. It is powered by large foundation models trained on
large-scale order sequence data and offers simulation with multi-step generation.


35



1.0


0.8


0.6


0.4


0.2


0.0


0.2



sqrt(Q/V)


volatility


resiliency


LOB_depth


LOB_pressure





Figure 26: Correlation matrix of Square-Root-Law factors and three new factors.


_{_ - _VQ_ _[,][ mid-price][,][ agent]_ ~~_[r]_~~ _[eplay][,][ agent]_ ~~_[r]_~~ _[ollout][,][ LOB]_ ~~_[d]_~~ _[epth][,][ LOB]_ ~~_[p]_~~ _[ressure][,][ resiliency][}]_ [.] _mid-price_ is
the mid-price before trading. _agent_ ~~_r_~~ _ollout_ and _agent_ ~~_r_~~ _eplay_ are defined as below:


        - _tradetrade_ ~~_es_~~ _tartnd_ _[agent]_ ~~_[t]_~~ _[rans]_ ~~_[v]_~~ _[olume]_ _t_
_agent_ ~~_r_~~ _ollout_ = (12)
_total_ ~~_t_~~ _ransaction_ ~~_v_~~ _olume_ ~~_o_~~ _f_ ~~_r_~~ _ollout_

        - _tradetrade_ ~~_es_~~ _tartnd_ _[agent]_ ~~_[t]_~~ _[rans]_ ~~_[v]_~~ _[olume]_ _t_
_agent_ ~~_r_~~ _eplay_ = (13)
_total_ ~~_t_~~ _ransaction_ ~~_v_~~ _olume_ ~~_o_~~ _f_ ~~_r_~~ _eplay_


The training process is based on the synthetic long-term market impact generated by TWAP agent
( _L_ 1 _−_ _P_ 0 _._ 1). We use the torch-diff Chen (2018) to optimize _W_, where the objective is set as the
MSE reconstruction loss along with the L1 regularization.


For the base-ODE we used as a baseline in Fig. 10a, we use the basic form of the Square-Root
Process (Gatheral, 2010), which is defined as:



_dY_ ( _t_ )

= _σ_
_dt_




~~�~~ _Q_ 1
~~_√_~~ (14)
_V_ _t_


