# **Profiling and Analyzing Climate Change Statements in IPCC Reports**

**Anonymous submission**


**Abstract**



We propose new methods to extract and profile the climate
change statements from the Sixth Assessment Reports of the
Intergovernmental Panel on Climate Change (IPCC). We represent the 10,393 statements from the latest IPCC reports
(AR6) with associated uncertainty levels and glossary terms.
We profile their distributions across different parts of the
6000+ page AR6 reports. We also present a few case studies
centered around the glossary term “wetland”, namely linking
related statements across summary sections and chapter content, finding and profiling supporting references, and comparing them with large language models for statement summarization. We believe this work marks an initial step towards
in-depth information extraction regarding climate change. It
lays the groundwork for more advanced automated analysis
of climate-related statements and broader integrative scientific assessments.


**Introduction**


A scientific statement is a factual statement which prescribes
or entails the conditions for its verification (Miller 1947).
Statements are often viewed as a basic unit of scientific
discourse. With the scientific literature growing rapidly in
volume, keeping track of a large set of related statements
is a widely-recognized challenge across diverse fields such
as biomedicine, public health and law (Achakulvisut et al.
2019; Li, Burns, and Peng 2021; Wuehrl, Grimminger, and
Klinger 2023; Surdeanu, Nallapati, and Manning 2010; Li
et al. 2022). Climate science, a multi-disciplinary field that
studies the changing dynamics of the Earth’s climate system
and its implications for mankind, is a critical research field
given the urgency of combating climate change (Klenk and
Meehan 2015). Research findings on climate change would
facilitate critical policy decisions and enhance public understanding (Kasperson and Stern 2010). However, because
climate science is complex and multifaceted, keeping track
of the scientific literature and scientific statements on climate change poses bigger challenges than traditional scientific fields.
The Intergovernmental Panel on Climate Change (IPCC)
is the United Nations (UN) body for assessing the science
related to climate change (IPCC 2024b). One important output from IPCC are the integrative assessment report series

- the latest sixth Assessment Reports (AR6) were released



Figure 1: Turning IPCC statements into structured text.
(Top) An input text segment from the IPCC AR6 WGII report. (Bottom) System output – semi-structured representation of the corresponding statement.


between 2021 and 2023 (Arias et al. 2021; Adler et al. 2022;
Shukla et al. 2022).
The IPCC has developed protocols to recruit experts (IPCC 2024a), evaluate a large body of literature (IPCC 2024b), and encode uncertainties and consensus (Mastrandrea et al. 2010), which make the scientific
statements in these reports more robust. Therefore, IPCC
reports serve as an authoritative source of scientific findings on climate change. However, the result of the largescale collaboration, AR6, is an extremely long assessment
report totalling 10,000+ pages. We posit that the volume of
information here is too large for anyone to read and comprehend. Developing computational tools to automatically
process and digest such long reports would benefit not only
scientists and policy makers but also the general public. Because existing computational tools for understanding scientific literature are mostly designed for collections of papers (Callaghan et al. 2021), which are much shorter and focused, there is a need to develop new methods and tools for
IPCC reports, which are much longer and topically diverse.
This work takes the first few steps towards extracting information from IPCC reports. First, we design and implement a tool to extract scientific statements from IPCC Working Group (WG) reports. An example of the extracted statements, containing its text, uncertainty (i.e., confidence and
likelihood) levels, source and key terms, is shown in Figure 1. Second, we present a comprehensive profile of 10,393
statements across three IPCC WG reports. Profiling these


statements with confidence and likelihood levels provides
insights into the robustness and reliability of the information, which is crucial for informed decision-making. Our
analysis shows that WGII has a higher proportion of _high_
and _very-high_ confidence statements, and 33.98% of statements appear in different summary content rather than chapter content. Additionally, profiling the distribution of key
terms in statements across the reports helps in understanding
the thematic focus and terminological consistency. Lastly,
we present three case studies that take the first steps towards
linking related statements (Case Study 1) to highlight the
connections between different parts of the reports; identifying supporting references (Case Study 2) to provide a deeper
context for the statements; and comparing statement summarization with those by large language models (Case Study 3)
to assess the effectiveness of automated tools in summarizing complex scientific information.


We hope that this constitutes a useful first step towards
analysing other integrated assessment reports (Mach and
Field 2017), which include and obviously not limited to
the Millennium Ecosystem Assessment (Assessment 2005),
the Global Energy Assessment (Global Energy Assessment 2012), the Scientific Assessments of Ozone Depletion (Meredith et al. 2014), and upcoming assessment on AI
that forms the foundation for UN AI Governance (UN Advisory Body on Artificial Intelligence 2023). We will release
the statements at https://anonymous.com.


**Related Work**


Statement or claim extraction from scientific documents is
a crucial task across various domains. Such as biomedical domain, (Achakulvisut et al. 2019; Li, Burns, and Peng
2021; Wuehrl, Grimminger, and Klinger 2023), and legal
domain (Surdeanu, Nallapati, and Manning 2010). For the
recent COVID-19 pandemic, Li et al. (2022) build a system
to extract, structure, and monitor statements from various
sources in real-time. Unlike them, our work addresses a new
problem in building NLP tools for climate change (Stede and
Patz 2021).


For the climate change domain, there is research that focuses on extracting climate-related statements as datasets
for downstream tasks such as fact-checking. The datasets
include Climate Fever (Diggelmann et al. 2020), Climate
Feedback (Walter, Görlach, and Brüggemann 2020), and
Skeptical Science (Winkler et al. 2021). However, these
sources derive statements from social media, news, websites, etc., not from IPCC reports.Specifically, Lacombe,
Wu, and Dilworth (2023) provide a dataset by extracting statements from three IPCC Sixth Assessment Reports, aligning with our goal. However, their PDF extraction method misses some statements, introduces inaccuracies, and overlooks statements with likelihood levels. Additionally, their classification based on confidence labels is
unreasonable, as experienced climate experts consider multiple factors beyond the statements themselves.



**IPCC Reports and Scientific Statements**
**Therein**


The IPCC Sixth Assessment Report comprises three Working Group (WG) reports, which are released sequentially
from 2021 to 2022. They are WGI (2021), focusing on the
Physical Science Basis; WGII (2022), on Impacts, Adaptation, and Vulnerability; and WGIII (2022), on Mitigation
of Climate Change. The structure of each WG report consists of a Summary for Policymakers, a Technical Summary,
and a set of numbered chapters. The three WG reports have
12, 18 and 17 chapters, respectively. We leave analyzing
the Special Reports and Synthesis Report of AR6 as future
work.
Statements are one of the scientific building blocks of
IPCC reports, with each statement clearly categorized by
confidence levels and likelihoods, to provide a nuanced and
comprehensive overview of climate impacts and risks. _Con-_
_fidence_ and _likelihood_ levels, which are key metrics used by
the IPCC, express scientific uncertainty. Confidence levels in
the IPCC assessment process reflect the validity of a statement based on the type, amount, and quality of evidence supporting it, while likelihood levels denote the probability of
the occurrence of an event or outcome, calculated through
statistical methods and expert judgment. The IPCC provides
a framework that details the confidence and likelihood levels
(Adler et al. 2022). The framework structure can be found in
Figure 5 in the Appendix 1. Confidence is assessed using a
5-level scale that includes the categories of _very_ _low_, _low_,
_medium_, _high_, and _very_ _high_ confidence. The likelihood is
divided into 10 scales, from _exceptionally_ _unlikely_ (0-1%)
to _virtually certain_ (99-100%).
We store the three reports by first chunking them into
paragraphs (excluding figures, plots, etc.) and injecting the
paragraphs into the Elasticsearch [1] database to facilitate
search and analysis. An overall dataset profile is in Table 1,
showing that the three WG reports collectively have more
than 6K pages, nearly 20K paragraphs, and more than 2.5M
words. Various summary content, including the Summaries
for Policymakers (SummPol), Technical Summaries (TechSumm), and Chapter Executive Summaries (ChapSumm),
constitutes approximately 10% of the entire AR6. Note that
Annexes, Atlas, and Front Matter are excluded from Table 1.


**Extracting and Profiling Statements from**
**IPCC AR6**


We propose a method to automatically extract scientific
statements from IPCC reports and represent each statement
_s_ as a faceted tuple:

s = { _t_, _c_, _l_, _o_, _w_ }.
Here _t_ represents the statement text; _c_ and _l_ represent the
confidence and likelihood level associated with statement
_s_, respectively - either of which can be absent; _o_ specifies
the source of _s_ in the IPCC reports, including the relevant
working group, chapter, and section; _w_ refers to a set of key
terms from IPCC Glossary that appear in the statement text.


1https://www.elastic.co/


|Source|WGI|Col3|Col4|WGII|Col6|Col7|WGIII|Col9|Col10|Total|Col12|Col13|
|---|---|---|---|---|---|---|---|---|---|---|---|---|
|Source|Page|Para.|Word|Page|Para.|Word|Page|Para.|Word|Page|Para.|Word|
|SummPol<br>TechSumm<br>ChapSumm<br>ChapBody|32<br>112<br>42<br>1,740|132<br>340<br>207<br>4,419|9,243<br>34,449<br>26,764<br>732,260|34<br>84<br>61<br>2,341|45<br>207<br>428<br>6,427|5,140<br>28,728<br>46,157<br>833,961|51<br>102<br>43<br>1,599|581<br>499<br>254<br>6,240|22,046<br>45,053<br>30,734<br>757,115|117<br>298<br>146<br>5,680|758<br>1,046<br>889<br>17,086|36,429<br>108,230<br>103,655<br>2,323,336|
|Total|1,926|5,098|802,716|2,520|7,107|913,986|1,795|7,574|854,948|6,241|19,779|2,571,650|


Table 1: Basic profile of IPCC AR6 Working Group (WGI, WGII, WGIII) Reports, containing the number of pages, paragraphs
and words by content type: Summary for Policymakers (SummPol), Technical Summary (TechSumm), Executive Summary of
Chapters (ChapSumm) and the remaining Chapter contents (ChapCont).


Figure 2: A treemap of statements by confidence levels ( _very_ _high,_ _high,_ _medium,_ _low,_ _very_ _low_ ) and sources (ChapBody,
ChapSumm, TechSumm, SummPol) for IPCC AR6 WGI, WGII, and WGIII reports. Block sizes correspond to the number of
statements.



The extraction method are described below, and an example
extraction result is in Figure 1.


**Extracting Statement Text**


The data source for extraction is the HTML webpages [234]
for the AR6 WGI, WGII and WGIII reports. While the
PDF versions of the reports are available, we find HTML
parsing more reliable despite recent developments in PDF
extraction tools (Bast and Korzen 2017; Meuschke et al.
2023). We assume that each statement has a confidence level
or a likelihood level tag. In the HTML file, such tags are
in italics (e.g., <span class="condensed_italic">high confidence</span>). We split the whole reports into individual
sentences and extract sentences with the italic confidence or
likelihood tags as statements.
The extracted statements are processed as follows: (1)
Footnote numbers embedded within sentences are removed;
(2) Statements shorter than 50 characters are filtered out;
(3) Due to the unavailability of Chapter 9’s executive summary HTML in the WGIII report, relevant statements are
manually extracted from the corresponding PDF. Furthermore, we encounter instances where a sentence includes
multiple statements, e.g., “There is _high_ _confidence_ that


2https://www.ipcc.ch/report/ar6/wg1/
3https://www.ipcc.ch/report/ar6/wg2/
4https://www.ipcc.ch/report/ar6/wg3/



coastal wetlands, especially mangroves, contain large carbon stocks relative to other ecosystems and _medium_ _con-_
_fidence_ that restoration will reinstate pre-disturbance carbon sequestration rates”. Complex sentences combine multiple pieces of information into a coherent, concise summary that facilitates comparison. If necessary—for example, for downstream tasks such as uncertainty level prediction—existing NLP tools, especially large language models
(LLMs), can effectively split such sentences into separate
statements. For instance, GPT-4 (OpenAI 2023) can extract
two statements from the given sentence: “There is _high con-_
_fidence_ that coastal wetlands, especially mangroves, contain large carbon stocks relative to other ecosystems” and
“There is _medium_ _confidence_ that restoration will reinstate
pre-disturbance carbon sequestration rates”. In this paper,
following (Lacombe, Wu, and Dilworth 2023), we treat such
sentences as a single comprehensive statement and assign
the confidence or likelihood level of the last-mentioned tag.


While all 5 confidence levels are found in the dataset, the
10 likelihood levels are used less consistently. In the reports,
a few new wordings are found in the tags that aren’t in the
set of pre-defined likelihood levels, such as ‘high certainty’.
We then manually merge all of the variants into the given 10
scales. Details are in Appendix 4.


**Detecting Glossary Terms in Statements**


The IPCC includes a glossary at the end of its reports, featuring a collection of key terms along with their precise definitions. These definitions clarify and standardize the concepts
and topics referenced in the statements.
We collect terms from IPCC-glossary portal [5], by storing
all the terms found in the glossaries of AR5, AR6, and the
special reports published between AR5 and AR6. Variations
of the same word, such as “aerosol” and “aerosols”, are both
present in the glossary. Given their grammatical and semantic similarities, we lemmatize and combine them into a single entity “aerosol”. In total, we have identified 1,504 terms
defined by IPCC that could potentially match the statement
text.
To identify the presence of key terms in statements, we
employ the SpaCy [6] tokenization and lemmatization tools
from on both the terms and the statement text, then do the
token-level matching. Additionally, we convert the statement text to lowercase and eliminate punctuation.


**Overall statement profile**


We obtained 10,393 statements, which is in excess of the
8,094 statements extracted by Lacombe, Wu, and Dilworth
(2023). We denote the 10,393 statements as set _S_ ; the subset of 9,252 statements with confidence levels as set _C_ =
_{s ∈_ _S,_ where _sc_ = _ϕ}_ ; the subset of 1,488 statements with
likelihood levels as set _L_ = _{s_ _∈_ _S,_ where _sl_ = _ϕ}_ . Set _C_
contains 3,444 statements from WGI, 4,656 from WGII, and
1,152 from WGIII. Set _L_ includes 1,195 statements from
WGI, 266 from WGII, and 27 from WGIII. There are 361
statements that include both confidence and likelihood levels. 91.3% of _C_ and 84.9% of _L_ contain at least one key
term. The overall distribution aligns with observations on
integrative assessment (Mach and Field 2017) – confidence
is most applicable when characterizing statements in WGII
(on impacts, adaptation, and vulnerability) because crossdisciplinary evidence is often required for such inquiry. By
contrast, likelihood is more common in WGI (on physical
science) since statements could come from single lines of
inquiry or similar inquiries whose likelihoods could be aggregated.
**How much can we trust scientific statements in IPCC**
**reports?** To answer this question, we plot the distribution of
statements based on confidence levels. Figure 2 contains a
breakdown of confidence levels across different parts of each
WG report. In general, most of the statements in _C_ are found
within the chapter bodies. Over 90% of the overall statements have confidence levels above medium (i.e., _medium_,
_high_, or _very high_ ). Specifically, _high confidence_ is the most
common confidence level for statements in most chapters,
except for those in the chapter bodies of the WGI and WGIII
reports. As for _L_, most of the statements are found in chapter bodies as well, and the majority of them have a _likely_
label. A detailed distribution is shown in Table 4, Appendix
1. Based on these statistics, we can conclude that the major

5https://apps.ipcc.ch/glossary/
6https://spacy.io/



WGII Disruption in water flows will significantly degrade
12.ES ecosystems such as high-elevation wetlands and affect
farming communities, public health and energy production ( _high confidence_ ).


WGII Disruptions in water flows will significantly degrade or
12.3 eliminate high-elevation wetlands ( _high confidence_ ) (Bury
et al., 2013; Dangles et al., 2017; Mark et al., 2017; Polk
et al., 2017; Cuesta et al., 2019).


Table 2: One statement from _D_ and one from _N_ with a similarity score higher than threshold _θ_ = 0 _._ 78. Key terms in
the statements are highlighted.


ity of the statements in IPCC reports are confident scientific
findings rather than scientific hypotheses.
**What** **are** **the** **main** **topics** **on** **climate** **change** **covered**
**by IPCC reports?** To answer this question, we plot the distribution of statements based on the key terms they contain.
Figure 3 shows the frequency of key terms occurring in _C_
and their distributions in the three WG reports. As expected,
general concepts such as “climate”, and “climate change”
are dominant terms in the statements. Other popular topics in
the climate change domain, like “emissions”, “ecosystem”,
and “global warming”, are also emphasized. More specific
terms, such as “sea ice” and “RCP8.5”, are covered by a certain number of statements as well. This reveals that our extracted statements offer comprehensive coverage in summarizing a variety of climate change-related findings. Specifically, the most frequently occurring terms in the statements
from different working groups closely align with the themes
of each WG. Statements from WGI (The Physical Science
Basis) focus on mitigation strategies and foundational terms
such as “ocean”, “trend”, and “anthropogenic”. WGII (Impacts, Adaptation, and Vulnerability) emphasizes terms like
“vulnerability”, “risk”, “impacts”, and “adaptation”. Meanwhile, WGIII (Mitigation of Climate Change) heavily utilizes terms such as “mitigation”, “emissions”, and “energy”.
These identified terms, especially when paired, can provide
deep insights into thematic overlaps and interdependencies
between different areas of climate science. For example,
the combination of “emissions” and “mitigation” can highlight the direct relationship between the volume of emissions
and the effectiveness of mitigation strategies. Furthermore,
lower-frequency terms may uncover niche topics or emerging trends in climate science that have not yet reached mainstream recognition, presenting vital directions for our future
research.
Glossary terms (and their combinations) can be further
used to identify statements related to specific topics of interest. In the rest of this paper, we present a few case studies
of linking, supporting, and comparing related scientific facts
using statements.


**Case Study 1: Linking Statements Across AR6**
We define two statements to be **linked** if they convey similar meanings or ideas, and pertain to comparable contexts or
topics. In this section, we link related statements across different parts of IPCC AR6. Since using the collection of 10k+
statements individually does not seem practical for readers


Figure 3: Frequency and breakdown of the top 57 terms (occurring 100 times or more) in set _C_ of IPCC AR6 statements.
Top:The number of times each term appears on log scale. Bottom: The proportion of each key term in each of WGI, II and III.


WGII TS.D Restoration of wetlands could support livelihoods and help sequester carbon ( _medium_ _confidence_ ), provided they are allowed accommodation space.

WGII 12.ES Inclusive water regimes that overcome social inequalities and approaches including nature-based solutions,
such as wetland restoration and water storage and infiltration infrastructure, with synergies for ecosystem
conservation and disaster risk reduction, have been found to be more successful for adaptation and sustainable development ( _high confidence_ ).

WGII 3.4 Without careful management of freshwater inputs, sediment augmentation and/or the restoration of shorelines to more natural states, transformation and loss of intertidal areas and wetland vegetation will increase
with SLR ( _high confidence_ ) (Doughty et al., 2019; Leuven et al., 2019; Yu et al., 2019; Raw et al., 2020;
Shih, 2020; Stein et al., 2020), with small, shallow microtidal estuaries being more vulnerable to impacts
than deeper estuaries with well-developed sediments ( _medium confidence_ ) (Leuven et al., 2019; Williamson
and Guinder, 2021).

WGIII 7.4 There is _medium_ _confidence_ that coastal wetland restoration has a technical potential of 0.3 (0.04–0.84)
gtco2-eq yr –1 of which 0.1 (0.05–0.2) gtco2-eq yr –1 is available up to usd100 tco2–1.

WGIII 7.4 There is _high confidence_ that coastal wetlands, especially mangroves, contain large carbon stocks relative
to other ecosystems and _medium confidence_ that restoration will reinstate pre-disturbance carbon sequestration rates.

WGIII 7.4 There is _low_ _confidence_ on the response of coastal wetlands to climate change; however, there is _high_
_confidence_ that coastal wetland restoration will provide a suite of valuable co-benefts.


Table 3: Statements that contain both “wetland” and “restoration” key terms.



of the IPCC report, we believe being able to identify topically similar and scientifically related groups is a tangible
first step.
We build this case study around the glossary term “wetland” - chosen due to both its relevance in our geographical area and the fact that the number of related statements
forms a small yet diverse set for building intuition and manual checking of validity. We focus on two sets of statements:
set _N_ contains all 6,861 statements in Chapter bodies, and
set _D_ contains 12 statements containing “wetland” in the
summary sections – SummPol, TechSumm and ChapSumm.
Both are proper subsets of set _C_ which contains all statements with confidence.
We compare these two sets of statements to highlight the
contrast between the broader discourse and specific mentions in summaries. This comparison reveals the extent and
consistency of wetland-related discussions across detailed
and summary contexts, helping us understand the promi


nence of these issues.
Throughout this section, glossary terms are highlighted
and categorized into nine distinct groups using a termclustering scheme we developed with GPT-4, described in
detail in Appendix 3.


**Measuring** **Similarity** We posit that related statements
should exhibit topical similarity. There appear to be two primary approaches: analyzing semantic similarity and filtering
and intersecting by glossary terms. In this section, we first
apply the semantic similarity analysis, followed by the glossary term method. To measure the pair-wise similarity, we
first embed statements _di_ _∈_ _D_ and _nj_ _∈_ _N_ into 1,536dimensional vectors using the text-embedding-3-small
model of OpenAI embedding API. We then calculate the cosine similarity between _di_ and _nj_ .
The average similarity score between each _di_ and all statements in _N_ approximates 0.5 (detailed distribution is in Fig

Figure 4: Word clouds generated from the abstracts of papers supporting the statements _s_ 1(left) and _s_ 2 (right).



ure 7 in Appendix 2. It suggests a moderate level of relatedness between statements in summary and base chapters.
To establish links between statements based on their similarity scores, we define a threshold _θ_ _∈_ [0 _,_ 1]. Statements
_di_ and _nj_ are considered linked if their similarity score
_Sim_ ( _di, nj_ ) _>_ _θ_ . We explored threshold values from 0.5
to 0.99 in increments of 0.01 and computed the average
difference in similarity scores between linked and unlinked
statement pairs. The trend of these differences (illustrated in
Figure 6, Appendix 2) reveals the steepest increase in the
gap as _θ_ transitions from 0.77 to 0.78. Consequently, we
set _θ_ = 0 _._ 78, linking statements only when their similarity
scores exceed this value.


**Results and Discussion** Table 2 shows the result that only
one pair of statements from set _D_ (of 12 wetland-related
statements) and _N_ are above the threshold, both originating
from the same chapter’s (WGII Chapter 12) executive summary and body. Upon reading these statements, we confirm
that they are essentially the same statement pitched at different levels of detail. This outcome demonstrates the high
precision of this semantic similarity-based method. However, the method may miss valid links as the recall is undetermined. This limitation may stem from the complexity in sentence structures and wording, and whole-sentence
embedding may not adequately capture these nuances. The
key terms in the base chapter statement (i.e., {“wetland”})
is a subset of those in the summary chapter statement (i.e.,
{“ecosystem”, “wetland”, “health”, “energy”}). This observation prompts further investigation into whether key term
overlaps could indicate potential links between statements.
To assess the potential for further matching, we (the authors of this work) examine all six statements that include
the glossary term “restoration” (in blue) as well as “wetland” (in green) - two from summary chapters and four
from chapter bodies, shown in Table 3. Despite the fact that
glossary terms among these statements intersect, we did not
identify any additional pairs that could be linked. For example, the first and fourth statements both mention that wetland restoration benefits carbon sequestration, but the first
is broader and mentions additional benefits such as supporting livelihoods, while the fourth is more detailed and quantitative. It encourages us to explore the integration of multidimensional features for linking statements in future work,
beyond mere semantic similarity or key term matching. For
completeness, all 26 statements from Chapter body text with
the glossary term “wetland”, are listed in Table 8 in Ap


pendix 3, and we denote this set _N_ _[′]_ .


**Case Study 2: Supporting References**


In this section, we attempt to identify the scientific research
supporting a statement by extracting its cited references
(named **supporting** **references** ). While this may be a trivial task for statements containing local citations, it becomes
more complex in general because many statements summarize several paragraphs or reference entire sections that include numerous irrelevant citations.


**Method** We select one statement _s_ 1 from the set _N_ _[′]_ as
a case study: “Otherwise, wetland ecosystems must migrate either inland or upstream, or face gradual submergence
in deeper, increasingly saline water (very high confidence)
(Section 3.4.2.4; Andres et al., 2019; Jones et al., 2019b;
Cohen et al., 2020; Mafi-Gholami et al., 2020; Magolan and
Halls, 2020; Sklar et al., 2021).” This statement contains
both a reference to WGII Section 3.4.2.4 and six local citations. From the section content, we aim to find the _evi-_
_dence sentences_ that contain the citation information that the
statement may refer to. In particular, we select sentences that
contain at least one local citation as candidate evidence sentences and compute similarities between the statement and
each candidate evidence sentence using the same methodology as in Case Study 1.
Section 3.4.2.4 contains 21 candidate evidence sentences
and their resulting similarity scores range from 0.39 to 0.69.
We select the three sentences with the highest scores (details
in Table 6, Appendix 5) together with the six local citations
for a total of 17 supporting references to _s_ 1.


**Results** **and** **Discussion** By obtaining and concatenating
the abstracts of the 17 supporting references via OpenAlex [7],
we built a word cloud, as shown on the left in Figure 4. Major keywords specific to _s_ 1 that are present in its word cloud,
including “mangrove” (i.e., one kind of wetland), “salinity”,
“increase” and “wetland”, also occur in _s_ 1, which indicates
the relativity between the supporting references and _s_ 1.
As a comparison, we perform the same analysis on another statement _s_ 2: “Appropriately implemented ecosystembased mitigation, such as reforestation with climate-resilient
native species (Section 13.3.1.4), peatland and wetland
restoration, and agroecology (Section 13.5.2), can enhance


7https://docs.openalex.org/


carbon sequestration or storage (medium confidence) (Seddon et al., 2020).” yielding a total of 14 supporting references, of which 13 are used to construct the word cloud on
the right in Figure 4, as one reference was not found on OpenAlex. Contrasting with _s_ 1, the major keywords for _s_ 2 are
“carbon”, “forest”, “mitigation”, “tree”, and “ecosystem”.
The difference in major keywords between the two word
clouds supports our supporting reference detection methodology: Although both _s_ 1 and _s_ 2 mention “wetland”, their
context and emphasis differ significantly, aligning with their
respective statements. This variation potentially validates
the precision of our approach in using text-based analysis to
extract and link supporting references to statements. However, the recall of this method still needs to be evaluated in
future work.


**Case Study 3: A Comparison with GPT**
**Extracted Statements**


We further conduct a case study to evaluate the quality of our
generated statements by comparing them with those generated by large language models (LLMs).


**Method** We focus on the question “What are the main scientific statements on _wetland restoration_ in IPCC reports?”
The specific LLM used was still the GPT-4 model (OpenAI
2023) and the details of the prompt given to GPT-4 are detailed in Appendix 6. Inspired by the pioneering work of
ChatClimate (Vaghefi et al. 2023), which builds a retrievalaugmented-generation (RAG)-based conversational LLM
using IPCC reports, we explored two methodologies: (1)
pure zero-shot learning with GPT, where we provided the
prompt directly to the model for statement extraction; and
(2) RAG-based GPT, which involved enhancing the GPT4 model’s performance by providing the top five retrieved
IPCC paragraphs relevant to the query. These paragraphs
were selected based on the cosine similarity between each
paragraph in our Elasticsearch database and the query. Additionally, we extracted statements from our database that contained the key terms “wetland” and “restoration” (as shown
in Table 3) for comparison.


**Results and Discussion** The full results generated by the
three methods are presented in Table 7 in Appendix 6. Unlike our method, the zero-shot GPT model often produces
statements that cite inaccurate IPCC sections. For instance,
all three generated statements that cite “WGII Section 6.5”
are incorrect - The term “wetland” does not appear in that
section. Furthermore, the RAG-based GPT model, assisted
by the top five retrieved paragraphs, exhibits improved accuracy in identifying IPCC sections related to wetland restoration (e.g., IPCC WGIII Section 7.4). However, it still tends
to excessively condense content and generate hallucinations,
similar to the zero-shot GPT model. This decreases the
quality of the generated statements. For instance, consider
the sentence “Their restoration and rewetting is crucial to
meet 1.5°C–2°C pathways by 2050” from the second statement generated by the RAG-based GPT model. In the cited
section (i.e., IPCC WGIII Section 7.4), we find sentences
such as “. . . both peatland protection and peatland restora


tion (Section 7.4.2.7) are needed to achieve a 2°C mitigation
...” and “...peatlands, coastal wetlands, and forests are particularly important as most carbon lost from these ecosystems is irrecoverable through restoration by the 2050 timeline ...”. However, there is insufficient evidence to justify
summarizing these specific details into the broader statement
provided by the model.

Thus, only our statements contain the scientific publication information that the statement refers to, e.g., “ _. . ._
Doughty et al., 2019; Leuven et al., _. . ._ ” (cf. the third statement in Table 3). As mentioned in Case Study 2, such references provide important scientific evidence supporting the
statements. Additionally, the GPT-generated statements lack
uncertainty assessment information as we do. Confidence
and likelihood levels are crucial for evaluating the validity and probability of the statements. On the other hand,
our own generated statements also face issues: they are not
comprehensive enough because we directly select sentences
from the IPCC reports. For example, it is difficult for readers
to fully understand the fourth statement “... wetland restoration has a technical potential of 0.3 (0.04–0.84) gtco2-eq yr
–1 of which 0.1 (0.05–0.2) gtco2-eq yr –1 is available up to
usd100 tco2–1.” that we generated, as it stands alone with
no in-context information. Providing background information such as explanations of terms (e.g., “gtco2-eq”) may
potentially enhance comprehension, which urges us to seek
engagement with more climate experts in the future.


**Conclusion**


Reading, comprehending, and tracking scientific statements
in large-scale literature, especially in the complex climate
change domain, is a critical but challenging task. In this paper, we take the first few steps towards profiling and analyzing statements from the IPCC assessment reports. By automating the process, we provide researchers, policymakers,
and stakeholders with a more accessible way to navigate the
extensive and complex information found in IPCC reports.
We aim to enable more informed decision-making and foster
a deeper understanding of climate change dynamics.


**Discussion** We reflect on several limitations in the current
dataset and methods, which could guide our future work.
Previous IPCC assessment reports such as AR5 and AR4 are
only available in PDF format, necessitating the exploration
of advanced PDF parsing tools. Once extracted, evolution of
statements across the different assessment reports over the
last few decades could be explored. The results of linking
statements (cf. Case Study 1) underscore the current challenges in understanding complex climate-related statements
using matching-based and data-driven methods. Additionally, as mentioned in Case Study 2, a systematic evaluation
is required to assess the coverage and validity of the supporting references. We believe that further engaging in crossdisciplinary collaborations involving climate scientists and
linguists can enhance our interpretation of statements and
help pave the way for designing tools that can ultimately
help scientists, policy-makers and other stakeholders.


**References**

Achakulvisut, T.; Bhagavatula, C.; Acuna, D.; and Kording,
K. 2019. Claim extraction in biomedical publications using
deep discourse model and transfer learning. _arXiv preprint_
_arXiv:1907.00962_ .

Adler, C.; Wester, P.; Bhatt, I.; Huggel, C.; Insarov, G.;
Morecroft, M.; Muccione, V.; and Prakash, A. 2022. _Cli-_
_mate Change 2022: Impacts, Adaptation, and Vulnerability._
_Contribution_ _of_ _Working_ _Group_ _II_ _to_ _the_ _Sixth_ _Assessment_
_Report of the Intergovernmental Panel on Climate Change_ .
Cambridge, UK and New York, NY, USA: Cambridge University Press.

Arias, P.; Bellouin, N.; Coppola, E.; Jones, C.; Krinner, G.;
Marotzke, J.; Naik, V.; Plattner, G.-K.; Rojas, M.; Sillmann,
J.; Storelvmo, T.; Thorne, P.; Trewin, B.; Achutarao, K.; Adhikary, B.; Armour, K.; Bala, G.; Barimalala, R.; Berger, S.;
and Zickfeld, K. 2021. Climate Change 2021: The Physical Science Basis. Contribution of Working Group I to the
Sixth Assessment Report of the Intergovernmental Panel on
Climate Change; Technical Summary.

Assessment, M. E. 2005. _Millennium ecosystem assessment_ .
Washington, DC: Island Press.

Bast, H.; and Korzen, C. 2017. A benchmark and evaluation for text extraction from PDF. In _2017 ACM/IEEE joint_
_conference on digital libraries (JCDL)_, 1–10. IEEE.

Callaghan, M.; Schleussner, C.-F.; Nath, S.; Lejeune, Q.;
Knutson, T. R.; Reichstein, M.; Hansen, G.; Theokritoff, E.;
Andrijevic, M.; Brecha, R. J.; et al. 2021. Machine-learningbased evidence and attribution mapping of 100,000 climate
impact studies. _Nature climate change_, 11(11): 966–972.

Diggelmann, T.; Boyd-Graber, J.; Bulian, J.; Ciaramita,
M.; and Leippold, M. 2020. Climate-fever: A dataset for
verification of real-world climate claims. _arXiv_ _preprint_
_arXiv:2012.00614_ .

Global Energy Assessment. 2012. _Global_ _Energy_ _Assess-_
_ment_ _—-_ _Toward_ _a_ _Sustainable_ _Future_ . Cambridge UK:
Cambridge Univ. Press.

IPCC. 2024a. How does the IPCC select its authors? https://www.ipcc.ch/site/assets/uploads/2024/04/
IPCCFactSheet_SelectAuthors.pdf. Accessed May 2024.

IPCC. 2024b. What is the IPCC? https://www.ipcc.ch/site/
assets/uploads/2024/04/IPCCFactSheet_WhatisIPCC.pdf.
Accessed May 2024.

Kasperson, R. E.; and Stern, P. C. 2010. _Facilitating_ _Cli-_
_mate_ _Change_ _Responses:_ _A_ _Report_ _of_ _Two_ _Workshops_ _on_
_Knowledge_ _from_ _the_ _Social_ _and_ _Behavioral_ _Sciences_ . National Academies Press.

Klenk, N.; and Meehan, K. 2015. Climate change and transdisciplinary science: Problematizing the integration imperative. _Environmental science & policy_, 54: 160–167.

Lacombe, R.; Wu, K.; and Dilworth, E. 2023. ClimateX:
Do LLMs Accurately Assess Human Expert Confidence in
Climate Statements? _arXiv preprint arXiv:2311.17107_ .

Li, M.; Gangi Reddy, R.; Wang, Z.; Chiang, Y.-s.; Lai, T.;
Yu, P.; Zhang, Z.; and Ji, H. 2022. COVID-19 Claim Radar:
A Structured Claim Extraction and Tracking System.



Li, X.; Burns, G.; and Peng, N. 2021. Scientific Discourse
Tagging for Evidence Extraction. In _Proceedings of the 16th_
_Conference of the European Chapter of the Association for_
_Computational Linguistics: Main Volume_, 2550–2562.

Mach, K. J.; and Field, C. B. 2017. Toward the next generation of assessment. _Annual_ _Review_ _of_ _Environment_ _and_
_Resources_, 42: 569–597.

Mastrandrea, M. D.; Field, C. B.; Stocker, T. F.; Edenhofer,
O.; Ebi, K. L.; Frame, D. J.; Held, H.; Kriegler, E.; Mach,
K. J.; Matschoss, P. R.; Plattner, G.-K.; Yohe, G. W.; and
Zwiers, F. W. 2010. Guidance Note for Lead Authors of
the IPCC Fifth Assessment Report on Consistent Treatment
of Uncertainties. https://www.ipcc.ch/site/assets/uploads/
2017/08/AR5_Uncertainty_Guidance_Note.pdf.

Meredith, M.; Orr, A.; Shanklin, J.; Turner, J.; et al. 2014.
Assessment for Decision-Makers: Scientific Assessment of
Ozone Depletion: 2014.

Meuschke, N.; Jagdale, A.; Spinde, T.; Mitrovi´c, J.; and
Gipp, B. 2023. A benchmark of pdf information extraction
tools using a multi-task and multi-domain evaluation framework for academic documents. In _International Conference_
_on Information_, 383–405. Springer.

Miller, D. L. 1947. The nature of scientific statements. _Phi-_
_losophy of Science_, 14(3): 219–223.

OpenAI. 2023. GPT-4: Generative Pre-trained Transformer
4. https://openai.com/research/gpt-4.

OpenAI. 2023. GPT-4 Technical Report. _arXiv_ _preprint_
_arXiv:2303.08774_ .

Shukla, P.; Skea, J.; Slade, R.; Khourdajie, A. A.; van
Diemen, R.; McCollum, D.; Pathak, M.; Some, S.; Vyas, P.;
Fradera, R.; Belkacemi, M.; Hasija, A.; Lisboa, G.; Luz, S.;
and (eds.), J. M. 2022. _Climate_ _Change_ _2022:_ _Mitigation_
_of_ _Climate_ _Change._ _Contribution_ _of_ _Working_ _Group_ _III_ _to_
_the Sixth Assessment Report of the Intergovernmental Panel_
_on Climate Change_ . Cambridge, UK: Cambridge University
Press.

Stede, M.; and Patz, R. 2021. The climate change debate
and natural language processing. In _Proceedings_ _of the_ _1st_
_Workshop on NLP for Positive Impact_, 8–18.

Surdeanu, M.; Nallapati, R.; and Manning, C. 2010. Legal
claim identification: Information extraction with hierarchically labeled data. In _LREC Workshop_, 22. Citeseer.

UN Advisory Body on Artificial Intelligence.
2023. Interim Report: Governing AI for Humanity.
https://www.un.org/sites/un2.un.org/files/un_ai_advisory_
body_governing_ai_for_humanity_interim_report.pdf.
Accessed: May 2024.

Vaghefi, S. A.; Stammbach, D.; Muccione, V.; Bingler, J.;
Ni, J.; Kraus, M.; Allen, S.; Colesanti-Senni, C.; Wekhof, T.;
Schimanski, T.; et al. 2023. ChatClimate: Grounding conversational AI in climate science. _Communications Earth &_
_Environment_, 4(1): 480.

Walter, S.; Görlach, J.; and Brüggemann, M. 2020. Climate
feedback: Science comments on journalism and develops
multi-system competence. _Publizistik_, 65: 567–589.


Figure 5: The IPCC AR6 framework for applying expert
judgment in the evaluation of degrees such as confidence
and likelihood of statements (cf. IPCC AR6 WGII Figure
TS.1).


Winkler, B.; Cook, J.; Lubitz, T.; and Rice, K. 2021. Skeptical Science. In _World_ _Scientific_ _Encyclopedia_ _of_ _Climate_
_Change:_ _Case_ _Studies_ _of_ _Climate_ _Risk,_ _Action,_ _and_ _Oppor-_
_tunity Volume 1_, 301–314. World Scientific.

Wuehrl, A.; Grimminger, L.; and Klinger, R. 2023. An
Entity-based Claim Extraction Pipeline for Real-world
Biomedical Fact-checking. In Akhtar, M.; Aly, R.;
Christodoulopoulos, C.; Cocarascu, O.; Guo, Z.; Mittal,
A.; Schlichtkrull, M.; Thorne, J.; and Vlachos, A., eds.,
_Proceedings_ _of_ _the_ _Sixth_ _Fact_ _Extraction_ _and_ _VERification_
_Workshop_ _(FEVER)_, 29–37. Dubrovnik, Croatia: Association for Computational Linguistics.


**Appendix**

**1. Uncertainty Degrees Defined in the IPCC AR6**

IPCC conducted guidelines for determining the degree of
certainty of statements, which is mapped into the confidence
and likelihood levels in our statement profile. The guideline
framework is shown in Figure 5.
Besides the distribution of the statements with confidence
levels ( _C_ ) shown in the main context (cf. Figure 2), the distribution of the statements according to the likelihood levels
( _L_ ) is shown in Table 4. Similar to _C_, the majority of statements in _L_ also located in chapter bodies. And the likelihood
labels are quite imbalanced, over 80% of _L_ are with a _likely_
label or a _very likely_ label.


**2. Semantic Similarity between Statements**

As described in Case Study 1, we calculate the cosine similarity between each statement ( _di_ ) in the 12-statement set
_D_ and all the statements in _N_ . Figure 7 illustrates the distribution of similarity scores for each _di_, with each box representing these scores. To determine the threshold _θ_ for defin


Figure 6: Setting different threshold _θ_, the corresponding
difference in average similarity score between linked and not
linked statement pairs. The vertical dashed lines indicate the
most steep gap between two possible thresholds, we chose
_θ_ = 0 _._ 78.


ing similar statements, we compared the average similarity
score between linked and unlinked statements under various
candidate thresholds. The results are shown in Figure 6. Notably, the steepest increase in the gap occurs between 0.77
and 0.78, leading us to set _θ_ = 0 _._ 78.


**3. Statements with the Key Term ‘wetland’**


The 38 statements that contain the key term “Wetland” are
shown in Table 8.
And the categories of the key terms that are highlighted
in different colors in the Table 8 are generated by asking
ChatGPT to categorize the key terms three times and the ensemble result of the three-time categorization. The mapping
dictionary is defined in Table 5, each cluster of key terms
is named as a category based on their meanings in climate
change domain.


**4. Merging Likelihood Level Variations**


We (the authors of this paper) found 12 variations of the
likelihood levels and manually combined them based on domain knowledge. The dictionary provided below illustrates
the mapping relationships, where the variation is the key and
the corresponding original likelihood level is the value.


 - **virtual certainty** : _virtually certain_

 - **very likely to be virtually certain** : _virtually certain_

 - **high certainty** : _virtually certain_

 - **highly likely** : _very likely_

 - **more or less likely** : _likely_

 - **more likely** : _likely_

 - **likely than not** : _more likely than not_

 - **as likely as not** : _about as likely as not_

 - **less likely** : _unlikely_

 - **not likely** : _unlikely_

 - **large uncertainty** : _unlikely_

 - **deep uncertainty** : _extremely unlikely_


Out of the 1,508 statements in set _L_, 29 of them have variant levels of likelihood and have been matched to the original 10 scales using the dictionary above.


|Col1|Total|SummPol|TechSumm|Chapters|Col6|
|---|---|---|---|---|---|
||**Total**|**SummPol**|**TechSumm**|**ChapSumm**|**ChapBody**|
|Virtually certain|169|4|33|21|111|
|Extremely likely|32|1|4|0|27|
|Very likely|442|17|37|36|352|
|Likely|752|16|67|90|579|
|More likely than not|20|0|1|2|17|
|About as likely as not|3|0|0|0|3|
|Unlikely|32|0|0|1|31|
|Very unlikely|13|0|0|2|11|
|Extremely unlikely|25|1|1|2|21|
|**Total**|1,488|39|143|154|1,152|


Table 4: Distribution of likelihood levels in Statements, including the number of statements by likelihood label: Summary for
Policymakers (SummPol), Technical Summary (TechSumm), Executive Summary of Chapters (ChapSumm) and the remaining
Chapter contents (ChapCont).


Figure 7: Comparative distribution of semantic similarity scores between each statement in _D_ (Statement 1–12) and all statements in _N_ .



**5. Evidence Sentences for Reference Checking**

Given the 21 candidate evidence sentences and the statement
_s_ 1, the top three evidence sentences with the highest similarity scores are shown in Table 6.
Based on the abstracts of the 17 cited papers in the three
evidence sentences, we build a word cloud of the concatenated abstracts. Specifically, we apply tokenization and
lemmatization using SpaCy, and remove English stopwords
(along with ‘climate”, ‘change’ and any single-character
words).


**6. Comparison with ChatGPT generated Statement**
**of ‘wetland restoration’**

To compare our statement extraction results (cr. Table 3)
with large language models (LLMs), we further employ
GPT-4 [8] model to extract statements on ‘wetland restoration’.
The full prompt is:


8https://chatgpt.com/?model=gpt-4



“You are a chatbot with knowledge in climate change
and IPCC report.
Using the prior knowledge in climate question, answer the user’s question, citing references back to the
report whenever possible.
Provide examples whenever possible.
Use clear, simple and concise language.
When citing references to the IPCC report, return a
link in Markdown format. E.g. if the citing Working Group (WG2) Chapter 3, Section 3.5.2, then return [IPCC WG2 3.5.2](https://www.ipcc.ch/report/
ar6/wg2/chapter/chapter-3/##3.5.2)”


Given the prompt and the query as “What are the main
scientific statements on wetland restoration?”, the full responses generated by vanilla GPT and RAG-based GPT, together with our retrieved statements, are listed in Table 7.


|Category|Key Terms|
|---|---|
|Climate<br>Pro-<br>cesses|Climate variability, Climate change,<br>Global warming, Green infrastructure|
|Climate Impact|Sea level rise (SLR), Flood, Drought,<br>Heat island, Impacts|
|Ecosystem Ser-<br>vices|Biodiversity,<br>Carbon<br>sequestration,<br>Ecosystem, Cultural services, Wetland|
|Climate<br>Re-<br>sponse|Adaptation,<br>Mitigation,<br>Resilience,<br>Sustainable development, Restoration,<br>Reforestation|
|Risk and Vul-<br>nerability|Risk, Disaster risk, Vulnerability, Un-<br>certainty, Trade-off|
|Energy and Re-<br>sources|Fossil<br>fuels,<br>Bioenergy,<br>Energy,<br>Biomass|
|Socio-<br>economic<br>Factors|Inequality,<br>Livelihood,<br>Access<br>(to<br>food), Health, Opportunities|
|Management<br>and Policy|Adaptive<br>capacity,<br>Capacity,<br>Forest<br>management,<br>Land<br>use,<br>Emissions,<br>Agreement|
|Miscellaneous|Assets, Stock, Driver, Technical poten-<br>tial, Region, Confdence, Urban, Po-<br>tential, Infrastructure, Measures, Trans-<br>formation, Drivers, Soil erosion, Land,<br>Scenario, Emission scenario, Peatlands,<br>Evidence, Cost, Settlements, Sequestra-<br>tion, Carbon stock, Forest, Extinction,<br>Agroecology, Co-benefts|



Table 5: The ChatGPT categorized key term categories.


**Similarity score** **Evidence sentence**


0.69 Elevated water levels also alter submergence
patterns for intertidal habitat (high confidence) (Andres et al., 2019), moving highwater levels inland (high confidence) (Peteet
et al., 2018; Appeaning Addo et al., 2020;
Liu et al., 2020e) and increasing the salinity
of coastal water tables and soils (high confidence) (Eswar et al., 2021).


0.65 These processes favour inland and/or upstream migration of intertidal habitat, where
it is unconstrained by infrastructure, topography or other environmental features (high
confidence) (Kirwan and Gedan, 2019;
Parker and Boyer, 2019; Langston et al.,
2020; Magolan and Halls, 2020; Saintilan et
al., 2020).


0.63 Along estuarine shorelines, changing submergence patterns and upstream penetration of saline waters interact synergistically to stress intertidal plants, changing
species composition and reducing aboveground biomass, in some cases favouring invasive species (Xue et al., 2018; Buffington
et al., 2020; Gallego-Tévar et al., 2020).


Table 6: The three most similar evidence sentences to _s_ 1 and
their corresponding similarity scores.


|Vanilla GPT-4|RAG-based GPT-4|Ours.|
|---|---|---|
|Wetland restoration is a critical aspect of climate<br>change mitigation and adaptation, as highlighted in<br>the IPCC reports. Here are some main scientifc<br>statements on wetland restoration based on the IPCC<br>reports:<br>**1. Carbon Sequestration:** Wetlands, including<br>peatlands, mangroves, and salt marshes, are highly<br>effective carbon sinks, storing signifcant amounts of<br>carbon dioxide (CO2) and other greenhouse gases.<br>Restoring degraded wetlands can enhance their ca-<br>pacity to sequester carbon, helping to mitigate cli-<br>mate change (IPCC WG1 5.4).<br>**2.Biodiversity Conservation:** Wetland restoration<br>contributes to biodiversity conservation by provid-<br>ing habitat for numerous plant and animal species.<br>This enhances ecosystem resilience and adaptive ca-<br>pacity in the face of climate change impacts (IPCC<br>WG2 6.5).<br>**3. Flood Regulation:** Healthy wetlands play a cru-<br>cial role in food regulation by absorbing and stor-<br>ing excess water during heavy rainfall events. Wet-<br>land restoration can help reduce the risk of fooding<br>in vulnerable areas, protecting communities and in-<br>frastructure (IPCC WG2 6.5).<br>**4. Coastal Protection:** Coastal wetlands, such as<br>mangroves and salt marshes, act as natural barri-<br>ers against coastal erosion and storm surges. Restor-<br>ing degraded coastal wetlands can enhance coastal<br>protection, reducing the vulnerability of coastal<br>communities to sea-level rise and extreme weather<br>events (IPCC WG2 6.5).<br>**5. Water Quality Improvement:** Wetlands play a<br>vital role in purifying water by fltering out pollu-<br>tants and excess nutrients. Restoring degraded wet-<br>lands can improve water quality, benefting both<br>ecosystems and human populations dependent on<br>clean water sources (IPCC WG3 5.4).<br>Overall, the IPCC reports emphasize the importance<br>of wetland restoration as a nature-based solution for<br>climate change mitigation and adaptation, offering<br>multiple co-benefts for biodiversity, ecosystem ser-<br>vices, and human well-being.|There are several main scientifc state-<br>ments on wetland restoration based on the<br>IPCC reports:<br>Coastal wetland restoration has a tech-<br>nical potential of 0.3 GtCO2-eq yr –1<br>and it offers valuable co-benefts includ-<br>ing enhanced water quality, biodiversity,<br>aesthetic values, and protection from ris-<br>ing sea levels. It can also help with fsh-<br>eries production, contributing to food se-<br>curity (IPCC WG3 7.4).<br>Restoration of coastal wetlands would de-<br>crease CO2 emissions and net GHG emis-<br>sions from degraded peatlands as per re-<br>cent literature with high to medium conf-<br>dence levels. Their restoration and rewet-<br>ting is crucial to meet 1.5°C–2°C path-<br>ways by 2050 (IPCC WG3 7.4).<br>Wetland restoration and preservation re-<br>store coastal protection and reduce com-<br>munity vulnerability to fooding. They<br>also maintain coastal ecosystem services.<br>Restoring coral reefs, oyster reefs, and<br>mangroves dissipate wave energy, accrete<br>sediment and elevate shorelines, thereby<br>reducing exposure to waves and storm<br>surges (IPCC WG2 3.6).<br>Restoration of peatlands helps in the in-<br>creased accumulation of carbon in vege-<br>tation and soils and avoids ongoing CO2<br>emissions. They deliver co-benefts for<br>biodiversity, regulate water fow, prevent<br>downstream fooding and reduce the risk<br>of fre (IPCC WG3 7.4).<br>It is important to note that there are risks<br>involved in the restoration of wetlands,<br>such as uncertain permanence under fu-<br>ture climate scenarios, increased methane<br>and nitrous oxide release, and confict with<br>other land uses IPCC WG3 7.4. There-<br>fore, restoration projects should involve<br>local communities at all stages and con-<br>sider both biophysical and socio-political<br>contexts.|• (WGII TS.D) Restoration of wetlands could<br>support livelihoods and help sequester carbon<br>(_medium confdence_), provided they are al-<br>lowed accommodation space.<br>• (WGII 12.ES) Inclusive water regimes that<br>overcome social inequalities and approaches<br>including nature-based solutions, such as wet-<br>land restoration and water storage and infltra-<br>tion infrastructure, with synergies for ecosys-<br>tem conservation and disaster risk reduction,<br>have been found to be more successful for<br>adaptation and sustainable development (_high_<br>_confdence_).<br>• (WGII 3.4) Without careful management<br>of freshwater inputs, sediment augmentation<br>and/or the restoration of shorelines to more<br>natural states, transformation and loss of in-<br>tertidal areas and wetland vegetation will in-<br>crease with SLR (_high confdence_) (Doughty<br>et al., 2019; Leuven et al., 2019; Yu et al.,<br>2019; Raw et al., 2020; Shih, 2020; Stein<br>et al., 2020), with small, shallow microtidal<br>estuaries being more vulnerable to impacts<br>than deeper estuaries with well-developed sed-<br>iments (_medium confdence_) (Leuven et al.,<br>2019; Williamson and Guinder, 2021).<br>• (WGIII 7.4) There is_ medium confdence_ that<br>coastal wetland restoration has a technical po-<br>tential of 0.3 (0.04–0.84) gtco2-eq yr –1 of<br>which 0.1 (0.05–0.2) gtco2-eq yr –1 is avail-<br>able up to usd100 tco2–1.<br>• (WGIII 7.4) There is _high confdence_ that<br>coastal<br>wetlands,<br>especially<br>mangroves,<br>contain large carbon stocks relative to other<br>ecosystems<br>and<br>_medium_<br>_confdence_<br>that<br>restoration<br>will<br>reinstate<br>pre-disturbance<br>carbon sequestration rates.<br>• (WGIII 7.4) There is _low confdence_ on<br>the response of coastal wetlands to climate<br>change; however, there is_ high confdence_ that<br>coastal wetland restoration will provide a suite<br>of valuable co-benefts.|



Table 7: Zero-shot learning GPT, RAG-based GPT and our retrieved ‘wetland restoration’-related statements.


|WG|Chapter|Section|Text|
|---|---|---|---|
|WGI|TechSumm|TechSumm.2|There is_ high confdence_ that this recent growth is largely driven by emissions from fossil<br>fuel exploitation, livestock, and waste, with ENSO driving multi-annual variability of wet-<br>land and biomass burning emissions.|
|WGII|SummPol|SummPol.C|Coastal wetlands protect against coastal erosion and fooding associated with storms and<br>sea level rise where suffcient space and adequate habitats are available until rates of sea<br>level rise exceed natural adaptive capacity to build sediment (_very high confdence_).|
|WGII|SummPol|SummPol.C|Natural river systems, wetlands and upstream forest ecosystems reduce food risk by storing<br>water and slowing water fow, in most circumstances (_high confdence_).|
|WGII|SummPol|SummPol.C|Enhancing natural water retention such as by restoring wetlands and rivers, land use plan-<br>ning such as no build zones or upstream forest management, can further reduce food risk<br>(_medium confdence_).|
|WGII|TechSumm|TechSumm.C|TS.C.5.1 Under all emissions scenarios, coastal wetlands will likely face high risk from sea<br>level rise in the mid-term (_medium confdence_), with substantial losses before 2100.|
|WGII|TechSumm|TechSumm.D|The options include vulnerability-reducing measures, avoidance (e.g., disincentivising de-<br>velopments in high-risk areas and addressing existing social vulnerabilities), hard and soft<br>protection (e.g., sea walls, coastal wetlands), accommodation (e.g., elevating houses), ad-<br>vance (e.g., building up and out to sea) and staged, managed retreat (e.g., landward move-<br>ment of people and development) interventions (_very high confdence_).|
|WGII|TechSumm|TechSumm.D|Nature-based interventions, for example wetlands and salt marshes, can reduce impacts and<br>costs while supporting biodiversity and livelihoods but have limits under high warming<br>levels and rapid sea level rise (_high confdence_).|
|WGII|TechSumm|TechSumm.D|Restoration of wetlands could support livelihoods and help sequester carbon (_medium con-_<br>_fdence_), provided they are allowed accommodation space.|
|WGII|TechSumm|TechSumm.D|Flood-risk measures that work with nature by allowing fooding within coastal and wet-<br>land ecosystems and support sediment accretion can reduce costs and bring substantial co-<br>benefts to ecosystems, liveability and livelihoods (_high confdence_).|
|WGII|12<br>Central<br>and<br>South<br>America|12.ES|Disruption in water fows will signifcantly degrade ecosystems such as high-elevation wet-<br>lands and affect farming communities, public health and energy production (_high conf-_<br>_dence_).|
|WGII|12<br>Central<br>and<br>South<br>America|12.ES|Inclusive water regimes that overcome social inequalities and approaches including nature-<br>based solutions, such as wetland restoration and water storage and infltration infrastructure,<br>with synergies for ecosystem conservation and disaster risk reduction, have been found to<br>be more successful for adaptation and sustainable development (_high confdence_).|
|WGIII|SummPol|SummPol.D|D.2.1 Sustainable urban planning and infrastructure design including green roofs and fa-<br>cades, networks of parks and open spaces, management of urban forests and wetlands, urban<br>agriculture, and water-sensitive design can deliver both mitigation and adaptation benefts<br>in settlements (_medium confdence_).|


|WG|Chapter|Section|Text|
|---|---|---|---|
|WGII|3<br>Ocean<br>and<br>coastal<br>ecosystems<br>and<br>their<br>services|3.4|Overall, warming will drive range shifts in wetland species (medium to _high confdence_),<br>but SLR poses the greatest risk for mangroves and salt marshes, with signifcant losses<br>projected under all future scenarios by mid-century (_medium confdence_) and substantially<br>greater losses by 2100 under all scenarios except SSP1-1.9 (_high confdence_).|
|WGII|3<br>Ocean<br>and<br>coastal<br>ecosystems<br>and<br>their<br>services|3.4|Under SSP5-8.5, wetlands are very likely at high risk from SLR, with larger impacts mani-<br>festing before 2040 (_medium confdence_).|
|WGII|3<br>Ocean<br>and<br>coastal<br>ecosystems<br>and<br>their<br>services|3.4|Otherwise, wetland ecosystems must migrate either inland or upstream, or face gradual<br>submergence in deeper, increasingly saline water (_very high confdence_) (section 3.4.2.4;<br>Andres et al., 2019; Jones et al., 2019b; Cohen et al., 2020; Maf-Gholami et al., 2020;<br>Magolan and Halls, 2020; Sklar et al., 2021).|
|WGII|3<br>Ocean<br>and<br>coastal<br>ecosystems<br>and<br>their<br>services|3.4|Nevertheless, previous declines have left wetland ecosystems more vulnerable to impacts<br>from climate-induced drivers and non-climate drivers (_high confdence_) (Friess et al., 2019;<br>Williamson and Guinder, 2021).|
|WGII|3<br>Ocean<br>and<br>coastal<br>ecosystems<br>and<br>their<br>services|3.4|Since AR5 and SRCCL, syntheses have emphasised that the vulnerability of rooted wetland<br>ecosystems to climate-induced drivers is exacerbated by non-climate drivers (_high conf-_<br>_dence_) (Elliott et al., 2019; Ostrowski et al., 2021; Williamson and Guinder, 2021) and cli-<br>mate variability (_high confdence_) (Day and Rybczyk, 2019; Kendrick et al., 2019; Shields<br>et al., 2019).|
|WGII|3<br>Ocean<br>and<br>coastal<br>ecosystems<br>and<br>their<br>services|3.4|Without careful management of freshwater inputs, sediment augmentation and/or the<br>restoration of shorelines to more natural states, transformation and loss of intertidal ar-<br>eas and wetland vegetation will increase with SLR (_high confdence_) (Doughty et al., 2019;<br>Leuven et al., 2019; Yu et al., 2019; Raw et al., 2020; Shih, 2020; Stein et al., 2020), with<br>small, shallow microtidal estuaries being more vulnerable to impacts than deeper estuaries<br>with well-developed sediments (_medium confdence_) (Leuven et al., 2019; Williamson and<br>Guinder, 2021).|
|WGII|4 Water|4.3|Many wetland-dependent species have seen a long-term decline, with the Living Planet<br>Index showing that 81% of populations of freshwater species are in decline and others being<br>threatened by extinction (Davidson and Finlayson, 2018; Darrah et al., 2019; Diaz et al.,<br>2019) (_high confdence_).|
|WGII|4 Water|4.3|The loss and degradation of freshwater ecosystems have been widely documented, and SR-<br>CCL assessed with_ medium confdence_ the loss of wetlands since the 1970s (Olsson et al.,<br>2020).|


|WG|Chapter|Section|Text|
|---|---|---|---|
|WGII|4 Water|4.5|SR1.5 concluded with _high confdence_ that limiting global warming to 1.5°C, rather than<br>2°C, will strongly beneft terrestrial and wetland ecosystems and their services, including<br>the cultural services provided by these ecosystems (Hoegh-Guldberg et al., 2018).|
|WGII|11 Australa-<br>sia|11.3|Improved coastal modelling, experiments and in situ studies are reducing uncertainties at a<br>local scale about the impact of future sea level rise (SLR) on coastal freshwater terrestrial<br>wetlands (_medium confdence_) (Shoo et al., 2014; Bayliss et al., 2018; Grieger et al., 2019).|
|WGII|12<br>Central<br>and<br>South<br>America|12.3|Drought has affected wetlands (_low confdence_) (Zhao et al., 2016; Domic et al., 2018) and<br>desert ecosystems (_medium confdence_: medium evidence, high agreement) (Acosta-Jamett<br>et al., 2016; Neilson et al., 2017; Díaz et al., 2019).|
|WGII|12<br>Central<br>and<br>South<br>America|12.3|The projected impacts of climate change will lead to profound changes in the annual food<br>dynamics for Pantanal wetlands, altering ecosystem functioning and severely affecting bio-<br>diversity (_high confdence_) (Thielen et al., 2020; Marengo et al., 2021).|
|WGII|12<br>Central<br>and<br>South<br>America|12.3|Disruptions in water fows will signifcantly degrade or eliminate high-elevation wetlands<br>(_high confdence_) (Bury et al., 2013; Dangles et al., 2017; Mark et al., 2017; Polk et al.,<br>2017; Cuesta et al., 2019).|
|WGII|13 Europe|13.3|Appropriately implemented ecosystem-based mitigation, such as reforestation with climate-<br>resilient native species (section 13.3.1.4), peatland and wetland restoration, and agroecology<br>(section 13.5.2), can enhance carbon sequestration or storage (_medium confdence_) (Seddon<br>et al., 2020).|
|WGII|13 Europe|13.3|Trade-offs between ecosystem protection, their services and human adaptation and miti-<br>gation needs can generate challenges, such as loss of habitats, increased emissions from<br>restored wetlands (Günther et al., 2020) and conficts between carbon capture services, and<br>provisioning of bioenergy, food, timber and water (_medium confdence_) (Lee et al., 2019;<br>Krause et al., 2020).|
|WGII|13 Europe|13.3|Average wetland area is not projected to change at 1.7°C GWL across Europe, while for<br>>4°C GWL expanding sites in NEU are not suffcient to balance losses in SEU and WCE<br>(_high confdence_) (Xi et al., 2021).|
|WGII|13 Europe|13.4|While rising sea levels will also directly threaten intertidal and beach ecosystems, coastal<br>wetlands will beneft (_medium confdence_), in case lateral accommodation space and the op-<br>portunity for systems to migrate landward and upwards is provided, enhancing their ability<br>to capture and store carbon (Lecocq et al., 2022; Rogers et al., 2019).|
|WGII|13 Europe|13.10|Ecosystem-based solutions, such as wetlands, can reduce waves’ propagation, provide co-<br>benefts for the environment and climate mitigation, and reduce costs for food defences<br>(_medium confdence_) (section 13.2.2.1).|
|WGII|13 Europe|13.10|Around 2°C GWL, losses accelerate in marine ecosystem and appear across systems, in-<br>cluding habitat losses especially in coastal wetlands (Roebeling et al., 2013; Clark et al.,<br>2020), biodiversity and biomass losses (Bryndum-Buchholz et al., 2019; Lotze et al., 2019)<br>and ecosystem services such as fshing (_high confdence_ on the direction of change, but<br>_medium confdence_ on the local and regional magnitude) (Raybaud et al., 2017).|


|WG|Chapter|Section|Text|
|---|---|---|---|
|WGII|14 North America|14.5|Other adaptation responses to reduce temperature effects include modifying structures (roofs,<br>engineered materials) and the urban landscape through green infrastructure (e.g., urban trees,<br>wetlands, green roofs), which increases climate resilience and quality of life by reducing<br>urban heat island effects, while additionally improving air quality, capturing stormwater and<br>delivering other co-benefts to the community (e.g., access to food, connection to nature,<br>social connectivity) (_high confdence_) (see box 14.7; Ballinas and Barradas, 2016; Emilsson<br>and Sang, 2017; Kabisch et al., 2017; Krayenhoff et al., 2018; Petrovic et al., 2019; Schell et<br>al., 2020).|
|WGII|14 North America|14.5|These environmental conditions also stress natural assets (e.g., urban forests, wetlands,<br>household gardens, green walls) and performance of green infrastructure leading to higher<br>operation and maintenance costs (_high confdence_) (Kabisch et al., 2017; Terton, 2017).|
|WGII|15 Small Islands|15.3|SLR has been projected to impact the terrestrial biodiversity of low-lying islands and coastal<br>regions via large habitat losses both directly (e.g., submergence) and indirectly (e.g., salinity<br>intrusion, salinisation of coastal wetlands and soil erosion) at even the 1-m scenario (medium<br>to_ high confdence_).|
|WGIII|7<br>Agriculture,<br>Forestry<br>and<br>Other Land Uses<br>(AFOLU)|7.4|There is _medium confdence_ that coastal wetland protection has a technical potential of 0.8<br>(0.06–5.4) gtco2-eq yr –1 of which 0.17 (0.06–0.27) gtco2-eq yr –1 is available up to usd100<br>tco2–1.|
|WGIII|7<br>Agriculture,<br>Forestry<br>and<br>Other Land Uses<br>(AFOLU)|7.4|There is _high confdence_ that coastal wetlands, especially mangroves, contain large carbon<br>stocks relative to other ecosystems and_ medium confdence_ that restoration will reinstate pre-<br>disturbance carbon sequestration rates.|
|WGIII|7<br>Agriculture,<br>Forestry<br>and<br>Other Land Uses<br>(AFOLU)|7.4|There is_ low confdence_ on the response of coastal wetlands to climate change; however, there<br>is_ high confdence_ that coastal wetland restoration will provide a suite of valuable co-benefts.|
|WGIII|7<br>Agriculture,<br>Forestry<br>and<br>Other Land Uses<br>(AFOLU)|7.4|There is_ medium confdence_ that coastal wetland restoration has a technical potential of 0.3<br>(0.04–0.84) gtco2-eq yr –1 of which 0.1 (0.05–0.2) gtco2-eq yr –1 is available up to usd100<br>tco2–1.|



Table 8: Statements that contain the keyword term ‘wetland’. The key terms in the statement are highlighted, colors represent
the categories they belong to.


