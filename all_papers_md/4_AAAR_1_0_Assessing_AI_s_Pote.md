# **AAAR-1.0: Assessing AI’s Potential to Assist Research**

**Anonymous submission**



**Abstract**


Numerous studies have assessed the proficiency of AI systems,
particularly large language models (LLMs), in facilitating everyday tasks such as email writing, question answering, and
creative content generation. However, researchers face unique
challenges and opportunities in leveraging LLMs for their own
work, such as brainstorming research ideas, designing experiments, and writing or reviewing papers. In this study, we introduce AAAR-1.0, a benchmark dataset designed to evaluate
LLM performance in three fundamental, expertise-intensive
research tasks: (i) EQUATIONINFERENCE, assessing the correctness of equations based on the contextual information in
paper submissions; (ii) EXPERIMENTDESIGN, designing experiments to validate research ideas and solutions; and (iii)
PAPERWEAKNESS, identifying weaknesses in paper submissions. AAAR-1.0 differs from prior benchmarks in two key
ways: first, it is explicitly research-oriented, with tasks requiring deep domain expertise; second, it is researcher-oriented,
mirroring the primary activities that researchers engage in on
a daily basis. An evaluation of both open-source and proprietary LLMs reveals their potential as well as limitations in
conducting sophisticated research tasks. We will release the
AAAR-1.0 and keep iterating it to new versions.


**Introduction**


Although AI has brought transformative changes to various aspects of life, its impact on researchers unfolds in a
nuanced manner. On the one hand, AI assists in various research disciplines, such as Social Science, Finance, Medicine,
GeoScience, Math, etc.(Yue et al. 2023; Li et al. 2023b), significantly expediting academic processes. However, many
of these applications are superficial, often limited to datadriven clustering or classification. On the flip side, the AI
era poses challenges for researchers. Despite its ability to
streamline some activities, researchers still face demanding,
cognitively intensive tasks such as staying current through extensive paper reading, rapidly generating ideas in response to
fast-paced advancements, conducting rigorous experiments
to substantiate claims, and managing an increasing volume
of peer reviews. Then a question looms: How effectively
can AI assist researchers in tasks that are domain-specific,
expertise-demanding, and knowledge-intensive?
Existing works proved the promising potential for using
LLMs in assisting AI research. Si, Yang, and Hashimoto



**Task #2: Experiment Design**


Figure 1: The input-output illustration of three tasks in the

proposed AAAR-1.0 benchmark.


(2024) conducted a large-scale human study and found that
LLMs can generate creative research ideas. Lu et al. (2024)
proposed an autonomous agent to handle complicated research workflow and write a whole research paper. However,
most of these works focus on addressing highly subjective
problems that require a high degree of expertise, making evaluation laborious and hard to reproduce. This underscores the
need for a comprehensive benchmark that rigorously assesses
LLMs’ capabilities in expertise-intensive research activities

To this end, in this work, we introduce AAAR-1.0, a
novel benchmark that aims to comprehensively assess the
LLMs’ capacity on expert-level research tasks. As illustrated
in Figure 1, AAAR-1.0 decomposes three distinct expertlevel AI research tasks from the researcher’s daily activities,
including i) EQUATIONINFERENCE, investigating whether
the LLMs can infer the equation correctness based on the
paper context; ii) EXPERIMENTDESIGN, validating LLMs’
ability on designing reliable experiments for a research idea;
iii) PAPERWEAKNESS, testing the quality of the weaknesses
criticism written by the LLMs. To ensure data quality, senior



**Task Instruction**

_Given the context of a paper, identify the missed_
_equation from the provided options (A, B, C, D)._







**Task Instruction**

_Given a paper, critique the weaknesses_
_within this research work._


**Paper Context**

_Title: Metric is All You Need_
_Abstract: Deep learning has been […]_
_Introduction: We propose a […]_


_1._ _Missed references […]_
_2._ _Insufficient experiments […]_
_3._ _Missed running details […]_



_(A). z = W*a+b_



**Weaknesses**
**Answer**



**Task #1: Equation Inference**


**Task Instruction**

_Given a partial paper, create a brief_
_experiment plan and explanations._



**Task #3: Paper Weakness**


_1._ _Compare performance on […]_
_2._ _Ablation study with […]_
_3._ _Significance test […]_

**Experiment Plan**


_1._ _To prove the effectiveness[…]_
_2._ _To study the impact of […]_
_3._ _To avoid randomness in […]_

**Motivation Explanation**


**Task #1: EQINFER** **Task #2: EXPDESIGN** **Task #3: WEAKNESS**















**1**



Source LaTeX

**Data Crawling and Cleaning**


**z = W*x*y**











**z = W*b + a**











**2**



**LLM-based Equation Synthesis** **3** **LLM-based Filtering**





Experts



**4**



**Expert-based Examination**



Figure 2: Data construction workflows of the three tasks in AAAR-1.0.



AI researchers with extensive domain expertise perform data
annotation for AAAR-1.0, followed by rigorous multi-round
data examination and filtering. All three tasks require models
to possess strong domain knowledge covering various cuttingedge research findings, as well as expert-level research experience, to the extent that even humans need substantial
research accumulation to tackle the tasks we designed. Crucially, tasks here are singular, stand-alone challenges (with
clear input and output expectations) rather than a complicated
task chain (Li et al. 2024; Lu et al. 2024), providing a more
transparent assessment of the model’s intermediate output.


Benefiting from the proposed automatic metrics, we conduct extensive experiments across numerous mainstream
LLMs, where we find that:


  - Closed-source LLMs generally outperform open-source
LLMs on AAAR-1.0, likely due to their richer scientific
knowledge stemming from a larger model size.


  - Contrary to human behaviour, neither extending the
input modality (i.e., leveraging text and figures) nor
enlarging the input context guarantees enhanced performance. This underlines most current LLMs’ limitations
in processing diverse, extensive information coming
from scientific documents.


  - LLM-designed experiments are innovative and more
diverse than those by humans; however, many are trivial,
lack feasibility, and stray from the original research
objectives.


  - LLM-generated weaknesses often lack ample domain
knowledge, especially on cutting-edge research topics,
leading to the vague weaknesses applicable to various
papers.



**AAAR-1.0**


Figure 2 provides an overview of constructing AAAR1.0. In the following sections, we elaborate on the
data collection details of the aforementioned three
tasks, including EQUATIONINFERENCE ( **EQINFER** ), EX
PERIMENTDESIGN ( **EXPDESIGN** ), and PAPERWEAKNESS ( **WEAKNESS** ).


**EQUATIONINFERENCE**


Writing a correct scientific equation is challenging because
it involves an in-depth understanding towards an algorithm
or the relations between the massive notations. However, directly asking LLMs to generate equations is over-challenging.
For this reason, in this task, we adopt the conventional multichoice classification paradigm for building EQINFER, as
shown in Figure 1.


① **Data crawling and cleaning.** For the data source, we
adopt the pre-compilation LaTeX code for two reasons: i)
existing PDF parsing tools, such as PyMuPDF and PaperMage (Lo et al. 2023), can introduce considerable noise to the
parsed equation text; ii) considering most of exiting LLMs are
capable with processing LaTeX code, using LaTeX source
instead of parsed text can be more accurate and provide
LLMs with richer information. Meanwhile, to avoid using
any low-quality human-written equations, we only crawl
those peer-reviewed papers accepted by top-tier conferences.
Accordingly, we first obtain the accepted paper list from ACL
Anthology, from year 2019 to 2023. Next, we search each
paper on arXiv to crawl its LaTeX source (if it exists). Finally,
we get a total of 1,762 papers’ source LaTeX packages.
We then clean the LaTeX sources by deleting all the comments and combining multiple cross-referred .tex files into
a main file. Afterwards, we use regex to randomly extract (at
most) 3 equations’ code snippets per paper, finally resulting


in 3,877 human-written equations are extracted.


② **LLM-based equation synthesis.** As we formulate this
task as classification, for each human-written positive equation, we have to craft at least three counterpart negative equations. To this end, we prompt GPT-4 to synthesize more equations based on the paper context. For each positive equation,
we repeat this prompting (with a high decoding temperature)
until three different negative equations are synthesized.


③ **LLM-based** **filtering.** However, the LLM-synthetic
equations can sometimes be context-unaligned, i.e., some
synthesized equations contain notations that are never defined
in the paper context, which is a superficial shortcut for the
classification tasks (Geirhos et al. 2020). To improve the data
quality, we prompt GPT-4 to identify those context-unaligned
negative equations. We then discard those instances where
all three negative equations are identified as contextually unaligned. This filtering leads to a final of 1,449 classification
instances (62.3% instances are filtered).


④ **Expert-based examination.** Furthermore, it’s also possible that synthesized negative equations are actually correct
(i.e., false negative options) — even if the negative and positive equations are written differently, the final compiled
results might be the same. To filter out the false negative
equations and to have a final check on the classification instances, we then employ human experts to conduct a further
data review.
We asked 5 senior PhD students who are experienced in AI
research to manually check all the instances. For each classification instance, we ask human experts to consider the following criteria: i) **are all four equations (both positive and**
**negative) grammatically correct?** ii) **after compilation, is**
**there** **only** **one** **correct** **answer?** We ask every human expert to use external LaTeX compilation tools (e.g., TeXlive),
and identify the instances that cannot meet the criteria. Each
instance is examined by at least two experts, and we only
keep instances that all experts decide to keep. After this strict
examination, a total of 1,049 instances are eventually kept
(27.6% instances are filtered)


**Final data.** We finally shuffle the four equations for each
classification instance and randomly assign letters (A, B, C,
and D) to the equations. We show the data statistics of the
final EQINFER in Table 4 and the sample data cases in Appendix .


**EXPERIMENTDESIGN**

Given a research topic, such as a novel ML algorithm, a
qualified researcher can design a solid experiment plan for
it, and clarify underlying motivation to ensure the reliability
of the designed experiment. Unlike the concurrent works
that focus on the experiment implementation (Lu et al. 2024;
Huang et al. 2024), we emphasize the importance of assessing the high-level experiment design of LLMs before the
subsequent implementation to avoid any expensive execution iteration. Therefore, as shown in Figure 1, we formulate EXPDESIGN as a text-generation task that takes preexperiment paper context as input, and then generates the
experiment and explanation list.



① **Data crawling.** As for the data source, we first collect
_≥_ 10k papers’ data from arXiv, including LaTeX sources and
PDFs, which cover broad AI categories, including cs.AI,
cs.CL, and cs.CV, from year 2018 to 2023. Similarly, to
ensure the source data quality, we only use papers that have
appeared at well-known conferences.


② **Domain-expert annotation.** Making a reliable and executable experiment plan requires solid foundation knowledge
of a specific research area. Consequently, we set a high standard for choosing annotators: i) be a senior PhD student with
at least one peer-reviewed publication; ii) have more than 4
years of AI research experience; iii) frequently serve as conference reviewers. Finally, we invite a total of 10 qualified
experts to participate in our data collection procedure. Given
the 10k crawled papers, we first ask every annotator to bid on
the papers that they are interested in. After bidding, each of
them is assigned 10 papers by us, i.e., a total of 100 papers to
be annotated. During annotation, we post each paper PDF on
online Google Drive and ask the annotator to first carefully
read the whole paper. Then, we ask them to identify and locate the key experiments in each paper (i.e., highlighting the
relevant paragraphs of each experiment). We don’t consider
some trivial experiments, such as those supplemental analyses in the appendix section. For each identified experiment,
the annotator has to concisely answer two questions: i) **what**
**did** **this** **experiment** **do?** ii) **why** **did** **the** **paper** **authors**
**conduct** **this** **experiment?** In other words, we ask the annotator to summarize all the key experiments in this paper
and explain the underlying motivations based on their rich
domain experience.


③ **Multi-round peer discussion.** Intuitively, different experts might have different opinions on the same research
topic. Particularly, when explaining the underlying motivation of an experiment, adopting only a single expert’s opinion
might introduce bias to our annotation. Hence, we conduct
a further multi-round peer discussion. For each online paper
PDF, where all the key experiments are identified, summarized, and explained, we ask a different expert (reviewer)
to review the annotation by considering the following three
criteria: i) **are the identified experiments all the key exper-**
**iments?** ii) **does each experiment summarization covers**
**all key information?** iii) **does each explanation sound rea-**
**sonable and reliable?** Each reviewer has to leave comments
to the online PDF regarding the above criteria, and then the
annotator has to respond to each comment — either accept
the suggestion and revise the previous annotation, or provide
a “rebuttal” to the reviewer to uphold the annotation. This
discussion iterates until both opinions align with each other.
Eventually, for each paper, we collect two lists: i) the experiment list, summarizing each experiment step of the paper;
ii) the explanation list, the underlying motivations that are
one-one corresponding to the experiment.


**Final data.** After annotation, we use the pre-experiment
context of each paper (according to the first-experiment location identified by the annotator) as the input. Furthermore,
we use GPT-4 to delete any sentence that potentially leaks


the experiment from the input. [1] Similar to the EQINFER, we
utilize the source LaTeX as the input text to avoid PDF paring
noise. As for the image input, we collect those figures within
each paper’s source LaTeX package and only keep figures
that are used in the pre-experiment context. Overall, a total of
100 instances are collected. As shown in Figure 1, the input
of each instance is the pre-experiment context (including the
figures), and the ground-truth output is the expert-annotated
experiment plan and the explanations. Table 5 shows data
statistics.


**PAPERWEAKNESS**


Another critical research task is paper review. Previous works
have demonstrated the usefulness of the LLM-based review
feedback (Gao, Brantley, and Joachims 2024; Jin et al. 2024;
Lu et al. 2024). However, as indicated by Du et al. (2024);
Liang et al. (2024), LLMs only excel at summarizing the
research strengths while falling significantly short on weakness criticism. Hence, we build WEAKNESS for particularly
investigating the LLM-generated weaknesses.


① **Data** **crawling.** We first crawl a total of 3,779 anonymous submissions of _ICLR 2023_ from OpenReview, [2] including PDF and other meta information (e.g., scores, decisions,
and tracks). As the _ICLR 2023_ has 13 distinct tracks while
the paper distribution across different tracks is highly biased,
we then uniformly sample papers from different research
tracks to improve the domain diversity. Meanwhile, during
sampling, we also keep the accept/reject papers distributed
equally to avoid data bias. In a word, we finally collect a
total of 1,000 papers (500 accepted; 500 rejected), uniformly
covering all 13 tracks. Please refer to Figure 3 for the track
and score distribution of the 1,000 papers.


② **LLM-based weakness extraction.** Since the raw comments crawled from _ICLR 2023_ are mixed with both strengths
and weaknesses, we further employ GPT-4 to extract all the
weaknesses from each reviewer’s comments and compose
multiple weaknesses into a list. Notably, we force GPT-4 to
keep the original text of the reviewer, i.e., all weaknesses
in our dataset are those original sentences written by the reviewer without any modifications. [3] What’s more, sometimes
one reviewer might repeatedly mention the same weakness
throughout the comment. In this case, we simply keep all
the repeated weaknesses because, if one weakness is repeatedly mentioned by the reviewer, it’s intuitively an important
weakness that the reviewer wants to emphasise; accordingly,
keeping the repeat items can penalize LLMs more on missing
this weakness.
All in all, for each paper, we can finally get multiple weakness lists (one weakness list per reviewer, one paper can have
multiple reviewers). We further delete a few papers without


1About 9.8% sentences are deleted.
2We adopt ICLR because it releases full submissions, while
some other conferences only release accepted papers.
3We manually checked GPT-4’s extraction results of 200 cases

- GPT-4 only missed _≤_ 1% of reviewer-written weaknesses and
maintained almost all the original text.



any weaknesses found in the raw comments, resulting in a
total of 993 instances, i.e., 993 {paper, weakness lists} pairs.


③ **Input** **data** **processing.** As we mentioned before, we
crawl papers from OpenReview instead of arXiv because the
under-review paper draft is required for this task. However,
not every paper from OpenReview can be found on arXiv,
i.e., the source LaTeX code and figures of most under-review
papers are unavailable. Therefore, we utilize VILA (Lin et al.
2023) to parse text data out from the PDF; we also employ
PDFFigures-2.0 (Clark and Divvala 2016) to extract all the
figures and tables (in image) from the paper, as Vila is not
good at processing the table data.


**Final data.** Our final data is composed of 993 instances,
each input is paper text along with figure/table images, and
each output is peer reviewers’ weakness lists. Table 6 shows
data statistics.


**Evaluation Criteria**


For EQINFER, we adopt accuracy as the classification criterion. For EXPDESIGN and WEAKNESS, since both tasks have
natural language outputs, semantic-based metrics are necessary. Hence, in addition to the conventional ROUGE (Lin
2004), we also develop several novel similarity-based metrics
for each specific task, including:


  - **S-F** 1 (equation 1 and 2): similarity-based F1 for assessing the experiment design quality. It measures how well
each model-generated experiment aligns with the human
experiments.

  - **S-Match** (equation 3): “soft” match score for evaluating the explanation. It calculates the similarity between
human and model-generated explanations.

  - **SN-F** 1 (equation 4 and 5): updated version of S-F1 to
deal with the “nested” review weaknesses.

  - **ITF-IDF** (equation 6): inspired by the classic TF-IDF;
measures the inter- and intra-paper diversity of modelgenerated weaknesses.


We sincerely recommend referring to Appendix for the
formal equation definitions of the above metrics.


**Experiments and Analyses**


In this section, we conduct extensive experiments
on AAAR-1.0, across various popular LLMs, to quantify the current LLMs’ capacity to tackle high-level
research tasks. Specifically, the following sections include **EQINFER**, **EXPDESIGN**, and **WEAKNESS** .
Please refer to the Appendix for details on how to reproduce
our experiment results.


**EQUATIONINFERENCE**


**Settings.** As different LLMs have distinct context windows,
to ensure a fair comparison, we fix the maximum input length
for all models. According to the data statistics of Table 4,
we empirically use 1,000 words for both contexts before and
after equations, i.e., 2,000 surrounded words.


**Main results.** Table 1 shows the main results. Firstly, the
open-source LLMs, especially the Falcon and Gemma, perform unexpectedly disappointing (even worse than random
guesses). These screwed scores are mainly due to the poor
long-context instruction following ability, where we find
some open-source LLMs are confused with the massive
input and often copy the LaTeX code from the input. In
contrast, closed-source LLMs generally achieve superior accuracy, probably owing to the richer scientific knowledge
from the larger model parameters. However, considering the
conventional multi-choice QA formulation of EQINFER, the
recently-released GPT-4o solely gets 43.18, implying the
unique challenge of EQINFER compared with other scientific QA benchmarks (Song et al. 2023). Notably, with the
help of internal CoT, o1 gains stronger performances than
GPT-4/GPT-4o, indicating the potential benefits of adopting
reasoning for this task.


_Q_ **: do more contexts boost performance?** Table 1 unifies
the input context lengths to 1,000 words for various LLMs. In
this paragraph, we experiment with long-context LLMs to investigate the impact of the input context lengths. Particularly,
we scale the input length (per side) from 100 to 1,500 words.
As shown in Figure 4, for the open-source LLMs (Llama and
Qwen), after 300 words length, increasing the input context
doesn’t help the performance and even significantly drops
Qwen’s scores. While for the closed-source GPT-4-Turbo
and GPT-4o, scaling up input length gradually boosts the performances at the first 1,000 words, but stabilizes afterwards.
This is in line with human intuition, i.e., surrounding context
is required for the equation inference, as the adjacent context
usually provides important information, such as the target algorithm description or the notation definition. However, after
exceeding a specific threshold, more context information is
not beneficial anymore and even confuses those LLMs with
poor long-context handling capacity (Wang et al. 2024; Liu
et al. 2024).


**EXPERIMENTDESIGN**

**Settings.** Similarly, we unify the input context length of
different LLMs to ensure a fair comparison. According to
Table 5, we set 2,000 and 3,000 input words for open- and
closed-source LLMs, respectively. Meanwhile, as motivation explanation is the subsequent task of experiment design,
using model-generated experiments can propagate errors in
explanation, leading to inferior results for most LLMs. To
this end, we provide LLMs with the oracle experiments when
generating explanations.


**Main results.** Table 2 shows the main results. For the experiment design, the closed-source LLMs generally outperform
open-source LLMs, and both closed-/open-source LLMs are
superior to the “Copy Input” baseline (except the Falcon).
Despite the higher S-Precision, the open-source LLMs are
seriously deficient in S-Recall compared with closed-source
LLMs ( _∼_ 10% _↓_ ). We find that closed-source LLMs are more
creative in experiment design and tend to generate more experiment ideas than open-source LLMs (though most of the
experiment ideas are trivial), leading to excellent S-Recall.
As for the motivation explanation, the S-Match scores of



**Methods** **Accuracy (%)**


Random Guess 25.00


Table 1: Various LLMs’ performances on the 1,049 instances
of EQINFER task.


closed-source LLMs still surpass the open-source LLMs,
while the score difference is not significant. Furthermore,
we find the negative correlation between S-Match and the
ROUGE, where the ROUGE scores of closed-source LLMs
are broadly inferior. We find that the open-source LLMs often
try to copy the terms or phrases from the given experiment, or
even simply paraphrase the experiment instead of explaining,
which results in a high superficial overlap with the groundtruth explanation. This observation highlights the importance
of adopting the proposed S-Match to avoid evaluation bias of
traditional generation metrics.


_Q_ 1 **: can self-contained experiments enhance the explana-**
**tion of motivation?** When generating the explanation in
Table 2, we provide LLMs with each individual experiment
and let them explain one by one, because we find that, when
providing the whole experiment list, those open-source models only explain partial experiments because of their poor
instruction-following capacity. However, there are intuitively
some semantic or logical relations between different experiments, e.g., some experiments are prerequisites to others.
Therefore, this one-by-one prompting might break the selfcontainment of an experiment plan. Consequently, we test
with the “whole-list” prompting, where the LLMs are given
the complete experiment list and are asked to explain all
experiment steps together.
As shown in Table 8, unlike the open-source LLMs, the
explanation performances of those closed-source LLMs are
generally improved after adopting whole-list prompting. According to further manual checking, after maintaining the
self-containment of the experiments, the LLMs can refer to
other experiments and better grasp the underlying motivation
of the current experiment.


_Q_ 2 **:** **do** **human** **evaluation** **results** **align** **with** **automatic**
**metrics for explanation?** As the explanation can be openended, in this paragraph, we provide the human evaluation


**Experiment Design** **Motivation Explanation**

**Methods**
**S-F** 1 **S-Precision** **S-Recall** **S-Match** **ROUGE-L** **ROUGE-1**


Copy Input 21.13 17.94 26.76 40.32 22.06 25.28


Table 2: Various LLMs’ performances on the 100 instances of EXPDESIGN . The motivation explanation is based on the oracle
experiments to prevent error propagation. “Copy Input” is a random baseline: for experiment design, randomly select 5 sentences
from the input paper; for motivation explanation, directly copy each experiment idea.



results on different LLMs’ motivation explanation outputs.
In detail, we randomly select 20 out of 100 papers and ask 5
annotators to read the experiments along with each model’s
explanations; we then let the annotator decide whether each
model’s explanation is acceptable (see Appendix for more details). Table 9 illustrates the results, where the score variance
is higher than Table 2. However, the performance ranking
of both tables is perfectly correlated with each other (Spearman’s rank correlation coefficient = 1), demonstrating the
effectiveness of S-Match.


_Q_ 3 **: do more contexts boost performance?** We also investigate the impact of input context length for EXPDESIGN.
As shown in Figure 5, we scale up the input pre-experiment
context length from 0.1k to 10k words (10k words is the maximum paper context length in the dataset). For the experiment
planning, more input context does improve the performance
of different LLMs, while this benefit stops after exceeding 5k
words, which is similar to EQINFER’s scaling results — after
the necessary information has been covered, scaling more
up doesn’t boost the performance. Meanwhile, the results
of the motivation explanation demonstrate that explaining
motivations almost doesn’t require any paper context, i.e.,
the LLMs solely rely on the given experiments. However,
we do not expect this because we hope LLMs can explain
the motivation based on a thorough understanding of the paper, just like how human experts do. Hence, there is still a
considerable gap between the LLMs and humans in terms of
grasping research motivations.


_Q_ 4 **: does multi-modal input boost performance?** Intuitively, besides the text, when designing experiments for a
given research topic, the figures can provide rich supplementary information, such as an algorithm illustration that can
help better understand this research topic and underlying mo


tivations. Hence, we test different MLLMs’ performances,
including GPT4-o, GPT-4, and InternVL2 (Chen et al. 2024b).
Table 10 shows the ablation results on the figure data. To our
surprise, the figure data doesn’t improve the MLLMs’ results
in this task, even harming the performances. This might be
due to the low informativeness of the figures, as figures usually consume more input tokens but act only as supplementary
information to the text, indicating future work on developing
MLLMs that can effectively leverage the scientific figures.


**PAPERWEAKNESS**
**Settings.** Intuitively, the full paper context is necessary for
conducting a review. Therefore, instead of setting a maximum
input length, in WEAKNESS, we try to feed all the paper context into the LLMs. As the input length of WEAKNESS is
extremely long (see Table 6), we adopt a “split-combine”
method — we first split the whole paper into several smaller
pieces and let LLMs predict the weaknesses of each piece
separately; after that, we combine all pieces’ weaknesses as
a final complete prediction. In practice, for the length of each
small piece, we set 2,000 and 3,000 words for open- and
closed-source LLMs, respectively. Additionally, in this task,
we also examine the performance of a recent agent framework, namely AI-SCI (Lu et al. 2024), which enhances GPT4o’s paper review ability by leveraging advanced prompting
techniques, e.g., self-reflection (Shinn et al. 2024) and response ensembling (Wang et al. 2023). [4]


**Main results.** Table 3 shows the main results, where the
closed-source LLMs’ overall performances are generally superior to the results of open-source LLMs. Similarly, closed

4We don’t run AI-SCI on EXPDESIGN, because AI-SCI takes
model-generated ideas as the inputs, which are incompatible with
our task setting.


**Review Diversity**
**Methods** **SN-F** 1 **(%)** **SN-Precision (%)** **SN-Recall (%)** **ITF-IDF (** _↑_ **)**


Peer Review - - - 7.69


Table 3: Various LLMs’ performances on the 993 instances of WEAKNESS .



source LLMs are particularly excellent in SN-Recall because
of more generated weaknesses. However, there is still a considerable gap in the weakness diversity between the LLMs
and human experts. [5] Compared with human review, most
LLM-generated weaknesses are vague and lack the necessary
knowledge about some frontier research works. Surprisingly,
AI-SCI performs worse than backbone GPT-4o, especially
on ITF-IDF, which suggests the challenge of WEAKNESS,
i.e., simply adopting popular prompting techniques cannot
well address this task.


_Q_ 1 **: is the split-combine effective?** Ideally, if the LLM has
a sufficient context window size, it is not that necessary to
split the input papers for separate processing. Consequently,
in this paragraph, we utilize the LLMs accepting long context input to compare “split-combine” with “no-split”, i.e.,
letting LLMs write weaknesses by giving the full paper. In
practice, we set the maximum number of input words to 20k,
which ensures _≥_ 95% papers in the WEAKNESS can be fully
processed. As shown in Table 7, compared with giving the
full paper contexts, split-combine generally brings about superior performances. During manual checking, we find that,
when full paper is available, LLMs frequently neglect some
important sections and omit weaknesses accordingly, while
split-combine ensures that the LLMs can carefully brainstorm weaknesses within each smaller piece. Surprisingly,
the LLMs’ performances with full paper context can be even
worse than just remaining the first 3,000 words. This implies
that even the current powerful long-context LLMs still fall


5Note that the human’s ITF-IDF score in Table 3 can be slightly
underestimated. This is because we keep the repeated weaknesses in
the human review, which affects the human review’s informativeness
(lower ITF) but is useful when calculating the SN-Recall for LLMs.



short when processing long scientific documents (Liu et al.
2024).


_Q_ 2 **:** **does** **multi-modal** **input** **boost** **performance?** Our
dataset covers both tables and figure illustrations extracted
from the paper PDF as inputs. Intuitively, when reviewing
a paper, both figures and tables are critical, not only for a
better understanding, but also because some weaknesses are
related to tables/figures. [6] Therefore, in Table 11, we adopt
two MLLMs to investigate the effectiveness of image inputs.
Overall, image information, including both figures and tables, doesn’t bring significant performance improvement, i.e.,
only InternVL2 gains a performance boost after incorporating figures; while tables slightly drop both models’ results.
This is probably because the MLLMs cannot reason well
over the information-intensive images, especially the table
images (Deng et al. 2024).


**Conclusion**


In this work, we propose AAAR-1.0, a novel benchmark targeting a comprehensive evaluation of the current LLMs’ AI
research capacity. We devise three distinct expertise-intensive
tasks along with the curated evaluation metrics, and collect
high-quality data by employing senior AI researchers. Multiround strict data examination and filtering are conducted to
try our best to avoid any significant noise in the data. Extensive experiments across various mainstream LLMs highlight
the challenges and values of AAAR-1.0, where there is still
a considerable gap between LLMs and human experts.


6We find that there is approximately one human-written weakness related to figures or tables in each paper.


**References**

Almazrouei, E.; Alobeidli, H.; Alshamsi, A.; Cappelli, A.;
Cojocaru, R.; Debbah, M.; Goffinet, E.; Heslow, D.; Launay,
J.; Malartic, Q.; Noune, B.; Pannier, B.; and Penedo, G. 2023.
Falcon-40B: an open large language model with state-of-theart performance.

Anil, R.; Borgeaud, S.; Wu, Y.; Alayrac, J.-B.; Yu, J.; Soricut,
R.; Schalkwyk, J.; Dai, A. M.; Hauth, A.; Team, G.; et al.
2023. Gemini: a family of highly capable multimodal models.
_arXiv preprint arXiv:2312.11805_ .

Anthropic. 2024. Introducing Claude 3.5 Sonnet. https:
//www.anthropic.com/news/claude-3-5-sonnet.

Chamoun, E.; Schlichktrull, M.; and Vlachos, A. 2024. Automated Focused Feedback Generation for Scientific Writing
Assistance. _arXiv preprint arXiv:2405.20477_ .

Chen, Z.; Chen, S.; Ning, Y.; Zhang, Q.; Wang, B.; Yu,
B.; Li, Y.; Liao, Z.; Wei, C.; Lu, Z.; et al. 2024a. ScienceAgentBench: Toward Rigorous Assessment of Language
Agents for Data-Driven Scientific Discovery. _arXiv preprint_
_arXiv:2410.05080_ .

Chen, Z.; Wang, W.; Tian, H.; Ye, S.; Gao, Z.; Cui, E.;
Tong, W.; Hu, K.; Luo, J.; Ma, Z.; et al. 2024b. How Far
Are We to GPT-4V? Closing the Gap to Commercial Multimodal Models with Open-Source Suites. _arXiv_ _preprint_
_arXiv:2404.16821_ .

Clark, C.; and Divvala, S. 2016. Pdffigures 2.0: Mining
figures from research papers. In _Proceedings_ _of_ _the_ _16th_
_ACM/IEEE-CS_ _on_ _Joint_ _Conference_ _on_ _Digital_ _Libraries_,
143–152.


Deng, N.; Sun, Z.; He, R.; Sikka, A.; Chen, Y.; Ma, L.; Zhang,
Y.; and Mihalcea, R. 2024. Tables as Texts or Images: Evaluating the Table Reasoning Ability of LLMs and MLLMs.
In Ku, L.-W.; Martins, A.; and Srikumar, V., eds., _Findings_
_of the Association for Computational Linguistics ACL 2024_,
407–426. Bangkok, Thailand and virtual meeting: Association for Computational Linguistics.

Du, J.; Wang, Y.; Zhao, W.; Deng, Z.; Liu, S.; Lou, R.; Zou,
H. P.; Venkit, P. N.; Zhang, N.; Srinath, M.; Zhang, H. R.;
Gupta, V.; Li, Y.; Li, T.; Wang, F.; Liu, Q.; Liu, T.; Gao,
P.; Xia, C.; Xing, C.; Cheng, J.; Wang, Z.; Su, Y.; Shah,
R. S.; Guo, R.; Gu, J.; Li, H.; Wei, K.; Wang, Z.; Cheng, L.;
Ranathunga, S.; Fang, M.; Fu, J.; Liu, F.; Huang, R.; Blanco,
E.; Cao, Y.; Zhang, R.; Yu, P. S.; and Yin, W. 2024. LLMs
Assist NLP Researchers: Critique Paper (Meta-)Reviewing.
In _The 2024 Conference on Empirical Methods in Natural_
_Language Processing_ .

Gao, Z.; Brantley, K.; and Joachims, T. 2024. Reviewer2:
Optimizing Review Generation Through Prompt Generation.
_arXiv preprint arXiv:2402.10886_ .

Geirhos, R.; Jacobsen, J.-H.; Michaelis, C.; Zemel, R.; Brendel, W.; Bethge, M.; and Wichmann, F. A. 2020. Shortcut
learning in deep neural networks. _Nature_ _Machine_ _Intelli-_
_gence_, 2(11): 665–673.

Groeneveld, D.; Beltagy, I.; Walsh, P.; Bhagia, A.; Kinney,
R.; Tafjord, O.; Jha, A. H.; Ivison, H.; Magnusson, I.; Wang,
Y.; Arora, S.; Atkinson, D.; Authur, R.; Chandu, K.; Cohan,



A.; Dumas, J.; Elazar, Y.; Gu, Y.; Hessel, J.; Khot, T.; Merrill,
W.; Morrison, J.; Muennighoff, N.; Naik, A.; Nam, C.; Peters,
M. E.; Pyatkin, V.; Ravichander, A.; Schwenk, D.; Shah,
S.; Smith, W.; Subramani, N.; Wortsman, M.; Dasigi, P.;
Lambert, N.; Richardson, K.; Dodge, J.; Lo, K.; Soldaini, L.;
Smith, N. A.; and Hajishirzi, H. 2024. OLMo: Accelerating
the Science of Language Models. _Preprint_ .

Huang, Q.; Vora, J.; Liang, P.; and Leskovec, J. 2024. MLAgentBench: Evaluating Language Agents on Machine Learning Experimentation. In _Forty-first International Conference_
_on Machine Learning_ .

Jiang, A. Q.; Sablayrolles, A.; Mensch, A.; Bamford, C.;
Chaplot, D. S.; Casas, D. d. l.; Bressand, F.; Lengyel, G.;
Lample, G.; Saulnier, L.; et al. 2023. Mistral 7B. _arXiv_
_preprint arXiv:2310.06825_ .

Jiang, A. Q.; Sablayrolles, A.; Roux, A.; Mensch, A.; Savary,
B.; Bamford, C.; Chaplot, D. S.; Casas, D. d. l.; Hanna, E. B.;
Bressand, F.; et al. 2024. Mixtral of experts. _arXiv preprint_
_arXiv:2401.04088_ .

Jin, Y.; Zhao, Q.; Wang, Y.; Chen, H.; Zhu, K.; Xiao, Y.; and
Wang, J. 2024. AgentReview: Exploring Peer Review Dynamics with LLM Agents. _arXiv preprint arXiv:2406.12708_ .

Kumar, S.; Ghosal, T.; Goyal, V.; and Ekbal, A. 2024. Can
Large Language Models Unlock Novel Scientific Research
Ideas? _arXiv preprint arXiv:2409.06185_ .

Labrak, Y.; Bazoge, A.; Morin, E.; Gourraud, P.-A.; Rouvier,
M.; and Dufour, R. 2024. Biomistral: A collection of opensource pretrained large language models for medical domains.
_arXiv preprint arXiv:2402.10373_ .

Li, H.; Jiang, H.; Zhang, T.; Yu, Z.; Yin, A.; Cheng, H.; Fu, S.;
Zhang, Y.; and He, W. 2023a. TrainerAgent: Customizable
and Efficient Model Training through LLM-Powered MultiAgent System. _arXiv preprint arXiv:2311.06622_ .

Li, R.; Patel, T.; Wang, Q.; and Du, X. 2024. MLR-Copilot:
Autonomous Machine Learning Research based on Large
Language Models Agents. _arXiv preprint arXiv:2408.14033_ .

Li, Z.; Zhou, W.; Chiang, Y.-Y.; and Chen, M. 2023b. Geolm:
Empowering language models for geospatially grounded language understanding. _arXiv preprint arXiv:2310.14478_ .

Liang, W.; Zhang, Y.; Cao, H.; Wang, B.; Ding, D. Y.; Yang,
X.; Vodrahalli, K.; He, S.; Smith, D. S.; Yin, Y.; et al. 2024.
Can large language models provide useful feedback on research papers? A large-scale empirical analysis. _NEJM AI_,
1(8): AIoa2400196.


Lin, C.-Y. 2004. Rouge: A Package for Automatic Evaluation
of Summaries. In _Text summarization branches out_, 74–81.

Lin, J.; Yin, H.; Ping, W.; Lu, Y.; Molchanov, P.; Tao, A.;
Mao, H.; Kautz, J.; Shoeybi, M.; and Han, S. 2023. VILA: On
Pre-training for Visual Language Models. arXiv:2312.07533.

Liu, N. F.; Lin, K.; Hewitt, J.; Paranjape, A.; Bevilacqua,
M.; Petroni, F.; and Liang, P. 2024. Lost in the middle:
How language models use long contexts. _Transactions of the_
_Association for Computational Linguistics_, 12: 157–173.


Lo, K.; Shen, Z.; Newman, B.; Chang, J. Z.; Authur, R.;
Bransom, E.; Candra, S.; Chandrasekhar, Y.; Huff, R.; Kuehl,


B.; et al. 2023. PaperMage: A Unified Toolkit for Processing, Representing, and Manipulating Visually-Rich Scientific
Documents. In _Proceedings of the 2023 Conference on Em-_
_pirical_ _Methods_ _in_ _Natural_ _Language_ _Processing:_ _System_
_Demonstrations_, 495–507.

Lu, C.; Lu, C.; Lange, R. T.; Foerster, J.; Clune, J.; and
Ha, D. 2024. The AI Scientist: Towards Fully Automated Open-Ended Scientific Discovery. _arXiv_ _preprint_
_arXiv:2408.06292_ .

MetaAI. 2024. Introducing Llama 3.1: Our most capable
models to date. https://ai.meta.com/blog/meta-llama-3-1/.

OpenAI. 2024a. Hello GPT-4o. https://openai.com/index/
hello-gpt-4o/.

OpenAI. 2024b. Introducing OpenAI o1. https://openai.com/
index/introducing-openai-o1-preview/.

OpenAI; Achiam, J.; Adler, S.; Agarwal, S.; Ahmad, L.;
Akkaya, I.; Aleman, F. L.; Almeida, D.; Altenschmidt, J.;
Altman, S.; Anadkat, S.; et al. 2023. Gpt-4 technical report.
_arXiv preprint arXiv:2303.08774_ .

Reimers, N. 2019. Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks. _arXiv_ _preprint_
_arXiv:1908.10084_ .

Shinn, N.; Cassano, F.; Gopinath, A.; Narasimhan, K.; and
Yao, S. 2024. Reflexion: Language agents with verbal reinforcement learning. _Advances_ _in_ _Neural_ _Information_ _Pro-_
_cessing Systems_, 36.

Si, C.; Yang, D.; and Hashimoto, T. 2024. Can llms generate
novel research ideas? a large-scale human study with 100+
nlp researchers. _arXiv preprint arXiv:2409.04109_ .

Song, L.; Zhang, J.; Cheng, L.; Zhou, P.; Zhou, T.; and Li,
I. 2023. Nlpbench: Evaluating large language models on
solving nlp problems. _arXiv preprint arXiv:2309.15630_ .

Tang, X.; Liu, Y.; Cai, Z.; Shao, Y.; Lu, J.; Zhang, Y.; Deng,
Z.; Hu, H.; An, K.; Huang, R.; et al. 2023. ML-Bench:
Evaluating Large Language Models and Agents for Machine
Learning Tasks on Repository-Level Code. _arXiv e-prints_,
arXiv–2311.

Team, G. 2024a. Google launches Gemma 2, its next generation of open models. https://blog.google/technology/
developers/google-gemma-2/.

Team, Q. 2024b. Qwen2.5: A Party of Foundation Models.

Wang, M.; Chen, L.; Fu, C.; Liao, S.; Zhang, X.; Wu, B.; Yu,
H.; Xu, N.; Zhang, L.; Luo, R.; et al. 2024. Leave no document behind: Benchmarking long-context llms with extended
multi-doc qa. _arXiv preprint arXiv:2406.17419_ .

Wang, X.; Wei, J.; Schuurmans, D.; Le, Q. V.; Chi, E. H.;
Narang, S.; Chowdhery, A.; and Zhou, D. 2023. SelfConsistency Improves Chain of Thought Reasoning in Language Models. In _The Eleventh International Conference on_
_Learning Representations, ICLR 2023, Kigali, Rwanda, May_
_1-5, 2023_ . OpenReview.net.


Yu, B.; Baker, F. N.; Chen, Z.; Ning, X.; and Sun, H. 2024.
Llasmol: Advancing large language models for chemistry
with a large-scale, comprehensive, high-quality instruction
tuning dataset. _arXiv preprint arXiv:2402.09391_ .



Yue, X.; Ni, Y.; Zhang, K.; Zheng, T.; Liu, R.; Zhang,
G.; Stevens, S.; Jiang, D.; Ren, W.; Sun, Y.; et al. 2023.
Mmmu: A massive multi-discipline multimodal understanding and reasoning benchmark for expert agi. _arXiv preprint_
_arXiv:2311.16502_ .


**Appendices**

Within this supplementary material, we elaborate on the following aspects:


 - Related Work

 - Formal Definition of Evaluation Criteria

 - Data Statistics and Diversity

 - Implementation Details

 - More Experiment Results

 - Prompt Templates


**Related Work**

**LLMs for AI Research.** With the rapid evolution of pertaining techniques, LLMs are found to be useful in assisting various research disciplines (Yu et al. 2024; Labrak
et al. 2024), particularly in AI research, such as generating
novel research ideas (Kumar et al. 2024), reviewing research
draft (Gao, Brantley, and Joachims 2024; Du et al. 2024;
Liang et al. 2024), and writing scientific papers (Chamoun,
Schlichktrull, and Vlachos 2024; Lu et al. 2024). For example, Si, Yang, and Hashimoto (2024) conducted a large-scale
human investigation on LLM-generated research ideas and
found that LLMs can generate novel ideas compared with
humans while lacking feasibility. Du et al. (2024) found that
while LLMs are effective at summarizing papers, they tend
to overly trust the authors’ claimed strengths and struggle
to identify weaknesses specific to the paper. Furthermore,
some works try to employ LLMs to solve more complicated
research tasks that are composed of multiple steps (Li et al.
2024, 2023a; Tang et al. 2023). Notably, Lu et al. (2024)
proposed AI-SCIENTIST, an autonomous agent framework
that can handle a series of challenging research tasks consecutively, including generating research ideas, coming up with
the corresponding experiments along with the implementations, and then writing the final research paper — exactly how
human conduct a whole research pipeline. However, there is
still a lack of systematic evaluations and quantitative analyses on the LLMs’ (intermediate) output of each single-step
research task. Our work focuses on building a benchmark
that has individual research steps with clear input-output expectations, thus making it suitable for comprehensive LLMs
evaluation.


**Benchmarks for AI Research Tasks.** Existing “LLM assists research” benchmarks mainly focus on the implementation and execution part of the research pipeline (Lu et al.
2024; Chen et al. 2024a; Li et al. 2024). For instance, Huang
et al. (2024) proposed MLAgentBench to test the LLMs’ capacity for writing project code and training the ML models,
where the evaluation metric is the test performance of the
models trained by LLMs. However, real-world AI research
activities are diverse and some of them are hard to assess


for quality, such as generating research ideas, which requires
intensive manual assessment (Si, Yang, and Hashimoto 2024;
Liang et al. 2024), or LLM-based estimation (Lu et al. 2024).
Our work mainly focuses on high-level experience-based research tasks, and we try to build curated task-specific metrics
for every single task for a more efficient and accurate LLMs
appraisal.


**Formal Definition of Evaluation Criteria**


For the experiment plan list of EXPDESIGN, we hope the
LLMs can mention as many similar experiment steps as the
expert’s plan. Nevertheless, we also don’t expect LLMs to
generate too many irrelevant or redundant steps in the plan.
This intuition covers both the “recall” and “precision” aspects. Therefore, we develop semantic similarity-based F1
score, denoted as S-F1, which is the harmonic mean of SPrecision and S-Recall:




- _mj_ log - _mj_

_i_ =1 _Oi_ _[j]_




- - _w_
_×_ log
_Ri_ _[j]_




1
_mj_




- [�]



ITF-IDF = [1]

_w_



_w_



_j_ =1



(6)



_Oi_ _[j]_ [=]


_Ri_ _[j]_ [=]



_mj_

- sim( _p_ _[j]_ _i_ _[, p][j]_ _k_ [)] (7)


_k_ =1



_w_

- max sim( _p_ _[j]_ _i_ _[, p]_ _s_ _[l]_ [)] (8)

_s_
_l_ =1



S-Precision = [1]

_m_



S-Precision = [1]



S-Recall = [1]

_n_



_m_

- max sim( _pi, gj_ ) (1)

_j_
_i_ =1


_n_

- max sim( _gj, pi_ ) (2)

_i_
_j_ =1



where the _p_ and _g_ represent the LLM’s prediction plan
and the ground-truth plan, respectively. The _m_ and _n_ are the
list length of _p_ and _g_ (e.g., _m_ experiment steps in _p_ ). We
use SentenceBERT (Reimers 2019) to measure the semantic
similarity between the _pi_ step and the _gj_ step.
Meanwhile, S-F1 omits the item order difference of two
lists, but when giving same-length lists (items have one-one
correspondence), we can utilize the following similaritybased matching score:



S-Match = [1]

_m_



_m_

- sim( _pi, gi_ ) (3)


_i_ =1



Unlike EXPDESIGN, the output of WEAKNESS is multiple
reviewers’ weakness lists, which means we have to measure
LLM’s single prediction list with a “nested” list. Hence, we
rewrite S-Precision, S-Recall to SN-Precision, SN-Recall:




- max sim( _pi, gj_ _[k]_ [)]

_j_
_k_ =1




1
_r_



_r_








SN-Precision = [1]

_m_



_m_



_i_ =1



where the _w_ is the total number of papers in the dataset,
_p_ _[j]_ is _j_ -th paper’s prediction weakness list, _p_ _[j]_ _i_ [is] [the] _[i]_ [-th]
weakness in _p_ _[j]_ . Moreover, _Oi_ _[j]_ [calculates the intra-paper oc-]
currence frequency of _p_ _[j]_ _i_ [;] _[ R]_ _i_ _[j]_ [is the “soft” number of papers]
that also contain the _p_ _[j]_ _i_ [,] [which] [is] [computed] [by] [summing]
the maximum similarity scores between _p_ _[j]_ _i_ [and other paper’s]
weaknesses. In a word, _Oi_ _[j]_ [measures informativeness, and] _[ R]_ _i_ _[j]_
measures specificity. The complete ITF-IDF consider both
aspects and reflects the overall weakness diversity.


**Data Statistics and Diversity**


We provide the detailed data statistics of three datasets in our
benchmark, as shown in Table 4, 5, and 6. We use the NLTK
package [7] to tokenize words and count the length. When calculating the length of equations, we use the pylatexenc tool [8]
to simplify the equations first.
Meanwhile, for the WEAKNESS, we also plot the review
scores distribution of the papers used in the dataset, as well as
the track distribution. As can be found in Figure 3, our dataset
has a decent distribution, where the papers are uniformly
distributed across 13 tracks, and most papers’ scores ranged
from 5 to 8 (i.e., most papers are weakly rejected or accepted).


**Implementation Details**

**Metric Details**


When calculating the metrics, specifically for the similaritybased scores, we utilize SentenceBERT (Reimers 2019) to
encode each segment (e.g., each experiment idea in the list)
into a dense vector, and then calculate the cosine similarity, [9]
which takes about 1GB of memory when running on a single
A100 GPU.


**LLMs Running Details**


In our experiments, we utilize various LLMs, including both
closed and open-sourced. We list the model weight sources
for the open-source LLMs:


 - OLMo-7B: https://huggingface.co/allenai/OLMo-7B

 - Falcon-40B: https://huggingface.co/tiiuae/falcon-40b

 - Gemma 2-27B: https://huggingface.co/google/gemma-227b


7https://www.nltk.org/
8https://github.com/phfaist/pylatexenc
9https://huggingface.co/sentence-transformers/all-mpnet-basev2



_nk_





- max sim( _gj_ _[k][, p]_ _i_ [)]

_i_
_j_ =1







(4)


(5)




1
_nk_



SN-Recall = [1]

_r_



_r_



_k_ =1



where _r_ is the number of reviewers of the given paper, _nk_
means the length of _k_ -th reviewer’s weakness list, and _gj_ _[k]_
indicates the _j_ -th item in _k_ -th reviewer’s weakness list.
Additionally, in the real world, we would think a review
weakness is reliable if it is specific to a paper. Meanwhile, we
also hope the review is informative, i.e., no excessive similar
weaknesses in one review. Inspired by the classic TF-IDF, we
propose a novel review diversity metric:


# of classification instances 1,049
# of source papers 869


ave. “left” input context length (in words) 4,377
ave. “right” input context length (in words) 6,362
max “left” input context length (in words) 24,849
max “right” input context length (in words) 32,948
min “left” input context length (in words) 711
min “right” input context length (in words) 8


ave. “pos.” output equation length (in character) 55
ave. “neg.” output equation length (in character) 48
max “pos.” output equation length (in character) 1,039
max “neg.” output equation length (in character) 306
min “pos.” output equation length (in character) 6
min “neg.” output equation length (in character) 4


Table 4: The statistics of EQINFER . Here, the “left” and
“right” input context indicates the paper contexts before and
after the missed equation; “pos.” means the ground-truth
equations (written by the source paper authors), while “neg.”
is the GPT4-synthetic wrong equations.


# of instances 100
# of source papers 100


ave. input context length (in words) 4,288
max input context length (in words) 9,799
min input context length (in words) 698
ave. # of input figures 2.6
max # of input figures 16.0
min # of input figures 0.0


ave. length of Experiment&Explanation list 5.7
ave. length per experiment (in words) 34.3
ave. length per explanation (in words) 27.1
max length of Experiment&Explanation list 13
max length per experiment (in words) 135
max length per explanation (in words) 89
min length of Experiment&Explanation list 2
min length per experiment (in words) 9
min length per explanation (in words) 9


Table 5: The statistics of EXPDESIGN .


 - Mistral-7B: https://huggingface.co/mistralai/Mistral-7BInstruct-v0.3

 - Mixtral-8x22B-MoE: https://huggingface.co/mistralai/
Mixtral-8x22B-Instruct-v0.1

 - Llama 3.1-70B: https://huggingface.co/metallama/Llama-3.1-70B

 - Qwen 2.5-72B: https://huggingface.co/Qwen/Qwen2.572B

We use VLLM to unify the inference endpoints of all the
above models. [10] We use Pytorch 2.4.0 with CUDA 12.1, and


10https://github.com/vllm-project/vllm



# of instances 993
# of source papers 993


ave. input context length (in words) 9,811
max input context length (in words) 49,195
min input context length (in words) 24
ave. # of input figures 7.0
max # of input figures 37.0
min # of input figures 0.0
ave. # of input tables 4.3
max # of input tables 53.0
min # of input tables 0.0


ave. # of reviewers per paper 3.8
max # of reviewers per paper 9.0
min # of reviewers per paper 3.0
ave. # of weaknesses per reviewer 4.8
max # of weaknesses per reviewer 39.0
min # of weaknesses per reviewer 1.0
ave. length of weakness (in words) 39.1
max length of weakness (in words) 371.0
min length of weakness (in words) 2.0


Table 6: The statistics of WEAKNESS .


use 8 NVIDIA A100 GPUs for the LLMs inference.
Meanwhile, we use the gpt-4o-2024-08-06, gpt-4-1106preview, o1-preview-2024-09-12, gemini-1.5-pro-002, and
claude-3-5-sonnet-20240620 for the closed-source LLMs.
We use LiteLLM to unify the API calling for all these
LLMs. [11]
Given the unstable performance of LLMs, particularly
closed-source ones, we run each model thrice during our
experiments, selecting the median result from these repeated
runs.


**More Experiment Results**

**Input Context Scaling Investigation**

Figure 4, Figure 5, and Table 7 show the context scaling
results of EQINFER, EXPDESIGN, and WEAKNESS.


**Two Different Explanation Generation Methods for**
**LLMs**

We post the explanation generation performance comparison
of containing self-containment in Table 8.


**Human Evaluation on LLM-Generated Explanation**

We ask 5 annotators to evaluate the LLM-generated explanations. Specifically, each of them is assigned 4 or 5 papers,
along with the corresponding experiment lists. For each paper, the annotator is given 5 different models’ outputs (model
names are anonymized), and the annotator has to decide if


11https://github.com/BerriAI/litellm


**Input Context** **Window Size**
**Models** **SN-F1** **SN-Precision** **SN-Recall** **ITF-IDF**
**Processing** **(in words)**


split-combine 3,000 **47.66** 42.15 **55.19** 5.31
GPT-4-Turbo no-split 3,000 45.80 **43.66** 48.39 **5.58**
no-split 20,000 44.99 42.64 47.82 **5.58**


split-combine 3,000 **47.73** 42.09 **55.48** 5.95
GPT-4o no-split 3,000 45.74 **43.45** 48.54 5.92
no-split 20,000 45.47 42.97 48.51 **6.02**


split-combine 3,000 **45.05** 40.02 **51.91** 2.23
AI-SCI no-split 3,000 42.56 **40.90** 44.65 2.53
no-split 20,000 42.53 40.75 44.78 **2.58**


Table 7: The performance comparison of different input processing methods for WEAKNESS . We use GPT-4o and GPT-4-Turbo
because both accept a maximum of 128k tokens input. We also put the results of AI-SCI in the table for reference. Here,
“split-combine” splits the input paper into several pieces, where each piece’s length is denoted as “window size”; “no-split”
means the conventional input cutting, for example, if the window size is 3,000, then only the first 3,000 words in the paper are
used. According to the data statistics, 20,000 words can cover maximum lengths of more than 95% of the papers in our dataset.



**Models** **One-by-One** **Whole-List**


Llama 3.1-70B 50.05 49.36 ( _↓_ 0.7)

Qwen 2.5-72B 51.12 48.56 ( _↓_ 2.6)


Gemini 1.5 Pro 52.87 57.48 ( _↑_ 4.6)

Claude 3.5 sonnet 53.03 59.11 ( _↑_ 6.1)

GPT-4 55.03 56.95 ( _↑_ 1.9)

GPT-4o 54.79 58.54 ( _↑_ 3.8)

o1-preview 58.55 61.58 ( _↑_ 3.0)


Table 8: The impact on S-Match scores of maintaining the
experiment’s self-containment for EXPDESIGN .


**Models** **Acc. ratio**


Llama 3.1-70B 22.93
Gemini 1.5 Pro 55.07
Claude 3.5 sonnet 61.46
GPT-4o 69.72
o1-preview **76.14**


Table 9: The human evaluation results on LLMs’ output
explanations of EXPDESIGN . “Acc. ratio” means how many
model outputs are accepted by the annotator.


each LLM-generated explanation is acceptable according to
the experiment. We show the human evaluation results in
Table 9,



**Multi-Modal Input Ablation**

We post the multi-modal ablation study of EXPDESIGN and WEAKNESS in Table 10 and Table 11.


**Data cases and Annotation Platform Illustration**

As shown in Figure 7, 8, and 9, we show the sample cases of
the three tasks in AAAR-1.0. Meanwhile, we illustrate the
screenshot of our annotation platform in Figure 6.


**Prompt Templates**

In this appendix, we attach all the prompts used in this work,
including prompts in data collection and model prediction,
as shown in Figure 10, 11, and 12.


**Experiment Design** **Motivation Explanation**

**Models**
**S-F1** **S-Precision** **S-Recall** **S-Match** **ROUGE-L** **ROUGE-1**


GPT-4o **53.00** **51.24** **55.12** **58.54** **29.25** **35.50**
w/ figures 50.11 48.94 51.59 58.53 27.87 34.30


GPT-4 **43.89** 42.34 **45.82** **56.95** **25.98** **33.37**
w/ figures 43.54 **42.56** 44.85 55.03 22.82 30.01


InternVL2-26B **40.52** **48.95** **35.20** 50.03 29.13 **34.26**
w/ figures 38.83 46.91 33.70 **50.29** **29.29** 34.06


Table 10: The figure inputs ablation of EXPDESIGN . For the maximum text input length, same as the setting in Table 2, we use
2,000 and 3,000 words for open- and closed-source models, respectively. For the closed-source GPT-4o and GPT-4, as they have
long context window, we use all the figures of each paper. While for InternVL2, we randomly select two figures per input paper.


**Models** **SN-F1** **SN-Precision** **SN-Recall** **ITF-IDF**


GPT-4o **47.73** **42.09** **55.48** **5.95**
w/ tables 46.76 41.32 54.17 5.53
w/ figures 46.62 41.20 54.04 5.48
w/ tables & figures 46.58 41.17 53.98 5.36


InternVL2-26B 41.91 41.02 43.28 **1.48**
w/ tables 40.55 40.37 42.91 1.46
w/ figures **42.88** **42.10** **43.76** 1.46
w/ tables & figures 42.44 42.00 43.31 1.44


Table 11: The ablation study about the paper tables and figures of WEAKNESS . Based on the conclusion in Table 7, we use the
“split-combine” to process the text input here (2,000 and 3,000 words context window size for open- and closed-source models).
For GPT-4o, we use all the table/figure images; while for InternVL2, we randomly select two images per paper, i.e., two random
figures, two random tables, or one random figure + table.


|PT-4o<br>PT-4-Turbo<br>wen-2.5|Col2|Col3|Col4|Col5|
|---|---|---|---|---|
|ama-3.1|||||
||||||
||||||
||||||
||||||
||||||
||||||
||||||


100 300 500 700 900 1,100 1,300 1,500
Input Context Length (# of Words)





45.0


42.5


40.0


37.5


35.0


32.5


30.0


27.5


25.0











Figure 4: The input context length scaling trend on
the EQINFER task.





|-4o<br>-4-Turbo|Col2|Col3|Col4|Col5|
|---|---|---|---|---|
|n-2.5<br>a-3.1|||||
||||||
||||||
||||||
||||||
|~~0.5k~~<br>~~1k~~<br>Input Context<br>-4o<br>-4-Turbo|~~3k~~<br>~~5~~<br> Length (# of|~~k~~<br><br>   Words)|~~8k~~<br>~~1~~|~~k~~|
|n-2.5<br>a-3.1|||||
||||||
||||||
||||||
||||||
||||||


0.1k 0.5k 1k 3k 5k 8k 10k
Input Context Length (# of Words)



(a) The review score distribution of the papers used in

WEAKNESS .



80


70


60


50


40


30


20


10


0









(b) The track distribution of the papers used in WEAKNESS .


Figure 3: The data diversity illustration of WEAKNESS,
including the score distribution and track distribution of the
papers used in our dataset.



55


50


45


40


35


30


58


56


54


52


50


48



Figure 5: The input context length scaling trend of different
LLMs on the EXPDESIGN task.


Figure 6: The annotation platform for collecting the annotation of EXPDESIGN . We ask annotators to first make comments on
the Google Drive PDF, then move all the annotations to the online Google Doc (for further verification and discussion).


Figure 7: A sample case of EQINFER .


Figure 8: A sample case of EXPDESIGN .


Figure 9: A sample case of WEAKNESS .


Figure 10: The prompts used in EQINFER, including both data collection and model prediction.


Figure 11: The prompts used in EXPDESIGN, including both data collection and model prediction.


Figure 12: The prompts used in WEAKNESS .


