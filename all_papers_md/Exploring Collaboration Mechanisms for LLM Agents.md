# **Exploring Collaboration Mechanisms for LLM Agents:** **A Social Psychology View**

**Jintian Zhang** _[♠∗]_ **, Xin Xu** _[♠]_ _**[∗]**_ **, Ningyu Zhang** _[♠†]_ **, Ruibo Liu** _[♥]_ **, Bryan Hooi** _[♣]_ **, Shumin Deng** _[♣][†]_

_♠_ Zhejiang University _♣_ National University of Singapore, NUS-NCS Joint Lab
_♥_ Google DeepMind
{zhangjintian,xxucs,zhangningyu,231sm}@zju.edu.cn
ruiboliu@google.com, {dcsbhk,shumin}@nus.edu.sg

**[https://zjunlp.github.io/project/MachineSoM](https://zjunlp.github.io/project/MachineSoM)**



**Abstract**


As Natural Language Processing (NLP) systems are increasingly employed in intricate social environments, a pressing query emerges:
_Can_ _these_ _NLP_ _systems_ _mirror_ _human-esque_
_collaborative intelligence, in a multi-agent so-_
_ciety_ _consisting_ _of_ _multiple_ _large_ _language_
_models_ _(LLMs)?_ This paper probes the collaboration mechanisms among contemporary
NLP systems by melding practical experiments with theoretical insights. We fabricate four unique ‘societies’ comprised of
LLM agents, where each agent is characterized by a specific ‘trait’ (easy-going or overconfident) and engages in collaboration with
a distinct ‘thinking pattern’ (debate or reflection). Through evaluating these multi-agent
societies on three benchmark datasets, we discern that certain collaborative strategies not
only outshine previous top-tier approaches but
also optimize efficiency (using fewer API tokens). Moreover, our results further illustrate
that LLM agents manifest human-like social
behaviors, such as conformity and consensus
reaching, mirroring foundational social psychology theories. In conclusion, we integrate
insights from social psychology to contextualize the collaboration of LLM agents, inspiring further investigations into the collaboration mechanism for LLMs. We have shared
our code and datasets [1], hoping to catalyze further research in this promising avenue.


**1** **Introduction**


With the prevalence of LLMs (Zhao et al., 2023;
Yin et al., 2023; Zhu et al., 2023) integral to daily
social collaboration, there is a growing imperative
to cultivate AI systems embodied with social intelligence. This also resonates with the Society of
Mind (SoM) concept (Li et al., 2023a; Zhuge et al.,


_∗_ Equal Contribution.

_†_ Corresponding Author.
[1https://github.com/zjunlp/MachineSoM.](https://github.com/zjunlp/MachineSoM)



2023; Wang et al., 2023), which suggests that intelligence emerges when computational modules
interact with each other, achieving collective objectives that surpass the capabilities of individual
modules (Minsky, 1988; Singh, 2003). Previous
studies (Park et al., 2023; Du et al., 2023b; Liang
et al., 2023; Shinn et al., 2023; Madaan et al., 2023;
Hao et al., 2023; Liu et al., 2024; Akata et al.,
2023) have delved into strategies where LLM instances, termed agents (Wang et al., 2024c; Xi et al.,
2023; Gao et al., 2023a; Cheng et al., 2024; Ma
et al., 2024b), cooperate synergistically ( _e.g._, debate and reflect) to accomplish tasks (Du et al.,
2023a; Pezeshkpour et al., 2024; Guo et al., 2024;
Du et al., 2024; Han et al., 2024). As illustrated in
Figure 1, such collaboration fosters divergent thinking processes in LLMs, making them particularly
effective for tasks demanding profound reflection.

Intuitively, reflecting on human societies (Siegal
and Varley, 2002; Leslie et al., 2004; Sap et al.,
2022; Shapira et al., 2024), where a myriad of individuals with distinct goals and roles coexist, the
SoM framework champions harmonious interactions (Singh, 2003). Intriguingly, despite the fusion of social psychology (Tajfel, 1982; Tajfel and
Turner, 2004; Johnson and Johnson, 2009) in SoM
with human group dynamics (Woolley et al., 2010;
Alderfer, 1987), which illuminates psychological
patterns within social groups, its interpretation in
the realm of LLMs is relatively uncharted (Ke et al.,
2024). Besides, our grasp of how social behaviors
influence LLMs is still in its nascent stages.
To address these issues, we delve into the machine society, probing the extent and ways that
LLMs manifest social intelligence and collaboration capabilities (Mei et al., 2024). Utilizing powerful LLMs like GPT-3.5 (OpenAI, 2022), we build a
test-bed across three datasets: MATH (Hendrycks
et al., 2021b), MMLU (Hendrycks et al., 2021a)
and Chess Move Validity (Srivastava et al., 2022).
Our approach incorporates four **societies** character


14544

_Proceedings of the 62nd Annual Meeting of the Association for Computational Linguistics (Volume 1:_ _Long Papers)_, pages 14544–14607
August 11-16, 2024 ©2024 Association for Computational Linguistics


Figure 1: An example of the chess move validity task. Given previous chess game moves, agents are required to
predict a valid next move for a specified piece.



ized by two **individual traits** ( _easy-going_ and _over-_
_confident_ ) with three agents: totally/mostly easygoing; totally/mostly overconfident. These traits
are employed to emulate nuanced human society
dynamics (Soni et al., 2024; Wang et al., 2024b,a;
Li et al., 2023b; Kong et al., 2023).
Moreover, we delve into two distinct **thinking**
**patterns** under multi-round collaboration: _debate_
(Perelman, 1971; Sunstein, 2005; Amgoud and
Prade, 2009; Du et al., 2023b; Liang et al., 2023)
and _reflection_ (Bogumil, 1985; Mezirow, 2003;
Bolton, 2010). With the permutation of thinking
patterns, we can constitute various **collaborative**
**strategies** . To this end, we implement two patterns
of collaboration in the collaborative strategies: ( _i_ )
All agents adopt the same thinking pattern at each
round; ( _ii_ ) One agents adopts the different thinking
patterns from others at each round. We then execute
these multi-round collaborative strategies within
different societies. Through our empirical analysis,
we primarily discern the following insights (Further
takeaways are in §3, §4 & Appendix A):
(1) Collaborative strategies with various permutations of thinking patterns vary significantly in
performance, and engaging in substantive debates
enhances collaboration performance. Intriguingly,
multi-agent societies composed of agents with different traits do not clearly differ in performance.
(2) Employing uniform thinking patterns across
all agents within a round of collaboration enhances efficiency. Besides, merely increasing the
number of agents or the number of collaboration
rounds does not consistently yield better outcomes.
The balance between agent quantity and strategies
emerges as a key determinant in collaboration.
(3) LLM agents manifest behaviors reminiscent
of human social tendencies, such as conformity



(Allen and Levine, 1969; Cialdini and Goldstein,
2004) or the principle of majority rule in group
thinking (Seal et al., 1998), which resonate with
several fundamental theories in social psychology
(Castro and Liskov, 1999; Tajfel and Turner, 2004).
Concretely, our findings challenge the dominant
belief that mere scale is the key. We posit that smallgroup collaboration with rational strategies might
present a more efficacious approach to utilizing
LLMs. In wrapping up, we encapsulate the core
contributions of this research as follows:

  - We initiate an elaborate exploration into collaboration mechanisms in multi-agent society.
Our goal is to identify how and to what extent
LLMs manifest social intelligence through
collaboration. To enrich our inquiry, we draw
upon theories from social psychology, contextualizing the behaviors and tendencies displayed by LLM agents.

  - Our research framework includes a meticulously crafted test-bed, integrating diverse
multi-agent societies with agent individual
traits, thinking patterns, and collaborative
strategies, evaluated over three datasets. Notably, our empirical findings can inspire
how to design a better multi-agent system
through collaboration, beyond merely scaling
up LLMs and Agents.

  - Interestingly, our observations underscore a
fascinating parallel: LLM agents mirror certain social behaviors typical of human collaboration. It could further emphasize the potential
of human-AI interaction. Generally, fostering
effective and efficient collaborative strategies
for multi-agent systems could be the key to
more socially-aware AI.



14545


|Col1|Debate Reflection<br>Based on Based on responses from I offer my sincere apologies<br>responses agent 1 and agent 3, I think for the previous erroneous<br>from agent 1 this problem ...... Agen t1 response. I made an error ...<br>and agent 2, Agen t2<br>I think this<br>problem ......<br>Apologize once again for the<br>Agen t3 incorrect answer in my<br>Agen t2 previous response ......<br>Debate<br>Agen t1<br>After reflecting on my previ-<br>Based on responses from agent 2 and ous answer, I believe that<br>agent 3, I think this problem ...... Agen t3 ......|
|---|---|
|Agent<br>Agent<br>Agent<br>** 1**<br>Agent<br>Agent<br>Agent<br>Agent<br> <br>**(c) Two Thinking Patterns.**<br>**(b) Four Societies**<br> <br>**(d) Simulating the Collaboration of a Machine Society.**<br>After reflecting on my previ-<br>ous answer, I believe that<br>......<br>Based on responses from agent 2 and<br>agent 3, I think this problem ......<br>Agent<br>Create a<br>Society<br>Round 1<br>Round 2<br>Round 3<br>......<br>Round<br>Handle a<br>Task<br>Debate<br>Reflection<br>Debate<br>Reflection<br>Reflection<br>Reflection<br>one of 8 possibilities<br>All 8 possibilities<br> -Debate  -Reflection<br>p0<br>p0<br>p1<br>p0p0<br>p0p0p1<br>p0p1p0<br>p0p1p1<br>p1 p0p0<br>p1 p0p1<br>p1 p1p0<br>p1 p1p1<br>Debate<br>Reflection<br>Debate<br><br>Solve problem<br>Society 4 (S )<br>4<br>Society 3 (S )<br>3<br>Society 2 (S )<br>2<br>Society 1 (S )<br>1<br>** 1**<br>Agent** 1**<br>Agent** 2**<br>Agent** 3**<br>** 2**<br>** 3**<br>** 1**<br>Agent** 2**<br>** 3**<br>** 1**<br>Agent** 2**<br>** 3**<br>Agent** 3**<br>Selected<br>Alternative|After reflecting on my previ-<br>ous answer, I believe that<br>......<br>Based on responses from agent 2 and<br>agent 3, I think this problem ......<br>Agent** 1**<br>Agent** 3**|


Figure 2: The overview of machine society simulation. Multiple agents with different traits make up diverse
machine societies. These agents engage in debate or self-reflection across multiple rounds to complete tasks.



**2** **Explore Collaboration Mechanisms**
**with Multiple LLM Agents**


In this section, we formulate and simulate the collaboration mechanisms explored within the machine society, drawing upon relevant concepts. We
also illustrate the society settings in Figure 2.


**2.1** **Preliminary Concepts in Collaboration**

**Individual Trait.** Inspired by intelligence emerging from the collective efforts of numerous smaller
and relatively simple agents (Minsky, 1988), each
characterized by diverse traits, we set two types of
agents exhibiting typically contrasting traits: _**easy-**_
_**going**_ and _**overconfident**_, as shown in Figure 2(a).
Easy-going agents keep things in perspective, adapt
well to different situations, and are compatible with
various types of agents (Friedman and Schustack,
1999), which results in a harmonious societal structure with democracy (Mutz, 2006; Held, 2006).
Conversely, overconfident agents tend to overestimate their competence, ignore potential risks, and
resist others’ opinions (Moore and Healy, 2008).


**Thinking** **Pattern.** Considering the SoM concept (Minsky, 1988) states that intelligence
emerges when specialized individuals within a
society cooperate through thinking, we aim to
study what thinking patterns are most successful



in producing such emerging intelligence. Thus we
explore two thinking patterns: _**debate**_ (Sunstein,
2005; Du et al., 2023b; Liang et al., 2023) and
_**reflection**_ (Bogumil, 1985; Bolton, 2010; Shinn
et al., 2023), as illustrated in Figure 2(c). ( _i_ ) In
the _debate_ pattern, several agents propose ideas,
exchange responses, engage in collective argumentation, and ultimately reach a consensus. This fosters knowledge sharing, facilitates learning, and
promotes adaptation among all agents within the
society (Weiß, 1995; Stone and Veloso, 2000; Vidal, 2006; Wooldridge, 2009). ( _ii_ ) In the _reflection_
pattern, agents review their prior responses, extract
lessons from their experiences, and refine their answers accordingly. These two patterns can unfold
over several rounds.


**Collaborative** **Strategy.** Through both critical
reflection and active participation in debate, agents
are poised to challenge their existing assumptions,
acquire fresh perspectives, and ultimately refine
their viewpoints. Employing a collaboration mechanism built on these two thinking patterns can foster more insightful decision-making (Wooldridge,
2009; Amgoud and Prade, 2009) and improve reasoning outcomes (Mezirow, 2018). In societal settings, agents typically engage in multiple rounds
of collaboration for problem-solving. In this paper,
we characterize the collaborative strategy as **a per-**



14546


**mutation of thinking patterns** throughout multiround collaboration, as illustrated in Figure 2(d)
and further elaborated in §2.2.


**2.2** **Society Simulation**


Symbols Definition

_T_ Set of agent traits
_to_ Trait : overconfident
_te_ Trait : easy-going
_A_ Set of agent instances
_ai_ The _i_ -th agent
_P_ Set of thinking patterns
_p_ 0 Debate
_p_ 1 Reflection
_S_ Set of societies
_Si_ The _i_ -th society


Table 1: The description of the symbols.


We simulate the multi-agent collaborative society, as detailed with symbols shown in Table 1.
Specifically, we construct a machine society consisting of _n_ LLM agents, denoted as _A_ = _{ai}i_ _[n]_ =1 [.]
This society contains two distinct agent traits:
= _to, te_, where _to_ and _te_ respectively de_T_ _{_ _}_
notes the overconfident and easy-going trait. For
each agent, at any round of collaboration, there
are two thinking patterns to choose from, symbolized as = _p_ 0 _, p_ 1, where _p_ 0 and _p_ 1 cor_P_ _{_ _}_
responds to _**debate**_ and _**reflection**_ respectively. By
endowing agents _A_ with the traits of _T_, we can
emulate various machine societies. In our primary study (§3), we establish four distinct societies, = _S_ 1 _, S_ 2 _, S_ 3 _, S_ 4, each consisting of
_S_ _{_ _}_
three agents: _a_ 1 _, a_ 2 _, a_ 3 . The societies are con_{_ _}_
structed based on the combination of three agents
with distinct traits, as illustrated in Figure 2(b):



In our subsequent analysis (§3.2), we delve into
more intricate scenarios, introducing a larger number of agents, increased collaboration rounds, and
a broader range of collaborative strategies.


**2.3** **Experimental Settings**

**Datasets.** We conduct a rigorous evaluation of
the reasoning and decision-making capabilities
of various machine societies across three distinct
tasks, utilizing diverse collaborative strategies:


  - _High School Multiple-Choice_ . Leveraging the
**MMLU** (Hendrycks et al., 2021a) dataset,
where problems span high school subjects
such as statistics, mathematics, computer science, biology, chemistry, and physics, agents
are required to identify the correct answer
among four multiple-choice options. Our evaluation set consists of 50 randomly selected
questions from this dataset.


  - _Math_ . Drawing from **MATH** dataset
(Hendrycks et al., 2021b), a repository of math
problems sourced from competitive events
and expressed in LaTeX, we assess the model
proficiency in advanced mathematical and scientific reasoning. The dataset segments these
problems into five graded difficulty levels, and
for our evaluation, we have randomly chosen
50 cases from Level 3 to 5.



_S_ 1 = _{_ ( _a_ 1 _←_ _to_ ) _,_ ( _a_ 2 _←_ _to_ ) _,_ ( _a_ 3 _←_ _to_ ) _}_ ( _totally overconfident_ )
_S_ 2 = _{_ ( _a_ 1 _←_ _to_ ) _,_ ( _a_ 2 _←_ _to_ ) _,_ ( _a_ 3 _←_ _te_ ) _}_ ( _mostly overconfident_ )
_S_ 3 = _{_ ( _a_ 1 _←_ _to_ ) _,_ ( _a_ 2 _←_ _te_ ) _,_ ( _a_ 3 _←_ _te_ ) _}_ ( _mostly easy-going_ )
_S_ 4 = _{_ ( _a_ 1 _←_ _te_ ) _,_ ( _a_ 2 _←_ _te_ ) _,_ ( _a_ 3 _←_ _te_ ) _}_ ( _totally easy-going_ )




- _Chess_ _Move_ _Validity_ . Utilizing the dataset
from the chess state tracking task [2] within
the comprehensive **BIG-Bench Benchmark**
(Srivastava et al., 2022), a sequence of chess
moves denoted in UCI notation [3] is provided.
Agents are required to predict a legitimate subsequent move for a specified chess piece.



where ( _ai_ _tj_ ) denotes that the agent _ai_ pos_←_
sesses the trait _tj_ . If there is an even number of
agents, we can also constitute a society with half
overconfident and half easy-going agents. In our
simulation, all agents consistently employ the same
thinking pattern at each round of collaboration, similar to Du et al. (2023b). It gives rise to eight possible 3-round collaborative strategies:


_p_ 0 _p_ 0 _p_ 0 _,_ _p_ 0 _p_ 0 _p_ 1 _,_ _p_ 0 _p_ 1 _p_ 0 _,_ _p_ 0 _p_ 1 _p_ 1 _,_

_p_ 1 _p_ 0 _p_ 0 _,_ _p_ 1 _p_ 0 _p_ 1 _,_ _p_ 1 _p_ 1 _p_ 0 _,_ _p_ 1 _p_ 1 _p_ 1



**Setups.** We craft specific instructions for each
task, trait, and strategy, which can be referred to Table 5 at Appendix D.3. To enhance result reliability,
we present average accuracy ( **Acc** ) and their respective standard deviations across five trials. Notably,
our experiments exhibit substantial standard deviations. Hence, we introduce WIN-TIE ( **W-T** ) metric, indicating the frequency (over five trials) where
the accuracy either matches or surpasses the continuous debate baseline (Du et al., 2023b). Meanwhile, we gauge the average token costs ( **Cost** )



[2https://github.com/google/BIG-bench/blob/main/bigbench/](https://github.com/google/BIG-bench/blob/main/bigbench/benchmark_tasks/chess_state_tracking/synthetic_short/task.json)
[benchmark_tasks/chess_state_tracking/synthetic_short/task.json.](https://github.com/google/BIG-bench/blob/main/bigbench/benchmark_tasks/chess_state_tracking/synthetic_short/task.json)
[3https://en.wikipedia.org/wiki/Universal_Chess_Interface.](https://en.wikipedia.org/wiki/Universal_Chess_Interface)



14547


|Col1|Metric<br>(Strategy)|Society|Collaborative Strategy<br>p0p0p0 p0p0p1 p0p1p0 p0p1p1 p1p0p0 p1p0p1 p1p1p0 p1p1p1|Metric (Society)<br>Cost ↓ W-T ↑|
|---|---|---|---|---|
|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>**66.4**_±_**1.7**<br>**65.2**_±_**3.6**<br>52.8_±_4.8<br>59.2_±_3.6<br>45.6_±_1.7<br>51.6_±_2.2<br>62.0_±_0.0<br>46.0_±_0.0<br>2970<br>2<br>_S_2<br>**66.0**_±_**0.0**<br>**65.2**_±_**1.8**<br>58.0_±_0.0<br>**66.0**_±_**0.0**<br>44.0_±_0.0<br>46.0_±_0.0<br>53.2_±_2.7<br>46.0_±_0.0<br>3081<br>9<br>_S_3<br>**70.4**_±_**4.3**<br>**64.4**_±_**0.9**<br>57.6_±_1.7<br>52.8_±_2.3<br>41.2_±_5.4<br>49.2_±_4.6<br>51.2_±_1.8<br>62.0_±_0.0<br>3172<br>1<br>_S_4<br>**69.6**_±_**3.9**<br>**65.2**_±_**3.6**<br>54.8_±_5.2<br>58.4_±_1.7<br>34.4_±_2.2<br>46.0_±_4.9<br>56.4_±_2.2<br>62.0_±_0.0<br>3090<br>2<br>Cost_ ↓_<br>All<br>4364<br>3510<br>3295<br>2665<br>3476<br>2651<br>2691<br>1976<br>-<br>W-T_ ↑_<br>All<br>-<br>**9**<br>0<br>5<br>0<br>0<br>0<br>0|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>**66.4**_±_**1.7**<br>**65.2**_±_**3.6**<br>52.8_±_4.8<br>59.2_±_3.6<br>45.6_±_1.7<br>51.6_±_2.2<br>62.0_±_0.0<br>46.0_±_0.0<br>2970<br>2<br>_S_2<br>**66.0**_±_**0.0**<br>**65.2**_±_**1.8**<br>58.0_±_0.0<br>**66.0**_±_**0.0**<br>44.0_±_0.0<br>46.0_±_0.0<br>53.2_±_2.7<br>46.0_±_0.0<br>3081<br>9<br>_S_3<br>**70.4**_±_**4.3**<br>**64.4**_±_**0.9**<br>57.6_±_1.7<br>52.8_±_2.3<br>41.2_±_5.4<br>49.2_±_4.6<br>51.2_±_1.8<br>62.0_±_0.0<br>3172<br>1<br>_S_4<br>**69.6**_±_**3.9**<br>**65.2**_±_**3.6**<br>54.8_±_5.2<br>58.4_±_1.7<br>34.4_±_2.2<br>46.0_±_4.9<br>56.4_±_2.2<br>62.0_±_0.0<br>3090<br>2<br>Cost_ ↓_<br>All<br>4364<br>3510<br>3295<br>2665<br>3476<br>2651<br>2691<br>1976<br>-<br>W-T_ ↑_<br>All<br>-<br>**9**<br>0<br>5<br>0<br>0<br>0<br>0|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>**66.4**_±_**1.7**<br>**65.2**_±_**3.6**<br>52.8_±_4.8<br>59.2_±_3.6<br>45.6_±_1.7<br>51.6_±_2.2<br>62.0_±_0.0<br>46.0_±_0.0<br>2970<br>2<br>_S_2<br>**66.0**_±_**0.0**<br>**65.2**_±_**1.8**<br>58.0_±_0.0<br>**66.0**_±_**0.0**<br>44.0_±_0.0<br>46.0_±_0.0<br>53.2_±_2.7<br>46.0_±_0.0<br>3081<br>9<br>_S_3<br>**70.4**_±_**4.3**<br>**64.4**_±_**0.9**<br>57.6_±_1.7<br>52.8_±_2.3<br>41.2_±_5.4<br>49.2_±_4.6<br>51.2_±_1.8<br>62.0_±_0.0<br>3172<br>1<br>_S_4<br>**69.6**_±_**3.9**<br>**65.2**_±_**3.6**<br>54.8_±_5.2<br>58.4_±_1.7<br>34.4_±_2.2<br>46.0_±_4.9<br>56.4_±_2.2<br>62.0_±_0.0<br>3090<br>2<br>Cost_ ↓_<br>All<br>4364<br>3510<br>3295<br>2665<br>3476<br>2651<br>2691<br>1976<br>-<br>W-T_ ↑_<br>All<br>-<br>**9**<br>0<br>5<br>0<br>0<br>0<br>0|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>**66.4**_±_**1.7**<br>**65.2**_±_**3.6**<br>52.8_±_4.8<br>59.2_±_3.6<br>45.6_±_1.7<br>51.6_±_2.2<br>62.0_±_0.0<br>46.0_±_0.0<br>2970<br>2<br>_S_2<br>**66.0**_±_**0.0**<br>**65.2**_±_**1.8**<br>58.0_±_0.0<br>**66.0**_±_**0.0**<br>44.0_±_0.0<br>46.0_±_0.0<br>53.2_±_2.7<br>46.0_±_0.0<br>3081<br>9<br>_S_3<br>**70.4**_±_**4.3**<br>**64.4**_±_**0.9**<br>57.6_±_1.7<br>52.8_±_2.3<br>41.2_±_5.4<br>49.2_±_4.6<br>51.2_±_1.8<br>62.0_±_0.0<br>3172<br>1<br>_S_4<br>**69.6**_±_**3.9**<br>**65.2**_±_**3.6**<br>54.8_±_5.2<br>58.4_±_1.7<br>34.4_±_2.2<br>46.0_±_4.9<br>56.4_±_2.2<br>62.0_±_0.0<br>3090<br>2<br>Cost_ ↓_<br>All<br>4364<br>3510<br>3295<br>2665<br>3476<br>2651<br>2691<br>1976<br>-<br>W-T_ ↑_<br>All<br>-<br>**9**<br>0<br>5<br>0<br>0<br>0<br>0|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>**66.4**_±_**1.7**<br>**65.2**_±_**3.6**<br>52.8_±_4.8<br>59.2_±_3.6<br>45.6_±_1.7<br>51.6_±_2.2<br>62.0_±_0.0<br>46.0_±_0.0<br>2970<br>2<br>_S_2<br>**66.0**_±_**0.0**<br>**65.2**_±_**1.8**<br>58.0_±_0.0<br>**66.0**_±_**0.0**<br>44.0_±_0.0<br>46.0_±_0.0<br>53.2_±_2.7<br>46.0_±_0.0<br>3081<br>9<br>_S_3<br>**70.4**_±_**4.3**<br>**64.4**_±_**0.9**<br>57.6_±_1.7<br>52.8_±_2.3<br>41.2_±_5.4<br>49.2_±_4.6<br>51.2_±_1.8<br>62.0_±_0.0<br>3172<br>1<br>_S_4<br>**69.6**_±_**3.9**<br>**65.2**_±_**3.6**<br>54.8_±_5.2<br>58.4_±_1.7<br>34.4_±_2.2<br>46.0_±_4.9<br>56.4_±_2.2<br>62.0_±_0.0<br>3090<br>2<br>Cost_ ↓_<br>All<br>4364<br>3510<br>3295<br>2665<br>3476<br>2651<br>2691<br>1976<br>-<br>W-T_ ↑_<br>All<br>-<br>**9**<br>0<br>5<br>0<br>0<br>0<br>0|
|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>**66.4**_±_**1.7**<br>**65.2**_±_**3.6**<br>52.8_±_4.8<br>59.2_±_3.6<br>45.6_±_1.7<br>51.6_±_2.2<br>62.0_±_0.0<br>46.0_±_0.0<br>2970<br>2<br>_S_2<br>**66.0**_±_**0.0**<br>**65.2**_±_**1.8**<br>58.0_±_0.0<br>**66.0**_±_**0.0**<br>44.0_±_0.0<br>46.0_±_0.0<br>53.2_±_2.7<br>46.0_±_0.0<br>3081<br>9<br>_S_3<br>**70.4**_±_**4.3**<br>**64.4**_±_**0.9**<br>57.6_±_1.7<br>52.8_±_2.3<br>41.2_±_5.4<br>49.2_±_4.6<br>51.2_±_1.8<br>62.0_±_0.0<br>3172<br>1<br>_S_4<br>**69.6**_±_**3.9**<br>**65.2**_±_**3.6**<br>54.8_±_5.2<br>58.4_±_1.7<br>34.4_±_2.2<br>46.0_±_4.9<br>56.4_±_2.2<br>62.0_±_0.0<br>3090<br>2<br>Cost_ ↓_<br>All<br>4364<br>3510<br>3295<br>2665<br>3476<br>2651<br>2691<br>1976<br>-<br>W-T_ ↑_<br>All<br>-<br>**9**<br>0<br>5<br>0<br>0<br>0<br>0|Cost_ ↓_|All|4364<br>3510<br>3295<br>2665<br>3476<br>2651<br>2691<br>1976|4364<br>3510<br>3295<br>2665<br>3476<br>2651<br>2691<br>1976|
|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>**66.4**_±_**1.7**<br>**65.2**_±_**3.6**<br>52.8_±_4.8<br>59.2_±_3.6<br>45.6_±_1.7<br>51.6_±_2.2<br>62.0_±_0.0<br>46.0_±_0.0<br>2970<br>2<br>_S_2<br>**66.0**_±_**0.0**<br>**65.2**_±_**1.8**<br>58.0_±_0.0<br>**66.0**_±_**0.0**<br>44.0_±_0.0<br>46.0_±_0.0<br>53.2_±_2.7<br>46.0_±_0.0<br>3081<br>9<br>_S_3<br>**70.4**_±_**4.3**<br>**64.4**_±_**0.9**<br>57.6_±_1.7<br>52.8_±_2.3<br>41.2_±_5.4<br>49.2_±_4.6<br>51.2_±_1.8<br>62.0_±_0.0<br>3172<br>1<br>_S_4<br>**69.6**_±_**3.9**<br>**65.2**_±_**3.6**<br>54.8_±_5.2<br>58.4_±_1.7<br>34.4_±_2.2<br>46.0_±_4.9<br>56.4_±_2.2<br>62.0_±_0.0<br>3090<br>2<br>Cost_ ↓_<br>All<br>4364<br>3510<br>3295<br>2665<br>3476<br>2651<br>2691<br>1976<br>-<br>W-T_ ↑_<br>All<br>-<br>**9**<br>0<br>5<br>0<br>0<br>0<br>0|<br>W-T_ ↑_|All|-<br>**9**<br>0<br>5<br>0<br>0<br>0<br>0|-<br>**9**<br>0<br>5<br>0<br>0<br>0<br>0|
|<br>MATH<br>Acc_ ↑_<br>_S_1<br>**46.8**_±_**4.2**<br>**46.4**_±_**3.3**<br>42.8_±_4.6<br>33.6_±_7.4<br>38.8_±_2.7<br>38.4_±_3.9<br>45.2_±_2.7<br>35.2_±_1.1<br>3417<br>8<br>_S_2<br>**50.4**_±_**2.6**<br>**52.8**_±_**2.3**<br>49.6_±_3.0<br>38.8_±_3.9<br>38.8_±_3.6<br>45.6_±_2.2<br>46.4_±_4.1<br>35.2_±_1.1<br>3623<br>8<br>_S_3<br>**47.6**_±_**4.8**<br>**48.0**_±_**3.2**<br>47.2_±_4.8<br>38.0_±_7.1<br>37.6_±_3.3<br>39.2_±_5.4<br>42.4_±_3.0<br>40.0_±_2.5<br>3757<br>8<br>_S_4<br>**50.4**_±_**1.7**<br>49.6_±_1.7<br>**53.2**_±_**1.1**<br>40.0_±_2.0<br>44.0_±_3.2<br>45.6_±_4.3<br>45.6_±_3.6<br>41.6_±_1.7<br>3658<br>10<br>Cost_ ↓_<br>All<br>4439<br>3965<br>3857<br>3414<br>3840<br>3234<br>3482<br>2681<br>-<br>W-T_ ↑_<br>All<br>-<br>**14**<br>13<br>0<br>0<br>1<br>6<br>0|<br>Acc_ ↑_|_S_1<br>_S_2<br>_S_3<br>_S_4|**46.8**_±_**4.2**<br>**46.4**_±_**3.3**<br>42.8_±_4.6<br>33.6_±_7.4<br>38.8_±_2.7<br>38.4_±_3.9<br>45.2_±_2.7<br>35.2_±_1.1<br>**50.4**_±_**2.6**<br>**52.8**_±_**2.3**<br>49.6_±_3.0<br>38.8_±_3.9<br>38.8_±_3.6<br>45.6_±_2.2<br>46.4_±_4.1<br>35.2_±_1.1<br>**47.6**_±_**4.8**<br>**48.0**_±_**3.2**<br>47.2_±_4.8<br>38.0_±_7.1<br>37.6_±_3.3<br>39.2_±_5.4<br>42.4_±_3.0<br>40.0_±_2.5<br>**50.4**_±_**1.7**<br>49.6_±_1.7<br>**53.2**_±_**1.1**<br>40.0_±_2.0<br>44.0_±_3.2<br>45.6_±_4.3<br>45.6_±_3.6<br>41.6_±_1.7|3417<br>8<br>3623<br>8<br>3757<br>8<br>3658<br>10|
|<br>MATH<br>Acc_ ↑_<br>_S_1<br>**46.8**_±_**4.2**<br>**46.4**_±_**3.3**<br>42.8_±_4.6<br>33.6_±_7.4<br>38.8_±_2.7<br>38.4_±_3.9<br>45.2_±_2.7<br>35.2_±_1.1<br>3417<br>8<br>_S_2<br>**50.4**_±_**2.6**<br>**52.8**_±_**2.3**<br>49.6_±_3.0<br>38.8_±_3.9<br>38.8_±_3.6<br>45.6_±_2.2<br>46.4_±_4.1<br>35.2_±_1.1<br>3623<br>8<br>_S_3<br>**47.6**_±_**4.8**<br>**48.0**_±_**3.2**<br>47.2_±_4.8<br>38.0_±_7.1<br>37.6_±_3.3<br>39.2_±_5.4<br>42.4_±_3.0<br>40.0_±_2.5<br>3757<br>8<br>_S_4<br>**50.4**_±_**1.7**<br>49.6_±_1.7<br>**53.2**_±_**1.1**<br>40.0_±_2.0<br>44.0_±_3.2<br>45.6_±_4.3<br>45.6_±_3.6<br>41.6_±_1.7<br>3658<br>10<br>Cost_ ↓_<br>All<br>4439<br>3965<br>3857<br>3414<br>3840<br>3234<br>3482<br>2681<br>-<br>W-T_ ↑_<br>All<br>-<br>**14**<br>13<br>0<br>0<br>1<br>6<br>0|Cost_ ↓_|All|4439<br>3965<br>3857<br>3414<br>3840<br>3234<br>3482<br>2681|4439<br>3965<br>3857<br>3414<br>3840<br>3234<br>3482<br>2681|
|<br>MATH<br>Acc_ ↑_<br>_S_1<br>**46.8**_±_**4.2**<br>**46.4**_±_**3.3**<br>42.8_±_4.6<br>33.6_±_7.4<br>38.8_±_2.7<br>38.4_±_3.9<br>45.2_±_2.7<br>35.2_±_1.1<br>3417<br>8<br>_S_2<br>**50.4**_±_**2.6**<br>**52.8**_±_**2.3**<br>49.6_±_3.0<br>38.8_±_3.9<br>38.8_±_3.6<br>45.6_±_2.2<br>46.4_±_4.1<br>35.2_±_1.1<br>3623<br>8<br>_S_3<br>**47.6**_±_**4.8**<br>**48.0**_±_**3.2**<br>47.2_±_4.8<br>38.0_±_7.1<br>37.6_±_3.3<br>39.2_±_5.4<br>42.4_±_3.0<br>40.0_±_2.5<br>3757<br>8<br>_S_4<br>**50.4**_±_**1.7**<br>49.6_±_1.7<br>**53.2**_±_**1.1**<br>40.0_±_2.0<br>44.0_±_3.2<br>45.6_±_4.3<br>45.6_±_3.6<br>41.6_±_1.7<br>3658<br>10<br>Cost_ ↓_<br>All<br>4439<br>3965<br>3857<br>3414<br>3840<br>3234<br>3482<br>2681<br>-<br>W-T_ ↑_<br>All<br>-<br>**14**<br>13<br>0<br>0<br>1<br>6<br>0|<br>W-T_ ↑_|All|-<br>**14**<br>13<br>0<br>0<br>1<br>6<br>0|-<br>**14**<br>13<br>0<br>0<br>1<br>6<br>0|
|<br>Chess Move Validity<br>Acc_ ↑_<br>_S_1<br>**54.4**_±_**1.7**<br>**52.0**_±_**0.0**<br>52.0_±_5.1<br>51.6_±_5.2<br>**54.4**_±_**1.7**<br>51.2_±_1.8<br>50.4_±_1.7<br>**52.0**_±_**0.0**<br>2443<br>11<br>_S_2<br>48.0_±_0.0<br>49.2_±_1.1<br>46.0_±_0.0<br>**54.0**_±_**0.0**<br>50.0_±_0.0<br>**52.0**_±_**0.0**<br>42.0_±_2.5<br>**52.0**_±_**0.0**<br>2442<br>25<br>_S_3<br>48.4_±_1.7<br>48.0_±_2.8<br>**54.8**_±_**5.0**<br>45.2_±_3.4<br>48.4_±_2.6<br>44.8_±_3.4<br>50.4_±_1.7<br>**53.6**_±_**0.9**<br>2451<br>23<br>_S_4<br>51.6_±_4.6<br>44.0_±_2.5<br>**54.4**_±_**3.0**<br>**53.6**_±_**5.5**<br>45.6_±_2.2<br>48.0_±_2.0<br>43.6_±_0.9<br>52.0_±_0.0<br>2404<br>12<br>Cost_ ↓_<br>All<br>3046<br>2611<br>2604<br>2179<br>2705<br>2251<br>2252<br>1830<br>-<br>W-T_ ↑_<br>All<br>-<br>10<br>12<br>10<br>11<br>9<br>5<br>**14**<br>|<br>Acc_ ↑_|_S_1<br>_S_2<br>_S_3<br>_S_4|**54.4**_±_**1.7**<br>**52.0**_±_**0.0**<br>52.0_±_5.1<br>51.6_±_5.2<br>**54.4**_±_**1.7**<br>51.2_±_1.8<br>50.4_±_1.7<br>**52.0**_±_**0.0**<br>48.0_±_0.0<br>49.2_±_1.1<br>46.0_±_0.0<br>**54.0**_±_**0.0**<br>50.0_±_0.0<br>**52.0**_±_**0.0**<br>42.0_±_2.5<br>**52.0**_±_**0.0**<br>48.4_±_1.7<br>48.0_±_2.8<br>**54.8**_±_**5.0**<br>45.2_±_3.4<br>48.4_±_2.6<br>44.8_±_3.4<br>50.4_±_1.7<br>**53.6**_±_**0.9**<br>51.6_±_4.6<br>44.0_±_2.5<br>**54.4**_±_**3.0**<br>**53.6**_±_**5.5**<br>45.6_±_2.2<br>48.0_±_2.0<br>43.6_±_0.9<br>52.0_±_0.0|2443<br>11<br>2442<br>25<br>2451<br>23<br>2404<br>12|
|<br>Chess Move Validity<br>Acc_ ↑_<br>_S_1<br>**54.4**_±_**1.7**<br>**52.0**_±_**0.0**<br>52.0_±_5.1<br>51.6_±_5.2<br>**54.4**_±_**1.7**<br>51.2_±_1.8<br>50.4_±_1.7<br>**52.0**_±_**0.0**<br>2443<br>11<br>_S_2<br>48.0_±_0.0<br>49.2_±_1.1<br>46.0_±_0.0<br>**54.0**_±_**0.0**<br>50.0_±_0.0<br>**52.0**_±_**0.0**<br>42.0_±_2.5<br>**52.0**_±_**0.0**<br>2442<br>25<br>_S_3<br>48.4_±_1.7<br>48.0_±_2.8<br>**54.8**_±_**5.0**<br>45.2_±_3.4<br>48.4_±_2.6<br>44.8_±_3.4<br>50.4_±_1.7<br>**53.6**_±_**0.9**<br>2451<br>23<br>_S_4<br>51.6_±_4.6<br>44.0_±_2.5<br>**54.4**_±_**3.0**<br>**53.6**_±_**5.5**<br>45.6_±_2.2<br>48.0_±_2.0<br>43.6_±_0.9<br>52.0_±_0.0<br>2404<br>12<br>Cost_ ↓_<br>All<br>3046<br>2611<br>2604<br>2179<br>2705<br>2251<br>2252<br>1830<br>-<br>W-T_ ↑_<br>All<br>-<br>10<br>12<br>10<br>11<br>9<br>5<br>**14**<br>|Cost_ ↓_|All|3046<br>2611<br>2604<br>2179<br>2705<br>2251<br>2252<br>1830|3046<br>2611<br>2604<br>2179<br>2705<br>2251<br>2252<br>1830|
|<br>Chess Move Validity<br>Acc_ ↑_<br>_S_1<br>**54.4**_±_**1.7**<br>**52.0**_±_**0.0**<br>52.0_±_5.1<br>51.6_±_5.2<br>**54.4**_±_**1.7**<br>51.2_±_1.8<br>50.4_±_1.7<br>**52.0**_±_**0.0**<br>2443<br>11<br>_S_2<br>48.0_±_0.0<br>49.2_±_1.1<br>46.0_±_0.0<br>**54.0**_±_**0.0**<br>50.0_±_0.0<br>**52.0**_±_**0.0**<br>42.0_±_2.5<br>**52.0**_±_**0.0**<br>2442<br>25<br>_S_3<br>48.4_±_1.7<br>48.0_±_2.8<br>**54.8**_±_**5.0**<br>45.2_±_3.4<br>48.4_±_2.6<br>44.8_±_3.4<br>50.4_±_1.7<br>**53.6**_±_**0.9**<br>2451<br>23<br>_S_4<br>51.6_±_4.6<br>44.0_±_2.5<br>**54.4**_±_**3.0**<br>**53.6**_±_**5.5**<br>45.6_±_2.2<br>48.0_±_2.0<br>43.6_±_0.9<br>52.0_±_0.0<br>2404<br>12<br>Cost_ ↓_<br>All<br>3046<br>2611<br>2604<br>2179<br>2705<br>2251<br>2252<br>1830<br>-<br>W-T_ ↑_<br>All<br>-<br>10<br>12<br>10<br>11<br>9<br>5<br>**14**<br>|<br>W-T_ ↑_<br>|All<br>|-<br>10<br>12<br>10<br>11<br>9<br>5<br>**14**<br>|-<br>10<br>12<br>10<br>11<br>9<br>5<br>**14**<br>|


occurrences where performance exceeds the strategy _p_ 0 _p_ 0 _p_ 0 under the same collaborative strategy / society. The
significances test on societies and strategies are respectively shown in Table 6, 7 at Appendix E. The experiments
of comparison with the single LLM agent is shown in Figure 21(a)-(f) at Appendix G.2.



consumed by the agents across societies, shedding
light on the efficacy of the different collaborative
strategies employed. For these evaluations, ChatGPT serves as the LLM agent accessible through
the OpenAI API gpt-3.5-turbo-1106 [4] . Further comprehensive details on data sampling and
result evaluation are introduced in Appendix D.

**3** **Analysis of Machine Social Collaboration**

Our experiments are primarily driven by the following research queries: **(RQ1)** How does problemsolving effectiveness vary under different collaborative strategies across diverse societies? **(RQ2)**
How to configure the machine society variables for
optimal performance? **(RQ3)** How does machine
social collaboration mimic the human society?


**3.1** **Main Results with Quantitative Analysis**

_To_ _address_ _**RQ1**_, we present the performance of
four distinct societies in Table 2, each employing
one of eight possible collaborative strategies, evaluated across three datasets with ChatGPT. To make
the experimental findings more general, we evaluate on other LLMs, shown in Appendix H. Our
experiments yield several pivotal observations:


**(1)** **Societies** **do** **not** **clearly** **differ** **in** **perfor-**
**mance but differ significantly in their tendency**

[4https://platform.openai.com/docs/models/gpt-3-5.](https://platform.openai.com/docs/models/gpt-3-5)



**to reach a consensus.** As observed from Table 2,
among different 3-agent societies _S_ 1 _∼_ _S_ 4 employing the same collaborative strategy (a _vertical com-_
_parison_ on Acc), the variations in accuracy are not
pronounced. We also conduct a significance test of
societies using ChatGPT in Appendix E, and other
LLMs in Appendix H, further demonstrating insignificant differences between the societies. Thus
we conclude that distinct societies composed of 3
agents possessing varied traits play an indistinctive
role in shaping performance. We infer that this
is due to LLM alignment (Ouyang et al., 2022),
inhibiting agents from displaying extreme overconfidence, which contradicts human alignment (Liu
et al., 2022). Sharma et al. (2024) also demonstrate
that LLMs tend to show sycophancy, as illustrated
in Figure 11, 12. Furthermore, we increase the
number of agents (2 to 10), accordingly resulting
in more diverse societies, as seen in Figure 14, indicating that the impact of societies on performance
remains indistinctive. We further analyze consensus reaching, _i.e._, agents reach a consistent answer
(Chen et al., 2023b), shown in Figure 16 at Appendix E, and find that more diverse societies (5
types of societies, with 2 to 10 agents) observably
impact the average quantity of consensus. **Gen-**
**erally,** **a** **society** **totally** **comprising** **easy-going**
**agents is more likely to reach a consensus.**



14548


**(2)** **Permutation** **of** **thinking** **patterns** **is** **cru-**
**cial for collaboration,** **where debate-initial and**
**debate-dominant** **strategies** **exhibit** **superiority.**
For instance, on MMLU dataset, _debate-dominant_
collaborative strategies, like _p_ 0 _p_ 0 _p_ 1, _p_ 0 _p_ 1 _p_ 0, and
_p_ 1 _p_ 0 _p_ 0, all containing two rounds of debate,
display a pronounced outperformance (65.2 for
_p_ 0 _p_ 0 _p_ 1 in _S_ 4 versus 34.4 for _p_ 1 _p_ 0 _p_ 0 in _S_ 4). As seen
from Table 2, collaborative strategies starting with
the thinking pattern of debate _p_ 0 ( _debate-initial_ ),
such as _p_ 0 _p_ 0 _p_ 0 _, p_ 0 _p_ 0 _p_ 1 _, p_ 0 _p_ 1 _p_ 0, and _p_ 0 _p_ 1 _p_ 1, generally outperform others across all datasets. Furthermore, observed from the performance ( _i_ ) under
strategies with different (3 _∼_ 10) rounds of collaboration on ChatGPT, as shown in Figure 4 and Figure 18, 19 at Appendix F, debate-initial/dominant
strategies are overall better; ( _ii_ ) on LlaMA2 Chat
13B in Table 14 and Qwen 72B in Table 26, debateinitial stategies are generally superior; ( _iii_ ) on
LlaMA2 Chat 70B in Table 20 and Mixtral 8 _×_ 7B
in Table 32, debate-dominant stategies are superior. Observed from different 3-round collaborative
strategies _pipjpk_ applied within the same society
(a _horizontal comparison_ on Acc), the variations
in accuracy are notably pronounced. Besides, the
significance test of different collaborative strategies
using ChatGPT in Appendix E and other LLMs in
Appendix H demonstrate that the order of thinking
patterns significantly impacts the effectiveness.


**(3)** **Tasks** **behave** **better** **under** **collaborative**
**strategies starting with continuous debate, and**
**debate** **combined** **with** **continuous** **reflection** **is**
**superior for difficult tasks.** Seen from Table 2,
when comparing the best performance (marked in
blue) and the worst (marked in red) within the same
societies, the difference in results for Chess Move
Validity is slight. This stands in sharp contrast to
MMLU and MATH, which suggests that _the_ _ef-_
_fectiveness of collaborative strategies depends on_
_the task_ . We then illustrate the performance under
different collaborative strategies in view of task domains and difficulty in Figure 13 at Appendix E; on
other LLMs in Figure 24, 33, 42, 56 at Appendix H.
Figure 13(a) exhibits task-specific impacts and Figure 13(b),(c) reflects domain-dependent impacts under different collaborative strategies, where _p_ 0 _p_ 0 _p_ 0
and _p_ 0 _p_ 0 _p_ 1 starting with continuous debate are generally superior. For the mathematics domain seen
from Figure 13(d), like MMLU mathematics and
MATH level 3 & 4, the performance variations
under different strategies are relatively small, but



for the more difficult task, _i.e._, MATH level 5, the
strategies containing debate and continuous reflection ( _i.e._, _p_ 0 _p_ 1 _p_ 1, _p_ 1 _p_ 1 _p_ 0) behave superiorly. These
nuanced disparities imply that _the marginal bene-_
_fits_ _derived_ _from_ _collaborative_ _strategies_ _may_ _be_
_task-dependent and difficulty-sensitive_ .


**3.2** **Impact of Machine Society Settings**

_To_ _address_ _**RQ2**_, we delve deeper into the variables influencing multi-agent society collaboration,
exploring the intricacies of agent composition, collaboration rounds, and collaborative strategies.


**Different** **Numbers** **of** **Agents.** To evaluate the
impact of different numbers of agents, we analyze performance within societies comprising
2 _∼_ 10 agents, presented in Figure 3(a). Different
numbers of agents would constitute five types of
societies, where the agents’ traits could be: _to-_
_tally/mostly_ _easy-going/overconfident;_ _half_ _easy-_
_going/overconfident_ . We observe that odd numbers
of agents generally outperform others within all
types of societies, and the possible reason is that
odd-number agents can avoid ties. Besides, we
also find that the variations of accuracy among
odd-number agents are indistinctive. Thus we
conclude that **the** **optimal** **number** **of** **agents** **is**
**3, considering both performance and efficiency** .
We also implement a significance test of the number of agents shown in Table 11 at Appendix F,
demonstrating that different numbers of agents significantly impact performance. Besides, we illustrate consensus reaching with different numbers
of agents in Figure 3(b), demonstrating that **more**
**agents are more likely to reach a consensus** .


**Different Rounds.** We then delve into the effects
of different numbers of collaboration rounds, and
further scale up the rounds of collaboration, presenting the performance under 3 to 10 rounds in
Figure 4. Despite some fluctuation in performance
from 3 to 10 rounds of collaboration, the variations
are not extremely remarkable. Considering both
accuracy and cost, we infer that **3-round** **collab-**
**oration** **is** **relatively** **effective** **and** **efficient** . We
also conduct a significance test on different rounds
of collaborative strategies, shown in Table 12 at
Appendix F, and observe that the impact of rounds
significantly relies on the collaborative strategy
employed. Generally, **the** **strategies** **starting** **or**
**dominating** **with** **reflection** _p_ 1 **differ** **clearly** **in**
**performance under different rounds** .



14549


_∼_
on _Chess Move Validity_, using _ChatGPT_ . The significance test on agent numbers and comprehensive results under
other strategies are shown in Table 11 and Figure 15, 17 at Appendix F due to space limits.











45

40

35


55


50



3 4
(p1) (p1)



5
(p1)



6
(p1)



7 8
(p1) (p1)



9
(p1)



10
(p1)



45

40


45


40



3
(p1)



4
(p1)



5
(p1)



6
(p1)



7 8
(p1) (p1)



9
(p1)



10
(p1)



55

50

45


35


30


25



3
(p0)



4
(p0)



5
(p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)



10
(p0)



45


40


35


52.5


50.0


47.5



3
(p0)



4
(p0)



5
(p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)



10
(p0)











3
(p0)



4
(p1)



5
(p0)



6
(p1)



7
(p0)



8
(p1)



9
(p0)



10
(p1)



3
(p1)



4
(p0)



5
(p1)



6
(p0)



7
(p1)



8
(p0)



9
(p1)



10
(p0)



3
(p1)



4
(p1)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



10
(p1)



3
(p0)



4
(p0)



5
(p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)



10
(p0)



Figure 4: Accuracy under different (3 10) rounds of collaboration within 3-agent society _S_ 2 (1 easy-going and 2
_∼_
overconfident agents) on MATH, using _ChatGPT_ . The significance test on rounds and experiments on MMLU and
Chess Move Validity are shown in Table 12 and Figure 18, 19 at Appendix F due to space limits.



MMLU



Chess Move Validity



0p0 p0p0p1
Part    All Part



60


50



p0p0p0
All Part



0p0 p0p0p1
Part    All Part



0p1 p0p1p0
Part    All Part



1p0 p0p1p1
Part    All Part



1p1 p1p0p0
Part    All Part



0p0 p1p0p1
Part    All Part



0p1 p1p1p0
Part    All Part



1p0 p1p1p1
Part    All Part



50


45


40


35



p0p0p0
All Part



0p1 p0p1p0
Part    All Part



p1p0 p0p1p1
Part    All Part



1p1 p1p0p0
Part    All Part



p0p0 p1p0p1
Part    All Part



0p1 p1p1p0
Part    All Part



1p0 p1p1p1
Part    All Part



Figure 5: The effect on accuracy of whether all agents in a society execute the same thinking pattern in one round,
using _ChatGPT_ . “All” and “Part” respectively refer to all agents applying the same and different thinking pattern(s)
in one round. Results on MATH and the significance test are shown in Figure 20 and Table 13 at Appendix F.



**Other Collaborative Strategies.** Venturing into
scenarios with more intricate collaboration, we allow agents to adopt varied thinking patterns in each
round of collaboration. For example, given three
agents, in a specific round of collaboration, two
agents engage in debate while the other one engages in reflection. To increase diversity, we perform a random allocation of thinking patterns to
agents in each round, steering clear of scenarios
where all agents adopt the same thinking pattern.
Intriguingly, as shown in Figure 5, the presence of
inconsistent thinking patterns within a society tends
to negatively impact performance. Given the observation, we claim that **maintaining a consistent**
**thinking** **pattern** **for** **all** **agents** **in** **a** **particular**
**round would maximize collaborative efficacy** .


**4** **Phenomena of Conformity and**
**Consensus Reaching**


_To_ _address_ _**RQ3**_, we embark on further analysis
from a social psychology view (Tajfel, 1982; Tajfel



and Turner, 2004; Johnson and Johnson, 2009), to
discern alignment between machine society collaboration and human societal dynamics (Woolley et al., 2010). Our findings indicate that machine society collaboration echoes specific human
societal phenomena or theories, such as **confor-**
**mity** (Cialdini and Goldstein, 2004; Allen and
Levine, 1969; Coultas and van Leeuwen, 2015)
and **consensus** **reaching** (Scheff, 1967; Degroot,
1974; Baronchelli, 2018) (more analysis are in
Appendix G.1). We also analyze **group** **dynam-**
**ics** (Cartwright and Zander, 1968; Alderfer, 1987;
Forsyth, 2014; Bion, 2018; Forsyth, 2018) in multiagent collaboration at Appendix G.2 as page limits.
We embark on a detailed analysis, to discern
the conformity and consensus-reaching phenomena in collaboration. For instance, as depicted in
Figure 8(a) at Appendix D.3, an agent initially responds correctly to a question. However, swayed
by the misguided answers and explanations from
the other two agents, eventually, the three agents
conform to an incorrect answer. This phenomenon



14550


|30%<br>85.60%|Col2|Col3|Col4|3.50% 75.25%|
|---|---|---|---|---|
||||||
||||||
||||||
||||||


|Col1|76.86%<br>74.13%|Col3|Col4|Col5|
|---|---|---|---|---|
||||||
||||||
||||||


where _conformity brings about benefits_ : Ratio(False _→_ True + True _→_ True) _>_ Ratio(True _→_ False + False _→_ False);


rounds of collaboration with 3-round collaborative strategies, _using ChatGPT_ . _Smaller quantity of consensus clus-_
_ters, more easier it is to reach a consensus._ Round 0 is equal to self-consistency. More details are in Appendix G.1.



mirrors detriments in “groupthink” (Janis, 1972;
Jehn, 1995), suggesting that members of tightknit groups tend to value harmony and consensus
over objective critique of divergent views, potentially leading to flawed decisions. Contrastingly,
in another scenario illustrated in Figure 8(b) at
Appendix D.3, all three agents converge on the
right answer after engaging in a society-wide debate. This mirrors benefits in “groupthink” (Jehn,
1995) and “SoM” (Minsky, 1988; Singh, 2003),
where a multitude of agents collaboratively yield
intelligence. Within such debates, agents furnish
varied viewpoints and information. Through these
exchanges, conflicts are resolved, ideas are honed,
and the group gravitates toward an informed consensus (Fisher et al., 2011; Forsyth, 2018).
We also conduct a quantitative analysis of the
prevalence of conformity and consensus-reaching
phenomena. We analyze answer correctness changing at each round of collaboration in the situation of
conformity, shown in Figure 6 on ChatGPT and Figure 28, 37, 51, 65 on other LLMs at Appendix H.
We also present the ratio of consensus reaching
at each round in Figure 7 on ChatGPT and Figure 29, 38, 52, 66 on other LLMs at Appendix H.
We summarize the following obeservations:


  - **Conformity** **is** **widespread**, and the proportion of conformity increases with the round
increases in general.




  - Generally, **reflection results in** increasing the
quantity of consensus clusters, demonstrating
**more difficulty to reach a consensus**, while
**debate is more likely to reach a consensus** .

**5** **Conclusion and Future Work**


This study has highlighted the potential of collaboration mechanisms with LLMs. Our findings reveal
the impressive collaboration capabilities of LLM
agents, with different individual traits, thinking patterns, and collaborative strategies. The emergence
of human-like behaviors in these agents, resonating
with social psychology theories, further emphasizes
the potential of human-AI interaction. Moving forward, a deeper exploration into the multi-agent
society is warranted, focusing on collaboration behavior refinement; integrating further insights from
social psychology could also guide the development of socially aware NLP systems.




- Overall, considering performance improvement, **conformity** **is** **beneficial** **in** **on** **Chat-**
**GPT, Qwen 72B; and harmful on LlaMA2**
**Chat 13B/70B, Mixtral 8** _×_ **7B** .

- **As** **the** **number** **of** **rounds** **increases,** **bene-**
**fits of conformity will weaken** (the ratio difference between True and False answers becomes smaller); and **detriments** **of** **confor-**
**mity** **enhance** (the ratio difference between
False and True answers becomes larger).



14551


**Limitations**


Although we explored various societies and collaborative strategies, our study still has its limitations.
Firstly, limited by expense, we don’t explore the
impact of multiple agents respectively based on
different LLMs, which may lead to more interesting findings at the social level due to the usage of
differently distributed pre-trained data and strategies aligned with human intentions. Furthermore,
we traversed all possible scenarios by search alone,
lacking a way to let the agents adaptively make
autonomous decisions on collaborative strategies
in specific scenarios. Although _debate_ can be as
close as possible to the upper limit, this approach
entails a larger consumption and there exist some
strategies that can achieve better performance with
less overhead. Additionally, our experimental setup
is relatively straightforward, as we have not considered more intricate configurations, such as a
broader range of traits or a larger-scale society. Finally, we evaluate performance through manual validation and rule-based matching, which also limits
the ability to validate more realistic and creative
tasks, such as literary creation.


**Reproducibility Statement**


All code and data can be found in the GitHub repository [5] . For specific experimental implementation
details, please refer to Appendix D.


**Ethics Statement**


This research was conducted in line with the highest ethical standards and best practices in research.
The data employed were extracted from publicly
accessible datasets, ensuring no usage of proprietary or confidential information. Consequently,
this research is free from any ethical concerns.


**Acknowledgments**


We would like to express gratitude to the anonymous reviewers for their kind and helpful comments. We extend our sincere gratitude to MinYen Kan and team members from NUS Web IR
/ NLP Group advised by Min-Yen Kan; Tao Gui
and team members from FudanNLP Group; and
Diyi Yang from Stanford University for providing insightful and constructive feedback on this
paper. This work was supported by the National Natural Science Foundation of China (No.


[5https://github.com/zjunlp/MachineSoM.](https://github.com/zjunlp/MachineSoM)



62206246), the Fundamental Research Funds for
the Central Universities (226-2023-00138), Zhejiang Provincial Natural Science Foundation of
China (No. LGG22F030011), Yongjiang Talent
Introduction Programme (2021A-156-G), Tencent
AI Lab Rhino-Bird Focused Research Program
(RBFR2024003), Information Technology Center
and State Key Lab of CAD&CG, Zhejiang University, and NUS-NCS Joint Laboratory (A-000854200-00).


**References**

Elif Akata, Lion Schulz, Julian Coda-Forno,
Seong Joon Oh, Matthias Bethge, and Eric
Schulz. 2023. Playing repeated games with large
[language models.](https://doi.org/10.48550/arXiv.2305.16867) _CoRR_, abs/2305.16867.


Lize Alberts, Geoff Keeling, and Amanda McCroskery.
2024. What makes for a [’good’](https://arxiv.org/abs/2401.09082) social actor? us[ing respect as a lens to evaluate interactions with lan-](https://arxiv.org/abs/2401.09082)
[guage agents.](https://arxiv.org/abs/2401.09082) _CoRR_, abs/2401.09082.


Clayton P Alderfer. 1987. An [intergroup](https://apps.dtic.mil/sti/citations/ADA135582) perspective
[on group dynamics.](https://apps.dtic.mil/sti/citations/ADA135582) _Handbook of organizational be-_
_havior_, 190:222.


Vernon L. Allen and John M. Levine. 1969. [Consen-](https://doi.org/https://doi.org/10.1016/0022-1031(69)90032-8)
[sus and conformity.](https://doi.org/https://doi.org/10.1016/0022-1031(69)90032-8) _Journal of Experimental Social_
_Psychology_, 5(4):389–399.


Leila Amgoud and Henri Prade. 2009. [Using](https://doi.org/10.1016/j.artint.2008.11.006) arguments for making [and](https://doi.org/10.1016/j.artint.2008.11.006) explaining decisions. _Artif._
_Intell._, 173(3-4):413–436.


Jinze Bai, Shuai Bai, Yunfei Chu, Zeyu Cui, Kai Dang,
Xiaodong Deng, Yang Fan, Wenbin Ge, Yu Han, Fei
Huang, Binyuan Hui, Luo Ji, Mei Li, Junyang Lin,
Runji Lin, Dayiheng Liu, Gao Liu, Chengqiang Lu,
Keming Lu, Jianxin Ma, Rui Men, Xingzhang Ren,
Xuancheng Ren, Chuanqi Tan, Sinan Tan, Jianhong
Tu, Peng Wang, Shijie Wang, Wei Wang, Shengguang Wu, Benfeng Xu, Jin Xu, An Yang, Hao Yang,
Jian Yang, Shusheng Yang, Yang Yao, Bowen Yu,
Hongyi Yuan, Zheng Yuan, Jianwei Zhang, Xingxuan Zhang, Yichang Zhang, Zhenru Zhang, Chang
Zhou, Jingren Zhou, Xiaohuan Zhou, and Tianhang Zhu. 2023. Qwen [technical](https://doi.org/10.48550/ARXIV.2309.16609) report. _CoRR_,
abs/2309.16609.


Andrea Baronchelli. 2018. The [emergence](https://doi.org/10.1098/rsos.172189) of consensus: a primer. _Royal_ _Society_ _open_ _science_,
5(2):172189.


Marcel Binz and Eric Schulz. 2023. [Using](https://doi.org/10.1073/pnas.2218523120) cognitive psychology [to](https://doi.org/10.1073/pnas.2218523120) understand gpt-3. _Pro-_
_ceedings_ _of_ _the_ _National_ _Academy_ _of_ _Sciences_,
120(6):e2218523120.


Wilfred R Bion. 2018. Group [dynamics:](https://www.taylorfrancis.com/chapters/edit/10.4324/9780429477546-19/group-dynamics-re-view-bion) A re-view.
_New directions in psychoanalysis_, pages 440–477.



14552


R. J. Bogumil. 1985. The [reflective](https://doi.org/10.1109/PROC.1985.13210) practitioner:
How [professionals](https://doi.org/10.1109/PROC.1985.13210) think in action. _Proc._ _IEEE_,
73(4):845–846.


Gillie Bolton. 2010. _Reflective_ _[practice:](https://uk.sagepub.com/en-gb/eur/reflective-practice/book252252)_ _Writing_ _and_
_[professional development](https://uk.sagepub.com/en-gb/eur/reflective-practice/book252252)_ . Sage publications.


Dorwin Cartwright and Alvin Zander. 1968. [Group dy-](https://psycnet.apa.org/record/1968-12031-000)
[namics.](https://psycnet.apa.org/record/1968-12031-000)


Miguel Castro and Barbara Liskov. 1999. [Practical](https://dl.acm.org/citation.cfm?id=296824)
[byzantine fault tolerance.](https://dl.acm.org/citation.cfm?id=296824) In _OSDI_, pages 173–186.
USENIX Association.


Guangyao Chen, Siwei Dong, Yu Shu, Ge Zhang,
Jaward Sesay, Börje F. Karlsson, Jie Fu, and Yemin
Shi. 2023a. [Autoagents: A framework for automatic](https://doi.org/10.48550/ARXIV.2309.17288)
[agent generation.](https://doi.org/10.48550/ARXIV.2309.17288) _CoRR_, abs/2309.17288.


Huaben Chen, Wenkang Ji, Lufeng Xu, and Shiyu
Zhao. 2023b. Multi-agent [consensus](https://doi.org/10.48550/ARXIV.2310.20151) seeking via
[large language models.](https://doi.org/10.48550/ARXIV.2310.20151) _CoRR_, abs/2310.20151.


Justin Chih-Yao Chen, Swarnadeep Saha, and Mohit
Bansal. 2023c. Reconcile: [Round-table](http://arxiv.org/abs/2309.13007) conference
improves reasoning via [consensus](http://arxiv.org/abs/2309.13007) among diverse
[llms.](http://arxiv.org/abs/2309.13007) _arxiv preprint_, 2309.07864.


Po-Lin Chen and Cheng-Shang Chang. 2023. [Interact:](https://doi.org/10.48550/ARXIV.2308.01552)
[Exploring the potentials of chatgpt as a cooperative](https://doi.org/10.48550/ARXIV.2308.01552)
[agent.](https://doi.org/10.48550/ARXIV.2308.01552) _CoRR_, abs/2308.01552.


Weize Chen, Yusheng Su, Jingwei Zuo, Cheng Yang,
Chenfei Yuan, Chen Qian, Chi-Min Chan, Yujia Qin,
Yaxi Lu, Ruobing Xie, Zhiyuan Liu, Maosong Sun,
and Jie Zhou. 2024. Agentverse: [Facilitating multi-](https://openreview.net/forum?id=EHg5GDnyq1)
agent collaboration and [exploring](https://openreview.net/forum?id=EHg5GDnyq1) emergent behav[iors in agents.](https://openreview.net/forum?id=EHg5GDnyq1) In _ICLR_ . OpenReview.net.


Yuheng Cheng, Ceyao Zhang, Zhengwen Zhang, Xiangrui Meng, Sirui Hong, Wenhao Li, Zihao Wang,
Zekai Wang, Feng Yin, Junhua Zhao, and Xiuqiang
He. 2024. [Exploring large language model based in-](https://arxiv.org/abs/2401.03428)
[telligent agents: Definitions, methods, and prospects.](https://arxiv.org/abs/2401.03428)
_CoRR_, abs/2401.03428.


Yun-Shiuan Chuang, Agam Goyal, Nikunj Harlalka,
Siddharth Suresh, Robert Hawkins, Sijia Yang, Dhavan Shah, Junjie Hu, and Timothy T. Rogers. 2023.
[Simulating opinion dynamics with networks of llm-](https://doi.org/10.48550/ARXIV.2311.09618)
[based agents.](https://doi.org/10.48550/ARXIV.2311.09618) _CoRR_, abs/2311.09618.


Yun-Shiuan Chuang and Timothy T. Rogers. 2023.

Computational [agent-based](https://doi.org/10.48550/ARXIV.2306.03446) models in opinion dynamics: A survey on [social](https://doi.org/10.48550/ARXIV.2306.03446) simulations and empir[ical studies.](https://doi.org/10.48550/ARXIV.2306.03446) _CoRR_, abs/2306.03446.


Robert B. Cialdini and Noah J. Goldstein. 2004. [So-](https://doi.org/10.1146/annurev.psych.55.090902.142015)
cial influence: [Compliance](https://doi.org/10.1146/annurev.psych.55.090902.142015) and conformity. _An-_
_nual Review of_ _Psychology_, 55(1):591–621. PMID:
14744228.


Karl Cobbe, Vineet Kosaraju, Mohammad Bavarian,
Mark Chen, Heewoo Jun, Lukasz Kaiser, Matthias
Plappert, Jerry Tworek, Jacob Hilton, Reiichiro
Nakano, Christopher Hesse, and John Schulman.
2021. Training verifiers to [solve](https://arxiv.org/abs/2110.14168) math word prob[lems.](https://arxiv.org/abs/2110.14168) _arXiv prepring_, abs/2110.14168.



Julie C Coultas and Edwin JC van Leeuwen. 2015.

Conformity: [Definitions,](https://link.springer.com/chapter/10.1007/978-3-319-12697-5_15) types, and evolutionary
[grounding.](https://link.springer.com/chapter/10.1007/978-3-319-12697-5_15) _Evolutionary perspectives on social psy-_
_chology_, pages 189–202.


Maxwell Crouse, Ibrahim Abdelaziz, Kinjal Basu, Soham Dan, Sadhana Kumaravel, Achille Fokoue, Pavan Kapanipathi, and Luis A. Lastras. 2023. [For-](https://doi.org/10.48550/ARXIV.2310.08535)
mally specifying the [high-level](https://doi.org/10.48550/ARXIV.2310.08535) behavior of llm[based agents.](https://doi.org/10.48550/ARXIV.2310.08535) _CoRR_, abs/2310.08535.


Morris H. Degroot. 1974. Reaching [a](https://doi.org/10.1080/01621459.1974.10480137) consensus.
_Journal_ _of_ _the_ _American_ _Statistical_ _Association_,
69(345):118–121.


Dorottya Demszky, Diyi Yang, David S. Yeager,
Christopher J. Bryan, Margarett Clapper, Susannah
Chandhok, Johannes C. Eichstaedt, Cameron Hecht,
Jeremy Jamieson, Meghann Johnson, Michaela
Jones, Danielle Krettek-Cobb, Leslie Lai, Nirel
JonesMitchell, Desmond C. Ong, Carol S. Dweck,
James J. Gross, and James W. Pennebaker. 2023. [Us-](https://doi.org/10.1038/s44159-023-00241-5)
ing large language [models](https://doi.org/10.1038/s44159-023-00241-5) in psychology. _Nature_
_Reviews Psychology_, 2(11):688–701.


Hung Du, Srikanth Thudumu, Rajesh Vasa, and Kon
Mouzakis. 2024. A survey on [context-aware](https://arxiv.org/abs/2402.01968) multi[agent systems: Techniques, challenges and future di-](https://arxiv.org/abs/2402.01968)
[rections.](https://arxiv.org/abs/2402.01968) _CoRR_, abs/2402.01968.


Yali Du, Joel Z. Leibo, Usman Islam, Richard Willis,
and Peter Sunehag. 2023a. [A review of cooperation](https://doi.org/10.48550/ARXIV.2312.05162)
[in multi-agent learning.](https://doi.org/10.48550/ARXIV.2312.05162) _CoRR_, abs/2312.05162.


Yilun Du, Shuang Li, Antonio Torralba, Joshua B.
Tenenbaum, and Igor Mordatch. 2023b. [Improving](https://doi.org/10.48550/arXiv.2305.14325)
[factuality and reasoning in language models through](https://doi.org/10.48550/arXiv.2305.14325)
[multiagent debate.](https://doi.org/10.48550/arXiv.2305.14325) _CoRR_, abs/2305.14325.


Edgar A. Duéñez-Guzmán, Suzanne Sadedin, Jane X.
Wang, Kevin R. McKee, and Joel Z. Leibo. 2023. [A](https://doi.org/10.1038/S42256-023-00754-X)
[social path to human-like artificial intelligence.](https://doi.org/10.1038/S42256-023-00754-X) _Nat._
_Mac. Intell._, 5(11):1181–1188.


Joshua M Epstein. 2012. _Generative_ _social_ _sci-_
_ence:_ _Studies_ _in_ _[agent-based](https://press.princeton.edu/books/ebook/9781400842872/generative-social-science)_ _computational_ _model-_
_[ing](https://press.princeton.edu/books/ebook/9781400842872/generative-social-science)_ . Princeton University Press.


Xueyang Feng, Zhi-Yuan Chen, Yujia Qin, Yankai
Lin, Xu Chen, Zhiyuan Liu, and Ji-Rong Wen.
2024. Large language [model-based](https://arxiv.org/abs/2402.12914) human-agent
collaboration for [complex](https://arxiv.org/abs/2402.12914) task solving. _CoRR_,
abs/2402.12914.


Roger Fisher, William L Ury, and Bruce Patton. 2011.

_Getting_ _to_ _yes:_ _[Negotiating](https://www.pon.harvard.edu/shop/getting-to-yes-negotiating-agreement-without-giving-in/)_ _agreement_ _without_ _giv-_
_[ing in](https://www.pon.harvard.edu/shop/getting-to-yes-negotiating-agreement-without-giving-in/)_ . Penguin.


Andreas Flache, Michael Mäs, Thomas Feliciani, Edmund Chattoe-Brown, Guillaume Deffuant, Sylvie
Huet, and Jan Lorenz. 2017. [Models of social influ-](https://doi.org/10.18564/JASSS.3521)
ence: Towards [the](https://doi.org/10.18564/JASSS.3521) next frontiers. _J._ _Artif._ _Soc._ _Soc._
_Simul._, 20(4).


Donelson R Forsyth. 2014. _Group_ _[dynamics](https://scholarship.richmond.edu/bookshelf/5/)_ .
Wadsworth Cengage Learning.



14553


Donelson R Forsyth. 2018. _[Group dynamics](https://books.google.com.sg/books?id=vg9EDwAAQBAJ&newbks=0&source=newbks_fb&redir_esc=y)_ . Cengage
Learning.


Howard S Friedman and Miriam W Schustack. 1999.

_Personality:_ _Classic_ _[theories](https://books.google.com/books/about/Personality.html?id=ziTvDAAAQBAJ)_ _and_ _modern_ _research_ .
Allyn and Bacon Boston, MA.


Andrew Fuchs, Andrea Passarella, and Marco Conti.
2023. Optimizing [delegation](https://doi.org/10.48550/ARXIV.2309.14718) between human and
[AI collaborative agents.](https://doi.org/10.48550/ARXIV.2309.14718) _CoRR_, abs/2309.14718.


Chen Gao, Xiaochong Lan, Nian Li, Yuan Yuan, Jingtao Ding, Zhilun Zhou, Fengli Xu, and Yong Li.
2023a. Large language [models](https://arxiv.org/abs/2312.11970) empowered agentbased modeling and [simulation:](https://arxiv.org/abs/2312.11970) A survey and per[spectives.](https://arxiv.org/abs/2312.11970) _CoRR_, abs/2312.11970.


Chen Gao, Xiaochong Lan, Zhihong Lu, Jinzhu Mao,
Jinghua Piao, Huandong Wang, Depeng Jin, and
Yong Li. 2023b. S [3] : [Social-network simulation sys-](https://doi.org/10.48550/ARXIV.2307.14984)
tem with large language [model-empowered](https://doi.org/10.48550/ARXIV.2307.14984) agents.
_CoRR_, abs/2307.14984.


Yiming Gao, Feiyu Liu, Liang Wang, Zhenjie Lian, Dehua Zheng, Weixuan Wang, Wenjin Yang, Siqin Li,
Xianliang Wang, Wenhui Chen, Jing Dai, Qiang Fu,
Wei Yang, Lanxiao Huang, and Wei Liu. 2024. [En-](https://openreview.net/forum?id=BqEvdOS1Hs)
[hancing human experience](https://openreview.net/forum?id=BqEvdOS1Hs) in human-agent collaboration: [A human-centered modeling approach based](https://openreview.net/forum?id=BqEvdOS1Hs)
[on positive human gain.](https://openreview.net/forum?id=BqEvdOS1Hs) In _ICLR_ . OpenReview.net.


Nigel Gilbert and Pietro Terna. 2000. How [to](https://link.springer.com/article/10.1007/BF02512229) build
[and use agent-based models in social science.](https://link.springer.com/article/10.1007/BF02512229) _Mind_
_& Society_, 1:57–72.


Taicheng Guo, Xiuying Chen, Yaqi Wang, Ruidi
Chang, Shichao Pei, Nitesh V. Chawla, Olaf Wiest,
and Xiangliang Zhang. 2024. [Large language model](https://arxiv.org/abs/2402.01680)
based multi-agents: A [survey](https://arxiv.org/abs/2402.01680) of progress and chal[lenges.](https://arxiv.org/abs/2402.01680) _CoRR_, abs/2402.01680.


Thilo Hagendorff. 2023. Machine [psychology:](https://doi.org/10.48550/ARXIV.2303.13988) Investigating emergent [capabilities](https://doi.org/10.48550/ARXIV.2303.13988) and behavior in
[large language models using psychological methods.](https://doi.org/10.48550/ARXIV.2303.13988)
_CoRR_, abs/2303.13988.


Shanshan Han, Qifan Zhang, Yuhang Yao, Weizhao
Jin, Zhaozhuo Xu, and Chaoyang He. 2024. [Llm](https://arxiv.org/abs/2402.03578)
[multi-agent systems: Challenges and open problems.](https://arxiv.org/abs/2402.03578)
_CoRR_, abs/2402.03578.


Rui Hao, Linmei Hu, Weijian Qi, Qingliu Wu, Yirui
Zhang, and Liqiang Nie. 2023. [Chatllm](https://doi.org/10.48550/arXiv.2304.12998) network: More [brains,](https://doi.org/10.48550/arXiv.2304.12998) more intelligence. _CoRR_,
abs/2304.12998.


Zhitao He, Pengfei Cao, Yubo Chen, Kang Liu,
Ruopeng Li, Mengshu Sun, and Jun Zhao. 2023.
[LEGO: A multi-agent collaborative framework with](https://aclanthology.org/2023.findings-emnlp.613)
role-playing and iterative feedback for causality explanation [generation.](https://aclanthology.org/2023.findings-emnlp.613) In _EMNLP_ _(Findings)_, pages
9142–9163. Association for Computational Linguistics.


David Held. 2006. _[Models of democracy](https://www.sup.org/books/title/?id=10597)_ . Polity.



Dan Hendrycks, Collin Burns, Steven Basart, Andy
Zou, Mantas Mazeika, Dawn Song, and Jacob Steinhardt. 2021a. Measuring [massive](https://openreview.net/forum?id=d7KBjmI3GmQ) multitask lan[guage understanding.](https://openreview.net/forum?id=d7KBjmI3GmQ) In _ICLR_ . OpenReview.net.


Dan Hendrycks, Collin Burns, Saurav Kadavath, Akul
Arora, Steven Basart, Eric Tang, Dawn Song, and
Jacob Steinhardt. 2021b. Measuring mathematical problem solving [with](https://datasets-benchmarks-proceedings.neurips.cc/paper/2021/hash/be83ab3ecd0db773eb2dc1b0a17836a1-Abstract-round2.html) the MATH dataset. In
_NeurIPS Datasets and Benchmarks_ .


Sirui Hong, Mingchen Zhuge, Jonathan Chen, Xiawu
Zheng, Yuheng Cheng, Jinlin Wang, Ceyao Zhang,
Zili Wang, Steven Ka Shing Yau, Zijuan Lin, Liyang
Zhou, Chenyu Ran, Lingfeng Xiao, Chenglin Wu,
and Jürgen Schmidhuber. 2024. Metagpt: Meta programming for [multi-agent](https://openreview.net/forum?id=VtmBAGCN7o) collaborative framework.
In _ICLR_ . OpenReview.net.


Yaqing Hou, Mingyang Sun, Yifeng Zeng, Yew-Soon
Ong, Yaochu Jin, Hongwei Ge, and Qiang Zhang.
2023. A multi-agent [cooperative](https://doi.org/10.1109/TEVC.2023.3268076) learning system
with evolution of social roles. _IEEE_ _Transactions_
_on Evolutionary Computation_, pages 1–1.


Irving L Janis. 1972. _Victims_ _of_ _[Groupthink:](https://psycnet.apa.org/record/1975-29417-000)_ _A_ _psy-_
_[chological study of foreign-policy decisions and fias-](https://psycnet.apa.org/record/1975-29417-000)_
_[coes.](https://psycnet.apa.org/record/1975-29417-000)_ Houghton Mifflin.


Karen A Jehn. 1995. A multimethod examination of
the benefits and [detriments](https://doi.org/10.2307/2393638) of intragroup conflict.
_Administrative science quarterly_, pages 256–282.


Ziwei Ji, Nayeon Lee, Rita Frieske, Tiezheng Yu,
Dan Su, Yan Xu, Etsuko Ishii, Yejin Bang, Andrea
Madotto, and Pascale Fung. 2023. [Survey of halluci-](https://doi.org/10.1145/3571730)
[nation in natural language generation.](https://doi.org/10.1145/3571730) _ACM Comput._
_Surv._, 55(12):248:1–248:38.


Albert Q. Jiang, Alexandre Sablayrolles, Arthur Mensch, Chris Bamford, Devendra Singh Chaplot, Diego
de Las Casas, Florian Bressand, Gianna Lengyel,
Guillaume Lample, Lucile Saulnier, Lélio Renard Lavaud, Marie-Anne Lachaux, Pierre Stock,
Teven Le Scao, Thibaut Lavril, Thomas Wang, Timothée Lacroix, and William El Sayed. 2023. [Mistral](https://doi.org/10.48550/ARXIV.2310.06825)
[7b.](https://doi.org/10.48550/ARXIV.2310.06825) _CoRR_, abs/2310.06825.


Albert Q. Jiang, Alexandre Sablayrolles, Antoine
Roux, Arthur Mensch, Blanche Savary, Chris
Bamford, Devendra Singh Chaplot, Diego de las
Casas, Emma Bou Hanna, Florian Bressand, Gianna Lengyel, Guillaume Bour, Guillaume Lample, Lélio Renard Lavaud, Lucile Saulnier, MarieAnne Lachaux, Pierre Stock, Sandeep Subramanian,
Sophia Yang, Szymon Antoniak, Teven Le Scao,
Théophile Gervet, Thibaut Lavril, Thomas Wang,
Timothée Lacroix, and William El Sayed. 2024.
[Mixtral of experts.](https://arxiv.org/abs/2401.04088) _CoRR_, abs/2401.04088.


David W Johnson and Roger T Johnson. 2009. [An ed-](https://citeseerx.ist.psu.edu/document?repid=rep1&type=pdf&doi=72585feb1200d53a81d4fb3e64862d69317b72c3)
ucational psychology [success](https://citeseerx.ist.psu.edu/document?repid=rep1&type=pdf&doi=72585feb1200d53a81d4fb3e64862d69317b72c3) story: Social interdependence theory and cooperative learning. _Educa-_
_tional researcher_, 38(5):365–379.



14554


Luoma Ke, Song Tong, Peng Chen, and Kaiping Peng.
2024. Exploring the [frontiers](https://arxiv.org/abs/2401.01519) of llms in psychological applications: A [comprehensive](https://arxiv.org/abs/2401.01519) review. _CoRR_,
abs/2401.01519.


Chuyi Kong, Yaxin Fan, Xiang Wan, Feng Jiang, and
Benyou Wang. 2023. Large [language](https://doi.org/10.48550/ARXIV.2308.11534) model as a
[user simulator.](https://doi.org/10.48550/ARXIV.2308.11534) _CoRR_, abs/2308.11534.


Tatsuki Kuribayashi, Yohei Oseki, and Timothy Baldwin. 2024. Psychometric [predictive](https://doi.org/10.48550/arXiv.2311.07484) power of large
[language](https://doi.org/10.48550/arXiv.2311.07484) models. In _NAACL_ _(Findings)_ . Association for Computational Linguistics.


Marc Lanctot, Kate Larson, Yoram Bachrach, Luke
Marris, Zun Li, Avishkar Bhoopchand, Thomas W.
Anthony, Brian Tanner, and Anna Koop. 2023. [Eval-](https://doi.org/10.48550/ARXIV.2312.03121)
uating agents using social choice theory. _CoRR_,
abs/2312.03121.


Alan M Leslie, Ori Friedman, and Tim P German. 2004.

[Core mechanisms in ‘theory of mind’.](https://doi.org/https://doi.org/10.1016/j.tics.2004.10.001) _Trends in cog-_
_nitive sciences_, 8(12):528–533.


Guohao Li, Hasan Hammoud, Hani Itani, Dmitrii
Khizbullin, and Bernard Ghanem. 2023a. [CAMEL:](http://papers.nips.cc/paper_files/paper/2023/hash/a3621ee907def47c1b952ade25c67698-Abstract-Conference.html)
communicative agents [for](http://papers.nips.cc/paper_files/paper/2023/hash/a3621ee907def47c1b952ade25c67698-Abstract-Conference.html) "mind" exploration of
[large language model society.](http://papers.nips.cc/paper_files/paper/2023/hash/a3621ee907def47c1b952ade25c67698-Abstract-Conference.html) In _NeurIPS_ .


Yuan Li, Yixuan Zhang, and Lichao Sun. 2023b.

Metaagents: [Simulating](https://doi.org/10.48550/ARXIV.2310.06500) interactions of human
behaviors for [llm-based](https://doi.org/10.48550/ARXIV.2310.06500) task-oriented coordination via [collaborative](https://doi.org/10.48550/ARXIV.2310.06500) generative agents. _CoRR_,
abs/2310.06500.


Yujia Li, David H. Choi, Junyoung Chung, Nate Kushman, Julian Schrittwieser, Rémi Leblond, Tom Eccles, James Keeling, Felix Gimeno, Agustin Dal
Lago, Thomas Hubert, Peter Choy, Cyprien de Masson d’Autume, Igor Babuschkin, Xinyun Chen, PoSen Huang, Johannes Welbl, Sven Gowal, Alexey
Cherepanov, James Molloy, Daniel J. Mankowitz,
Esme Sutherland Robson, Pushmeet Kohli, Nando
de Freitas, Koray Kavukcuoglu, and Oriol Vinyals.
2022. Competition-level [code](https://doi.org/10.1126/science.abq1158) generation with al[phacode.](https://doi.org/10.1126/science.abq1158) _Science_, 378(6624):1092–1097.


Tian Liang, Zhiwei He, Wenxiang Jiao, Xing Wang,
Yan Wang, Rui Wang, Yujiu Yang, Zhaopeng Tu,
and Shuming Shi. 2023. [Encouraging](https://doi.org/10.48550/arXiv.2305.19118) divergent
thinking in large [language](https://doi.org/10.48550/arXiv.2305.19118) models through multi[agent debate.](https://doi.org/10.48550/arXiv.2305.19118) _CoRR_, abs/2305.19118.


Yuanzhi Liang, Linchao Zhu, and Yi Yang. 2024. [An-](https://arxiv.org/abs/2401.06509)
teval: [Quantitatively evaluating informativeness and](https://arxiv.org/abs/2401.06509)
expressiveness of [agent](https://arxiv.org/abs/2401.06509) social interactions. _CoRR_,
abs/2401.06509.


Ruibo Liu, Ruixin Yang, Chenyan Jia, Ge Zhang,
Denny Zhou, Andrew M. Dai, Diyi Yang, and
Soroush Vosoughi. 2024. Training [socially](https://openreview.net/forum?id=NddKiWtdUm) aligned
language models in [simulated](https://openreview.net/forum?id=NddKiWtdUm) human society. In
_ICLR_ . OpenReview.net.



Ruibo Liu, Ge Zhang, Xinyu Feng, and Soroush
Vosoughi. 2022. [Aligning generative language mod-](https://doi.org/10.18653/V1/2022.FINDINGS-NAACL.18)
els with [human](https://doi.org/10.18653/V1/2022.FINDINGS-NAACL.18) values. In _NAACL-HLT_ _(Findings)_,
pages 241–252. Association for Computational Linguistics.


Zijun Liu, Yanzhe Zhang, Peng Li, Yang Liu, and Diyi
Yang. 2023. Dynamic [llm-agent](https://doi.org/10.48550/ARXIV.2310.02170) network: An llm[agent collaboration framework with agent team opti-](https://doi.org/10.48550/ARXIV.2310.02170)
[mization.](https://doi.org/10.48550/ARXIV.2310.02170) _CoRR_, abs/2310.02170.


Jan Lorenz, Martin Neumann, and Tobias Schröder.
2021. Individual attitude [change](https://doi.org/10.1037/rev0000291) and societal dynamics: Computational [experiments](https://doi.org/10.1037/rev0000291) with psycho[logical theories.](https://doi.org/10.1037/rev0000291) _Psychological Review_, 128(4):623.


Junliang Luo, Tianyu Li, Di Wu, Michael Jenkin, Steve
Liu, and Gregory Dudek. 2024. [Hallucination detec-](https://arxiv.org/abs/2401.08358)
tion and hallucination [mitigation:](https://arxiv.org/abs/2401.08358) An investigation.
_CoRR_, abs/2401.08358.


Chang Ma, Junlei Zhang, Zhihao Zhu, Cheng Yang,
Yujiu Yang, Yaohui Jin, Zhenzhong Lan, Lingpeng
Kong, and Junxian He. 2024a. [Agentboard:](https://arxiv.org/abs/2401.13178) An analytical evaluation board of multi-turn llm agents.
_CoRR_, abs/2401.13178.


Qun Ma, Xiao Xue, Deyu Zhou, Xiangning Yu,
Donghua Liu, Xuwen Zhang, Zihan Zhao, Yifan
Shen, Peilin Ji, Juanjuan Li, Gang Wang, and Wanpeng Ma. 2024b. Computational [experiments](https://arxiv.org/abs/2402.00262) meet
large language model [based](https://arxiv.org/abs/2402.00262) agents: A survey and
[perspective.](https://arxiv.org/abs/2402.00262) _CoRR_, abs/2402.00262.


Aman Madaan, Niket Tandon, Prakhar Gupta, Skyler
Hallinan, Luyu Gao, Sarah Wiegreffe, Uri Alon,
Nouha Dziri, Shrimai Prabhumoye, Yiming Yang,
Shashank Gupta, Bodhisattwa Prasad Majumder,
Katherine Hermann, Sean Welleck, Amir Yazdanbakhsh, and Peter Clark. 2023. [Self-refine:](http://papers.nips.cc/paper_files/paper/2023/hash/91edff07232fb1b55a505a9e9f6c0ff3-Abstract-Conference.html) Iterative
[refinement with self-feedback.](http://papers.nips.cc/paper_files/paper/2023/hash/91edff07232fb1b55a505a9e9f6c0ff3-Abstract-Conference.html) In _NeurIPS_ .


Qiaozhu Mei, Yutong Xie, Walter Yuan, and
Matthew O. Jackson. 2024. A [turing](https://doi.org/10.1073/pnas.2313925121) test of
whether ai chatbots are [behaviorally](https://doi.org/10.1073/pnas.2313925121) similar to hu[mans.](https://doi.org/10.1073/pnas.2313925121) _Proceedings of the National Academy of Sci-_
_ences_, 121(9):e2313925121.


Jack Mezirow. 2003. How critical [reflection](https://www.colorado.edu/plc/sites/default/files/attached-files/how_critical_reflection_triggers_transfo.pdf) triggers
[transformative learning.](https://www.colorado.edu/plc/sites/default/files/attached-files/how_critical_reflection_triggers_transfo.pdf) _Adult and Continuing Edu-_
_cation:_ _Teaching, learning and research_, 4:199.


Jack Mezirow. 2018. Transformative learning theory.
In _Contemporary_ _theories_ _of_ _learning_, pages 114–
128. Routledge.


Marvin Minsky. 1988. _Society_ _of_ _mind_ . Simon and
Schuster.


Don A Moore and Paul J Healy. 2008. [The trouble with](https://healy.econ.ohio-state.edu/papers/Moore_Healy-TroubleWithOverconfidence_WP.pdf)
[overconfidence.](https://healy.econ.ohio-state.edu/papers/Moore_Healy-TroubleWithOverconfidence_WP.pdf) _Psychological review_, 115(2):502.


Diana C Mutz. 2006. _Hearing_ _the_ _[other](https://www.cambridge.org/core/books/hearing-the-other-side/7CB061238546313D287668FF8EFE2EF7)_ _side:_ _Delib-_
_[erative versus participatory democracy](https://www.cambridge.org/core/books/hearing-the-other-side/7CB061238546313D287668FF8EFE2EF7)_ . Cambridge
University Press.



14555


OpenAI. 2022. Chatgpt: Optimizing language models
for dialogue. [https://openai.com/blog/chatgpt/.](https://openai.com/blog/chatgpt/)


Long Ouyang, Jeffrey Wu, Xu Jiang, Diogo Almeida,
Carroll L. Wainwright, Pamela Mishkin, Chong
Zhang, Sandhini Agarwal, Katarina Slama, Alex
Ray, John Schulman, Jacob Hilton, Fraser Kelton,
Luke Miller, Maddie Simens, Amanda Askell, Peter
Welinder, Paul F. Christiano, Jan Leike, and Ryan
Lowe. 2022. [Training language models to follow in-](http://papers.nips.cc/paper_files/paper/2022/hash/b1efde53be364a73914f58805a001731-Abstract-Conference.html)
[structions with human feedback.](http://papers.nips.cc/paper_files/paper/2022/hash/b1efde53be364a73914f58805a001731-Abstract-Conference.html) In _NeurIPS_ .


Joon Sung Park, Joseph C. O’Brien, Carrie Jun
Cai, Meredith Ringel Morris, Percy Liang, and
Michael S. Bernstein. 2023. Generative agents: Interactive simulacra of human behavior. In _UIST_,
pages 2:1–2:22. ACM.


Chaim Perelman. 1971. _[The new rhetoric](https://link.springer.com/chapter/10.1007/978-94-010-1713-8_8)_ . Springer.


Pouya Pezeshkpour, Eser Kandogan, Nikita Bhutani,
Sajjadur Rahman, Tom Mitchell, and Estevam
Hruschka. 2024. Reasoning [capacity](https://arxiv.org/abs/2402.01108) in multiagent systems: [Limitations,](https://arxiv.org/abs/2402.01108) challenges and human[centered solutions.](https://arxiv.org/abs/2402.01108) _CoRR_, abs/2402.01108.


Shuofei Qiao, Ningyu Zhang, Runnan Fang, Yujie
Luo, Wangchunshu Zhou, Yuchen Eleanor Jiang,
Chengfei Lv, and Huajun Chen. 2024. [Autoact:](https://arxiv.org/abs/2401.05268)
Automatic agent [learning](https://arxiv.org/abs/2401.05268) from scratch via self[planning.](https://arxiv.org/abs/2401.05268) In _ACL_ . Association for Computational
Linguistics.


Vipula Rawte, Amit P. Sheth, and Amitava Das. 2023.

[A survey of hallucination in large foundation models.](https://doi.org/10.48550/arXiv.2309.05922)
_CoRR_, abs/2309.05922.


Maarten Sap, Ronan Le Bras, Daniel Fried, and Yejin
Choi. 2022. Neural [theory-of-mind?](https://doi.org/10.18653/V1/2022.EMNLP-MAIN.248) on the limits
[of social intelligence in large lms.](https://doi.org/10.18653/V1/2022.EMNLP-MAIN.248) In _EMNLP_, pages
3762–3780. Association for Computational Linguistics.


Thomas J Scheff. 1967. Toward a [sociological](https://doi.org/10.2307/2091716) model
[of consensus.](https://doi.org/10.2307/2091716) _American Sociological Review_, pages
32–46.


David Wyatt Seal, Laura M Bogart, and Anke A
Ehrhardt. 1998. Small group [dynamics:](https://doi.org/10.1037/1089-2699.2.4.253) The util[ity of focus group discussions as a research method.](https://doi.org/10.1037/1089-2699.2.4.253)
_Group_ _Dynamics:_ _Theory,_ _Research,_ _and_ _Practice_,
2(4):253.


Murray Shanahan, Kyle McDonell, and Laria
Reynolds. 2023. Role play with large language
[models.](https://doi.org/10.1038/S41586-023-06647-8) _Nat._, 623(7987):493–498.


Natalie Shapira, Mosh Levy, Seyed Hossein Alavi,
Xuhui Zhou, Yejin Choi, Yoav Goldberg, Maarten
Sap, and Vered Shwartz. 2024. [Clever hans or neu-](https://aclanthology.org/2024.eacl-long.138)
ral theory of mind? [stress testing social reasoning in](https://aclanthology.org/2024.eacl-long.138)
large [language](https://aclanthology.org/2024.eacl-long.138) models. In _EACL_ _(1)_, pages 2257–
2273. Association for Computational Linguistics.



Mrinank Sharma, Meg Tong, Tomasz Korbak, David
Duvenaud, Amanda Askell, Samuel R. Bowman, Newton Cheng, Esin Durmus, Zac HatfieldDodds, Scott R. Johnston, Shauna Kravec, Timothy Maxwell, Sam McCandlish, Kamal Ndousse,
Oliver Rausch, Nicholas Schiefer, Da Yan, Miranda
Zhang, and Ethan Perez. 2024. [Towards understand-](https://openreview.net/forum?id=tvhaxkMKAn)
[ing sycophancy in language models.](https://openreview.net/forum?id=tvhaxkMKAn) In _ICLR_ . OpenReview.net.


Noah Shinn, Beck Labash, and Ashwin Gopinath.
2023. Reflexion: an [autonomous](https://doi.org/10.48550/arXiv.2303.11366) agent with dynamic memory [and](https://doi.org/10.48550/arXiv.2303.11366) self-reflection. _arXiv_ _preprint_,
abs/2303.11366.


Michael Siegal and Rosemary Varley. 2002. [Neural](https://doi.org/10.1038/nrn844)
systems involved [in](https://doi.org/10.1038/nrn844) ’theory of mind’. _Nature_ _Re-_
_views Neuroscience_, 3(6):463–471.


Push Singh. 2003. Examining the [society](http://www.cai.sk/ojs/index.php/cai/article/view/467) of mind.
_Comput. Artif. Intell._, 22(6):521–543.


Paul Smaldino. 2023. _[Modeling social behavior: Math-](https://press.princeton.edu/books/paperback/9780691224145/modeling-social-behavior)_
_[ematical and agent-based models of social dynamics](https://press.princeton.edu/books/paperback/9780691224145/modeling-social-behavior)_
_[and cultural evolution](https://press.princeton.edu/books/paperback/9780691224145/modeling-social-behavior)_ . Princeton University Press.


Nikita Soni, Niranjan Balasubramanian, H. Andrew
Schwartz, and Dirk Hovy. 2024. [Comparing](https://arxiv.org/abs/2401.12492)
human-centered [language](https://arxiv.org/abs/2401.12492) modeling: Is it better to
model groups, [individual](https://arxiv.org/abs/2401.12492) traits, or both? _CoRR_,
abs/2401.12492.


Aarohi Srivastava, Abhinav Rastogi, Abhishek Rao,
Abu Awal Md Shoeb, Abubakar Abid, Adam
Fisch, Adam R. Brown, Adam Santoro, Aditya
Gupta, Adrià Garriga-Alonso, Agnieszka Kluska,
Aitor Lewkowycz, Akshat Agarwal, Alethea Power,
Alex Ray, Alex Warstadt, Alexander W. Kocurek,
Ali Safaya, Ali Tazarv, Alice Xiang, Alicia Parrish, Allen Nie, Aman Hussain, Amanda Askell,
Amanda Dsouza, Ameet Rahane, Anantharaman S.
Iyer, Anders Andreassen, Andrea Santilli, Andreas
Stuhlmüller, Andrew M. Dai, Andrew La, Andrew K. Lampinen, Andy Zou, Angela Jiang, Angelica Chen, Anh Vuong, Animesh Gupta, Anna
Gottardi, Antonio Norelli, Anu Venkatesh, Arash
Gholamidavoodi, Arfa Tabassum, Arul Menezes,
Arun Kirubarajan, Asher Mullokandov, Ashish Sabharwal, Austin Herrick, Avia Efrat, Aykut Erdem,
Ayla Karakas, and et al. 2022. Beyond the imitation game: [Quantifying](https://doi.org/10.48550/arXiv.2206.04615) and extrapolating the
capabilities of [language](https://doi.org/10.48550/arXiv.2206.04615) models. _arXiv_ _preprint_,
abs/2206.04615.


Peter Stone and Manuela M. Veloso. 2000. [Multiagent](https://doi.org/10.1023/A:1008942012299)
systems: [A survey from a machine learning perspec-](https://doi.org/10.1023/A:1008942012299)
[tive.](https://doi.org/10.1023/A:1008942012299) _Auton. Robots_, 8(3):345–383.


Cass R Sunstein. 2005. _Why_ _[societies](https://doi.org/10.4159/9780674267657)_ _need_ _dissent_ .
Harvard University Press.


Henri Tajfel. 1982. Social [psychology](https://www.annualreviews.org/doi/abs/10.1146/annurev.ps.33.020182.000245?journalCode=psych) of intergroup
[relations.](https://www.annualreviews.org/doi/abs/10.1146/annurev.ps.33.020182.000245?journalCode=psych) _Annual review of psychology_, 33(1):1–39.


Henri Tajfel and John C Turner. 2004. [The social iden-](https://psycnet.apa.org/record/2004-13697-016)
tity theory of [intergroup](https://psycnet.apa.org/record/2004-13697-016) behavior. In _Political_ _psy-_
_chology_, pages 276–293. Psychology Press.



14556


Yashar Talebirad and Amirhossein Nadiri. 2023. [Multi-](https://doi.org/10.48550/ARXIV.2306.03314)
agent collaboration: [Harnessing the power of intelli-](https://doi.org/10.48550/ARXIV.2306.03314)
[gent LLM agents.](https://doi.org/10.48550/ARXIV.2306.03314) _CoRR_, abs/2306.03314.


Hugo Touvron, Thibaut Lavril, Gautier Izacard, Xavier
Martinet, Marie-Anne Lachaux, Timothée Lacroix,
Baptiste Rozière, Naman Goyal, Eric Hambro,
Faisal Azhar, Aurélien Rodriguez, Armand Joulin,
Edouard Grave, and Guillaume Lample. 2023.
[Llama: Open and efficient foundation language mod-](https://doi.org/10.48550/ARXIV.2302.13971)
[els.](https://doi.org/10.48550/ARXIV.2302.13971) _CoRR_, abs/2302.13971.


José M. Vidal. 2006. _[Fundamentals of Multiagent Sys-](http://www.multiagent.com/fmas)_
_tems:_ _Using_ _[NetLogo](http://www.multiagent.com/fmas)_ _Models_ . Unpublished. [http:](http://www.multiagent.com)
[//www.multiagent.com.](http://www.multiagent.com)


Cheng Wang, Chuwen Wang, Yu Zhao, Shirong Zeng,
Wang Zhang, and Ronghui Ning. 2024a. [Behavioral](https://arxiv.org/abs/2401.09851)
simulation: Exploring a [possible](https://arxiv.org/abs/2401.09851) next paradigm for
[science.](https://arxiv.org/abs/2401.09851) _CoRR_, abs/2401.09851.


Haonan Wang, James Zou, Michael Mozer, Anirudh
Goyal, Alex Lamb, Linjun Zhang, Weijie J Su,
Zhun Deng, Michael Qizhe Xie, Hannah Brown, and
Kenji Kawaguchi. 2024b. Can ai be [as](https://arxiv.org/abs/2401.01623) creative as
[humans?](https://arxiv.org/abs/2401.01623) _CoRR_, abs/2401.01623.


Lei Wang, Chen Ma, Xueyang Feng, Zeyu Zhang, Hao
Yang, Jingsen Zhang, Zhiyuan Chen, Jiakai Tang,
Xu Chen, Yankai Lin, Wayne Xin Zhao, Zhewei Wei,
and Ji-Rong Wen. 2024c. A survey [on](https://doi.org/10.1007/s11704-024-40231-1) large lan[guage model based autonomous agents.](https://doi.org/10.1007/s11704-024-40231-1) _Front. Com-_
_put. Sci._, 18.


Zekun Wang, Ge Zhang, Kexin Yang, Ning Shi,
Wangchunshu Zhou, Shaochun Hao, Guangzheng
Xiong, Yizhi Li, Mong Yuan Sim, Xiuying Chen,
Qingqing Zhu, Zhenzhu Yang, Adam Nik, Qi Liu,
Chenghua Lin, Shi Wang, Ruibo Liu, Wenhu Chen,
Ke Xu, Dayiheng Liu, Yike Guo, and Jie Fu. 2023.
Interactive natural [language](https://doi.org/10.48550/arXiv.2305.13246) processing. _CoRR_,
abs/2305.13246.


Gerhard Weiß. 1995. [Adaptation and learning in multi-](https://doi.org/10.1007/3-540-60923-7_16)
agent systems: Some [remarks](https://doi.org/10.1007/3-540-60923-7_16) and a bibliography.
In _Adaption_ _and_ _Learning_ _in_ _Multi-Agent_ _Systems_,
volume 1042 of _Lecture Notes in Computer Science_,
pages 1–21. Springer.


Michael J. Wooldridge. 2009. _[An Introduction to Mul-](https://www.cs.ox.ac.uk/people/michael.wooldridge/pubs/imas/IMAS2e.html)_
_[tiAgent Systems, Second Edition](https://www.cs.ox.ac.uk/people/michael.wooldridge/pubs/imas/IMAS2e.html)_ . Wiley.


Anita Williams Woolley, Christopher F. Chabris, Alex
Pentland, Nada Hashmi, and Thomas W. Malone.
2010. Evidence for a [collective](https://doi.org/10.1126/science.1193147) intelligence factor in the [performance](https://doi.org/10.1126/science.1193147) of human groups. _Science_,
330(6004):686–688.


Zhiheng Xi, Wenxiang Chen, Xin Guo, Wei He, Yiwen
Ding, Boyang Hong, Ming Zhang, Junzhe Wang,
Senjie Jin, Enyu Zhou, Rui Zheng, Xiaoran Fan,
Xiao Wang, Limao Xiong, Yuhao Zhou, Weiran
Wang, Changhao Jiang, Yicheng Zou, Xiangyang
Liu, Zhangyue Yin, Shihan Dou, Rongxiang Weng,
Wensen Cheng, Qi Zhang, Wenjuan Qin, Yongyan
Zheng, Xipeng Qiu, Xuanjing Huan, and Tao Gui.



2023. The rise and [potential](https://doi.org/10.48550/arXiv.2309.07864) of large language
model based [agents:](https://doi.org/10.48550/arXiv.2309.07864) A survey. _arxiv_ _preprint_,
abs/2309.07864.


Yang Xiao, Yi Cheng, Jinlan Fu, Jiashuo Wang, Wenjie
Li, and Pengfei Liu. 2023. How far [are](https://arxiv.org/abs/2312.17115) we from
believable ai agents? [a framework for evaluating the](https://arxiv.org/abs/2312.17115)
believability of [human](https://arxiv.org/abs/2312.17115) behavior simulation. _CoRR_,
abs/2312.17115.


Chengxing Xie, Canyu Chen, Feiran Jia, Ziyu Ye, Kai
Shu, Adel Bibi, Ziniu Hu, Philip H. S. Torr, Bernard
Ghanem, and Guohao Li. 2024. Can [large](https://doi.org/10.48550/ARXIV.2402.04559) lan[guage model agents simulate human trust behaviors?](https://doi.org/10.48550/ARXIV.2402.04559)
_CoRR_, abs/2402.04559.


Tianbao Xie, Fan Zhou, Zhoujun Cheng, Peng Shi,
Luoxuan Weng, Yitao Liu, Toh Jing Hua, Junning
Zhao, Qian Liu, Che Liu, Leo Z. Liu, Yiheng Xu,
Hongjin Su, Dongchan Shin, Caiming Xiong, and
Tao Yu. 2023. Openagents: An [open](https://doi.org/10.48550/ARXIV.2310.10634) platform for
[language agents in the wild.](https://doi.org/10.48550/ARXIV.2310.10634) _CoRR_, abs/2310.10634.


Lin Xu, Zhiyuan Hu, Daquan Zhou, Hongyu Ren, Zhen
Dong, Kurt Keutzer, See-Kiong Ng, and Jiashi Feng.
2023. Magic: [Investigation of large language model](https://doi.org/10.48550/ARXIV.2311.08562)
[powered multi-agent in cognition, adaptability, ratio-](https://doi.org/10.48550/ARXIV.2311.08562)
[nality and collaboration.](https://doi.org/10.48550/ARXIV.2311.08562) _CoRR_, abs/2311.08562.


Ruoxi Xu, Yingfei Sun, Mengjie Ren, Shiguang Guo,
Ruotong Pan, Hongyu Lin, Le Sun, and Xianpei Han.
2024. [Ai for social science and social science of ai:](https://doi.org/https://doi.org/10.1016/j.ipm.2024.103665)
A [survey.](https://doi.org/https://doi.org/10.1016/j.ipm.2024.103665) _Information_ _Processing_ _&_ _Management_,
61(3):103665.


Shukang Yin, Chaoyou Fu, Sirui Zhao, Ke Li, Xing
Sun, Tong Xu, and Enhong Chen. 2023. [A survey on](https://doi.org/10.48550/arXiv.2306.13549)
multimodal large language models. _arXiv_ _preprint_,
abs/2306.13549.


Ceyao Zhang, Kaijie Yang, Siyi Hu, Zihao Wang,
Guanghe Li, Yihang Sun, Cheng Zhang, Zhaowei
Zhang, Anji Liu, Song-Chun Zhu, Xiaojun Chang,
Junge Zhang, Feng Yin, Yitao Liang, and Yaodong
Yang. 2024. Proagent: [Building](https://doi.org/10.1609/AAAI.V38I16.29710) proactive cooperative agents with [large](https://doi.org/10.1609/AAAI.V38I16.29710) language models. In _AAAI_,
pages 17591–17599. AAAI Press.


Xijia Zhang, Yue Guo, Simon Stepputtis, Katia P.
Sycara, and Joseph Campbell. 2023a. [Explaining](https://doi.org/10.48550/ARXIV.2309.10346)
agent behavior with [large](https://doi.org/10.48550/ARXIV.2309.10346) language models. _CoRR_,
abs/2309.10346.


Xijia Zhang, Yue Guo, Simon Stepputtis, Katia P.
Sycara, and Joseph Campbell. 2023b. [Understand-](https://doi.org/10.48550/ARXIV.2311.18062)
ing your agent: [Leveraging](https://doi.org/10.48550/ARXIV.2311.18062) large language models
[for behavior explanation.](https://doi.org/10.48550/ARXIV.2311.18062) _CoRR_, abs/2311.18062.


Yue Zhang, Yafu Li, Leyang Cui, Deng Cai, Lemao
Liu, Tingchen Fu, Xinting Huang, Enbo Zhao,
Yu Zhang, Yulong Chen, Longyue Wang, Anh Tuan
Luu, Wei Bi, Freda Shi, and Shuming Shi. 2023c.
Siren’s song in the [AI](https://doi.org/10.48550/ARXIV.2309.01219) ocean: A survey on
hallucination in [large](https://doi.org/10.48550/ARXIV.2309.01219) language models. _CoRR_,
abs/2309.01219.



14557


Wayne Xin Zhao, Kun Zhou, Junyi Li, Tianyi Tang, Xiaolei Wang, Yupeng Hou, Yingqian Min, Beichen
Zhang, Junjie Zhang, Zican Dong, Yifan Du, Chen
Yang, Yushuo Chen, Zhipeng Chen, Jinhao Jiang,
Ruiyang Ren, Yifan Li, Xinyu Tang, Zikang Liu,
Peiyu Liu, Jian-Yun Nie, and Ji-Rong Wen. 2023.
A survey of large language models. _arXiv_ _preprint_,
abs/2303.18223.


Wangchunshu Zhou, Yuchen Eleanor Jiang, Long Li,
Jialong Wu, Tiannan Wang, Shi Qiu, Jintian Zhang,
Jing Chen, Ruipu Wu, Shuai Wang, Shiding Zhu,
Jiyu Chen, Wentao Zhang, Ningyu Zhang, Huajun Chen, Peng Cui, and Mrinmaya Sachan. 2023.
[Agents: An open-source framework for autonomous](https://doi.org/10.48550/arXiv.2309.07870)
[language agents.](https://doi.org/10.48550/arXiv.2309.07870) _CoRR_, abs/2309.07870.


Yuqi Zhu, Xiaohan Wang, Jing Chen, Shuofei Qiao,
Yixin Ou, Yunzhi Yao, Shumin Deng, Huajun
Chen, and Ningyu Zhang. 2023. [Llms](https://doi.org/10.48550/arXiv.2305.13168) for
knowledge graph [construction](https://doi.org/10.48550/arXiv.2305.13168) and reasoning: Recent capabilities and future opportunities. _CoRR_,
abs/2305.13168.


Mingchen Zhuge, Haozhe Liu, Francesco Faccio, Dylan R. Ashley, Róbert Csordás, Anand Gopalakrishnan, Abdullah Hamdi, Hasan Abed Al Kader Hammoud, Vincent Herrmann, Kazuki Irie, Louis Kirsch,
Bing Li, Guohao Li, Shuming Liu, Jinjie Mai, Piotr Piekos, Aditya Ramesh, Imanol Schlag, Weimin
Shi, Aleksandar Stanic, Wenyi Wang, Yuhui Wang,
Mengmeng Xu, Deng-Ping Fan, Bernard Ghanem,
and Jürgen Schmidhuber. 2023. [Mindstorms](https://doi.org/10.48550/arXiv.2305.17066) in
natural [language-based](https://doi.org/10.48550/arXiv.2305.17066) societies of mind. _CoRR_,
abs/2305.17066.


14558


**Overview of Appendices**


We summarize the overview of Appendices below:
**§A:** Key Takeaways.
**§B:** Related Work.
**§C:** Potential Real-World Applications.
**§D:** Implementation Details.
Experimental Setup (§D.1)
Experimental Evaluation (§D.2)
Illustration of Agent Collaboration (§D.3)
**§E:** Further Analysis on Machine Social Collaboration (Backbone: ChatGPT).
**§F:** Analysis on Machine Society Settings (Backbone: ChatGPT).
**§G:** A Social Psychology View on Conformity,
Consensus Reaching, and Group Dynamics (Backbone: ChatGPT).
Conformity, Consensus Reaching (§G.1)
Group Dynamics (§G.2)
**§H:** Analysis on Different Backbone LLMs.
LlaMA2 Chat 13B (§H.1)
LlaMA2 Chat 70B (§H.2)
Qwen 72B (§H.3)
Mixtral 8 _×_ 7B (§H.4)
**§I:** Assessing the Effectiveness of Prompts.


**A** **Key Takeaways**


Drawing from our comprehensive analysis, we distill valuable insights for future multi-agent collaboration designs concerning _Strategy Selection_, _Soci-_
_ety Settings_, and _Social Psychology View_ .
Regarding _Strategy Selection_,


  - Starting or dominating multi-agent collaboration with debate, yields relatively optimal outcomes, as seen from Table 2, 8, 14, 20, 26, 32.


  - Totally-reflection strategy like _p_ 1 _p_ 1 _p_ 1 is generally worst in performance, as observed from
Table 2, 8, 14, 20, 26, 32.


  - For difficult tasks, debate combined with continuous reflection is superior; for simple tasks,
self-consistency or reflection is enough, as
seen from Figure 13, 24, 33, 42, 56.


Regarding _Society Settings_,


  - Surprisingly, “overconfident” agents lose that
trait in groups, as observed from word clouds
in Figure 11, 22, 31, 40, 54 and answer keeping in Figure 12, 23, 32, 41, 55!




  - Setting agent numbers to 3 is generally advantageous in performance and cost, as seen from
Figure 15, 25, 34, 43, 57.


  - The rounds of collaboration are relatively suitable to set as 3 since it’s both effective and efficient, as seen from Figure 18, 4, 19 on ChatGPT; Figure 26, 35 on LlaMA 13B/70B; Figure 47, 48, 49 on Qwen 72B; Figure 61, 62, 63
on Mixtral 8 _×_ 7B.

  - Employing the uniform thinking patterns
across all agents within a round enhance efficacy, as seen from Figure 5, 20, 27, 36, 50, 64.

Regarding _Social Psychology View_,

  - Collaboration is generally effective in the
group, especially for tackling difficult tasks,
as observed from Figure 13, 24, 33, 42, 56;
and Figure 21, 30, 39, 53, 67.


  - Collaboration widely leads to conformity, either beneficial or harmful in performance. As
observed from Figure 6, 28, 37, 51, 65.


  - As the number of rounds increases, the benefits of conformity will decrease, and the detriments of conformity will increase, as observed
from Figure 6, 28, 37, 51, 65.


  - The totally easy-going society is more likely
to reach a consensus, debate helps to consensus reaching while reflection impedes it, as
observed from Figure 16, 45, 59; and Figure 7, 29, 38, 52, 66.


**B** **Related Work**

**Multi-Agent** **Collaboration.** With the development of Large Language Models (LLMs) (Zhao
et al., 2023; Yin et al., 2023; Zhu et al., 2023),
study on LLM-based _agents_ (Wang et al., 2024c;
Xi et al., 2023; Gao et al., 2023a; Cheng et al.,
2024), has drawn considerable attention. Recently
there has been a proliferation of various agent systems, such as Generative Agents (Park et al., 2023),
MetaGPT (Hong et al., 2024), ProAgent (Zhang
et al., 2024), Agents (Zhou et al., 2023), OpenAgents (Xie et al., 2023), AutoAgents (Chen et al.,
2023a), MAgIC (Xu et al., 2023), AgentBoard (Ma
et al., 2024a), InterAct (Chen and Chang, 2023),
and AutoAct (Qiao et al., 2024). These works have
primarily focused on the elaborate design/evaluation of agent components, such as memory, environment, and planning. There are also some works



14559


exploring what kind of mindset can fully exploit
the comprehensive performance of the multi-agent
system (Guo et al., 2024; Pezeshkpour et al., 2024;
Du et al., 2024; Han et al., 2024), including _de-_
_bate_ (Du et al., 2023b; Liang et al., 2023) and _re-_
_flection_ (Shinn et al., 2023; Madaan et al., 2023).

AgentVerse (Chen et al., 2024) draws on the
above two types of work to explore the multi-agent
architecture and design two collaboration patterns:
_Horizonal Communication_ (similar to debate (Du
et al., 2023b; Liang et al., 2023)) and _Vertical Com-_
_munication_ (similar to self-refine (Madaan et al.,
2023)). These two collaboration patterns are included in our experiment framework. In addition, we have also explored a variety of other societies and collaborative strategies. Besides, there
are also some researches focusing on exploring
cooperation between agents constituted by different model compositions, such as ReConcile (Chen
et al., 2023c). Although we do not demonstrate this
kind of method, our work can easily expand to it.


**Human-Agent** **Simulation.** When the pretrained LLMs ( _e.g._, LLM-empowered agents) are
socially aligned (Duéñez-Guzmán et al., 2023; Liu
et al., 2024; Gao et al., 2023b), they could exhibit
human-like intelligence (Minsky, 1988; Singh,
2003; Zhuge et al., 2023; Li et al., 2023a; Xu et al.,
2024; Talebirad and Nadiri, 2023). Specifically,
agents can simulate human-like behaviors (Mei
et al., 2024; Wang et al., 2024a; Xiao et al., 2023;
Li et al., 2023b; Zhang et al., 2023b,a; Chuang
et al., 2023; Chuang and Rogers, 2023; Crouse
et al., 2023; Xie et al., 2024; Liang et al., 2024),
play roles like humans (Shanahan et al., 2023; Hou
et al., 2023; He et al., 2023), and even collaborate
with humans (Fuchs et al., 2023; Gao et al., 2024;
Feng et al., 2024; Alberts et al., 2024).

Notably, multi-agent collaboration can echo human society phenomena or theories in a social psychology view (Binz and Schulz, 2023; Demszky
et al., 2023; Hagendorff, 2023; Kuribayashi et al.,
2024), such as _conformity_ (Cialdini and Goldstein,
2004; Allen and Levine, 1969; Coultas and van
Leeuwen, 2015), _consensus reaching_ (Scheff, 1967;
Degroot, 1974; Baronchelli, 2018), _group dynamics_
(Cartwright and Zander, 1968; Alderfer, 1987; Seal
et al., 1998; Forsyth, 2014; Bion, 2018; Forsyth,
2018) and _social science_ (Gilbert and Terna, 2000;
Epstein, 2012; Flache et al., 2017; Lorenz et al.,
2021; Smaldino, 2023; Lanctot et al., 2023).



**C** **Potential Real-world Applications**

In this section, we present some potential applications (Ke et al., 2024) of our work, which could
benefit from the LLM agents’ ability to collaborate
effectively, similar to how human collaboration is
enriched inspired by social psychology.

  - **Social** **Research** : LLM agents can be used
to simulate social interactions to study phenomena like conformity, leadership, or group
decision-making.


  - **Negotiation** **and** **Mediation** : LLMs could
simulate multiple parties in a negotiation so
that offering fair solutions based on social psychology principles.


  - **AI Ethics and Governance** : By understanding the dynamics of social behaviors, LLM
agents could help in forming guidelines for
AI ethics, ensuring AI systems are developed
and deployed responsibly.


  - **Advanced** **Team** **Collaboration** **Tools** : By
understanding social dynamics, LLM agents
could facilitate better team collaboration, suggesting initiatives, mediating discussions, and
optimizing workflow.


  - **Intelligent Tutoring Systems** : Collaborative
LLM agents could personalize education by
interacting with students in a more human-like
manner, adapting to individual learning styles
and requirements.


  - **Healthcare** **Coordination** : LLM agents
could collaborate to provide care advice,
cross-referencing patient data, and medical
knowledge to assist healthcare professionals.


  - **Crisis** **Management** : During emergencies,
LLM agents could work together to analyze
data, manage communications, and provide
real-time information to the public.


  - **Content** **Creation** : Collaborative LLMs
could produce complex content, such as
scripts or articles, by dividing tasks based on
different expertise areas or writing styles.


  - **Interactive** **Entertainment** : In gaming and
virtual reality, LLM agents could provide
more dynamic and responsive narratives, by
collaborating to adapt the storyline to the players’ actions and intentions.



14560


Experiment Collaboration Number of
Model Dataset Society
Type Round Agents



Different
Number
of Agents


Different
Collboration
Rounds


Different
Strategy



gpt-3.5-turbo-1106 See the
Mixtral 8x7B Chess Move Validity 3 2 _∼_ 10 Figure 15
Qwen 72B and Table 11.



gpt-3.5-turbo-1106
LlaMA-13B-Chat
LlaMA-70B-Chat
Mixtral 8x7B
Qwen 72B



MMLU
MATH 3 3 _S_ 2
Chess Move Validity



LlaMA-13B-Chat MMLU
3 2 4
LlaMA-70B-Chat Chess Move Validity _∼_



Only one
easy-going
agent in
the society



gpt-3.5-turbo-1106
Mixtral 8x7B
Qwen 72B



MMLU
MATH 10 3 _S_ 2
Chess Move Validity



LlaMA-13B-Chat MMLU
4 3 _S_ 2
LlaMA-70B-Chat Chess Move Validity



Table 3: The detailed society settings in the three different experiments mentioned in Section 3.2.



**D** **Implementation Details**


**D.1** **Experimental Setup**


Model Temperature Top K Top P

gpt-3.5-turbo-1106 0.00  - 1.00
LlaMA2 Chat 13B 0.75 50 0.95
LlaMA2 Chat 70B 0.75 50 0.95
Mixtral 8 _×_ 7B 0.75 50 0.95
Qwen 72B 0.75 50 0.80


Table 4: Decoding parameters of different models.


The detailed society settings of the experiments
in §3.2 are shown in Table 3. Due to the context
length constraints of the LlaMA2 Chat 13B and
LlaMA2 Chat 70B, which support a maximum of
4096 tokens, it’s challenging to scale up the number
of agents and the rounds of collaboration. Consequently, we have capped the collaboration rounds
at 4 and also restricted the maximum agent number
to 4. We select MMLU and Chess Move Validity datasets in our analysis. Nevertheless, a small
fraction of cases still exceed the maximum length
constraint. To address this, we strategically prune
content from the earlier rounds to ensure compliance with the length limitation. As for other LLMs
(ChatGPT, Mixtral 8 _×_ 7B, and Qwen 72B), in terms
of experiments on the number of agents, adding an
additional agent results in substantial costs. This
is due to the necessity of conducting 5 replicate
experiments and accommodating 8 collaborative
strategies. Therefore, our experiments on these
LLMs are carried out on the less token-intensive
dataset: Chess Move Validity. As for trials con


cerning the rounds of collaboration, the quantity of
viable collaborative strategies increases exponentially with each additional round – for instance, 10
rounds would yield 2 [10] unique strategies. Considering the complexity, we analyze on 8 strategies that
are representative of the broader set of possibilities.
The decoding parameters for various models are
detailed in Table 4. In gpt-3.5-turbo-1106,
we align our approach with Du et al. (2023b) by
setting the temperature to 0, while adhering to the
default settings for the remaining parameters. For
_Qwen_ _72B_, we utilize the default parameters as
furnished by the official documentation. For the
remaining models, we configure the temperature to
0.7 and respectively adjust the Top P and Top K
values to 50 and 0.95. This configuration is primarily based on insights from Demszky et al. (2023),
which advocates for the recognition and integration
of the inherent stochastic nature of LLM outputs
into analytical frameworks, in a manner akin to the
treatment of stochastic variables in psychological
studies. It is noteworthy that even with the temperature parameter set to 0, gpt-3.5-turbo-1106
may still exhibit randomness in the outputs.
The prompts used in our experiments are shown
in Table 5. On the MMLU dataset, we curated
questions from 6 domains (statistics, mathematics,
computer science, biology, chemistry, and physics)
and performed a random sampling of 50 samples,
maintaining a proportion of 8 : 8 : 8 : 8 : 9 :
9 for each domain. On the MATH dataset, we
randomly selected 50 cases from Level 3, 4, and 5,
distributing them in a ratio of 22 : 22 : 6. On the



14561


|Task|Type|Prompt|
|---|---|---|
|Math<br>easy-going<br>_You are an expert skilled in solving mathematical problems and are objective_<br>_and unbiased, and you can be persuaded if other agent’s answers make sense._<br>_Please keep this in mind. If you understand please say ok only._<br>overconﬁdent<br>_Imagine you are an expert in solving mathematical problems and are conﬁdent_<br>_in your answer and often persuades other agents to believe in you. Please keep_<br>_this in mind. If you understand please say ok only._<br>question<br>_Here is a math problem written in LaTeX: <problem>\n Please carefully_<br>_consider it and explain your reasoning. Put your answer in the form_<br>_\boxed{{answer}}, at the end of your response._<br>debate<br>_These are the solutions to the problem from other agents:_<br>_<other agent responses>Using the reasoning from other agents as_<br>_additional information and referring to your historical answers, can_<br>_you give an updated answer? Put your answer in the form \boxed{{answer}},_<br>_at the end of your response._<br>reﬂection<br>_Can you double check that your answer is correct? Please reiterate your_<br>_answer, with your answer in the form \boxed{{answer}}, at the end of_<br>_your response._|easy-going|_You are an expert skilled in solving mathematical problems and are objective_<br>_and unbiased, and you can be persuaded if other agent’s answers make sense._<br>_Please keep this in mind. If you understand please say ok only._|
|Math<br>easy-going<br>_You are an expert skilled in solving mathematical problems and are objective_<br>_and unbiased, and you can be persuaded if other agent’s answers make sense._<br>_Please keep this in mind. If you understand please say ok only._<br>overconﬁdent<br>_Imagine you are an expert in solving mathematical problems and are conﬁdent_<br>_in your answer and often persuades other agents to believe in you. Please keep_<br>_this in mind. If you understand please say ok only._<br>question<br>_Here is a math problem written in LaTeX: <problem>\n Please carefully_<br>_consider it and explain your reasoning. Put your answer in the form_<br>_\boxed{{answer}}, at the end of your response._<br>debate<br>_These are the solutions to the problem from other agents:_<br>_<other agent responses>Using the reasoning from other agents as_<br>_additional information and referring to your historical answers, can_<br>_you give an updated answer? Put your answer in the form \boxed{{answer}},_<br>_at the end of your response._<br>reﬂection<br>_Can you double check that your answer is correct? Please reiterate your_<br>_answer, with your answer in the form \boxed{{answer}}, at the end of_<br>_your response._|overconﬁdent|_Imagine you are an expert in solving mathematical problems and are conﬁdent_<br>_in your answer and often persuades other agents to believe in you. Please keep_<br>_this in mind. If you understand please say ok only._|
|Math<br>easy-going<br>_You are an expert skilled in solving mathematical problems and are objective_<br>_and unbiased, and you can be persuaded if other agent’s answers make sense._<br>_Please keep this in mind. If you understand please say ok only._<br>overconﬁdent<br>_Imagine you are an expert in solving mathematical problems and are conﬁdent_<br>_in your answer and often persuades other agents to believe in you. Please keep_<br>_this in mind. If you understand please say ok only._<br>question<br>_Here is a math problem written in LaTeX: <problem>\n Please carefully_<br>_consider it and explain your reasoning. Put your answer in the form_<br>_\boxed{{answer}}, at the end of your response._<br>debate<br>_These are the solutions to the problem from other agents:_<br>_<other agent responses>Using the reasoning from other agents as_<br>_additional information and referring to your historical answers, can_<br>_you give an updated answer? Put your answer in the form \boxed{{answer}},_<br>_at the end of your response._<br>reﬂection<br>_Can you double check that your answer is correct? Please reiterate your_<br>_answer, with your answer in the form \boxed{{answer}}, at the end of_<br>_your response._|question|_Here is a math problem written in LaTeX: <problem>\n Please carefully_<br>_consider it and explain your reasoning. Put your answer in the form_<br>_\boxed{{answer}}, at the end of your response._|
|Math<br>easy-going<br>_You are an expert skilled in solving mathematical problems and are objective_<br>_and unbiased, and you can be persuaded if other agent’s answers make sense._<br>_Please keep this in mind. If you understand please say ok only._<br>overconﬁdent<br>_Imagine you are an expert in solving mathematical problems and are conﬁdent_<br>_in your answer and often persuades other agents to believe in you. Please keep_<br>_this in mind. If you understand please say ok only._<br>question<br>_Here is a math problem written in LaTeX: <problem>\n Please carefully_<br>_consider it and explain your reasoning. Put your answer in the form_<br>_\boxed{{answer}}, at the end of your response._<br>debate<br>_These are the solutions to the problem from other agents:_<br>_<other agent responses>Using the reasoning from other agents as_<br>_additional information and referring to your historical answers, can_<br>_you give an updated answer? Put your answer in the form \boxed{{answer}},_<br>_at the end of your response._<br>reﬂection<br>_Can you double check that your answer is correct? Please reiterate your_<br>_answer, with your answer in the form \boxed{{answer}}, at the end of_<br>_your response._|debate|_These are the solutions to the problem from other agents:_<br>_<other agent responses>Using the reasoning from other agents as_<br>_additional information and referring to your historical answers, can_<br>_you give an updated answer? Put your answer in the form \boxed{{answer}},_<br>_at the end of your response._|
|Math<br>easy-going<br>_You are an expert skilled in solving mathematical problems and are objective_<br>_and unbiased, and you can be persuaded if other agent’s answers make sense._<br>_Please keep this in mind. If you understand please say ok only._<br>overconﬁdent<br>_Imagine you are an expert in solving mathematical problems and are conﬁdent_<br>_in your answer and often persuades other agents to believe in you. Please keep_<br>_this in mind. If you understand please say ok only._<br>question<br>_Here is a math problem written in LaTeX: <problem>\n Please carefully_<br>_consider it and explain your reasoning. Put your answer in the form_<br>_\boxed{{answer}}, at the end of your response._<br>debate<br>_These are the solutions to the problem from other agents:_<br>_<other agent responses>Using the reasoning from other agents as_<br>_additional information and referring to your historical answers, can_<br>_you give an updated answer? Put your answer in the form \boxed{{answer}},_<br>_at the end of your response._<br>reﬂection<br>_Can you double check that your answer is correct? Please reiterate your_<br>_answer, with your answer in the form \boxed{{answer}}, at the end of_<br>_your response._|reﬂection|_Can you double check that your answer is correct? Please reiterate your_<br>_answer, with your answer in the form \boxed{{answer}}, at the end of_<br>_your response._|
|MMLU<br>easy-going<br>_You are an expert in biology, chemistry, computer science, mathematics,_<br>_physics and are objective and unbiased, and you can be persuaded if other_<br>_agent’s answers make sense. Please keep this in mind. If you understand_<br>_please say ok only._<br>overconﬁdent<br>_Imagine you are an expert in biology, chemistry, computer science,_<br>_mathematics, physics and are conﬁdent in your answer and often persuades_<br>_other agents to believe in you. Please keep this in mind. If you_<br>_understand please say ok only._<br>question<br>_Can you answer the following question as accurately as possible? <Question>:_<br>_A) <A>, B) <B>, C) <C>, D) <D>Explain your answer, putting the answer in_<br>_the form (X) at the end of your response._<br>debate<br>_These are the solutions to the problem from other agents:_<br>_<other agent responses>Using the reasoning from other agents as additional_<br>_advice, can you give an updated answer? Examine your solution and that other_<br>_agents. Put your answer in the form (X) at the end of your response._<br>reﬂection<br>_Can you double check that your answer is correct. Put your ﬁnal answer in_<br>_the form (X) at the end of your response._|easy-going|_You are an expert in biology, chemistry, computer science, mathematics,_<br>_physics and are objective and unbiased, and you can be persuaded if other_<br>_agent’s answers make sense. Please keep this in mind. If you understand_<br>_please say ok only._|
|MMLU<br>easy-going<br>_You are an expert in biology, chemistry, computer science, mathematics,_<br>_physics and are objective and unbiased, and you can be persuaded if other_<br>_agent’s answers make sense. Please keep this in mind. If you understand_<br>_please say ok only._<br>overconﬁdent<br>_Imagine you are an expert in biology, chemistry, computer science,_<br>_mathematics, physics and are conﬁdent in your answer and often persuades_<br>_other agents to believe in you. Please keep this in mind. If you_<br>_understand please say ok only._<br>question<br>_Can you answer the following question as accurately as possible? <Question>:_<br>_A) <A>, B) <B>, C) <C>, D) <D>Explain your answer, putting the answer in_<br>_the form (X) at the end of your response._<br>debate<br>_These are the solutions to the problem from other agents:_<br>_<other agent responses>Using the reasoning from other agents as additional_<br>_advice, can you give an updated answer? Examine your solution and that other_<br>_agents. Put your answer in the form (X) at the end of your response._<br>reﬂection<br>_Can you double check that your answer is correct. Put your ﬁnal answer in_<br>_the form (X) at the end of your response._|overconﬁdent|_Imagine you are an expert in biology, chemistry, computer science,_<br>_mathematics, physics and are conﬁdent in your answer and often persuades_<br>_other agents to believe in you. Please keep this in mind. If you_<br>_understand please say ok only._|
|MMLU<br>easy-going<br>_You are an expert in biology, chemistry, computer science, mathematics,_<br>_physics and are objective and unbiased, and you can be persuaded if other_<br>_agent’s answers make sense. Please keep this in mind. If you understand_<br>_please say ok only._<br>overconﬁdent<br>_Imagine you are an expert in biology, chemistry, computer science,_<br>_mathematics, physics and are conﬁdent in your answer and often persuades_<br>_other agents to believe in you. Please keep this in mind. If you_<br>_understand please say ok only._<br>question<br>_Can you answer the following question as accurately as possible? <Question>:_<br>_A) <A>, B) <B>, C) <C>, D) <D>Explain your answer, putting the answer in_<br>_the form (X) at the end of your response._<br>debate<br>_These are the solutions to the problem from other agents:_<br>_<other agent responses>Using the reasoning from other agents as additional_<br>_advice, can you give an updated answer? Examine your solution and that other_<br>_agents. Put your answer in the form (X) at the end of your response._<br>reﬂection<br>_Can you double check that your answer is correct. Put your ﬁnal answer in_<br>_the form (X) at the end of your response._|question|_Can you answer the following question as accurately as possible? <Question>:_<br>_A) <A>, B) <B>, C) <C>, D) <D>Explain your answer, putting the answer in_<br>_the form (X) at the end of your response._|
|MMLU<br>easy-going<br>_You are an expert in biology, chemistry, computer science, mathematics,_<br>_physics and are objective and unbiased, and you can be persuaded if other_<br>_agent’s answers make sense. Please keep this in mind. If you understand_<br>_please say ok only._<br>overconﬁdent<br>_Imagine you are an expert in biology, chemistry, computer science,_<br>_mathematics, physics and are conﬁdent in your answer and often persuades_<br>_other agents to believe in you. Please keep this in mind. If you_<br>_understand please say ok only._<br>question<br>_Can you answer the following question as accurately as possible? <Question>:_<br>_A) <A>, B) <B>, C) <C>, D) <D>Explain your answer, putting the answer in_<br>_the form (X) at the end of your response._<br>debate<br>_These are the solutions to the problem from other agents:_<br>_<other agent responses>Using the reasoning from other agents as additional_<br>_advice, can you give an updated answer? Examine your solution and that other_<br>_agents. Put your answer in the form (X) at the end of your response._<br>reﬂection<br>_Can you double check that your answer is correct. Put your ﬁnal answer in_<br>_the form (X) at the end of your response._|debate|_These are the solutions to the problem from other agents:_<br>_<other agent responses>Using the reasoning from other agents as additional_<br>_advice, can you give an updated answer? Examine your solution and that other_<br>_agents. Put your answer in the form (X) at the end of your response._|
|MMLU<br>easy-going<br>_You are an expert in biology, chemistry, computer science, mathematics,_<br>_physics and are objective and unbiased, and you can be persuaded if other_<br>_agent’s answers make sense. Please keep this in mind. If you understand_<br>_please say ok only._<br>overconﬁdent<br>_Imagine you are an expert in biology, chemistry, computer science,_<br>_mathematics, physics and are conﬁdent in your answer and often persuades_<br>_other agents to believe in you. Please keep this in mind. If you_<br>_understand please say ok only._<br>question<br>_Can you answer the following question as accurately as possible? <Question>:_<br>_A) <A>, B) <B>, C) <C>, D) <D>Explain your answer, putting the answer in_<br>_the form (X) at the end of your response._<br>debate<br>_These are the solutions to the problem from other agents:_<br>_<other agent responses>Using the reasoning from other agents as additional_<br>_advice, can you give an updated answer? Examine your solution and that other_<br>_agents. Put your answer in the form (X) at the end of your response._<br>reﬂection<br>_Can you double check that your answer is correct. Put your ﬁnal answer in_<br>_the form (X) at the end of your response._|reﬂection|_Can you double check that your answer is correct. Put your ﬁnal answer in_<br>_the form (X) at the end of your response._|
|Chess<br>Move<br>Validity<br>easy-going<br>_You are an expert skilled in playing chess and are objective and unbiased,_<br>_and you can be persuaded if other agent’s answers make sense. Please keep_<br>_this in mind. If you understand, please say ok only._<br>overconﬁdent<br>_Imagine you are an expert skilled in playing chess and are conﬁdent in_<br>_your answer and often persuades other agents to believe in you. Please keep_<br>_this in mind. If you understand, please say ok only._<br>question<br>_Given the chess game <chess move>, give one valid destination square for_<br>_the chess piece at <square>. Give a one-line explanation of why your_<br>_destination square is a valid move. State your ﬁnal answer in a newline with a_<br>_2 letter response following the regex [a-h][1-8]._<br>debate<br>_Here are destination square suggestions from other agents:_<br>_Can you double check that your destination square is a valid move? Check the_<br>_valid move justiﬁcations from other agents and your historical answers. State_<br>_your ﬁnal answer in a newline with a 2-letter response following the regex_<br>_[a-h][1-8]._<br>reﬂection<br>_Can you double check that your destination square is a valid move? Check the_<br>_valid move justiﬁcations from your historical answers. State your ﬁnal_<br>_answer in a newline with a 2 letter response following the regex [a-h][1-8]._|easy-going|_You are an expert skilled in playing chess and are objective and unbiased,_<br>_and you can be persuaded if other agent’s answers make sense. Please keep_<br>_this in mind. If you understand, please say ok only._|
|Chess<br>Move<br>Validity<br>easy-going<br>_You are an expert skilled in playing chess and are objective and unbiased,_<br>_and you can be persuaded if other agent’s answers make sense. Please keep_<br>_this in mind. If you understand, please say ok only._<br>overconﬁdent<br>_Imagine you are an expert skilled in playing chess and are conﬁdent in_<br>_your answer and often persuades other agents to believe in you. Please keep_<br>_this in mind. If you understand, please say ok only._<br>question<br>_Given the chess game <chess move>, give one valid destination square for_<br>_the chess piece at <square>. Give a one-line explanation of why your_<br>_destination square is a valid move. State your ﬁnal answer in a newline with a_<br>_2 letter response following the regex [a-h][1-8]._<br>debate<br>_Here are destination square suggestions from other agents:_<br>_Can you double check that your destination square is a valid move? Check the_<br>_valid move justiﬁcations from other agents and your historical answers. State_<br>_your ﬁnal answer in a newline with a 2-letter response following the regex_<br>_[a-h][1-8]._<br>reﬂection<br>_Can you double check that your destination square is a valid move? Check the_<br>_valid move justiﬁcations from your historical answers. State your ﬁnal_<br>_answer in a newline with a 2 letter response following the regex [a-h][1-8]._|overconﬁdent|_Imagine you are an expert skilled in playing chess and are conﬁdent in_<br>_your answer and often persuades other agents to believe in you. Please keep_<br>_this in mind. If you understand, please say ok only._|
|Chess<br>Move<br>Validity<br>easy-going<br>_You are an expert skilled in playing chess and are objective and unbiased,_<br>_and you can be persuaded if other agent’s answers make sense. Please keep_<br>_this in mind. If you understand, please say ok only._<br>overconﬁdent<br>_Imagine you are an expert skilled in playing chess and are conﬁdent in_<br>_your answer and often persuades other agents to believe in you. Please keep_<br>_this in mind. If you understand, please say ok only._<br>question<br>_Given the chess game <chess move>, give one valid destination square for_<br>_the chess piece at <square>. Give a one-line explanation of why your_<br>_destination square is a valid move. State your ﬁnal answer in a newline with a_<br>_2 letter response following the regex [a-h][1-8]._<br>debate<br>_Here are destination square suggestions from other agents:_<br>_Can you double check that your destination square is a valid move? Check the_<br>_valid move justiﬁcations from other agents and your historical answers. State_<br>_your ﬁnal answer in a newline with a 2-letter response following the regex_<br>_[a-h][1-8]._<br>reﬂection<br>_Can you double check that your destination square is a valid move? Check the_<br>_valid move justiﬁcations from your historical answers. State your ﬁnal_<br>_answer in a newline with a 2 letter response following the regex [a-h][1-8]._|question|_Given the chess game <chess move>, give one valid destination square for_<br>_the chess piece at <square>. Give a one-line explanation of why your_<br>_destination square is a valid move. State your ﬁnal answer in a newline with a_<br>_2 letter response following the regex [a-h][1-8]._|
|Chess<br>Move<br>Validity<br>easy-going<br>_You are an expert skilled in playing chess and are objective and unbiased,_<br>_and you can be persuaded if other agent’s answers make sense. Please keep_<br>_this in mind. If you understand, please say ok only._<br>overconﬁdent<br>_Imagine you are an expert skilled in playing chess and are conﬁdent in_<br>_your answer and often persuades other agents to believe in you. Please keep_<br>_this in mind. If you understand, please say ok only._<br>question<br>_Given the chess game <chess move>, give one valid destination square for_<br>_the chess piece at <square>. Give a one-line explanation of why your_<br>_destination square is a valid move. State your ﬁnal answer in a newline with a_<br>_2 letter response following the regex [a-h][1-8]._<br>debate<br>_Here are destination square suggestions from other agents:_<br>_Can you double check that your destination square is a valid move? Check the_<br>_valid move justiﬁcations from other agents and your historical answers. State_<br>_your ﬁnal answer in a newline with a 2-letter response following the regex_<br>_[a-h][1-8]._<br>reﬂection<br>_Can you double check that your destination square is a valid move? Check the_<br>_valid move justiﬁcations from your historical answers. State your ﬁnal_<br>_answer in a newline with a 2 letter response following the regex [a-h][1-8]._|debate|_Here are destination square suggestions from other agents:_<br>_Can you double check that your destination square is a valid move? Check the_<br>_valid move justiﬁcations from other agents and your historical answers. State_<br>_your ﬁnal answer in a newline with a 2-letter response following the regex_<br>_[a-h][1-8]._|
|Chess<br>Move<br>Validity<br>easy-going<br>_You are an expert skilled in playing chess and are objective and unbiased,_<br>_and you can be persuaded if other agent’s answers make sense. Please keep_<br>_this in mind. If you understand, please say ok only._<br>overconﬁdent<br>_Imagine you are an expert skilled in playing chess and are conﬁdent in_<br>_your answer and often persuades other agents to believe in you. Please keep_<br>_this in mind. If you understand, please say ok only._<br>question<br>_Given the chess game <chess move>, give one valid destination square for_<br>_the chess piece at <square>. Give a one-line explanation of why your_<br>_destination square is a valid move. State your ﬁnal answer in a newline with a_<br>_2 letter response following the regex [a-h][1-8]._<br>debate<br>_Here are destination square suggestions from other agents:_<br>_Can you double check that your destination square is a valid move? Check the_<br>_valid move justiﬁcations from other agents and your historical answers. State_<br>_your ﬁnal answer in a newline with a 2-letter response following the regex_<br>_[a-h][1-8]._<br>reﬂection<br>_Can you double check that your destination square is a valid move? Check the_<br>_valid move justiﬁcations from your historical answers. State your ﬁnal_<br>_answer in a newline with a 2 letter response following the regex [a-h][1-8]._|reﬂection|_Can you double check that your destination square is a valid move? Check the_<br>_valid move justiﬁcations from your historical answers. State your ﬁnal_<br>_answer in a newline with a 2 letter response following the regex [a-h][1-8]._|


Table 5: Prompts in each task.


14562


Chess Move Validity dataset, we similarly selected
50 samples for testing.


**D.2** **Experimental Evaluation**

The evaluation process involves two fundamental
steps: ( _i_ ) A unified answer is selected from the
machine society. To achieve this, we employ the
majority vote method to ascertain the consensus
reached by the society after multiple rounds of collaboration. If the unanimity among agents is not
achieved, it will be considered as an error. Additionally, if an individual agent provides multiple answers without following our prompts, its response
will be disregarded. ( _ii_ ) Answer responses from
agents are matched against the ground truth. This
step presents two main challenges. Firstly, there is
the concern of non-compliance with instructions.
Despite providing explicit prompts and specifying
the desired output format for evaluation, it’s inevitable that agents may occasionally deviate from
the given instructions. Secondly, the answers may
manifest in non-unique forms, leading to potential
variations, such as the equivalence between “3 _/_ 4”
and “0 _._ 75” in the MATH (Hendrycks et al., 2021b)
dataset. To address these challenges, a comprehensive set of matching rules is employed. Nonetheless, it is important to acknowledge the possibility
of encountering a small number of values that fall
outside the purview of these rules.


**D.3** **Illustration of Multi-Agent Collaboration**

As seen from Figure 8, the conformity phenomenon
in multi-agent collaboration can be both beneficial
( _i.e._, changing the answer from wrong to correct)
and harmful ( _i.e._, changing the answer from correct
to wrong) in problem-solving.
We also illustrate the detailed conversation process for multi-agent collaboration in Figure 9 and
Figure 10, regarding the conformity phenomenon
presented in Figure 8.


**E** **Further Analysis on Machine Social**
**Collaboration (Backbone:** **ChatGPT)**


We conduct a rigorous **significance** **test** for the
main experiment in §3.1. Given our experimental design incorporating two key factors, namely
_collaborative strategy_ and _society_, we respectively
opt for a one-way analysis of variance. Before
delving into the analysis, we ensured that the data
adhered to a normal distribution and satisfied the assumption of homogeneity of variance. We present



Collaborative MMLU MATH Chess Move Validity
Strategy p-value p-value p-value

_p_ 0 _p_ 0 _p_ 0 0.079 0.274 0.004
_p_ 0 _p_ 0 _p_ 1 0.956 0.011 0.000
_p_ 0 _p_ 1 _p_ 0 0.120 0.003 0.009
_p_ 0 _p_ 1 _p_ 1 0.000 0.323 0.014
_p_ 1 _p_ 0 _p_ 0 0.000 0.027 0.000
_p_ 1 _p_ 0 _p_ 1 0.063 0.017 0.000
_p_ 1 _p_ 1 _p_ 0 0.000 0.300 0.000
_p_ 1 _p_ 1 _p_ 1 0.000 0.000 0.000


Table 6: One-Way ANOVA results for the impact of
society on accuracy with fixed collaborative strategy,
based on experiments from Table 2 using _ChatGPT_ .


MMLU MATH Chess Move Validity
Society p-value p-value p-value

_S_ 1 0.000 0.000 0.293
_S_ 2 - 0.000 _S_ 3 0.000 0.001 0.000
_S_ 4 0.000 0.000 0.000


Table 7: One-Way ANOVA results for the impact of
collaborative strategy on accuracy with fixed society,
based on experiments from Table 2 using _ChatGPT_ . ‘’: It doesn’t pass homogeneity test for variance.


the _p_ -values for society and collaborative strategy
across three datasets in Table 6, 7.
We then present the **main** **results** and **sig-**
**nificance** **tests** of societies andcollaborative
strategies **on** **ChatGPT** **(with** **the** **engine** **of**
**gpt-3.5-turbo** **employed** **between** **July** **10**
**and July 23, 2023)** in Table 8, 9, 10.
Notably, the _p_ -value of the collaborative strategy
(on ChatGPT, engine: gpt-3.5-turbo-1106;
gpt-3.5-turbo in July) is significantly below
the threshold of 0.05, indicating that collaborative strategies have substantial impact on performance. Besides, on the backbone LLM of ChatGPT, the _p_ -value of the society (with the engine
of gpt-3.5-turbo-1106) is smaller than 0.05
in 17 out 24 cases, in contrast, the _p_ -value of the
society (with the engine of gpt-3.5-turbo employed between July 10 and July 23, 2023) is larger
than 0.05 in 23 out 24 cases. Generally, this corroborates our earlier conclusion in §3.1, emphasizing
that the influence of collaborative strategies outweighs that of societies.
We also present the **word clouds** in Figure 11,
and **answer** **changing** **of** **agents** **with** **different**
**traits** in Figure 11, to reveal that indistinctive impact of 3-agent societies on performance. Furthermore, we demonstrate that the tasks with different



14563


(a) Case in MMLU using strategy p p p .0 0 1 (b) Case in Chess Move Validity using strategy p p p .1 0 0

Figure 8: The conformity phenomenon in multi-agent collaboration, seen from changes in the answers during the
process of solving a certain task with 3 agents in the society _S_ 4 (all agents are easy-going), using _ChatGPT_ .









































Figure 9: In the _S_ 4 society, three agents engage in a complete dialogue using the strategy _p_ 1 _p_ 0 _p_ 0 to predict the
next piece position given a chess game from the Chess Move Validity dataset, corresponding to Figure 8(b).


14564


Figure 10: In the _S_ 4 society, three agents engage in a complete dialogue using the strategy _p_ 0 _p_ 0 _p_ 1 to solve a
high-school chemical question from MMLU, corresponding to Figure 8(a).


14565


_**MMLU**_ _**Math**_ _**Chess Move Validity**_







three overconfident agents, while society _S_ 4 comprises three easy-going agents. We first manually curated a list of
task-relevant, high-frequency words. From this list, the top 50 words are selected to construct the word clouds.


Society _S_ 1 features three overconfident agents, while society _S_ 4 comprises three easy-going agents.


14566


|Col1|Metric<br>(Strategy)|Society|Collaborative Strategy<br>p0p0p0 p0p0p1 p0p1p0 p0p1p1 p1p0p0 p1p0p1 p1p1p0 p1p1p1|Metric (Society)<br>Cost ↓ W-T ↑|
|---|---|---|---|---|
|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>64.4_±_1.7<br>**66.4**_±_**2.2**<br>58.0_±_3.7<br>55.2_±_4.4<br>37.6_±_7.0<br>42.4_±_7.1<br>50.4_±_4.3<br>44.8_±_2.7<br>5050<br>5<br>_S_2<br>67.2_±_4.1<br>**67.6**_±_**7.1**<br>53.2_±_6.4<br>53.2_±_5.0<br>38.4_±_5.5<br>40.4_±_5.2<br>53.6_±_4.8<br>45.2_±_3.6<br>5076<br>2<br>_S_3<br>62.0_±_6.2<br>**67.6**_±_**3.8**<br>52.0_±_6.8<br>57.2_±_6.4<br>42.4_±_5.2<br>37.6_±_5.5<br>55.2_±_6.6<br>40.0_±_6.2<br>5073<br>**8**<br>_S_4<br>**64.8**_±_**4.4**<br>64.8_±_5.8<br>58.4_±_3.0<br>51.6_±_3.8<br>38.0_±_3.7<br>42.0_±_2.4<br>54.0_±_5.8<br>41.2_±_5.2<br>5080<br>5<br>Cost_ ↓_<br>All<br>7528<br>5957<br>5402<br>4374<br>5812<br>4215<br>4272<br>3001<br>-<br>W-T_ ↑_<br>All<br>-<br>**14**<br>2<br>3<br>0<br>0<br>1<br>0|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>64.4_±_1.7<br>**66.4**_±_**2.2**<br>58.0_±_3.7<br>55.2_±_4.4<br>37.6_±_7.0<br>42.4_±_7.1<br>50.4_±_4.3<br>44.8_±_2.7<br>5050<br>5<br>_S_2<br>67.2_±_4.1<br>**67.6**_±_**7.1**<br>53.2_±_6.4<br>53.2_±_5.0<br>38.4_±_5.5<br>40.4_±_5.2<br>53.6_±_4.8<br>45.2_±_3.6<br>5076<br>2<br>_S_3<br>62.0_±_6.2<br>**67.6**_±_**3.8**<br>52.0_±_6.8<br>57.2_±_6.4<br>42.4_±_5.2<br>37.6_±_5.5<br>55.2_±_6.6<br>40.0_±_6.2<br>5073<br>**8**<br>_S_4<br>**64.8**_±_**4.4**<br>64.8_±_5.8<br>58.4_±_3.0<br>51.6_±_3.8<br>38.0_±_3.7<br>42.0_±_2.4<br>54.0_±_5.8<br>41.2_±_5.2<br>5080<br>5<br>Cost_ ↓_<br>All<br>7528<br>5957<br>5402<br>4374<br>5812<br>4215<br>4272<br>3001<br>-<br>W-T_ ↑_<br>All<br>-<br>**14**<br>2<br>3<br>0<br>0<br>1<br>0|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>64.4_±_1.7<br>**66.4**_±_**2.2**<br>58.0_±_3.7<br>55.2_±_4.4<br>37.6_±_7.0<br>42.4_±_7.1<br>50.4_±_4.3<br>44.8_±_2.7<br>5050<br>5<br>_S_2<br>67.2_±_4.1<br>**67.6**_±_**7.1**<br>53.2_±_6.4<br>53.2_±_5.0<br>38.4_±_5.5<br>40.4_±_5.2<br>53.6_±_4.8<br>45.2_±_3.6<br>5076<br>2<br>_S_3<br>62.0_±_6.2<br>**67.6**_±_**3.8**<br>52.0_±_6.8<br>57.2_±_6.4<br>42.4_±_5.2<br>37.6_±_5.5<br>55.2_±_6.6<br>40.0_±_6.2<br>5073<br>**8**<br>_S_4<br>**64.8**_±_**4.4**<br>64.8_±_5.8<br>58.4_±_3.0<br>51.6_±_3.8<br>38.0_±_3.7<br>42.0_±_2.4<br>54.0_±_5.8<br>41.2_±_5.2<br>5080<br>5<br>Cost_ ↓_<br>All<br>7528<br>5957<br>5402<br>4374<br>5812<br>4215<br>4272<br>3001<br>-<br>W-T_ ↑_<br>All<br>-<br>**14**<br>2<br>3<br>0<br>0<br>1<br>0|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>64.4_±_1.7<br>**66.4**_±_**2.2**<br>58.0_±_3.7<br>55.2_±_4.4<br>37.6_±_7.0<br>42.4_±_7.1<br>50.4_±_4.3<br>44.8_±_2.7<br>5050<br>5<br>_S_2<br>67.2_±_4.1<br>**67.6**_±_**7.1**<br>53.2_±_6.4<br>53.2_±_5.0<br>38.4_±_5.5<br>40.4_±_5.2<br>53.6_±_4.8<br>45.2_±_3.6<br>5076<br>2<br>_S_3<br>62.0_±_6.2<br>**67.6**_±_**3.8**<br>52.0_±_6.8<br>57.2_±_6.4<br>42.4_±_5.2<br>37.6_±_5.5<br>55.2_±_6.6<br>40.0_±_6.2<br>5073<br>**8**<br>_S_4<br>**64.8**_±_**4.4**<br>64.8_±_5.8<br>58.4_±_3.0<br>51.6_±_3.8<br>38.0_±_3.7<br>42.0_±_2.4<br>54.0_±_5.8<br>41.2_±_5.2<br>5080<br>5<br>Cost_ ↓_<br>All<br>7528<br>5957<br>5402<br>4374<br>5812<br>4215<br>4272<br>3001<br>-<br>W-T_ ↑_<br>All<br>-<br>**14**<br>2<br>3<br>0<br>0<br>1<br>0|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>64.4_±_1.7<br>**66.4**_±_**2.2**<br>58.0_±_3.7<br>55.2_±_4.4<br>37.6_±_7.0<br>42.4_±_7.1<br>50.4_±_4.3<br>44.8_±_2.7<br>5050<br>5<br>_S_2<br>67.2_±_4.1<br>**67.6**_±_**7.1**<br>53.2_±_6.4<br>53.2_±_5.0<br>38.4_±_5.5<br>40.4_±_5.2<br>53.6_±_4.8<br>45.2_±_3.6<br>5076<br>2<br>_S_3<br>62.0_±_6.2<br>**67.6**_±_**3.8**<br>52.0_±_6.8<br>57.2_±_6.4<br>42.4_±_5.2<br>37.6_±_5.5<br>55.2_±_6.6<br>40.0_±_6.2<br>5073<br>**8**<br>_S_4<br>**64.8**_±_**4.4**<br>64.8_±_5.8<br>58.4_±_3.0<br>51.6_±_3.8<br>38.0_±_3.7<br>42.0_±_2.4<br>54.0_±_5.8<br>41.2_±_5.2<br>5080<br>5<br>Cost_ ↓_<br>All<br>7528<br>5957<br>5402<br>4374<br>5812<br>4215<br>4272<br>3001<br>-<br>W-T_ ↑_<br>All<br>-<br>**14**<br>2<br>3<br>0<br>0<br>1<br>0|
|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>64.4_±_1.7<br>**66.4**_±_**2.2**<br>58.0_±_3.7<br>55.2_±_4.4<br>37.6_±_7.0<br>42.4_±_7.1<br>50.4_±_4.3<br>44.8_±_2.7<br>5050<br>5<br>_S_2<br>67.2_±_4.1<br>**67.6**_±_**7.1**<br>53.2_±_6.4<br>53.2_±_5.0<br>38.4_±_5.5<br>40.4_±_5.2<br>53.6_±_4.8<br>45.2_±_3.6<br>5076<br>2<br>_S_3<br>62.0_±_6.2<br>**67.6**_±_**3.8**<br>52.0_±_6.8<br>57.2_±_6.4<br>42.4_±_5.2<br>37.6_±_5.5<br>55.2_±_6.6<br>40.0_±_6.2<br>5073<br>**8**<br>_S_4<br>**64.8**_±_**4.4**<br>64.8_±_5.8<br>58.4_±_3.0<br>51.6_±_3.8<br>38.0_±_3.7<br>42.0_±_2.4<br>54.0_±_5.8<br>41.2_±_5.2<br>5080<br>5<br>Cost_ ↓_<br>All<br>7528<br>5957<br>5402<br>4374<br>5812<br>4215<br>4272<br>3001<br>-<br>W-T_ ↑_<br>All<br>-<br>**14**<br>2<br>3<br>0<br>0<br>1<br>0|Cost_ ↓_|All|7528<br>5957<br>5402<br>4374<br>5812<br>4215<br>4272<br>3001|7528<br>5957<br>5402<br>4374<br>5812<br>4215<br>4272<br>3001|
|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>64.4_±_1.7<br>**66.4**_±_**2.2**<br>58.0_±_3.7<br>55.2_±_4.4<br>37.6_±_7.0<br>42.4_±_7.1<br>50.4_±_4.3<br>44.8_±_2.7<br>5050<br>5<br>_S_2<br>67.2_±_4.1<br>**67.6**_±_**7.1**<br>53.2_±_6.4<br>53.2_±_5.0<br>38.4_±_5.5<br>40.4_±_5.2<br>53.6_±_4.8<br>45.2_±_3.6<br>5076<br>2<br>_S_3<br>62.0_±_6.2<br>**67.6**_±_**3.8**<br>52.0_±_6.8<br>57.2_±_6.4<br>42.4_±_5.2<br>37.6_±_5.5<br>55.2_±_6.6<br>40.0_±_6.2<br>5073<br>**8**<br>_S_4<br>**64.8**_±_**4.4**<br>64.8_±_5.8<br>58.4_±_3.0<br>51.6_±_3.8<br>38.0_±_3.7<br>42.0_±_2.4<br>54.0_±_5.8<br>41.2_±_5.2<br>5080<br>5<br>Cost_ ↓_<br>All<br>7528<br>5957<br>5402<br>4374<br>5812<br>4215<br>4272<br>3001<br>-<br>W-T_ ↑_<br>All<br>-<br>**14**<br>2<br>3<br>0<br>0<br>1<br>0|<br>W-T_ ↑_|All|-<br>**14**<br>2<br>3<br>0<br>0<br>1<br>0|-<br>**14**<br>2<br>3<br>0<br>0<br>1<br>0|
|<br>MATH<br>Acc_ ↑_<br>_S_1<br>46.8_±_8.1<br>46.0_±_8.1<br>44.0_±_5.3<br>44.4_±_5.2<br>**50.0**_±_**5.8**<br>49.2_±_8.1<br>42.0_±_3.2<br>42.0_±_4.0<br>5816<br>17<br>_S_2<br>47.2_±_6.4<br>**54.0**_±_**2.4**<br>48.4_±_3.8<br>43.6_±_4.3<br>48.0_±_4.2<br>44.4_±_7.9<br>50.8_±_3.6<br>38.8_±_9.1<br>5844<br>**22**<br>_S_3<br>**50.8**_±_**4.8**<br>42.8_±_6.6<br>45.6_±_6.8<br>45.2_±_4.4<br>49.2_±_4.8<br>46.4_±_5.5<br>45.2_±_8.4<br>43.6_±_2.6<br>5837<br>9<br>_S_4<br>50.8_±_5.4<br>45.2_±_7.0<br>48.8_±_9.4<br>44.8_±_3.3<br>49.2_±_8.7<br>**51.2**_±_**2.3**<br>48.4_±_6.5<br>40.8_±_6.1<br>5834<br>18<br>Cost_ ↓_<br>All<br>6919<br>6302<br>6221<br>5667<br>6149<br>5645<br>5924<br>4807<br>-<br>W-T_ ↑_<br>All<br>-<br>10<br>10<br>9<br>**13**<br>10<br>10<br>4|<br>Acc_ ↑_|_S_1<br>_S_2<br>_S_3<br>_S_4|46.8_±_8.1<br>46.0_±_8.1<br>44.0_±_5.3<br>44.4_±_5.2<br>**50.0**_±_**5.8**<br>49.2_±_8.1<br>42.0_±_3.2<br>42.0_±_4.0<br>47.2_±_6.4<br>**54.0**_±_**2.4**<br>48.4_±_3.8<br>43.6_±_4.3<br>48.0_±_4.2<br>44.4_±_7.9<br>50.8_±_3.6<br>38.8_±_9.1<br>**50.8**_±_**4.8**<br>42.8_±_6.6<br>45.6_±_6.8<br>45.2_±_4.4<br>49.2_±_4.8<br>46.4_±_5.5<br>45.2_±_8.4<br>43.6_±_2.6<br>50.8_±_5.4<br>45.2_±_7.0<br>48.8_±_9.4<br>44.8_±_3.3<br>49.2_±_8.7<br>**51.2**_±_**2.3**<br>48.4_±_6.5<br>40.8_±_6.1|5816<br>17<br>5844<br>**22**<br>5837<br>9<br>5834<br>18|
|<br>MATH<br>Acc_ ↑_<br>_S_1<br>46.8_±_8.1<br>46.0_±_8.1<br>44.0_±_5.3<br>44.4_±_5.2<br>**50.0**_±_**5.8**<br>49.2_±_8.1<br>42.0_±_3.2<br>42.0_±_4.0<br>5816<br>17<br>_S_2<br>47.2_±_6.4<br>**54.0**_±_**2.4**<br>48.4_±_3.8<br>43.6_±_4.3<br>48.0_±_4.2<br>44.4_±_7.9<br>50.8_±_3.6<br>38.8_±_9.1<br>5844<br>**22**<br>_S_3<br>**50.8**_±_**4.8**<br>42.8_±_6.6<br>45.6_±_6.8<br>45.2_±_4.4<br>49.2_±_4.8<br>46.4_±_5.5<br>45.2_±_8.4<br>43.6_±_2.6<br>5837<br>9<br>_S_4<br>50.8_±_5.4<br>45.2_±_7.0<br>48.8_±_9.4<br>44.8_±_3.3<br>49.2_±_8.7<br>**51.2**_±_**2.3**<br>48.4_±_6.5<br>40.8_±_6.1<br>5834<br>18<br>Cost_ ↓_<br>All<br>6919<br>6302<br>6221<br>5667<br>6149<br>5645<br>5924<br>4807<br>-<br>W-T_ ↑_<br>All<br>-<br>10<br>10<br>9<br>**13**<br>10<br>10<br>4|Cost_ ↓_|All|6919<br>6302<br>6221<br>5667<br>6149<br>5645<br>5924<br>4807|6919<br>6302<br>6221<br>5667<br>6149<br>5645<br>5924<br>4807|
|<br>MATH<br>Acc_ ↑_<br>_S_1<br>46.8_±_8.1<br>46.0_±_8.1<br>44.0_±_5.3<br>44.4_±_5.2<br>**50.0**_±_**5.8**<br>49.2_±_8.1<br>42.0_±_3.2<br>42.0_±_4.0<br>5816<br>17<br>_S_2<br>47.2_±_6.4<br>**54.0**_±_**2.4**<br>48.4_±_3.8<br>43.6_±_4.3<br>48.0_±_4.2<br>44.4_±_7.9<br>50.8_±_3.6<br>38.8_±_9.1<br>5844<br>**22**<br>_S_3<br>**50.8**_±_**4.8**<br>42.8_±_6.6<br>45.6_±_6.8<br>45.2_±_4.4<br>49.2_±_4.8<br>46.4_±_5.5<br>45.2_±_8.4<br>43.6_±_2.6<br>5837<br>9<br>_S_4<br>50.8_±_5.4<br>45.2_±_7.0<br>48.8_±_9.4<br>44.8_±_3.3<br>49.2_±_8.7<br>**51.2**_±_**2.3**<br>48.4_±_6.5<br>40.8_±_6.1<br>5834<br>18<br>Cost_ ↓_<br>All<br>6919<br>6302<br>6221<br>5667<br>6149<br>5645<br>5924<br>4807<br>-<br>W-T_ ↑_<br>All<br>-<br>10<br>10<br>9<br>**13**<br>10<br>10<br>4|<br>W-T_ ↑_|All|-<br>10<br>10<br>9<br>**13**<br>10<br>10<br>4|-<br>10<br>10<br>9<br>**13**<br>10<br>10<br>4|
|<br>Chess Move Validity<br>Acc_ ↑_<br>_S_1<br>47.2_±_3.6<br>**47.6**_±_**5.2**<br>45.6_±_7.8<br>40.0_±_4.5<br>42.8_±_2.3<br>29.2_±_4.6<br>42.4_±_6.5<br>20.0_±_6.0<br>2927<br>**10**<br>_S_2<br>**48.4**_±_**5.0**<br>45.6_±_6.1<br>43.6_±_4.3<br>39.6_±_3.3<br>48.4_±_5.2<br>35.6_±_5.2<br>43.2_±_8.8<br>18.8_±_5.8<br>2930<br>6<br>_S_3<br>**49.6**_±_**5.5**<br>48.0_±_5.8<br>47.6_±_5.5<br>37.6_±_9.9<br>41.6_±_6.1<br>35.2_±_8.3<br>40.4_±_3.8<br>14.8_±_6.1<br>2947<br>6<br>_S_4<br>48.4_±_3.3<br>**49.6**_±_**4.6**<br>46.0_±_3.5<br>36.8_±_4.1<br>38.8_±_3.3<br>27.2_±_3.9<br>38.0_±_6.3<br>14.0_±_4.7<br>2959<br>5<br>Cost_ ↓_<br>All<br>3736<br>3169<br>3196<br>2627<br>3266<br>2714<br>2698<br>2123<br>-<br>W-T_ ↑_<br>All<br>-<br>**11**<br>6<br>1<br>5<br>0<br>4<br>0|<br>Acc_ ↑_|_S_1<br>_S_2<br>_S_3<br>_S_4|47.2_±_3.6<br>**47.6**_±_**5.2**<br>45.6_±_7.8<br>40.0_±_4.5<br>42.8_±_2.3<br>29.2_±_4.6<br>42.4_±_6.5<br>20.0_±_6.0<br>**48.4**_±_**5.0**<br>45.6_±_6.1<br>43.6_±_4.3<br>39.6_±_3.3<br>48.4_±_5.2<br>35.6_±_5.2<br>43.2_±_8.8<br>18.8_±_5.8<br>**49.6**_±_**5.5**<br>48.0_±_5.8<br>47.6_±_5.5<br>37.6_±_9.9<br>41.6_±_6.1<br>35.2_±_8.3<br>40.4_±_3.8<br>14.8_±_6.1<br>48.4_±_3.3<br>**49.6**_±_**4.6**<br>46.0_±_3.5<br>36.8_±_4.1<br>38.8_±_3.3<br>27.2_±_3.9<br>38.0_±_6.3<br>14.0_±_4.7|2927<br>**10**<br>2930<br>6<br>2947<br>6<br>2959<br>5|
|<br>Chess Move Validity<br>Acc_ ↑_<br>_S_1<br>47.2_±_3.6<br>**47.6**_±_**5.2**<br>45.6_±_7.8<br>40.0_±_4.5<br>42.8_±_2.3<br>29.2_±_4.6<br>42.4_±_6.5<br>20.0_±_6.0<br>2927<br>**10**<br>_S_2<br>**48.4**_±_**5.0**<br>45.6_±_6.1<br>43.6_±_4.3<br>39.6_±_3.3<br>48.4_±_5.2<br>35.6_±_5.2<br>43.2_±_8.8<br>18.8_±_5.8<br>2930<br>6<br>_S_3<br>**49.6**_±_**5.5**<br>48.0_±_5.8<br>47.6_±_5.5<br>37.6_±_9.9<br>41.6_±_6.1<br>35.2_±_8.3<br>40.4_±_3.8<br>14.8_±_6.1<br>2947<br>6<br>_S_4<br>48.4_±_3.3<br>**49.6**_±_**4.6**<br>46.0_±_3.5<br>36.8_±_4.1<br>38.8_±_3.3<br>27.2_±_3.9<br>38.0_±_6.3<br>14.0_±_4.7<br>2959<br>5<br>Cost_ ↓_<br>All<br>3736<br>3169<br>3196<br>2627<br>3266<br>2714<br>2698<br>2123<br>-<br>W-T_ ↑_<br>All<br>-<br>**11**<br>6<br>1<br>5<br>0<br>4<br>0|Cost_ ↓_|All|3736<br>3169<br>3196<br>2627<br>3266<br>2714<br>2698<br>2123|3736<br>3169<br>3196<br>2627<br>3266<br>2714<br>2698<br>2123|
|<br>Chess Move Validity<br>Acc_ ↑_<br>_S_1<br>47.2_±_3.6<br>**47.6**_±_**5.2**<br>45.6_±_7.8<br>40.0_±_4.5<br>42.8_±_2.3<br>29.2_±_4.6<br>42.4_±_6.5<br>20.0_±_6.0<br>2927<br>**10**<br>_S_2<br>**48.4**_±_**5.0**<br>45.6_±_6.1<br>43.6_±_4.3<br>39.6_±_3.3<br>48.4_±_5.2<br>35.6_±_5.2<br>43.2_±_8.8<br>18.8_±_5.8<br>2930<br>6<br>_S_3<br>**49.6**_±_**5.5**<br>48.0_±_5.8<br>47.6_±_5.5<br>37.6_±_9.9<br>41.6_±_6.1<br>35.2_±_8.3<br>40.4_±_3.8<br>14.8_±_6.1<br>2947<br>6<br>_S_4<br>48.4_±_3.3<br>**49.6**_±_**4.6**<br>46.0_±_3.5<br>36.8_±_4.1<br>38.8_±_3.3<br>27.2_±_3.9<br>38.0_±_6.3<br>14.0_±_4.7<br>2959<br>5<br>Cost_ ↓_<br>All<br>3736<br>3169<br>3196<br>2627<br>3266<br>2714<br>2698<br>2123<br>-<br>W-T_ ↑_<br>All<br>-<br>**11**<br>6<br>1<br>5<br>0<br>4<br>0|<br>W-T_ ↑_|All|-<br>**11**<br>6<br>1<br>5<br>0<br>4<br>0|-<br>**11**<br>6<br>1<br>5<br>0<br>4<br>0|



Table 8: The impact of 8 collaborative strategies on the performance of 3 datasets across distinct societies, using

exceeds the strategy _p_ 0 _p_ 0 _p_ 0 under the same collaborative strategy / society. The significances test on societies and
strategies are respectively shown in Table 9, 10.



Collaborative MMLU MATH Chess Move Validity
Strategy p-value p-value p-value

_p_ 0 _p_ 0 _p_ 0 0.350 0.618 0.866
_p_ 0 _p_ 0 _p_ 1 0.797 0.069 0.716
_p_ 0 _p_ 1 _p_ 0 0.162 0.631 0.726
_p_ 0 _p_ 1 _p_ 1 0.350 0.945 0.807
_p_ 1 _p_ 0 _p_ 0 0.501 0.964 0.025
_p_ 1 _p_ 0 _p_ 1 0.497 0.378 0.079
_p_ 1 _p_ 1 _p_ 0 0.562 0.135 0.614
_p_ 1 _p_ 1 _p_ 1 0.236 0.642 0.293


Table 9: One-Way ANOVA results for the impact of
society on accuracy with fixed collaborative strategy,
based on experiments from Table 8 using _ChatGPT in_
_July_ .


MMLU MATH Chess Move Validity
Society p-value p-value p-value

_S_ 1 0.000 0.346 0.000
_S_ 2 0.000 0.008 0.000
_S_ 3 0.000 0.388 0.000
_S_ 4 0.000 0.213 0.000


Table 10: One-Way ANOVA results for the impact of
collaborative strategy on accuracy with fixed society,
based on experiments in Table 8 on _ChatGPT in July_ .



subjects and difficulty display varying sensitivity
to collaborative strategies, as presented with **radar**
**maps** in Figure 13.


**F** **Analysis on Machine Society Settings**
**(Backbone:** **ChatGPT)**

In this section, we conduct **significance tests** for
the experiments outlined in §3.2. The chosen
method is one-way analysis of variance. Prior to
the analysis, we performed a check for homogeneity of variance, with only one entry in Table 13 deviating from the criteria. The significance tests for
the number of agents, the number of rounds, and
different collaborative strategies are respectively
detailed in Table 11, Table 12 and Table 13.
**Different** **Numbers** **of** **Agents.** According to
the results of the _p_ -values in Table 11, the conclusion in §3.2 is confirmed, namely, different number
of agents results in a significant correlation on performance. By integrating the results in Figure 3, it
becomes evident that the presence of three agents
is relatively optimal.
We also analyze the _consensus_ _reaching_ with
different numbers of agents, and present the results
in Figure 16, 17.
**Different** **Rounds** **of** **Collaboration.** As



14567


Figure 13: Illustration of different collaborative strategies impacting accuracy diversely on the tasks considering
varied _subjects_ and _difficulty_, using _ChatGPT_ . The symbol ‘ ’ represents that there is at least one collaborative
strategy whose accuracy is better than self-consistency, while the symbol ‘ ’ indicates that there is no collaborative
strategy whose accuracy is worse than self-consistency. Both of these symbols represent the accuracy of selfconsistency. The accuracy under each collaborative strategy is a summation within all 3-agent societies.



Collaborative _S_ 1 _′_ _S_ 2 _′_ _S_ 3 _′_ _S_ 4 _′_ _S_ 5 _′_
Strategy p-value p-value p-value p-value p-value

_p_ 0 _p_ 0 _p_ 0 0.000 0.000 0.000 0.000 0.000
_p_ 0 _p_ 0 _p_ 1 0.000 0.000 0.000 0.000 0.000
_p_ 0 _p_ 1 _p_ 0 0.002 0.015 0.006 0.000 0.000
_p_ 0 _p_ 1 _p_ 1 0.000 0.000 0.000 0.000 0.000
_p_ 1 _p_ 0 _p_ 0 0.000 0.000 0.000 0.000 0.000
_p_ 1 _p_ 0 _p_ 1 0.000  - 0.000 0.001 0.000
_p_ 1 _p_ 1 _p_ 0 0.000 0.000 0.000 0.000 0.000
_p_ 1 _p_ 1 _p_ 1 0.000 0.000 0.000 0.005 0.000


Table 11: One-way ANOVA analysis of results in Figure 15 (different numbers of agents), using _ChatGPT_ .
_′_
_S_ 1 [:] [One] [overconfident] [agent] [and] [the] [others] [are] [all]
_′_
easygoing. _S_ 2 [:] [One] [easygoing] [agent] [among] [predom-]
_′_
inantly overconfident agents. _S_ 3 [:] [Equal] [numbers] [of]
_′_
overconfident and easygoing agents. _S_ 4 [:] [Entirely easy-]
_′_
going agents. _S_ 5 [:] [Entirely overconfident agents.] [‘-’:] [It]
doesn’t pass homogeneity test for variance.


Collaborative MMLU MATH Chess Move Validity
Strategy p-value p-value p-value

_p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 0.030 0.323 0.000
_p_ 1 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 0.000 0.070 0.161
_p_ 0 _p_ 1 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 0.101 0.332 0.000
_p_ 1 _p_ 0 _p_ 1 _p_ 0 _p_ 1 _p_ 0 _p_ 1 _p_ 0 _p_ 1 _p_ 0 0.000 0.077 0.871
_p_ 0 _p_ 1 _p_ 0 _p_ 1 _p_ 0 _p_ 1 _p_ 0 _p_ 1 _p_ 0 _p_ 1 0.051 0.062 0.000
_p_ 1 _p_ 0 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 0.000 0.021 0.630
_p_ 0 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 0.431 0.176 0.063
_p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 0.000 0.000 0.027


Table 12: One-way ANOVA analysis of the results in
Figure 4, 18, 19 (different rounds), using _ChatGPT_ .



observed from Table 12, we find that the
impact of rounds significantly relies on
the employed collaborative strategy. For
MMLU and Chess Move Validity, collaborative strategies where _p_ -values _<_ 0 _._ 05 are
_p_ 0 _p_ 1 _p_ 1 _p_ 0 _, p_ 0 _p_ 1 _p_ 1 _p_ 1 _, p_ 1 _p_ 0 _p_ 1 _p_ 0 _, p_ 1 _p_ 0 _p_ 1 _p_ 1 and
_{_ _}_
_p_ 0 _p_ 1 _p_ 1 _p_ 0 _, p_ 0 _p_ 1 _p_ 1 _p_ 1 _, p_ 1 _p_ 0 _p_ 1 _p_ 1 _, p_ 1 _p_ 1 _p_ 0 _p_ 0 _, p_ 1 _p_ 1 _p_ 0
_{_
_p_ 1 _, p_ 1 _p_ 1 _p_ 1 _p_ 0 . We also increase the rounds of
_}_
collaboration, from 3 to 10, and present the results
in Figure 18, 19. We find that although there would
be some fluctuations in performance if we scale
up the round of collaboration, the outperformance
is not obvious enough. While increasing rounds
of collaboration will result in more consumption
of tokens, which is not economic. Thus we infer
that the 3-round collaboration is relatively optimal
considering both performance and cost.
Furthermore, as seen from Figure 7, the strategy
after a round of debate tends to yield fewer consensus clusters compared to the preceding rounds.
Conversely, the strategy subsequent with a round
of reflection at the same juncture will increase consensus clusters. Adding an extra round of debate
at this juncture, as the conclusions in §4, is not
anticipated to bring about a discernible enhancement in performance. This confirms the efficacy of
the _early-stopping mechanism_ implemented in Liu
et al. (2023), drawing inspiration from Byzantine
Consensus theory (Castro and Liskov, 1999).
Moreover, we scrutinize the consensus reaching of these strategies in three rounds where _p_ values are below 0.05, as shown in Figure 7. Also
seen from Figure 7 and Figure 18, 4, 19, it becomes apparent that these collaborative strategies
exhibit substantial fluctuations in consensus reaching, demonstrating notably low answer consistency.



14568


14569


|Col1|Col2|Col3|Col4|Col5|Col6|Col7|Col8|Col9|Col10|Col11|Col12|Col13|Col14|Col15|Col16|Col17|Col18|Col19|Col20|Col21|Col22|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|


|Col1|Col2|Col3|Col4|Col5|Col6|Col7|Col8|Col9|Col10|Col11|Col12|Col13|Col14|Col15|Col16|Col17|Col18|Col19|Col20|Col21|Col22|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|||||||||||||||||||||||


|Col1|Col2|Col3|Col4|Col5|Col6|Col7|Col8|Col9|Col10|Col11|Col12|Col13|Col14|Col15|Col16|Col17|Col18|Col19|Col20|Col21|Col22|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|||||||||||||||||||||||


|Col1|Col2|Col3|Col4|Col5|Col6|Col7|Col8|Col9|Col10|Col11|Col12|Col13|Col14|Col15|Col16|Col17|Col18|Col19|Col20|Col21|Col22|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|


|Col1|Col2|Col3|Col4|Col5|Col6|Col7|Col8|Col9|Col10|Col11|Col12|Col13|Col14|Col15|Col16|Col17|Col18|Col19|Col20|Col21|Col22|Col23|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
||||||||||||||||||||||||
|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|


|Col1|Col2|Col3|Col4|Col5|Col6|Col7|Col8|Col9|Col10|Col11|Col12|Col13|Col14|Col15|Col16|Col17|Col18|Col19|Col20|Col21|Col22|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|||||||||||||||||||||||


|Col1|Col2|Col3|Col4|Col5|Col6|Col7|Col8|Col9|Col10|Col11|Col12|Col13|Col14|Col15|Col16|Col17|Col18|Col19|Col20|Col21|Col22|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|m<br>(<br>)<br> ccuracy of_ different numbers (2∼10) of agents_ under 3-round collaborative strategies, using<br> nce test is shown in Table 11.|


14570


( ) m
Figure 16: Average quantity of _consensus clusters (unique answers among multiple agents)_ in _different societies_
with 2 _∼_ 10 agents under each round of 3-round collaborative strategies, using _ChatGPT_ .







Figure 17: Average ratio of _consensus_ _clusters_ _(unique_ _answers_ _among_ _multiple_ _agents)_ with _different_ _numbers_
_(2∼10) of agents_ under each round of 3-round collaborative strategies, using _ChatGPT_ .


14571


3
(p1)


3
(p1)







4
(p1)



3
(p0)


3
(p0)



70


60


70


60



3
(p1)


3
(p0)



4
(p1)



4
(p1)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



50


40


50


40



4
(p1)



4
(p0)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



65


60


55

3
(p0)


60


50


3
(p1)



4
(p0)



5
(p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)



45


40


35


70.0


67.5



4 5
(p0) (p0)



4 5
(p0) (p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)











5
(p0)



6
(p1)



7
(p0)



8
(p1)



9
(p0)



10
(p1)


10
(p1)



5
(p1)



6
(p0)



7
(p1)



8
(p0)



9
(p1)



10
(p1)


10
(p0)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



10
(p0)


10
(p1)



6
(p0)



7
(p0)



8
(p0)



9
(p0)



10
(p0)


10
(p0)



Figure 18: Accuracy of _different_ _(3_ _10)_ _rounds_ _of_ _collaboration_ within 3-agent society _S_ 2 (1 easy-going and 2
_∼_
overconfident agents) on MMLU, using _ChatGPT_ .





60


50


50


40



3
(p1)


3
(p0)







4
(p1)



3
(p1)


3
(p1)





3
(p0)


3
(p0)



4
(p1)



4
(p1)



5
(p1)



6
(p1)



7
(p1)



8 9
(p1) (p1)



55

50

45


55


50


45



4
(p1)



4
(p0)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



55


50


3
(p0)


55


50


45

3
(p1)



4
(p0)



5
(p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)



55


50


50


45



4
(p0)



4
(p0)



5
(p0)



5
(p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)











5
(p0)



6
(p1)



7
(p0)



8 9
(p1) (p0)



10
(p1)


10
(p1)



5
(p1)



6
(p0)



7
(p1)



8
(p0)



9
(p1)



10
(p1)


10
(p0)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



10
(p0)


10
(p1)



6
(p0)



7
(p0)



8
(p0)



9
(p0)



10
(p0)


10
(p0)



Figure 19: Accuracy of _different_ _(3_ _10)_ _rounds_ _of_ _collaboration_ within 3-agent society _S_ 2 (1 easy-going and 2
_∼_
overconfident agents) on Chess Move Validity, using _ChatGPT_ .



For _p_ 0 _p_ 0 _p_ 0 _p_ 0 on Chess Move Validity, although
continuous reflection results in a gradual increase
in the number of consensus clusters, a more stable
trend with smaller fluctuations renders it less sensitive to the rounds of collaboration. Conversely,
collaborative strategies where _p_ -values _>_ 0 _._ 05 often display higher levels of answer consistency.


Figure 20: The effect on the accuracy of whether all
agents in a society execute the same thinking pattern in
one round on MATH, using _ChatGPT_ . “All” and “Part”
respectively refer to all agents applying the same and
different thinking pattern(s) in one round. The significance test is shown in Table 13 at Appendix F.


**Other Collaborative Strategies.** We show the
results of all agents in a society executing the same
or inconsistent thinking pattern(s) at one round in
Figure 20. Seen from Table 13, we observe pronounced impacts of keeping a consistent thinking
pattern on Chess Move Validity, while its influence



Collaborative MMLU MATH Chess Move Validity
Strategy p-value p-value p-value

_p_ 0 _p_ 0 _p_ 0 0.402 0.856 0.147
_p_ 0 _p_ 0 _p_ 1 0.007 0.002 0.001
_p_ 0 _p_ 1 _p_ 0 0.550 0.641 0.002
_p_ 0 _p_ 1 _p_ 1  - 0.276 0.000
_p_ 1 _p_ 0 _p_ 0  - 0.051  _p_ 1 _p_ 0 _p_ 1  - 0.784 0.000
_p_ 1 _p_ 1 _p_ 0 0.014 0.294 0.172
_p_ 1 _p_ 1 _p_ 1 1.000 0.000 0.347


Table 13: One-way ANOVA analysis of the results of
Figure 5 (other collaborative strategies), using _Chat-_
_GPT_ . ‘-’: It doesn’t pass homogeneity test for variance.


on MMLU and MATH is less significant.


**G** **A Social Psychology View on**
**Conformity, Consensus Reaching, and**
**Group Dynamics**


**G.1** **Conformity and Consensus Reaching**

Figures 6, 28, 37, 65, and 51 illustrate the conformity. Figures 7, 28, 37, 65, and 51 illustrate the
consensus. This section provides a detailed explanation of the methodologies used to calculate both
conformity and consensus.
For conformity, we solely focus on agents actively engaging in debate, disregarding those in re


14572


(a-c) & (d-f) respectively show the token cost and accuracy of different strategies before and after 3-round collaboration. Figure (g-i) present the percentage of different behavioral features (mainly analyzed by the change of
answer correctness) (Zhang et al., 2023b,a) under different collaborative strategies. All results are summarized
across all societies. The results on other LLMs are shown in Figure 30, 39, 53, 67 at Appendix H.



flection during a given round. Let the answer of the
_i_ -th agent at _j_ -th round be denoted as _a_ - _i,j_ . For the
_k_ -th agent at� _j_ -th round, if “Frequency _{ai,j−_ 1 _|i ∈_

[1 _, n_ ] = _ak,j_ ”, we identify this as the occur_}_
rence of conformity by agent _k_ at _j_ -th round, where
Frequency( _·_ ) represents the most frequently given
answer (excluding instances where all answers occur only once, as such cases are considered as nonconformity). Additionally, we categorize the correctness of answers both before and after conformity into four cases, with ‘True’ denoting correct
and ‘False’ denoting incorrect.
For consensus, we examine the evolution of the
number of distinct answers ( _i.e._, consensus clusters) with increasing rounds of collaboration. Let
the answer of the _i_ -th agent at time _j_ be denoted
as _ai,j_ . For the _j_ -th round, consensus clusters is
defined as Set( _ai,j_ _i_ [1 _, n_ ] ), where Set( )
_∥_ _{_ _|_ _∈_ _}_ _∥_ _∥_ _·_ _∥_
represents the count of different answers. This
computational approach has been utilized in the
analysis presented in Figures 17, 16, 60, 59, 46, 45.


**G.2** **Group Dynamics**

We seek to elucidate how performance is impacted by group dynamics, _i.e._, the patterns of
interaction between group members and different
processes that may occur within a social group.



Diving into the intricacies of collaboration, each
agent generates four answers, including the initial answer without collaboration, as shown in
Figure 2(d). To determine the answer for each
round, we employ the majority vote (Cobbe et al.,
2021; Li et al., 2022). Given ‘ _T_ ’ and ‘ _F_ ’ respectively denoting a round that yields a correct and an incorrect answer, we could obtain
2 [4] =16 possible answer sequences over the four
rounds. We select 10 sequences [6] of them and categorize them into 3 groups: _**Correcting Mistakes**_
( _FFFT, FFTT, FTTT_ ), _**Changing Correct An-**_
_**swers**_ ( _TFFF, TTFF, TTTF_ ), and _**Wavering**_
_**Answers**_ ( _FTFT, FTTF, TFTF, TFFT_ ). Particularly, _**Wavering Answers**_ resemble model hallucination (Rawte et al., 2023; Zhang et al., 2023c; Ji
et al., 2023; Luo et al., 2024) due to the occurrence
of self-contradictory answers. Our categorization
is under society-agnostic collaborative strategies,
considering the performance variance between societies is negligible. From the results on ChatGPT
shown in Figure 21, and on other LLMs shown in
Appendix H, we summarize the following findings:
**(1)** **Debate-initial/dominant** **collaborative**

6The selected 10 sequences adhere to patterns: (1)

[ _F_ ] _i>_ 0[ _T_ ] _j>_ 0, _e.g._, _FFFT_ ; (2) [ _T_ ] _i>_ 0[ _F_ ] _j>_ 0, _e.g._, _TFFF_ ;
(3) [ _TF_ ] _i≥_ 0[ _FT_ ] _j≥_ 0, _e.g._, _FTFT_, where [ _·_ ] _i_, [ _·_ ] _j_ respectively denotes repetition for _i_, _j_ times.



14573


**strategies are generally effective.** As seen from
the red bars in Figure 21 30, 39, 53, 67(d-f), we
find that the collaborative strategies starting from
or dominant with debate _p_ 0 are more effective than
other, and mostly outperform self-consistency, even
though they cost more tokens (seen from blue bars).
**(2)** **Reflection** **experiences** **greater** **insta-**
**bility** **(a** **heightened** **risk** **of** **model** **hallucina-**
**tion).** As observed from the purple bars in Figure 21 30, 39, 53, 67(g-h), comparing _pipjp_ 0 &
_pipjp_ 1; _pip_ 0 _pj_ & _pip_ 1 _pj_, _pipjp_ 0 and _pip_ 0 _pj_ are
more likely to wavering answers than _pipjp_ 1 and
_pip_ 1 _pj_, demonstrating that reflection is more likely
to cause model hallucination than debate.


14574


**H** **Analysis on Different Backbone LLMs**


To make the findings in this paper more general, we
also implement all the experiments with some other
open-resource backbone LLMs, such as **LlaMA2**
**Chat** **13B** (Touvron et al., 2023), **LlaMA2** **Chat**
**70B** (Touvron et al., 2023), **Qwen 72B** (Bai et al.,
2023) and **Mixtral 8** _×_ **7B** (Jiang et al., 2023, 2024).

**H.1** **LlaMA2 Chat 13B**

**Analysis on Machine Social Collaboration.** We
present the **main results** and **significance tests** of
societies and strategies on LlaMA2 Chat 13B in
Table 14, 15, 16. We present the **word clouds** of
LlaMA2 Chat 13B in Figure 22, and **proportion**
**of** **agents** **with** **different** **traits** **keeping** **answers**
**in different societies** on LlaMA2 Chat 13B in Figure 23. Furthermore, we demonstrate that the tasks
with different subjects and difficulty display varying sensitivity to collaborative strategies, as presented with **radar maps** on LlaMA2 Chat 13B in
Figure 24.
**Analysis** **on** **Different** **Numbers** **of** **Agents.**
We present the significance test for different numbers of agents with LlaMA2 Chat 13B in Table 17.
We also show the performance varying from agent
numbers in Figure 25.
**Analysis on Different Rounds.** We present the
significance test for different rounds of collaboration with LlaMA2 Chat 13B in Table 18. We also
show the performance varying from collaboration
rounds in Figure 26.
**Analysis** **on** **Other** **Collaborative** **Strategies.**
We present the significance test for other collaborative strategies (executing the same or hybrid
thinking patterns in a certain round) with LlaMA2
Chat 13B in Table 19. We also show the performance varying from other strategies in Figure 27.
**A** **Social** **Psychology** **View** **on** **Conformity,**
**Consensus Reaching and Group Dynamics.** We
then show the variation of answer correctness in
the situation of conformity in Figure 28; and the
quantity of consensus clusters among 3-agent answers in Figure 29. We present group dynamics
reflected by different answer-changing behaviors
on LlaMA2 Chat 13B in Figure 30.


14575


|Col1|Metric<br>(Strategy)|Society|Collaborative Strategy<br>p0p0p0 p0p0p1 p0p1p0 p0p1p1 p1p0p0 p1p0p1 p1p1p0 p1p1p1|Metric (Society)<br>Cost ↓ W-T ↑|
|---|---|---|---|---|
|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>37.2_±_5.9<br>47.2_±_3.9<br>**48.4**_±_**3.9**<br>46.0_±_5.7<br>**47.2**_±_**2.3**<br>46.8_±_2.7<br>45.2_±_4.4<br>46.8_±_3.0<br>7447<br>35<br>_S_2<br>38.4_±_4.6<br>42.8_±_3.9<br>43.6_±_3.6<br>**45.2**_±_**3.6**<br>44.8_±_4.6<br>**47.2**_±_**3.9**<br>44.4_±_6.2<br>42.8_±_3.4<br>7413<br>33<br>_S_3<br>36.0_±_3.7<br>44.8_±_3.0<br>44.8_±_4.8<br>**46.4**_±_**1.7**<br>41.6_±_4.3<br>**46.4**_±_**2.2**<br>43.2_±_6.6<br>42.4_±_3.3<br>7370<br>33<br>_S_4<br>34.8_±_2.7<br>42.4_±_5.0<br>42.0_±_4.5<br>**44.0**_±_**2.8**<br>40.4_±_3.0<br>**43.6**_±_**3.9**<br>40.8_±_3.0<br>41.6_±_2.6<br>7423<br>35<br>Cost_ ↓_<br>All<br>11429<br>9476<br>8166<br>6419<br>8452<br>5734<br>5733<br>3900<br>-<br>W-T_ ↑_<br>All<br>-<br>**20**<br>**20**<br>**20**<br>18<br>**20**<br>19<br>19|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>37.2_±_5.9<br>47.2_±_3.9<br>**48.4**_±_**3.9**<br>46.0_±_5.7<br>**47.2**_±_**2.3**<br>46.8_±_2.7<br>45.2_±_4.4<br>46.8_±_3.0<br>7447<br>35<br>_S_2<br>38.4_±_4.6<br>42.8_±_3.9<br>43.6_±_3.6<br>**45.2**_±_**3.6**<br>44.8_±_4.6<br>**47.2**_±_**3.9**<br>44.4_±_6.2<br>42.8_±_3.4<br>7413<br>33<br>_S_3<br>36.0_±_3.7<br>44.8_±_3.0<br>44.8_±_4.8<br>**46.4**_±_**1.7**<br>41.6_±_4.3<br>**46.4**_±_**2.2**<br>43.2_±_6.6<br>42.4_±_3.3<br>7370<br>33<br>_S_4<br>34.8_±_2.7<br>42.4_±_5.0<br>42.0_±_4.5<br>**44.0**_±_**2.8**<br>40.4_±_3.0<br>**43.6**_±_**3.9**<br>40.8_±_3.0<br>41.6_±_2.6<br>7423<br>35<br>Cost_ ↓_<br>All<br>11429<br>9476<br>8166<br>6419<br>8452<br>5734<br>5733<br>3900<br>-<br>W-T_ ↑_<br>All<br>-<br>**20**<br>**20**<br>**20**<br>18<br>**20**<br>19<br>19|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>37.2_±_5.9<br>47.2_±_3.9<br>**48.4**_±_**3.9**<br>46.0_±_5.7<br>**47.2**_±_**2.3**<br>46.8_±_2.7<br>45.2_±_4.4<br>46.8_±_3.0<br>7447<br>35<br>_S_2<br>38.4_±_4.6<br>42.8_±_3.9<br>43.6_±_3.6<br>**45.2**_±_**3.6**<br>44.8_±_4.6<br>**47.2**_±_**3.9**<br>44.4_±_6.2<br>42.8_±_3.4<br>7413<br>33<br>_S_3<br>36.0_±_3.7<br>44.8_±_3.0<br>44.8_±_4.8<br>**46.4**_±_**1.7**<br>41.6_±_4.3<br>**46.4**_±_**2.2**<br>43.2_±_6.6<br>42.4_±_3.3<br>7370<br>33<br>_S_4<br>34.8_±_2.7<br>42.4_±_5.0<br>42.0_±_4.5<br>**44.0**_±_**2.8**<br>40.4_±_3.0<br>**43.6**_±_**3.9**<br>40.8_±_3.0<br>41.6_±_2.6<br>7423<br>35<br>Cost_ ↓_<br>All<br>11429<br>9476<br>8166<br>6419<br>8452<br>5734<br>5733<br>3900<br>-<br>W-T_ ↑_<br>All<br>-<br>**20**<br>**20**<br>**20**<br>18<br>**20**<br>19<br>19|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>37.2_±_5.9<br>47.2_±_3.9<br>**48.4**_±_**3.9**<br>46.0_±_5.7<br>**47.2**_±_**2.3**<br>46.8_±_2.7<br>45.2_±_4.4<br>46.8_±_3.0<br>7447<br>35<br>_S_2<br>38.4_±_4.6<br>42.8_±_3.9<br>43.6_±_3.6<br>**45.2**_±_**3.6**<br>44.8_±_4.6<br>**47.2**_±_**3.9**<br>44.4_±_6.2<br>42.8_±_3.4<br>7413<br>33<br>_S_3<br>36.0_±_3.7<br>44.8_±_3.0<br>44.8_±_4.8<br>**46.4**_±_**1.7**<br>41.6_±_4.3<br>**46.4**_±_**2.2**<br>43.2_±_6.6<br>42.4_±_3.3<br>7370<br>33<br>_S_4<br>34.8_±_2.7<br>42.4_±_5.0<br>42.0_±_4.5<br>**44.0**_±_**2.8**<br>40.4_±_3.0<br>**43.6**_±_**3.9**<br>40.8_±_3.0<br>41.6_±_2.6<br>7423<br>35<br>Cost_ ↓_<br>All<br>11429<br>9476<br>8166<br>6419<br>8452<br>5734<br>5733<br>3900<br>-<br>W-T_ ↑_<br>All<br>-<br>**20**<br>**20**<br>**20**<br>18<br>**20**<br>19<br>19|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>37.2_±_5.9<br>47.2_±_3.9<br>**48.4**_±_**3.9**<br>46.0_±_5.7<br>**47.2**_±_**2.3**<br>46.8_±_2.7<br>45.2_±_4.4<br>46.8_±_3.0<br>7447<br>35<br>_S_2<br>38.4_±_4.6<br>42.8_±_3.9<br>43.6_±_3.6<br>**45.2**_±_**3.6**<br>44.8_±_4.6<br>**47.2**_±_**3.9**<br>44.4_±_6.2<br>42.8_±_3.4<br>7413<br>33<br>_S_3<br>36.0_±_3.7<br>44.8_±_3.0<br>44.8_±_4.8<br>**46.4**_±_**1.7**<br>41.6_±_4.3<br>**46.4**_±_**2.2**<br>43.2_±_6.6<br>42.4_±_3.3<br>7370<br>33<br>_S_4<br>34.8_±_2.7<br>42.4_±_5.0<br>42.0_±_4.5<br>**44.0**_±_**2.8**<br>40.4_±_3.0<br>**43.6**_±_**3.9**<br>40.8_±_3.0<br>41.6_±_2.6<br>7423<br>35<br>Cost_ ↓_<br>All<br>11429<br>9476<br>8166<br>6419<br>8452<br>5734<br>5733<br>3900<br>-<br>W-T_ ↑_<br>All<br>-<br>**20**<br>**20**<br>**20**<br>18<br>**20**<br>19<br>19|
|<br>MATH<br>Acc_ ↑_<br>_S_1<br>5.2_±_2.3<br>**6.8**_±_**2.3**<br>**5.6**_±_**2.6**<br>**5.6**_±_**2.6**<br>4.8_±_3.0<br>4.4_±_1.7<br>5.6_±_3.9<br>3.2_±_1.1<br>8639<br>24<br>_S_2<br>5.2_±_3.6<br>5.2_±_3.4<br>6.0_±_2.0<br>**6.8**_±_**1.8**<br>6.0_±_0.0<br>**6.8**_±_**1.8**<br>**6.8**_±_**1.1**<br>4.8_±_1.1<br>8451<br>22<br>_S_3<br>**6.8**_±_**1.8**<br>**6.8**_±_**3.0**<br>6.8_±_3.4<br>6.0_±_2.8<br>5.2_±_1.8<br>5.2_±_1.8<br>6.0_±_3.7<br>3.6_±_1.7<br>8501<br>16<br>_S_4<br>4.8_±_2.3<br>**6.8**_±_**3.4**<br>**7.2**_±_**1.1**<br>5.6_±_2.2<br>5.6_±_1.7<br>5.2_±_2.3<br>5.2_±_3.6<br>4.0_±_1.4<br>8475<br>28<br>Cost_ ↓_<br>All<br>10655<br>9508<br>9501<br>7900<br>9319<br>7761<br>7800<br>5687<br>-<br>W-T_ ↑_<br>All<br>-<br>15<br>**16**<br>13<br>13<br>11<br>13<br>9|<br>Acc_ ↑_|_S_1<br>_S_2<br>_S_3<br>_S_4|5.2_±_2.3<br>**6.8**_±_**2.3**<br>**5.6**_±_**2.6**<br>**5.6**_±_**2.6**<br>4.8_±_3.0<br>4.4_±_1.7<br>5.6_±_3.9<br>3.2_±_1.1<br>5.2_±_3.6<br>5.2_±_3.4<br>6.0_±_2.0<br>**6.8**_±_**1.8**<br>6.0_±_0.0<br>**6.8**_±_**1.8**<br>**6.8**_±_**1.1**<br>4.8_±_1.1<br>**6.8**_±_**1.8**<br>**6.8**_±_**3.0**<br>6.8_±_3.4<br>6.0_±_2.8<br>5.2_±_1.8<br>5.2_±_1.8<br>6.0_±_3.7<br>3.6_±_1.7<br>4.8_±_2.3<br>**6.8**_±_**3.4**<br>**7.2**_±_**1.1**<br>5.6_±_2.2<br>5.6_±_1.7<br>5.2_±_2.3<br>5.2_±_3.6<br>4.0_±_1.4|8639<br>24<br>8451<br>22<br>8501<br>16<br>8475<br>28|
|<br>MATH<br>Acc_ ↑_<br>_S_1<br>5.2_±_2.3<br>**6.8**_±_**2.3**<br>**5.6**_±_**2.6**<br>**5.6**_±_**2.6**<br>4.8_±_3.0<br>4.4_±_1.7<br>5.6_±_3.9<br>3.2_±_1.1<br>8639<br>24<br>_S_2<br>5.2_±_3.6<br>5.2_±_3.4<br>6.0_±_2.0<br>**6.8**_±_**1.8**<br>6.0_±_0.0<br>**6.8**_±_**1.8**<br>**6.8**_±_**1.1**<br>4.8_±_1.1<br>8451<br>22<br>_S_3<br>**6.8**_±_**1.8**<br>**6.8**_±_**3.0**<br>6.8_±_3.4<br>6.0_±_2.8<br>5.2_±_1.8<br>5.2_±_1.8<br>6.0_±_3.7<br>3.6_±_1.7<br>8501<br>16<br>_S_4<br>4.8_±_2.3<br>**6.8**_±_**3.4**<br>**7.2**_±_**1.1**<br>5.6_±_2.2<br>5.6_±_1.7<br>5.2_±_2.3<br>5.2_±_3.6<br>4.0_±_1.4<br>8475<br>28<br>Cost_ ↓_<br>All<br>10655<br>9508<br>9501<br>7900<br>9319<br>7761<br>7800<br>5687<br>-<br>W-T_ ↑_<br>All<br>-<br>15<br>**16**<br>13<br>13<br>11<br>13<br>9|Cost_ ↓_|All|10655<br>9508<br>9501<br>7900<br>9319<br>7761<br>7800<br>5687|10655<br>9508<br>9501<br>7900<br>9319<br>7761<br>7800<br>5687|
|<br>Chess Move Validity<br>Acc_ ↑_<br>_S_1<br>**16.4**_±_**3.0**<br>7.2_±_3.0<br>**9.2**_±_**2.3**<br>2.8_±_1.8<br>8.8_±_3.0<br>4.8_±_2.3<br>9.2_±_4.4<br>2.0_±_2.8<br>3754<br>2<br>_S_2<br>**11.6**_±_**5.2**<br>8.0_±_1.4<br>10.8_±_4.2<br>2.8_±_1.8<br>**11.6**_±_**2.6**<br>6.0_±_3.2<br>10.8_±_5.0<br>3.6_±_2.6<br>3725<br>10<br>_S_3<br>**14.8**_±_**3.0**<br>8.4_±_4.8<br>10.0_±_4.2<br>5.2_±_1.1<br>**14.0**_±_**4.5**<br>6.8_±_3.0<br>9.6_±_6.2<br>2.8_±_3.0<br>3678<br>5<br>_S_4<br>**16.0**_±_**4.2**<br>6.8_±_2.7<br>**12.4**_±_**6.2**<br>4.0_±_2.5<br>10.0_±_4.2<br>7.2_±_6.7<br>10.0_±_3.2<br>4.0_±_2.5<br>3647<br>4<br>Cost_ ↓_<br>All<br>4889<br>4123<br>4061<br>3324<br>4045<br>3293<br>3292<br>2581<br>-<br>W-T_ ↑_<br>All<br>-<br>2<br>4<br>0<br>**7**<br>1<br>**7**<br>0|<br>Acc_ ↑_|_S_1<br>_S_2<br>_S_3<br>_S_4|**16.4**_±_**3.0**<br>7.2_±_3.0<br>**9.2**_±_**2.3**<br>2.8_±_1.8<br>8.8_±_3.0<br>4.8_±_2.3<br>9.2_±_4.4<br>2.0_±_2.8<br>**11.6**_±_**5.2**<br>8.0_±_1.4<br>10.8_±_4.2<br>2.8_±_1.8<br>**11.6**_±_**2.6**<br>6.0_±_3.2<br>10.8_±_5.0<br>3.6_±_2.6<br>**14.8**_±_**3.0**<br>8.4_±_4.8<br>10.0_±_4.2<br>5.2_±_1.1<br>**14.0**_±_**4.5**<br>6.8_±_3.0<br>9.6_±_6.2<br>2.8_±_3.0<br>**16.0**_±_**4.2**<br>6.8_±_2.7<br>**12.4**_±_**6.2**<br>4.0_±_2.5<br>10.0_±_4.2<br>7.2_±_6.7<br>10.0_±_3.2<br>4.0_±_2.5|3754<br>2<br>3725<br>10<br>3678<br>5<br>3647<br>4|
|<br>Chess Move Validity<br>Acc_ ↑_<br>_S_1<br>**16.4**_±_**3.0**<br>7.2_±_3.0<br>**9.2**_±_**2.3**<br>2.8_±_1.8<br>8.8_±_3.0<br>4.8_±_2.3<br>9.2_±_4.4<br>2.0_±_2.8<br>3754<br>2<br>_S_2<br>**11.6**_±_**5.2**<br>8.0_±_1.4<br>10.8_±_4.2<br>2.8_±_1.8<br>**11.6**_±_**2.6**<br>6.0_±_3.2<br>10.8_±_5.0<br>3.6_±_2.6<br>3725<br>10<br>_S_3<br>**14.8**_±_**3.0**<br>8.4_±_4.8<br>10.0_±_4.2<br>5.2_±_1.1<br>**14.0**_±_**4.5**<br>6.8_±_3.0<br>9.6_±_6.2<br>2.8_±_3.0<br>3678<br>5<br>_S_4<br>**16.0**_±_**4.2**<br>6.8_±_2.7<br>**12.4**_±_**6.2**<br>4.0_±_2.5<br>10.0_±_4.2<br>7.2_±_6.7<br>10.0_±_3.2<br>4.0_±_2.5<br>3647<br>4<br>Cost_ ↓_<br>All<br>4889<br>4123<br>4061<br>3324<br>4045<br>3293<br>3292<br>2581<br>-<br>W-T_ ↑_<br>All<br>-<br>2<br>4<br>0<br>**7**<br>1<br>**7**<br>0|Cost_ ↓_|All|4889<br>4123<br>4061<br>3324<br>4045<br>3293<br>3292<br>2581|4889<br>4123<br>4061<br>3324<br>4045<br>3293<br>3292<br>2581|



Table 14: The impact of eight different collaborative strategies on the performance of three datasets across distinct
societies ( _using_ _LlaMA2-chat-13B_ ). The significances test on societies and strategies are respectively shown in
Table 15, 16. The experiments of comparison with the single LLM agent is shown in Figure 30(a)-(f).


_**MMLU**_ _**Math**_ _**Chess Move Validity**_







Figure 22: Comparative word clouds on three datasets in societies _S_ 1 and _S_ 4, using _LlaMA2-13B-chat_ . Society _S_ 1
features three overconfident agents, while society _S_ 4 comprises three easy-going agents.



Collaborative MMLU MATH Chess Move Validity
Strategy p-value p-value p-value

_p_ 0 _p_ 0 _p_ 0 0.611 0.632 0.251
_p_ 0 _p_ 0 _p_ 1 0.252 0.791 0.854
_p_ 0 _p_ 1 _p_ 0 0.142 0.714 0.706
_p_ 0 _p_ 1 _p_ 1 0.755 0.839 0.164
_p_ 1 _p_ 0 _p_ 0 0.039 0.789 0.175
_p_ 1 _p_ 0 _p_ 1 0.318 0.277 0.809
_p_ 1 _p_ 1 _p_ 0 0.585 0.884 0.959
_p_ 1 _p_ 1 _p_ 1 0.071 0.310 0.672


Table 15: One-Way ANOVA results for the impact of
society on accuracy with fixed collaborative strategy,
based on experiments from Table 14 using _LlaMA2-_
_chat-13B_ .



MMLU MATH Chess Move Validity
Society p-value p-value p-value

_S_ 1 0.006 0.548 0.000
_S_ 2 0.129 0.664 0.000
_S_ 3 0.005 0.518 0.000
_S_ 4 0.009 0.490 0.001


Table 16: One-Way ANOVA results for the impact of
collaborative strategy on accuracy with fixed society,
based on experiments from Table 14 using _LlaMA-13B-_
_Chat_ .



14576


Figure 23: Proportion of agents with different traits keeping answers in societies _S_ 1 and _S_ 4, using _LlaMA2-13B-_
_chat_ . Society _S_ 1 features three overconfident agents, while society _S_ 4 comprises three easy-going agents.


14577


Figure 24: Illustration of different collaborative strategies impacting accuracy diversely on the tasks considering
varied _subjects_ and _difficulty_, using _LlaMA2-13B-chat_ . The symbol ‘ ’ represents that there is at least one collaborative strategy whose accuracy is better than self-consistency, while the symbol ‘ ’ indicates that there is no
collaborative strategy whose accuracy is worse than self-consistency. Both of these symbols represent the accuracy
of self-consistency. The accuracy under each collaborative strategy is a summation within all 3-agent societies.


The significance test is shown in Table 17.


_Round 1:_ _Round 2-4:_ _i_ _j_ _k_ _Round 1:_ _Round 2-4:_ _i_ _j_ _k_


Figure 26: Accuracy at round 2,3,4 within 4-round collaborative societies, where the thinking pattern of round 1 is
fixed ( _p_ 0 or _p_ 1), using _LlaMA2-13B-chat_ . The significance test is shown in Table 18.


Collaborative MMLU Chess Move Validity
Strategy p-value p-value

_p_ 0 _p_ 0 _p_ 0 0.186 0.001
_p_ 0 _p_ 0 _p_ 1 0.019 0.000
_p_ 0 _p_ 1 _p_ 0 0.175 0.000
_p_ 0 _p_ 1 _p_ 1 0.010 0.178
_p_ 1 _p_ 0 _p_ 0 0.023 0.001
_p_ 1 _p_ 0 _p_ 1 0.002 0.005
_p_ 1 _p_ 1 _p_ 0 0.098 0.005
_p_ 1 _p_ 1 _p_ 1 0.004 0.002


Table 17: One-way ANOVA analysis of the results in
Figure 25 (different numbers of agents), _using LlaMA2-_
_chat-13B_ .


14578


Collaborative MMLU Chess Move Validity
Strategy p-value p-value

_p_ 0 _p_ 0 _p_ 0 _p_ 0 0.000 0.361
_p_ 0 _p_ 0 _p_ 0 _p_ 1 0.111 0.598
_p_ 0 _p_ 0 _p_ 1 _p_ 0 0.082 0.335
_p_ 0 _p_ 0 _p_ 1 _p_ 1 0.529 0.076
_p_ 0 _p_ 1 _p_ 0 _p_ 0 0.293 0.176
_p_ 0 _p_ 1 _p_ 0 _p_ 1 0.641 0.259
_p_ 0 _p_ 1 _p_ 1 _p_ 0 0.536 0.026
_p_ 0 _p_ 1 _p_ 1 _p_ 1 0.812 0.052
_p_ 1 _p_ 0 _p_ 0 _p_ 0 0.010 0.629
_p_ 1 _p_ 0 _p_ 0 _p_ 1 0.547 0.029
_p_ 1 _p_ 0 _p_ 1 _p_ 0 0.749 0.055
_p_ 1 _p_ 0 _p_ 1 _p_ 1 0.600 0.007
_p_ 1 _p_ 1 _p_ 0 _p_ 0 0.605 0.009
_p_ 1 _p_ 1 _p_ 0 _p_ 1 0.988 0.012
_p_ 1 _p_ 1 _p_ 1 _p_ 0 0.889 0.097
_p_ 1 _p_ 1 _p_ 1 _p_ 1 0.742 0.884


Table 18: One-way ANOVA analysis of the results in
Figure 26 (different rounds), _using LlaMA2-chat-13B_ .


Collaborative MMLU MATH Chess Move Validity
Strategy p-value p-value p-value

_p_ 0 _p_ 0 _p_ 0 0.419 0.659 0.203
_p_ 0 _p_ 0 _p_ 1 0.441 1.000 0.141
_p_ 0 _p_ 1 _p_ 0 0.086 0.074 0.264
_p_ 0 _p_ 1 _p_ 1 0.001 0.161 0.347
_p_ 1 _p_ 0 _p_ 0 0.030  - 0.000
_p_ 1 _p_ 0 _p_ 1 0.003 0.004 0.380
_p_ 1 _p_ 1 _p_ 0 0.070 0.001 0.005
_p_ 1 _p_ 1 _p_ 1 0.169 0.008 0.128


Table 19: One-way ANOVA analysis of the results
in Figure 27 (other collaborative strategies), _using_
_LlaMA2-chat-13B_ .



MMLU



50


45


40


35



p0p0p0
All Part



0p0 p0p0p1

AllPart Part



0p1 p0p1p0

AllPart Part



1p0 p0p1p1

AllPart Part



1p1 p1p0p0

AllPart Part



0p0 p1p0p1

AllPart Part



0p1 p1p1p0

AllPart Part



1p0 p1p1p1

AllPart Part



MATH



8


6


4


2



p0p0p0
All Part



0p0 p0p0p1

AllPart Part



0p1 p0p1p0

AllPart Part



1p0 p0p1p1

AllPart Part



1p1 p1p0p0

AllPart Part



0p0 p1p0p1

AllPart Part



0p1 p1p1p0

AllPart Part



1p0 p1p1p1

AllPart Part



Chess Move Validity



15


10


5


0



p0p0p0
All Part



0p0 p0p0p1

AllPart Part



0p1 p0p1p0

AllPart Part



1p0 p0p1p1

AllPart Part



1p1 p1p0p0

AllPart Part



0p0 p1p0p1

AllPart Part



0p1 p1p1p0

AllPart Part



1p0 p1p1p1

AllPart Part



Figure 27: The effect on the accuracy of whether all
agents in society execute the same thinking pattern in
one round, using _LlaMA2-13B-chat_ . “All” and “Part”
refer to all agents applying the same thinking pattern
and different thinking patterns in one round respectively. The significance test is shown in Table 19.



14579


_brings about detriments_ : Ratio(False _→_ True + True _→_ True) _<_ Ratio(True _→_ False + False _→_ False).


Figure 29: Average quantity of _consensus_ _clusters_ _(i.e.,_ _unique_ _answers_ _among_ _multiple_ _agents)_ under different
rounds of collaboration with 3-round collaborative strategies, on _LlaMA2-13B-chat_ . _Smaller quantity of consensus_


Figure (a-c) & (d-f) respectively show the token cost and accuracy of different strategies before and after 3-round
collaboration. Figure (g-i) present the percentage of different behavioral features (mainly analyzed by the change
of answer correctness) (Zhang et al., 2023b,a) under different collaborative strategies. All results are summarized
across all societies.


14580


**H.2** **LlaMA2 Chat 70B**

**Analysis on Machine Social Collaboration.** We
present the **main results** and **significance tests** of
societies and strategies on LlaMA2 Chat 70B in
Table 20, 21, 22. We present the **word clouds** of
LlaMA2 Chat 70B in Figure 31, and **proportion**
**of** **agents** **with** **different** **traits** **keeping** **answers**
**in different societies** on LlaMA2 Chat 70B in Figure 32. Furthermore, we demonstrate that the tasks
with different subjects and difficulty display varying sensitivity to collaborative strategies, as presented with **radar maps** on LlaMA2 Chat 70B in
Figure 33.
**Analysis** **on** **Different** **Numbers** **of** **Agents.**
We present the significance test for different numbers of agents with LlaMA2 Chat 70B in Table 23.
We also show the performance varying from agent
numbers in Figure 34.
**Analysis on Different Rounds.** We present the
significance test for different rounds of collaboration with LlaMA2 Chat 70B in Table 24. We also
show the performance varying from collaboration
rounds in Figure 35.
**Analysis** **on** **Other** **Collaborative** **Strategies.**
We present the significance test for other collaborative strategies (executing the same or hybrid
thinking patterns in a certain round) with LlaMA2
Chat 70B in Table 25. We also show the performance varying from other strategies in Figure 36.
**A** **Social** **Psychology** **View** **on** **Conformity,**
**Consensus Reaching and Group Dynamics.** We
then show the variation of answer correctness in
the situation of conformity in Figure 37; and the
quantity of consensus clusters among 3-agent answers in Figure 38. We present group dynamics
reflected by different answer-changing behaviors
on LlaMA2 Chat 70B in Figure 39.


14581


|Col1|Metric<br>(Strategy)|Society|Collaborative Strategy<br>p0p0p0 p0p0p1 p0p1p0 p0p1p1 p1p0p0 p1p0p1 p1p1p0 p1p1p1|Metric (Society)<br>Cost ↓ W-T ↑|
|---|---|---|---|---|
|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>**40.8**_±_**2.7**<br>**43.6**_±_**3.9**<br>36.0_±_2.8<br>38.4_±_3.3<br>35.6_±_4.3<br>35.6_±_2.6<br>30.4_±_4.3<br>24.0_±_5.7<br>6915<br>7<br>_S_2<br>44.4_±_3.9<br>**49.2**_±_**4.6**<br>**45.2**_±_**3.9**<br>42.0_±_0.0<br>34.4_±_4.3<br>34.4_±_8.3<br>31.6_±_8.4<br>25.6_±_3.6<br>6946<br>11<br>_S_3<br>**44.0**_±_**5.5**<br>**45.6**_±_**4.6**<br>39.2_±_2.7<br>42.8_±_3.0<br>35.2_±_5.4<br>32.4_±_4.3<br>28.0_±_7.3<br>25.6_±_5.2<br>6931<br>8<br>_S_4<br>**47.6**_±_**4.1**<br>**48.0**_±_**5.1**<br>46.0_±_6.3<br>45.2_±_3.9<br>26.8_±_3.6<br>30.8_±_6.9<br>32.8_±_1.8<br>33.6_±_6.2<br>6936<br>8<br>Cost_ ↓_<br>All<br>10811<br>8608<br>7904<br>6177<br>7535<br>5410<br>5287<br>3722<br>-<br>W-T_ ↑_<br>All<br>-<br>**16**<br>5<br>11<br>1<br>0<br>1<br>0|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>**40.8**_±_**2.7**<br>**43.6**_±_**3.9**<br>36.0_±_2.8<br>38.4_±_3.3<br>35.6_±_4.3<br>35.6_±_2.6<br>30.4_±_4.3<br>24.0_±_5.7<br>6915<br>7<br>_S_2<br>44.4_±_3.9<br>**49.2**_±_**4.6**<br>**45.2**_±_**3.9**<br>42.0_±_0.0<br>34.4_±_4.3<br>34.4_±_8.3<br>31.6_±_8.4<br>25.6_±_3.6<br>6946<br>11<br>_S_3<br>**44.0**_±_**5.5**<br>**45.6**_±_**4.6**<br>39.2_±_2.7<br>42.8_±_3.0<br>35.2_±_5.4<br>32.4_±_4.3<br>28.0_±_7.3<br>25.6_±_5.2<br>6931<br>8<br>_S_4<br>**47.6**_±_**4.1**<br>**48.0**_±_**5.1**<br>46.0_±_6.3<br>45.2_±_3.9<br>26.8_±_3.6<br>30.8_±_6.9<br>32.8_±_1.8<br>33.6_±_6.2<br>6936<br>8<br>Cost_ ↓_<br>All<br>10811<br>8608<br>7904<br>6177<br>7535<br>5410<br>5287<br>3722<br>-<br>W-T_ ↑_<br>All<br>-<br>**16**<br>5<br>11<br>1<br>0<br>1<br>0|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>**40.8**_±_**2.7**<br>**43.6**_±_**3.9**<br>36.0_±_2.8<br>38.4_±_3.3<br>35.6_±_4.3<br>35.6_±_2.6<br>30.4_±_4.3<br>24.0_±_5.7<br>6915<br>7<br>_S_2<br>44.4_±_3.9<br>**49.2**_±_**4.6**<br>**45.2**_±_**3.9**<br>42.0_±_0.0<br>34.4_±_4.3<br>34.4_±_8.3<br>31.6_±_8.4<br>25.6_±_3.6<br>6946<br>11<br>_S_3<br>**44.0**_±_**5.5**<br>**45.6**_±_**4.6**<br>39.2_±_2.7<br>42.8_±_3.0<br>35.2_±_5.4<br>32.4_±_4.3<br>28.0_±_7.3<br>25.6_±_5.2<br>6931<br>8<br>_S_4<br>**47.6**_±_**4.1**<br>**48.0**_±_**5.1**<br>46.0_±_6.3<br>45.2_±_3.9<br>26.8_±_3.6<br>30.8_±_6.9<br>32.8_±_1.8<br>33.6_±_6.2<br>6936<br>8<br>Cost_ ↓_<br>All<br>10811<br>8608<br>7904<br>6177<br>7535<br>5410<br>5287<br>3722<br>-<br>W-T_ ↑_<br>All<br>-<br>**16**<br>5<br>11<br>1<br>0<br>1<br>0|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>**40.8**_±_**2.7**<br>**43.6**_±_**3.9**<br>36.0_±_2.8<br>38.4_±_3.3<br>35.6_±_4.3<br>35.6_±_2.6<br>30.4_±_4.3<br>24.0_±_5.7<br>6915<br>7<br>_S_2<br>44.4_±_3.9<br>**49.2**_±_**4.6**<br>**45.2**_±_**3.9**<br>42.0_±_0.0<br>34.4_±_4.3<br>34.4_±_8.3<br>31.6_±_8.4<br>25.6_±_3.6<br>6946<br>11<br>_S_3<br>**44.0**_±_**5.5**<br>**45.6**_±_**4.6**<br>39.2_±_2.7<br>42.8_±_3.0<br>35.2_±_5.4<br>32.4_±_4.3<br>28.0_±_7.3<br>25.6_±_5.2<br>6931<br>8<br>_S_4<br>**47.6**_±_**4.1**<br>**48.0**_±_**5.1**<br>46.0_±_6.3<br>45.2_±_3.9<br>26.8_±_3.6<br>30.8_±_6.9<br>32.8_±_1.8<br>33.6_±_6.2<br>6936<br>8<br>Cost_ ↓_<br>All<br>10811<br>8608<br>7904<br>6177<br>7535<br>5410<br>5287<br>3722<br>-<br>W-T_ ↑_<br>All<br>-<br>**16**<br>5<br>11<br>1<br>0<br>1<br>0|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>**40.8**_±_**2.7**<br>**43.6**_±_**3.9**<br>36.0_±_2.8<br>38.4_±_3.3<br>35.6_±_4.3<br>35.6_±_2.6<br>30.4_±_4.3<br>24.0_±_5.7<br>6915<br>7<br>_S_2<br>44.4_±_3.9<br>**49.2**_±_**4.6**<br>**45.2**_±_**3.9**<br>42.0_±_0.0<br>34.4_±_4.3<br>34.4_±_8.3<br>31.6_±_8.4<br>25.6_±_3.6<br>6946<br>11<br>_S_3<br>**44.0**_±_**5.5**<br>**45.6**_±_**4.6**<br>39.2_±_2.7<br>42.8_±_3.0<br>35.2_±_5.4<br>32.4_±_4.3<br>28.0_±_7.3<br>25.6_±_5.2<br>6931<br>8<br>_S_4<br>**47.6**_±_**4.1**<br>**48.0**_±_**5.1**<br>46.0_±_6.3<br>45.2_±_3.9<br>26.8_±_3.6<br>30.8_±_6.9<br>32.8_±_1.8<br>33.6_±_6.2<br>6936<br>8<br>Cost_ ↓_<br>All<br>10811<br>8608<br>7904<br>6177<br>7535<br>5410<br>5287<br>3722<br>-<br>W-T_ ↑_<br>All<br>-<br>**16**<br>5<br>11<br>1<br>0<br>1<br>0|
|<br>MATH<br>Acc_ ↑_<br>_S_1<br>8.4_±_3.6<br>**10.4**_±_**3.9**<br>**9.2**_±_**1.1**<br>4.0_±_2.5<br>9.2_±_4.2<br>8.4_±_4.3<br>6.8_±_2.7<br>3.6_±_1.7<br>7000<br>16<br>_S_2<br>8.0_±_2.5<br>**9.6**_±_**2.6**<br>**8.8**_±_**3.0**<br>6.4_±_2.6<br>7.2_±_4.4<br>6.8_±_1.1<br>8.4_±_4.3<br>4.8_±_2.3<br>7013<br>19<br>_S_3<br>**8.4**_±_**4.6**<br>7.2_±_3.9<br>**8.4**_±_**3.6**<br>5.6_±_3.6<br>7.2_±_1.8<br>7.2_±_4.8<br>6.8_±_3.0<br>0.8_±_1.1<br>7157<br>15<br>_S_4<br>6.0_±_2.0<br>**7.2**_±_**1.8**<br>6.0_±_2.0<br>4.0_±_2.0<br>5.2_±_3.0<br>6.8_±_1.1<br>**8.8**_±_**4.4**<br>3.6_±_2.6<br>6934<br>23<br>Cost_ ↓_<br>All<br>9465<br>7850<br>7662<br>6294<br>7520<br>6302<br>6382<br>4734<br>-<br>W-T_ ↑_<br>All<br>-<br>**14**<br>**14**<br>5<br>13<br>9<br>**14**<br>4|<br>Acc_ ↑_|_S_1<br>_S_2<br>_S_3<br>_S_4|8.4_±_3.6<br>**10.4**_±_**3.9**<br>**9.2**_±_**1.1**<br>4.0_±_2.5<br>9.2_±_4.2<br>8.4_±_4.3<br>6.8_±_2.7<br>3.6_±_1.7<br>8.0_±_2.5<br>**9.6**_±_**2.6**<br>**8.8**_±_**3.0**<br>6.4_±_2.6<br>7.2_±_4.4<br>6.8_±_1.1<br>8.4_±_4.3<br>4.8_±_2.3<br>**8.4**_±_**4.6**<br>7.2_±_3.9<br>**8.4**_±_**3.6**<br>5.6_±_3.6<br>7.2_±_1.8<br>7.2_±_4.8<br>6.8_±_3.0<br>0.8_±_1.1<br>6.0_±_2.0<br>**7.2**_±_**1.8**<br>6.0_±_2.0<br>4.0_±_2.0<br>5.2_±_3.0<br>6.8_±_1.1<br>**8.8**_±_**4.4**<br>3.6_±_2.6|7000<br>16<br>7013<br>19<br>7157<br>15<br>6934<br>23|
|<br>MATH<br>Acc_ ↑_<br>_S_1<br>8.4_±_3.6<br>**10.4**_±_**3.9**<br>**9.2**_±_**1.1**<br>4.0_±_2.5<br>9.2_±_4.2<br>8.4_±_4.3<br>6.8_±_2.7<br>3.6_±_1.7<br>7000<br>16<br>_S_2<br>8.0_±_2.5<br>**9.6**_±_**2.6**<br>**8.8**_±_**3.0**<br>6.4_±_2.6<br>7.2_±_4.4<br>6.8_±_1.1<br>8.4_±_4.3<br>4.8_±_2.3<br>7013<br>19<br>_S_3<br>**8.4**_±_**4.6**<br>7.2_±_3.9<br>**8.4**_±_**3.6**<br>5.6_±_3.6<br>7.2_±_1.8<br>7.2_±_4.8<br>6.8_±_3.0<br>0.8_±_1.1<br>7157<br>15<br>_S_4<br>6.0_±_2.0<br>**7.2**_±_**1.8**<br>6.0_±_2.0<br>4.0_±_2.0<br>5.2_±_3.0<br>6.8_±_1.1<br>**8.8**_±_**4.4**<br>3.6_±_2.6<br>6934<br>23<br>Cost_ ↓_<br>All<br>9465<br>7850<br>7662<br>6294<br>7520<br>6302<br>6382<br>4734<br>-<br>W-T_ ↑_<br>All<br>-<br>**14**<br>**14**<br>5<br>13<br>9<br>**14**<br>4|Cost_ ↓_|All|9465<br>7850<br>7662<br>6294<br>7520<br>6302<br>6382<br>4734|9465<br>7850<br>7662<br>6294<br>7520<br>6302<br>6382<br>4734|
|<br>Chess Move Validity<br>Acc_ ↑_<br>_S_1<br>**20.4**_±_**6.2**<br>16.8_±_3.6<br>17.2_±_4.2<br>8.4_±_2.2<br>**21.2**_±_**5.8**<br>10.8_±_3.0<br>10.4_±_1.7<br>4.8_±_3.0<br>3563<br>7<br>_S_2<br>**18.4**_±_**4.8**<br>9.6_±_3.6<br>13.2_±_1.1<br>5.6_±_2.2<br>**14.4**_±_**3.9**<br>7.2_±_3.0<br>13.2_±_3.4<br>4.0_±_2.8<br>3557<br>4<br>_S_3<br>**18.4**_±_**6.5**<br>11.2_±_3.0<br>12.0_±_5.8<br>8.0_±_2.0<br>**20.8**_±_**4.6**<br>8.4_±_4.3<br>12.8_±_2.7<br>2.8_±_3.4<br>3629<br>7<br>_S_4<br>15.2_±_4.2<br>11.6_±_2.2<br>**15.2**_±_**2.3**<br>10.4_±_1.7<br>**18.0**_±_**4.7**<br>8.0_±_4.7<br>10.8_±_2.7<br>5.2_±_2.3<br>3679<br>12<br>Cost_ ↓_<br>All<br>4778<br>3947<br>3830<br>3082<br>4139<br>3314<br>3259<br>2508<br>-<br>W-T_ ↑_<br>All<br>-<br>4<br>6<br>2<br>**13**<br>1<br>4<br>0|<br>Acc_ ↑_|_S_1<br>_S_2<br>_S_3<br>_S_4|**20.4**_±_**6.2**<br>16.8_±_3.6<br>17.2_±_4.2<br>8.4_±_2.2<br>**21.2**_±_**5.8**<br>10.8_±_3.0<br>10.4_±_1.7<br>4.8_±_3.0<br>**18.4**_±_**4.8**<br>9.6_±_3.6<br>13.2_±_1.1<br>5.6_±_2.2<br>**14.4**_±_**3.9**<br>7.2_±_3.0<br>13.2_±_3.4<br>4.0_±_2.8<br>**18.4**_±_**6.5**<br>11.2_±_3.0<br>12.0_±_5.8<br>8.0_±_2.0<br>**20.8**_±_**4.6**<br>8.4_±_4.3<br>12.8_±_2.7<br>2.8_±_3.4<br>15.2_±_4.2<br>11.6_±_2.2<br>**15.2**_±_**2.3**<br>10.4_±_1.7<br>**18.0**_±_**4.7**<br>8.0_±_4.7<br>10.8_±_2.7<br>5.2_±_2.3|3563<br>7<br>3557<br>4<br>3629<br>7<br>3679<br>12|
|<br>Chess Move Validity<br>Acc_ ↑_<br>_S_1<br>**20.4**_±_**6.2**<br>16.8_±_3.6<br>17.2_±_4.2<br>8.4_±_2.2<br>**21.2**_±_**5.8**<br>10.8_±_3.0<br>10.4_±_1.7<br>4.8_±_3.0<br>3563<br>7<br>_S_2<br>**18.4**_±_**4.8**<br>9.6_±_3.6<br>13.2_±_1.1<br>5.6_±_2.2<br>**14.4**_±_**3.9**<br>7.2_±_3.0<br>13.2_±_3.4<br>4.0_±_2.8<br>3557<br>4<br>_S_3<br>**18.4**_±_**6.5**<br>11.2_±_3.0<br>12.0_±_5.8<br>8.0_±_2.0<br>**20.8**_±_**4.6**<br>8.4_±_4.3<br>12.8_±_2.7<br>2.8_±_3.4<br>3629<br>7<br>_S_4<br>15.2_±_4.2<br>11.6_±_2.2<br>**15.2**_±_**2.3**<br>10.4_±_1.7<br>**18.0**_±_**4.7**<br>8.0_±_4.7<br>10.8_±_2.7<br>5.2_±_2.3<br>3679<br>12<br>Cost_ ↓_<br>All<br>4778<br>3947<br>3830<br>3082<br>4139<br>3314<br>3259<br>2508<br>-<br>W-T_ ↑_<br>All<br>-<br>4<br>6<br>2<br>**13**<br>1<br>4<br>0|Cost_ ↓_|All|4778<br>3947<br>3830<br>3082<br>4139<br>3314<br>3259<br>2508|4778<br>3947<br>3830<br>3082<br>4139<br>3314<br>3259<br>2508|



Table 20: The impact of eight different collaborative strategies on the performance of three datasets across distinct
societies ( _using_ _LlaMA2-chat-70B_ ). The significances test on societies and strategies are respectively shown in
Table 21, 22. The experiments of comparison with the single LLM agent is shown in Figure 39(a)-(f).


_**MMLU**_ _**Math**_ _**Chess Move Validity**_







Figure 31: Comparative word clouds on three datasets in societies _S_ 1 and _S_ 4, using _LlaMA2-70B-chat_ . Society _S_ 1
features three overconfident agents, while society _S_ 4 comprises three easy-going agents.



Collaborative MMLU MATH Chess Move Validity
Strategy p-value p-value p-value

_p_ 0 _p_ 0 _p_ 0 0.122 0.621 0.532
_p_ 0 _p_ 0 _p_ 1 0.251 0.291 0.014
_p_ 0 _p_ 1 _p_ 0 0.004 0.248 0.185
_p_ 0 _p_ 1 _p_ 1 0.018 0.430 0.015
_p_ 1 _p_ 0 _p_ 0 0.020 0.381 0.132
_p_ 1 _p_ 0 _p_ 1 0.601 0.854 0.506
_p_ 1 _p_ 1 _p_ 0 0.641 0.750 0.282
_p_ 1 _p_ 1 _p_ 1 0.044 0.037 0.585


Table 21: One-Way ANOVA results for the impact of
society on accuracy with fixed collaborative strategy,
based on experiments from Table 20 using _LlaMA2-_
_chat-70B_ .



MMLU MATH Chess Move Validity
Society p-value p-value p-value

_S_ 1 0.000 0.013 0.000
_S_ 2 0.000 0.297 0.000
_S_ 3 0.000 0.040 0.000
_S_ 4 0.000 0.056 0.000


Table 22: One-Way ANOVA results for the impact of
collaborative strategy on accuracy with fixed society,
based on experiments from Table 20 using _LlaMA-70B-_
_Chat_ .



14582


Figure 32: Proportion of agents with different traits keeping answers in societies _S_ 1 and _S_ 4, using _LlaMA2-70B-_
_chat_ . Society _S_ 1 features three overconfident agents, while society _S_ 4 comprises three easy-going agents.


14583


Figure 33: Illustration of different collaborative strategies impacting accuracy diversely on the tasks considering
varied _subjects_ and _difficulty_, using _LlaMA2-70B-chat_ . The symbol ‘ ’ represents that there is at least one collaborative strategy whose accuracy is better than self-consistency, while the symbol ‘ ’ indicates that there is no
collaborative strategy whose accuracy is worse than self-consistency. Both of these symbols represent the accuracy
of self-consistency. The accuracy under each collaborative strategy is a summation within all 3-agent societies.


The significance test is shown in Table 23.


_Round 1:_ _Round 2-4:_ _i_ _j_ _k_ _Round 1:_ _Round 2-4:_ _i_ _j_ _k_


Figure 35: Accuracy at round 2,3,4 within 4-round collaborative societies, where the thinking pattern of round 1 is
fixed ( _p_ 0 or _p_ 1), using _LlaMA2-70B-chat_ . The significance test is shown in Table 24.


Collaborative MMLU Chess Move Validity
Strategy p-value p-value

_p_ 0 _p_ 0 _p_ 0 0.481 0.006
_p_ 0 _p_ 0 _p_ 1 0.000 0.001
_p_ 0 _p_ 1 _p_ 0 0.000 0.000
_p_ 0 _p_ 1 _p_ 1  - 0.023
_p_ 1 _p_ 0 _p_ 0 0.001 0.035
_p_ 1 _p_ 0 _p_ 1 0.003 0.000
_p_ 1 _p_ 1 _p_ 0 0.002 0.036
_p_ 1 _p_ 1 _p_ 1 0.024 0.423


Table 23: One-way ANOVA analysis of the results of
Figure 34 (different numbers of agents), _using LlaMA2-_
_chat-70B_ .


14584


Collaborative MMLU Chess Move Validity
Strategy p-value p-value

_p_ 0 _p_ 0 _p_ 0 _p_ 0 0.034 0.545
_p_ 0 _p_ 0 _p_ 0 _p_ 1 0.008 0.019
_p_ 0 _p_ 0 _p_ 1 _p_ 0 0.020 0.004
_p_ 0 _p_ 0 _p_ 1 _p_ 1 0.643 0.004
_p_ 0 _p_ 1 _p_ 0 _p_ 0 0.045 0.034
_p_ 0 _p_ 1 _p_ 0 _p_ 1 0.164 0.902
_p_ 0 _p_ 1 _p_ 1 _p_ 0 0.046 0.006
_p_ 0 _p_ 1 _p_ 1 _p_ 1 0.082 0.000
_p_ 1 _p_ 0 _p_ 0 _p_ 0 0.706 0.207
_p_ 1 _p_ 0 _p_ 0 _p_ 1 0.449 0.494
_p_ 1 _p_ 0 _p_ 1 _p_ 0 0.782 0.095
_p_ 1 _p_ 0 _p_ 1 _p_ 1 0.664 0.070
_p_ 1 _p_ 1 _p_ 0 _p_ 0 0.360 0.041
_p_ 1 _p_ 1 _p_ 0 _p_ 1 0.391 0.018
_p_ 1 _p_ 1 _p_ 1 _p_ 0 0.394 0.088
_p_ 1 _p_ 1 _p_ 1 _p_ 1 0.031 0.033


Table 24: One-way ANOVA analysis of the results in
Figure 35 (different rounds), _using LlaMA2-chat-70B_ .


Collaborative MMLU MATH Chess Move Validity
Strategy p-value p-value p-value

_p_ 0 _p_ 0 _p_ 0 0.029 0.296 0.004
_p_ 0 _p_ 0 _p_ 1 0.005 0.020 0.724
_p_ 0 _p_ 1 _p_ 0 0.018 0.191 0.000
_p_ 0 _p_ 1 _p_ 1 0.000 0.809 0.684
_p_ 1 _p_ 0 _p_ 0 0.894 0.503 0.045
_p_ 1 _p_ 0 _p_ 1 0.747 0.050 0.328
_p_ 1 _p_ 1 _p_ 0 0.928 0.007 0.001
_p_ 1 _p_ 1 _p_ 1 0.004 1.000 0.557


Table 25: One-way ANOVA analysis of the results
in Figure 36 (other collaborative strategies), _using_
_LlaMA2-chat-70B_ .



MMLU



50


40


30



p0p0p0
All Part



0p0 p0p0p1

AllPart Part



0p1 p0p1p0

AllPart Part



1p0 p0p1p1

AllPart Part



1p1 p1p0p0

AllPart Part



0p0 p1p0p1

AllPart Part



0p1 p1p1p0

AllPart Part



1p0 p1p1p1

AllPart Part



MATH



10


5



0



0p0 p0p0p1

AllPart Part



p0p0p0
All Part



0p1 p0p1p0

AllPart Part



1p0 p0p1p1

AllPart Part



1p1 p1p0p0

AllPart Part



0p0 p1p0p1

AllPart Part



0p1 p1p1p0

AllPart Part



1p0 p1p1p1

AllPart Part



Chess Move Validity



20


10


0



p0p0p0
All Part



0p0 p0p0p1

AllPart Part



0p1 p0p1p0

AllPart Part



1p0 p0p1p1

AllPart Part



1p1 p1p0p0

AllPart Part



0p0 p1p0p1

AllPart Part



0p1 p1p1p0

AllPart Part



1p0 p1p1p1

AllPart Part



Figure 36: The effect on the accuracy of whether all
agents in society execute the same thinking pattern in
one round, using _LlaMA2-70B-chat_ . “All” and “Part”
refers to all agents applying the same thinking pattern
and different thinking patterns in one round respectively. The significance test is shown in Table 25.



14585


_brings about detriments_ : Ratio(False _→_ True + True _→_ True) _<_ Ratio(True _→_ False + False _→_ False).


Figure 38: Average quantity of _consensus_ _clusters_ _(i.e.,_ _unique_ _answers_ _among_ _multiple_ _agents)_ under different
rounds of collaboration with 3-round collaborative strategies, on _LlaMA2-70B-chat_ . _Smaller quantity of consensus_
_clusters, more easier it is to reach a consensus._ Round 0 is equal to self-consistency.


Figure (a-c) & (d-f) respectively show the token cost and accuracy of different strategies before and after 3-round
collaboration. Figure (g-i) present the percentage of different behavioral features (mainly analyzed by the change
of answer correctness) (Zhang et al., 2023b,a) under different collaborative strategies. All results are summarized
across all societies.


14586


**H.3** **Qwen 72B**

**Analysis on Machine Social Collaboration.** We
present the **main** **results** and **significance** **tests**
of societies and strategies on Qwen 72B in Table 26, 27, 28. We present the **word** **clouds** of
Qwen 72B in Figure 40, and **proportion of agents**
**with** **different** **traits** **keeping** **answers** **in** **differ-**
**ent societies** on Qwen 72B in Figure 41. Furthermore, we demonstrate that the tasks with different
subjects and difficulty display varying sensitivity
to collaborative strategies, as presented with **radar**
**maps** on Qwen 72B in Figure 42.
**Analysis** **on** **Different** **Numbers** **of** **Agents.**
We present the significance test for different numbers of agents with Qwen 72B in Table 29. We also
show the performance varying from agent numbers
in Figure 43, varying from societies containing
2 _∼_ 10 agents in Figure 44. We also analyze the _con-_
_sensus reaching_ with different numbers of agents,
and present the results in Figure 45, 46.
**Analysis on Different Rounds.** We present the
significance test for different rounds of collaboration with Qwen 72B in Table 30. We also show the
performance varying from collaboration rounds in
Figure 47, 48, 49.
**Analysis** **on** **Other** **Collaborative** **Strategies.**
We present the significance test for other collaborative strategies (executing the same or hybrid
thinking patterns in a certain round) with Qwen
72B in Table 31. We also show the performance
varying from other strategies in Figure 50.
**A** **Social** **Psychology** **View** **on** **Conformity,**
**Consensus Reaching and Group Dynamics.** We
then show the variation of answer correctness in
the situation of conformity in Figure 51; and the
quantity of consensus clusters among 3-agent answers in Figure 52. We present group dynamics
reflected by different answer-changing behaviors
on Qwen 72B in Figure 53.


14587


|Col1|Metric<br>(Strategy)|Society|Collaborative Strategy<br>p0p0p0 p0p0p1 p0p1p0 p0p1p1 p1p0p0 p1p0p1 p1p1p0 p1p1p1|Metric (Society)<br>Cost ↓ W-T ↑|
|---|---|---|---|---|
|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>64.8_±_6.4<br>**66.4**_±_**6.8**<br>**65.6**_±_**9.7**<br>63.6_±_5.0<br>58.0_±_4.2<br>58.4_±_3.0<br>60.0_±_8.8<br>63.6_±_2.6<br>3661<br>14<br>_S_2<br>60.4_±_5.9<br>60.8_±_5.2<br>**62.8**_±_**2.3**<br>61.6_±_4.6<br>53.2_±_5.6<br>57.6_±_2.6<br>61.2_±_7.8<br>**62.4**_±_**4.3**<br>3657<br>21<br>_S_3<br>64.0_±_4.7<br>64.4_±_3.9<br>**66.0**_±_**2.8**<br>**65.2**_±_**3.0**<br>56.8_±_5.9<br>57.6_±_5.2<br>59.6_±_4.3<br>64.4_±_2.6<br>3690<br>17<br>_S_4<br>62.4_±_6.2<br>**64.8**_±_**3.9**<br>64.0_±_7.1<br>**66.8**_±_**7.3**<br>53.2_±_5.4<br>56.8_±_4.2<br>60.4_±_7.4<br>58.4_±_3.9<br>3570<br>14<br>Cost_ ↓_<br>All<br>5960<br>4560<br>4017<br>3158<br>4024<br>2761<br>2746<br>1927<br>-<br>W-T_ ↑_<br>All<br>-<br>12<br>**14**<br>13<br>4<br>4<br>9<br>10|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>64.8_±_6.4<br>**66.4**_±_**6.8**<br>**65.6**_±_**9.7**<br>63.6_±_5.0<br>58.0_±_4.2<br>58.4_±_3.0<br>60.0_±_8.8<br>63.6_±_2.6<br>3661<br>14<br>_S_2<br>60.4_±_5.9<br>60.8_±_5.2<br>**62.8**_±_**2.3**<br>61.6_±_4.6<br>53.2_±_5.6<br>57.6_±_2.6<br>61.2_±_7.8<br>**62.4**_±_**4.3**<br>3657<br>21<br>_S_3<br>64.0_±_4.7<br>64.4_±_3.9<br>**66.0**_±_**2.8**<br>**65.2**_±_**3.0**<br>56.8_±_5.9<br>57.6_±_5.2<br>59.6_±_4.3<br>64.4_±_2.6<br>3690<br>17<br>_S_4<br>62.4_±_6.2<br>**64.8**_±_**3.9**<br>64.0_±_7.1<br>**66.8**_±_**7.3**<br>53.2_±_5.4<br>56.8_±_4.2<br>60.4_±_7.4<br>58.4_±_3.9<br>3570<br>14<br>Cost_ ↓_<br>All<br>5960<br>4560<br>4017<br>3158<br>4024<br>2761<br>2746<br>1927<br>-<br>W-T_ ↑_<br>All<br>-<br>12<br>**14**<br>13<br>4<br>4<br>9<br>10|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>64.8_±_6.4<br>**66.4**_±_**6.8**<br>**65.6**_±_**9.7**<br>63.6_±_5.0<br>58.0_±_4.2<br>58.4_±_3.0<br>60.0_±_8.8<br>63.6_±_2.6<br>3661<br>14<br>_S_2<br>60.4_±_5.9<br>60.8_±_5.2<br>**62.8**_±_**2.3**<br>61.6_±_4.6<br>53.2_±_5.6<br>57.6_±_2.6<br>61.2_±_7.8<br>**62.4**_±_**4.3**<br>3657<br>21<br>_S_3<br>64.0_±_4.7<br>64.4_±_3.9<br>**66.0**_±_**2.8**<br>**65.2**_±_**3.0**<br>56.8_±_5.9<br>57.6_±_5.2<br>59.6_±_4.3<br>64.4_±_2.6<br>3690<br>17<br>_S_4<br>62.4_±_6.2<br>**64.8**_±_**3.9**<br>64.0_±_7.1<br>**66.8**_±_**7.3**<br>53.2_±_5.4<br>56.8_±_4.2<br>60.4_±_7.4<br>58.4_±_3.9<br>3570<br>14<br>Cost_ ↓_<br>All<br>5960<br>4560<br>4017<br>3158<br>4024<br>2761<br>2746<br>1927<br>-<br>W-T_ ↑_<br>All<br>-<br>12<br>**14**<br>13<br>4<br>4<br>9<br>10|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>64.8_±_6.4<br>**66.4**_±_**6.8**<br>**65.6**_±_**9.7**<br>63.6_±_5.0<br>58.0_±_4.2<br>58.4_±_3.0<br>60.0_±_8.8<br>63.6_±_2.6<br>3661<br>14<br>_S_2<br>60.4_±_5.9<br>60.8_±_5.2<br>**62.8**_±_**2.3**<br>61.6_±_4.6<br>53.2_±_5.6<br>57.6_±_2.6<br>61.2_±_7.8<br>**62.4**_±_**4.3**<br>3657<br>21<br>_S_3<br>64.0_±_4.7<br>64.4_±_3.9<br>**66.0**_±_**2.8**<br>**65.2**_±_**3.0**<br>56.8_±_5.9<br>57.6_±_5.2<br>59.6_±_4.3<br>64.4_±_2.6<br>3690<br>17<br>_S_4<br>62.4_±_6.2<br>**64.8**_±_**3.9**<br>64.0_±_7.1<br>**66.8**_±_**7.3**<br>53.2_±_5.4<br>56.8_±_4.2<br>60.4_±_7.4<br>58.4_±_3.9<br>3570<br>14<br>Cost_ ↓_<br>All<br>5960<br>4560<br>4017<br>3158<br>4024<br>2761<br>2746<br>1927<br>-<br>W-T_ ↑_<br>All<br>-<br>12<br>**14**<br>13<br>4<br>4<br>9<br>10|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>64.8_±_6.4<br>**66.4**_±_**6.8**<br>**65.6**_±_**9.7**<br>63.6_±_5.0<br>58.0_±_4.2<br>58.4_±_3.0<br>60.0_±_8.8<br>63.6_±_2.6<br>3661<br>14<br>_S_2<br>60.4_±_5.9<br>60.8_±_5.2<br>**62.8**_±_**2.3**<br>61.6_±_4.6<br>53.2_±_5.6<br>57.6_±_2.6<br>61.2_±_7.8<br>**62.4**_±_**4.3**<br>3657<br>21<br>_S_3<br>64.0_±_4.7<br>64.4_±_3.9<br>**66.0**_±_**2.8**<br>**65.2**_±_**3.0**<br>56.8_±_5.9<br>57.6_±_5.2<br>59.6_±_4.3<br>64.4_±_2.6<br>3690<br>17<br>_S_4<br>62.4_±_6.2<br>**64.8**_±_**3.9**<br>64.0_±_7.1<br>**66.8**_±_**7.3**<br>53.2_±_5.4<br>56.8_±_4.2<br>60.4_±_7.4<br>58.4_±_3.9<br>3570<br>14<br>Cost_ ↓_<br>All<br>5960<br>4560<br>4017<br>3158<br>4024<br>2761<br>2746<br>1927<br>-<br>W-T_ ↑_<br>All<br>-<br>12<br>**14**<br>13<br>4<br>4<br>9<br>10|
|<br>MATH<br>Acc_ ↑_<br>_S_1<br>**47.2**_±_**5.6**<br>43.6_±_4.6<br>**46.0**_±_**6.5**<br>43.6_±_5.0<br>40.4_±_6.5<br>41.6_±_8.1<br>42.0_±_4.9<br>39.6_±_3.9<br>3537<br>11<br>_S_2<br>**49.6**_±_**5.4**<br>48.4_±_6.1<br>**48.8**_±_**6.7**<br>47.2_±_5.9<br>41.2_±_4.4<br>41.6_±_5.4<br>40.0_±_4.0<br>37.6_±_4.1<br>3513<br>7<br>_S_3<br>**44.8**_±_**6.4**<br>**44.4**_±_**5.5**<br>43.6_±_4.3<br>42.0_±_7.1<br>40.4_±_7.8<br>37.6_±_6.7<br>41.6_±_7.5<br>36.4_±_8.7<br>3595<br>9<br>_S_4<br>**46.0**_±_**6.6**<br>44.8_±_8.6<br>**46.0**_±_**8.0**<br>43.6_±_5.4<br>39.2_±_5.0<br>41.6_±_4.8<br>37.6_±_6.7<br>35.6_±_3.9<br>3595<br>11<br>Cost_ ↓_<br>All<br>4813<br>4182<br>4187<br>3549<br>3571<br>2912<br>2985<br>2281<br>-<br>W-T_ ↑_<br>All<br>-<br>9<br>**13**<br>7<br>3<br>3<br>2<br>1|<br>Acc_ ↑_|_S_1<br>_S_2<br>_S_3<br>_S_4|**47.2**_±_**5.6**<br>43.6_±_4.6<br>**46.0**_±_**6.5**<br>43.6_±_5.0<br>40.4_±_6.5<br>41.6_±_8.1<br>42.0_±_4.9<br>39.6_±_3.9<br>**49.6**_±_**5.4**<br>48.4_±_6.1<br>**48.8**_±_**6.7**<br>47.2_±_5.9<br>41.2_±_4.4<br>41.6_±_5.4<br>40.0_±_4.0<br>37.6_±_4.1<br>**44.8**_±_**6.4**<br>**44.4**_±_**5.5**<br>43.6_±_4.3<br>42.0_±_7.1<br>40.4_±_7.8<br>37.6_±_6.7<br>41.6_±_7.5<br>36.4_±_8.7<br>**46.0**_±_**6.6**<br>44.8_±_8.6<br>**46.0**_±_**8.0**<br>43.6_±_5.4<br>39.2_±_5.0<br>41.6_±_4.8<br>37.6_±_6.7<br>35.6_±_3.9|3537<br>11<br>3513<br>7<br>3595<br>9<br>3595<br>11|
|<br>MATH<br>Acc_ ↑_<br>_S_1<br>**47.2**_±_**5.6**<br>43.6_±_4.6<br>**46.0**_±_**6.5**<br>43.6_±_5.0<br>40.4_±_6.5<br>41.6_±_8.1<br>42.0_±_4.9<br>39.6_±_3.9<br>3537<br>11<br>_S_2<br>**49.6**_±_**5.4**<br>48.4_±_6.1<br>**48.8**_±_**6.7**<br>47.2_±_5.9<br>41.2_±_4.4<br>41.6_±_5.4<br>40.0_±_4.0<br>37.6_±_4.1<br>3513<br>7<br>_S_3<br>**44.8**_±_**6.4**<br>**44.4**_±_**5.5**<br>43.6_±_4.3<br>42.0_±_7.1<br>40.4_±_7.8<br>37.6_±_6.7<br>41.6_±_7.5<br>36.4_±_8.7<br>3595<br>9<br>_S_4<br>**46.0**_±_**6.6**<br>44.8_±_8.6<br>**46.0**_±_**8.0**<br>43.6_±_5.4<br>39.2_±_5.0<br>41.6_±_4.8<br>37.6_±_6.7<br>35.6_±_3.9<br>3595<br>11<br>Cost_ ↓_<br>All<br>4813<br>4182<br>4187<br>3549<br>3571<br>2912<br>2985<br>2281<br>-<br>W-T_ ↑_<br>All<br>-<br>9<br>**13**<br>7<br>3<br>3<br>2<br>1|Cost_ ↓_|All|4813<br>4182<br>4187<br>3549<br>3571<br>2912<br>2985<br>2281|4813<br>4182<br>4187<br>3549<br>3571<br>2912<br>2985<br>2281|
|<br>Chess Move Validity<br>Acc_ ↑_<br>_S_1<br>**43.2**_±_**7.0**<br>**42.4**_±_**4.6**<br>41.2_±_9.7<br>36.8_±_6.4<br>27.6_±_4.8<br>22.0_±_5.3<br>20.4_±_4.8<br>6.4_±_3.3<br>2557<br>6<br>_S_2<br>**46.8**_±_**4.2**<br>**42.8**_±_**4.2**<br>39.2_±_4.6<br>34.8_±_4.2<br>29.6_±_5.2<br>16.8_±_2.7<br>22.8_±_5.8<br>8.8_±_3.4<br>2499<br>1<br>_S_3<br>**42.4**_±_**8.7**<br>**38.4**_±_**9.9**<br>38.0_±_6.9<br>36.8_±_7.8<br>26.8_±_5.8<br>19.6_±_2.6<br>19.6_±_2.6<br>6.0_±_2.8<br>2496<br>3<br>_S_4<br>**36.0**_±_**8.1**<br>32.4_±_4.6<br>**34.0**_±_**5.8**<br>26.0_±_4.9<br>26.8_±_5.4<br>20.8_±_5.4<br>22.4_±_5.9<br>11.2_±_2.3<br>2455<br>4<br>Cost_ ↓_<br>All<br>3148<br>2621<br>2585<br>2118<br>2904<br>2384<br>2393<br>1860<br>-<br>W-T_ ↑_<br>All<br>-<br>**6**<br>**6**<br>2<br>0<br>0<br>0<br>0|<br>Acc_ ↑_|_S_1<br>_S_2<br>_S_3<br>_S_4|**43.2**_±_**7.0**<br>**42.4**_±_**4.6**<br>41.2_±_9.7<br>36.8_±_6.4<br>27.6_±_4.8<br>22.0_±_5.3<br>20.4_±_4.8<br>6.4_±_3.3<br>**46.8**_±_**4.2**<br>**42.8**_±_**4.2**<br>39.2_±_4.6<br>34.8_±_4.2<br>29.6_±_5.2<br>16.8_±_2.7<br>22.8_±_5.8<br>8.8_±_3.4<br>**42.4**_±_**8.7**<br>**38.4**_±_**9.9**<br>38.0_±_6.9<br>36.8_±_7.8<br>26.8_±_5.8<br>19.6_±_2.6<br>19.6_±_2.6<br>6.0_±_2.8<br>**36.0**_±_**8.1**<br>32.4_±_4.6<br>**34.0**_±_**5.8**<br>26.0_±_4.9<br>26.8_±_5.4<br>20.8_±_5.4<br>22.4_±_5.9<br>11.2_±_2.3|2557<br>6<br>2499<br>1<br>2496<br>3<br>2455<br>4|
|<br>Chess Move Validity<br>Acc_ ↑_<br>_S_1<br>**43.2**_±_**7.0**<br>**42.4**_±_**4.6**<br>41.2_±_9.7<br>36.8_±_6.4<br>27.6_±_4.8<br>22.0_±_5.3<br>20.4_±_4.8<br>6.4_±_3.3<br>2557<br>6<br>_S_2<br>**46.8**_±_**4.2**<br>**42.8**_±_**4.2**<br>39.2_±_4.6<br>34.8_±_4.2<br>29.6_±_5.2<br>16.8_±_2.7<br>22.8_±_5.8<br>8.8_±_3.4<br>2499<br>1<br>_S_3<br>**42.4**_±_**8.7**<br>**38.4**_±_**9.9**<br>38.0_±_6.9<br>36.8_±_7.8<br>26.8_±_5.8<br>19.6_±_2.6<br>19.6_±_2.6<br>6.0_±_2.8<br>2496<br>3<br>_S_4<br>**36.0**_±_**8.1**<br>32.4_±_4.6<br>**34.0**_±_**5.8**<br>26.0_±_4.9<br>26.8_±_5.4<br>20.8_±_5.4<br>22.4_±_5.9<br>11.2_±_2.3<br>2455<br>4<br>Cost_ ↓_<br>All<br>3148<br>2621<br>2585<br>2118<br>2904<br>2384<br>2393<br>1860<br>-<br>W-T_ ↑_<br>All<br>-<br>**6**<br>**6**<br>2<br>0<br>0<br>0<br>0|Cost_ ↓_|All|3148<br>2621<br>2585<br>2118<br>2904<br>2384<br>2393<br>1860|3148<br>2621<br>2585<br>2118<br>2904<br>2384<br>2393<br>1860|



Table 26: The impact of eight different collaborative strategies on the performance of three datasets across distinct
societies ( _using Qwen 72B_ ). The significances test on societies and strategies are respectively shown in Table 27, 28.
The experiments of comparison with the single LLM agent is shown in Figure 53(a)-(f).


_**MMLU**_ _**Math**_ _**Chess Move Validity**_







Figure 40: Comparative word clouds on three datasets in societies _S_ 1 and _S_ 4, using _Qwen 72B_ . Society _S_ 1 features
three overconfident agents, while society _S_ 4 comprises three easy-going agents.



Collaborative MMLU MATH Chess Move Validity
Strategy p-value p-value p-value

_p_ 0 _p_ 0 _p_ 0 0.654 0.637 0.162
_p_ 0 _p_ 0 _p_ 1 0.388 0.649 0.064
_p_ 0 _p_ 1 _p_ 0 0.841 0.667 0.445
_p_ 0 _p_ 1 _p_ 1 0.455 0.567 0.034
_p_ 1 _p_ 0 _p_ 0 0.387 0.963 0.817
_p_ 1 _p_ 0 _p_ 1 0.933 0.690 0.281
_p_ 1 _p_ 1 _p_ 0 0.987 0.647 0.695
_p_ 1 _p_ 1 _p_ 1 0.061 0.688 0.048


Table 27: One-Way ANOVA results for the impact of
society on accuracy with fixed collaborative strategy,
based on experiments from Table 26 using _Qwen 72B_ .



MMLU MATH Chess Move Validity
Society p-value p-value p-value

_S_ 1 0.257 0.418 0.000
_S_ 2 0.093 0.004 0.000
_S_ 3 0.004 0.449 0.000
_S_ 4 0.015 0.088 0.000


Table 28: One-Way ANOVA results for the impact of
collaborative strategy on accuracy with fixed society,
based on experiments from Table 26 using _Qwen 72B_ .



14588


Society _S_ 1 features three overconfident agents, while society _S_ 4 comprises three easy-going agents.


Figure 42: Illustration of different collaborative strategies impacting accuracy diversely on the tasks considering
varied _subjects_ and _difficulty_, using _Qwen 72B_ . The symbol ‘ ’ represents that there is at least one collaborative
strategy whose accuracy is better than self-consistency, while the symbol ‘ ’ indicates that there is no collaborative
strategy whose accuracy is worse than self-consistency. Both of these symbols represent the accuracy of selfconsistency. The accuracy under each collaborative strategy is a summation within all 3-agent societies.


14589


|Col1|Col2|Col3|Col4|Col5|Col6|
|---|---|---|---|---|---|
|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|
|||||||
|||||||


14590


14591


( ) m

Figure 45: Average quantity of _consensus clusters (unique answers among multiple agents)_ in _different societies_
with 2 _∼_ 10 agents under each round of 3-round collaborative strategies, using _Qwen 72B_ .







Figure 46: Average ratio of _consensus_ _clusters_ _(unique_ _answers_ _among_ _multiple_ _agents)_ with _different_ _numbers_
_(2∼10) of agents_ under each round of 3-round collaborative strategies, using _Qwen 72B_ .

14592


4
(p1)



65


60


65


60



3
(p1)


3
(p0)



4
(p1)



4
(p1)



5
(p1)



6
(p1)



7
(p1)



8 9
(p1) (p1)



60


50


60


50



3
(p1)


3
(p1)



4
(p1)



4
(p0)



5
(p1)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



65


60

3
(p0)


65


60


3
(p1)



4
(p0)



5
(p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)



60


50


65


60



3
(p0)


3
(p0)



4
(p0)



4
(p0)



5
(p0)



5
(p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)











5
(p0)



6
(p1)



7
(p0)



8 9
(p1) (p0)



10
(p1)


10
(p1)



6
(p0)



7
(p1)



8
(p0)



9
(p1)



10
(p1)


10
(p0)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



10
(p0)


10
(p1)



6
(p0)



7
(p0)



8
(p0)



9
(p0)



10
(p0)


10
(p0)



Figure 47: Accuracy of _different_ _(3_ _10)_ _rounds_ _of_ _collaboration_ within 3-agent society _S_ 2 (1 easy-going and 2
_∼_
overconfident agents) on MMLU, using _Qwen 72B_ . The significance test is shown in Table 30.











3
(p1)


3
(p1)



47.5

45.0

42.5


50


40



3
(p1)


3
(p0)



4
(p1)



4
(p1)



4
(p1)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



45

40

35


50


40



4
(p1)



4
(p0)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



45


40

3
(p0)


40


35


3
(p1)



4
(p0)



5
(p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)



45


40


45


40



3
(p0)


3
(p0)



4
(p0)



4
(p0)



5
(p0)



5
(p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)











5
(p0)



6
(p1)



7
(p0)



8
(p1)



9
(p0)



10
(p1)


10
(p1)



5
(p1)



6
(p0)



7
(p1)



8
(p0)



9
(p1)



10
(p1)


10
(p0)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



10
(p0)


10
(p1)



6
(p0)



7
(p0)



8
(p0)



9
(p0)



10
(p0)


10
(p0)



Figure 48: Accuracy of _different_ _(3_ _10)_ _rounds_ _of_ _collaboration_ within 3-agent society _S_ 2 (1 easy-going and 2
_∼_
overconfident agents) on MATH, using _Qwen 72B_ . The significance test is shown in Table 30.









4
(p1)



3
(p1)


3
(p1)





45

40

35


40


30



3
(p1)


3
(p0)



3
(p0)


3
(p0)



4
(p1)



4
(p1)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



30


20


30


20



4
(p1)



4
(p0)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



50


40


3
(p0)


10


5


3
(p1)



4
(p0)



5
(p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)



35


30


50


40



4 5
(p0) (p0)



4 5
(p0) (p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)







10
(p1)


10
(p0)







5
(p0)



6
(p1)



7
(p0)



8
(p1)



9
(p0)



10
(p1)


10
(p1)



5
(p1)



6
(p0)



7
(p1)



8
(p0)



9
(p1)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



10
(p0)


10
(p1)



6
(p0)



7
(p0)



8
(p0)



9
(p0)



10
(p0)


10
(p0)



Figure 49: Accuracy of _different_ _(3_ _10)_ _rounds_ _of_ _collaboration_ within 3-agent society _S_ 2 (1 easy-going and 2
_∼_
overconfident agents) on Chess Move Validity, using _Qwen 72B_ . The significance test is shown in Table 30.


14593


Collaborative _S_ 1 _′_ _S_ 2 _′_ _S_ 3 _′_ _S_ 4 _′_ _S_ 5 _′_
Strategy p-value p-value p-value p-value p-value

_p_ 0 _p_ 0 _p_ 0 0.005 0.001 0.003 0.041 0.015
_p_ 0 _p_ 0 _p_ 1 0.017 0.010 0.037 0.001 0.006
_p_ 0 _p_ 1 _p_ 0 0.006 0.016 0.002 0.000 0.001
_p_ 0 _p_ 1 _p_ 1 0.020 0.002 0.010 0.001 0.004
_p_ 1 _p_ 0 _p_ 0 0.000 0.005 0.000 0.000 0.000
_p_ 1 _p_ 0 _p_ 1 0.002 0.008 0.004 0.000 0.054
_p_ 1 _p_ 1 _p_ 0 0.003 0.000 0.002  - 0.000
_p_ 1 _p_ 1 _p_ 1 0.064 0.008 0.005 0.016 0.000


Table 29: One-way ANOVA analysis of results in Figure 43 (different numbers of agents), using _Qwen 72B_ .
_′_
_S_ 1 [:] [One] [overconfident] [agent] [and] [the] [others] [are] [all]
_′_
easygoing. _S_ 2 [:] [One] [easygoing] [agent] [among] [predom-]
_′_
inantly overconfident agents. _S_ 3 [:] [Equal] [numbers] [of]
_′_
overconfident and easygoing agents. _S_ 4 [:] [Entirely easy-]
_′_
going agents. _S_ 5 [:] [Entirely overconfident agents.] [‘-’:] [It]
doesn’t pass homogeneity test for variance.


Collaborative MMLU MATH Chess Move Validity
Strategy p-value p-value p-value

_p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 0.262 0.987 0.956
_p_ 1 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 0.753 0.697 0.124
_p_ 0 _p_ 1 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 0.914 0.962 0.386
_p_ 1 _p_ 0 _p_ 1 _p_ 0 _p_ 1 _p_ 0 _p_ 1 _p_ 0 _p_ 1 _p_ 0 0.673 0.715 0.154
_p_ 0 _p_ 1 _p_ 0 _p_ 1 _p_ 0 _p_ 1 _p_ 0 _p_ 1 _p_ 0 _p_ 1 0.922 0.987 0.700
_p_ 1 _p_ 0 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 0.845 0.843 0.282
_p_ 0 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 0.928 0.585 0.583
_p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 0.832 0.801 0.731


Table 30: One-way ANOVA analysis of the results in
Figure 48, 48, 49 (different rounds), using _Qwen 72B_ .


MMLU


70


60


50



Collaborative MMLU MATH Chess Move Validity
Strategy p-value p-value p-value

_p_ 0 _p_ 0 _p_ 0 0.704 0.142 0.003
_p_ 0 _p_ 0 _p_ 1 0.136 0.184 0.000
_p_ 0 _p_ 1 _p_ 0 0.899 0.157 0.001
_p_ 0 _p_ 1 _p_ 1 0.180 0.194 0.089
_p_ 1 _p_ 0 _p_ 0 0.157 0.856 0.004
_p_ 1 _p_ 0 _p_ 1 0.521 0.152 0.019
_p_ 1 _p_ 1 _p_ 0  - 0.790 0.004
_p_ 1 _p_ 1 _p_ 1 0.391 0.688 1.000


Table 31: One-way ANOVA analysis of results in Figure 50 (other collaborative strategies), _using Qwen 72B_ .
‘-’ means it doesn’t pass homogeneity test for variance.



p0p0p0
All Part



0p0 p0p0p1

AllPart Part



0p1 p0p1p0

AllPart Part



1p0 p0p1p1

AllPart Part



1p1 p1p0p0

AllPart Part



0p0 p1p0p1

AllPart Part



0p1 p1p1p0

AllPart Part



1p0 p1p1p1

AllPart Part



MATH



50


40



30



0p0 p0p0p1

AllPart Part



p0p0p0
All Part



0p1 p0p1p0

AllPart Part



1p0 p0p1p1

AllPart Part



1p1 p1p0p0

AllPart Part



0p0 p1p0p1

AllPart Part



0p1 p1p1p0

AllPart Part



1p0 p1p1p1

AllPart Part



Chess Move Validity



40


20



p0p0p0
All Part



0p0 p0p0p1

AllPart Part



0p1 p0p1p0

AllPart Part



1p0 p0p1p1

AllPart Part



1p1 p1p0p0

AllPart Part



0p0 p1p0p1

AllPart Part



0p1 p1p1p0

AllPart Part



1p0 p1p1p1

AllPart Part



Figure 50: The effect on the accuracy of whether all
agents in society execute the same thinking pattern in
one round, using _Qwen 72B_ . “All” and “Part” refers to
all agents applying the same thinking pattern and different thinking patterns in one round respectively. The
significance test is shown in Table 31.


14594


_about detriments_ : Ratio(False _→_ True + True _→_ True) _<_ Ratio(True _→_ False + False _→_ False).


Figure 52: Average quantity of _consensus_ _clusters_ _(i.e.,_ _unique_ _answers_ _among_ _multiple_ _agents)_ under different
rounds of collaboration with 3-round collaborative strategies, using _Qwen_ _72B_ . _Smaller_ _quantity_ _of_ _consensus_


(a-c) & (d-f) respectively show the token cost and accuracy of different strategies before and after 3-round collaboration. Figure (g-i) present the percentage of different behavioral features (mainly analyzed by the change of
answer correctness) (Zhang et al., 2023b,a) under different collaborative strategies. All results are summarized
across all societies.


14595


**H.4** **Mixtral 8** _×_ **7B**

**Analysis on Machine Social Collaboration.** We
present the **main results** and **significance tests** of
societies and strategies on Mixtral 8 _×_ 7B in Table 32, 33, 34. We present the word clouds of
Mixtral 8 _×_ 7B in Figure 54, and the proportion of
agents with different traits keepging answers in
different societies on Mixtral 8 _×_ 7B in Figure 55.
Furthermore, we demonstrate that the tasks with
different subjects and difficulty display varying sensitivity to collaborative strategies, as presented with
**radar maps** on Mixtral 8 _×_ 7B in Figure 56.
**Analysis** **on** **Different** **Numbers** **of** **Agents.**
We present the significance test for different numbers of agents with Mixtral 8 _×_ 7B in Table 35. We
also show the performance varying from agent numbers in Figure 57, varying from societies containing
2 _∼_ 10 agents in Figure 58. We also analyze the _con-_
_sensus reaching_ with different numbers of agents,
and present the results in Figure 59, 60.
**Analysis on Different Rounds.** We present the
significance test for different rounds of collaboration with Mixtral 8 _×_ 7B in Table 36. We also show
the performance varying from collaboration rounds
in Figure 61, 62, 63.
**Analysis** **on** **Other** **Collaborative** **Strategies.**
We present the significance test for other collaborative strategies (executing the same or hybrid
thinking patterns in a certain round) with Mixtral
8 _×_ 7B in Table 37. We also show the performance
varying from other strategies in Figure 64.
**A** **Social** **Psychology** **View** **on** **Conformity,**
**Consensus Reaching and Group Dynamics.** We
then show the variation of answer correctness in
the situation of conformity in Figure 65; and the
quantity of consensus clusters among 3-agent answers in Figure 66. We present group dynamics
reflected by different answer-changing behaviors
on Mxitral-8 _×_ 7B in Figure 67.


14596


|Col1|Metric<br>(Strategy)|Society|Collaborative Strategy<br>p0p0p0 p0p0p1 p0p1p0 p0p1p1 p1p0p0 p1p0p1 p1p1p0 p1p1p1|Metric (Society)<br>Cost ↓ W-T ↑|
|---|---|---|---|---|
|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>60.0_±_8.1<br>59.6_±_3.9<br>58.4_±_4.3<br>**60.0**_±_**1.4**<br>60.0_±_5.8<br>**60.4**_±_**5.2**<br>59.6_±_2.6<br>60.0_±_2.0<br>4479<br>17<br>_S_2<br>59.2_±_7.7<br>60.0_±_7.9<br>60.0_±_6.5<br>60.8_±_5.8<br>61.2_±_3.6<br>**62.8**_±_**5.4**<br>**62.8**_±_**5.4**<br>**61.2**_±_**2.7**<br>4475<br>27<br>_S_3<br>62.4_±_5.2<br>**63.6**_±_**4.3**<br>**65.2**_±_**3.0**<br>**65.2**_±_**3.0**<br>59.2_±_4.4<br>61.2_±_4.2<br>61.6_±_2.6<br>59.6_±_3.6<br>4489<br>18<br>_S_4<br>60.0_±_3.7<br>62.4_±_3.6<br>**63.2**_±_**3.4**<br>62.8_±_2.7<br>60.0_±_5.1<br>60.4_±_5.5<br>**64.8**_±_**5.8**<br>62.0_±_6.6<br>4396<br>25<br>Cost_ ↓_<br>All<br>6891<br>5371<br>4871<br>3944<br>4996<br>3594<br>3495<br>2516<br>-<br>W-T_ ↑_<br>All<br>-<br>14<br>**15**<br>14<br>9<br>11<br>13<br>11|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>60.0_±_8.1<br>59.6_±_3.9<br>58.4_±_4.3<br>**60.0**_±_**1.4**<br>60.0_±_5.8<br>**60.4**_±_**5.2**<br>59.6_±_2.6<br>60.0_±_2.0<br>4479<br>17<br>_S_2<br>59.2_±_7.7<br>60.0_±_7.9<br>60.0_±_6.5<br>60.8_±_5.8<br>61.2_±_3.6<br>**62.8**_±_**5.4**<br>**62.8**_±_**5.4**<br>**61.2**_±_**2.7**<br>4475<br>27<br>_S_3<br>62.4_±_5.2<br>**63.6**_±_**4.3**<br>**65.2**_±_**3.0**<br>**65.2**_±_**3.0**<br>59.2_±_4.4<br>61.2_±_4.2<br>61.6_±_2.6<br>59.6_±_3.6<br>4489<br>18<br>_S_4<br>60.0_±_3.7<br>62.4_±_3.6<br>**63.2**_±_**3.4**<br>62.8_±_2.7<br>60.0_±_5.1<br>60.4_±_5.5<br>**64.8**_±_**5.8**<br>62.0_±_6.6<br>4396<br>25<br>Cost_ ↓_<br>All<br>6891<br>5371<br>4871<br>3944<br>4996<br>3594<br>3495<br>2516<br>-<br>W-T_ ↑_<br>All<br>-<br>14<br>**15**<br>14<br>9<br>11<br>13<br>11|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>60.0_±_8.1<br>59.6_±_3.9<br>58.4_±_4.3<br>**60.0**_±_**1.4**<br>60.0_±_5.8<br>**60.4**_±_**5.2**<br>59.6_±_2.6<br>60.0_±_2.0<br>4479<br>17<br>_S_2<br>59.2_±_7.7<br>60.0_±_7.9<br>60.0_±_6.5<br>60.8_±_5.8<br>61.2_±_3.6<br>**62.8**_±_**5.4**<br>**62.8**_±_**5.4**<br>**61.2**_±_**2.7**<br>4475<br>27<br>_S_3<br>62.4_±_5.2<br>**63.6**_±_**4.3**<br>**65.2**_±_**3.0**<br>**65.2**_±_**3.0**<br>59.2_±_4.4<br>61.2_±_4.2<br>61.6_±_2.6<br>59.6_±_3.6<br>4489<br>18<br>_S_4<br>60.0_±_3.7<br>62.4_±_3.6<br>**63.2**_±_**3.4**<br>62.8_±_2.7<br>60.0_±_5.1<br>60.4_±_5.5<br>**64.8**_±_**5.8**<br>62.0_±_6.6<br>4396<br>25<br>Cost_ ↓_<br>All<br>6891<br>5371<br>4871<br>3944<br>4996<br>3594<br>3495<br>2516<br>-<br>W-T_ ↑_<br>All<br>-<br>14<br>**15**<br>14<br>9<br>11<br>13<br>11|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>60.0_±_8.1<br>59.6_±_3.9<br>58.4_±_4.3<br>**60.0**_±_**1.4**<br>60.0_±_5.8<br>**60.4**_±_**5.2**<br>59.6_±_2.6<br>60.0_±_2.0<br>4479<br>17<br>_S_2<br>59.2_±_7.7<br>60.0_±_7.9<br>60.0_±_6.5<br>60.8_±_5.8<br>61.2_±_3.6<br>**62.8**_±_**5.4**<br>**62.8**_±_**5.4**<br>**61.2**_±_**2.7**<br>4475<br>27<br>_S_3<br>62.4_±_5.2<br>**63.6**_±_**4.3**<br>**65.2**_±_**3.0**<br>**65.2**_±_**3.0**<br>59.2_±_4.4<br>61.2_±_4.2<br>61.6_±_2.6<br>59.6_±_3.6<br>4489<br>18<br>_S_4<br>60.0_±_3.7<br>62.4_±_3.6<br>**63.2**_±_**3.4**<br>62.8_±_2.7<br>60.0_±_5.1<br>60.4_±_5.5<br>**64.8**_±_**5.8**<br>62.0_±_6.6<br>4396<br>25<br>Cost_ ↓_<br>All<br>6891<br>5371<br>4871<br>3944<br>4996<br>3594<br>3495<br>2516<br>-<br>W-T_ ↑_<br>All<br>-<br>14<br>**15**<br>14<br>9<br>11<br>13<br>11|<br> <br>MMLU<br>Acc_ ↑_<br>_S_1<br>60.0_±_8.1<br>59.6_±_3.9<br>58.4_±_4.3<br>**60.0**_±_**1.4**<br>60.0_±_5.8<br>**60.4**_±_**5.2**<br>59.6_±_2.6<br>60.0_±_2.0<br>4479<br>17<br>_S_2<br>59.2_±_7.7<br>60.0_±_7.9<br>60.0_±_6.5<br>60.8_±_5.8<br>61.2_±_3.6<br>**62.8**_±_**5.4**<br>**62.8**_±_**5.4**<br>**61.2**_±_**2.7**<br>4475<br>27<br>_S_3<br>62.4_±_5.2<br>**63.6**_±_**4.3**<br>**65.2**_±_**3.0**<br>**65.2**_±_**3.0**<br>59.2_±_4.4<br>61.2_±_4.2<br>61.6_±_2.6<br>59.6_±_3.6<br>4489<br>18<br>_S_4<br>60.0_±_3.7<br>62.4_±_3.6<br>**63.2**_±_**3.4**<br>62.8_±_2.7<br>60.0_±_5.1<br>60.4_±_5.5<br>**64.8**_±_**5.8**<br>62.0_±_6.6<br>4396<br>25<br>Cost_ ↓_<br>All<br>6891<br>5371<br>4871<br>3944<br>4996<br>3594<br>3495<br>2516<br>-<br>W-T_ ↑_<br>All<br>-<br>14<br>**15**<br>14<br>9<br>11<br>13<br>11|
|<br>MATH<br>Acc_ ↑_<br>_S_1<br>30.4_±_3.3<br>**36.0**_±_**1.4**<br>**33.6**_±_**2.2**<br>32.8_±_4.2<br>31.2_±_3.4<br>30.4_±_2.6<br>30.8_±_2.3<br>27.6_±_1.7<br>5362<br>23<br>_S_2<br>31.6_±_6.1<br>29.2_±_5.4<br>30.4_±_6.8<br>28.0_±_3.7<br>**32.4**_±_**3.6**<br>29.2_±_3.9<br>**32.0**_±_**6.0**<br>27.6_±_3.0<br>5369<br>14<br>_S_3<br>32.4_±_6.7<br>**32.8**_±_**7.8**<br>**34.8**_±_**4.8**<br>32.0_±_4.7<br>30.8_±_4.2<br>28.8_±_4.2<br>30.8_±_2.3<br>24.8_±_3.9<br>5343<br>18<br>_S_4<br>**32.0**_±_**4.7**<br>31.2_±_2.7<br>31.2_±_5.2<br>**32.0**_±_**5.1**<br>29.2_±_4.4<br>30.0_±_7.2<br>31.2_±_1.1<br>27.2_±_3.4<br>5238<br>18<br>Cost_ ↓_<br>All<br>6630<br>5814<br>6116<br>5042<br>5915<br>4745<br>4818<br>3540<br>-<br>W-T_ ↑_<br>All<br>-<br>12<br>13<br>9<br>**14**<br>11<br>10<br>4|<br>Acc_ ↑_|_S_1<br>_S_2<br>_S_3<br>_S_4|30.4_±_3.3<br>**36.0**_±_**1.4**<br>**33.6**_±_**2.2**<br>32.8_±_4.2<br>31.2_±_3.4<br>30.4_±_2.6<br>30.8_±_2.3<br>27.6_±_1.7<br>31.6_±_6.1<br>29.2_±_5.4<br>30.4_±_6.8<br>28.0_±_3.7<br>**32.4**_±_**3.6**<br>29.2_±_3.9<br>**32.0**_±_**6.0**<br>27.6_±_3.0<br>32.4_±_6.7<br>**32.8**_±_**7.8**<br>**34.8**_±_**4.8**<br>32.0_±_4.7<br>30.8_±_4.2<br>28.8_±_4.2<br>30.8_±_2.3<br>24.8_±_3.9<br>**32.0**_±_**4.7**<br>31.2_±_2.7<br>31.2_±_5.2<br>**32.0**_±_**5.1**<br>29.2_±_4.4<br>30.0_±_7.2<br>31.2_±_1.1<br>27.2_±_3.4|5362<br>23<br>5369<br>14<br>5343<br>18<br>5238<br>18|
|<br>MATH<br>Acc_ ↑_<br>_S_1<br>30.4_±_3.3<br>**36.0**_±_**1.4**<br>**33.6**_±_**2.2**<br>32.8_±_4.2<br>31.2_±_3.4<br>30.4_±_2.6<br>30.8_±_2.3<br>27.6_±_1.7<br>5362<br>23<br>_S_2<br>31.6_±_6.1<br>29.2_±_5.4<br>30.4_±_6.8<br>28.0_±_3.7<br>**32.4**_±_**3.6**<br>29.2_±_3.9<br>**32.0**_±_**6.0**<br>27.6_±_3.0<br>5369<br>14<br>_S_3<br>32.4_±_6.7<br>**32.8**_±_**7.8**<br>**34.8**_±_**4.8**<br>32.0_±_4.7<br>30.8_±_4.2<br>28.8_±_4.2<br>30.8_±_2.3<br>24.8_±_3.9<br>5343<br>18<br>_S_4<br>**32.0**_±_**4.7**<br>31.2_±_2.7<br>31.2_±_5.2<br>**32.0**_±_**5.1**<br>29.2_±_4.4<br>30.0_±_7.2<br>31.2_±_1.1<br>27.2_±_3.4<br>5238<br>18<br>Cost_ ↓_<br>All<br>6630<br>5814<br>6116<br>5042<br>5915<br>4745<br>4818<br>3540<br>-<br>W-T_ ↑_<br>All<br>-<br>12<br>13<br>9<br>**14**<br>11<br>10<br>4|Cost_ ↓_|All|6630<br>5814<br>6116<br>5042<br>5915<br>4745<br>4818<br>3540|6630<br>5814<br>6116<br>5042<br>5915<br>4745<br>4818<br>3540|
|<br>Chess Move Validity<br>Acc_ ↑_<br>_S_1<br>**22.8**_±_**2.7**<br>**21.6**_±_**3.3**<br>21.2_±_5.6<br>20.8_±_3.0<br>18.8_±_5.4<br>18.8_±_4.6<br>17.6_±_7.0<br>18.8_±_1.1<br>2300<br>9<br>_S_2<br>**22.0**_±_**5.7**<br>18.0_±_2.8<br>18.8_±_3.4<br>16.4_±_2.6<br>**22.0**_±_**8.4**<br>18.8_±_4.8<br>16.0_±_2.8<br>16.0_±_0.0<br>2280<br>10<br>_S_3<br>**21.2**_±_**2.7**<br>20.0_±_3.2<br>18.0_±_2.5<br>18.0_±_2.5<br>**20.0**_±_**2.8**<br>18.8_±_3.0<br>16.4_±_4.6<br>15.6_±_1.7<br>2269<br>9<br>_S_4<br>18.0_±_3.7<br>16.4_±_3.9<br>19.2_±_4.6<br>16.4_±_2.6<br>20.0_±_1.4<br>**20.8**_±_**3.6**<br>**20.4**_±_**3.9**<br>18.8_±_2.3<br>2253<br>23<br>Cost_ ↓_<br>All<br>2956<br>2458<br>2396<br>1973<br>2630<br>2063<br>2083<br>1644<br>-<br>W-T_ ↑_<br>All<br>-<br>7<br>8<br>6<br>9<br>**10**<br>6<br>5|<br>Acc_ ↑_|_S_1<br>_S_2<br>_S_3<br>_S_4|**22.8**_±_**2.7**<br>**21.6**_±_**3.3**<br>21.2_±_5.6<br>20.8_±_3.0<br>18.8_±_5.4<br>18.8_±_4.6<br>17.6_±_7.0<br>18.8_±_1.1<br>**22.0**_±_**5.7**<br>18.0_±_2.8<br>18.8_±_3.4<br>16.4_±_2.6<br>**22.0**_±_**8.4**<br>18.8_±_4.8<br>16.0_±_2.8<br>16.0_±_0.0<br>**21.2**_±_**2.7**<br>20.0_±_3.2<br>18.0_±_2.5<br>18.0_±_2.5<br>**20.0**_±_**2.8**<br>18.8_±_3.0<br>16.4_±_4.6<br>15.6_±_1.7<br>18.0_±_3.7<br>16.4_±_3.9<br>19.2_±_4.6<br>16.4_±_2.6<br>20.0_±_1.4<br>**20.8**_±_**3.6**<br>**20.4**_±_**3.9**<br>18.8_±_2.3|2300<br>9<br>2280<br>10<br>2269<br>9<br>2253<br>23|
|<br>Chess Move Validity<br>Acc_ ↑_<br>_S_1<br>**22.8**_±_**2.7**<br>**21.6**_±_**3.3**<br>21.2_±_5.6<br>20.8_±_3.0<br>18.8_±_5.4<br>18.8_±_4.6<br>17.6_±_7.0<br>18.8_±_1.1<br>2300<br>9<br>_S_2<br>**22.0**_±_**5.7**<br>18.0_±_2.8<br>18.8_±_3.4<br>16.4_±_2.6<br>**22.0**_±_**8.4**<br>18.8_±_4.8<br>16.0_±_2.8<br>16.0_±_0.0<br>2280<br>10<br>_S_3<br>**21.2**_±_**2.7**<br>20.0_±_3.2<br>18.0_±_2.5<br>18.0_±_2.5<br>**20.0**_±_**2.8**<br>18.8_±_3.0<br>16.4_±_4.6<br>15.6_±_1.7<br>2269<br>9<br>_S_4<br>18.0_±_3.7<br>16.4_±_3.9<br>19.2_±_4.6<br>16.4_±_2.6<br>20.0_±_1.4<br>**20.8**_±_**3.6**<br>**20.4**_±_**3.9**<br>18.8_±_2.3<br>2253<br>23<br>Cost_ ↓_<br>All<br>2956<br>2458<br>2396<br>1973<br>2630<br>2063<br>2083<br>1644<br>-<br>W-T_ ↑_<br>All<br>-<br>7<br>8<br>6<br>9<br>**10**<br>6<br>5|Cost_ ↓_|All|2956<br>2458<br>2396<br>1973<br>2630<br>2063<br>2083<br>1644|2956<br>2458<br>2396<br>1973<br>2630<br>2063<br>2083<br>1644|



Table 32: The impact of eight different collaborative strategies on the performance of three datasets across distinct societies ( _using_ _Mixtral-8×7B_ ). The significances test on societies and strategies are respectively shown in
Table 33, 34. The experiments of comparison with the single LLM agent is shown in Figure 67(a)-(f).


_**MMLU**_ _**Math**_ _**Chess Move Validity**_







Figure 54: Comparative word clouds on three datasets in societies _S_ 1 and _S_ 4, using _Mixtral-8_ _7B_ . Society _S_ 1
_×_
features three overconfident agents, while society _S_ 4 comprises three easy-going agents.


Collaborative MMLU MATH Chess Move Validity
Strategy p-value p-value p-value



_p_ 0 _p_ 0 _p_ 0 0.873 0.941 0.261
_p_ 0 _p_ 0 _p_ 1 0.578 0.216 0.109
_p_ 0 _p_ 1 _p_ 0 0.114 0.500 0.666
_p_ 0 _p_ 1 _p_ 1 0.142 0.347 0.062
_p_ 1 _p_ 0 _p_ 0 0.930 0.638 0.809
_p_ 1 _p_ 0 _p_ 1 0.863 0.949 0.825
_p_ 1 _p_ 1 _p_ 0 0.325  - 0.485
_p_ 1 _p_ 1 _p_ 1 0.785 0.438 0.004


Table 33: One-Way ANOVA results for the impact
of society on accuracy with fixed collaborative strategy, based on experiments from Table 32 using _Mixtral_
_8×7B_ . ‘-’: It doesn’t pass homogeneity test for variance.



MMLU MATH Chess Move Validity
Society p-value p-value p-value

_S_ 1 0.999 0.002 0.585
_S_ 2 0.970 0.693 0.202
_S_ 3 0.129 0.127 0.078
_S_ 4 0.706 0.714 0.300


Table 34: One-Way ANOVA results for the impact
of collaborative strategy on accuracy with fixed society, based on experiments from Table 32 using _Mixtral_
_8×7B_ .



14597


_×_
Society _S_ 1 features three overconfident agents, while society _S_ 4 comprises three easy-going agents.


Figure 56: Illustration of different collaborative strategies impacting accuracy diversely on the tasks considering
varied _subjects_ and _difficulty_, using _Mixtral-8×7B_ . The symbol ‘ ’ represents that there is at least one collaborative strategy whose accuracy is better than self-consistency, while the symbol ‘ ’ indicates that there is no
collaborative strategy whose accuracy is worse than self-consistency. Both of these symbols represent the accuracy
of self-consistency. The accuracy under each collaborative strategy is a summation within all 3-agent societies.


14598


|Col1|Col2|Col3|Col4|Col5|Col6|
|---|---|---|---|---|---|
|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|m<br>(<br>)|


|Col1|Col2|
|---|---|
|m<br>(<br>)<br>ccuracy of different numbers (2_∼_10) of agents under different collaborative strategies, <br> niﬁcance test is shown in Table 35.|m<br>(<br>)<br>ccuracy of different numbers (2_∼_10) of agents under different collaborative strategies, <br> niﬁcance test is shown in Table 35.|


14599


_8×7B_ .


14600


( ) m
Figure 59: Average quantity of _consensus clusters (unique answers among multiple agents)_ in _different societies_
with 2 _∼_ 10 agents under each round of 3-round collaborative strategies, using _Mixtral-8×7B_ .







Figure 60: Average ratio of _consensus_ _clusters_ _(unique_ _answers_ _among_ _multiple_ _agents)_ with _different_ _numbers_
_(2∼10) of agents_ under each round of 3-round collaborative strategies, using _Mixtral-8×7B_ .

14601


4
(p1)



65


60


55


65

60

55



3
(p1)


3
(p0)





3
(p1)


3
(p1)



3
(p0)


3
(p0)



4
(p1)



4
(p1)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



65


60


55


65


60


55



4
(p1)



4
(p0)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



65


60


55

3
(p0)


65

60

55

3
(p1)



4
(p0)



5
(p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)



70


60


65


60



4
(p0)



4
(p0)



5
(p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)











5
(p0)



6
(p1)



7
(p0)



8
(p1)



9
(p0)



10
(p1)


10
(p1)



5
(p1)



6
(p0)



7
(p1)



8
(p0)



9
(p1)



10
(p1)


10
(p0)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



10
(p0)


10
(p1)



5
(p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)



10
(p0)


10
(p0)



Figure 61: Accuracy of _different_ _(3_ _10)_ _rounds_ _of_ _collaboration_ within 3-agent society _S_ 2 (1 easy-going and 2
_∼_
overconfident agents) on MMLU, using _Mixtral-8×7B_ . The significance test is shown in Table 36.



40


30


20


40


30



3
(p1)


3
(p0)





3
(p1)


3
(p1)





3
(p0)


3
(p0)







4
(p1)



4
(p1)



4
(p1)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



40


30


40


30



4
(p1)



4
(p0)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



40


30


3
(p0)


30


20

3
(p1)



4
(p0)



5
(p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)



35


30


25


35

30

25



4
(p0)



4
(p0)



5
(p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)











5
(p0)



6
(p1)



7
(p0)



8
(p1)



9
(p0)



10
(p1)


10
(p1)



5
(p1)



6
(p0)



7
(p1)



8
(p0)



9
(p1)



10
(p1)


10
(p0)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



10
(p0)


10
(p1)



5
(p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)



10
(p0)


10
(p0)



Figure 62: Accuracy of _different_ _(3_ _10)_ _rounds_ _of_ _collaboration_ within 3-agent society _S_ 2 (1 easy-going and 2
_∼_
overconfident agents) on MATH, using _Mixtral-8×7B_ . The significance test is shown in Table 36.











3
(p0)


3
(p0)



3
(p1)


3
(p1)



20.0


17.5


25


20



3
(p1)


3
(p0)



4
(p1)



4
(p1)



4
(p1)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



25


20


25


20



4
(p1)



4
(p0)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



25


20

3
(p0)


22.5


20.0


17.5


3
(p1)



4
(p0)



5
(p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)



30


20


25


20



4
(p0)



4
(p0)



5
(p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)











5
(p0)



6
(p1)



7
(p0)



8
(p1)



9
(p0)



10
(p1)


10
(p1)



5
(p1)



6
(p0)



7
(p1)



8
(p0)



9
(p1)



10
(p1)


10
(p0)



5
(p1)



6
(p1)



7
(p1)



8
(p1)



9
(p1)



10
(p0)


10
(p1)



5
(p0)



6
(p0)



7
(p0)



8
(p0)



9
(p0)



10
(p0)


10
(p0)



Figure 63: Accuracy of _different_ _(3_ _10)_ _rounds_ _of_ _collaboration_ within 3-agent society _S_ 2 (1 easy-going and 2
_∼_
overconfident agents) on Chess Move Validity, using _Mixtral-8×7B_ . The significance test is shown in Table 36.


14602


Collaborative _S_ 1 _′_ _S_ 2 _′_ _S_ 3 _′_ _S_ 4 _′_ _S_ 5 _′_
Strategy p-value p-value p-value p-value p-value

_p_ 0 _p_ 0 _p_ 0 0.188 0.406 0.235 0.805 0.009
_p_ 0 _p_ 0 _p_ 1 0.106 0.112 0.238 0.459 0.008
_p_ 0 _p_ 1 _p_ 0 0.142 0.145 0.227 0.739 0.227
_p_ 0 _p_ 1 _p_ 1 0.013 0.004 0.035 0.138 0.075
_p_ 1 _p_ 0 _p_ 0 0.159 0.082 0.105 0.018 0.088
_p_ 1 _p_ 0 _p_ 1 0.029 0.003 0.002 0.004 0.018
_p_ 1 _p_ 1 _p_ 0 0.051 0.028 0.010 0.001 0.247
_p_ 1 _p_ 1 _p_ 1 0.002 0.016 0.003 0.000 0.001


Table 35: One-way ANOVA analysis of results in Figure 57 (different numbers of agents), using _Mixtral_
_′_
_8×7B_ . _S_ 1 [:] [One overconfident agent and the others are] _′_
all easygoing. _S_ 2 [:] [One] [easygoing] [agent] [among] [pre-]
_′_
dominantly overconfident agents. _S_ 3 [:] [Equal] [numbers]
_′_
of overconfident and easygoing agents. _S_ 4 [:] [Entirely]
_′_
easygoing agents. _S_ 5 [:] [Entirely overconfident agents.]


Collaborative MMLU MATH Chess Move Validity
Strategy p-value p-value p-value

_p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 0.607 0.911 0.789
_p_ 1 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 0.578 0.581 0.939
_p_ 0 _p_ 1 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 _p_ 0 0.936 0.665 0.123
_p_ 1 _p_ 0 _p_ 1 _p_ 0 _p_ 1 _p_ 0 _p_ 1 _p_ 0 _p_ 1 _p_ 0 0.377 0.896 0.952
_p_ 0 _p_ 1 _p_ 0 _p_ 1 _p_ 0 _p_ 1 _p_ 0 _p_ 1 _p_ 0 _p_ 1 0.987 0.651 0.271
_p_ 1 _p_ 0 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 0.989 0.878 0.919
_p_ 0 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 0.989 0.982 1.000
_p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 _p_ 1 0.945 0.995 0.903


Table 36: One-way ANOVA analysis of the results
in Figure 61, 62, 63 (different rounds), using _Mixtral_
_8×7B_ .

MMLU


65


60


55



Collaborative MMLU MATH Chess Move Validity
Strategy p-value p-value p-value

_p_ 0 _p_ 0 _p_ 0 0.618 0.898 0.390
_p_ 0 _p_ 0 _p_ 1 0.919 0.143 0.058
_p_ 0 _p_ 1 _p_ 0 0.797 0.548 0.031
_p_ 0 _p_ 1 _p_ 1 0.521 0.141 0.049
_p_ 1 _p_ 0 _p_ 0 0.040 0.409 0.290
_p_ 1 _p_ 0 _p_ 1 0.658 0.400 0.373
_p_ 1 _p_ 1 _p_ 0 0.193 0.318 0.142
_p_ 1 _p_ 1 _p_ 1 0.536 0.453  

Table 37: One-way ANOVA analysis of results in
Figure 64 (other collaborative strategies), _on_ _Mixtral_
_8×7B_ . ‘-’ means it doesn’t pass homogeneity test for
variance.



p0p0p0
All Part



0p0 p0p0p1

AllPart Part



0p1 p0p1p0

AllPart Part



1p0 p0p1p1

AllPart Part



1p1 p1p0p0

AllPart Part



0p0 p1p0p1

AllPart Part



0p1 p1p1p0

AllPart Part



1p0 p1p1p1

AllPart Part



MATH



35


30


25


20



p0p0p0
All Part



0p0 p0p0p1

AllPart Part



0p1 p0p1p0

AllPart Part



1p0 p0p1p1

AllPart Part



1p1 p1p0p0

AllPart Part



0p0 p1p0p1

AllPart Part



0p1 p1p1p0

AllPart Part



1p0 p1p1p1

AllPart Part



Chess Move Validity



30


25


20


15



p0p0p0
All Part



0p0 p0p0p1

AllPart Part



0p1 p0p1p0

AllPart Part



1p0 p0p1p1

AllPart Part



1p1 p1p0p0

AllPart Part



0p0 p1p0p1

AllPart Part



0p1 p1p1p0

AllPart Part



1p0 p1p1p1

AllPart Part



Figure 64: The effect on the accuracy of whether all
agents in society execute the same thinking pattern in
one round, using _Mxitral-8×7B_ . “All” and “Part” refers
to all agents applying the same thinking pattern and different thinking patterns in one round respectively. The
significance test is shown in Table 37.


14603


_about detriments_ : Ratio(False _→_ True + True _→_ True) _<_ Ratio(True _→_ False + False _→_ False).


Figure 66: Average quantity of _consensus_ _clusters_ _(i.e.,_ _unique_ _answers_ _among_ _multiple_ _agents)_ under different
rounds of collaboration with 3-round collaborative strategies, using _Mixtral-8×7B_ . _Smaller quantity of consensus_
_clusters, more easier it is to reach a consensus._ Round 0 is equal to self-consistency.


Figure (a-c) & (d-f) respectively show the token cost and accuracy of different strategies before and after 3-round
collaboration. Figure (g-i) present the percentage of different behavioral features (mainly analyzed by the change
of answer correctness) (Zhang et al., 2023b,a) under different collaborative strategies. All results are summarized
across all societies.


14604


**I** **Assessing the Effectiveness of Prompts**


In this section, we conduct a sanity check to ensure that the agents’ actions reflect align with our
instruction, such as reflecting easy-going or overconfident traits.
Prompts play a critical role in our experiments
and are the primary focus of this sanity check.
The word cloud analysis presented in Figure 11
supports the appropriateness of the “easy-going”
prompt. Consequently, confirming the effectiveness of the “overconfident” prompt is crucial. We
use “reflecting the _overconfident_ trait” as a case
study to explore the validity of our prompts. In the
absence of established validation techniques, we
combine experimental results and experiential insights to evaluate prompt effectiveness from three
perspectives:


  - **Granularity** **of** **Description.** As illustrated
in Table 5, we describe two behaviors, _i.e._,
“being confident in your answer” and “persuading other agents to believe in you”, both
aligning with the behavioral facets of “overconfident”.


  - **Model** **Response.** We employ the role-play
method to prompt the model and subsequently
inquire its awareness, as illustrated in Table 5.
If the prompts potentially instruct the model
to generate harmful content, the model refuses
to comply with the prompt. Upon reviewing
our logs, it is noteworthy that the model ac


cepted all prompts without refusal. Instead, it
responded with “OK” as corroborated by the
‘role-play’ part in Figure 9 and Figure 10.


  - **Ask** **Again.** We revisit the initial scenarios
and responses using the role-play format. We
then query the model again: “If one agent’s
answer differs from yours, what should you
do?” The model replies: “In a situation where
another agent’s answer differs from mine, I
should respectfully present my perspective,
_providing supporting evidence or reasoning_
_to_ _demonstrate_ _confidence_ _in_ _my_ _response._
It’s important to engage in constructive dialogue and potentially find common ground,
but _maintaining clarity and conviction in my_
_position is crucial to persuading others to con-_
_sider my viewpoint._ ” This response, emphasizing overconfidence, demonstrates the rationality of our prompts. The complete conversation
record is illustrated in Figure 68.
**Example** **Analysis.** We further validate the
“Ask Again” method with specific interactions involving an overconfident and an easy-going agent,
by showing a concrete example. We provide the
conversation records with an overconfident agent
and an easy-going agent, respectively in Figure 69
and Figure 70. Despite the model’s response being incorrect and our prompted answer being accurate, the model consistently maintains its viewpoint,
reaffirming our prompt design’s effectiveness.



14605


Imagine you are an expert in biology, chemistry, computer science, mathematics,
physics and are _**confident in your answer**_ and often _**persuades other agents to**_
_**believe in you**_ . Please keep this in mind. If you understand please say ok only.


Ok.


If one agent’s answer differs from yours, what should you do?



In a situation where another agent’s answer differs from mine, I should respectfully
present my perspective, _**providing supporting evidence or reasoning to**_
_**demonstrate the confidence in my response**_ . It’s important to engage in constructive dialogue and potentially find common ground, but _**maintaining clarity**_
_**and conviction in my position is crucial to persuading others to consider my**_
_**viewpoint**_ .


Figure 68: The conversation record with an _overconfident_ agent.





Imagine you are an expert in biology, chemistry, computer science, mathematics,
physics and are _**confident in your answer**_ and often _**persuades other agents to**_
_**believe in you**_ . Please keep this in mind. If you understand please say ok only.


Ok.



Can you answer the following question as accurately as possible?
Which of the following has an octet of electrons around the central atom?
A. BF3 B. NH4+ C. PF5 D. SF6
Explain your answer, putting the answer in the form (X) at the end of your
response.



_**The**_
_**answer**_
_**is B.**_



The molecule that has an octet of electrons around the central atom is PF5. Phosphorus (P) has 5 valence electrons, and each fluorine (F) atom contributes 1
electron. Therefore, the phosphorus atom in PF5 has 10 electrons around it,
resulting in an octet. (C)


You are not right. The answer is B.


I understand your perspective, but _**I'm confident that the correct answer is C**_ .
PF5. Phosphorus pentafluoride (PF5) has an octet of electrons around the central
phosphorus atom, satisfying the octet rule. This allows the phosphorus atom to
have a stable electron configuration.



Figure 69: The conversation record with an _overconfident_ agent, given a specific example of “Ask Again”.


14606


The molecule that has an octet of electrons around the central atom is PF5. Phosphorus (P) has five valence electrons, and in PF5, it forms five covalent bonds
with fluorine (F) atoms, resulting in a total of 10 electrons around the phosphorus
atom, fulfilling the octet rule. Therefore, the correct answer is C. PF5. (C)


You are not right. The answer is B.


I _**apologize for the mistake.**_ Upon further consideration, I can see that the correct
answer is indeed B. NH4+. In the ammonium ion (NH4+), the central nitrogen
atom has four covalent bonds with hydrogen atoms, resulting in a total of eight
electrons around the nitrogen atom, fulfilling the octet rule. Thank you for pointing
that out. The correct answer is B. NH4+. (B)



You are an expert in biology, chemistry, computer science, mathematics, physics
and are _**objective and unbiased**_, and you _**can be persuaded**_ if other agent’s
answers make sense. Please keep this in mind. If you understand please say ok.


Ok.



Can you answer the following question as accurately as possible?
Which of the following has an octet of electrons around the central atom?
A. BF3 B. NH4+ C. PF5 D. SF6
Explain your answer, putting the answer in the form (X) at the end of your
response.



_**The**_
_**answer**_
_**is B.**_



Figure 70: The conversation record with an _easy-going_ agent, given a specific example of “Ask Again”.


14607


