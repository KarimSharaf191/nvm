This CVPR workshop paper is the Open Access version, provided by the Computer Vision Foundation.
Except for this watermark, it is identical to the accepted version;
the final published version of the proceedings is available on IEEE Xplore.

# **Dynamic Multimodal Fusion**


Zihui Xue Radu Marculescu
The University of Texas at Austin



**Abstract**


_Deep_ _multimodal_ _learning_ _has_ _achieved_ _great_ _progress_
_in_ _recent_ _years._ _However,_ _current_ _fusion_ _approaches_ _are_
_static_ _in_ _nature,_ _i.e.,_ _they_ _process_ _and_ _fuse_ _multimodal_ _in-_
_puts_ _with_ _identical_ _computation,_ _without_ _accounting_ _for_
_diverse_ _computational_ _demands_ _of_ _different_ _multimodal_
_data._ _In_ _this_ _work,_ _we_ _propose_ _dynamic_ _multimodal_ _fu-_
_sion (DynMM), a new approach that adaptively fuses mul-_
_timodal_ _data_ _and_ _generates_ _data-dependent_ _forward_ _paths_
_during inference._ _To this end, we propose a gating function_
_to_ _provide_ _modality-level_ _or_ _fusion-level_ _decisions_ _on-the-_
_fly based on multimodal features and a resource-aware loss_
_function_ _that_ _encourages_ _computational_ _efficiency._ _Results_
_on various multimodal tasks demonstrate the efficiency and_
_wide_ _applicability of_ _our_ _approach._ _For_ _instance,_ _DynMM_
_can_ _reduce_ _the_ _computation_ _costs_ _by_ _46.5%_ _with_ _only_ _a_
_negligible accuracy loss (CMU-MOSEI sentiment analysis)_
_and improve segmentation performance with over 21% sav-_
_ings_ _in_ _computation_ _(NYU_ _Depth_ _V2_ _semantic_ _segmenta-_
_tion)_ _when_ _compared_ _with_ _static_ _fusion_ _approaches._ _We_
_believe_ _our_ _approach_ _opens_ _a_ _new_ _direction_ _towards_ _dy-_
_namic_ _multimodal_ _network_ _design,_ _with_ _applications_ _to_ _a_
_wide range of multimodal tasks._ [1]


**1. Introduction**

Humans perceive the world in a multimodal way,
through vision, hearing, touch, taste, etc. Recent years have
witnessed great progress of deep learning approaches that
leverage data of multiple modalities. Consequently, multimodal fusion has boosted the performance of many classical problems, such as sentiment analysis [21, 38, 50], action
recognition [6, 36], or semantic segmentation [35, 45].

Despite these advances, how to best combine information characterized by multiple modalities remains a fundamental challenge in multimodal learning [2]. Various research efforts [14, 20, 25, 26, 29, 42, 43, 50] have been put
into designing new fusion paradigms that can effectively
fuse multimodal data. These approaches are generally task

1Our code is available at [https://github.com/zihuixue/](https://github.com/zihuixue/DynMM)
[DynMM.](https://github.com/zihuixue/DynMM)



Figure 1. Two examples in CMU-MOSEI [51] for emotion recognition. Figure (a) shows an “easy” multimodal instance as using
textual information is sufficient to predict emotions correctly (this
is a positive emotion). Figure (b) shows a “hard” example where
all three modalities are required to make correct predictions (this
is a negative emotion). While static multimodal fusion networks
process “hard” and “easy” inputs identically, we propose _dynamic_
_instance-wise inference_ that can achieve computational savings for
“easy” examples and preserve representation power for “hard” instances. For (a), DynMM only activates the text path and skips
paths corresponding to the other two modalities, thus leading to
computational efficiency.


and modality-specific and require manual design. Building
on the success of Neural Architecture Search (NAS), a few
recent works [33,39,49] have adopted NAS to find effective
fusion architectures automatically.

However, both manually-designed and NAS-based approaches process all the instances in a _single_ fusion architecture and lack adaptability to diverse multimodal data.
Namely, once the fusion network is trained, it performs
static inference on each piece of data, without accounting
for the inherent differences in characteristics of different
multimodal inputs. Thus, the computational efficiency, as
well as the representation power of a well-designed fusion
architecture may be limited by its static nature. As a motivating example, consider the two multimodal instances in


2575



Text


Vision


Audio


Text


Vision


Audio



_I just got finished watching_
_an excellent movie called_
_Mars needs moms_


(a)


_Um I wish we wished that it_
_would have been the guy._


(b)






Figure 1. As shown, it is relatively easy to classify emotions for the upper example: the text modality alone provides strong evidence for a positive emotion. On the other
hand, it is unlikely to correctly predict emotions for the
lower example based solely on the textual information since
this sentence is confusing. Audio and visual modalities can
provide important cues to a multimodal network to make
correct decisions. From this example, we can see that multimodal data enable a model to learn from the rich representations of “hard” inputs; it can also bring redundancy in
computations for the “easy” inputs.

Inspired by this observation, we propose _dynamic_ _mul-_
_timodal_ _fusion_ (DynMM), a new approach that _adaptively_
_fuses_ input data from multiple modalities. Compared with
a static multimodal architecture, DynMM enjoys the benefits of reduced computation, improved representation power
and robustness. More precisely, dynamic fusion leads to
computational savings for “easy” inputs that can be correctly predicted using only a subset of modalities or simple
fusion operations. For “hard” multimodal inputs, DynMM
can match the representation power of a static network by
relying on all modalities and complex fusion operations for
prediction. In addition, real-world multimodal data may
be noisy and contradictory [22]. In such cases, skipping
paths that involve noisy modalities for certain instances in
DynMM can reduce noise and boost performance.

Dynamic neural networks [11] have gained increasing attention over the past few years and enjoys a broad range of
applications, such as image recognition [5, 28, 44, 46], semantic segmentation [23, 41] and machine translation [37].
Motivated by the great success of dynamic inference for
unimodal networks, this paper aims at proposing multimodal fusion as a new application domain. To this end,
we draw inspiration from the natural redundancy of multimodal data, which provides a different angle from existing
work. To be specific, we propose _progressive_ _fusion_, both
at _modality level_ and at _fusion level_ . At modality level, we
train a gating network to select a subset of input modalities
(or all modalities) for predictions based on each input. At
fusion level, the gating network provides sample-wise decisions on which fusion operation to adopt and when to stop
fusion. On one hand, by allowing exits at the early fusion
stages for “easy” inputs, DynMM saves the computations
of executing the later fusion modules. On the other hand,
in terms of “hard” multimodal inputs, DynMM can turn all
fusion modules on for accurate predictions.

To verify the efficacy and generalizability of our approach, we conduct experiments on various popular multimodal tasks. DynMM strikes a good balance between
computational efficiency and learning performance. For instance, for RGB-D semantic segmentation tasks, DynMM
achieves a +0.7% mIoU improvement with over 21% reductions in multiply-add operations (MAdds) for the depth



encoder when compared against [35]. Moreover, we find
that DynMM yields better predictions than static fusion networks when the input modality is perturbed by noise; this
suggests possible use of DynMM to improve the multimodal robustness.


**2. Related Work**

**2.1. Dynamic Neural Networks**

Dynamic neural networks have demonstrated a great potential in classical computer vision problems, such as image classification [5, 28, 44, 46], object detection [7, 52],
or semantic segmentation [23, 41]. While popular deep
learning approaches perform inference in a static manner,
dynamic networks allow the network structure to adapt to
the input characteristics during inference. This flexibility
yields many benefits, including high efficiency, representation power and results interpretability [10,34,47]. Dynamic
network designs can be categorized into: (a) dynamic depth;
(b) dynamic width; (c) dynamic routing [11].

The idea of dynamic depth is to adjust the network depth
based on each sample. By providing early exits [4, 40] in
shallow layers, one can save computations by not activating deep layers for “easy” samples. For dynamic width, the
idea is to adapt the network width in a sample-wise manner.
To build a dynamic width network and achieve inference
efficiency, previous works have proposed to skip neurons
in fully-connected layers [3], skip branches in Mixture-ofExperts (MoE) [28, 37], or skip channels in Convolutional
Neural Networks (CNNs) [17]. To enable more flexibility,
recent works [5, 23] build SuperNets with multiple inference paths. Dynamic routing is thus performed inside the
SuperNet to generate data-dependent forward paths during
inference. Our proposed modality-level DynMM belongs to
the category of _dynamic width_ approaches; the fusion-level
DynMM can be seen as a _dynamic routing_ approach.


**2.2. Multimodal Learning**

Multimodal fusion networks have a clear advantage over
their unimodal counterparts in various applications, such as
sentiment analysis [21,38,50], action recognition [6,36], or
semantic segmentation [8, 35, 45]. However, how to effectively combine multimodal features to better exploit information remains a big challenge. Existing works either propose hand-crafted fusion designs based on domain knowledge [20, 25, 26, 29, 43, 50], or apply NAS to find good architectures automatically [33, 39, 49]. However, the scope
of these works is limited to static networks only.

There have been some early attempts in adopting dynamic neural networks for multimodal applications, such
as semantic segmentation [45], video recognition [9,
32], visual-inertial odometry [48] and medical classification [12]. Among them, CEN [45] dynamically exchanges


2576


channels between sub-networks of the RGB and depth
modality for performance improvement. Han _et al_ . [12] proposes to dynamically evaluate feature-level and modalitylevel informativeness of different samples for more trustworthy medical classification, yet the angle of computational efficiency brought by the dynamic neural networks is
overlooked. The work of Gao _et al_ . [9] and AdaMML [32]
are most relevant to our approach as they also adaptively
utilize modalities for efficient video recognition. However,
their methods are tailored for video data and action recognition. In this work, we aim to make the first step towards a
systematic and general formulation of dynamic multimodal
fusion that can suit various multimodal tasks.


**3. Method**

In this section, we present the key design contributions of our proposed dynamic multimodal fusion network (DynMM). First, we introduce new decision making
schemes that enable DynMM to generate data-dependent
forward paths during inference. Two levels of granularity are considered, _i.e._, modality-level (coarse level) and
fusion-level (fine level) decision making. Next, we propose
new training strategies for DynMM, which consist of (1) a
training objective that accounts for resource budgets, and
(2) optimization of a non-differentiable gating network.

**3.1. Modality-level Decision**

Assume that input data has _M_ modalities, denoted by
**x** = ( _x_ 1 _, x_ 2 _,_ _, xM_ ). Following the classical Mixture-of_· · ·_
Experts (MoE) [27] framework, we design a set of expert
networks as follows. Each expert specializes in a subset of
all _M_ modalities. If _M_ = 3, for example, we can have up
to 7 expert networks, denoted by _E_ 1( _x_ 1), _E_ 2( _x_ 2), _E_ 3( _x_ 3),
_E_ 4( _x_ 1 _, x_ 2), _E_ 5( _x_ 2 _, x_ 3), _E_ 6( _x_ 1 _, x_ 2), _E_ 7( _x_ 1 _, x_ 2 _, x_ 3). In real
applications, the candidate expert networks can be narrowed
down with domain expertise. For instance, depth images
can provide useful cues when combined with RGB images,
but often perform poorly by themselves in semantic segmentation. In such a case, we do not consider adopting an
expert network that only takes depth as input.

Let _B_ represent the number of expert networks that get
selected. We propose a _gating_ _network_, denoted by _G_ ( **x** ),
to decide which expert network should be activated. This
gating network takes multimodal inputs **x** to form a global
view and then produces a _B_ -dimensional sparse vector **g**
as output. The final output _y_ takes the form of: _y_ =
P _Bi_ =1 _[g][i][E][i]_ [(] **[x]** _[i]_ [)][,] [where] **[x]** _[i]_ [denotes] [the] [subset] [of] [modalities]

that the _i_ -th expert takes as input.

Different from conventional MoEs [27] where the output is a weighted summation of expert networks and every
branch is executed, in our formulation, the output of the gating network **g** is a one-hot encoding, _i.e._, only _one_ branch
is selected for each instance. Therefore, the computations



Figure 2. An illustration of modality-level DynMM, where input
data has two modalities, denoted by _x_ 1 and _x_ 2, and the output is
denoted by _y_ . We design a set of expert networks _{Ei}_ that specialize in different subsets of modalities and adopt a gating network _G_ ( **x** ) to generate data-dependent decisions on which expert
network to select.


required for other expert networks can be saved. Note that
since our expert network already covers a broad range of
modality combinations, we only select one branch (as opposed to say selecting top _K_ branches) during each forward
pass for maximum computational savings. Figure 2 provides an illustration of the proposed design with 2 modalities and 3 expert networks ( _i.e._, _M_ = 2 and _B_ = 3).

The design of the gating network _G_ ( **x** ) follows two general requirements: (1) it should be computationally cheap
to have a small overhead (2) it needs to be sufficiently expressive to make informative decisions on which expert to
select. Various gating networks have been proposed previously; they are usually tailored for specific tasks and
network architectures [11]. In the experiments, we consider different gating networks ( _i.e._, a multi-layer perceptron (MLP) gate, a transformer gate and a convolutional
gate) for three multimodal tasks and provide the detailed
description of our gating network architecture in Sec. 4.

One remaining problem is the training of gating network
_G_ ( **x** ). Due to the non-differentiability of the discrete decisions given by _G_ ( **x** ), the network can not be directly trained
with back-propagation. Thus, we propose reparameterization techniques and discuss them later in Sec. 3.4.

Finally, this gating network _G_ ( **x** ) is not restricted to taking input-level features; it can also take intermediate features per modality as inputs. Thus, modality-level DynMM
can be plugged into any part of a multimodal network and
achieve savings in computations after this gating network.

**3.2. Fusion-level Decision**

While the modality-level decisions directly impact the
computational efficiency, completely skipping computations of one modality will likely lead to a downgraded performance for some challenging tasks, _e.g._, semantic segmentation. Thus, we provide a finer-grain formulation of
DynMM with fusion-level decisions next.

We first present the design of a _fusion_ _cell_ . Assume input data has _M_ modalities, _i.e._, **x** = ( _x_ 1 _, x_ 2 _,_ _, xM_ ).
_· · ·_


2577










|Col1|Col2|Col3|Col4|Fusion|Col6|Col7|Col8|Fusion|Col10|Col11|Col12|Fusion|Col14|Col15|Col16|Fusion|Col18|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|||block 1||Fusion<br>Cell 1||block 2||Fusion<br>Cell 2||block 3||Fusion<br>Cell 3||block 4||Fusion<br>Cell 4||
|||block 1|block 1|block 1|block 1|block 2|block 2|block 2|block 2|block 3|block 3|block 3|block 3|block 4|block 4|block 4|block 4|








|Col1|Col2|
|---|---|
|||























Figure 3. (a) An illustration of fusion-level DynMM, where input data has two modalities, denoted by _x_ 1 and _x_ 2. We design a fusion cell
with a set of candidate operations _{Oi}_ and a gating network _G_ ( **x** ). _h_ represents output of the cell. (b) A dynamic multimodal architecture
with stacked fusion cells, where we interlace static feature extraction blocks (colored with green and yellow) with dynamic fusion cells.
Gating network _G_ ( **x** ) in four fusion cells are integrated as one global gating network _G_ [ˆ] ( **x** ) that outputs decisions for four cells at once.
(c) An example architecture when the gating network chooses _O_ 2 for the first 2 fusion cells and _O_ 1 for the last 2 cells. Consequently,
computations of fusion cell 3 & 4 and feature extraction cell 3 & 4 for _x_ 2 are saved.



Denote a set of fusion operations as _Oi_ . _Oi_ can be
_{_ _}_
implemented as any function to fuse multimodal features,
such as simple identity mapping ( _i.e._, _Oi_ = _x_ 1), addition ( _i.e._, _Oi_ = _x_ 1 + _x_ 2 + + _xM_ ), concatenation ( _i.e._,
_· · ·_
_Oi_ = [ _x_ 1 _, x_ 2 _,_ _, xM_ ]) and self-attention. Figure 3 (a)
_· · ·_
presents an example design of the fusion cell with two input modalities ( _i.e._, **x** = ( _x_ 1 _, x_ 2)) and three operations ( _i.e._,
_O_ 1 = _x_ 1, _O_ 2 = _x_ 1 + _x_ 2, _O_ 3 = _w_ 1 _x_ 1 + _w_ 2 _x_ 2), where _w_ 1
and _w_ 2 are learnable parameters. Note that here we simplify the operation set for illustration; in practice, we can
always adopt more complex fusion operations in each cell
to enlarge the representation power. Let _B_ represent the
total number of operations. A gating network _G_ ( **x** ) takes
multimodal inputs and produces a _B_ -dimensional vector **g**
that decides which operation to execute. The output of cell
_h_ can be represented as: _h_ = [P] _[B]_ _i_ =1 _[g][i][O][i]_ [(] **[x]** [)][.] [Following the]

previous discussion, we adopt hard gates ( _i.e._, **g** is one-hot)
for computational efficiency.


Fusion-level DynMM allows decisions at a finer granularity and in a more flexible way by stacking fusion cells
to build a dynamic network. We provide an example architecture in Figure 3 (b) that we use in our experiments for
semantic segmentation ( _x_ 1 and _x_ 2 denote RGB and depth
images, respectively). The network consists of four fusion
blocks and a global gating network, which allows us to flexibly control the degree of fusion in a sample-wise manner.
For instance, we show the resulting architecture in Figure
3 (c), when the gating network selects _O_ 2 for fusion cell
1 & 2, and _O_ 1 for fusion cell 3 & 4. This not only skips
complex fusion operations that are not selected within the
fusion cell, but also saves unnecessary computations in the
feature extraction layer. Since we only adopt features from
modality 1 after fusion cell 2, there is no need to further
process features from modality 2. Thus, we can skip computations in the feature extraction layers for _x_ 2 ( _i.e._, blocks



3-4 marked in gray). This strategy resembles early exiting in unimodal dynamic networks, yet with different motivations. In essence, fusion-level DynMM saves future fusion and modality-wise operations for some multimodal inputs when combining low-level features from each modality ( _i.e._, fusing at early stages) is sufficient for good predictions. On the other hand, for “hard” instances, DynMM provides the option of combining multimodal features in each
cell with complex fusion operations for maximum representation power. Note that we replace the four individual gating
networks _G_ ( **x** ) in each fusion cell with a global gating network _G_ [ˆ] ( **x** ) for better integration; _G_ [ˆ] ( **x** ) takes multimodal
features ( _x_ 1 _, x_ 2) as input and makes decisions on which fusion operation to adopt for the four fusion cells.

This paradigm is especially helpful in tasks where the
final prediction is mainly based on a dominant modality,
while the other auxiliary modalities provide useful cues to
improve the prediction. Fusion-level DynMM provides a
flexible way to control _how_ and _when_ the auxiliary modality
comes in to assist the main prediction process. _Progressive_
_fusion_ is achieved by our carefully designed fusion cell and
dynamic architecture, leading to great computational savings, strong representation power and improved robustness.

Note that modality-level DynMM and fusion-level
DynMM are two approaches targeting different granularity
levels. In our experiments, we use modality-level DynMM
to solve two classification tasks, while the fusion-level
DynMM is used for the more challenging semantic segmentation task ( _i.e._, a dense prediction problem).


**3.3. Training Objective**

We notice that for both modality-level and fusion-level
DynMM designs, the computation for each expert network
_Ei_ (operation _Oi_ ) is different. Normally, an expert network
(an operation) that is computationally heavy has strong rep

2578


resentation power. If we directly train the network by minimizing a task-specific loss, the gating network is likely
to learn a trivial solution that always chooses the branch
with the heavy computation. To achieve efficient inference, we introduce a _resource-aware loss_ function into the
training objective. Let _C_ ( _Ei_ ) denote the computation cost
( _e.g._, MAdds) of executing an expert network _Ei_ . Similarly, _C_ ( _Oi,j_ ) represents the computation cost of the _i_ -th
fusion operation in the _j_ -th cell. Note that the computation
cost can be pre-determined before training and is a constant
term. The training objectives are shown below:



soft **˜g** in Equations (1)-(2) to enable back-propagation. During training, we anneal _⌧_ so that **˜g** gradually converges to
a desired one-hot vector. (b) Following the straight-through
technique [18], we adopt hard **g** in the forward pass and soft
**˜g** in the backward propagation with the gradient approximation _r_ **g** _⇡r_ **˜g** . In this way, the gating network still outputs
a discrete decision during training. Note that we always use
hard **g** during inference for computational benefits. Next,
we propose a two-stage training of DynMM that jointly optimizes the multimodal network and gating modules.

**Stage I** : _Pre-training_ . We find that following sparse decisions of the gating network in the early stage of training can result in a biased optimization. Branches that are
rarely selected have fewer and smaller weight updates; poor
performance may result in them getting selected less often
(thus never improving). The goal of a pre-training stage is
to ensure that every branch of DynMM is fully optimized
before the gating modules get involved. For modality-level
DynMM, we sufficiently train each expert network at this
stage. For fusion-level DynMM, we adopt random decisions ( _i.e._, randomly an operation from the set of candidate
operations) for each fusion cell so that each path of the dynamic network is optimized uniformly.

**Stage** **II** : _Fine-tuning_ . We incorporate gating networks
into our optimization process at this stage. With the
reparamterization technique introduced above, we jointly
optimize the dynamic network along with gating networks
in an end-to-end fashion.


**4. Experiments**

**4.1. Experimental Setup**

We conduct experiments on three multimodal tasks: (a)
movie genre classification on MM-IMDB [1]; (b) sentiment
analysis on CMU-MOSEI [51]; (c) semantic segmentation
on NYU Depth V2 [30]. To demonstrate the wide applicability of our proposed DynMM, we select the above three
tasks that include different modalities ( _i.e._, image and text
in task (a), video, audio and text in task (b), RGB and depth
images in task (c)). We adopt modality-level DynMM for
the first two tasks and fusion-level DynMM for the more
challenging semantic segmentation task. Due to space limitations, we present: (1) implementation details; (2) visualization of the gating network decision; (3) an analysis of
varying regularization strength _λ_ ; and (4) an ablation study
on training strategies of DynMM in the Appendix.


**4.2. Movie Genre Classification**

MM-IMDB is the largest publicly available multimodal
dataset for genre prediction on movies. It comprises 25,959
movie titles, metadata and movie posters. We select two
movie genres ( _i.e._, drama and comedy) for multi-label classification from posters (image modality) and text descrip

2579



_L_ = _Ltask_ + _λ_



X _B_


_i_ =1

X _B_


_i_ =1



_giC_ ( _Ei_ ) (modality-level) (1)


_gi_ [(] _[j]_ [)] _[C]_ [(] _[O][i,j]_ [)] (fusion-level) (2)



_L_ = _Ltask_ + _λ_



X _F_


_j_ =1



where _task_ denotes the _task_ _loss_, _e.g._, cross entropy be_L_
tween the network prediction and true label for classification. **g** [(] _[j]_ [)] represents the decision vector given by the _j_ -th
fusion cell. _B_ is the total number of experts (operations)
and _F_ is the number of fusion cells. _λ_ is a hyperparamter
controlling the relative importance of the two loss terms.

The new objectives (1) and (2) account for the computation cost of executing each path and enables DynMM to
achieve a desired tradeoff between accuracy and efficiency.
We can adjust the value of _λ_ based on the deployment constraints. For large _λ_, DynMM will prioritize lightweight
computations for high computational efficiency. For small
_λ_, DynMM will explore these computationally heavy paths
more often, thus yielding higher accuracy.


**3.4. Optimization**

We aim to train DynMM in an end-to-end manner. Since
the current gating network provides discrete decisions, the
branch selection is not directly differentiable with respect to
the gating network. Gumbel-softmax and reparameterization techniques [18] are introduced in the training process.
Recall that **g** denotes the desired one-hot _B_ -dimensional
decision vector produced by a gating network _G_ ( **x** ), _i.e._,
**g** = one-hot(arg max _i G_ ( **x** ) _i_ ). We adopt a real-valued soft
vector **˜g** with the following form:



exp(( _logG_ ( **x** ) _i_ + _bi_ ) _/⌧_ )
_g_ ˜ _i_ = ~~P~~ _B_



_j_ =1 [exp((] _[logG]_ [(] **[x]** [)] _[j]_ [+] _[ b][j]_ [)] _[/⌧]_ [)]



_i_ = 1 _,_ 2 _, . . ., B_



(3)
where _b_ 1 _, b_ 2 _, . . ., bB_ are samples independently drawn
from Gumbel(0, 1) [18] and _⌧_ denotes the softmax temperature. The distribution of **˜g** is more uniform with large _⌧_ and
resembles a categorical distribution with small _⌧_ . **˜g** serves
as a continuous, differentiable approximation of **g** . We consider two training techniques: (a) Hard **g** is replaced with


Micro
Method Modality
F1 (%)



Macro
F1 (%)



MAdds

(M)



Image Network I 39.99 25.26 5.0
Text Network ( _E_ 1) T 59.16 47.21 0.7
Late Fusion [24] ( _E_ 2) 59.55 50.94 10.3
LRTF [26] 59.18 49.26 10.3
MI-Matrix [19] 58.45 48.36 10.3

DynMM-a **59.57** 48.84 1.6

DynMM-b **59.59** 50.42 7.8

I+T

DynMM-c **59.72** **51.20** 9.8
DynMM-d **60.35** **51.60** 12.1



DynMM-a



I+T



Table 1. Results on the MM-IMDB Movie Genre Classification.
Modalities I and T denote image and text, respectively. The computation cost is measured by multiply-add operations (MAdds)
with one image-text pair as the input. M denotes million. Each
DynMM variant is obtained using a different value of the regularization hyperparameter _λ_ during training.


tions (text modality). We follow the original data split in [1],
and use 15,552 data for training, 2,608 for validation and
7,799 for testing. For preprocessing, we adopt the same
method as [1, 24] to extract text and image features.

We adopt two expert networks for this task, namely, a
unimodal network _E_ 1 that takes textual features as input and
another multimodal network _E_ 2 that adopts late fusion [24]
to combine image and text features. We do not consider the
use of an image-only network here due to its poor performance on this task. The gating network is a 2-layer MLP
with hidden dimension of 128, which takes concatenated
image and text features as input and outputs a 2-dimensional
vector for expert network selection. We set the temperature
of Gumbel-softmax as 1 and adopt straight-through training
( _i.e._, the gating network outputs a one-hot decision vector
in the forward propagation).

Table 1 provides the comparison of our proposed
modality-level DynMM with static unimodal networks and
multimodal networks. We provide results of DynMM under different resource requirements ( _i.e._, use different _λ_ in
the loss). From Table 1, we can see that DynMM achieves
a good balance between computational efficiency and performance. Compared to the static _E_ 2 network, DynMM-c
improves both MAdds and macro F1 score. DynMM-d provides maximum representation power by using soft gates
(which leads to more computation) and achieves best micro
and macro F1 scores. On the other hand, DynMM-a involves much less computation, while still maintaining good
performance (outperforms _E_ 1 by 1.6% in macro F1). This
demonstrates the great flexibility and efficacy of DynMM.

In addition, we vary _λ_ in Equation (1) to control the
importance of resource loss during training. The resulting
DynMM models have varying computation costs and performance, as shown in Figure 4 (a). On one hand, when



(a) (b)


Figure 4. Analysis of DynMM with varying resource regularization strength ( _λ_ ) on MM-IMDB. (a): Comparison of DynMM
with static unimodal (UM) and multimodal (MM) baselines. (b):
Branch selection ratio in DynMM with respect to _λ_ . DynMM offers a wide range of choices that balance computation and learning
behavior well.


compared against a multimodal baseline that is computationally heavy, DynMM maintains good performance with
much fewer MAdds. On the other hand, DynMM has better
representation power than a unimodal network and thus improves the F1 score. Figure 4 (b) shows the selection ratio
of each expert network in DynMM with respect to _λ_ . We
observe that as _λ_ increases, DynMM focuses more on reducing computation and thus is more likely to select expert
network 1 ( _E_ 1) with a small computation cost. Note that
for the _λ_ = 0 case, we adopt soft gates, _i.e._, every expert
network is activated and the output is a weighted combination of predictions given by the two expert networks. Thus,
DynMM achieves the best performance at the cost of increased computation. This also demonstrates the flexibility
of DynMM, as we can easily adjust _λ_ to target high performance or high inference efficiency.

**4.3. Sentiment Analysis**

CMU Multimodal Opinion Sentiment and Emotion Intensity (CMU-MOSEI) is the largest dataset of sentiment
analysis and emotion recognition. It contains 3,228 realworld online videos from more than 1000 speakers and 250
topics. Each video is split into short segments of 10-20 seconds. Each segment is annotated for a sentiment from -3
(strongly negative) to 3 (strongly positive). The task is to
predict the sentiment scores from video, audio and text. Following [24], we use 16,265 data for training, 1,869 data for
validation and 4,643 data for testing. The feature extraction
steps are the same as [24].

As text is the best performing modality in this task, we
adopt a unimodal network that takes textual features as input to be the expert network _E_ 1. The second expert network ( _E_ 2) of our DynMM is selected as a late fusion network [24] that receives inputs from three modalities. The
gating network is designed as a lightweight transformer
network with hidden dimension equal to 512 and 2 attention heads, followed by a linear layer. The gating network
receives concatenated features from three modalities and


2580


Method Modality Acc [2] (%) MAE MAdds (M)

Video Network V 69.02 0.80 123.1
Audio Network A 67.68 0.82 123.3
Text Network ( _E_ 1) T 78.35 0.62 124.7
Early Fusion [24] 78.45 0.65 313.5
V+A+T
Late Fusion [24] ( _E_ 2) 79.54 **0.60** 309.6



DynMM-a



DynMM-a 79.07 0.62 165.5

DynMM-b V+A+T **79.73** 0.61 254.5
DynMM-c **79.75** **0.60** 295.8



V+A+T



Table 2. Results on CMU-MOSEI Sentiment Analysis. Modalities
V, A, T represent video, audio and text, respectively. Acc [2] denotes
binary accuracy ( _i.e._, positive/negative sentiments) and MAE represents mean absolute error. MAdds are measured with a videoaudio-text tuple. Each DynMM variant is obtained using a different value of the regularization hyperparameter _λ_ during training.


_- Hi, I'm here to review In the Name of the King_

_- It's a film currently in theaters_

_- (uhh) It's based on the xxx series of video games_

_- This one was horrible_
_..._

_- (uhh) It tries to be Lord of the Rings_

_- It has a many of orge like creatures, a wizard, a medieval time setting_

_- Basically stole everything out of Lord of the Rings but made a million_
_times worse_

_- (umhh) It's just everyone stay away from this film_

_- It was a horrible movie_

_- (umm) So yea that's In the Name of the King in a nutshell_


Text Video + Audio + Text


Figure 5. We visualize a few test instances on CMU-MOSEI for a
negative sentiment. DynMM identifies sentences marked with red
as “easy” instances and only uses textual information for prediction. For sentences marked with blue, DynMM takes multimodal
inputs ( _i.e._, video+audio+text) for more accurate predictions.


generates sample-wise decisions on which expert network
to activate during inference time. We set temperature of
Gumbel-softmax as 1 and adopt straight-through training.

Results are summarized in Table 2. We provide three
DynMM networks trained with different _λ_ . Compared
with the best performing static network ( _i.e._, Late Fusion), DynMM-a can reduce computations by 46.5% with
a slightly decreased accuracy ( _i.e._, -0.47%). By allowing
more computation, DynMM-b improves both inference efficiency ( _i.e._, reduce MAdds by 17.8%) and prediction accuracy. Finally, DynMM-c further improves the accuracy
by trading off some computation; it achieves best accuracy
and smallest mean absolute error with reduced computation
cost. These results demonstrate the great advantages of dynamic multimodal fusion. Since multimodal data naturally
brings redundancy, we observe that many computations can
be reduced without loss in accuracy.

To have an intuitive sense of our gating network deci


ESANet [35] (baseline) 50.5 24.7   DynMM (Stage I) 48.5 11.7 52.6%
DynMM-a (Stage II) 49.9 11.1 55.1%
DynMM-b (Stage II) 51.0 19.5 21.1%


Table 3. Results on RGB-D semantic segmentation. mIoU denotes mean Intersection-over-Union. MAdds are calculated for input size of 3 _⇥_ 480 _⇥_ 640. G stands for Giga.


We adopt fusion-level DynMM for this task and base our
dynamic architecture design on a (static) efficient architecture, ESANet [35]. As illustrated in Figure 3, we incorporate four fusion cells in the encoder design, where each fusion cell contains two operations. Operation 1 is an identity
mapping of RGB features, _i.e._, _O_ 1 = _x_ 1. For the second
operation, we use channel attention fusion, where features
from both modalities are first reweighted with a Squeeze
and Excitation module [15] and then added element-wisely.
Two ResNet-50 [13] are used as feature extraction models
for RGB and depth modality. The decoder design is identical to [35]. The gating network comprises a pipeline of
2 convolution blocks with kernel size 5 _⇥_ 5 and stride size
2, a global average pooling and a linear layer. RGB and
depth features after the first convolutional layer are concantenated together and passed to the convolutional gate.
The gating network outputs a 4-dimensional vector per sample that determines which operation to select for each fusion cell. We experiment with two training strategies: (1)
DynMM-a in Table 3 is trained with straight-through technique with Gumbel-softmax temperature _⌧_ = 1; (2) We


2581



sion on which modality to select, we provide visualization
results of several test instances in Figure 5. For simplicity only the text modality is shown here, and the other two
modalities ( _i.e._, video and audio) are omitted. The gating network chooses _E_ 1 for sentences marked with red and
_E_ 2 for sentences marked with dark blue. We find that the
sentences marked with red often possess strong evidence
indicating the sentiments of this sample, _e.g._, ‘horrible’,
‘amazingly good’. Therefore, they belong to the “easy”
samples category that can be correctly predicted using the
text modality alone. On the contrary, the sentences marked
with dark blue are vague and require additional modalities
to help with the prediction. These results indicate that the
gating function is well trained and can provide reasonable
decisions based on input characteristics.

**4.4. Semantic Segmentation**

NYU Depth V2 is an indoor dataset for semantic segmentation. It contains 1,449 RGB-D images with 40-class
labels; 795 images are used for training and 654 images are
for testing. The two modalities are RGB and depth images.



mIoU
Method

(%)



Depth Enc
MAdds (G)



MAdds
Reduction (%)


Method Modality Backbone mIoU (%) MAdds (G)



LW-RefineNet [31] ResNet-50 41.7 **38.5**
RGB
LW-RefneNet [31] ResNet-101 43.6 61.2
ACNet [16] ResNet-50 48.3 126.2

SA-Gate [8] ResNet-50 50.4 147.6

RGB+D

CEN [45] ResNet-101 **51.1** 618.3
ESANet [35] ResNet-50 50.5 56.9



DynMM-a ResNet-50 49.9 **43.4**
RGB+D
DynMM-b ResNet-50 **51.0** 52.2


Table 4. Comparison of our approach with SOTA methods for
RGB-D semantic segmentation on NYU Depth V2 test data.


obtain DynMM-b in Table 3 by exponentially decaying _⌧_
from 1 to 0.0001 during 500 epochs.

Table 3 provides the detailed results of fusion-level
DynMM. We report performance of DynMM after firststage training in the second row; its great performance validates the design of our random gating function in the pretraining stage. This also lends support to our claim that
there exists a lot of redundancy in multimodal networks.
Utilizing the finding that depth modality plays an auxiliary
role in this task, fusion-level DynMM effectively reduces
computations of the depth encoder. DynMM-a reduces
MAdds by 55.1% with only -0.4% mIoU drop. Furthermore, DynMM-b achieves a mIoU improvement of 0.7%
and 21.1% reduction in MAdds at the same time, thus
demonstrating the superiority of DynMM over static fusion.

Table 4 presents a comparison of the resulting DynMMa and DynMM-b with SOTA semantic segmentation methods. For baseline methods, we list mIoU reported in their
original papers and report MAdds. These results clearly
show that our proposed method achieves the best balance
between performance and efficiency. The computation cost
of DynMM is similar to a unimodal lightweight RefineNet,
yet its performance can match methods that use ResNet-101
as the backbone and involve significantly larger MAdds.

Finally, we conduct experiments to demonstrate the improved robustness of DynMM compared to ESANet. We
consider three settings by injecting random Gaussian noise
with probability 1/3 to (1) RGB modality; (2) depth modality and (3) both modalities. We experiment with different degrees of random Gaussian noise and plot the performance degradation of two approaches in Figure 6. From
the figure, we observe that the performance gap between
DynMM and ESANet becomes larger when the noise level
of depth images increases; This demonstrates another advantage of DynMM in reducing data noise and improving
robustness. Figure 7 shows some qualitative segmentation
results. While ESANet generates reasonable predictions in
the normal setting ( _i.e._, first and third row), its performance
becomes significantly worse when multimodal data is perturbed by noise ( _i.e._, the second and fourth row). On the
contrary, our DynMM is robust to noise and provides a good



Figure 6. DynMM vs. ESANet on NYU Depth V2 with different
degrees of Gaussian noise injected into RGB / depth images.


**RGB** **depth** **ground truth** **ESANet** **DynMM (ours)**


**clean**


**w/ noise**


**clean**


**w/ noise**


Figure 7. Qualitative segmentation results on NYU Depth V2.
DynMM is more robust to noisy multimodal data compared with
the static ESANet.


prediction for both scenarios. These results suggest the potential of a dynamic neural network architecture for improving robustness of multimodal fusion.


**5. Conclusion**


Multimodal data enable models to learn from an enriched representation space, but it also bring significant redundancy. Motivated by this observation, we have proposed dynamic multimodal fusion (DynMM), a new approach that adaptively fuses inputs during inference. Experimental results on three very different multimodal tasks
demonstrate the efficacy of DynMM. More importantly, our
work demonstrates the potential of dynamic multimodal fusion and opens up a new research direction. Considering
the benefits of a dynamic architecture ( _i.e._, reduced computation, improved performance and robustness), we believe
that developing dynamic networks tailored for multimodal
fusion is a topic worthy of further investigations.

DynMM has limitations that we plan to address through
three areas of improvement in our future work. These include designing better dynamic architectures that can account for multimodal redundancy, extending DynMM to sequential decision-making tasks, such as long video prediction and exploring the performance of DynMM on different
multimodal tasks and modalities.


2582


**References**


[1] John Arevalo, Thamar Solorio, Manuel Montes-y G´omez,

and Fabio A Gonz´alez. Gated multimodal units for information fusion. _arXiv_ _preprint_ _arXiv:1702.01992_, 2017. 5,
6

[2] Tadas Baltruˇsaitis, Chaitanya Ahuja, and Louis-Philippe

Morency. Multimodal machine learning: A survey and taxonomy. _IEEE Transactions on Pattern Analysis and Machine_
_Intelligence_, 41(2):423–443, 2018. 1

[3] Emmanuel Bengio, Pierre-Luc Bacon, Joelle Pineau, and

Doina Precup. Conditional computation in neural networks
for faster models. _arXiv preprint arXiv:1511.06297_, 2015. 2

[4] Tolga Bolukbasi, Joseph Wang, Ofer Dekel, and Venkatesh

Saligrama. Adaptive neural networks for efficient inference. In _International_ _Conference_ _on_ _Machine_ _Learning_,
volume 70, pages 527–536. PMLR, 2017. 2

[5] Shaofeng Cai, Yao Shu, and Wei Wang. Dynamic routing

networks. In _Winter_ _Conference_ _on_ _Applications_ _of_ _Com-_
_puter Vision_, pages 3588–3597, 2021. 2

[6] Chen Chen, Roozbeh Jafari, and Nasser Kehtarnavaz. Utd
mhad: A multimodal dataset for human action recognition
utilizing a depth camera and a wearable inertial sensor. In
_International_ _Conference_ _on_ _Image_ _Processing_, pages 168–
172. IEEE, 2015. 1, 2

[7] Chunlin Chen and Qiang Ling. Adaptive convolution
for object detection. _IEEE_ _Transactions_ _on_ _Multimedia_,
21(12):3205–3217, 2019. 2

[8] Xiaokang Chen, Kwan-Yee Lin, Jingbo Wang, Wayne Wu,

Chen Qian, Hongsheng Li, and Gang Zeng. Bi-directional
cross-modality feature propagation with separation-andaggregation gate for rgb-d semantic segmentation. In _Eu-_
_ropean_ _Conference_ _on_ _Computer_ _Vision_, pages 561–577.
Springer, 2020. 2, 8

[9] Ruohan Gao, Tae-Hyun Oh, Kristen Grauman, and Lorenzo

Torresani. Listen to look: Action recognition by previewing audio. In _Conference_ _on_ _Computer_ _Vision_ _and_ _Pattern_
_Recognition_, pages 10457–10467, 2020. 2, 3

[10] Alex Graves. Adaptive computation time for recurrent neural

networks. _arXiv preprint arXiv:1603.08983_, 2016. 2

[11] Yizeng Han, Gao Huang, Shiji Song, Le Yang, Honghui

Wang, and Yulin Wang. Dynamic neural networks: A survey. _IEEE_ _Transactions_ _on_ _Pattern_ _Analysis_ _and_ _Machine_
_Intelligence_, 2021. 2, 3

[12] Zongbo Han, Fan Yang, Junzhou Huang, Changqing Zhang,

and Jianhua Yao. Multimodal dynamics: Dynamical fusion
for trustworthy multimodal classification. In _Proceedings of_
_the IEEE/CVF Conference_ _on Computer_ _Vision and Pattern_
_Recognition_, pages 20707–20717, 2022. 2, 3

[13] Kaiming He, Xiangyu Zhang, Shaoqing Ren, and Jian Sun.

Deep residual learning for image recognition. In _Conference_
_on_ _Computer_ _Vision_ _and_ _Pattern_ _Recognition_, pages 770–
778, 2016. 7

[14] Chiori Hori, Takaaki Hori, Teng-Yok Lee, Ziming Zhang,

Bret Harsham, John R Hershey, Tim K Marks, and Kazuhiko
Sumi. Attention-based multimodal fusion for video description. In _International Conference on Computer Vision_, pages
4193–4202, 2017. 1




[15] Jie Hu, Li Shen, and Gang Sun. Squeeze-and-excitation
networks. In _Conference_ _on_ _Computer_ _Vision_ _and_ _Pattern_
_Recognition_, pages 7132–7141, 2018. 7

[16] Xinxin Hu, Kailun Yang, Lei Fei, and Kaiwei Wang. Acnet:

Attention based network to exploit complementary features
for rgbd semantic segmentation. In _International Conference_
_on Image Processing_, pages 1440–1444. IEEE, 2019. 8

[17] Gao Huang, Shichen Liu, Laurens Van der Maaten, and Kil
ian Q Weinberger. Condensenet: An efficient densenet using
learned group convolutions. In _Conference on Computer Vi-_
_sion and Pattern Recognition_, pages 2752–2761, 2018. 2

[18] Eric Jang, Shixiang Gu, and Ben Poole. Categorical
reparameterization with gumbel-softmax. _arXiv_ _preprint_
_arXiv:1611.01144_, 2016. 5

[19] Siddhant M Jayakumar, Wojciech M Czarnecki, Jacob

Menick, Jonathan Schwarz, Jack Rae, Simon Osindero,
Yee Whye Teh, Tim Harley, and Razvan Pascanu. Multiplicative interactions and where to find them. 2020. 6

[20] Hamid Reza Vaezi Joze, Amirreza Shaban, Michael L Iuz
zolino, and Kazuhito Koishida. Mmtm: Multimodal transfer
module for cnn fusion. In _Conference_ _on_ _Computer_ _Vision_
_and Pattern Recognition_, pages 13289–13299, 2020. 1, 2

[21] Ramandeep Kaur and Sandeep Kautish. Multimodal sentiment analysis: A survey and comparison. _International_
_Journal_ _of_ _Service_ _Science,_ _Management,_ _Engineering,_ _and_
_Technology_, 10(2):38–58, 2019. 1, 2

[22] Michelle A Lee, Matthew Tan, Yuke Zhu, and Jeannette

Bohg. Detect, reject, correct: Crossmodal compensation of
corrupted sensors. In _International Conference on Robotics_
_and Automation_, pages 909–916. IEEE, 2021. 2

[23] Yanwei Li, Lin Song, Yukang Chen, Zeming Li, Xiangyu

Zhang, Xingang Wang, and Jian Sun. Learning dynamic
routing for semantic segmentation. In _Conference_ _on_ _Com-_
_puter_ _Vision_ _and_ _Pattern_ _Recognition_, pages 8553–8562,
2020. 2

[24] Paul Pu Liang, Yiwei Lyu, Xiang Fan, Zetian Wu, Yun

Cheng, Jason Wu, Leslie Chen, Peter Wu, Michelle A
Lee, Yuke Zhu, et al. Multibench: Multiscale benchmarks for multimodal representation learning. _arXiv preprint_
_arXiv:2107.07502_, 2021. 6, 7

[25] Kuan Liu, Yanen Li, Ning Xu, and Prem Natarajan. Learn

to combine modalities in multimodal deep learning. _arXiv_
_preprint arXiv:1805.11730_, 2018. 1, 2

[26] Zhun Liu, Ying Shen, Varun Bharadhwaj Lakshminarasimhan, Paul Pu Liang, Amir Zadeh, and Louis-Philippe
Morency. Efficient low-rank multimodal fusion with
modality-specific factors. _arXiv preprint arXiv:1806.00064_,
2018. 1, 2, 6

[27] Saeed Masoudnia and Reza Ebrahimpour. Mixture of ex
perts: a literature survey. _Artificial_ _Intelligence_ _Review_,
42(2):275–293, 2014. 3

[28] Ravi Teja Mullapudi, William R Mark, Noam Shazeer, and

Kayvon Fatahalian. Hydranets: Specialized dynamic architectures for efficient inference. In _Conference on Computer_
_Vision and Pattern Recognition_, pages 8080–8089, 2018. 2

[29] Arsha Nagrani, Shan Yang, Anurag Arnab, Aren Jansen,

Cordelia Schmid, and Chen Sun. Attention bottlenecks for


2583


multimodal fusion. _Advances_ _in_ _Neural_ _Information_ _Pro-_
_cessing Systems_, 34, 2021. 1, 2

[30] Pushmeet Kohli Nathan Silberman, Derek Hoiem and Rob

Fergus. Indoor segmentation and support inference from
rgbd images. In _European Conference on Computer Vision_,
2012. 5

[31] Vladimir Nekrasov, Chunhua Shen, and Ian Reid. Lightweight refinenet for real-time semantic segmentation. _arXiv_
_preprint arXiv:1810.03272_, 2018. 8

[32] Rameswar Panda, Chun-Fu Richard Chen, Quanfu Fan, Xi
meng Sun, Kate Saenko, Aude Oliva, and Rogerio Feris.
Adamml: Adaptive multi-modal learning for efficient video
recognition. In _Proceedings of the IEEE/CVF International_
_Conference on Computer Vision_, pages 7576–7585, 2021. 2,
3

[33] Juan-Manuel P´erez-R´ua, Valentin Vielzeuf, St´ephane Pa
teux, Moez Baccouche, and Fr´ed´eric Jurie. Mfas: Multimodal fusion architecture search. In _Conference_ _on_ _Com-_
_puter_ _Vision_ _and_ _Pattern_ _Recognition_, pages 6966–6975,
2019. 1, 2

[34] Sara Sabour, Nicholas Frosst, and Geoffrey E Hinton. Dy
namic routing between capsules. _Advances in Neural Infor-_
_mation Processing Systems_, 30, 2017. 2

[35] Daniel Seichter, Mona K¨ohler, Benjamin Lewandowski, Tim

Wengefeld, and Horst-Michael Gross. Efficient rgb-d semantic segmentation for indoor scene analysis. In _Inter-_
_national_ _Conference_ _on_ _Robotics_ _and_ _Automation_, pages
13525–13531. IEEE, 2021. 1, 2, 7, 8, 11

[36] Amir Shahroudy, Tian-Tsong Ng, Yihong Gong, and Gang

Wang. Deep multimodal feature analysis for action recognition in rgb+ d videos. _IEEE Transactions on Pattern Analysis_
_and Machine Intelligence_, 40(5):1045–1058, 2017. 1, 2

[37] Noam Shazeer, Azalia Mirhoseini, Krzysztof Maziarz, Andy

Davis, Quoc Le, Geoffrey Hinton, and Jeff Dean. Outrageously large neural networks: The sparsely-gated mixtureof-experts layer. _arXiv preprint arXiv:1701.06538_, 2017. 2

[38] Mohammad Soleymani, David Garcia, Brendan Jou, Bj¨orn

Schuller, Shih-Fu Chang, and Maja Pantic. A survey of multimodal sentiment analysis. _Image_ _and_ _Vision_ _Computing_,
65:3–14, 2017. 1, 2

[39] Peng Sun, Wenhu Zhang, Huanyu Wang, Songyuan Li, and

Xi Li. Deep rgb-d saliency detection with depth-sensitive
attention and automatic multi-modal fusion. In _Conference_
_on_ _Computer_ _Vision_ _and_ _Pattern_ _Recognition_, pages 1407–
1417, 2021. 1, 2

[40] Surat Teerapittayanon, Bradley McDanel, and Hsiang-Tsung

Kung. Branchynet: Fast inference via early exiting from
deep neural networks. In _International_ _Conference_ _on_ _Pat-_
_tern Recognition_, pages 2464–2469. IEEE, 2016. 2

[41] Hiroki Tokunaga, Yuki Teramoto, Akihiko Yoshizawa, and

Ryoma Bise. Adaptive weighting multi-field-of-view cnn for
semantic segmentation in pathology. In _Conference on Com-_
_puter_ _Vision_ _and_ _Pattern_ _Recognition_, pages 12597–12606,
2019. 2

[42] Valentin Vielzeuf, Alexis Lechervy, St´ephane Pateux, and

Fr´ed´eric Jurie. Centralnet: a multilayer approach for multimodal fusion. In _European Conference on Computer Vision_
_Workshops_, pages 0–0, 2018. 1




[43] Valentin Vielzeuf, St´ephane Pateux, and Fr´ed´eric Jurie. Tem
poral multimodal fusion for video emotion classification in
the wild. In _International Conference on Multimodal Inter-_
_action_, pages 569–576, 2017. 1, 2

[44] Xin Wang, Fisher Yu, Zi-Yi Dou, Trevor Darrell, and

Joseph E Gonzalez. Skipnet: Learning dynamic routing in
convolutional networks. In _European_ _Conference_ _on_ _Com-_
_puter Vision_, pages 409–424, 2018. 2

[45] Yikai Wang, Wenbing Huang, Fuchun Sun, Tingyang Xu, Yu

Rong, and Junzhou Huang. Deep multimodal fusion by channel exchanging. _Advances in Neural Information Processing_
_Systems_, 33:4835–4845, 2020. 1, 2, 8

[46] Yulin Wang, Kangchen Lv, Rui Huang, Shiji Song, Le Yang,

and Gao Huang. Glance and focus: a dynamic approach
to reducing spatial redundancy in image classification. _Ad-_
_vances in Neural Information Processing Systems_, 33:2432–
2444, 2020. 2

[47] Brandon Yang, Gabriel Bender, Quoc V Le, and Jiquan

Ngiam. Condconv: Conditionally parameterized convolutions for efficient inference. _Advances in Neural Information_
_Processing Systems_, 32, 2019. 2

[48] Mingyu Yang, Yu Chen, and Hun-Seok Kim. Efficient deep

visual and inertial odometry with adaptive visual modality
selection. _arXiv preprint arXiv:2205.06187_, 2022. 2

[49] Zhou Yu, Yuhao Cui, Jun Yu, Meng Wang, Dacheng Tao,

and Qi Tian. Deep multimodal neural architecture search. In
_International Conference on Multimedia_, pages 3743–3752,
2020. 1, 2

[50] Amir Zadeh, Minghai Chen, Soujanya Poria, Erik Cam
bria, and Louis-Philippe Morency. Tensor fusion network for multimodal sentiment analysis. _arXiv_ _preprint_
_arXiv:1707.07250_, 2017. 1, 2

[51] Amir Zadeh and Paul Pu. Multimodal language analysis in

the wild: Cmu-mosei dataset and interpretable dynamic fusion graph. In _Annual Meeting of the Association for Com-_
_putational Linguistics_, 2018. 1, 5

[52] Hong-Yu Zhou, Bin-Bin Gao, and Jianxin Wu. Adaptive
feeding: Achieving fast and accurate detections by adaptively combining object detectors. In _International_ _Confer-_
_ence on Computer Vision_, pages 3505–3513, 2017. 2


2584


