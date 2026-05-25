This CVPR paper is the Open Access version, provided by the Computer Vision Foundation.
Except for this watermark, it is identical to the accepted version;
the final published version of the proceedings is available on IEEE Xplore.

# **FAME-ViL: Multi-Tasking Vision-Language Model** **for Heterogeneous Fashion Tasks**


Xiao Han [1] _[,]_ [2] Xiatian Zhu [1] _[,]_ [3] Licheng Yu Li Zhang [4] Yi-Zhe Song [1] _[,]_ [2] Tao Xiang [1] _[,]_ [2]

1 CVSSP, University of Surrey 2 iFlyTek-Surrey Joint Research Centre on Artificial Intelligence
3 Surrey Institute for People-Centred Artificial Intelligence 4 Fudan University

_{_ xiao.han, xiatian.zhu, y.song, t.xiang _}_ @surrey.ac.uk


lichengyu24@gmail.com lizhangfd@fudan.edu.cn


**Abstract**



_In_ _the_ _fashion_ _domain,_ _there_ _exists_ _a_ _variety_ _of_ _vision-_
_and-language (V+L) tasks, including cross-modal retrieval,_
_text-guided image retrieval, multi-modal classification, and_
_image_ _captioning._ _They_ _differ_ _drastically_ _in_ _each_ _individ-_
_ual_ _input/output format_ _and_ _dataset size._ _It has_ _been_ _com-_
_mon_ _to_ _design_ _a_ _task-specific_ _model_ _and_ _fine-tune_ _it_ _in-_
_dependently_ _from_ _a_ _pre-trained_ _V+L_ _model_ _(_ e.g _.,_ _CLIP)._
_This_ _results_ _in_ _parameter_ _inefficiency_ _and_ _inability_ _to_ _ex-_
_ploit inter-task relatedness._ _To address such issues, we pro-_
_pose_ _a_ _novel_ _**FA**_ _shion-focused_ _**M**_ _ulti-task_ _**E**_ _fficient_ _learn-_
_ing method for_ _**Vi**_ _sion-and-_ _**L**_ _anguage tasks (_ _**FAME-ViL**_ _) in_
_this work._ _Compared with existing approaches,_ _FAME-ViL_
_applies_ _a_ _single_ _model_ _for_ _multiple_ _heterogeneous_ _fashion_
_tasks,_ _therefore_ _being_ _much_ _more_ _parameter-efficient._ _It_
_is_ _enabled_ _by_ _two_ _novel_ _components:_ _(1)_ _a_ _task-versatile_
_architecture with cross-attention adapters and task-specific_
_adapters integrated into a unified V+L model, and (2) a sta-_
_ble_ _and_ _effective_ _multi-task_ _training_ _strategy_ _that_ _supports_
_learning_ _from_ _heterogeneous_ _data_ _and_ _prevents_ _negative_
_transfer._ _Extensive experiments on four fashion tasks show_
_that_ _our_ _FAME-ViL_ _can_ _save_ _61.5%_ _of_ _parameters_ _over_
_alternatives,_ _while_ _significantly_ _outperforming_ _the_ _conven-_
_tional_ _independently_ _trained_ _single-task_ _models._ _Code_ _is_
_available at https://github.com/BrandonHanx/FAME-ViL._


**1. Introduction**


A variety of real-world multi-modal, particularly Visionand-Language (V+L) tasks exist in the fashion domain, including multi-modal recognition [44, 53, 61], multi-modal
retrieval [21, 83] and image captioning [85]. The models
developed for these tasks have been applied in diverse ecommerce applications, improving product discoverability,
seller-buyer engagement, and customer conversion rate after catalogue browsing. Intrinsically, those V+L tasks are



Figure 1. By multi-task learning a single model for heterogeneous
fashion tasks, our FAME-ViL can significantly improve parameter
efficiency, while boosting the model performance per task over
existing _independently_ _fine-tuned_ _single-task_ _models_ . Note, each
axis is **normalized** according to the respective maximum value for
easier visualization.


_**heterogeneous**_ in terms of (1) different input and output
formats ( _e.g_ ., text-guided garment retrieval [83] and image
captioning [85] have completely different inputs and outputs); (2) different dataset sizes as the annotation difficulty
of each task differ ( _e.g_ ., the labeling effort for text-guided
image retrieval is much harder than that for text-to-image
retrieval [48, 83]).
Due to the heterogeneous nature of the V+L fashion
tasks, existing methods [21, 24, 33, 87, 94] typically take
a pre-trained generic V+L model [7, 38, 41–43, 49, 60, 67,
72, 79] and fine-tune it on every single task independently.
Such an approach suffers from two limitations. _**(1) Low pa-**_
_**rameter efficiency**_ : Each real-world application requires the
deployment of its dedicated fine-tuned model, where there
is no parameter or inference computation sharing. This
leads to a linearly increasing storage and inference compute redundancy in the long run. _**(2)**_ _**Lack**_ _**of**_ _**inter-task**_
_**relatedness**_ : Though the fashion tasks are heterogeneous



2669


in nature, the fundamental components of the models are
closely related in that all tasks require a deep content (image/sentence) understanding. Exploiting the shared information across tasks thus has the potential to improve model
generalization capability leading to a performance boost.

Perhaps a natural solution would be applying Multi-Task
Learning (MTL) [13]. However, most existing multi-task
training methods [8, 36, 46, 56, 63] are designed for homogeneous tasks ( _i.e_ ., one dataset with multi-task labels) and
thus cannot be directly applied to the heterogeneous fashion
tasks. In our case, we are facing two challenges in building the fashion-domain MTL model: (1) _Architecturally_, it
is non-trivial to model the diverse tasks in one unified architecture. Taking the popular CLIP [60] as an example,
its two-stream architecture is designed for image-text alignment [52] and thus lacks the modality fusion mechanism as
required by many V+L fashion tasks ( _e.g_ ., text-guided image retrieval [2,83] and image captioning [85]). (2) In terms
of _optimization_, a fashion-domain MTL model is prone to
the notorious _negative transfer_ problem [8,13,36,46,56,63]
due to both task input/output format differences and imbalanced dataset sizes. To the best of our knowledge, there has
been no attempt at V+L MTL for the fashion domain.

In this work, we introduce a novel **FA** shion-focused
**M** ulti-task **E** fficient learning method for various **Vi** sionand- **L** anguage based fashion tasks, dubbed as **FAME-ViL** .
It achieves superior performance across a set of diverse
fashion tasks with much fewer parameters as in Fig. 1.
Specifically, we design a task-versatile architecture on top
of a pre-trained generic V+L model ( _i.e_ ., CLIP [60]). To
adapt the simple two-stream architecture of CLIP to various
fashion tasks, we introduce a lightweight _**Cross-Attention**_
_**Adapter (XAA)**_ to enable the cross-modality interaction between the two streams. It makes the model flexible to
support multiple task modes ( _e.g_ ., contrastive mode for
retrieval, fusion mode for understanding, and generative
mode for generation). To address the negative transfer challenge, we introduce a _**Task-Specific**_ _**Adapter**_ _**(TSA)**_ to absorb inter-task input/output format incompatibilities by introducing lightweight additional per-task parameters. For
further handling the dataset imbalance problem, a _**multi-**_
_**teacher**_ _**distillation**_ scheme [12] is formulated for our heterogeneous MTL problem. It leverages the pre-trained pertask teachers to guide the optimization of our multi-task
model, mitigating the overfitting risks of those tasks with
smaller training dataset sizes.
Our _**contributions**_ are summarized as follows: **(I)** For the
first time, we investigate the problem of multi-task learning
on heterogeneous fashion tasks, eliminating the parameter
redundancy and exploiting the inter-task relatedness. **(II)**
We propose FAME-ViL with two novel adapters, adapting
a pre-trained CLIP model to all tasks. **(III)** We introduce
an efficient and effective multi-task training strategy sup

|Cross-Modal Retrieval (XMR)|Col2|
|---|---|
|**Text Query:Long sleeve relaxed-fit silk blazer in light peach. Shawl collar. Single-button**<br>**closure and patch pockets at front. Breast pocket. Slits at sleeve cuffs. Vented at back.**<br>|**Text Query:Long sleeve relaxed-fit silk blazer in light peach. Shawl collar. Single-button**<br>**closure and patch pockets at front. Breast pocket. Slits at sleeve cuffs. Vented at back.**<br>|
|||
|**Reference ImageModifying Text:is a black and white dress, is strapless**<br>**Text-Guided Image Retrieval (TGIR)**|**Reference ImageModifying Text:is a black and white dress, is strapless**<br>**Text-Guided Image Retrieval (TGIR)**|
|**Generated Caption:Grey & brown**<br>**camo print tank top. Relaxed-fit**<br>**tank top in tones of grey, brown,**<br>**and black. Signature snake**<br>**graphic print throughout. Ribbed**<br>**crewneck collar. Tonal stitching.**<br>**Fashion Image Captioning (FIC)**<br>**Slouchy lamb nubuck patrol**<br>**hat in black. Wrinkling and**<br>**light distressing throughout.**<br>**Fully lined.**<br>**Predicted Class:[FLAT CAPS]**<br>**Sub-Category Recognition (SCR)**|**Generated Caption:Grey & brown**<br>**camo print tank top. Relaxed-fit**<br>**tank top in tones of grey, brown,**<br>**and black. Signature snake**<br>**graphic print throughout. Ribbed**<br>**crewneck collar. Tonal stitching.**<br>**Fashion Image Captioning (FIC)**<br>**Slouchy lamb nubuck patrol**<br>**hat in black. Wrinkling and**<br>**light distressing throughout.**<br>**Fully lined.**<br>**Predicted Class:[FLAT CAPS]**<br>**Sub-Category Recognition (SCR)**|
|**Slouchy lamb nubuck patrol**<br>**hat in black. Wrinkling and**<br>**light distressing throughout.**<br>**Fully lined.**<br>**Predicted Class:[FLAT CAPS]**<br>**Sub-Category Recognition (SCR)**|**Generated Caption:Grey & brown**<br>**camo print tank top. Relaxed-fit**<br>**tank top in tones of grey, brown,**<br>**and black. Signature snake**<br>**graphic print throughout. Ribbed**<br>**crewneck collar. Tonal stitching.**<br>**Fashion Image Captioning (FIC)**|



Figure 2. An illustration of four diverse fashion V+L Tasks studied
in this work: cross-modal retrieval, text-guided image retrieval,
sub-category recognition, and fashion image captioning. Note,
all predictions shown in this figure are made by our FAME-ViL.
Green box indicates the ground truth matches of retrieval tasks.


porting heterogeneous task modes in one unified model.
**(IV)** Comprehensive experiments on four diverse fashion
tasks ( _i.e_ ., cross-modal retrieval [52, 61], text-guided image retrieval [75, 83], multi-modal classification [61, 94],
and image captioning [85]) show that our method significantly outperforms the previous single-task state-of-the-art
with 61.5% parameter saving (see Fig. 1).


**2. Related work**


**Vision-Language Pre-training (VLP).** With the advent of
Transformers [15, 17, 73], many pioneer studies [7, 31, 38,
41–43, 49, 67, 88] have demonstrated that VLP is effective
in boosting various downstream V+L tasks in the generic
domain. Since then, we have witnessed further developments of VLP methods, being bigger [19, 34, 60], more unified [50, 66, 77–79, 86] and more flexible [18, 76, 84].
**Fashion** **V+L** **tasks.** There exist a variety of heterogeneous tasks in the fashion domain. As depicted in Fig. 2,
we consider four popular fashion tasks in this work: (1)
_Cross-Modal_ _Retrieval_ _(XMR)_ requests to efficiently retrieve the most matched image/sentence from a large candidate pool given a text/image query [52, 61]. (2) _Text-_
_Guided_ _Image_ _Retrieval_ _(TGIR)_ is a special type of image retrieval with a multi-modal query (a combination of
a reference image and a modifying text) matched against
a set of images [6, 14, 23, 37, 39, 64]. It not only requires a strong fusion of the reference image and modifying text, but also an efficient matching between the fused
representation and all candidate images [24, 83]. (3) _Sub-_









2670


_Category Recognition (SCR)_ requires an accurate class prediction made upon the fusion of an image-text pair [61, 94].
(4) _Fashion_ _Image_ _Captioning_ _(FIC)_ generates a caption
to describe the given image with semantically meaningful,
fine-grained, and accurate words [85]. Many recent works
have been trying to address these fashion tasks through
VLP [21, 22, 24, 33, 55, 87, 94]. Most of them focus on the
pre-training, then simply fine-tune the pre-trained model on
each downstream task independently. In contrast, we integrate all these tasks into a unified architecture and thus no
separate fine-tuning is needed. Since our fashion data is also
abundant, most early works pre-train on the fashion domain
directly. However, a number of recent works [2,3,10,16,52]
suggest that a generic-domain pre-trained CLIP [60] generalizes even better on the fashion tasks. In this work, we also
exploit a pre-trained CLIP model. Different from the existing methods, we use a single multi-task learned model for
all mentioned fashion tasks during fine-tuning.


**Parameter-efficient tuning.** Due to the increase in the size
of V+L models, there is a growing interest in developing
parameter-efficient methods to quickly adapt a large pretrained model to specific tasks by using as few extra parameters as possible. The most representative methods are
adapters [5, 9, 28, 70], prompt tuning [35, 47, 91, 92], lowrank adaptation [29] and their unified variants [25, 54, 89].
Interestingly, whilst MTL can save much larger parameters,
it is under-studied in V+L modeling. In this work, we propose two kinds of adapters (XAA and TSA in Sec. 3.1) to
adapt CLIP specifically designed for MTL in the fashion
domain. Besides parameter-efficiently adapting CLIP, our
proposed adapters also serve as the key component for taskversatile architecture design and enabling stable MTL.


**Multi-task** **learning.** Although MTL offers many advantages like improved data efficiency and reduced over-fitting,
how to avoid negative transfer and cope with severely imbalanced dataset sizes is still an open question. One common solution is to weight per-task losses or combine pertask gradients into a joint update direction using various
heuristics [8, 36, 46, 56, 63]. These works require the MTL
model to have at least one forward propagation on each task
so that they can manipulate the overall losses or gradients.
However, since V+L tasks are typically heterogeneous (especially in the fashion domain), this requirement cannot be
easily satisfied, making these methods not directly applicable. In contrast, Task Sampling-based MTL (TS-MTL) is
without such a requirement and thus being widely adopted
in V+L models [7, 30, 51, 57, 66]. In TS-MTL, only one
task along with its data point is sampled per iteration. Since
then, the heuristic task-sampling strategies [30, 32, 51] have
been proposed, aiming to balance different tasks, avoiding
the _over-fitting_ on easier (or data-poor) tasks or _catastrophic_
_forgetting_ [20] on harder (or data-rich) tasks. However, it is
found that TS-MTL on its own often underperforms single




















Figure 3. An illustration of a task-versatile Transformer layer
equipped with two newly-introduced adapters: cross-attention
adapter (XAA) and task-specific adapter (TSA).


task trained models; it is thus typically followed by an additional per-task fine-tuning step [30, 51]. In this work, we
formulate an effective knowledge distillation with multiple
single-task teachers [12] to alleviate the negative transfer
without further fine-tuning on each task.


**3. Methodology**


We aim to address the most popular fashion tasks (shown
in Fig. 2) using one single unified model. We introduce the
details of our proposed FAME-ViL as follows.


**3.1. Model architecture**


FAME-ViL consists of a language encoder and a vision
encoder initialized from the pre-trained CLIP (ViT-B/16
version) [60], as well as a set of newly introduced adapters.
**Transformer** **layers.** The key component in CLIP is the
transformer backbone [17, 73]. A vanilla Transformer encoder consists of an input embedding layer (word embedding for language input and patch embedding for vision input) and several alternating layers made of Multi-Head SelfAttention (MHSA) and MLP blocks. Layer Normalization
(LN) [1] is applied before every block, and residual connections after every block [17, 60]. As shown in the middle of
Fig. 3, this process can be formulated as follows:


**z** 0 = Embedding( **x** ) _,_ **z** 0 _∈_ R _[N]_ _[×][D]_ _,_ (1)
**z** _[′]_ _ℓ_ [= MHSA(LN(] **[z]** _[ℓ][−]_ [1][)) +] **[ z]** _[ℓ][−]_ [1] _[,]_ _ℓ_ = 1 _. . . L,_ (2)
**z** _ℓ_ = MLP(LN( **z** _[′]_ _ℓ_ [)) +] **[ z]** _[′]_ _ℓ_ _[,]_ _ℓ_ = 1 _. . . L,_ (3)


where _N, D, L_ denotes the number of input tokens, transformer dimension, and the number of layers, respectively.
**Proposed** **adapters.** To adapt the original Transformer
layers in CLIP to different fashion downstream tasks, we
design two kinds of adapters in architecture design: **(1)**
Task-Specific Adapter (TSA) for accommodating the nonshareable characteristics of each individual task (Fig. 3 left);



2671


**(2)** Cross-Attention Adapter (XAA) for enabling the interaction between different modalities (Fig. 3 right).
For TSA we adopt the scaled parallel adapter [5, 25] that
adds another bottleneck MLP (AdaptMLP) in parallel with
the original MLP of each transformer layer. Given an immediate input **z** _[′]_ _ℓ_ [, it produces the adapted features,] **[ z]** _[tsa]_ _ℓ_, via:

**z** _[tsa]_ _ℓ_ = _s ·_ AdaptMLP(LN( **z** _[′]_ _ℓ_ [))] _[,]_ (4)


where _s_ represents a learnable scale.
We construct an XAA module by further adding another
Multi-Head Cross Attention (MHXA) layer [18, 49, 72] at
the bottom of a TSA. Specifically, this MHXA uses the hidden state of the current stream **z** _[′]_ _ℓ_ [as the queries and the out-]
put **y** _ℓ_ ( _e.g_ ., hidden state after MHSA or MLP) of another
stream as the keys and values. The attended cross-modality
features **z** _[xaa]_ _ℓ_ are computed via:

**z** _[xaa]_ _ℓ_ = _s ·_ AdaptMLP(LN(MHXA( **z** _[′]_ _ℓ_ _[,]_ **[ y]** _[ℓ]_ [)))] _[.]_ (5)


Within this mechanism, our XAA can exchange the information between different modalities.
Finally, both **z** _[tsa]_ _ℓ_ and **z** _[xaa]_ _ℓ_ are added up to the original
output via residual connections. We thus rewrite Eq. (3) as:

**z** _ℓ_ = MLP(LN( **z** _[′]_ _ℓ_ [))+] **[z]** _[′]_ _ℓ_ [+] **[z]** _[tsa]_ _ℓ_ + _ϵ_ _·_ **z** _[xaa]_ _ℓ_ _, ϵ ∈{_ 0 _,_ 1 _},_ (6)


where _ϵ_ represents a gate that can turn on/off XAA according to the task need.
**Operational** **modes** **and** **fashion** **tasks.** Our FAME-ViL
can switch among three operational modes to flexibly support various fashion tasks (see Fig. 4).
_⋄_ **Contrastive mode** : This mode supports _Cross-Modal_
_Retrieval_ (XMR) tasks, including both text-to-image and
image-to-text retrieval [24, 52]. All XAA modules are
turned off as no need for inter-modal interaction, whereas
only TSA modules are applied as in Fig. 4(a). During
training, given a batch of _B_ image-text pairs ( **I**, **T** ) =
_{_ ( _I_ 1 _, T_ 1) _, . . .,_ ( _IB, TB_ ) _}_, we first compute their unimodal
representations by the TSA-equipped vision and language
encoders independently. Then, we maximize their similarities via symmetrical contrastive loss:

_L_ XMR = [1] (7)

2 [[] _[L]_ [InfoNCE][(] **[T]** _[,]_ **[ I]** [) +] _[ L]_ [InfoNCE][(] **[I]** _[,]_ **[ T]** [)]] _[,]_



_fθ_ [[] _[f]_ []][([] _[T]_ [;] _[ I]_ [])] [1][.] [This] [mode] [is] [useful] [for] [the] _[Sub-Category]_
_Recognition_ (SCR) [61, 94] and _Text-Guided_ _Image_ _Re-_
_trieval_ (TGIR) [75, 83].
SCR aims to predict the subcategory of fashion products
based on both input image and text. We thus append a classifier on top of the fused representation and minimize its
cross-entropy loss:

            -             _L_ SCR = _−_ E( _I,T_ ) _∼D_ log _P_ _fθ_ [[] _[f]_ []][(] _[I, T]_ [)] _._ (9)


Considering the unique challenges of TGIR ( _i.e_ ., requiring not only strong fusion but also efficient matching),
FAME-ViL operates in a hybrid mode for it. Formally,
given a batch of _{_ reference images **I** _[r]_, modifying text **T**,
target images **I** _[t]_ _}_, we first calculate the fused representation
_fθ_ [[] _[f]_ []][(] _[I]_ _[r][, T]_ [)] [in] [the] [fusion] [mode;] [Then,] [we] [obtain] [the] [target]
image representation _fθ_ [[] _[c]_ []][(] _[I]_ _[t]_ [)][ in the contrastive mode.] [Dur-]
ing training, we pull them closer in a contrastive way:

               _L_ TGIR = _L_ InfoNCE ( **I** _[r]_ _,_ **T** ) _,_ **I** _[t]_ [�] _,_ (10)


where the similarity is still measured by dot product:
_s_ (( _Ii_ _[r][, T][i]_ [)] _[, I]_ _j_ _[t]_ [) =] _[ f]_ [ [] _θ_ _[f]_ []][(] _[I]_ _i_ _[r][, T][i]_ [)] _[T]_ _[·][ f]_ [ [] _θ_ _[c]_ []][(] _[I]_ _j_ _[t]_ [)][.]
_⋄_ **Generative mode** : This works as a seq2seq model
performing the generative tasks auto-regressively [11, 71],
_e.g_ ., _Fashion Image Captioning_ (FIC) [85]. As in Fig. 4(c),
we use a TSA-equipped vision encoder as the encoder, a
TSA-equipped language encoder as the decoder, and only
image-to-text XAA modules for the conditional caption
synthesis. Following a standard encoder-decoder architecture, our image encoder provides _layer-wise_ latent memory
and the text decoder learns to maximize the conditional likelihood of the paired text under the forward auto-regressive
factorization [40, 86]:



_L_ FIC = _−_ E( _I,T_ ) _∼D_




- _A_ - 
log _P_ _Ta_ ��� _f_ [ _θg_ ] [(] _[I]_ [;] _[ T][<a]_ [)] _,_ (11)
_a_ =1



where _A_ denotes the length of each sentence, and
_fθ_ [[] _[g]_ []][([] _[I]_ [;] _[ T]_ [])][ denotes the representation.] [During training, we]
enforce the teacher forcing [81] to achieve parallel computation and thus maximize the learning efficiency.


**3.2. Heterogeneous multi-task learning**


For training _T_ fashion tasks, we need to optimize both
task-agnostic parameters _θs_ ( _i.e_ ., CLIP backbone & XAA
modules) and a set of task-specific components _{θt}_ _[T]_ _t_ =1
( _i.e_ ., TSA modules & task heads). Our objective is to maximize the overall performance across all tasks. The heterogeneity nature of fashion tasks causes the discrepancy in


1More complex fusion methods ( _e.g_ ., [2]) may yield better results. We
leave this for future study.



_L_ InfoNCE( **X** _,_ **Y** ) = _−_ _B_ [1]




- _B_ log ~~�~~ _B_ exp( _s_ ( _Xi, Yi_ ) _/τ_ ) _,_ (8)

_i_ =1 _j_ =1 [exp(] _[s]_ [(] _[X][i][, Y][j]_ [)] _[/τ]_ [)]



where _τ_ is a learnable temperature. The similarity is measured by the dot product of their pooled then normalized
features: _s_ ( _Ii, Tj_ ) = _fθ_ [[] _[c]_ []][(] _[I][i]_ [)] _[T]_ _[·][ f]_ [ [] _θ_ _[c]_ []][(] _[T][j]_ [)] _[.]_
_⋄_ **Fusion** **mode** : As in Fig. 4(b), both XAA and TSA
modules are enabled in this mode. Given an input imagetext pair ( _I, T_ ), the model serves as a single-stream fusion encoder producing two cross-modal attended representations: _fθ_ [[] _[f]_ []][([] _[I]_ [;] _[ T]_ [])][ and] _[ f]_ [ [] _θ_ _[f]_ []][([] _[T]_ [;] _[ I]_ [])][.] [The final fused repre-]
sentation is a simple addition: _fθ_ [[] _[f]_ []][(] _[I, T]_ [)] [=] _[f]_ [ [] _θ_ _[f]_ []][([] _[I]_ [;] _[ T]_ []) +]



2672


**Vision Input** **Language Input**













|Head<br>L灅<br>TSA MLP<br>MHSA|Col2|Col3|
|---|---|---|
|**TSA**<br>**MHSA**<br>**MLP**<br>**Head**<br>**L灅**|**TSA**<br>**MHSA**<br>**MLP**<br>**Head**<br>**L灅**||
|**TSA**<br>**MHSA**<br>**MLP**<br>**Head**<br>**L灅**|**M**|**LP**|
|**TSA**<br>**MHSA**<br>**MLP**<br>**Head**<br>**L灅**|**M**||
|**TSA**<br>**MHSA**<br>**MLP**<br>**Head**<br>**L灅**|**M**|**SA**|
|**TSA**<br>**MHSA**<br>**MLP**<br>**Head**<br>**L灅**|**M**||


**Vision Input**



|Head<br>L灅<br>MLP TSA<br>MHSA|Col2|
|---|---|
|||
|**M**|**LP**|
|||
|**MH**|**MH**|
|||


**Language Input**



**Vision Input** **Language Output (shifted right)**



**(a) Contrastive Mode** **(b) Fusion Mode** **(c) Generative Mode**


Figure 4. Schematic overview of three operational modes with our FAME-ViL. XAA: Cross-Attention Adapter; TSA: Task-Specific
Adapter. Layer normalization and original residual connections are not shown here for simplicity.



mini-batch construction and training dynamics ( _e.g_ ., converging speed, overfitting) as well as data imbalance, making our multi-task learning particularly challenging. To address all these issues, we exploit the idea of Multi-Teacher
Distillation (MTD) [12, 27].
Specifically, MTD consists of two stages. In the _**first**_
_**stage**_, we train a teacher model with the identical architecture as our multi-task model on every task. Then in the
_**second**_ _**stage**_, we apply these teachers to guide the training of the multi-task model ( _i.e_ ., the student) with designed
per-task distillation objectives.
For XMR, we first compute the image-text similarity using the features of the single-task teacher _g_ xmr: _s_ ˜( _Ii, Tj_ ) =
_g_ xmr( _Ii_ ) _[T]_ _·_ _g_ xmr( _Tj_ ) _._ Using this similarity as a pseudotarget, we maximize its mutual information with the student’s counterpart [41]:



1
_L_ [D] XMR [=]
2 _B_




- _B_

(KL ( **s** _b,·_ _∥_ ˜ **s** _b,·_ ) + KL ( **s** _·,b_ _∥_ ˜ **s** _·,b_ )) _,_ (12)

_b_



where KL( _·_ _∥·_ ) denotes the Kullback–Leibler divergence
loss on the softmax of the inputs.
For TGIR, we use a similar method as XMR to distill the
knowledge from single-task teacher _g_ tgir:



where _P_ denotes the sampling probability. To tackle data
imbalance, unless stated otherwise we set the sampling
probability for a particular task _τ_ linearly proportional to
the size of its dataset _|Dτ_ _|_ [30, 62]. We name this strategy
as _**size-proportional sampling**_ .


**4. Experiments**


**Datasets.** We evaluate our model on the datasets commonly
used in the previous methods. Specifically, we test FashionGen [61] for XMR, SCR, and FIC, and FashionIQ [83]
for TGIR. FashionGen [61] contains 68 _k_ fashion products
accompanied by text descriptions. Each product includes
1 _∼_ 6 images from different angles, resulting in 260 _._ 5 _k_
image-text pairs for training, and 35 _._ 5 _k_ for testing. FashionIQ contains 18 _k_ training triplets ( _i.e_ ., reference image,
modifying text, target image) and 6 _k_ validation triplets over
three categories: Dress, Shirt, and Toptee. Each pair (reference image, target image) is manually annotated with two
modifying texts, which are concatenated [83].
**Implementation** **details.** We use MMF [65] and PyTorch

[59] to implement our FAME-ViL. We use the off-the-shelf
CLIP from HuggingFace [82] as our pre-trained model. We
use 4 RTX 3090 GPUs for the multi-task training. The default bottleneck dimension of AdaptMLP is 64. More implementation details are listed in the supplementary file.
**Performance** **metrics.** Following [24, 94], we report
R@K for retrieval, Accuracy & Macro _F_ 1 for classification and BLEU-4 [58] & METEOR [4] & ROUGE-L [45]
& CIDEr [74] for captioning. **(1)** For each task, we
first _|M_ 1 _|_ �report _|jM_ =0 _|_ _[M]_ _**the**_ _[T][i][,j]_ _**average**_ [.] **[(2)]** [Since] _**absolute**_ [there] _**performance**_ [is] [no] [unified] : [objective] _μTi_ =
among multiple tasks and the scale of per-task metrics
often varies largely, we then report _**the**_ _**average**_ _**per-task**_
_**relative**_ _**performance**_ Δ _Ti_ w.r.t. the single-task baseline:
Δ _Ti_ = ( _μTi_ _−_ _μ_ STL) _/μ_ STL _._ This can clearly indicate the
positive/negative transfer effect. **(3)** We also report _**the rela-**_
_**tive parameters saving**_ of FAME-ViL and its variants w.r.t.
the vanilla CLIP baseline. Note, inference speed comparison is infeasible as this depends on the application of different tasks (no fixed rules).



_L_ [D] TGIR [=] [1]

_B_




- _B_ - 
KL **s** ( _b,b_ ) _,·_ _∥_ ˜ **s** ( _b,b_ ) _,·_ _,_ (13)
_b_



where the soft target is calculated via: _s_ ˜(( _Ii_ _[r][, T][i]_ [)] _[, I]_ _j_ _[t]_ [)] [=]
_g_ tgir( _Ii_ _[r][, T][i]_ [)] _[T]_ _[·][ g]_ [tgir][(] _[I]_ _j_ _[t]_ [)][.]
For SCR and FIC, we directly use the classification probabilities predicted by the teachers as pseudo-targets:

        -        _L_ [D] SCR [= KL] _fθ_ [[] _[f]_ []][(] _[I, T]_ [)] _[ ∥]_ _[g]_ [scr][(] _[I, T]_ [)] _,_ (14)



_L_ [D] FIC [=]




- _A_ - 
KL _fθ_ [[] _[g]_ []][(] _[I]_ [;] _[ T][<a]_ [)] _[a]_ _[∥]_ _[g]_ [fic][(] _[I]_ [;] _[ T][<a]_ [)] _[a]_ _._ (15)
_a_ =1



**Task** **scheduling.** For training simplicity, we randomly
sample one task per iteration. We optimize the summation
of the original loss and distillation loss as:

_L_ = _L_ [task] + _L_ [D] [task] _[,]_ [task] _∼{P_ XMR _,_ TGIR _,_ SCR _,_ FIC _},_ (16)



2673


|Methods|Image to Text Text to Image<br>Mean<br>R@1 R@5 R@10 R@1 R@5 R@10|
|---|---|
|**FashionBERT** [21]<br>**OSCAR** [43]<br>**KaledioBERT** [94]<br>**EI-CLIP** [52]<br>**MVLT** [33]<br>**FashionViL** [24]<br>**FashionViL**(_vit_)|23.96<br>46.31<br>52.12<br>26.75<br>46.48<br>55.74<br>41.89<br>23.39<br>44.67<br>52.55<br>25.10<br>49.14<br>56.68<br>41.92<br>27.99<br>60.09<br>68.37<br>33.88<br>60.60<br>68.59<br>53.25<br>38.70<br>72.20<br>84.25<br>40.06<br>71.99<br>82.90<br>65.02<br>33.10<br>77.20<br>91.10<br>34.60<br>78.00<br>89.50<br>67.25<br>65.54<br>91.34<br>96.30<br>61.88<br>87.32<br>93.22<br>82.60<br>63.74<br>90.02<br>95.98<br>60.76<br>86.18<br>92.96<br>81.61|
|**FAME-ViL**(_ST_)<br>**FAME-ViL**|65.02<br>90.96<br>96.20<br>**63.56**<br>86.84<br>93.06<br>82.61<br>**65.94**<br>**91.92**<br>**97.22**<br>62.86<br>**87.38**<br>**93.52**<br>**83.14**|


Table 1. Cross-Modal Retrieval (XMR) results on FashionGen [61]. Test protocol: random 100 [21, 24, 94].


**4.1. Comparisons with prior art methods**


We compare our models with the previous state-of-theart methods on each task. For extensive and fair comparisons, all the prior competitors are based on large-scale
pre-trained models. We even implement an enhanced variant of the latest art model FashionViL [24] by replacing
ResNet50 [26] with the ViT-B/16 [17] backbone (same as
our FAME-ViL), denoted as FashionViL( _vit_ ). In design, all the previous methods adopt Single-Task Learning
(STL). We compare them with two variants of our model:
**(1)** single unified MTL model; **(2)** STL variant of our
FAME-ViL, denoted as FAME-ViL( _ST_ ), which is trained
on each task independently using the same TSA and XAA
design as FAME-ViL.
**XMR evaluation.** We consider both image-to-text retrieval
and text-to-image retrieval with two kinds of protocols used
by previous methods: **(1)** random 100: For each query,
100 candidates are randomly sampled from the same category to construct a retrieval database; The goal is to locate the positive match depicting the same garment instance
from these 100 same-category negative matches [21,33,94].
**(2)** full database: We also adopt a more challenging
and practical protocol that conducts retrieval on the entire
product set [24, 52], which is in line with actual product retrieval scenarios. We use random 100 to compare with
prior art methods while using full database to do ablation studies. The results of XMR on FashionGen [61] are
reported in Tab. 1. We draw several observations: **(1)** Our
FAME-ViL outperforms all prior art fashion models often
by a large margin, validating the performance advantages of
our method over alternatives in addition to better parameter
efficiency. **(2)** FAME-ViL is superior over its single-task
variant FAME-ViL( _ST_ ) in most cases and on the average
accuracy, suggesting that our multi-task learning strategy is
effective in exploiting the inter-task relatedness. **(3)** Our
FAME-ViL( _ST_ ) can surpass all prior models pre-trained on
large fashion domain data [21, 24, 43, 94], suggesting that
using fashion data in pre-training is not necessarily most important, and model design ( _e.g_ ., our TSA and XAA) could
play a more significant role. Similarly, its large margin over



|Methods|Dress Shirt Toptee<br>Mean<br>R@10 R@50 R@10 R@50 R@10 R@50|
|---|---|
|**CIRPLANT** [48]<br>**TIRG**(_bert_) [75]_†_<br>**FashionVLP** [22]<br>**FashionViL** [24]<br>**FashionViL**(_vit_)<br>**Baldrati****_ et al_.** [2]<br>**Zhao****_ et al_.** [90]|17.45<br>40.41<br>17.53<br>38.81<br>21.64<br>45.38<br>30.20<br>27.17<br>53.25<br>22.28<br>45.58<br>27.84<br>57.11<br>38.87<br>26.77<br>53.20<br>22.67<br>46.22<br>28.51<br>57.47<br>39.14<br>33.47<br>59.94<br>25.17<br>50.39<br>34.98<br>60.79<br>44.12<br>31.53<br>57.91<br>26.74<br>50.69<br>36.77<br>61.81<br>44.24<br>33.81<br>59.40<br>39.99<br>60.45<br>41.41<br>65.37<br>50.07<br>33.60<br>58.90<br>39.45<br>61.78<br>43.96<br>68.33<br>51.00|
|**FAME-ViL**(_ST_)<br>**FAME-ViL**|37.78<br>63.86<br>45.63<br>66.78<br>47.22<br>70.88<br>55.36<br>**42.19**<br>**67.38**<br>**47.64**<br>**68.79**<br>**50.69**<br>**73.07**<br>**58.29**|


Table 2. Text-Guided Image Retrieval (TGIR) results on FashionIQ [83]. _†_ : The results taken from [24].

|Methods|SCR<br>Acc F1 Mean|FIC<br>B M R C Mean|
|---|---|---|
|**FashionBERT** [21]_†_<br>**OSCAR** [43]_†_<br>**KaleidoBERT** [94]<br>**FashionViL** [24]<br>**MVLT** [33]<br>**FashionViL**(_vit_)|85.27<br>62.00<br>73.64<br>84.23<br>59.10<br>71.67<br>88.07<br>63.60<br>75.84<br>92.23<br>83.02<br>87.63<br>93.57<br>82.90<br>88.24<br>94.01<br>85.77<br>89.89|3.30<br>9.80<br>29.70<br>30.10<br>18.23<br>4.50<br>10.90<br>30.10<br>30.70<br>19.05<br>5.70<br>12.80<br>32.90<br>32.60<br>21.00<br>16.71<br>**25.97**<br>37.82<br>39.08<br>29.90<br>-<br>-<br>-<br>-<br>-<br>16.18<br>25.60<br>37.23<br>39.30<br>29.58|
|**FAME-ViL**(_ST_)<br>**FAME-ViL**|94.33<br>86.21<br>90.27<br>**94.67**<br>**88.21**<br>**91.44**|29.97<br>24.83<br>54.79<br>145.1<br>63.67<br>**30.73**<br>25.04<br>**55.83**<br>**150.4**<br>**65.50**|



Table 3. Results of Subcategory Recognition (SCR) and Fashion
Image Captioning (FIC) on FashionGen [61]. _†_ : copied from [94].


the previous pre-trained CLIP-based model [52] further validates the significance of model architecture design.
**TGIR** **evaluation.** We compare our FAME-ViL with
TGIR-specialist methods [2, 22, 48, 75, 90] and the art
fashion-focused V+L model FashionViL [24] under the
original protocol used by FashionIQ [83]. The results are
given in Tab. 2. We have similar observations as on XMR.
In particular, we note that our single-task variant already
achieve a new art performance. With a simple additionbased fusion mechanism, FAME-ViL can even outperform
significantly [2] with the same CLIP pre-training and a complex fusion module. We attribute this mostly to the contribution of XAA-backed inter-modal interaction (See Tab. 4).
**SCR** **evaluation.** We report the performance of SCR
in the left part of Tab. 3, following the common protocol [21, 24, 94]. Similar to TGIR, our FAME-ViL surpasses
clearly all previous works [21,24,33,43,94] with heavier fusion mechanisms ( _e.g_ ., modality-agnostic self-attention implemented by concatenating text tokens and image patches
at the very beginning). This validates the efficacy of our
proposed XAA, suggesting the superiority of modality interaction over the conventional fusion at the input point.
**FIC** **evaluation.** The original FashionViL [24] has no decoder and cannot support generation tasks. For comparison,
we equip it with masked language modelling (MLM) autoregressively [43,93,94] enabling the image captioning. The
results of FIC are shown in the right part of Tab. 3, following
the common protocol [94]. Our FAME-ViL again achieves
state-of-the-art performance with a clear margin.



2674


|Groups|Methods|#Params (%) T1: XMR T2: TGIR T3: SCR T4: FIC μ¯ Δ¯<br>μ Δ μ Δ μ Δ μ Δ|
|---|---|---|
|I<br>(Sec. 4.2)|(1)<br>**STL**<br>(2)<br>**STL + TSA**<br>(3)<br>**STL + XAA**<br>(4)<br>**STL + TSA + XAA (FAME-ViL**(_ST_))|**0.0**<br>66.30<br>0.0<br>51.87<br>0.0<br>**90.34**<br>**0.0**<br>-<br>-<br>52.13<br>0.0<br>+1.35<br>**69.99**<br>**+5.56**<br>52.59<br>+1.39<br>90.10<br>-0.27<br>-<br>-<br>53.25<br>+1.67<br>+14.70<br>66.30<br>0.0<br>53.83<br>+3.78<br>89.89<br>-0.50<br>**63.70**<br>**0.0**<br>68.43<br>+0.82<br>+15.96<br>**69.99**<br>**+5.56**<br>**55.47**<br>**+6.94**<br>90.27<br>-0.07<br>63.67<br>-0.05<br>**69.85**<br>**+3.10**|
|II<br>(Sec. 4.2)|(5)<br>**MTL**<br>(6)<br>**MTL + TSA**<br>(7)<br>**MTL + XAA**<br>(8)<br>**MTL + TSA + XAA (base MTL)**|**-70.43**<br>57.65<br>-13.05<br>49.57<br>-4.43<br>85.95<br>-4.86<br>-<br>-<br>48.29<br>-5.59<br>-70.11<br>67.97<br>+2.52<br>52.04<br>+0.33<br>90.32<br>-0.02<br>-<br>-<br>52.58<br>+0.71<br>-67.65<br>65.87<br>-0.65<br>52.59<br>+1.39<br>**90.93**<br>**+0.65**<br>60.99<br>-4.25<br>67.60<br>-0.72<br>-67.33<br>**69.31**<br>**+4.54**<br>**55.41**<br>**+6.82**<br>90.84<br>+0.55<br>**65.17**<br>**+2.31**<br>**70.18**<br>**+3.56**|
|III<br>(Sec. 4.3)|(9)<br>**base MTL + MTD (FAME-ViL)**<br>(10)<br>**base MTL + MTD + Uniform**<br>(11)<br>**base MTL + MTD + Round-robin**<br>(12)<br>**base MTL + IAS [32]**<br>(13)<br>**base MTL + MTD + IAS [32]**<br>(14)<br>**base MTL + IMTLG [46]**<br>(15)<br>**base MTL + MTD + IMTLG [46]**|-67.33<br>70.00<br>+5.56<br>**58.29**<br>**+12.38**<br>**91.44**<br>**+1.22**<br>65.50<br>+2.83<br>**71.31**<br>**+5.50**<br>-67.33<br>67.70<br>+2.11<br>57.31<br>+10.49<br>91.36<br>+1.13<br>65.12<br>+2.23<br>70.37<br>+3.99<br>-67.33<br>67.79<br>+2.25<br>57.47<br>+10.80<br>91.35<br>+1.12<br>64.87<br>+1.84<br>70.37<br>+4.00<br>-67.33<br>69.13<br>+4.27<br>55.26<br>+6.54<br>90.51<br>+0.19<br>63.67<br>-0.05<br>69.64<br>+2.74<br>-67.33<br>**70.11**<br>**+5.75**<br>57.97<br>+11.76<br>90.88<br>+0.60<br>**65.66**<br>**+3.08**<br>71.16<br>+5.30<br>-67.33<br>64.11<br>-3.30<br>47.12<br>-9.16<br>90.21<br>-0.14<br>55.61<br>-12.70<br>64.26<br>-6.33<br>-67.33<br>67.14<br>+1.27<br>57.22<br>+10.31<br>90.09<br>-0.28<br>58.14<br>-9.56<br>68.15<br>+0.44|
|IV<br>(Sec. 4.4)|(16)<br>**FAME-ViL (bottleneck dim. = 128)**<br>(17)<br>**FAME-ViL (bottleneck dim. = 256)**<br>(18)<br>**FAME-ViL (bottleneck dim. = 512)**|**-65.14**<br>70.73<br>+6.68<br>58.03<br>+11.88<br>**91.54**<br>**+1.33**<br>66.20<br>+3.92<br>71.63<br>+5.95<br>-62.67<br>71.77<br>+8.25<br>58.45<br>+12.69<br>91.10<br>+0.84<br>66.81<br>+4.88<br>72.03<br>+6.67<br>-57.73<br>**72.32**<br>**+9.08**<br>**58.51**<br>**+12.80**<br>90.96<br>+0.69<br>**66.92**<br>**+5.05**<br>**72.18**<br>**+6.91**|


Table 4. Ablation study and further analysis of our method. Groups (I) and (II): Ablation experiments of the proposed XAA
and TSA under the single-task learning (STL) and multi-task learning (MTL) scenarios. Group (III): The comparison among our
multi-teacher distillation (MTD) and other alternatives designed for task-sampling based MTL (TS-MTL). Group (IV): The effect of
the bottleneck dimension of XAA and TSA. Yellow background: The baseline performance used per column; Red background: negative
transfer; Green background: positive transfer. **Bold number** : The best result in each group.



All the above comparisons show the superior generalization capability of our method in both generative and discriminative tasks.


**4.2. Ablation study on architecture**


Given the strong performance of our method as evaluated
in Sec. 4.1, we ablate the proposed model architecture with
a focus on two newly introduced adapters (TSA and XAA)
in both STL and MTL settings.
**Single-task** **learning** **setting.** For comparison on XMR,
TGIR and SCR, we design the baseline as directly finetuning the vanilla CLIP without any new modules (L1).
With the two-stream design, CLIP cannot tackle FIC, and
we hence further equip it with our XAA as the baseline
for FIC (L3). From the results shown in group (I) of Tab. 4,
we find that TSA and XAA can bring in 3%-6% relative improvements for XMR and TGIR. In particular, XAA gives
TGIR a significant improvement, demonstrating the superiority of our layer-wise modality interaction mechanism.
However, these adapters have only a marginal impact on
the performance of SCR and FIC, with a performance drop
of less than 0.5% when the model is independently trained
on a single task.
**Multi-task** **learning** **setting.** Similarly, we construct the
baselines for the MTL setting using the vanilla CLIP
and XAA-equipped CLIP (L5 and L7). As shown in L5 in
the group (II) of Tab. 4, a severe negative transfer occurs
with an overall 5.59% performance drop. Likewise, there is
also a negative transfer for the XAA-equipped CLIP model
(L7), albeit with a slight increase in performance. This sug


gests the challenges of heterogeneous multi-task learning
in the fashion domain. This problem can be well solved
using our TSA, with an overall 4% _∼_ 6% improvement (L5
_vs_ . L7 and L6 _vs_ . L8), even though only a few extra taskspecific parameters are introduced (1.35% of the original
CLIP size). Interestingly, we also found that XAA and TSA
are reciprocal: **(1)** When TSA and XAA work together, the
model can achieve better relative performance than the sum
of their gains (L4 _vs_ . L2+L3 and L8 _vs_ . L6+L7) **(2)** When
TSA or XAA is applied in isolation, the multi-task model
always underperforms its single-task counterpart (L6 _vs_ . L2
and L7 _vs_ . L3). But the multi-task model with both TSA and
XAA exceeds the single-task counterpart (L8 vs L4), indicating that TSA and XAA play complementary roles better
in the multi-task setting, as expected and designed so.


**4.3. Ablation study on multi-task training strategy**


Following the above architecture analysis, we further ablate the proposed multi-teacher distillation (MTD) based
training strategy. We compare extensively with previous
sampling strategies and gradient manipulation algorithms.
**Task sampling.** We start by comparing two common sampling strategies (uniform and round-robin) with our
size-proportional strategy. Round-robin sampling
is a special case of uniform sampling – each task is sampled
one by one. As shown in L9-L11 in the group (III) of Tab. 4,
both uniform and round-robin sampling underperform our
size-proportional sampling by a gap of 1.5%. This is due
to imbalanced dataset sizes across different tasks, which is
ignored in uniform sampling and round-robin sampling.



2675


**Gradient** **manipulation.** To compare with our MTD
scheme, we consider two kinds of gradient manipulation algorithms: Implicit Adaptive Scheduling
(IAS) [32] and IMTLG [46]. In particular, IAS is a representative strong method that adaptively changes the task
sampling ratio, learning rate, or gradient scale for each
task [51,80]. Specifically, it scales the gradients of each task
according to the performance on the validation set. Instead,
IMTLG is a representative of those methods manipulating
all the gradients together [8, 36, 56, 63]. It is featured by a
closed-form solution to optimize the scaling factors of each
task, such that the aggregated gradients (sum of raw gradients weighted by the scaling factors) have equal projections
onto individual tasks. Since IMTLG cannot be directly applied to task-sampling based MTL, we further adapt it by
maintaining a gradient buffer to store the gradients of each
task and update the parameters every four iterations (each
corresponding one of the four fashion tasks). As shown in
the group (III) of Tab. 4, the performance of IAS (L12) and
IMTLG (L14) are significantly lower than that of our MTD
(L9). In particular, IMTLG suffers from a severe negative
transfer (-6.33%). There are two plausible reasons: **(1)** Relying on a heuristic strategy, IAS struggles in finding the
optimal status over all tasks, despite the access to the validation performance. **(2)** IMTLG may experience over-fitting
for the tasks with smaller training data ( _e.g_ ., TGIR), which
cannot be addressed by the idea of ensuring the final gradient direction to have the same impact on each task. On
the contrary, our MTD can implicitly regularize the gradients via knowledge distillation, without a costly need of
monitoring the validation performance. Guided by the soft
ground truth of each teacher, overfitting can be well avoided
in an elegant manner. Considering the methodical orthogonality, we further apply our MTD on top of IAS (L13) and
IMTLG (L15). It is shown that this can improve both by a
large margin (L13 _vs_ . L12 and L15 _vs_ . L14), demonstrating
the generic usability of our training method.


**4.4. Further analysis**


**Regularizing** **effect** **of** **MTD.** To shed more light on the
regularization effect of MTD, we plot the validation performance curves in Fig. 5. Without MTD, the baseline MTL
model is prone to overfit on TGIR after about 20 _k_ iterations
due to much less training data than other tasks. Interestingly, this overfitting is even amplified by IMTLG. This is
because IMTLG needs to pay more attention to TIRG in order to achieve impartial learning. Overall, neither IAS nor
IMTLG can improve over the baseline MTL, regardless of
overfitting or not. Encouragingly, our MTD yields consistent and significant performance boost per task, rendering it
a more stable and effective learning strategy.
**Scaling** **up** **bottleneck** **dimension.** We evaluate the effect of the bottleneck dimension of the AdaptMLP in XAA



Figure 5. Training dynamics of our multi-teacher distillation
(MTD) and alternative multi-task learning methods (IAS [32] and
IMTLG [46]). Metric: The validation performance curves.


and TSA (the only hyper-parameter of our architecture).
We vary this dimension from 64 to 512. As shown in the
group (IV) of Tab. 4, it is evident that the overall relative
performance is positively correlated with this bottleneck dimension. This indicates that FAME-ViL could be potentially more performing at the cost of more parameters. Also,
we observe a trade-off between model size increase and
performance gain. For example, 10% more parameters are
needed for exchanging a relative performance gain of 1.4%
(L18 _vs_ . L9). We also notice that further improvement is
not consistent over tasks. For instance, the performance of
SCR will gradually deteriorate with the increase of bottleneck dimension. An interesting direction for future works
could be exploiting adaptive algorithms [68,69] to optimize
the best bottleneck dimension per task.
**Qualitative examples.** To reflect the output of FAME-ViL
more intuitively, in addition to Fig. 2, we show more illustrative outputs from FAME-ViL in the supplementary file.


**5. Conclusions**


We have introduced FAME-ViL for heterogeneous fashion tasks, grounded upon a generic off-the-shelf V+L
model. It addresses cross-modal retrieval, text-guided image retrieval, multi-modal classification, and image captioning in a unified architecture. This is made possible by
the proposed task-versatile architecture with cross-attention
adapters and task-specific adapters, and a scalable multitask training pipeline with multi-teacher distillation. Extensive experiments showed that our FAME-ViL achieves new
state-of-the-art performance on all tasks with significantly
fewer parameters.



2676


**References**


[1] Jimmy Lei Ba, Jamie Ryan Kiros, and Geoffrey E Hinton. Layer normalization. _arXiv preprint arXiv:1607.06450_,
2016. 3

[2] Alberto Baldrati, Marco Bertini, Tiberio Uricchio, and Alberto Del Bimbo. Conditioned and composed image retrieval
combining and partially fine-tuning clip-based features. In
_CVPR workshops_, 2022. 2, 3, 4, 6

[3] Alberto Baldrati, Marco Bertini, Tiberio Uricchio, and Alberto Del Bimbo. Effective conditioned and composed image retrieval combining clip-based features. In _CVPR_, 2022.
3

[4] Satanjeev Banerjee and Alon Lavie. Meteor: An automatic
metric for mt evaluation with improved correlation with human judgments. In _ACL workshops_, 2005. 5

[5] Shoufa Chen, Chongjian Ge, Zhan Tong, Jiangliu Wang,
Yibing Song, Jue Wang, and Ping Luo. Adaptformer: Adapting vision transformers for scalable visual recognition. In
_NeurIPS_, 2022. 3, 4

[6] Yanbei Chen, Shaogang Gong, and Loris Bazzani. Image
search with text feedback by visiolinguistic attention learning. In _CVPR_, 2020. 2

[7] Yen-Chun Chen, Linjie Li, Licheng Yu, Ahmed El Kholy,
Faisal Ahmed, Zhe Gan, Yu Cheng, and Jingjing Liu. Uniter:
Universal image-text representation learning. In _ECCV_,
2020. 1, 2, 3

[8] Zhao Chen, Vijay Badrinarayanan, Chen-Yu Lee, and Andrew Rabinovich. Gradnorm: Gradient normalization for
adaptive loss balancing in deep multitask networks. In _ICML_,
2018. 2, 3, 8

[9] Zhe Chen, Yuchen Duan, Wenhai Wang, Junjun He, Tong
Lu, Jifeng Dai, and Yu Qiao. Vision transformer adapter for
dense predictions. _arXiv_ _preprint_ _arXiv:2205.08534_, 2022.
3

[10] Patrick John Chia, Giuseppe Attanasio, Federico Bianchi,
Silvia Terragni, Ana Rita Magalh˜aes, Diogo Goncalves, Ciro
Greco, and Jacopo Tagliabue. Fashionclip: Connecting language and images for product representations. _arXiv preprint_
_arXiv:2204.03972_, 2022. 3

[11] Kyunghyun Cho, Bart Van Merri¨enboer, Caglar Gulcehre,
Dzmitry Bahdanau, Fethi Bougares, Holger Schwenk, and
Yoshua Bengio. Learning phrase representations using
rnn encoder-decoder for statistical machine translation. In
_EMNLP_, 2014. 4

[12] Kevin Clark, Minh-Thang Luong, Urvashi Khandelwal,
Christopher D Manning, and Quoc Le. Bam! born-again
multi-task networks for natural language understanding. In
_ACL_, 2019. 2, 3, 5

[13] Michael Crawshaw. Multi-task learning with deep neural
networks: A survey. _arXiv preprint arXiv:2009.09796_, 2020.
2

[14] Ginger Delmas, Rafael S Rezende, Gabriela Csurka, and Diane Larlus. Artemis: Attention-based retrieval with textexplicit matching and implicit similarity. In _ICLR_, 2022. 2

[15] Jacob Devlin, Ming-Wei Chang, Kenton Lee, and Kristina
Toutanova. BERT: Pre-training of deep bidirectional trans


formers for language understanding. In _NAACL-HLT_, 2019.
2

[16] Eric Dodds, Jack Culpepper, and Gaurav Srivastava. Training and challenging models for text-guided fashion image
retrieval. _arXiv preprint arXiv:2204.11004_, 2022. 3

[17] Alexey Dosovitskiy, Lucas Beyer, Alexander Kolesnikov,
Dirk Weissenborn, Xiaohua Zhai, Thomas Unterthiner,
Mostafa Dehghani, Matthias Minderer, Georg Heigold, Sylvain Gelly, et al. An image is worth 16x16 words: Transformers for image recognition at scale. In _ICLR_, 2020. 2, 3,
6

[18] Zi-Yi Dou, Aishwarya Kamath, Zhe Gan, Pengchuan Zhang,
Jianfeng Wang, Linjie Li, Zicheng Liu, Ce Liu, Yann LeCun, Nanyun Peng, et al. Coarse-to-fine vision-language
pre-training with fusion in the backbone. In _NeurIPS_, 2022.
2, 4

[19] Nanyi Fei, Zhiwu Lu, Yizhao Gao, Guoxing Yang, Yuqi Huo,
Jingyuan Wen, Haoyu Lu, Ruihua Song, Xin Gao, Tao Xiang, et al. Towards artificial general intelligence via a multimodal foundation model. _Nature_ _Communications_, 2022.
2

[20] Robert M French. Catastrophic forgetting in connectionist
networks. _Trends in cognitive sciences_, 1999. 3

[21] Dehong Gao, Linbo Jin, Ben Chen, Minghui Qiu, Peng Li,
Yi Wei, Yi Hu, and Hao Wang. Fashionbert: Text and image matching with adaptive loss for cross-modal retrieval. In
_SIGIR_, 2020. 1, 3, 6

[22] Sonam Goenka, Zhaoheng Zheng, Ayush Jaiswal, Rakesh
Chada, Yue Wu, Varsha Hedau, and Pradeep Natarajan.
Fashionvlp: Vision language transformer for fashion retrieval with feedback. In _CVPR_, 2022. 3, 6

[23] Xiao Han, Sen He, Li Zhang, Yi-Zhe Song, and Tao Xiang.
Uigr: Unified interactive garment retrieval. In _CVPR work-_
_shops_, 2022. 2

[24] Xiao Han, Licheng Yu, Xiatian Zhu, Li Zhang, Yi-Zhe Song,
and Tao Xiang. Fashionvil: Fashion-focused vision-andlanguage representation learning. In _ECCV_, 2022. 1, 2, 3, 4,
5, 6

[25] Junxian He, Chunting Zhou, Xuezhe Ma, Taylor BergKirkpatrick, and Graham Neubig. Towards a unified view
of parameter-efficient transfer learning. In _ICLR_, 2022. 3, 4

[26] Kaiming He, Xiangyu Zhang, Shaoqing Ren, and Jian Sun.
Deep residual learning for image recognition. In _CVPR_,
2016. 6

[27] Geoffrey Hinton, Oriol Vinyals, Jeff Dean, et al. Distilling the knowledge in a neural network. _arXiv_ _preprint_
_arXiv:1503.02531_, 2015. 5

[28] Neil Houlsby, Andrei Giurgiu, Stanislaw Jastrzebski, Bruna
Morrone, Quentin De Laroussilhe, Andrea Gesmundo, Mona
Attariyan, and Sylvain Gelly. Parameter-efficient transfer
learning for nlp. In _ICML_, 2019. 3

[29] Edward J Hu, Yelong Shen, Phillip Wallis, Zeyuan AllenZhu, Yuanzhi Li, Shean Wang, Lu Wang, and Weizhu Chen.
Lora: Low-rank adaptation of large language models. _arXiv_
_preprint arXiv:2106.09685_, 2021. 3

[30] Ronghang Hu and Amanpreet Singh. Unit: Multimodal multitask learning with a unified transformer. In _ICCV_, 2021. 3,
5



2677


[31] Zhicheng Huang, Zhaoyang Zeng, Yupan Huang, Bei Liu,
Dongmei Fu, and Jianlong Fu. Seeing out of the box: End-toend pre-training for vision-language representation learning.
In _CVPR_, 2021. 2

[32] S´ebastien Jean, Orhan Firat, and Melvin Johnson. Adaptive scheduling for multi-task learning. _arXiv_ _preprint_
_arXiv:1909.06434_, 2019. 3, 7, 8

[33] Ge-Peng Ji, Mingchen Zhuge, Dehong Gao, Deng-Ping Fan,
Christos Sakaridis, and Luc Van Gool. Masked visionlanguage transformer in fashion. _Machine_ _Intelligence_ _Re-_
_search_, 2022. 1, 3, 6

[34] Chao Jia, Yinfei Yang, Ye Xia, Yi-Ting Chen, Zarana Parekh,
Hieu Pham, Quoc Le, Yun-Hsuan Sung, Zhen Li, and Tom
Duerig. Scaling up visual and vision-language representation
learning with noisy text supervision. In _ICML_, 2021. 2

[35] Menglin Jia, Luming Tang, Bor-Chun Chen, Claire Cardie,
Serge Belongie, Bharath Hariharan, and Ser-Nam Lim. Visual prompt tuning. In _ECCV_, 2022. 3

[36] Alex Kendall, Yarin Gal, and Roberto Cipolla. Multi-task
learning using uncertainty to weigh losses for scene geometry and semantics. In _CVPR_, 2018. 2, 3, 8

[37] Jongseok Kim, Youngjae Yu, Hoeseong Kim, and Gunhee
Kim. Dual compositional learning in interactive image retrieval. In _AAAI_, 2021. 2

[38] Wonjae Kim, Bokyung Son, and Ildoo Kim. Vilt: Visionand-language transformer without convolution or region supervision. In _ICML_, 2021. 1, 2

[39] Seungmin Lee, Dongwan Kim, and Bohyung Han. Cosmo:
Content-style modulation for image retrieval with text feedback. In _CVPR_, 2021. 2

[40] Junnan Li, Dongxu Li, Caiming Xiong, and Steven Hoi.
Blip: Bootstrapping language-image pre-training for unified
vision-language understanding and generation. In _ICML_,
2022. 4

[41] Junnan Li, Ramprasaath Selvaraju, Akhilesh Gotmare,
Shafiq Joty, Caiming Xiong, and Steven Chu Hong Hoi.
Align before fuse: Vision and language representation learning with momentum distillation. In _NeurIPS_, 2021. 1, 2,
5

[42] Liunian Harold Li, Mark Yatskar, Da Yin, Cho-Jui Hsieh,
and Kai-Wei Chang. Visualbert: A simple and performant baseline for vision and language. _arXiv_ _preprint_
_arXiv:1908.03557_, 2019. 1, 2

[43] Xiujun Li, Xi Yin, Chunyuan Li, Pengchuan Zhang, Xiaowei
Hu, Lei Zhang, Lijuan Wang, Houdong Hu, Li Dong, Furu
Wei, et al. Oscar: Object-semantics aligned pre-training for
vision-language tasks. In _ECCV_, 2020. 1, 2, 6

[44] Lizi Liao, Xiangnan He, Bo Zhao, Chong-Wah Ngo, and TatSeng Chua. Interpretable multimodal retrieval for fashion
products. In _ACM MM_, 2018. 1

[45] Chin-Yew Lin. Rouge: A package for automatic evaluation
of summaries. In _ACL workshops_, 2004. 5

[46] Liyang Liu, Yi Li, Zhanghui Kuang, Jing-Hao Xue, Yimin
Chen, Wenming Yang, Qingmin Liao, and Wayne Zhang.
Towards impartial multi-task learning. In _ICLR_, 2020. 2,
3, 7, 8




[47] Pengfei Liu, Weizhe Yuan, Jinlan Fu, Zhengbao Jiang, Hiroaki Hayashi, and Graham Neubig. Pre-train, prompt, and
predict: A systematic survey of prompting methods in natural language processing. _arXiv preprint arXiv:2107.13586_,
2021. 3

[48] Zheyuan Liu, Cristian Rodriguez-Opazo, Damien Teney, and
Stephen Gould. Image retrieval on real-life images with pretrained vision-and-language models. In _ICCV_, 2021. 1, 6

[49] Jiasen Lu, Dhruv Batra, Devi Parikh, and Stefan Lee. Vilbert:
Pretraining task-agnostic visiolinguistic representations for
vision-and-language tasks. In _NeurIPS_, 2019. 1, 2, 4

[50] Jiasen Lu, Christopher Clark, Rowan Zellers, Roozbeh Mottaghi, and Aniruddha Kembhavi. Unified-io: A unified
model for vision, language, and multi-modal tasks. _arXiv_
_preprint arXiv:2206.08916_, 2022. 2

[51] Jiasen Lu, Vedanuj Goswami, Marcus Rohrbach, Devi
Parikh, and Stefan Lee. 12-in-1: Multi-task vision and language representation learning. In _CVPR_, 2020. 3, 8

[52] Haoyu Ma, Handong Zhao, Zhe Lin, Ajinkya Kale,
Zhangyang Wang, Tong Yu, Jiuxiang Gu, Sunav Choudhary,
and Xiaohui Xie. Ei-clip: Entity-aware interventional contrastive learning for e-commerce cross-modal retrieval. In
_CVPR_, 2022. 2, 3, 4, 6

[53] Yihui Ma, Jia Jia, Suping Zhou, Jingtian Fu, Yejun Liu, and
Zijian Tong. Towards better understanding the clothing fashion styles: A multimodal deep learning approach. In _AAAI_,
2017. 1

[54] Yuning Mao, Lambert Mathias, Rui Hou, Amjad Almahairi, Hao Ma, Jiawei Han, Scott Yih, and Madian Khabsa.
Unipelt: A unified framework for parameter-efficient language model tuning. In _ACL_, 2022. 3

[55] Suvir Mirchandani, Licheng Yu, Mengjiao Wang, Animesh
Sinha, Wenwen Jiang, Tao Xiang, and Ning Zhang. Fad-vlp:
Fashion vision-and-language pre-training towards unified retrieval and captioning. In _EMNLP_, 2022. 3

[56] Aviv Navon, Aviv Shamsian, Idan Achituve, Haggai Maron,
Kenji Kawaguchi, Gal Chechik, and Ethan Fetaya. Multitask learning as a bargaining game. In _ICML_, 2022. 2, 3,
8

[57] Duy-Kien Nguyen and Takayuki Okatani. Multi-task learning of hierarchical vision-language representation. In _CVPR_,
2019. 3

[58] Kishore Papineni, Salim Roukos, Todd Ward, and Wei-Jing
Zhu. Bleu: a method for automatic evaluation of machine
translation. In _ACL_, 2002. 5

[59] Adam Paszke, Sam Gross, Francisco Massa, Adam Lerer,
James Bradbury, Gregory Chanan, Trevor Killeen, Zeming
Lin, Natalia Gimelshein, Luca Antiga, et al. Pytorch: An
imperative style, high-performance deep learning library. In
_NeurIPS_, 2019. 5

[60] Alec Radford, Jong Wook Kim, Chris Hallacy, Aditya
Ramesh, Gabriel Goh, Sandhini Agarwal, Girish Sastry,
Amanda Askell, Pamela Mishkin, Jack Clark, et al. Learning transferable visual models from natural language supervision. In _ICML_, 2021. 1, 2, 3

[61] Negar Rostamzadeh, Seyedarian Hosseini, Thomas Boquet,
Wojciech Stokowiec, Ying Zhang, Christian Jauvin, and



2678


Chris Pal. Fashion-gen: The generative fashion dataset and
challenge. _arXiv preprint arXiv:1806.08317_, 2018. 1, 2, 3,
4, 5, 6

[62] Victor Sanh, Thomas Wolf, and Sebastian Ruder. A hierarchical multi-task approach for learning embeddings from
semantic tasks. In _AAAI_, 2019. 5

[63] Ozan Sener and Vladlen Koltun. Multi-task learning as
multi-objective optimization. In _NeurIPS_, 2018. 2, 3, 8

[64] Minchul Shin, Yoonjae Cho, Byungsoo Ko, and Geonmo
Gu. Rtic: Residual learning for text and image composition using graph convolutional network. _arXiv_ _preprint_
_arXiv:2104.03015_, 2021. 2

[65] Amanpreet Singh, Vedanuj Goswami, Vivek Natarajan, Yu
Jiang, Xinlei Chen, Meet Shah, Marcus Rohrbach, Dhruv
Batra, and Devi Parikh. Mmf: A multimodal framework for
vision and language research. https://github.com/
facebookresearch/mmf, 2020. 5

[66] Amanpreet Singh, Ronghang Hu, Vedanuj Goswami, Guillaume Couairon, Wojciech Galuba, Marcus Rohrbach, and
Douwe Kiela. Flava: A foundational language and vision
alignment model. In _CVPR_, 2022. 2, 3

[67] Weijie Su, Xizhou Zhu, Yue Cao, Bin Li, Lewei Lu, Furu
Wei, and Jifeng Dai. VL-BERT: pre-training of generic
visual-linguistic representations. In _ICLR_, 2020. 1, 2

[68] Benyuan Sun, Jin Dai, Zihao Liang, Congying Liu, Yi
Yang, and Bo Bai. Gppf: A general perception pre-training
framework via sparsely activated multi-task learning. _arXiv_
_preprint arXiv:2208.02148_, 2022. 8

[69] Ximeng Sun, Rameswar Panda, Rogerio Feris, and Kate
Saenko. Adashare: Learning what to share for efficient deep
multi-task learning. In _NeurIPS_, 2020. 8

[70] Yi-Lin Sung, Jaemin Cho, and Mohit Bansal. Vl-adapter:
Parameter-efficient transfer learning for vision-and-language
tasks. In _CVPR_, 2022. 3

[71] Ilya Sutskever, Oriol Vinyals, and Quoc V Le. Sequence to
sequence learning with neural networks. In _NeurIPS_, 2014.
4

[72] Hao Tan and Mohit Bansal. LXMERT: Learning crossmodality encoder representations from transformers. In
_EMNLP-IJCNLP_, 2019. 1, 4

[73] Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N. Gomez, Lukasz Kaiser, and Illia
Polosukhin. Attention is all you need. In _NeurIPS_, 2017. 2,
3

[74] Ramakrishna Vedantam, C Lawrence Zitnick, and Devi
Parikh. Cider: Consensus-based image description evaluation. In _CVPR_, 2015. 5

[75] Nam Vo, Lu Jiang, Chen Sun, Kevin Murphy, Li-Jia Li, Li
Fei-Fei, and James Hays. Composing Text and Image for
Image Retrieval - an Empirical Odyssey. In _CVPR_, 2019. 2,
4, 6

[76] Jianfeng Wang, Xiaowei Hu, Zhe Gan, Zhengyuan Yang,
Xiyang Dai, Zicheng Liu, Yumao Lu, and Lijuan Wang.
Ufo: A unified transformer for vision-language representation learning. _arXiv preprint arXiv:2111.10023_, 2021. 2

[77] Peng Wang, An Yang, Rui Men, Junyang Lin, Shuai Bai,
Zhikang Li, Jianxin Ma, Chang Zhou, Jingren Zhou, and



Hongxia Yang. Ofa: Unifying architectures, tasks, and
modalities through a simple sequence-to-sequence learning
framework. In _ICML_, 2022. 2

[78] Wenhui Wang, Hangbo Bao, Li Dong, Johan Bjorck, Zhiliang Peng, Qiang Liu, Kriti Aggarwal, Owais Khan Mohammed, Saksham Singhal, Subhojit Som, et al. Image as a
foreign language: Beit pretraining for all vision and visionlanguage tasks. _arXiv preprint arXiv:2208.10442_, 2022. 2

[79] Wenhui Wang, Hangbo Bao, Li Dong, and Furu Wei.
Vlmo: Unified vision-language pre-training with mixture-ofmodality-experts. _arXiv preprint arXiv:2111.02358_, 2021. 1,
2

[80] Weiyao Wang, Du Tran, and Matt Feiszli. What makes training multi-modal classification networks hard? In _CVPR_,
2020. 8

[81] Ronald J Williams and David Zipser. A learning algorithm
for continually running fully recurrent neural networks. _Neu-_
_ral computation_, 1989. 4

[82] Thomas Wolf, Lysandre Debut, Victor Sanh, Julien Chaumond, Clement Delangue, Anthony Moi, Pierric Cistac, Tim
Rault, R´emi Louf, Morgan Funtowicz, Joe Davison, Sam
Shleifer, Patrick von Platen, Clara Ma, Yacine Jernite, Julien
Plu, Canwen Xu, Teven Le Scao, Sylvain Gugger, Mariama
Drame, Quentin Lhoest, and Alexander M. Rush. Transformers: State-of-the-art natural language processing. In _EMNLP_,
2020. 5

[83] Hui Wu, Yupeng Gao, Xiaoxiao Guo, Ziad Al-Halah, Steven
Rennie, Kristen Grauman, and Rogerio Feris. Fashion iq: A
new dataset towards retrieving images by natural language
feedback. In _CVPR_, 2021. 1, 2, 4, 5, 6

[84] Xiao Xu, Chenfei Wu, Shachar Rosenman, Vasudev Lal,
and Nan Duan. Bridge-tower: Building bridges between
encoders in vision-language representation learning. _arXiv_
_preprint arXiv:2206.08657_, 2022. 2

[85] Xuewen Yang, Heming Zhang, Di Jin, Yingru Liu, Chi-Hao
Wu, Jianchao Tan, Dongliang Xie, Jue Wang, and Xin Wang.
Fashion captioning: Towards generating accurate descriptions with semantic rewards. In _ECCV_, 2020. 1, 2, 3, 4

[86] Jiahui Yu, Zirui Wang, Vijay Vasudevan, Legg Yeung, Mojtaba Seyedhosseini, and Yonghui Wu. Coca: Contrastive
captioners are image-text foundation models. _arXiv preprint_
_arXiv:2205.01917_, 2022. 2, 4

[87] Licheng Yu, Jun Chen, Animesh Sinha, Mengjiao MJ Wang,
Hugo Chen, Tamara L Berg, and Ning Zhang. Commercemm: Large-scale commerce multimodal representation learning with omni retrieval. In _KDD_, 2022. 1, 3

[88] Pengchuan Zhang, Xiujun Li, Xiaowei Hu, Jianwei Yang,
Lei Zhang, Lijuan Wang, Yejin Choi, and Jianfeng Gao.
Vinvl: Revisiting visual representations in vision-language
models. In _CVPR_, 2021. 2

[89] Yuanhan Zhang, Kaiyang Zhou, and Ziwei Liu. Neural
prompt search. _arXiv preprint arXiv:2206.04673_, 2022. 3

[90] Yida Zhao, Yuqing Song, and Qin Jin. Progressive learning
for image retrieval with hybrid-modality queries. In _SIGIR_,
2022. 6

[91] Kaiyang Zhou, Jingkang Yang, Chen Change Loy, and Ziwei Liu. Conditional prompt learning for vision-language
models. In _CVPR_, 2022. 3



2679


[92] Kaiyang Zhou, Jingkang Yang, Chen Change Loy, and Ziwei
Liu. Learning to prompt for vision-language models. _IJCV_,
2022. 3

[93] Luowei Zhou, Hamid Palangi, Lei Zhang, Houdong Hu, Jason Corso, and Jianfeng Gao. Unified vision-language pretraining for image captioning and vqa. In _AAAI_, 2020. 6

[94] Mingchen Zhuge, Dehong Gao, Deng-Ping Fan, Linbo Jin,
Ben Chen, Haoming Zhou, Minghui Qiu, and Ling Shao.
Kaleido-bert: Vision-language pre-training on fashion domain. In _CVPR_, 2021. 1, 2, 3, 4, 5, 6



2680


