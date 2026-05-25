## **Embodied Agent Interface: Benchmarking LLMs for** **Embodied Decision Making**

**Manling Li** [1, 2] _[∗]_ **,** **Shiyu Zhao** [1] _[∗]_ **, Qineng Wang** [1, 2] _[∗]_ **, Kangrui Wang** [1, 2] _[∗]_ **, Yu Zhou** [1] _[∗]_ **,**
**Sanjana Srivastava** [1] **, Cem Gokmen** [1] **, Tony Lee** [1] **, Li Erran Li** [3] **, Ruohan Zhang** [1] **, Weiyu Liu** [1] **,**
**Percy Liang** [1] **, Li Fei-Fei** [1] **, Jiayuan Mao** [4] **, Jiajun Wu** [1]

1Stanford 2Northwestern 3Amazon 4MIT


**[embodied-agent-interface.github.io](https://embodied-agent-interface.github.io)**


[Data](https://huggingface.co/datasets/Inevitablevalor/EmbodiedAgentInterface)            - [Code](https://github.com/embodied-agent-eval/embodied-agent-eval)            - [PyPI](https://pypi.org/project/eai-eval/)            - [Docker](https://hub.docker.com/r/jameskrw/eai-eval)            - [Video](https://embodied-agent-interface.github.io/eai.mp4)            - [Docs](https://embodied-agent-eval.readthedocs.io)


**Abstract**


We aim to evaluate Large Language Models (LLMs) for embodied decision making. While a significant body of work has been leveraging LLMs for decision
making in embodied environments, we still lack a systematic understanding of their
performance because they are usually applied in different domains, for different
purposes, and built based on different inputs and outputs. Furthermore, existing
evaluations tend to rely solely on a final success rate, making it difficult to pinpoint
what ability is missing in LLMs and where the problem lies, which in turn blocks
embodied agents from leveraging LLMs effectively and selectively. To address
these limitations, we propose a generalized interface (EMBODIED AGENT INTERFACE) that supports the formalization of various types of tasks and input-output
specifications of LLM-based modules. Specifically, it allows us to unify 1) a
broad set of embodied decision-making tasks involving both state and temporally
extended goals, 2) four commonly-used LLM-based modules for decision making: goal interpretation, subgoal decomposition, action sequencing, and transition
modeling, and 3) a collection of fine-grained metrics which break down evaluation
into various types of errors, such as hallucination errors, affordance errors, various
types of planning errors, etc. Overall, our benchmark offers a comprehensive
assessment of LLMs’ performance for different subtasks, pinpointing the strengths
and weaknesses in LLM-powered embodied AI systems and providing insights for
effective and selective use of LLMs in embodied decision making.


**1** **Introduction**


Large Language Models (LLMs) have emerged as powerful tools for building embodied decisionmaking agents capable of following human instructions (such as “ _cleaning the refrigerator_ ”, “ _upol-_
_ishing furniture_ ”) and achieving the specified goals through a sequence of actions in various digital
and physical environments [1–3]. Despite many reports of their success, our understanding of LLMs’
full capabilities and limitations in embodied decision-making remains limited. Existing evaluation
methods fall short of providing a comprehensive insight due to three key limitations: the lack of
standardization of 1) embodied decision-making tasks, 2) modules that an LLM can interface with or
be implemented for, and 3) fine-grained evaluation metrics beyond a single success rate. In this paper,
we propose EMBODIED AGENT INTERFACE, to address these challenges.


(1) **Standardization of goal specifications** : We want embodied agents to achieve goals. However,
the specification of goals and the criteria for agents’ success evaluation vary significantly across
different domains, even for similar tasks. For example, BEHAVIOR [4] focuses on achieving a state
that satisfies certain _state goals_ (e.g., “not _stained_ (fridge)” in Figure 1), while VirtualHome [5] uses


_∗_ Equal contribution.


38th Conference on Neural Information Processing Systems (NeurIPS 2024) Track on Datasets and Benchmarks.


Representations


Ability Modules



Large Language Models (LLMs)


Embodied Agent Interface


Object State Action Goal Trajectory


Goal Interpretation Subgoal Decomposition Action Sequencing Transition Modeling


Task: use the rag to clean the refrigerator



































Figure 1: EMBODIED AGENT INTERFACE unifies a broad set of tasks involving both state and temporally
extended goals and four LLM-based modules for decision-making.


temporally extended goals by imposing temporal order constraints on actions. We include an extended
discussion in Appendix C.1. Our EMBODIED AGENT INTERFACE implements a general objectcentric state and action representation, where object states, relations, and actions are represented in
abstract language terms (see Figure 1). Our innovation is to describe goals as linear temporal logic
(LTL) formulas, which define task-success criteria over trajectories. LTL affords the specification of
both state-based and temporally extended goals and allows for alternative goal interpretations.







**Output**


**LLM for**


**Input**































Figure 2: The input and output formulation of four ability modules.


(2) **Standardization of modules and interfaces** : Existing LLM-based embodied agent frameworks
often make different assumptions based on the availability of additional knowledge and external
modules. For instance, Code as Policies [6] and SayCan [2] utilize LLMs for action sequencing given
a given set of primitive skills, while LLM+P [7] uses LLMs for goal interpretation and generates
plans using PDDL planners with given domain definitions; Ada [8] leverages LLMs to generate
high-level planning domain definitions in PDDL and uses a low-level planner to generate control
commands. Consequently, they have defined different input-output specifications for the LLM
module, making comparisons and evaluations challenging. In EMBODIED AGENT INTERFACE,
built on top of our object-centric and LTL-based task specification, we formalize four critical _ability_
_modules_ in LLM-based embodied decision making, as illustrated in Figure 1: _Goal Interpretation_,
_Subgoal Decomposition_, _Action Sequencing_, _Transition Modeling_ . We formalize the input-output


2


specifications that LLMs can use to interface with other modules in the environment. This modular
interface automatically enables the integration of different LLM-based and external modules. Figure 2
shows the input and output formulation of four ability modules. Taking _Subgoal Decomposition_ as an
example, this module takes initial states (eg. a fridge is stained initially) and a task goal (eg. clean
fridge), and asks LLMs to generate a subgoal trajectory (eg. first cloth is soaked, then a cloth is held
by the agent, then the agent is next to the fridge, in the end, the fridge is clean). Formal definitions
and notations can be found in Table 1.


(3) **Broad** **coverage** **of** **evaluation** **and** **fine-grained** **metrics** : Current evaluations of LLMs for
embodied decision-making have been overly simplified, usually focusing on the success rate of a
single task. The recent work LOTA-Bench [9] aims to break down the evaluation but is limited
to generating action sequences and does not support analysis of fine-grained planning errors. Our
EMBODIED AGENT INTERFACE, leveraging object-centric and factorized representations of states
and actions, implements a collection of fine-grained evaluation metrics, designed to automatically
locate different types of errors such as hallucination errors, different types of planning errors (e.g.,
object affordance errors, wrong action orders, etc.). Figure 3 illustrates different types of errors
made by GPT-4o on four different ability modules across two simulators. Specifically, we evaluate
two aspects of each module: trajectory evaluation, which checks if the generated plan can be
executed in simulators, and goal evaluation, which ensures the plan achieves correct outcomes.
Goal evaluation applies to goal interpretation, action sequencing, and subgoal decomposition, while
trajectory evaluation applies to action sequencing, subgoal decomposition, and transition modeling.



Task Success









**False Positives**


**Missing Goals**


**Format Errors**
**Halucination**


**Additional Step**


**Missing Step**


**Affordance Error**

**Wrong Order**

**Format Errors**

**Hallucination**



Object

































Success Rate



Executable

Rate



Gemini1.5-Pro GPT4o Llama3-70B





Claude3.5-Sonnet o1



Mixtral-8x22B



Figure 3: EMBODIED AGENT INTERFACE supports a collection of fine-grained metrics and provides automatic
toolkits for error analysis and benchmarking different LLMs on various embodied decision-making tasks.

We implement EMBODIED AGENT INTERFACE on two embodied decision-making benchmarks:
BEHAVIOR [4] and VirtualHome [5], and evaluated 18 different LLMs. Figure 3 visualizes the
performance of 5 representative LLMs on different tasks in Behavior. Our key findings are


 - Most LLMs struggle to faithfully translate natural language instructions into grounded states
(objects, object states, and relations) in the environment. They sometimes predict intermediate
subgoals as part of the final goals, e.g., predicting the state _open_ (freezer) for task “ _drinking water_ ”.

 - Reasoning ability is a crucial aspect that LLMs should improve. Trajectory feasibility errors are
common (45.2%), with a large portion of missing step (19.5%) and additional step (14.2%) errors,
often due to overlooking preconditions. For instance, LLMs may ignore the agent’s _sitting_ or _lying_
state and fail to include a _standup_ action before executing other actions. They sometimes also fail
to understand the need to _open_ a _closed_ object before _fetching_ items from inside. Additional step
errors frequently occur when LLMs output actions for previously achieved goals.

 - Trajectory evaluation performance decreases as the trajectory sequence length increases; goal
evaluation performance, which refers to evaluating if a plan can achieve task goals when executed,
decreases when the environment becomes more complex, involving a larger variety of object and
state features.

 - LLM errors include not only hallucinations of nonexistent objects and actions but also a heavy
reporting bias. They often ignore commonsense preconditions that are elided in the language . For
example, “put the turkey on the table” should be interpreted as “put the turkey on a plate, and place
the plate on the table.”

 - Subgoal decomposition, though designed to simplify planning, is as complex as action sequencing
in abstract spaces, as LLMs must declaratively strategize how to break down goals into feasible
steps.


3


Table 1: Summary of notations used in EMBODIED AGENT INTERFACE.


**Notation** **Symbol** **Description**

Object _u ∈U_ An object, which has relational features _f_
State _s_ = _⟨U, F⟩∈S_ A tuple of the universe of objects and relational features
Action _a_ = _⟨name, args⟩∈A_ A tuple of the action name and arguments
Operator _o_ = _⟨name, vars⟩∈O_ An action schema: a tuple of the name and a list of parameters.
Each _o_ can be instantiated into an action _a_
Transition Model _M_ : _S_ _× A →S_ The deterministic transition function of the environment
Natural Language Goal _lg_ A sentence in English
LTL Goal _g_ An LTL formula. Here, we only consider formulas containing a
sequence of action items and a conjunction of propositions (for the
fnal state): _g_ = _a_ 1 **then** _. . ._ **then** _ak_ **then** ( _p_ 1 _∧_ _. . . ∧_ _pℓ_ ).
Action Trajectory _a_ ¯ = _{ai}_ _[n]_ _i_ =1 A sequence of _n_ actions
Subgoal Trajectory _ϕ_ ¯ = _{ϕi}_ _[m]_ _i_ =1 A sequence of LTL subgoals _ϕi_ connected by “ **then** ”
State-action Trajectory _t_ ¯ = _⟨{si}_ _[n]_ _i_ =0 _[,][ {][a][i][}]_ _i_ _[n]_ =1 _[⟩]_ A sequence of state-action pairs. _∀t.st_ +1 = _M_ ( _st, at_ )
Task _⟨s_ 0 _, g, lg⟩_ A tuple of the initial state and the LTL/Natural Language goals


 - We further provide quantitative analysis for the robustness of the modules through sensitivity
analysis, pipeline-based versus modularized comparison, and replanning. These analyses aim to
identify potential ways to integrate LLM-based and external modules.

 - o1-preview significantly outperforms others, especially on the BEHAVIOR simulator (74.9% vs.
64.2%). It excels in goal interpretation on VirtualHome, as well as action sequencing, transition
modeling, and subgoal decomposition on both BEHAVIOR and VirtualHome. Claude-3.5 Sonnet
is strong in goal interpretation on BEHAVIOR and transition modeling on VirtualHome, while
Mistral Large performs well in action sequencing on VirtualHome.


**2** **Embodied Agent Interface Based on LTL**


Table 1 summarizes our EMBODIED AGENT INTERFACE. First, we define an **embodied decision-**
**making problem representation** _⟨U, S, A, g, ϕ,_ ¯ _a⟩_, which is a language-based, object-centric abstraction for embodied agent environments with _objects_ ( _o_ _∈U_ ), _states_ ( _s_ _∈S_ ), _actions_ ( _a_ _∈A_ ),
_goal g_, _subgoal ϕ_, and trajectories ¯ _a_ . Second, we formally define four **ability modules** _⟨G,_ Φ _, Q, T ⟩_,
including their standardized input-output specifications. They are fundamental and commonly-used
modules that LLMs can be implemented for and interface with the _goal interpretation_ module _G_,
the _action sequencing_ module _Q_, the _subgoal decomposition_ module Φ, and the _transition modeling_
module _T_ . In this paper, we focus on object-centric modeling: states are described as relational
features among entities in the environment, actions are defined functions that take entity names as
inputs and can be executed in the environment, goals and subgoals are defined as linear-temporal
logic (LTL) [10] formulas on states and actions. We define each component in detail as follows.


**2.1** **Representation for Objects, States and Actions**


In EMBODIED AGENT INTERFACE, a state is represented as a tuple _s_ = _⟨U, F⟩_, where _U_ is the
universe of objects, assumed to be a fixed finite set. _F_ is a set of relational Boolean features. Each
_f_ _∈F_ is a table where each entry is associated with a tuple of objects ( _o_ 1 _, · · ·_ _, ok_ ). Each entry
has the value of the feature in the state, and _k_ is the arity of the feature. Actions can be viewed as
primitive functions that take objects as inputs, denoted as _⟨name, args⟩_ . Throughout the paper, we
focus on tasks where states and actions are described in abstract language forms, including object
states (e.g., _is-open_ (cabinet1)), relations (e.g., _is-on_ (rag0 _,_ window3)), and actions (e.g., _soak_ (rag0)).


**2.2** **Representation for Goals, Subgoals, Action Sequences, and State-Action Trajectories**


In EMBODIED AGENT INTERFACE, goals _g_, subgoals _ϕ_, and action sequences _a_ ¯ are modeled as
linear temporal logic (LTL) formulas. This is motivated by two critical desiderata. First, we need an
expressive and compact language to describe both state-based and temporally extended goals. Second,
we need a unified interface between different LLM-based modules. LTL addresses both challenges.
At a high level, an LTL formula can describe state constraints (e.g., a subgoal should be achieved),


4


**Abilities** **Simulator Output**



**Metrics**


Goal _F1_


Execution Success Rate
Task Success Rate


Execution Success Rate
Task Success Rate



**Goal**
**Interpretation**


**Subgoal**
**Decomposition**


**Action**
**Sequencing**


**Transition**

**Modeling**

















Goal State


Action Trajectory
Final State


Action Trajectory
Final State





Action Trajectory Transition Model _F1_

(PDDL) Planner Success Rate



Figure 4: The overview of evaluation pipeline for four abilities. For each ability module, to provide a comprehensive evaluation for it, we isolate this single module to be handled by the LLMs while using existing data or tools
for the other modules. Note that the pipeline consists of goal interpretation, action sequencing to achieve the
goal, and transition modeling that predicts how each action operate the environment’s state. Evaluating subgoal
decomposition presents a challenge since it cannot be evaluated directly with no unified annotation strategy. To
address this, we employ breadth-first search (BFS) to identify potential action sequences that accomplish each
subgoal, allowing us to convert state trajectories into executable action sequences that can be evaluated in the
simulator. Transition modeling poses another challenge, we first annotate transition models for _F_ 1 followed with
a PDDL planner to validate the feasibility of supporting potential plans. We also conduct a pipeline-based vs
modularized analysis, detailed in the Appendix G.


action constraints (e.g., a particular action should be executed), and possible temporal orders among
them (e.g., all dishes should be cleaned before we cook). By combining temporal connectives (such
as “eventually”) and propositional logic connectives (such as “or”), we can also flexibly describe
alternative goals or trajectories. As a byproduct, using a single description language for all inputs and
outputs enables us to design a unified metric to measure accuracy, which we detail in Appendix C.1.


In EMBODIED AGENT INTERFACE, we use a fragment of the full linear temporal logic (LTL)
formalism on finite trajectories. We allow two types of atomic propositions: state propositions (object
properties and relations) and action propositions. Our LTL language contains Boolean conjunction _∧_,
disjunction _∨_, negation _¬_, implication _⇒_, first-order logic quantifiers _∀_, _∃_, _∃_ [=] _[n]_ (the equal quantifier:
there are exactly _n_ objects satisfying a condition), and the temporal connective **then** .


An LTL formula is a trajectory classifier semantically: the function _eval_ ( _ϕ,_ _t_ [¯] ) evaluates an LTL
formula _ϕ_ on a state-action sequence _t_ [¯] . We say that the state-action sequence satisfies _ϕ_ if _eval_ ( _ϕ,_ _t_ [¯] ) =
_true_ (i.e., the goal _ϕ_ is satisfied). For state formulas _ϕ_ (formulas without **then** ), we define _eval_ ( _ϕ,_ _t_ [¯] ) =
_∃t.ϕ_ ( _st_ ) (“eventually” the goal is satisfied). For formulas connected by **then**, _eval_ ( _ϕ_ 1 **then** _ϕ_ 2 _,_ _t_ [¯] ) =
_∃k.ϕ_ 1( _t_ [¯] _≤k_ ) _∧_ _ϕ_ 2( _t_ [¯] _>k_ ) ( _ϕ_ 2 is achieved after _ϕ_ 1), where _t_ [¯] _≤k_ and _t_ [¯] _>k_ denote prefixes and suffixes.
Currently, we have not implemented other temporal connectives such as “globally” and “until” but
our overall framework can be extended to them. An LTL formula example of a subgoal plan for task
_browse Internet_ is: “ _ontop_ (character, chair) **then** _holds_rh_ (character, mouse) _∧_ _holds_lh_ (character,
keyboard) **then** _facing_ (character, computer)”. We include LTL details in Appendix C.3.


**2.3** **Ability Module 1:** **Goal Interpretation** _G_ : _⟨s_ 0 _, lg⟩→_ _g_


**Input-Output Specification.** The _goal interpretation_ module takes the state _s_ 0 and a natural language
instruction _lg_ as input, and generates an LTL goal ˆ _g_, as a formal goal specification which a symbolic
planner can conceivably take as input. In this paper, we only generate simple LTL goals formed by
an ordered action sequence and a conjunction of propositions to be satisfied in the final state.


**Evaluation Metric.** An LTL goal can be evaluated by directly comparing it with the ground truth
goal _g_ . While we have restricted generated ˆ _g_ to be simple LTL goals, we do not require the ground
truth goal _g_ to be simple. Therefore, we additionally define _G_ that takes the object universe _U_ as
input to translate _g_ to a set of simple LTL goals _g_ 0 _, g_ 1 _, . . ., gk_ where all _gi_ ’s entail _g_ . We describe
our implementation in the Appendix. Given two simple LTL goals _gi_ and ˆ _g_, the accuracy of ˆ _g_ can be
computed as an F1 set-matching score between them. Let _g_ = _a_ 1 [then] _. . . ak_ **then** ( _p_ 1 _∧_ _. . . ∧_ _pℓ_ ). We
define _set_ ( _g_ ) = _{{ai}_ _[k]_ _i_ =1 _[} ∪{][p][i][}][ℓ]_ _i_ =1 [(i.e., the action sequence] _[ {][a][i][}]_ [ is treated as a single element).]
The F1 score between _g_ and ˆ _g_ is defined as: F1( _g,_ ˆ _g_ ) = max _gi∈G_ ( _g,U_ ) F1 ( _set_ ( _gi_ ) _, set_ (ˆ _g_ )) _._

**2.4** **Ability Module 2:** **Subgoal Decomposition** Φ : _⟨s_ 0 _, g⟩→_ _ϕ_ [¯]


**Input-Output** **Specification.** The _subgoal_ _decomposition_ module takes the task _⟨s_ 0 _, g⟩_ as input
and generates a sequence of subgoals _ϕ_ [¯] = _{ϕi}_ _[k]_ _i_ =1 [, where each] _[ ϕ][i]_ [is an LTL formula.] [The entire]


5


sequence _ϕ_ [¯] can also be represented as a single LTL formula. One may refer to Appendix D.3 for
decomposition choice-making.


**Evaluation Metric.** To evaluate the subgoal decomposition module, we use a customized planner
to refine it into an action sequence ¯ _a_ . This subgoal-action mapping function _AM_ ( _ϕ, s_ [¯] 0) takes the
LTL representation of _ϕ_ [¯] and _s_ 0 and generates a state-action sequence _t_ [¯] . We implement this with a
breadth-first search. Then, we use the same metrics in _action sequencing_ for evaluation: trajectory
feasibility and goal satisfaction. Since each _ϕ_ can be grounded into different action sequences, we
restrict the number of actions per subgoal to generate a finite set of possible action sequences _a_ ¯ _i_
satisfying _ϕ_ . Then, we compute the metrics for each ¯ _ai_ and report the maximum score across all ¯ _ai_ ’s
as the trajectory feasibility and the goal satisfaction scores for _ϕ_ .


**2.5** **Ability Module 3:** **Action Sequencing** _Q_ : _⟨s_ 0 _, g⟩, M →_ _a_ ¯


**Input-Output Specification.** The _action sequencing_ module takes the task _⟨s_ 0 _, g⟩_ as input, and the
transition model _M_, and generates an action sequence ¯ _a_ = _{ai}_ _[n]_ _i_ =1 [.]


**Evaluation Metric.** We use two evaluation metrics for the action sequencing module. First, the
_trajectory feasibility evaluation_ focuses on evaluating whether the trajectory is executable (i.e., all
actions are feasible). We will execute the trajectory _a_ ¯ from _s_ 0 in the simulator. When infeasible
action presents, the execution may stop at an early step and we categorize the execution failure into
missing steps, additional steps, wrong temporal order, and affordance errors.


Second, the _goal satisfaction evaluation_ evaluates if the goal is satisfied after executing ¯ _a_ . Specifically,
we obtain _T_ = _⟨{si}_ _[m]_ _i_ =0 _[,][ {][a][i][}][m]_ _i_ =1 _[⟩]_ [by executing][ ¯] _[a]_ [, and directly use the] _[ eval]_ [(] _[g, T]_ [)][ function to check]
for goal satisfaction. We also evaluate the _partial goal satisfaction evaluation_, which is the percentage
of “subgoals” in _g_ that are satisfied in _a_ ¯. To compute this partial success rate, we again consider
all simple LTL goals _gi_ derived from _g_ . Let _gi_ = _a_ 1 [then] _. . ._ _ak_ **then** ( _p_ 1 _∧_ _. . . ∧_ _pℓ_ ). If there is a
subsequence in ¯ _a_ that is the same as _{aj}_ _[k]_ _j_ =1 [, we consider the action sequence successfully executed.]
Next, we evaluate all final state propositions _pj_ and give models partial credits based on the number
of propositions satisfied in _sm_ . Finally, _PartialSucc_ (¯ _a, g_ ) = max _gi∈G_ ( _g,U_ ) _PartialSucc_ (¯ _a, gi_ ).


**2.6** **Ability Module 4:** **Transition Modeling** _T_ : _⟨s_ 0 _, g⟩, o →⟨pre, eff⟩_


**Input-Output Specification.** The _transition modeling_ module takes the task _⟨s_ 0 _, g⟩_ and a set of
operator definitions _{oi}_ as input, and generates a PDDL operator definition [11] for each _oi_ . In
this module, we aim to create a formal definition of actions in order to generate plans to solve the
task. During evaluation, we first extract relevant operator definitions, _{oi}_, based on the ground truth
action trajectory ¯ _a_ associated with each task, with details provided in Appendix C.3. Then, the LLM
generates the preconditions and effects _{⟨prei, effi⟩}_ for all operators _{oi}_ .


**Evaluation Metric.** The _transition modeling_ module can be evaluated in two ways. First, the _logic_
_matching_ _score_ for an operator _oi_ compares the generated _prei_ and _effi_ against the ground truth
operator definition annotated by human experts. This comparison uses a surface form matching score
to produce an F1-based score between two logic formulas. Intuitively, when both the LLM-generated
_prei_ and ground truth _pre_ _[gt]_ _i_ [are] [conjunctions] [of] [propositions,] [the] [F][1] [score] [is] [computed] [as] [the] [set]
matching score between the sets of propositions. More complex logic formulas (e.g., _∀x.ϕ_ ( _x_ )) are
evaluated recursively, as detailed in Appendix C.3. The evaluation of effects is performed similarly.


Furthermore, the _planning success rate_ assesses whether the preconditions and effects of different
operators enable a viable plan. This is computed by running an external PDDL planner [12] based
on generated operator definitions to achieve _g_ from the initial state _s_ 0. For simplicity, we only state
goals in _g_ (and ignore action subgoals). The planning success rate is 1 if the planner finds a plan.


**3** **Dataset Annotations and Benchmark Implementations**


**Annotations.** Focusing on complex long-horizon tasks, we select BEHAVIOR (B) and VirtualHome
(V) as our evaluation simulators based on their task length and scene complexity. We include a
comparison of different simulators and detailed selection considerations in Appendix M.1. Table
2 shows our annotations. Apart from the goal and trajectory annotations, we introduce the Goal
Action annotation to reflect necessary actions that do not have post effects, such as the goal action
_touch_ in the task “ _pet the cat_ ”, as detailed in Appendix M.3. In the subset of VirtualHome tasks we
work on, 80.7% task categories include instructions with action steps longer than 10, and 33% of the
instructions have step lengths of more than 10.


6


We select BEHAVIOR as another simulator
for our evaluation due to its task complexity.
BEHAVIOR BDDL goals may contain quantifiers, such as (forpairs (?jar ?apple)
(inside ?apple ?jar)), which need to
be translated into grounded goals of only
atomic propositions, e.g., and ((inside
apple_1 jar_1) (inside apple_2
jar_2)). There can be different grounded
goals that satisfy the same BDDL goal, such
as ((inside apple_2 jar_1) (inside
apple_1 jar_2)). We call them goal options.
In general, one BDDL goal corresponds to a
number of goal options. The average number
of grounded goals for each task is 6 _._ 7, and
there are 4 _,_ 164 _._ 4 goal options for each task
on average. We show data distributions of goal
options and other statistics in Appendix M.2.



Table 2: Simulator dataset statistics. New annotations
collected in this paper are highlighted in color.


#task name 26 100



**Implementation on simulators.** As BEHAVIOR does not have an action transition model layer,
we implemented a symbolic simulator with an action transition model layer. Our implementation,
EvalGibson, offers 30 actions that agents can use to change the states of objects. Implementation
details are in Appendix N.1. We also revise the VirtualHome simulator to support accurate evaluation,
as detailed in Appendix N.2. Evaluation settings for each large model are detailed in Appendix O.


**4** **Results**


We evaluate 18 open-weight and proprietary LLMs on four embodied agent ability modules across
two benchmark simulators: BEHAVIOR and VirtualHome. Table 3 gives an overview. Table 4,
Table 5, Table 6, and Table 7 break down the analysis of four representative LLMs on four ability
modules. Figure 5 shows examples of different types of error. We start with the overall analysis.


**Model** **Comparison.** Shown in Figure 3, the top performing models overall are o1-preview,
Claude-3.5 Sonnet and GPT-4o, with o1-preview leading in all aspects except **object** **states** and
Gemini 1.5 Pro leading in its **object state reasoning** ability. Among all open-weight models, the best
performing models are Llama-3-70B and Mistral-Large-2402, while there is still a performance gap
with commercial models.


**Ability Comparison.** o1-preview shows a clear advantage over other models, particularly on the
BEHAVIOR simulator, where it achieves 74.9% compared to 64.2%. It leads in several areas,
including goal interpretation on VirtualHome and both action sequencing and transition modeling
on BEHAVIOR. Moreover, it outperforms in subgoal decomposition across both BEHAVIOR and
VirtualHome simulators. In contrast, Claude-3.5 Sonnet shines in goal interpretation on BEHAVIOR
and transition modeling on VirtualHome, while Mistral Large stands out in action sequencing on
VirtualHome. Mixtral-8x22B shines in transition modeling among open-weight LLMs, and Llama-370B Instruct in goal interpretation.


We also observe a performance gap between different simulators. Models achieve significantly lower
trajectory feasibility scores on BEHAVIOR compared to VirtualHome, but achieve higher scores on
goal interpretation. This is because BEHAVIOR tasks have a much longer horizon (avg 14.6 steps)
while VirtualHome goals have a larger state space to search (such as “ _work_ ”), as detailed in Appendix
L.2. It shows the inverse correlation between trajectory evaluation performance and sequence length,
as well as between goal evaluation performance and environment complexity. We further perform a
systematic analysis to discover the cofactors for the goal success rate, including the number of task
goals, particularly node goals, the ground truth action length, and the task object length, with details
in Appendix E.5.


**Object States vs Relationship.** Relational goals are generally harder to reason about compared to
object-state goals. Spatial relations have a significantly lower recall in the goal interpretation task
(Table 4) and a lower goal satisfaction rate (Table 5). Some non-spatial relations (e.g., _hold_ ) are even
more difficult for LLM to predict than spatial relations, as shown in the transition modeling accuracy
(Table 6): for example _holding_ (toothbrush) should be a precondition for brushing teeth.


7


Table 3: Results (%) overview. _V_ : VirtualHome, _B_ : BEHAVIOR. Full results in Appendix E.


**Goal Interpretation** **Action Sequencing** **Subgoal Decomposition** **Transition Modeling** **Average Perf.**

**Model** _F1_ _Task SR_ _Execution SR_ _Task SR_ _Execution SR_ _F1_ _Planner SR_ _Module SR_

_V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_

Claude-3 Haiku 28.0 52.5 43.3 26.0 48.5 32.0 78.4 30.0 82.8 35.0 42.3 51.6 30.4 64.0 46.5 41.6
Claude-3 Sonnet 29.4 69.4 62.9 44.0 67.2 57.0 83.1 39.0 86.4 43.0 41.2 56.2 13.2 80.0 50.7 55.1
Claude-3 Opus 31.4 77.0 66.2 51.0 70.8 59.0 86.7 41.0 89.9 47.0 48.8 63.4 61.8 82.0 59.9 60.4
Claude-3.5 Sonnet 33.0 **82.7** 72.8 60.0 75.4 69.0 89.1 39.0 92.0 44.0 **48.9** 67.9 80.5 82.0 64.9 64.2
Cohere Command R 36.7 36.0 24.6 16.0 37.7 19.0 71.3 15.0 78.1 25.0 11.7 24.1 51.1 41.0 41.0 24.9
Cohere Command R+ 22.4 51.2 63.3 27.0 70.2 35.0 77.8 25.0 83.7 37.0 30.8 49.7 37.2 59.0 49.4 39.1
Gemini 1.0 Pro 23.8 60.0 34.4 27.0 45.9 32.0 70.4 24.0 84.6 33.0 41.8 45.8 11.8 16.0 38.9 35.5
Gemini 1.5 Flash 26.8 74.8 61.9 40.0 67.2 52.0 89.1 34.0 **94.1** 42.0 45.7 53.4 46.6 66.0 56.0 52.1
Gemini 1.5 Pro 37.9 79.6 73.1 42.0 83.3 54.0 87.0 31.0 91.1 37.0 34.1 45.8 **91.9** 39.0 65.3 48.8
GPT-3.5-turbo 22.7 50.4 14.7 16.0 31.8 20.0 69.2 24.0 81.4 36.0 30.0 42.1 0.7 41.0 30.5 33.0
GPT-4-turbo 33.2 77.2 57.0 38.0 65.6 45.0 85.5 38.0 **94.1** 47.0 42.9 44.2 56.1 46.0 56.3 49.6
GPT-4o 36.5 79.2 61.6 47.0 71.1 53.0 87.6 49.0 91.1 55.0 46.7 60.9 68.2 67.0 60.8 59.8
Llama 3 8B Instruct 22.6 28.3 21.6 10.0 25.9 16.0 48.8 22.0 58.0 29.0 12.9 35.0 28.7 29.0 28.5 23.1
Llama 3 70B Instruct 26.9 70.9 55.7 34.0 63.0 42.0 78.4 21.0 87.3 30.0 37.4 55.1 12.2 78.0 46.5 48.1
Mistral Large 26.8 74.3 **73.4** 33.0 **83.6** 50.0 84.3 31.0 92.0 38.0 36.1 49.5 31.1 77.0 54.5 50.4
Mixtral 8x22B MoE 26.6 54.7 46.2 30.0 49.5 40.0 80.5 28.0 90.2 33.0 42.0 52.4 37.5 55.0 48.3 41.6
o1-mini 31.2 76.4 65.9 56.0 68.9 65.0 79.3 31.0 84.6 39.0 41.5 56.4 69.0 77.0 57.9 57.5
o1-preview **42.7** 81.6 71.1 **81.0** 78.4 **91.0** **89.4** **57.0** 93.2 **62.0** 48.0 **70.8** 72.4 **89.0** **65.8** **74.9**


Table 4: Logic form accuracy for _goal interpretation_ (%). Full results in Table 9.


**State Goal** **Relation Goal** **Action Goal** **Overall**

**Model** _Precision_ _Recall_ _F1_ _Precision_ _Recall_ _F1_ _Precision_ _Recall_ _F1_ _Precision_ _Recall_ _F1_

_V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_

Claude-3.5 Sonnet 25.3 74.0 **60.9** 94.8 35.8 83.1 31.1 **84.4** **63.8** 81.3 41.8 **82.9** 14.0 - **98.8** - 24.5 - 21.7 **81.1** **69.6** 84.4 33.0 **82.7**

Gemini 1.5 Pro **45.4** **94.0** 49.1 92.8 **47.2** **93.4** 40.0 74.4 9.7 76.7 15.6 75.6 **26.8** - 80.9 - **40.3** - **35.2** 78.8 41.1 80.4 37.9 79.6

GPT-4o 29.0 67.1 60.0 94.8 39.1 78.6 31.5 81.1 43.6 78.5 36.6 79.8 20.5 - 85.8 - 33.1 - 26.4 76.5 59.1 82.2 36.5 79.2

Llama 3 70B 23.9 69.5 61.2 **95.4** 34.3 80.4 22.6 70.0 37.5 73.3 28.2 71.6 11.2 - 88.8 - 19.8 - 17.5 64.7 58.0 78.3 26.9 70.9

o1-mini 26.3 63.8 58.6 90.8 36.3 74.9 30.4 77.3 39.9 76.5 34.5 76.9 13.5 - 56.8 - 21.8 - 22.4 73.3 51.3 79.8 31.2 76.4

o1-preview 28.2 66.8 60.3 94.8 38.5 78.4 **44.9** 82.9 62.4 **82.7** **52.2** 82.8 26.0 - 81.5 - 39.5 - 31.8 78.1 65.4 **85.4** **42.7** 81.6




|Col1|Col2|
|---|---|
|**Parsing**|**Parsing**|






















|Col1|Col2|
|---|---|
|**Hallucination**|**Hallucination**|


|LLM Output<br>. . .<br>FIND(television.410)<br>SWITCH_ON(television.410)|Col2|
|---|---|
|||
|**Error Info: State Unsatisfied**|**Error Info: State Unsatisfied**|


|LLM Output<br>. . .<br>FIND(cat.1000)<br>TURN_TO(cat.1000)|Col2|
|---|---|
|||
|**Error Info: Action Unsatisfied**|**Error Info: Action Unsatisfied**|































Figure 5: Examples of different types of errors in trajectory feasibility, logic form parsing (e.g., in subgoals
decomposition and transition modeling), and goal satisfaction rates.


**Reporting Bias and Imprecise Physical Expressions.** Given the task “ _serve a meal_ ”, all LLMs
predict the incorrect goal _ontop_ (chicken, table) instead of _ontop_ (chicken, plate), due to the commonly
used natural language expression “ _put_ _the_ _chicken_ _on_ _the_ _table_ ”. Also, for the task “ _cleaning_
_sneakers_ ”, the goal state _onfloor_ (gym_shoe, floor) is missing from all LLM predictions, as chat
models ignore the _onfloor_ spatial relationship as implicit for conversational language. However, such
precise physical relationships are essential for embodied task planning.


8


Table 5: Goal satisfaction rates (%) for _action_ _sequencing_ and _subgoal_ _decomposition_ . Full results in Appendix E.2. Behavior does not include action goals.


**Action Sequencing** **Subgoal Decomposition**
**State Goal** **Relation Goal** **Action Goal** **Total** **State Goal** **Relation Goal** **Action Goal** **Total**

**Model**

_V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_

Claude-3.5 Sonnet 81.3 63.0 **79.4** 62.4 57.4 - **74.9** 62.6 92.9 41.0 **88.6** 39.5 87.0 - 90.1 39.9
Claude-3 Opus 64.7 45.0 **79.4** 53.0 50.7 - 65.7 50.8 92.4 43.0 **88.6** 41.6 83.3 - 89.1 42.0
Gemini 1.5 Pro 81.7 41.0 77.2 43.2 **60.1** - 75.1 42.6 91.2 31.0 72.5 37.1 89.5 - 83.9 35.4
GPT-4o 82.0 49.0 67.8 45.5 50.7 - 70.1 46.5 92.1 50.0 84.2 53.2 **93.2** - 89.4 52.3
Llama 3 70B 42.8 31.0 64.4 45.5 45.9 - 50.0 41.5 **93.2** 25.0 63.4 27.7 82.7 - 80.0 27.0
o1-mini 75.2 64.0 68.3 66.9 51.4 - 67.3 66.1 89.7 28.0 68.8 38.0 81.5 - 80.3 35.3
o1-preview **86.0** **89.5** 71.1 **84.4** 56.1 - 74.3 **85.8** 91.8 **56.5** 88.3 **69.4** 92.6 - **90.6** **65.9**


Table 6: Trajectory evaluation results (%) for _action sequencing_ and _subgoal decomposition_ . Full results in
Appendix E.3.


**Goal Evaluation** **Trajectory Evaluation**

**Grammar Error (** _↓_ **)** **Runtime Error (** _↓_ **)**
_Task SR_ _Execution SR_

**Model** _Parsing_ _Hallucination_ _Predicate-Arg Num_ _Wrong Order_ _Missing Step_ _Affordance_ _Additional Step_

_V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_
_Action Sequencing_
Claude-3.5 Sonnet 72.8 60.0 75.4 69.0 **0.0** **0.0** 2.3 **0.0** **0.0** **0.0** 1.0 5.0 19.7 25.0 1.6 **1.0** **5.2** 2.0
Claude-3 Opus 66.2 51.0 70.8 59.0 **0.0** **0.0** 14.1 **0.0** **0.0** **0.0** 0.7 3.0 14.1 35.0 **0.3** 3.0 6.2 2.0
Gemini 1.5 Pro **73.1** 42.0 **83.3** 54.0 **0.0** **0.0** 1.6 **0.0** 0.3 **0.0** **0.3** 6.0 13.1 39.0 1.3 **1.0** 5.6 2.0
GPT-4o 61.6 47.0 71.1 53.0 0.3 **0.0** **1.3** 1.0 0.3 **0.0** **0.3** 9.0 25.2 36.0 1.3 **1.0** 4.9 **0.0**
Llama 3 70B 55.7 34.0 63.0 42.0 **0.0** **0.0** 23.3 2.0 1.0 **0.0** 2.0 15.0 **7.9** 38.0 3.0 3.0 7.9 6.0
o1-mini 65.9 56.0 68.9 65.0 0.3 **0.0** 5.2 3.0 3.3 **0.0** **0.3** 7.0 21.6 17.0 0.3 6.0 5.9 5.0
o1-preview 71.1 **81.0** 78.4 **91.0** 2.0 **0.0** 8.2 **0.0** **0.0** **0.0** **0.3** **0.0** 34.1 **6.0** **0.3** 2.0 8.9 3.0
_Subgoal Decomposition_
Claude-3.5 Sonnet 89.1 39.0 92.0 44.0 **0.0** **0.0** 1.8 1.0 **0.0** **0.0** 1.5 11.0 2.7 44.0 2.1 **0.0** 24.6 4.0
Claude-3 Opus 87.0 39.0 89.9 47.0 0.3 **0.0** 3.3 3.0 **0.0** **0.0** 1.2 5.0 3.0 45.0 2.4 **0.0** 16.0 5.0
Gemini 1.5 Pro 87.0 31.0 91.1 37.0 **0.0** 1.0 **1.5** **0.0** 1.8 1.0 **0.0** **3.0** 5.6 59.0 **0.0** **0.0** 16.0 2.0
GPT-4o 88.8 48.0 90.2 55.0 **0.0** **0.0** 6.2 3.0 **0.0** **0.0** 1.2 5.0 **2.4** 37.0 **0.0** **0.0** 15.7 5.0
Llama 3 70B 78.4 20.0 87.3 30.0 **0.0** 1.0 2.4 5.0 0.9 1.0 2.4 8.0 5.3 51.0 1.8 4.0 20.4 4.0
o1-mini 79.3 31.0 84.6 39.0 **0.0** **0.0** **1.5** 3.0 0.6 3.0 0.3 7.0 8.9 46.0 4.1 2.0 21.9 **1.0**
o1-preview **89.4** **57.0** **93.2** **62.0** **0.0** 2.0 **1.5** 3.0 **0.0** **0.0** 0.3 5.0 2.7 **25.0** 2.4 3.0 **12.1** 7.0


**4.1** **Ability Module Analysis**


**Goal Interpretation.** LLMs generally have difficulties distinguishing intermediate subgoals and
final goals. For example, in the VirtualHome task _Drink_, GPT-4o predicts some intermediate states as
part of the final goal (e.g., _open_ (freezer) and _inside_ (water, glass)). Overall, we observe that LLMs
tend to translate NL goals word-by-word into their symbolic correspondence, rather than grounding
them in the environment state. More analyses are in Appendix E.1.


**Subgoal** **Decomposition** **and** **Action** **Sequencing** **on** **Trajectory** **Feasibility.** Most errors are
runtime errors (rather than syntax errors). We illustrate examples in Figure 5. Overall, LLMs are
more likely to make missing-step and additional-step errors than wrong-order or affordance errors.
Missing-step errors occur when a precondition is not satisfied before the execution of an action (e.g.,
fetching an object without opening the box containing it). Additional steps form the most frequent
errors, even for the most powerful models—it occurs when a goal has already been achieved but the
model still predicts to execute an additional action to achieve it (e.g., opening a box twice). More
analysis is in Appendix E.3.


**Subgoal Decomposition and Action Sequencing on Goal Satisfaction Rates.** Shown in Table
5, object goals (such as _toggled_on_ ) are generally easier to achieve than relational goals (such as
_ontop_ (agent, chair)). More analysis is provided in Appendix E.2.


**Transition** **Modeling.** Table 7 shows the overall performance of the logic form accuracy. For a
systematic evaluation, we further categorize the tasks into five distinct ability categories requiring
the transition modeling for different types of object states and relations (see Appendix F.3). Overall,
we reveal significant variations in performance across different models; relational preconditions and
effects are generally harder to predict than object-state ones. For instance, the Claude-3 Opus model
excelled in object states (63% on VirtualHome), but its performance in spatial relations is weak.
Additionally, in tasks that focus on object properties, models generally perform poorly in reasoning
about object orientation (e.g., the agent should be facing the TV to watch it). We also provide a
sensitivity analysis tool to visualize how different transition modeling errors result in downstream


9


Table 7: Logic form accuracy (F1) and planner success rate (SR) for _transition modeling_ (%). Full results in
Appendix E.4.


**Object States** **Object Orientation** **Object Affordance** **Spatial Relations** **Non-Spatial Relations**

**Model** _F1_ _SR_ _F1_ _SR_ _F1_ _SR_ _F1_ _SR_ _F1_ _SR_

_V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_

Claude-3.5 Sonnet 60.5 **78.8** 67.4 **86.7** **95.3** - 96.4 - 76.6 - 67.7 - **42.4** **58.6** **96.6** 80.9 5.9 73.6 **91.9** 80.3
Claude-3 Opus **63.0** 71.9 63.5 84.4 62.6 - 71.4 - 75.5 - 58.7 - 38.7 54.6 64.8 80.9 7.0 68.8 55.4 82.0
Gemini 1.5 Pro 18.8 55.9 **94.4** 35.6 90.9 - 89.3 - **77.7** - **95.8** - 38.7 35.9 89.0 40.4 7.8 52.8 83.8 39.3
GPT-4o 54.6 71.3 71.9 68.9 52.8 - 78.6 - 74.9 - 63.5 - 40.8 45.9 66.9 64.9 7.5 73.0 68.9 68.9
Llama-3 70B 32.5 66.3 10.1 68.9 56.6 - 3.6 - 57.0 - 6.6 - 27.0 47.2 15.2 77.7 3.0 58.9 18.9 85.2
o1-mini 59.0 41.3 63.5 77.8 56.3 - 82.1 - 58.5 - 59.3 - 32.5 53.1 75.9 77.7 4.5 67.5 71.6 75.4
o1-preview 58.5 78.3 69.1 **86.7** 78.4 - **100.0** - 77.5 - 67.1 - 38.8 56.3 76.6 **89.4** **11.8** **83.5** 78.4 **90.2**


planning errors (see Appendix F and E.4). We found that LLMs tend to overstate object states in
effects while understating them in preconditions. Conversely, they overstate spatial relationships in
preconditions and understate them in effects. As a result, in many cases, even if the downstream
planner successfully generates a plan, it may not be feasible in the actual environment.


**Implications in Embodied Agent System Design.** We further investigate the potential integration of
LLM-based ability modules and their robustness through **sensitive analysis** (Appendix F), **modular-**
**ized vs pipeline-based** experiments (Appendix G), and **replanning** (Appendix H). We observe that
trajectory feasibilities are similar, although with error accumulation from different module compositions, showing the potential of module composition. We have also compared different **prompting**
**strategies** for embodied decision-making tasks, and summarize the best practices in Appendix I.


**5** **Related Work**


Recent work in embodied decision making has been using LLMs to perform various tasks, and we
include a comprehensive summary in Appendix P, see also Table 8 for a quick summary. LLMs can
also be used to combine multiple of the above modules at once via chain-of-thought prompting or
pipelined queries, such as goal interpretation with action sequencing [13–32], goal interpretation with
subgoal decomposition [2, 27, 33], action sequencing with subgoal decomposition [27, 34, 18, 35],
action sequencing with transition modeling [8, 28, 32, 36, 37, 13, 38]. Our work aims to standardize
the interface between LLMs and various decision-making modules to support the seamless integration,
modular evaluation, and fine-grained metrics, aiming to provide implications on using LLMs in
embodied decision making more effectively and selectively. We provide additional related work on
agent interfaces [39–43, 18, 44, 42, 45] and simulation benchmarks in Appendix P.


Table 8: Existing work in leveraging LLMs for embodied agents.


**Goal Interpretation** **Subgoal Decomposition** **Action Sequencing** **Transition Modeling**

[2, 7, 46, 47, 21, 48, 22–25, 27–32, 13–15, 49–53] [2, 27, 34, 18, 33, 35, 54, 55] [6, 8, 35, 56–59, 16, 43, 60, 17, 19, 61, 20, 42, 62, 14, 63–66, 3, 15, 45, 67–69] [8, 28, 32, 36, 70, 37, 13, 38]


**6** **Conclusions and Future Work**


We propose a systematic evaluation framework EMBODIED AGENT INTERFACE to benchmark LLMs
for embodied decision-making. It focuses on 1) standardizing goal specifications using LTL formulas,
2) unifying decision-making tasks through a standard interface and four fundamental ability modules,
and 3) providing comprehensive fine-grained evaluation metrics and automatic error identification.
We highlight the limitations of current LLMs in interpreting complex goals and different errors in
reasoning, further attributing errors to various cofactors, including trajectory length, goal complexity,
spatial relation goals, etc.


**Limitations and future work:** Our current evaluation is limited to states, actions, and goals that
can be described in abstract language terms, with the input environment abstracted by relational
graphs of objects. Future work should extend this to include sensory inputs and actuation outputs,
possibly by extending the studied model class to include Vision-Language Models (VLMs), which
we discuss further in Appendix K. Other aspects of extension include the integration of memory
systems (episodic memory and state memory), geometric reasoning, and navigation.


**Acknowledgments and Disclosure of Funding**


This work was in part supported by the Stanford Institute for Human-Centered Artificial Intelligence
(HAI), NSF CCRI #2120095, AFOSR YIP FA9550-23-1-0127, ONR MURI N00014-22-1-2740,
ONR YIP N00014-24-1-2117, Amazon, and Microsoft.


10


**References**


[1] Jimmy Wu, Rika Antonova, Adam Kan, Marion Lepert, Andy Zeng, Shuran Song, Jeannette Bohg,
Szymon Rusinkiewicz, and Thomas Funkhouser. Tidybot: Personalized robot assistance with large
language models. _Autonomous Robots_, 47(8):1087–1102, 2023. 1


[2] Michael Ahn, Anthony Brohan, Noah Brown, Yevgen Chebotar, Omar Cortes, Byron David, Chelsea Finn,
Keerthana Gopalakrishnan, Karol Hausman, Alexander Herzog, Daniel Ho, Jasmine Hsu, Julian Ibarz,
Brian Ichter, Alex Irpan, Eric Jang, Rosario Jauregui Ruano, Kyle Jeffrey, Sally Jesmonth, Nikhil Jayant
Joshi, Ryan C. Julian, Dmitry Kalashnikov, Yuheng Kuang, Kuang-Huei Lee, Sergey Levine, Yao Lu,
Linda Luu, Carolina Parada, Peter Pastor, Jornell Quiambao, Kanishka Rao, Jarek Rettinghouse, Diego M
Reyes, Pierre Sermanet, Nicolas Sievers, Clayton Tan, Alexander Toshev, Vincent Vanhoucke, F. Xia, Ted
Xiao, Peng Xu, Sichun Xu, and Mengyuan Yan. Do as i can, not as i say: Grounding language in robotic
affordances. In _Conference on Robot Learning_, 2022. 2, 10, 96


[3] Wenlong Huang, P. Abbeel, Deepak Pathak, and Igor Mordatch. Language models as zero-shot planners:
Extracting actionable knowledge for embodied agents. _ArXiv_, abs/2201.07207, 2022. 1, 10, 96


[4] Sanjana Srivastava, Chengshu Li, Michael Lingelbach, Roberto Martín-Martín, Fei Xia, Kent Elliott
Vainio, Zheng Lian, Cem Gokmen, Shyamal Buch, Karen Liu, et al. Behavior: Benchmark for everyday
household activities in virtual, interactive, and ecological environments. In _Conference on robot learning_,
pages 477–490. PMLR, 2022. 1, 3, 28, 88, 100, 103


[5] Xavier Puig, Kevin Ra, Marko Boben, Jiaman Li, Tingwu Wang, Sanja Fidler, and Antonio Torralba.
Virtualhome: Simulating household activities via programs. In _Proceedings of the IEEE conference on_
_computer vision and pattern recognition_, pages 8494–8502, 2018. 1, 3, 28, 82, 89, 97, 100


[6] Jacky Liang, Wenlong Huang, F. Xia, Peng Xu, Karol Hausman, Brian Ichter, Peter R. Florence, and
Andy Zeng. Code as policies: Language model programs for embodied control. _2023 IEEE International_
_Conference on Robotics and Automation (ICRA)_, pages 9493–9500, 2022. 2, 10, 79, 96


[7] B. Liu, Yuqian Jiang, Xiaohan Zhang, Qian Liu, Shiqi Zhang, Joydeep Biswas, and Peter Stone. Llm+p:
Empowering large language models with optimal planning proficiency. _ArXiv_, abs/2304.11477, 2023. 2,
10, 96


[8] Li Siang Wong, Jiayuan Mao, Pratyusha Sharma, Zachary S. Siegel, Jiahai Feng, Noa Korneev, Joshua B
Tenenbaum, and Jacob Andreas. Learning adaptive planning representations with natural language
guidance. _ArXiv_, abs/2312.08566, 2023. 2, 10, 96


[9] Jae-Woo Choi, Youngwoo Yoon, Hyobin Ong, Jaehong Kim, and Minsu Jang. Lota-bench: Benchmarking
language-oriented task planners for embodied agents. _arXiv preprint arXiv:2402.08178_, 2024. 3, 22, 63,
97


[10] Amir Pnueli. The temporal logic of programs. In _18th Annual Symposium on Foundations of Computer_
_Science (sfcs 1977)_, pages 46–57, 1977. 4


[11] Richard E Fikes and Nils J Nilsson. STRIPS: A New Approach to the Application of Theorem Proving to
Problem Solving. _Artificial Intelligence_, 2(3-4):189–208, 1971. 6


[12] Malte Helmert. The fast downward planning system. _Journal of Artificial Intelligence Research_, 26:191–
246, 2006. 6


[13] Huaxiaoyue Wang, Gonzalo Gonzalez-Pumariega, Yash Sharma, and Sanjiban Choudhury. Demo2code:
From summarizing demonstrations to synthesizing code via extended chain-of-thought. _ArXiv_,
abs/2305.16744, 2023. 10, 96


[14] Boyi Li, Philipp Wu, Pieter Abbeel, and Jitendra Malik. Interactive task planning with language models.
_ArXiv_, abs/2310.10645, 2023. 10, 96


[15] Krishan Rana, Jesse Haviland, Sourav Garg, Jad Abou-Chakra, Ian D. Reid, and Niko Sünderhauf.
Sayplan: Grounding large language models using 3d scene graphs for scalable task planning. In
_Conference on Robot Learning_, 2023. 10, 96


[16] Mengdi Xu, Peide Huang, Wenhao Yu, Shiqi Liu, Xilun Zhang, Yaru Niu, Tingnan Zhang, Fei Xia, Jie
Tan, and Ding Zhao. Creative robot tool use with large language models. _ArXiv_, abs/2310.13065, 2023.
10, 97


[17] Yuchen Liu, Luigi Palmieri, Sebastian Koch, Ilche Georgievski, and Marco Aiello. Delta: Decomposed
efficient long-term robot task planning using large language models. _ArXiv_, abs/2404.03275, 2024. 10, 97


11


[18] Yongchao Chen, Jacob Arkin, Yang Zhang, Nicholas A. Roy, and Chuchu Fan. Autotamp: Autoregressive
task and motion planning with llms as translators and checkers. _ArXiv_, abs/2306.06531, 2023. 10, 96, 97


[19] Zhe Ni, Xiao-Xin Deng, Cong Tai, Xin-Yue Zhu, Xiang Wu, Y. Liu, and Long Zeng. Grid: Scene-graphbased instruction-driven robotic task planning. _ArXiv_, abs/2309.07726, 2023. 10, 97


[20] Yike Wu, Jiatao Zhang, Nan Hu, LanLing Tang, Guilin Qi, Jun Shao, Jie Ren, and Wei Song. Mldt: Multilevel decomposition for complex long-horizon robotic task planning with open-source large language
model. _ArXiv_, abs/2403.18760, 2024. 10, 97


[21] Wenlong Huang, F. Xia, Ted Xiao, Harris Chan, Jacky Liang, Peter R. Florence, Andy Zeng, Jonathan
Tompson, Igor Mordatch, Yevgen Chebotar, Pierre Sermanet, Noah Brown, Tomas Jackson, Linda Luu,
Sergey Levine, Karol Hausman, and Brian Ichter. Inner monologue: Embodied reasoning through
planning with language models. In _Conference on Robot Learning_, 2022. 10, 96


[22] Rishi Hazra, Pedro Zuidberg Dos Martires, and Luc De Raedt. Saycanpay: Heuristic planning with large
language models using learnable domain knowledge. _ArXiv_, abs/2308.12682, 2023. 10, 97


[23] Frank Joublin, Antonello Ceravola, Pavel Smirnov, Felix Ocker, Joerg Deigmoeller, Anna Belardinelli,
Chao Wang, Stephan Hasler, Daniel Tanneberg, and Michael Gienger. Copal: Corrective planning of
robot actions with large language models. _ArXiv_, abs/2310.07263, 2023. 97


[24] Lihan Zha, Yuchen Cui, Li-Heng Lin, Minae Kwon, Montse Gonzalez Arenas, Andy Zeng, Fei Xia, and
Dorsa Sadigh. Distilling and retrieving generalizable knowledge for robot manipulation via language
corrections. _ArXiv_, abs/2311.10678, 2023. 97


[25] Wenlong Huang, Fei Xia, Dhruv Shah, Danny Driess, Andy Zeng, Yao Lu, Pete Florence, Igor Mordatch,
Sergey Levine, Karol Hausman, and Brian Ichter. Grounded decoding: Guiding text generation with
grounded models for embodied agents. In _Neural Information Processing Systems_, 2023. 10, 97


[26] Yu Zhou, Sha Li, Manling Li, Xudong Lin, Shih-Fu Chang, Mohit Bansal, and Heng Ji. Non-sequential
graph script induction via multimedia grounding. _arXiv preprint arXiv:2305.17542_, 2023.


[27] Xizhou Zhu, Yuntao Chen, Hao Tian, Chenxin Tao, Weijie Su, Chenyu Yang, Gao Huang, Bin Li, Lewei
Lu, Xiaogang Wang, Y. Qiao, Zhaoxiang Zhang, and Jifeng Dai. Ghost in the minecraft: Generally
capable agents for open-world environments via large language models with text-based knowledge and
memory. _ArXiv_, abs/2305.17144, 2023. 10, 97


[28] L. Guan, Karthik Valmeekam, Sarath Sreedharan, and Subbarao Kambhampati. Leveraging pre-trained
large language models to construct and utilize world models for model-based task planning. _ArXiv_,
abs/2305.14909, 2023. 10, 97


[29] Naoki Wake, Atsushi Kanehira, Kazuhiro Sasabuchi, Jun Takamatsu, and Katsushi Ikeuchi. Chatgpt
empowered long-step robot control in various environments: A case application. _IEEE Access_, 11:95060–
95078, 2023. 97


[30] Zhenyu Wu, Ziwei Wang, Xiuwei Xu, Jiwen Lu, and Haibin Yan. Embodied task planning with large
language models. _ArXiv_, abs/2307.01848, 2023. 97


[31] Shu Wang, Muzhi Han, Ziyuan Jiao, Zeyu Zhang, Yingnian Wu, Song-Chun Zhu, and Hangxin Liu.
Llm3: Large language model-based task and motion planning with motion failure reasoning. _ArXiv_,
abs/2403.11552, 2024. 97


[32] Pavel Smirnov, Frank Joublin, Antonello Ceravola, and Michael Gienger. Generating consistent pddl
domains with large language models. _ArXiv_, abs/2404.07751, 2024. 10, 96


[33] Chan Hee Song, Jiaman Wu, Clay Washington, Brian M. Sadler, Wei-Lun Chao, and Yu Su. Llmplanner: Few-shot grounded planning for embodied agents with large language models. _2023 IEEE/CVF_
_International Conference on Computer Vision (ICCV)_, pages 2986–2997, 2022. 10, 97


[34] Kolby Nottingham, Prithviraj Ammanabrolu, Alane Suhr, Yejin Choi, Hannaneh Hajishirzi, Sameer
Singh, and Roy Fox. Do embodied agents dream of pixelated sheep?: Embodied decision making using
language guided world modelling. In _International Conference on Machine Learning_, 2023. 10, 96


[35] Guanzhi Wang, Yuqi Xie, Yunfan Jiang, Ajay Mandlekar, Chaowei Xiao, Yuke Zhu, Linxi (Jim) Fan,
and Anima Anandkumar. Voyager: An open-ended embodied agent with large language models. _ArXiv_,
abs/2305.16291, 2023. 10, 79, 96


12


[36] S. Sundar Raman, Vanya Cohen, Eric Rosen, Ifrah Idrees, David Paulius, and Stefanie Tellex. Cape:
Corrective actions from precondition errors using large language models. In _ICRA_, 2022. 10, 97


[37] Ishika Singh, Valts Blukis, Arsalan Mousavian, Ankit Goyal, Danfei Xu, Jonathan Tremblay, Dieter
Fox, Jesse Thomason, and Animesh Garg. Progprompt: Generating situated robot task plans using large
language models. _2023_ _IEEE_ _International_ _Conference_ _on_ _Robotics_ _and_ _Automation_ _(ICRA)_, pages
11523–11530, 2022. 10, 97


[38] Zhaoyi Li, Kelin Yu, Shuo Cheng, and Danfei Xu. LEAGUE++: EMPOWERING CONTINUAL ROBOT
LEARNING THROUGH GUIDED SKILL ACQUISITION WITH LARGE LANGUAGE MODELS. In
_ICLR 2024 Workshop on Large Language Model (LLM) Agents_, 2024. 10, 97


[39] Georgios Fainekos, Hadas Kress-Gazit, and George Pappas. Temporal logic motion planning for mobile
robots. _Proceedings of the 2005 IEEE International Conference on Robotics and Automation_, pages
2020–2025, 2005. 10, 97


[40] Hadas Kress-Gazit, Georgios Fainekos, and George Pappas. Temporal-logic-based reactive mission and
motion planning. _IEEE Transactions on Robotics_, 25:1370–1381, 2009.


[41] Stephen L. Smith, Jana Tumova, Calin A. Belta, and Daniela Rus. Optimal path planning for surveillance
with temporal-logic constraints*. _The International Journal of Robotics Research_, 30:1695 – 1708, 2011.
97


[42] A. Mavrogiannis, Christoforos Mavrogiannis, and Yiannis Aloimonos. Cook2ltl: Translating cooking
recipes to ltl formulae using large language models. _ArXiv_, abs/2310.00163, 2023. 10, 96, 97


[43] J. Wang, Jiaming Tong, Kai Liang Tan, Yevgeniy Vorobeychik, and Yiannis Kantaros. Conformal
temporal logic planning using large language models: Knowing when to do what and when to ask for
help. _ArXiv_, abs/2309.10092, 2023. 10, 97


[44] Amir Pnueli. The temporal logic of programs. _18th Annual Symposium on Foundations of Computer_
_Science (sfcs 1977)_, pages 46–57, 1977. 10, 97


[45] Wenqi Zhang, Ke Tang, Hai Wu, Mengna Wang, Yongliang Shen, Guiyang Hou, Zeqi Tan, Peng
Li, Yueting Zhuang, and Weiming Lu. Agent-pro: Learning to evolve via policy-level reflection and
optimization, 2024. 10


[46] Yan Ding, Xiaohan Zhang, Chris Paxton, and Shiqi Zhang. Task and motion planning with large language
models for object rearrangement. _2023 IEEE/RSJ International Conference on Intelligent Robots and_
_Systems (IROS)_, pages 2086–2092, 2023. 10, 96


[47] Yaqi Xie, Chenyao Yu, Tongyao Zhu, Jinbin Bai, Ze Gong, and Harold Soh. Translating natural language
to planning goals with large-language models. _ArXiv_, abs/2302.05128, 2023. 10, 97


[48] Kevin Lin, Christopher Agia, Toki Migimatsu, Marco Pavone, and Jeannette Bohg. Text2motion: from
natural language instructions to feasible plans. _Autonomous Robots_, 47:1345 – 1365, 2023. 10, 96


[49] Zeyuan Yang, Jiageng Liu, Peihao Chen, Anoop Cherian, Tim K Marks, Jonathan Le Roux, and Chuang
Gan. Rila: Reflective and imaginative language agent for zero-shot semantic audio-visual navigation. 10


[50] Yang Zhang, Shixin Yang, Chenjia Bai, Fei Wu, Xiu Li, Zhen Wang, and Xuelong Li. Towards efficient
llm grounding for embodied multi-agent collaboration, 2024.


[51] Xudong Guo, Kaixuan Huang, Jiale Liu, Wenhui Fan, Natalia Vélez, Qingyun Wu, Huazheng Wang,
Thomas L. Griffiths, and Mengdi Wang. Embodied llm agents learn to cooperate in organized teams,
2024.


[52] Hongxin Zhang, Weihua Du, Jiaming Shan, Qinhong Zhou, Yilun Du, Joshua B Tenenbaum, Tianmin
Shu, and Chuang Gan. Building cooperative embodied agents modularly with large language models. In
_The Twelfth International Conference on Learning Representations_, 2023.


[53] Yue Wu, Xuan Tang, Tom M. Mitchell, and Yuanzhi Li. Smartplay: A benchmark for llms as intelligent
agents, 2024. 10


[54] Enshen Zhou, Yiran Qin, Zhenfei Yin, Yuzhou Huang, Ruimao Zhang, Lu Sheng, Yu Qiao, and Jing Shao.
Minedreamer: Learning to follow instructions via chain-of-imagination for simulated-world control, 2024.
10


13


[55] Zihao Wang, Shaofei Cai, Guanzhou Chen, Anji Liu, Xiaojian Ma, and Yitao Liang. Describe, explain,
plan and select: Interactive planning with large language models enables open-world multi-task agents.
_arXiv preprint arXiv:2302.01560_, 2023. 10


[56] Tom Silver, Soham Dan, Kavitha Srinivas, Joshua B. Tenenbaum, Leslie Pack Kaelbling, and Michael
Katz. Generalized planning in pddl domains with pretrained large language models. In _AAAI Conference_
_on Artificial Intelligence_, 2023. 10, 97


[57] Shibo Hao, Yi Gu, Haodi Ma, Joshua Jiahua Hong, Zhen Wang, Daisy Zhe Wang, and Zhiting Hu.
Reasoning with language model is planning with world model. _ArXiv_, abs/2305.14992, 2023. 97


[58] Jacky Liang, Fei Xia, Wenhao Yu, Andy Zeng, Montse Gonzalez Arenas, Maria Attarian, Maria Bauza,
Matthew Bennice, Alex Bewley, Adil Dostmohamed, Chuyuan Fu, Nimrod Gileadi, Marissa Giustina,
Keerthana Gopalakrishnan, Leonard Hasenclever, Jan Humplik, Jasmine Hsu, Nikhil Joshi, Ben Jyenis,
Chase Kew, Sean Kirmani, Tsang-Wei Edward Lee, Kuang-Huei Lee, Assaf Hurwitz Michaely, Joss
Moore, Kenneth Oslund, Dushyant Rao, Allen Z. Ren, Baruch Tabanpour, Quan Ho Vuong, Ayzaan
Wahid, Ted Xiao, Ying Xu, Vincent Zhuang, Peng Xu, Erik Frey, Ken Caluwaerts, Ting-Yu Zhang, Brian
Ichter, Jonathan Tompson, Leila Takayama, Vincent Vanhoucke, Izhak Shafran, Maja Mataric, Dorsa
Sadigh, Nicolas Manfred Otto Heess, Kanishka Rao, Nik Stewart, Jie Tan, and Carolina Parada. Learning
to learn faster from human feedback with language model predictive control. _ArXiv_, abs/2402.11450,
2024. 97


[59] Yongchao Chen, Jacob Arkin, Yilun Hao, Yang Zhang, Nicholas Roy, and Chuchu Fan. Prompt optimization in multi-step tasks (promst): Integrating human feedback and preference alignment. _ArXiv_,
abs/2402.08702, 2024. 10, 97


[60] Yingdong Hu, Fanqi Lin, Tong Zhang, Li Yi, and Yang Gao. Look before you leap: Unveiling the power
of gpt-4v in robotic vision-language planning. _ArXiv_, abs/2311.17842, 2023. 10, 97


[61] Georgia Chalvatzaki, Ali Younes, Daljeet Nandha, An T. Le, Leonardo F. R. Ribeiro, and Iryna Gurevych.
Learning to reason over scene graphs: a case study of finetuning gpt-2 into a robot language model for
grounded task planning. _Frontiers in Robotics and AI_, 10, 2023. 10, 97


[62] Mandi Zhao, Shreeya Jain, and Shuran Song. Roco: Dialectic multi-robot collaboration with large
language models. _ArXiv_, abs/2307.04738, 2023. 10, 96


[63] Huaxiaoyue Wang, K. Kedia, Juntao Ren, Rahma Abdullah, Atiksh Bhardwaj, Angela Chao, Kelly Y
Chen, Nathaniel Chin, Prithwish Dan, Xinyi Fan, Gonzalo Gonzalez-Pumariega, Aditya Kompella,
Maximus Adrian Pace, Yash Sharma, Xiangwan Sun, Neha Sunkara, and Sanjiban Choudhury. Mosaic:
A modular system for assistive and interactive cooking. _ArXiv_, abs/2402.18796, 2024. 10, 96


[64] Murtaza Dalal, Tarun Chiruvolu, Devendra Singh Chaplot, and Ruslan Salakhutdinov. Plan-seq-learn:
Language model guided rl for solving long horizon robotics tasks. In _ICLR_, 2024. 96


[65] Meenal Parakh, Alisha Fong, Anthony Simeonov, Abhishek Gupta, Tao Chen, and Pulkit Agrawal.
Lifelong robot learning with human assisted language planners. _arXiv:2309.14321_, 2023. 96


[66] Zeyi Liu, Arpit Bahety, and Shuran Song. Reflect: Summarizing robot experiences for failure explanation
and correction. _ArXiv_, abs/2306.15724, 2023. 10, 96


[67] Yiran Qin, Enshen Zhou, Qichang Liu, Zhenfei Yin, Lu Sheng, Ruimao Zhang, Yu Qiao, and Jing Shao.
Mp5: A multi-modal open-ended embodied system in minecraft via active perception, 2024. 10


[68] Qinhong Zhou, Sunli Chen, Yisong Wang, Haozhe Xu, Weihua Du, Hongxin Zhang, Yilun Du, Joshua B
Tenenbaum, and Chuang Gan. Hazard challenge: Embodied decision making in dynamically changing
environments. _arXiv preprint arXiv:2401.12975_, 2024.


[69] Zhonghan Zhao, Wenhao Chai, Xuan Wang, Li Boyi, Shengyu Hao, Shidong Cao, Tian Ye, Jenq-Neng
Hwang, and Gaoang Wang. See and think: Embodied agent in virtual environment, 2023. 10


[70] Yan Ding, Xiaohan Zhang, S. Amiri, Nieqing Cao, Hao Yang, Andy Kaminski, Chad Esselink, and Shiqi
Zhang. Integrating action knowledge and llms for task planning and situation handling in open worlds.
_Autonomous Robots_, 47:981 – 997, 2023. 10, 96


[71] Mohit Shridhar, Jesse Thomason, Daniel Gordon, Yonatan Bisk, Winson Han, Roozbeh Mottaghi, Luke
Zettlemoyer, and Dieter Fox. ALFRED: A Benchmark for Interpreting Grounded Instructions for
Everyday Tasks. In _The IEEE Conference on Computer Vision and Pattern Recognition (CVPR)_, 2020.
26, 97


14


[72] Yuke Zhu, Daniel Gordon, Eric Kolve, Dieter Fox, Li Fei-Fei, Abhinav Gupta, Roozbeh Mottaghi, and
Ali Farhadi. Visual semantic planning using deep successor representations. In _Proceedings of the IEEE_
_international conference on computer vision_, pages 483–492, 2017.


[73] Te-Lin Wu, Yu Zhou, and Nanyun Peng. Localizing active objects from egocentric vision with symbolic
world knowledge. _arXiv preprint arXiv:2310.15066_, 2023.


[74] De-An Huang, Suraj Nair, Danfei Xu, Yuke Zhu, Animesh Garg, Li Fei-Fei, Silvio Savarese, and
Juan Carlos Niebles. Neural task graphs: Generalizing to unseen tasks from a single video demonstration.
In _Proceedings of the IEEE/CVF conference on computer vision and pattern recognition_, pages 8565–
8574, 2019. 26


[75] Richard Bellman. A markovian decision process. _Indiana University Mathematics Journal_, 1957. 26


[76] Thomas L. Dean and Michael P. Wellman. _Planning and Control_ . 1991. 26


[77] Wenlong Huang, Chen Wang, Ruohan Zhang, Yunzhu Li, Jiajun Wu, and Li Fei-Fei. Voxposer: Composable 3d value maps for robotic manipulation with language models. _arXiv preprint arXiv:2307.05973_,
2023. 34, 79


[78] Wenlong Huang, Chen Wang, Yunzhu Li, Ruohan Zhang, and Li Fei-Fei. Rekep: Spatio-temporal
reasoning of relational keypoint constraints for robotic manipulation. _arXiv preprint arXiv:2409.01652_,
2024. 34


[79] Tom Silver and Rohan Chitnis. Pddlgym: Gym environments from pddl problems. _ArXiv_, abs/2002.06432,
2020. 37


[80] Jason Wei, Xuezhi Wang, Dale Schuurmans, Maarten Bosma, Ed Huai hsin Chi, F. Xia, Quoc Le,
and Denny Zhou. Chain of thought prompting elicits reasoning in large language models. _ArXiv_,
abs/2201.11903, 2022. 39


[81] Zihao Wang, Shaofei Cai, Anji Liu, Xiaojian Ma, and Yitao Liang. Describe, explain, plan and select: Interactive planning with large language models enables open-world multi-task agents. _ArXiv_,
abs/2302.01560, 2023. 63, 97


[82] Marta Skreta, Zihan Zhou, Jia Lin Yuan, Kourosh Darvish, Alán Aspuru-Guzik, and Animesh Garg.
Replan: Robotic replanning with perception and language models, 2024. 63


[83] Percy Liang, Rishi Bommasani, Tony Lee, Dimitris Tsipras, Dilara Soylu, Michihiro Yasunaga, Yian
Zhang, Deepak Narayanan, Yuhuai Wu, Ananya Kumar, Benjamin Newman, Binhang Yuan, Bobby
Yan, Ce Zhang, Christian Cosgrove, Christopher D. Manning, Christopher R’e, Diana Acosta-Navas,
Drew A. Hudson, E. Zelikman, Esin Durmus, Faisal Ladhak, Frieda Rong, Hongyu Ren, Huaxiu Yao, Jue
Wang, Keshav Santhanam, Laurel J. Orr, Lucia Zheng, Mert Yuksekgonul, Mirac Suzgun, Nathan S. Kim,
Neel Guha, Niladri S. Chatterji, O. Khattab, Peter Henderson, Qian Huang, Ryan Chi, Sang Michael
Xie, Shibani Santurkar, Surya Ganguli, Tatsunori Hashimoto, Thomas F. Icard, Tianyi Zhang, Vishrav
Chaudhary, William Wang, Xuechen Li, Yifan Mai, Yuhui Zhang, and Yuta Koreeda. Holistic evaluation
of language models. _Annals of the New York Academy of Sciences_, 1525:140 – 146, 2023. 76, 99


[84] Qinlin Zhao, Jindong Wang, Yixuan Zhang, Yiqiao Jin, Kaijie Zhu, Hao Chen, and Xing Xie. Competeai:
Understanding the competition behaviors in large language model-based agents. In _ICML_, 2024.


[85] Aarohi Srivastava, Abhinav Rastogi, Abhishek Rao, Abu Awal Md Shoeb, Abubakar Abid, Adam Fisch,
Adam R Brown, Adam Santoro, Aditya Gupta, Adrià Garriga-Alonso, et al. Beyond the imitation game:
Quantifying and extrapolating the capabilities of language models. _arXiv preprint arXiv:2206.04615_,
2022. 76


[86] Arjun Majumdar, Anurag Ajay, Xiaohan Zhang, Pranav Putta, Sriram Yenamandra, Mikael Henaff, Sneha
Silwal, Paul Mcvay, Oleksandr Maksymets, Sergio Arnaud, et al. Openeqa: Embodied question answering
in the era of foundation models. In _Proceedings of the IEEE/CVF Conference on Computer Vision and_
_Pattern Recognition_, pages 16488–16498, 2024. 78


[87] Jinming Li, Yichen Zhu, Zhiyuan Xu, Jindong Gu, Minjie Zhu, Xin Liu, Ning Liu, Yaxin Peng, Feifei
Feng, and Jian Tang. Mmro: Are multimodal llms eligible as the brain for in-home robotics? _arXiv_
_preprint arXiv:2406.19693_, 2024.


[88] Abhishek Das, Samyak Datta, Georgia Gkioxari, Stefan Lee, Devi Parikh, and Dhruv Batra. Embodied
question answering. In _Proceedings of the IEEE conference on computer vision and pattern recognition_,
pages 1–10, 2018.


15


[89] Baoxiong Jia, Ting Lei, Song-Chun Zhu, and Siyuan Huang. Egotaskqa: Understanding human tasks in
egocentric videos. _Advances in Neural Information Processing Systems_, 35:3343–3360, 2022.


[90] Yiqiao Jin, Minje Choi, Gaurav Verma, Jindong Wang, and Srijan Kumar. Mm-soc: Benchmarking
multimodal large language models in social media platforms. In _ACL_, 2024.


[91] Min Zhang, Jianye Hao, Xian Fu, Peilong Han, Hao Zhang, Lei Shi, Hongyao Tang, and Yan Zheng.
Mfe-etp: A comprehensive evaluation benchmark for multi-modal foundation models on embodied task
planning. _arXiv preprint arXiv:2407.05047_, 2024. 78


[92] Yunfan Jiang, Agrim Gupta, Zichen Zhang, Guanzhi Wang, Yongqiang Dou, Yanjun Chen, Li Fei-Fei,
Anima Anandkumar, Yuke Zhu, and Linxi Fan. Vima: General robot manipulation with multimodal
prompts. _arXiv preprint arXiv:2210.03094_, 2(3):6, 2022. 79


[93] Andrey Kurenkov, Roberto Martín-Martín, Jeff Ichnowski, Ken Goldberg, and Silvio Savarese. Semantic
and geometric modeling with neural message passing in 3d scene graphs for hierarchical mechanical
search. In _2021 IEEE International Conference on Robotics and Automation (ICRA)_, pages 11227–11233.
IEEE, 2021. 81


[94] Antoni Rosinol, Marcus Abate, Yun Chang, and Luca Carlone. Kimera: an open-source library for
real-time metric-semantic localization and mapping. In _2020 IEEE International Conference on Robotics_
_and Automation (ICRA)_, pages 1689–1696. IEEE, 2020.


[95] Qiao Gu, Ali Kuwajerwala, Sacha Morin, Krishna Murthy Jatavallabhula, Bipasha Sen, Aditya Agarwal,
Corban Rivera, William Paul, Kirsty Ellis, Rama Chellappa, et al. Conceptgraphs: Open-vocabulary
3d scene graphs for perception and planning. In _2024 IEEE International Conference on Robotics and_
_Automation (ICRA)_, pages 5021–5028. IEEE, 2024. 81


[96] Yunhao Ge, Yihe Tang, Jiashu Xu, Cem Gokmen, Chengshu Li, Wensi Ai, Benjamin Jose Martinez,
Arman Aydin, Mona Anvari, Ayush K Chakravarthy, et al. Behavior vision suite: Customizable dataset
generation via simulation. In _Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern_
_Recognition_, pages 22401–22412, 2024. 81


[97] Chengshu Li, Fei Xia, Roberto Martín-Martín, Michael Lingelbach, Sanjana Srivastava, Bokui Shen, Kent
Vainio, Cem Gokmen, Gokul Dharan, Tanish Jain, Andrey Kurenkov, C. Karen Liu, Hyowon Gweon,
Jiajun Wu, Li Fei-Fei, and Silvio Savarese. igibson 2.0: Object-centric simulation for robot learning of
everyday household tasks, 2021. 91


[98] Karthik Valmeekam, Alberto Olmo, Sarath Sreedharan, and Subbarao Kambhampati. Planbench: An
extensible benchmark for evaluating large language models on planning and reasoning about change. In
_Neural Information Processing Systems_, 2022. 97


[99] Karthik Valmeekam, Matthew Marquez, Alberto Olmo, Sarath Sreedharan, and Subbarao Kambhampati.
Planbench: an extensible benchmark for evaluating large language models on planning and reasoning
about change. In _Proceedings of the 37th International Conference on Neural Information Processing_
_Systems_, NIPS ’23, Red Hook, NY, USA, 2024. Curran Associates Inc. 97


[100] Qinhong Zhou, Sunli Chen, Yisong Wang, Haozhe Xu, Weihua Du, Hongxin Zhang, Yilun Du, Joshua B.
Tenenbaum, and Chuang Gan. Hazard challenge: Embodied decision making in dynamically changing
environments, 2024. 98


[101] Michael Hanna, Federico Pedeni, Alessandro Suglia, Alberto Testoni, and Raffaella Bernardi. ACT-thor:
A controlled benchmark for embodied action understanding in simulated environments. In Nicoletta
Calzolari, Chu-Ren Huang, Hansaem Kim, James Pustejovsky, Leo Wanner, Key-Sun Choi, Pum-Mo Ryu,
Hsin-Hsi Chen, Lucia Donatelli, Heng Ji, Sadao Kurohashi, Patrizia Paggio, Nianwen Xue, Seokhwan
Kim, Younggyun Hahm, Zhong He, Tony Kyungil Lee, Enrico Santus, Francis Bond, and Seung-Hoon
Na, editors, _Proceedings_ _of_ _the_ _29th_ _International_ _Conference_ _on_ _Computational_ _Linguistics_, pages
5597–5612, Gyeongju, Republic of Korea, October 2022. International Committee on Computational
Linguistics. 98


**Checklist**


1. For all authors...


(a) Do the main claims made in the abstract and introduction accurately reflect the paper’s
contributions and scope? [Yes] We clearly state our problem scope and contributions.
(b) Did you describe the limitations of your work? [Yes] See Appendix Section S.


16


(c) Did you discuss any potential negative societal impacts of your work? [Yes] See
Appendix Section S.
(d) Have you read the ethics review guidelines and ensured that your paper conforms to
them? [Yes]

2. If you are including theoretical results...


(a) Did you state the full set of assumptions of all theoretical results? [N/A]
(b) Did you include complete proofs of all theoretical results? [N/A]

3. If you ran experiments (e.g. for benchmarks)...


(a) Did you include the code, data, and instructions needed to reproduce the main experimental results (either in the supplemental material or as a URL)? [Yes] All code,
annotations, and instructions for reproducing our results are included in the supplementary materials.
(b) Did you specify all the training details (e.g., data splits, hyperparameters, how they
were chosen)? [Yes]
(c) Did you report error bars (e.g., with respect to the random seed after running experiments multiple times)? [N/A] LLM inference tasks are very resource intensive and
proprietary model APIs are too costly.
(d) Did you include the total amount of compute and the type of resources used (e.g., type
of GPUs, internal cluster, or cloud provider)? [Yes]

4. If you are using existing assets (e.g., code, data, models) or curating/releasing new assets...


(a) If your work uses existing assets, did you cite the creators? [Yes] We cite the existing
assets in the reference.
(b) Did you mention the license of the assets? [Yes] We mention them in our released
website and in the supplemental material.
(c) Did you include any new assets either in the supplemental material or as a URL? [Yes]

In the supplemental material.
(d) Did you discuss whether and how consent was obtained from people whose data you’re
using/curating? [Yes] The resources are from existing public data that is open-access.
(e) Did you discuss whether the data you are using/curating contains personally identifiable
information or offensive content? [Yes] The data we are using does not contain
personally identifiable information or offensive content.

5. If you used crowdsourcing or conducted research with human subjects...


(a) Did you include the full text of instructions given to participants and screenshots, if
applicable? [N/A]
(b) Did you describe any potential participant risks, with links to Institutional Review
Board (IRB) approvals, if applicable? [N/A]
(c) Did you include the estimated hourly wage paid to participants and the total amount
spent on participant compensation? [N/A]


17


# **Appendix**

### **Table of Contents**

**A** **Summary of Empirical Findings** **20**


**B** **Embodied Agent Interface Design** **22**

B.1 Motivation . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 22

B.2 Input and Output Details . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 23

B.3 Grounding to Markov Decision Process . . . . . . . . . . . . . . . . . . . . . . 26

B.4 The Relationship between Ability Modules . . . . . . . . . . . . . . . . . . . . 27


**C** **LTL Representation and Implementation** **28**

C.1 Why LTL . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 28

C.2 Comparision with Traditional LTL Representation . . . . . . . . . . . . . . . . 29

C.3 Syntax and Semantics of LTL Formulas . . . . . . . . . . . . . . . . . . . . . . 29


**D** **Fine-Grained Metrics and Automatic Error Detection** **31**

D.1 Goal Interpretation: State Goal, Relation Goal and Action Goal . . . . . . . . . 31

D.2 Action Sequencing: Trajectory Error Detection for Missing Step, Additional Step,
Wrong Temporal Order, Affordance Error . . . . . . . . . . . . . . . . . . . . . 32

D.3 Subgoal Decomposition: Converting Subgoal Trajectory to Action Trajectory with
BFS Searching . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 34

D.4 Transition Modeling: Evaluating with PDDL Planners . . . . . . . . . . . . . . 36


**E** **Full Results with 18 models** **38**

E.1 Goal Interpretation . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 38

E.2 Subgoal Decomposition . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 40

E.3 Action Sequencing . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 45

E.4 Transition Modeling . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 47

E.5 Correlection with Action Length and Goal Complexity . . . . . . . . . . . . . . 54


**F** **Sensitivity Analysis** **54**

F.1 Motivation and Problem Formulation . . . . . . . . . . . . . . . . . . . . . . . 54

F.2 Implementation Details . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 55

F.3 Result Analysis . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 55


**G** **Pipeline-Based vs Modularized** **61**

G.1 Motivation and Problem Formulation . . . . . . . . . . . . . . . . . . . . . . . 61

G.2 Implementation Details . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 62

G.3 Result Analysis . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 62


**H** **Replanning and Feedback** **63**

H.1 Motivation and Problem Formulation . . . . . . . . . . . . . . . . . . . . . . . 63

H.2 Implementation Details . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 63

H.3 Result Analysis . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 64

H.4 Replanning with Stochastic Actions . . . . . . . . . . . . . . . . . . . . . . . . 64


**I** **Prompt and Analysis** **65**

I.1 Prompt of Goal Interpretation . . . . . . . . . . . . . . . . . . . . . . . . . . . 65

I.2 Prompt of Subgoal Decomposition . . . . . . . . . . . . . . . . . . . . . . . . 66

I.3 Prompt of Action Sequencing . . . . . . . . . . . . . . . . . . . . . . . . . . . 67

I.4 Prompt of Transition Modeling . . . . . . . . . . . . . . . . . . . . . . . . . . 70


18


I.5 Prompt of Environment Representation . . . . . . . . . . . . . . . . . . . . . . 74
I.6 Prompt Analysis and Learned Lessons . . . . . . . . . . . . . . . . . . . . . . 76
I.7 Further Consideration about Prompt Variability . . . . . . . . . . . . . . . . . . 76


**J** **Human Performance Comparison** **78**


**K** **Further Discussion on Visual Information in Our Benchmark** **78**
K.1 Integration of Visual Inputs in Long-Horizon Decision Making . . . . . . . . . . 78
K.2 Impact of Perception and State Estimation Errors . . . . . . . . . . . . . . . . . 80
K.3 Assumptions on Scene Graphs in Our Benchmark . . . . . . . . . . . . . . . . 81


**L** **Dataset Statistics and Analysis** **81**
L.1 Dataset Structure . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 81
L.2 Data Statistics and Distribution . . . . . . . . . . . . . . . . . . . . . . . . . . 83
L.3 Goal Complexity Analysis . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 84
L.4 Task List . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 85
L.5 Task Categorization . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 86


**M** **Annotation Details** **87**
M.1 Simulator Comparison and Selection . . . . . . . . . . . . . . . . . . . . . . . 87
M.2 BEHAVIOR . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 88
M.3 VirtualHome . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 89
M.4 Quality Verification . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 90


**N** **Simulator Implementation Details** **91**
N.1 BEHAVIOR Implementation Details . . . . . . . . . . . . . . . . . . . . . . . 91
N.2 VirtualHome Implementation Details . . . . . . . . . . . . . . . . . . . . . . . 93


**O** **Evaluation Settings of LLMs** **94**
O.1 Decoding Parameters . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 94
O.2 Evaluation Cost . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 94
O.3 Model Cards . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 95


**P** **Extensive Related Work** **96**
P.1 LLMs for Embodied Planning . . . . . . . . . . . . . . . . . . . . . . . . . . . 96
P.2 LTL Agent Interface . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 97
P.3 Embodied Agent Benchmarks . . . . . . . . . . . . . . . . . . . . . . . . . . . 97


**Q** **Maintenance Plan** **98**
Q.1 Dataset URLs, License, and Hosting Plan . . . . . . . . . . . . . . . . . . . . . 98
Q.2 Long-term Preservation and DOI . . . . . . . . . . . . . . . . . . . . . . . . . 98
Q.3 URL of Croissant Metadata Record . . . . . . . . . . . . . . . . . . . . . . . . 98
Q.4 Author Statement . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 98
Q.5 URLs of Code and Re-productivity . . . . . . . . . . . . . . . . . . . . . . . . 98
Q.6 Code for VirtualHome Evaluator and Computation Resources . . . . . . . . . . 98
Q.7 Code for BEHAVIOR Evaluator and Computation Resources . . . . . . . . . . . 99
Q.8 Code for LLMs Implementations and Computation Resources . . . . . . . . . . 99


**R** **Datasheets for EMBODIED AGENT INTERFACE** **(EAI)** **99**


**S** **Impact, Limitations and Future Directions** **105**
S.1 Broader Impact . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 105
S.2 Limitations . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 106
S.3 Potential Negative Social Impact . . . . . . . . . . . . . . . . . . . . . . . . . 106
S.4 Potential Future Directions . . . . . . . . . . . . . . . . . . . . . . . . . . . . 106


19


**A** **Summary of Empirical Findings**


1. **Goal Interpretation:**


       - Most LLMs still struggle to faithfully translate natural language instructions into
grounded states (objects, object states, and relations) in the environment.

       - A common error is generating intermediate goals instead of final goals, e.g., predicting
the state _open_ (freezer) for the task “drinking water”.

       - Another common error is omitting conversationally uncommon spatial relationship
goals. For example, in the task “serving a meal”, with ground truth goal condition
_ontop_ (chicken.0, plate.2) and _ontop_ (plate.2, table.1), GPT-4o mistakenly predicts
_ontop_ (chicken.0, table.1), ignoring the crucial spatial relationship between the chicken,
plate, and table.

       - Gemini 1.5 Pro achieves the highest overall goal interpretation performance (F1-score)
in both VirtualHome and BEHAVIOR simulators, while Claude-3 Opus has the highest
successful ground truth goal retrieval rate (Recall) in both simulators. For example,
in the VirtualHome simulator, Gemini 1.5 Pro achieves an F1-score of 82.0%, and
Claude-3 Opus achieves a Recall of 89.1%.

       - State-of-the-art proprietary LLMs make few to no grammar errors, while top opensource LLMs like Llama 3 70B Instruct suffer more from format/parsing errors and
object/state hallucination. For instance, GPT-4o makes no parsing errors in both
simulators, while Llama 3 8B makes parsing errors in 0.6% of cases in VirtualHome
and 2.0% in BEHAVIOR.


2. **Action Sequencing:**


       - Reasoning ability is a crucial aspect that LLMs should improve. As shown in Fig 3 in
the main paper, trajectory runtime errors are common (41.2%), with a large portion
of missing step (15.5%) and additional step (16.2%) errors, often due to overlooking
preconditions. For instance, LLMs may ignore the agent’s _sitting_ or _lying_ state and
fail to include a _standup_ action before executing other actions. They sometimes also
fail to understand the need to _open_ a _closed_ object before _fetching_ items from inside.
Additional step errors frequently occur when LLMs output actions for previously
achieved goals.

       - In BEHAVIOR, o1-preview leads with the highest task success rate (81.0%) and
execution success rate (91.0%), followed by o1-mini in second place (56.0%, 65.0%).
The best non-o1-series model is GPT-4o (47.0%, 53.0%). Notably and interestingly, in
VirtualHome, Mistral Large (73.4%,83.6%) and Gemini 1.5 Pro (73.1%, 83.3%) both
outperform o1-preview (71.1%, 78.4%).

       - Better LLMs generally make fewer grammar errors compared to less advanced models.
For example, Claude-3 Opus makes no parsing errors in both simulators, while GPT3.5-turbo makes parsing errors in 4.0% of cases in BEHAVIOR.

       - The most common runtime errors are missing steps and wrong order in both simulators.
For instance, in BEHAVIOR, GPT-4o encounters missing step errors in 36.0% of cases
and wrong order errors in 9.0% of cases.

       - LLMs perform better in satisfying state goals than relation goals and struggle with
complex action goals. For example, in VirtualHome, GPT-4o achieves a state goal
success rate of 82.0% but a relation task success rate of 67.8%.

       - Task complexity, including the number of goals, state goals, relation goals, and action
sequence length, adversely affects the task success rate. For instance, in BEHAVIOR,
the success rate drops from around 60% for tasks with fewer than 5 goals to below 40%
for tasks with more than 10 goals.


3. **Subgoal Decomposition:**


       - Subgoal decomposition is not strictly easier than action sequencing in abstract action
spaces.

       - o1-preview demonstrates superior performance in both VirtualHome and BEHAVIOR
simulators compared to other state-of-the-art (SOTA) LLMs, with success rates of
89.4% and 57.0%, respectively. In VirtualHome, Gemini 1.5 Flash and Claude-3.5
Sonnet also exhibit high performance with success rates of 89.1%.


20


    - SOTA models generally avoid grammar errors but can hallucinate actions and objects.
For example, GPT-4o tends to hallucinate the action _POUR_ when dealing with the
task “make coffee” in VirtualHome, which is not defined in the subgoal decomposition
setting.

    - The most common runtime errors differ between simulators: additional steps in VirtualHome and missing steps in BEHAVIOR. For instance, in VirtualHome, all LLMs
are prone to produce additional step errors, even for SOTA LLMs like GPT-4o and
Claude-3 Opus. This is mainly because, in the initial scene state, some of the goals
have already been achieved, yet LLMs still prefer to plan the satisfied goals in their
output.

    - Stronger LLMs like o1-preview show higher accuracy in action task success rates
in VirtualHome compared to weaker models like Llama 3 8B. However, achieving
state and relation goals in BEHAVIOR is challenging due to more complex task
representations and stricter precondition checks. For example, in BEHAVIOR, most
state and relation goals are encapsulated within quantifiers, and quantifiers such as
“forall” or “forpairs” tend to fail if even a single state or relation goal is not met.

    - Overall LLM performance is lower in BEHAVIOR compared to VirtualHome due to
complex task representations involving quantifiers like “forall” and “forpairs”, which
articulate complex temporal and spatial requirements. For instance, most tasks in
BEHAVIOR have quantifiers with complex spatial or temporal requirements, while
VirtualHome tasks have much easier goal definitions.

4. **Transition Modeling:**


    - Models like Claude-3.5 Sonnet and o1-preview excel in specific categories like object
orientation and non-spatial relations, suggesting that targeted training or specialized
architectures enhance LLM capabilities in understanding different types of tasks in
transition modeling. For example, Claude-3.5 Sonnet achieves an F1-score of 78.8%
in object states in BEHAVIOR, while o1-preview achieves an F1-score of 83.5% in
non-spatial relations in BEHAVIOR.

    - Across various models, non-spatial relations consistently pose a challenge, highlighting
a gap in the ability of LLMs to grasp complex relational dynamics. For instance, in
VirtualHome, the best-performing model, o1-preview, only achieves an F1-score of
11.9% in non-spatial relations in VirtualHome.

    - The effectiveness of planning relies heavily on the consistency of the predicted action
space by LLMs; discrepancies between mixed predicted and ground truth actions lead
to reduced planner success. For example, if we mix the action spaces of GPT-4o predictions and ground truth, using “plug_in” from GPT-4o prediction and “walk_toward”
and “switch_on” from ground truth, the PDDL planner cannot find a feasible solution
for the task.

5. **Sensitivity Analysis:**


    - Specific actions like “plug_in” and “walk_towards” consistently show low success rates
due to complex preconditions and spatial requirements. For instance, in VirtualHome,
the success rate for “plug_in” is only 0.09, and for “walk_towards”, it is 0.63.

    - Complex interactions involving detailed object manipulation, such as
“slice_carvingknife” and “place_inside”, present notable challenges. For example, in BEHAVIOR, the success rate for “slice_carvingknife” is 0.00, and for
“place_inside”, it shows a rather low success rate in many tasks.

    - Current training regimens may not fully capture the diversity of real-world interactions,
especially in spatial and object-oriented tasks. This is evident from the generally lower
success rates for actions involving complex spatial relationships and object interactions.

6. **Pipeline-Based vs.** **Modularized:**


    - Both modularized and pipeline-based methods have similar trajectory executable rates.
For example, in the pipeline of Goal Interpretation and Action Sequencing in BEHAVIOR, the modularized method has an execution success rate of 53.0% for GPT-4o,
while the pipeline-based method has an execution success rate of 55.0%.

    - Pipeline-based methods suffer from error accumulation due to the composition of two
modules. For instance, in the pipeline of Goal Interpretation and Subgoal Decom

21


position in BEHAVIOR, the task success rate for GPT-4o drops from 48.0% in the
modularized method to 38.0% in the pipeline-based method.

       - SOTA LLMs generally avoid grammar errors for both pipeline-based and modularized
methods, unlike less advanced models. For example, GPT-4o makes no parsing errors
in both methods, while Llama 3 8B makes parsing errors in 2.0% of cases in the
pipeline-based method.

       - All LLMs, regardless of their advancement, are prone to runtime errors, missing
necessary steps in their generation process. For instance, in the pipeline of Goal
Interpretation and Action Sequencing in BEHAVIOR, GPT-4o encounters missing step
errors in 35.0% of cases in both modularized and pipeline-based methods.

7. **Replanning and Feedback:**


       - Incorporating replanning based on feedback significantly improves the model’s performance, demonstrating over a 10% increase in success rates. For example, with
replanning, GPT-4o’s task success rate increases from 47.0% to to 59.0%, and its
execution success rate increases from 53.0% to 63.0% in BEHAVIOR .

       - Replanning can sometimes result in the over-generation of actions, as indicated by
an increased rate of additional steps errors. For instance, with replanning, GPT-4o’s
additional step error rate increases from 0.0% to 3.0% in BEHAVIOR .


These empirical findings, along with the provided examples, highlight the strengths and weaknesses
of LLMs in embodied decision-making tasks across different ability modules and simulators. The
insights gained from these experiments can guide future research and development efforts to address
the identified challenges and improve the performance of LLM-based embodied agents. We present
more examples in Appendix E to illustrate the specific areas where LLMs excel or struggle, providing
a more concrete understanding of their capabilities and limitations in various scenarios.


**B** **Embodied Agent Interface Design**


We will introduce the additional details about the EMBODIED AGENT INTERFACE (EAI) in this
section, including the motivation of the current design and its relationship with the Markov Decision
Process.


**B.1** **Motivation**


Our research focus is **embodied decision making** capabilities of LLMs. EMBODIED AGENT INTERFACE (EAI) is a diagnostic benchmark by decomposing the LLM abilities involved in **embodied**
**decision** **making** . Given the natural language instructions from humans (such as “ _cleaning_ _the_
_refrigerator_ ”, “ _polishing furniture_ ”), LLMs serve as embodied agents to achieve the specified goals
through a sequence of actions in various embodied environments.


The key difference between language models and embodied agent models is the ability to (1) interact
with the environment, (2) be goal-driven, and (3) decision making to achieve the goal, As shown
in Figure 6. While some prior works [9] have proposed benchmarks with simulators to validate the
output plan with a success rate, they are in the high-level natural language planning space without
connecting to objects and state changes in the embodied environment, as shown in Figure 8. We
address the limitations of traditional evaluations on benchmarking embodied decision-making from
three aspects: (1) “benchmarking”: We propose **a broad coverage of evaluation and fine-grained**
**metrics** . Our interface offers fine-grained metrics to automatically identify various error types (such as
missing step, additional step, wrong temporal order, affordance error, etc), providing a comprehensive
evaluation of LLM performance. (2) “embodied”: We move from high-level natural language
planning to lower-level object interactions in the embodied environment. We **standardize** **goal**
**specifications as linear temporal logic (LTL) formulas based on object-centric representations**,
extending goals from states to temporally dependent logical transitions. (3) “decision making”: We
**standardize interface and modules approach** by unifying a broad set of decision-making tasks
involving states and temporally extended goals, four key LLM-based modules (goal interpretation,
subgoal decomposition, action sequencing, and transition modeling), covering the fundamental
abilities in the Markov Decision Process (detailed in Appendix B.3). Please see Section 1 in the


22


**Embodied Agent Interface**























next_to



toggled_on













not stained

(fridge.97)









CLEAN
(fridge.97)



GRASP

(rag.0)



PLACE_NEXTTO
(sink.82)











**Example Task** Clean Refrigerator: use the rag to clean the refrigerator and ...


Figure 6: Compared to general language models, the embodied agent has three key new abilities, including
interacting with environments, being goal-driven, and performing decision-making to achieve the goal. We
believe a systematic evaluation for embodied decision-making should cover three aspects by standardizing the
interface, the goal representation, and the evaluation metrics. Our EMBODIED AGENT INTERFACE addresses the
limitations of traditional evaluations by focusing on goal-driven evaluation, standard interface, and modules, as
well as broad coverage of evaluation and fine-grained metrics.


main paper for more details. Figure 7 summarizes the design of EMBODIED AGENT INTERFACE to
connect LLMs with embodied environments.


Figure 7: The EMBODIED AGENT INTERFACE aims to design a standard interface for LLMs to perform tasks in
the embodied environment.


The evaluation is based on a comprehensive annotation of tasks, where each task contains the natural
language task name, the natural language task instruction, the symbolic goal definition (including its
LTL form), the symbolic action trajectory, the transition models involved in the task, as detailed in
Figure 9 and Figure 10.


**B.2** **Input and Output Details**


As shown in Figure 11, the overall input of the interface consists of three main parts: (1) the task name
and instruction, (2) the agent instructions, including in-context examples, and (3) the environment


23


**Previous Work (Choi et al, 2023)**



State ( **Language** )


State ( **Env** )


Operator ( **Env** )







**Ours**



Skill Sequence

( **Language** )


Goal Interpretation ( **Env** )


Action Sequencing ( **Env** )


Subgoal Decomposition ( **Env** )


Transition Modeling ( **Env** )





**INPUT** **OUTPUT** **METRIC**


Figure 8: Comparison with existing benchmarks on LLMs for embodied decision making.

















**Data Stored in a Task Instruction**


Figure 9: VirtualHome dataset structure example.


representation, which includes objects, their states, and relations. The detailed prompt templates are
provided in Appendix I.


The input and output for each ability module differ, as illustrated in Figure 2. The mathematical
formulation has been detailed in Section 2 of the main paper. **Goal Interpretation (ability module 1)**
aims to ground the natural language instruction to the environment representations of objects, states,
relations, and actions. For example, in Figure 2, the task instruction “ _Use the rag to clean the trays,_
_the bowl, and the refrigerator._ _When you are done, leave the rag next to the sink..._ ” can be grounded
to specific objects with IDs, such as _fridge_ (ID: 97), _tray_ (ID: 1), _bowl_ (ID: 1), _rag_ (ID: 0), and _sink_
(ID: 82). Note that a simple natural language description can be grounded into a set of multiple goal
conditions (object state and relation).


The **Subgoal** **Decomposition** **(ability** **module** **2)** generates a sequence of states, where each
state can be a set of objects and their states. Here, we highlight the important states, such as
the transitions between a sequence of _next_to_ (rag.0, sink.82), _toggled_on_ (sink.82), _soaked_ (rag.0),
_toggled_off_ (sink.82), _open_ (fridge.97), _not_stained_ (fridge.97). To achieve these state transitions, we can use a high-level planner such as BFS to search for the **Action** **Sequences**
**(ability** **module** **3)** that achieve these state transitions. We obtain the following action se

24


**Existing Data & Annotations**

















**Additional Human Annotations**


Figure 10: BEHAVIOR dataset structure example.

























|Input Prompt<br>Environment Env Predicates<br>State:<br>VirtualHome<br>holds_rh: holds right hand<br>Objects Initial States holds_lh: holds left hand...<br>Action/Operator:<br>fridge.97... stained(fridge.97)<br>... RIGHT_GRASP ...|Agent Instruction<br>You are an embodied<br>agent to determine the<br>subgoal plan of a task..<br>In-Context Examples<br>## Initial States ...<br>## Task Goal States ...<br>## Output ...|
|---|---|
|stained(fridge.97)<br>**Environment**<br>**Initial States**<br>**VirtualHome**<br>...<br>fridge.97<br>**Objects**<br>...|stained(fridge.97)<br>**Environment**<br>**Initial States**<br>**VirtualHome**<br>...<br>fridge.97<br>**Objects**<br>...|


**LLMs**





**Output from Embodied Agent Interface**


**Goal Interpretation** **Subgoal Decomposition** **Action Sequencing** **Transition Modeling**



: **action** soak
: **parameters** (
?obj1 ?agent ?sink )
: **precondition** (
and ( _holding_ ?obj1)
( _next_to_ ?sink ?agent)
( _toggled_on_ ?sink))
: **effect** (
_soaked_ ?obj1)



**A1** RIGHT_GRASP (rag.0)
**A2** RIGHT_PLACE_NEXTTO(sink.82)
**A3** TOGGLE_ON (sink.82)
**A4** SOAK (rag.0)
**A5** TOGGLE_OFF (sink.82)
**A6** OPEN (fridge.97)
**A7** CLEAN (fridge.97) ...



not stained (fridge.97)
not stained (tray.1)
not stained (bowl.1)
soaked (rag.0)
next_to (rag.0, sink.82)
closed (fridge.97)



**S1** next_to (rag.0, sink.82)
**S2** toggled_on (sink.82)
**S3** soaked (rag.0)
**S4** toggled_off (sink.82)
**S5** open (fridge.97)
**S6** not stained (fridge.97)



... ...



**VirtualHome** **...**

**Metrics**





















Figure 11: Example input and output for the ability modules.


quence: RIGHT_GRASP(rag.0), RIGHT_PLACE_NEXTTO(sink.82), TOGGLE_ON(sink.82),
SOAK(rag.0), TOGGLE_OFF(sink.82), OPEN(fridge.97), CLEAN(fridge.97). Note that multi

25


ple actions may be required to achieve a single one-step state transition. For example, to perform the state transition _next_to_ (rag.0, sink.82) _→_ _toggled_on_ (sink.82), we need two actions
RIGHT_GRASP(rag.0), RIGHT_PLACE_NEXTTO(sink.82). We show a successful execution
of this piece of an action sequence in Figure 12.


**Transition Modeling (ability module 4)** is different from the previous modules. It serves as the
low-level controller to guide the simulator in performing state transitions from preconditions to
post-effects [71–74]. In Figure 2, the input is the operator name “ _soak_ ”, and the preconditions are
three states: “ _holding_ (?obj1)”, “ _next_to_ (?sink ?agent)”, and “ _toggled_on_ (?sink)”. The post effect
after executing SOAK is “ _soaked_ (?obj1)”.





next_to



toggled_on













not stained

(fridge.97)







CLEAN
(fridge.97)



GRASP











(rag.0)



PLACE_NEXTTO
(sink.82)





**Example Task** Clean Refrigerator: use the rag to clean the refrigerator and ...


Figure 12: An example of successful execution in BEHAVIOR .


**B.3** **Grounding to Markov Decision Process**


To support a wide range of tasks in various environments, we design the EMBODIED AGENT
INTERFACE based on the Markov Decision Process (MDP) [75], a fundamental mathematical
framework for robot learning to formalize sequential decision-making in embodied agents [76]. This
allows us to create a structured approach to benchmark the robot’s decision-making process.


Figure 13: Embodied Decision Making is a Markov Decision Process.


An embodied agent takes natural language instructions from humans and achieves the specified
goals through a sequence of physical state transitions. It is essentially a decision-making process
to determine the actions based on the goal and the current state in the embodied environment. As a
result, we formulate the MDP process as below to input natural language instructions and interact
with the environment to achieve the specified goals.


**MDP Formulation for Embodied Agents.** As shown in Figure 13, the Markov Decision Process
for an embodied agent can be defined by a tuple _⟨U, S, A, M, R, g⟩_, where:


_U_ is the universe of objects in the environment, which are the fundamental entities that the agent
interacts with. _S_ is the state space, where each state _s_ _∈S_ is represented as a tuple _⟨U, F⟩_ . _F_ is
a set of relational Boolean features that capture the properties and relations among objects in the
environment. _A_ is the action space, which represents the set of actions the embodied agent can
execute. Actions are represented as tuples _⟨name, args⟩_, where _name_ is the action name and _args_ are
the object arguments the action operates on. _M_ : _S × A →S_ is the environmental transition model,
which specifies the next state _st_ +1 given the current state _st_ and action _a_ . _R_ : _S_ _× A × g_ _→_ R
is the reward function. It depends on the current state, action, and the goal specification _g_ . For a


26


state _s_, action _a_, and goal _g_, _R_ ( _s, a, g_ ) = 1 if _eval_ ( _g, s_ ) = 1 (i.e., the goal is satisfied in state _s_ ),
and _R_ ( _s, a, g_ ) = 0 otherwise. Here, _eval_ : _g × S_ _→{_ 0 _,_ 1 _}_ determines whether a state satisfies the
goal specification. _g_ is the goal specification. The goal should be grounded in terms of the desired
final states of objects and their interactions (relations and executed actions), capturing the intended
outcome of the agent’s actions. The input of the goal can be a natural language, such as “ _cleaning the_
_refrigerator_ ” or “ _polishing furniture_ ”. We denote natural language goal specification as _lg_ .


**Grounding Our Evaluation Protocol to the Fundamental Modules of MDP.** The embodied agent
receives a natural language goal specification _lg_, translates it to the environment objects and their
states, relations, and actions as a goal specification _g_, and aims to achieve it through a sequence of
state transitions. To abstract the embodied environment, we design the representation to contain
_Object_, _State_, _Action_, and, based on that, _Goal_ (as final states) and _Trajectory_ (as temporally dependent
sequences of actions/states). Our interface is built upon a LTL REPRESENTATION layer based on
Linear Temporal Logic (LTL), which serves as a unified, expressive interface to communicate with
robots in different environments (e.g., different simulators such as BEHAVIOR and VirtualHome).


Figure 14: Our four ability modules are fundamental modules of the MDP process.


At each step, the agent observes the current state _s ∈S_, selects an action _a ∈A_ based on its policy
_π_ : _S_ _× g_ _→A_, and receives a reward _R_ ( _s, a, g_ ). The environment transitions to the next state
according to _M_ ( _s, a_ ). As shown in Figure 14, according to MDP, it essentially focuses on four
abilities:


    - Input of Goals, which corresponds to **Goal Interpretation (ability module 1)**, translating
the natural language goal to environment objects and their relations and actions.

    - Output of Trajectories, where the output can be a sequence of actions or a sequence of
states, which can be regarded as **Action** **Sequencing** **(ability** **module** **2)** and **Subgoal**
**Decomposition (ability module 3)** .

    - The core part of the **Transition Model (ability module 4)** Learning, which is covered by
the Transition Modeling (ability module 4).

    - The goal-evaluation function _eval_ and reward model can be reflected in the detailed, finegrained evaluation metrics we provide.


In this way, the EMBODIED AGENT INTERFACE has a comprehensive coverage of the fundamental
abilities and can provide a systematic evaluation of the foundational MDP process.


**B.4** **The Relationship between Ability Modules**


To identify the weaknesses and areas for improvement in LLMs for embodied decision-making, we
need to evaluate each ability module individually and focus on detailed, fine-grained tasks. Rather
than simply knowing that the final success rate is still insufficient, we aim to understand which
abilities are already well-developed and how we can effectively integrate different ability modules to
enhance overall performance. This includes exploring the integration between LLMs and external
tools, as well as LLMs across different modules, enable us to guide embodied agents to use LLMs
more selectively and effectively.


To achieve this, as shown in Figure 4, we design an evaluation protocol that isolates a single module to
be handled by the LLM while using existing data or tools to serve as the other modules. This approach


27


shifts the focus from an end-to-end evaluation to an accurate assessment of each individual component.
By doing so, we can probe the LLM’s capabilities and limitations within each specific ability in
detail, gaining a more nuanced understanding of its performance. This fine-grained evaluation
allows us to identify the strengths and weaknesses of LLMs in each ability module, guiding future
research efforts to address the identified challenges and improve the integration of LLMs in embodied
decision-making tasks.


The Subgoal Decomposition and Action Sequencing modules are similar in that they both involve
trajectory output and evaluate the ordering of decision-making. However, the fundamental distinction
between them lies in the nature of their outputs. Action sequencing produces imperative actions,
while subgoal decomposition generates declarative states, as illustrated in Figure 12.


Transition modeling can be considered as the low-level controller that governs the state transitions
when executing an action. The hallmark of transition modeling is the ability to search a path to
navigate from initial predicates to goal predicates using existing actions. Defining preconditions and
post effects for each action enables this search and backtracking.


**C** **LTL Representation and Implementation**


**C.1** **Why LTL**


Our EMBODIED AGENT INTERFACE is built on top of the linear temporal logic (LTL) language.
This is motivated by two critical desiderata of the interface. First, we need an expressive and
compact language to describe task specifications. Classical choices such as first-order logic formulas on goal states or reward functions both have their limitations: goal state formulas only
describe the requirements over the goal state but not any temporal ordering of how subgoals should
be achieved. On the other hand, reward functions are general in specifying preferences over trajectories but they usually can not be represented in a compact way due to their numeric nature.
Second, we need a unified interface between different modules of an embodied agent system, such
as the inputs and outputs for goal interpreters, subgoal generators, etc. For example, BEHAVIOR [4] uses BEHAVIOR Domain Definition Language (BDDL) to represent goals as a target state
with logic constraints, such as “not(stained(fridge_97)), forall tray.n.01-tray.n.01
inside(tray.n.01,fridge_97) not(stained(tray.n.01)), ...”. In contrast, VirtualHome [5] describes task goals in natural language with a different focus, such as “ _take everything_
_out of the fridge, throw anything outdated..._ ”. Furthermore, different agents may follow different
trajectories and have different criteria for achieving the same goal. For instance, BEHAVIOR focuses
on state transitions to match final states with goals (“not _stained_ (fridge)”), whereas VirtualHome
evaluates execution success without verifying whether the goal states are satisfied. BEHAVIOR
focuses on state transitions to match final states with goals(“not _stained_ (fridge)”), while VirtualHome
only considers execution success rates without checking whether goal states are satisfied. This leads
to significant differences in goal interpretation and object state representation across environments.


LTL provides an expressive and compact description language solution to these issues. At a high
level, an LTL formula can describe basic state constraints (e.g., a subgoal should be achieved),
action constraints (e.g., a particular action should be executed), and possible temporal orders and
dependencies among them (e.g., all dishes should be cleaned before we start to cook). By combining
temporal connectives such as “next” and propositional logic connectives, we can also flexibly describe
alternative goals or subgoal sequences. Therefore, state goals, action sequences, pre-conditions and
post-conditions of actions, subgoal sequences, or even sets of candidate subgoal sequences, can all be
expressed in LTL in a compact way. As a byproduct, using a single description language for all inputs
and outputs enables us to design a unified evaluation metric to measure the prediction accuracy, by
measuring the similarity between two LTL formulas, which is detailed in later sections.


Figure 15 illustrates the complete process of subgoal decomposition using LTL in our EMBODIED
AGENT INTERFACE. The process begins with describing the environment using LTL-like grammar.
Once the prompt is crafted, a language model generates the corresponding output, which is then
translated into a plain LTL formula. This formula is subsequently parsed into an LTL expression
tree. Finally, a concrete subgoal path is sampled and converted into an action sequence. This action
sequence is then executed in simulators to ensure two main criteria: (1) the subgoals are well-defined
and executable, and (2) if executable, whether they meet the final state.


28


**Universal Information**




















|You are determining the<br>subgoal plan of a task<br>solving by a robot ...<br>Vocabulary|## Relevant Objects<br>{'name': 'jar.0', ...}<br>## Initial States<br>...<br>## Task Goal States<br>...<br>## Output<br>ontop(strawberry.0,<br>countertop.84) ...|
|---|---|
|| ontop | (obj1.id, obj2.id) |<br>obj1 is on top of obj2 |<br>| under | (obj1.id, obj2.id) |<br>obj1 is under obj2 | ...|| ontop | (obj1.id, obj2.id) |<br>obj1 is on top of obj2 |<br>| under | (obj1.id, obj2.id) |<br>obj1 is under obj2 | ...|



**Task: Cleaning Windows**










|Col1|S1 then S2 then S3 then ... S8 then<br>holds_rh(rag.0) or holds_lh(rag.1) then S10|
|---|---|
|**S9**|**S9**|
|**holds_rh(rag.0)**<br>**holds_lh(rag.1)**<br>**S8**<br>**S9**<br>**S10**<br>**...**<br>A1<br>A2<br>A3<br>A8<br>A9.1<br>A9.2<br>A10<br>A10<br>**S1**<br>**S2**<br>**S0**|**holds_rh(rag.0)**<br>**holds_lh(rag.1)**<br>**S8**<br>**S9**<br>**S10**<br>**...**<br>A1<br>A2<br>A3<br>A8<br>A9.1<br>A9.2<br>A10<br>A10<br>**S1**<br>**S2**<br>**S0**|












|Relevant Objects In The Scene<br>{'name': 'window.1', 'category': 'window_n_01'}<br>{'name': 'rag.1', 'category': 'rag_n_01'} ...<br>...|Col2|
|---|---|
|**Initial States**|**Task Goal States**|
|<br>not soaked(rag.0)<br>not soaked(rag.1)<br>dusty(window.0)<br>···<br>|<br>(soaked ?rag.0) and<br>(soaked ?rag.1) and<br>(not (dusty ?window.0)) and<br>(not (dusty ?window.1))<br>|



Figure 15: Pipeline of subgoal decomposition based on LTL in EMBODIED AGENT INTERFACE


**C.2** **Comparision with Traditional LTL Representation**


Compared with initial LTL, our adaptation introduces relational state representations, quantifiers
(including a counting quantifier _∃_ [=] _[n]_ ), and a custom "then" operator for temporal ordering in finite
trajectories replacing typical "Next" and "Eventually" operators, making it more expressive for task
planning. While these extensions enhance the framework, the use of logical connectives and recursive
formula structure remains consistent with standard LTL.


**C.3** **Syntax and Semantics of LTL Formulas**


In EMBODIED AGENT INTERFACE, a state is represented as a tuple _s_ = _⟨U, F_ _⟩_, where _U_ is the
universe of entities, assumed to be a fixed finite set. _F_ is a set of relational Boolean features. Each
feature _f_ _∈_ _F_ can be viewed as a table where each entry is associated with a tuple of entities
( _o_ 1 _, · · ·_ _, ok_ ). Each entry has the value of the feature in the state, and _k_ is the arity of the feature. For
example, the feature _on_ ( _x, y_ ) is a binary predicate. Actions can be viewed as primitive functions that
take entities as inputs. For a physical robot, this corresponds to the available low-level controllers
that our algorithm can interface with, such as moving and grasping.


**LTL syntax.** Our EMBODIED AGENT INTERFACE uses a fragment of the full linear temporal logic
(LTL) formalism on finite trajectories. In particular, we consider the following two types of atomic
propositions. The arguments to these propositions can be either object in the state (e.g., book1, cat1)
or quantified variables (e.g., _x_ ).


(1) State propositions: Predicates that describe properties of object states and relations. For
example, _ontop_ (book1 _,_ chair1).
(2) Action propositions: Predicates that denote actions. For example, _touch_ (cat).


An LTL formula _ϕ_ is defined recursively as follows:

_ϕ_ ::= _p | ¬ϕ | ϕ_ 1 _∧_ _ϕ_ 2 _| ϕ_ 1 _∨_ _ϕ_ 2 _| ϕ_ 1 _⇒_ _ϕ_ 2 _| ∀x ϕ_ ( _x_ ) _| ∃x ϕ_ ( _x_ ) _| ∃_ [=] _[n]_ _x ϕ_ ( _x_ ) _|_ ( _ϕ_ ) _| ϕ_ 1 then _ϕ_ 2


where _ϕ_ 1 and _ϕ_ 2 are LTL formulas, _p_ is an atomic proposition. _¬_ (negation), _∧_ (and), _∨_ (or), _⇒_
(implies) are logical connectives. _∀_, _∃_ and _∃_ [=] _[n]_ are quantifiers. Note that, _∃x_ means that there is at
least one x such that _ϕ_ ( _x_ ) is satisfied, whereas _∃_ [=] _[n]_ _x_ means that there are exactly _n x_ ’s such that _ϕ_ ( _x_ )
is satisfied. **then** is a temporal connective, where _ϕ_ 1 **then** _ϕ_ 2 intuitively means _ϕ_ 1 should happen
before _ϕ_ 2 [†] . Note that the operator **then** is a combination of the “next” and the “eventually” operator
in standard LTL formalism, and we do not include “globally” and “until,” since the “then” operator is
sufficient for describing all the task and input-output specifications in our system, although we can
naturally extend our implementation to include them.





†The priority of LTL operators from highest to lowest is () _> ∀_ = _∃_ = _∃_ = _n_ _> ¬ > ∧_ _> ∨_ _> then_ .


29


Figure 16: An example of LTL representation.


**LTL semantics** . An LTL formula can be viewed as a classifier over trajectories semantically: we can
evaluate an LTL formulate _ϕ_ based on a state-action sequence. If the evaluation returns true, we say
the state-action sequence satisfies _ϕ_ . This can be directly used to evaluate whether a generated action
sequence satisfies the task specification. The task of a planner would be to take an LTL formula as its
specification and generate a state-action sequence that satisfies the formula.


Let a state-action trajectory _T_ be [ _s_ 0 _, a_ 1 _, s_ 1 _, . . ., an, sn_ ], _Ti_ = ( _si, ai_ ), and _U_ be the universe of
entities in _T_ . For a state-action pair, we can define the semantics of atomic propositions, logic
connectives, and quantifiers. In particular, for atomic propositions _p_, _eval_ ( _p,_ ( _si, ai_ )) is true if _p_ is
satisfied in _si_ (if _p_ is a state predicate) or _ai_ = _p_ (if _p_ is an action predicate). All logic connectives ( _¬_,
_∧_, _∨_, and _⇒_ ) and quantifiers ( _∀_ and _∃_ ) follows their semantics in first-order logic. The for-n counting
quantifier _∃_ [=] _[n]_ has the semantics that: _eval_ ( _∃_ [=] _[n]_ _x.ϕ_ ( _x_ ) _, Ti_ ) = 1[ [�] _x_ _[eval]_ [(] _[ϕ]_ [(] _[x]_ [)] _[, T][i]_ [) =] _[ n]_ []][, where][ 1][[] _[·]_ []]

is the indicator function. For compactness, if we apply a state-action formula _ϕ_ on a trajectory _T_
instead of a concrete state-action pair _Ti_ : _eval_ ( _ϕ, T_ ) = _∃k.eval_ ( _ϕ, Tk_ ). That is, _ϕ_ is satisfied in at
least one of the states in _T_ .


The semantics of the operator **then** is defined as the following:


_eval_ ( _ϕ_ 1 **then** _ϕ_ 2 _, T_ ) = _∃k.ϕ_ 1( _T≤k_ ) _∧_ _ϕ_ 2( _T>k_ ) _,_


where _T≤k_ is the first _k_ state-action pairs in _T_ and _T>k_ is the suffix sequence after _k_ steps. Intuitively,
it means, there exists a segmentation of the trajectory _T_ such that _ϕ_ 1 is satisfied in the first half while
_ϕ_ 2 is satisfied in the second half.


The LTL formula will be parsed into an LTL expression tree before the evaluation process, as
demonstrated in Figure 16. In order to evaluate the function _eval_ ( _ϕ, T_ ) given the LTL formula and a
state-action sequence, one needs to recursively evaluate components in _ϕ_ based on their semantics.
This is typically implemented with a dynamic programming algorithm over LTL formulas and
subsequences of _T_ .


30


**D** **Fine-Grained Metrics and Automatic Error Detection**


To evaluate each ability in the simulator, we design the evaluation pipeline of each ability and detailed
in this section.


**D.1** **Goal Interpretation:** **State Goal, Relation Goal and Action Goal**


Figure 17: Evaluation pipeline of goal interpretation. We evaluate the LLM’s output for the Goal Interpretation
in three key dimensions: single-object states, object-object relations, and agent-action goals. For each dimension,
we calculate precision, recall, and F1 score to measure the LLM’s false positive predictions, likelihood of missing
goals, and overall capability, respectively.


For the Goal Interpretation, the LLMs receive a natural language description of the overall goal, the
initial world state _S_ 0 (including relevant objects and their initial states), and the complete list of all
possible actions and object states recognized by the simulator. Based on these inputs, the LLMs are
expected to output a symbolic representation of the overall goal, denoted as _g_ . This task is designed
to assess the LLMs’ ability to act as a translation layer between a human user (who only interacts
with the system using natural language) and the embodied agent (which only understands symbolic
goals composed of simulator-recognizable states and relevant objects in the scene).


To evaluate the LLMs’ output for the Goal Interpretation, we first filter for grammatically correct predicted goals while keeping track of structurally incoherent predictions and object/state hallucinations.
Then, we evaluate the remaining grammatically correct predicted goals against ground truth goals in
three key dimensions:


    - **State Goal (or Node Goal)** with a focus on single-object states, e.g., _facing_ (cat), _clean_ (cup),
_switched_on_ (television).

    - **Relation Goal (or Edge Goal)** with a focus on object-object relations, e.g., _next_to_ (character,
cat), _on_ (cup, table), _on_ (character, sofa).

    - **Action Goal** with a focus on agent-action goals, e.g., _touch_ (cat), _touch_ (remote). Simulators
such as VirtualHome contain action goals for some short tasks that have key actions but no
post-effects to validate in the goal, such as the task of “ _pat cat_ ”. In contrast, other simulators
like BEHAVIOR-100 do not include such action goals.


For each of the three goals, we calculate the following metrics:


    - _Precision_ : Measures the LLMs’ false positive predictions, indicating the proportion of
predicted goals that are correct.

    - _Recall_ : Measures the LLMs’ likelihood to miss certain goals, indicating the proportion of
ground truth goals that are correctly predicted.

    - _F_ 1 Score: A joint measure that combines precision and recall, representing the overall
capability of the LLMs in each dimension.


31


These metrics provide a comprehensive evaluation of the LLMs’ performance in interpreting natural
language goals and translating them into symbolic representations that the embodied agent can
understand and execute.


**D.2** **Action Sequencing:** **Trajectory Error Detection for Missing Step, Additional Step, Wrong**
**Temporal Order, Affordance Error**


Action sequencing serves as an intuitive and pragmatic approach to evaluate the effectiveness of LLMs
in the context of embodied agents. This task requires LLMs to generate a sequence of executable
actions aimed at achieving predefined goals. The evaluation protocol for action sequencing focuses
on assessing the LLMs’ ability to produce accurate and executable action sequences within embodied
environments. The process involves several key steps to ensure that the generated plans are both
realistic and effective in achieving the specified goals.


Figure 18: Evaluation pipeline of action sequencing.


**Input Instruction.** In action sequencing tasks, LLMs are given an initial world state _s_ 0, a goal _g_,
and general environment information, such as the vocabulary of actions and predicates. The models
are required to output a sequence of executable actions ¯ _a_ = _{a_ 1 _, a_ 2 _, · · ·_ _, an}_, where each action _ai_
includes an action name and an ordered list of object names as parameters.


The input includes the objects present in the scene, the relative initial state (we use the changes
between the initial state and final state to decide a subset of relative objects and their states as the
relative initial state), node goals, edge goals, and action goals.


The next step involves rephrasing the concrete goals into natural language, which helps improve the
LLM’s understanding and execution of the tasks. The rephrased goals are structured as node goals
(specifying the state of an object in natural language, such as “ _the television is switched on_ ”), edge
goals (describing the relationship between objects in natural language, such as “ _the agent is next to_
_the television_ ”), action goals (describing the required actions in some tasks in natural language, such
as “ _the robot touches the remote control_ ”).


The evaluation framework for this task is divided into two primary components: trajectory evaluation
and goal achievement.


**Trajectory Evaluation.** The trajectory evaluation assesses whether the action sequence ¯ _a_ is executable. If ¯ _a_ is found to be non-executable, errors are categorized into three main classes, each with a
fine-grained set of subclasses:


a. **Grammar Errors:**


       - **Parsing** **Error** : Evaluates whether the output strictly adheres to specified format
requirements.

       - **Hallucination Error:** The format is correct, but the action names, object names, or
predicate names are not in the environment vocabulary.


32


**–** _Action Name Hallucination_       - Checks for the accuracy of action names supported
by the environment.

**–** _Object Name Hallucination_       - Ensures the correctness of object names supported by
the environment.

       - **Action-Argument Number Error**        - Verifies that the action or argument parameters are
incorrect, mainly if the length of parameters does not meet the specified requirements
of the action.

b. **Runtime Errors:**


       - **Affordance Error** : Determines if the properties of objects allow for the execution of
the action. For example, _open_ (shelf) is wrong as the shelf cannot be opened.

       - **Additional Step** : Detects when an action is redundant given the current state. If objects
are affordable for the action but the intended effect is already present in the current
state, it indicates an unnecessary additional step. For example, _toggle_on_ (light) cannot
be executed when the _light_ is already _toggled_on_ .

       - **Missing** **Step** : Identifies when a required precondition for an action is not met. If
(1) properties match, (2) the effect has not been satisfied in the current state, then the
execution error stems from an unsatisfied precondition. We then (3) check whether the
precondition has never been satisfied in the historical states, which means that a step is
missing to trigger the precondition. For example, _release_ (book) cannot be executed
when the precondition _grasped(book)_ is NOT satisfied. If the book has never been
grasped by the agent in the historical states, a necessary step _grasp_ (book) is missing.

       - **Wrong Order** : Determines if actions are executed out of sequence. If the precondition
is wrong but there has been a precondition being satisfied in the historical states, then
the error indicates a wrong order (i.e., the current step should be executed immediately
after the relevant historical state). In the previous example, if _grasped(book)_ has been
previously satisfied (e.g., the book was picked up by the agent but then placed on the
table), the order of actions is wrong, and _release_ (book) should be promoted to the
earlier steps when the book was still grasped.


Figure 19: Automatic Error Categorization for Trajectory: (1) Check if the action is affordable based on the
properties of objects in the current state. If not, return "Affordance Error". (2) If the action is affordable, check
if the intended effect of the action is already satisfied in the current state. If it is, return "Additional Step". (3) If
the effect is not redundant, check whether the precondition of the action is satisfied in the current state. If not,
proceed to the next step. Otherwise, return "No Runtime Error". (4) If the precondition is not satisfied, check
if it has ever been satisfied in any of the historical states. If the precondition has never been satisfied, return
"Missing Step". (5) If the precondition has been satisfied in a historical state, return "Wrong Order".


33


**Goal Satisfaction** The success criteria are based on the accurate achievement of all specified goal
conditions and the trajectory to achieve the goal. Both node goals, edge goals, and action goals are
checked to ensure that the final state of the environment matches the desired outcomes.


If the action sequence _a_ ¯ is executable, the subsequent evaluation examines whether executing _a_ ¯
from _s_ 0 satisfies the goal _g_ . The simulator executes the action sequence ¯ _a_ and collects the execution
information and intermediate world states _si_ for each action _ai_ _∈_ _a_ ¯. The execution continues until
either an action is not executable (marked as _a_ t) or all actions are executed successfully. The final
world state is denoted as _s_ t.


Given a goal _g_, each goal condition is assigned a category (node goal, edge goal, or action goal) based
on a simulator-based classification, similar to Appendix D.1. The satisfaction of each goal condition
_gi_ by _s_ t is then checked using the simulator. The primary metric for this evaluation is the _Success_
_Rate_, calculated as the ratio of tasks where _g_ is satisfied to the total number of tasks. Additionally,
recall is calculated for all goals and different categories of goals to evaluate goal satisfaction.


**D.3** **Subgoal Decomposition:** **Converting Subgoal Trajectory to Action Trajectory with BFS**
**Searching**


The subgoal decomposition module generates a sequence of declarative states in terms of temporal
order. While there is no reference decomposition and multiple optional ways to decompose goals,
our method avoids recursive translation between symbolic and natural language, which can add
complexity. Additionally, decomposing goals purely at the final time step is often inefficient for
search purposes. By using a temporal breakdown, the approach balances efficiency and effectiveness
and is commonly employed in embodied agent systems such as Voxposer [77] and ReKap [78]. To
validate the feasibility of the state transitions, we need to transform the state transitions into an
actionable trajectory that can be executed and evaluated in the simulator. As depicted in Figure 4, we
address this challenge by utilizing a customized planner to refine the subgoal decomposition output
into an actionable sequence. As detailed in Section 2 in the main paper, this subgoal-action mapping
function, denoted as _AM_ ( _ϕ, s_ [¯] 0), takes the LTL representation of the subgoal sequence _ϕ_ [¯] and the
initial state _s_ 0 as inputs and generates a corresponding state-action sequence _t_ [¯] .


We implement this mapping function using a Breadth-First Search (BFS) algorithm. We show this
process in Figure 20, where we find a sequence of actions that can transition the agent from the initial
state to the desired subgoal states.


The BFS algorithm starts from the initial state _s_ 0 and expands the search frontier by exploring all
possible actions at each step. It maintains a queue of states to be visited and keeps track of the path
from the initial state to each visited state. The search continues until a state satisfying the first subgoal
is reached. The process is then repeated, using the reached subgoal state as the new initial state and
the next subgoal as the target.

The subgoal-action mapping function _AM_ ( _ϕ, s_ [¯] 0) takes the LTL representation of the subgoal
sequence _ϕ_ [¯] and the initial state _s_ 0 as inputs and returns a state-action sequence _t_ [¯] that achieves the
subgoals. The function can be described as follows:


34


Figure 20: Evaluation pipeline of subgoal decomposition. The subgoal decomposition module generates a
sequence of declarative states, which need to be transformed into an actionable trajectory for execution in the
simulator.





By using the BFS algorithm to find action sequences that connect the subgoal states, we can effectively
transform the declarative subgoal decomposition into an imperative action trajectory. This allows us
to evaluate the subgoal decomposition module in the simulator and assess its effectiveness in guiding
the agent towards the desired goal.


Note that the BFS algorithm guarantees finding the shortest path between subgoals if one exists, but
it may be computationally expensive in large state spaces. In our current benchmark, due to limited
pre-defined action space and state space (detailed in Appendix N.2 and Appendix N.1), we do not
need to control the searching space. In practice, with larger action space and state space, heuristics or
domain-specific knowledge can be incorporated to guide the search and improve efficiency.


By employing this approach, we can effectively evaluate the subgoal decomposition module within
the simulator, ensuring that the generated subgoals can be successfully grounded and executed.
This allows us to assess the quality of the subgoal decomposition and its impact on the overall
decision-making process, even though the direct output of the module is declarative states rather than
imperative actions.


35


**A Generated Subgoal Plan**


Figure 21: The pipeline of evaluating a generated subgoal plan.


**D.4** **Transition Modeling:** **Evaluating with PDDL Planners**


The evaluation protocol for transition modeling aims to rigorously assess the ability of Large Language
Models (LLMs) to predict accurate action preconditions and effects within embodied agent scenarios.
This evaluation is critical for understanding the proficiency of LLMs in modeling the dynamics of
physical interactions in simulated environments.

































**LLMs Prediction**

:action put_on {
:parameters (?char - character ?obj1 - object ?obj2 - object)
:preconditions (or (and (holds_rh ?char ?obj1) (next_to ?char ?obj2)) (and (holds_lh ?char ?obj1) (next_to ?char ?obj2)))
:effects (and (when (holds_rh ?char ?obj1) (not (holds_rh ?char ?obj1))) (when (holds_lh ?char ?obj1) (not (holds_lh ?char ?obj1))) (obj_ontop ?obj1 ?obj2)) }


**Ground Truth**

:action put_on {
:parameters (?char - character ?obj1 - object ?obj2 - object)
:preconditions (or (and (next_to ?char ?obj2) (holds_lh ?char ?obj1)) (and (next_to ?char ?obj2) (holds_rh ?char ?obj1)) )
:effects (and (obj_next_to ?obj1 ?obj2) (obj_ontop ?obj1 ?obj2) (not (holds_lh ?char ?obj1)) (not (holds_rh ?char ?obj1))) }


Figure 22: Transition Model evaluation metrics based on Bipartite Graph Matching for pre-conditions and
post-effects.


The process begins with the preparation of input data, which includes goals, scripts, initial states, and
final states provided in the dataset. Initially, predicates representing relations and properties within
the domain are annotated. Based on these annotated predicates, PDDL (Planning Domain Definition
Language) operators are defined. Subsequently, a comprehensive PDDL domain file is constructed,
encapsulating the full set of predicates, including states, relations, and properties necessary for the
planning tasks. Next, relevant objects are identified from the script, initial state, and final states.
These objects are used to ground the goals into specific goals for each task. The problem file is
formulated using the relevant objects, initial states, and grounded goals.


36


With the domain and problem files prepared, the LLM is tasked with predicting the transition model,
that is, the action preconditions and effects. The inputs to the LLM include the predicate list, problem
file, action name, and action parameters, and the output is the actions body predicted by LLM.


The output evaluation involves several key steps to assess the accuracy and reliability of the predicted
transition model. First, the predicted preconditions and effects are extracted from the LLM-generated
actions. These predicates are categorized to facilitate detailed analysis of the model’s performance.
The evaluation protocol uses several metrics to assess the performance of the LLMs. The logical
scoring function evaluates the similarity between the predicted and gold action sequences by comparing the logical structure of preconditions and effects. The success rate by planner measures the
proportion of correctly predicted actions that exists a feasible solution to achieve the desired goals
under PDDL planner[79] [‡] . Sensitivity analysis demonstrates how difficult it is for LLM to predict
specific actions in different tasks.


**Logic Form Accuracy.** In PDDL, logical connectives such as _and_, _or_, _not_, _when_, _forall_, and _exists_
are used to define preconditions and effects. Logical matching evaluates the similarity between the
predicted and ground truth preconditions or effects by parsing them into clauses of disjunctive logical
form and performing bipartite matching.


Given a set of predicted clauses _P_ = _{p_ 1 _, p_ 2 _, . . ., pn}_ and a set of ground truth clauses _G_ =
_{g_ 1 _, g_ 2 _, . . ., gm}_, we define an adjacency matrix _A_ where _A_ [ _i, j_ ] = match( _pi, gj_ ) represents the
similarity between the _i_ -th predicted clause and the _j_ -th ground truth clause. The function match( _p, g_ )
returns 1 if the clauses are identical, and 0 otherwise. If the ground truth clause _gj_ is empty,
match( _p, g_ ) = 1 if and only if _pi_ is empty as well.


For clauses containing connectives, the clauses are recursively expanded and bipartite matching
is conducted on the expanded nodes. If _p_ and _g_ are literals, the match function compares their
equivalence directly. If either clause contains a connective, the clause is further decomposed and
bipartite matching is applied to the sub-clauses.


The evaluation metrics for logical matching include precision, recall, and F1-score. True Positives
(TP) are the matched clauses, False Positives (FP) are the unmatched clauses in the predicted set, and
False Negatives (FN) are the unmatched clauses in the ground truth set.


The process begins by parsing the preconditions or effects into disjunctive logical form and constructing the adjacency matrix _A_ based on clause similarity. Bipartite matching is performed to find the
optimal pairing of predicted and ground truth clauses. The matching process is recursive, expanding


[‡https://github.com/ronuchit/pddlgym_planners](https://github.com/ronuchit/pddlgym_planners)


37


nested clauses and applying bipartite matching at each level. If a sub-clause does not match, the
entire clause is considered a non-match.


Finally, the precision, recall, and F1-score are calculated using the formulas above, providing a robust
measure of the LLM’s accuracy in modeling logical relationships within PDDL tasks.


**Success Rate by External Planners.** The planner success rate evaluates the success of the LLM in
generating correct and executable action sequences for each task category. The overall success rate
across all categories is also reported to provide a comprehensive assessment.


Given a set of programs _P_ = _{p_ 1 _, p_ 2 _, . . ., pn}_ and a corresponding set of categories _C_ =
_{c_ 1 _, c_ 2 _, . . ., ck}_, let _Pcj_ _⊆_ _P_ denote the subset of programs belonging to category _cj_ . The success rate for category _cj_ is defined as:



SuccessRate( _cj_ ) =





_p∈Pcj_ [IsSuccessful][(] _[p]_ [)]

_|Pcj_ _|_



where IsSuccessful( _p_ ) is a binary function that returns 1 if the planner successfully generates an
executable action sequence for program _p_, and 0 otherwise.


The overall success rate across all categories is given by:



OverallSuccessRate =


**E** **Full Results with 18 models**




- _k_ _j_ =1 _p∈Pcj_ [IsSuccessful][(] _[p]_ [)]

  - _k_
_j_ =1 _[|][P][c]_ _j_ _[|]_



In this section, we provide the full results and analysis for each ability module using 18 different
models, and provide a further analysis on the potential coorelation with factors in Appendix E.5.


**E.1** **Goal Interpretation**


Table 9 examines the performance of various LLMs in translating natural language task instructions
into actionable symbolic goals within two different simulators: VirtualHome (V) and BEHAVIOR
(B). Additionally, Figures 23 and 24 show detailed qualitative error cases based on error type and
goal type. Below are the detailed result analysis and error case studies.


**E.1.1** **Result Analysis**


Table 9 contains the quantitative benchmarking results of the Goal Interpretation ability module for
both Behavior and VirtualHome simulators. The results are mainly broken down into three categories:
State Goals or Unary Goals (which describe the goal state of just one object), Spatial Goals or
Binary Goals (which describe the spatial relationship goal of two interacting objects), and Action
Goals specific to the VirtualHome simulator (which describe the actions that an agent should finish
to complete the task). Finally, we include an Overall section, where all goals’ performances are
combined with a micro average.


In Table 9, our evaluation metrics consist of precision, recall, and F1 score for each goal type and
overall performance. In the case of the goal interpretation task, precision measures how unlikely a
model is to make false-positive goal predictions given reasonable instructions and task information,
whereas recall measures how unlikely a model is to leave out ground truth goals in their prediction.
The F1 score combines these two metrics and reflects the overall model performance for each category.


Based on the results in Table 9, o1-preview achieves top overall goal interpretation performance
(F1 score) in the VirtualHome simulator over other LLMs, whereas Claude-3.5 Sonnet achieves the
highest performance (F1 score) in the BEHAVIOR simulator. Gemini 1.5 Pro also demonstrates very
high performance in both simulators with very close precision, recall, and F1 scores compared to the


38


top performer on each metric. Among open-source models, the top performer by a significant margin
is the Llama-3 70B Instruct model, leading in overall F1 scores for both simulators.


For individual goal types, the performance trends are less uniform: o1-preview and Claude-3.5 Sonnet
both demonstrate very strong spatial reasoning abilities, reaching top overall F1 scores in Behavior
and VirtualHome’s spatial goal categories, respectively. Gemini 1.5 Pro leads in terms of overall
performance in VirtualHome action goals and Behavior state goals, while the Cohere Command R
model leads in terms of VirtualHome state goals.


**E.1.2** **Case Studies**


In Figure 23, we show detailed examples of Behavior and VirtualHome goal interpretation errors
broken down into two main categories: goal prediction errors and grammar errors.


Grammar errors can be further broken down into hallucination errors (such as object/state/action
hallucination) and format errors (such as JSON parsing errors). In our empirical experiments, we find
that given a reasonably clear prompt with in-context examples, commercial LLMs such as the GPT
and Claude family models are extremely unlikely to display format errors, while open-source models
such as Llama 3 8B and Mixtral 8x22B tend to make a small number of mistakes.


Goal prediction errors can be further broken down into missing goals and false positive goals, which
can be quantitatively measured by recall and precision scores, respectively. Empirically, we find two
kinds of errors to be common among both open-source and proprietary LLMs:


(1) As shown in the false positive goal error sections of Figure 23 (a) and (b), LLMs tend to
output intermediate goal states in place of final goal predictions. This phenomenon can
be intuitively explained by the LLMs’ reliance on Chain-of-Thought Reasoning [80] for
question answering. While our carefully designed system and user prompts restrict LLMs to
directly output the predicted goals without any explicit intermediate explanation, models
may still try to perform such intermediate reasoning steps in their structured output, thus
leading to such errors.


(2) As shown in the missing goal error sections of Figure 23 (a) and (b), LLMs tend to leave out
simple object spatial relationships in their output. For the task "serving a meal," with ground
truth goal condition ONTOP( _chicken.0_, _plate.2_ ) and ONTOP( _plate.2_, _table.1_ ), GPT-4o
(along with many other models) mistakenly predicts ONTOP( _chicken.0_, _table.1_ ). While
the expression "chicken ontop of table" is acceptable in a conversational setting, it presents
a completely wrong set of physical relationships between the objects chicken, plate, and
table. This type of error is common in both Behavior and VirtualHome simulators for
various models, highlighting a significant issue of imprecise spatial relationship description
in applying LLMs for embodied planning and robotic control, where physical precision is
crucial for task success.





39


Table 9: All goal evaluation results (%) for goal interpretation


**Goal Interpretation**
**State** **Spatial** **Action** **Overall**

**Model Name**

_Precision_ _Recall_ _F1_ _Precision_ _Recall_ _F1_ _Precision_ _Recall_ _F1_ _Precision_ _Recall_ _F1_
_V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_
Claude-3 Haiku 21.8 22.8 58.9 93.5 31.8 36.7 24.2 64.5 50.8 64.6 32.8 64.6 12.2 - 95.7 - 21.6 - 18.0 41.5 63.2 71.2 28.0 52.5
Claude-3 Sonnet 23.3 36.8 57.1 88.9 33.1 52.0 26.6 76.2 53.0 79.8 35.5 77.9 12.4 - 85.8 - 21.7 - 19.3 60.2 61.5 81.9 29.4 69.4
Claude-3 Opus 27.0 72.6 66.9 93.5 38.5 81.7 22.6 75.2 46.8 79.2 30.5 77.1 14.5 - 92.6 - 25.1 - 20.7 72.2 65.0 82.5 31.4 77.0
Claude-3.5 Sonnet 25.3 74.0 60.9 94.8 35.8 83.1 31.1 **84.4** **63.8** 81.3 41.8 **82.9** 14.0 - **98.8** - 24.5 - 21.7 **81.1** **69.6** 84.4 33.0 **82.7**
Cohere Command R **51.1** 7.7 **69.6** 31.4 **58.9** 12.4 34.5 56.8 21.3 55.0 26.3 55.9 3.6 - 38.9 - 6.5 - 27.4 28.2 55.7 49.6 36.7 36.0
Cohere Command R+ 20.9 23.3 52.0 79.1 29.8 36.0 17.9 66.7 15.2 61.5 16.4 64.0 10.4 - 82.6 - 18.5 - 14.9 42.0 44.5 65.5 22.4 51.2
Gemini 1.0 Pro 25.3 27.4 57.9 81.1 34.9 41.0 17.0 75.2 20.6 70.4 18.6 72.7 9.9 - 68.7 - 17.2 - 16.2 51.0 45.2 72.8 23.8 60.0
Gemini 1.5 Flash 23.6 55.8 57.9 94.1 33.5 70.1 19.8 76.6 21.1 76.7 20.5 76.7 13.5 - 90.1 - 23.5 - 18.2 69.7 50.8 80.7 26.8 74.8
Gemini 1.5 Pro 45.4 **94.0** 49.1 92.8 47.2 **93.4** 40.0 74.4 9.7 76.7 15.6 75.6 **26.8** - 80.9 - **40.3** - **35.2** 78.8 41.1 80.4 37.9 79.6
GPT-3.5-turbo 22.4 52.0 50.0 66.7 30.9 58.5 8.5 51.5 18.8 46.9 11.7 49.1 15.2 - 60.5 - 24.4 - 15.7 49.5 40.5 51.4 22.7 50.4
GPT-4-turbo 28.6 70.4 58.5 86.9 38.4 77.8 24.7 77.5 32.9 76.4 28.2 76.9 19.0 - 82.1 - 30.9 - 24.0 75.6 53.8 78.8 33.2 77.2
GPT-4o 29.0 67.1 60.0 94.8 39.1 78.6 31.5 81.1 43.6 78.5 36.6 79.8 20.5 - 85.8 - 33.1 - 26.4 76.5 59.1 82.2 36.5 79.2
Llama 3 8B Instruct 21.7 17.3 54.4 80.4 31.0 28.4 14.0 51.4 7.4 20.8 9.7 29.6 11.1 - 79.4 - 19.4 - 15.5 24.1 41.9 34.3 22.6 28.3
Llama 3 70B Instruct 23.9 69.5 61.2 **95.4** 34.3 80.4 22.6 70.0 37.5 73.3 28.2 71.6 11.2 - 88.8 - 19.8 - 17.5 64.7 58.0 78.3 26.9 70.9
Mistral Large 23.6 63.5 59.1 92.2 32.8 75.2 23.7 75.1 40.3 76.2 29.8 75.6 11.2 - 84.0 - 19.7 - 17.5 69.6 57.1 79.8 26.8 74.3
Mixtral 8x22B MoE 23.6 22.9 56.9 83.7 33.4 36.0 22.2 70.7 36.3 67.7 27.5 69.2 11.2 - 94.8 - 20.0 - 17.4 44.4 56.2 71.3 26.6 54.7
o1-mini 26.3 63.8 58.6 90.8 36.3 74.9 30.4 77.3 39.9 76.5 34.5 76.9 13.5 - 56.8 - 21.8 - 22.4 73.3 51.3 79.8 31.2 76.4
o1-preview 28.2 66.8 60.3 94.8 38.5 78.4 **44.9** 82.9 62.4 **82.7** **52.2** 82.8 26.0 - 81.5 - 39.5 - 31.8 78.1 65.4 **85.4** **42.7** 81.6


**VirtualHome: Goal Interpretation – Errors**

**Goal Prediction Errors** **Grammar Errors**

**Missing Goal** **False Positive** **Hallucination** **Format Error**



**Model:** Mixtral 8x22b MoE
**Task Name:** Work



**Model:** Llama3 70B Instruct
**Task Name:** Drink



**Model:** Claude-3 Haiku
**Task Name:** Cook Some Food



**Model:** GPT-4o
**Task Name:** Drink




|Col1|Col2|
|---|---|
|**Ground Truth Goal**|**Ground Truth Goal**|


|Col1|Col2|
|---|---|
|**Ground Truth Goal**|**Ground Truth Goal**|


|Col1|Col2|
|---|---|
|**Ground Truth Goal**|**Ground Truth Goal**|


|Col1|Col2|
|---|---|
|**Ground Truth Goal**|**Ground Truth Goal**|



_empty_ (glass.0)
_open_ (cupboard.0)



_or holds_right_hand_ (cup.1)
_holds_left_hand_ (cup.1)



node_goals: [ _sitting(character.0)…_




|Ground Truth Goal on(oven.0) closed(oven.0) LLM Output|Col2|
|---|---|
|_…_<br>_plugged_in(oven.0)_<br>_on_(oven.0)<br>**LLM Output**|_…_<br>_plugged_in(oven.0)_<br>_on_(oven.0)<br>**LLM Output**|
|||
|**Error Reason**|**Error Reason**|


|LLM Output<br>full(glass.0)<br>open(cupboard.0)|Col2|
|---|---|
|||
|**Error Reason**|**Error Reason**|


|LLM Output<br>. . .<br>inside (cup.1, hands_both.0)|Col2|
|---|---|
|||
|**Error Reason**|**Error Reason**|


|LLM Output<br>. . .<br>SYMBOLIC NODE GOALS:\n[…|Col2|
|---|---|
|||
|**Error Reason**|**Error Reason**|



Object Hallucination:
“hands_both.0” is not a valid
VirtualHome simulator object



Wrong JSON Key:
SYMBOLIC NODE GOALS



Reporting Bias, Leaving Out Goal:
_closed_ (oven.0)



Outputting Intermediate Goal:
_full_ (glass.0)



(a) VirtualHome


**: Goal Interpretation – Errors**

**Goal Prediction Errors** **Grammar Errors**

**Missing Goal** **False Positive** **Hallucination** **Format Error**



**Model:** GPT-3.5-turbo
**Task Name:** bottling_fruit



**Model:** GPT-4-turbo
**Task Name:** cleaning_up_after_meal



**Model:** GPT-4o
**Task Name:** bringing_in_wood



**Model:** Claude-3 Sonnet
**Task Name:** bottling_fruit




|Col1|Col2|
|---|---|
|**Ground Truth Goal**|**Ground Truth Goal**|


|Col1|Col2|
|---|---|
|**Ground Truth Goal**|**Ground Truth Goal**|


|Col1|Col2|
|---|---|
|**Ground Truth Goal**|**Ground Truth Goal**|


|Col1|Col2|
|---|---|
|**Ground Truth Goal**|**Ground Truth Goal**|



_closed_ (jar.1)
_inside_ (peach.0, jar.1)



not _stained_ (tray.2)
not _stained_ (bowl.1)






|Ground Truth Goal onfloor(plywood.4,floor.2) onfloor(plywood.3,floor.1) LLM Output|Col2|
|---|---|
|. . .<br>_inside_(plywood.4, room.1)<br>_inside_(plywood.3, room.1)<br>**LLM Output**|. . .<br>_inside_(plywood.4, room.1)<br>_inside_(plywood.3, room.1)<br>**LLM Output**|
|||
|**Error Reason**|**Error Reason**|


|LLM Output<br>. . .<br>open(jar.1)<br>inside(peach.0, jar.1)|Col2|
|---|---|
|||
|**Error Reason**|**Error Reason**|


|LLM Output<br>. . .<br>cleaned(tray.2)<br>bowl(bowl.1)|Col2|
|---|---|
|||
|**Error Reason**|**Error Reason**|


|inside(peach.0, jar.1) closed(jar.1) LLM Output Ground Truth Goal|Col2|
|---|---|
|. . .<br>_inside_(peach.0, jar.1)<br>_closed_(jar.1))<br>**LLM Output**|. . .<br>_inside_(peach.0, jar.1)<br>_closed_(jar.1))<br>**LLM Output**|
|||
|**Error Reason**|**Error Reason**|



State Hallucination:
“ _cleaned_ ” is not a valid state of
the Behavior simulator



Additional Parentheses:
_closed_ (jar.1))



Reporting Bias, Leaving Out Goals:
_onfloor_ (plywood.4,floor.2)
_onfloor_ (plywood.3,floor.1)



Outputting Intermediate Goal:
_open_ (jar.1)



(b) BEHAVIOR


Figure 23: Goal evaluation error examples for goal interpretation.


**E.2** **Subgoal Decomposition**


**E.2.1** **Result Analysis**


Table 10 examines the performance of various LLMs in decomposing high-level goals into actionable
subgoals within two different simulators: VirtualHome (V) and BEHAVIOR (B). Additionally, Table
11 investigates fine-grained goal satisfaction for each LLM. Below are the results and related error
analysis.


40


(a) VirtualHome



(b) BEHAVIOR



Figure 24: Goal satisfaction error examples for goal interpretation


**VirtualHome** **(1) Executable and Goal Success Rate.** The top-performing model is **o1-preview**,
achieving the highest task success rate of **89.4%** in VirtualHome, closely followed by **Gemini 1.5**
**Flash** and **Claude-3.5 Sonnet**, both with success rates of **89.1%** . For execution success rate, both
**GPT-4-turbo** and **Gemini 1.5 Flash** achieve the highest performance with rates of **94.1%** . Notably,
o1-preview demonstrates superior performance overall, suggesting robust subgoal decomposition
that aligns well with the VirtualHome simulator’s requirements. Conversely, models like **Llama 3**
**8B**, with a success rate of **48.8%**, illustrate the challenges of effective subgoal decomposition.


**(2) Grammar Errors.** The occurrence of grammar errors is notably low across the board. Specifically,
most SOTA LLMs make no errors in parsing and predicate-argument numbers. This indicates that
current SOTA LLMs can follow the subgoal syntax. However, some models are prone to hallucinate
non-existing predicates. Specifically, GPT-4o tends to hallucinate the action _POUR_ when dealing
with the task ’make coffee’, which is not defined in the subgoal decomposition setting.


**(3) Runtime Errors.** In terms of runtime errors, compared with wrong temporal order and affordance
errors, LLMs are prone to make missing steps and additional steps errors, while additional step errors
are the most critical. (1) Wrong temporal order. Closed-sourced LLMs perform well in understanding
the temporal requirement among subgoals, while open-sourced LLMs like Llama 3 8B will make
more temporal order errors. For the wrong cases, LLMs tend to ignore the sitting or lying state of the
agent and fail to call action _STANDUP_ before they apply some actions requiring a standing state, yet
this state has been achieved earlier. (2) Missing step. LLMs sometimes are prone to fail to satisfy
some preconditions when applying actions. Among all LLMs, GPT-3.5-turbo performs worst in this
error type. Specifically, it tends to ignore opening a closed object before fetching something inside
it. (3) Affordance. Overall, Llama 3 8B performs much worse than other LLMs, meaning it cannot
understand the semantics very well. (4) Additional steps. All LLMs are prone to produce additional
step errors, even for SOTA LLMs like GPT-4o and Claude-3 Opus. This is mainly because in the
initial scene state, some of the goals have already been achieved, yet LLMs still prefer to plan the
satisfied goals in their output.


**(4) Goal Satisfaction Analysis.** In Table 11, we observe that LLMs perform well in understanding
and satisfying state goals in VirtualHome. Models like **GPT-4-turbo**, **Gemini 1.5 Flash**, and **o1-**
**preview** achieve state goal success rates of over **91%** . This is because state goals are usually isolated
from other objects with fewer logical requirements. However, achieving relation and action goals
is more challenging. For instance, **o1-preview** achieves the highest relation goal success rate of
**88.3%** and the highest action goal success rate of **92.6%**, while weaker models like **Llama 3 8B**
achieve less than **70%** in these categories. This suggests that stronger LLMs can better understand
the semantics of actions and relations, whereas weaker LLMs struggle with the complexity of these
goals. Generally, most SOTA LLMs are capable of achieving over **80%** of the goals defined in our
annotated data, likely due to the relatively simple and straightforward goals in RobotHow.


**BEHAVIOR** **(1) Executable and Goal Success Rate.** **o1-preview** stands out with the highest
task success rate at **57.0%** and an execution success rate of **62.0%** in BEHAVIOR, followed by
**GPT-4o** with a task success rate of **49.0%** and an execution success rate of **55.0%** . Overall, all
LLMs face challenges in achieving high performance in BEHAVIOR. This could be attributed to the
more complex task representations in BEHAVIOR compared to VirtualHome. For example, most


41


tasks in BEHAVIOR involve quantifiers like _forall_ and _forpairs_ with complex spatial or temporal
requirements, whereas VirtualHome tasks have simpler goal definitions.


**(2) Grammar Errors.** Most LLMs are proficient in generating grammatically correct subgoal plans
with no parsing errors and correct numbers of predicate parameters. There are a few exceptions,
though. For instance, **Cohere Command R** has a parsing error rate of **23.0%** in BEHAVIOR, mostly
due to illegal tokens or syntax for the LTL parser. Additionally, models like **Llama 3 8B** tend to
hallucinate non-existing objects in the scene, leading to higher hallucination error rates.


**(3) Runtime Errors.** The most prevalent runtime errors in BEHAVIOR across various LLMs are due
to missing steps. In fact, over half of the total cases involve such errors in the majority of LLMs. Even
**o1-preview**, which has the lowest missing step error rate, encounters these errors in **25.0%** of cases.
This can be attributed primarily to two reasons: First, the precondition checking in BEHAVIOR is
stricter than that in VirtualHome . For instance, VirtualHome does not verify preconditions such
as whether an agent is holding a cleaning tool before executing the _WASH_ action. In contrast,
BEHAVIOR requires such conditions to be satisfied before invoking a similar action like _CLEAN_ .
Secondly, the complexity of tasks in BEHAVIOR often leads LLMs to overlook seemingly trivial
actions, such as opening a closed container before retrieving an item inside it. This issue persists
even when a heads-up is included in the prompt. Interestingly, LLMs tend to make significantly fewer
additional step errors in BEHAVIOR compared to VirtualHome . This is likely because the majority
of task goals in BEHAVIOR are not satisfied in the initial state, making additional steps less frequent.


**(4)** **Goal** **Satisfaction** **Analysis.** From the statistics in Table 11, it is evident that LLMs do not
perform as well in achieving state and relation goals in BEHAVIOR. The discrepancy arises because
most state and relation goals are encapsulated within quantifiers. Consequently, quantifiers such as
_forall_ or _forpairs_ tend to fail if even a single state or relation goal is not met. For example, **o1-preview**
achieves a state goal success rate of **56.5%** and a relation goal success rate of **69.4%**, which are the
highest among the models but still significantly lower than in VirtualHome. Additionally, since most
quantifiers can have multiple solutions, it is challenging to obtain accurate statistics for state and
relation goals within quantifiers. Our evaluation metric considers quantifiers as a combined entity for
both state and relation goals, contributing to the lower success rates observed.


Table 10: All trajectory evaluation results (%) for subgoal decomposition.


**Goal Evaluation** **Trajectory Evaluation**



**Model**



**Grammar Error (** _↓_ **)** **Runtime Error (** _↓_ **)**
_Task SR_ _Execution SR_
_Parsing_ _Hallucination_ _Predicate-Arg Num_ _Wrong Order_ _Missing Step_ _Affordance_ _Additional Step_
_V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_



Claude-3 Haiku 78.4 30.0 82.8 35.0 0.3 **0.0** 2.4 1.0 1.8 **0.0** 1.8 3.0 2.7 58.0 8.3 3.0 20.4 3.0
Claude-3 Sonnet 83.1 39.0 86.4 43.0 **0.0** **0.0** 1.8 2.0 **0.0** 2.0 0.6 3.0 2.7 51.0 8.6 1.0 33.7 3.0
Claude-3 Opus 87.0 41.0 90.0 47.0 0.3 **0.0** 3.6 3.0 **0.0** **0.0** 1.2 5.0 3.0 45.0 2.4 **0.0** 16.0 6.0
Claude-3.5 Sonnet 89.1 39.0 92.0 44.0 **0.0** **0.0** 1.8 1.0 **0.0** **0.0** 1.5 11.0 2.7 44.0 2.1 **0.0** 24.6 4.0
Gemini 1.0 Pro 70.4 24.0 84.6 33.0 0.6 2.0 3.3 4.0 2.4 **0.0** 1.2 3.0 2.7 51.0 5.3 7.0 **10.4** 3.0
Gemini 1.5 Flash 89.1 34.0 94.1 42.0 **0.0** 2.0 1.5 1.0 **0.0** **0.0** 0.6 2.0 3.9 53.0 **0.0** **0.0** 13.3 3.0
Gemini 1.5 Pro 87.0 31.0 91.1 37.0 **0.0** 1.0 1.5 **0.0** 1.8 1.0 **0.0** 3.0 5.6 59.0 **0.0** **0.0** 16.0 2.0
GPT-3.5-turbo 69.2 24.0 81.4 36.0 1.5 2.0 **0.0** 3.0 0.6 **0.0** 1.5 4.0 11.8 51.0 3.3 4.0 20.4 3.0
GPT-4-turbo 85.5 38.0 **94.1** 47.0 **0.0** **0.0** 1.8 3.0 **0.0** **0.0** 1.5 9.0 **2.4** 40.0 0.3 1.0 22.2 6.0
GPT-4o 88.8 49.0 90.2 55.0 **0.0** **0.0** 6.2 3.0 **0.0** **0.0** 1.2 6.0 **2.4** 36.0 **0.0** **0.0** 15.7 5.0
Cohere Command R 71.3 15.0 79.6 25.0 2.1 23.0 3.9 10.0 0.9 **0.0** 1.5 **0.0** 6.2 37.0 5.9 5.0 14.5 4.0
Cohere Command R+ 79.0 25.0 83.7 37.0 1.5 2.0 4.5 4.0 2.1 **0.0** 0.9 4.0 7.7 52.0 2.7 1.0 16.0 6.0
Mistral Large 84.3 31.0 92.0 38.0 0.3 1.0 1.8 3.0 0.3 **0.0** 2.1 4.0 3.3 52.0 0.3 2.0 11.0 1.0
Mixtral 8x22B MoE 80.5 28.0 90.2 33.0 0.3 **0.0** 2.4 4.0 **0.0** **0.0** 3.0 2.0 3.9 59.0 0.3 2.0 11.2 **0.0**
Llama 3 8B 48.8 21.0 58.0 29.0 0.6 2.0 2.4 11.0 0.6 **0.0** 6.8 6.0 5.0 44.0 26.6 8.0 18.3 7.0
Llama 3 70B 78.4 20.0 87.3 30.0 **0.0** 1.0 2.4 5.0 0.9 1.0 2.4 8.0 5.3 51.0 1.8 4.0 20.4 4.0
o1-mini 79.3 31.0 84.6 39.0 **0.0** **0.0** 1.5 3.0 0.6 3.0 0.3 7.0 8.9 46.0 4.1 2.0 21.9 1.0
o1-preview **89.4** **57.0** 93.2 **62.0** **0.0** 2.0 1.5 3.0 **0.0** **0.0** 0.3 5.0 2.7 **25.0** 2.4 3.0 12.1 7.0


**E.2.2** **Case Studies**


In this section, we would like to investigate several error examples made in subgoal decomposition
by LLMs. Specifically, we explore in both VirtualHome and BEHAVIOR .


**Grammar** **Errors** Figure 25 illustrates parsing errors, predicate parameter length errors, and
hallucination errors for both VirtualHome and BEHAVIOR . Specifically: (1) Parsing errors: the
VirtualHome example duplicates ‘.’ in an object name, whereas the BEHAVIOR example repeatedly
outputs ‘)’. (2) Predicate parameter length errors: the VirtualHome example outputs two parameters


42


Table 11: All goal success results (%) for action sequencing and subgoal decomposition.


**Action Sequencing** **Subgoal Decomposition**

**Model** _State Goal_ _Relation Goal_ _Action Goal_ _Total_ _State Goal_ _Relation Goal_ _Action Goal_ _Total_

_V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_
Claude-3 Haiku 58.6 27.0 47.2 38.7 33.1    - 49.0 35.5 89.4 26.0 82.2 34.8 71.6    - 83.1 32.4
Claude-3 Sonnet 80.9 41.0 73.3 59.8 48.6     - 70.8 54.6 89.1 37.0 **89.3** 49.8 83.3     - 88.0 46.3
Claude-3 Opus 64.7 45.0 **79.4** 53.0 57.4    - 67.3 50.8 92.4 43.0 88.6 41.6 83.3    - 89.1 42.0
Claude-3.5 Sonnet 81.3 63.0 **79.4** 62.4 57.4     - 74.9 62.6 92.9 41.0 88.6 39.5 87.0     - 90.1 39.9
Gemini 1.0 Pro 52.2 28.0 36.1 32.0 42.6     - 45.0 30.9 84.4 26.0 61.5 31.1 72.8     - 73.5 29.7
Gemini 1.5 Flash 79.5 34.0 65.5 50.0 48.0     - 67.7 45.6 **93.5** 44.0 88.3 36.0 92.0     - **91.3** 38.2
Gemini 1.5 Pro 81.7 41.0 77.2 43.2 68.2     - 77.1 42.6 91.2 31.0 72.5 37.1 89.5     - 83.9 35.4
GPT-3.5-turbo 29.5 20.0 18.3 22.6 23.6    - 24.8 21.9 84.7 28.0 54.4 28.5 64.8    - 69.4 28.3
GPT-4-turbo 74.1 39.0 73.3 39.5 47.3    - 67.3 39.3 **93.5** 45.0 84.2 46.1 90.7    - 89.5 45.8
GPT-4o 82.0 49.0 67.8 45.5 57.4    - 71.8 46.5 92.1 50.0 84.2 53.2 **93.2**    - 89.4 52.3
Cohere Command R 24.1 20.0 40.0 25.9 37.1    - 32.0 24.3 85.3 20.0 67.4 21.4 60.5    - 73.6 21.0
Cohere Command R+ 71.2 28.0 63.9 32.0 60.2    - 66.3 30.9 89.4 34.0 66.8 29.6 75.9    - 78.3 30.8
Mistral Large 81.3 38.5 77.8 41.2 **75.0**     - **78.7** 40.4 92.9 33.0 71.5 35.6 90.1     - 84.4 34.9
Mixtral 8x22B MoE 48.9 30.0 56.1 36.8 37.2    - 48.2 35.0 92.1 30.0 74.8 34.1 87.7    - 84.8 33.0
Llama 3 8B 26.3 16.0 26.1 23.7 10.1    - 22.2 21.6 68.8 21.0 54.7 23.6 50.0    - 59.8 22.9
Llama 3 70B 42.8 31.0 64.4 45.5 53.4    - 51.8 41.5 93.2 25.0 63.4 27.7 82.7    - 80.0 27.0
o1-mini 75.2 64.0 68.3 66.9 51.4    - 67.3 66.1 89.7 28.0 68.8 38.0 81.5    - 80.3 35.3
o1-preview **86.0** **89.5** 71.1 **84.4** 56.1    - 74.3 **85.8** 91.8 **56.5** 88.3 **69.4** 92.6    - 90.6 **65.9**


for the action _GRAB_, while the BEHAVIOR example outputs three parameters for the state _nextto_ . (3)
Hallucination errors: the objects _kitchen.1_ and _countertop.84_ do not exist in the provided environment.







|Col1|VirtualHome: Gr|
|---|---|
|||
|**Parsing**|**Parsing**|


_inside_ (agent.65, kitchen.counter.230)


No terminal matches ‘.’ in parser

|Col1|Col2|
|---|---|
|**Action-Arg Len**|**Action-Arg Len**|



GRAB(char.65, soap.1000)


GRAB only has one param

|Col1|Col2|
|---|---|
|**Hallucination**|**Hallucination**|



_inside_ (agent.65, kitchen.1)

kitchen.1 is not in the scene



**Model:** Claude-3 Opus
**Task Name:** Drink
**Task ID:** scene_1_861_2


**Model:** Llama3 70B
**Task Name:** Wash Hands
**Task ID:** scene_1_258_2


**Model:** Claude-3 Opus
**Task Name:**
Put groceries in Fridge
**Task ID:** scene_1_837_2



**Model:** Llama3 70B
**Task Name:**
Sorting mail


**Model:** Llama3 70B
**Task Name:**
Sorting books


**Model:** GPT-4o
**Task Name:**
Cleaning stove



|Col1|: Gra|
|---|---|
|||
|**Parsing**|**Parsing**|


_touching_ (envelope.144, envelope.145))


No terminal matches ‘)’ in parser

|Col1|Col2|
|---|---|
|**Action-Arg Len**|**Action-Arg Len**|



_nextto_ (…, hardback.81, coffee_table.3)


_nextto_ only has two params

|Col1|Col2|
|---|---|
|**Hallucination**|**Hallucination**|



_ontop_ (rag.0, countertop.84)

countertop.84 is not in the scene



(a) VirtualHome



(b) BEHAVIOR



Figure 25: Parsing error examples for subgoal decomposition.


**Runtime Errors** 0Figure 26 demonstrates four types of runtime errors for both VirtualHome and
BEHAVIOR : wrong order, missing step, affordance, and additional step errors. Specifically: (1)
Wrong order errors: In the VirtualHome example, the agent needs to plug in _light.411_ but cannot do
so because both hands hold _novel.1000_ and _spectacles.1001_, whereas they were previously empty.
Similarly, in the BEHAVIOR example, the agent drops _soap.0_ before cleaning _top_cabinet.25_ despite
having held the soap beforehand. (2) Missing step errors: In the VirtualHome scenario, the agent
is required to retrieve _food.1000_, but it is inside the closed _freezer.289_ . The BEHAVIOR example
presents the agent tasked with placing _vidalia_onion.67_ on _countertop.26_, yet the onion is inside a
closed container. (3) Affordance errors: For VirtualHome, the agent is asked to plug in _computer.417_
and _mouse.413_, but these items lack plugs in VirtualHome . In the BEHAVIOR example, the agent is
asked to hold _fridge.97_ and slice _carving_knife.0_ . However, the fridge is too large to hold, and the
knife cannot be sliced. (4) Additional step errors: In VirtualHome, the agent is instructed to plug
in _washing_machine.1001_, a step already fulfilled initially. Similarly, in BEHAVIOR, the agent is
asked to open _carton.0_, which was already open in a previous state.


**Goal Satisfaction Errors** Figure 27 depicts missing state, and missing relation errors for both VirtualHome and BEHAVIOR, and includes an example of a missing goal action error for VirtualHome
. Specifically: (1) Missing state errors: In the VirtualHome example, the generated subgoal plan fails
to turn on _light.1002_, which is necessary for achieving the final goal. Similarly, in the BEHAVIOR
example, the subgoal plan fails to open _window.76_ and _window.81_, which are also required for the


43


final goal. (2) In VirtualHome, after execution, the agent does not place _clothes_pants.1001_ on
top of _washing_machine.1000_, as required by the final goal. In the BEHAVIOR example, LLMs
incorrectly instruct the agent to first put _plywood.78_ next to _plywood.79_, and then put _plywood.79_
next to _plywood.80_ . Once the second state is achieved, the state _nextto(plywood.78,_ _plywood.79)_
is no longer satisfied because _plywood.79_ has been moved. (3) Missing goal action error: In the
VirtualHome example, the agent is required to perform the action _DRINK_, but this action is missing
in the generated subgoal plan.



**Wrong Order**



**VirtualHome: Trajectory – Runtime Error**



















|holds_rh(novel.1000)<br>. . .<br>holds_lh(spectacles.<br>1001)<br>plugged_in(light.411)|Model:<br>Claude-3 Opus<br>Task Name:<br>Read book<br>Task ID:<br>scene_1_688_2|
|---|---|
|Precondition<br>_has_a_free_hand_(agent.65) =_False_<br>Historical State<br>_has_a_free_hand_(agent.65) =_True_|Precondition<br>_has_a_free_hand_(agent.65) =_False_<br>Historical State<br>_has_a_free_hand_(agent.65) =_True_|


**Wrong Order**


|holds_rh(food.1000)<br>nextto(agent.65,<br>freezer.289)<br>. . .|Model:<br>Claude-3 Opus<br>Task Name:<br>Put groceries<br>Task ID:<br>scene_1_609_2|
|---|---|
|Precondition<br>_not inside_(food.1000) =_False_<br>Historical State<br>_not inside_(food.1000) =_False_|Precondition<br>_not inside_(food.1000) =_False_<br>Historical State<br>_not inside_(food.1000) =_False_|


|plugged_in(comput<br>er.417)<br>plugged_in(mouse.<br>413)<br>. . .|Model:<br>Claude-3 Opus<br>Task Name:<br>Work<br>Task ID:<br>scene_1_670_2|
|---|---|
|Precondition<br>computer.417_has no plug_<br>Precondition<br>mouse.413_has no plug_|Precondition<br>computer.417_has no plug_<br>Precondition<br>mouse.413_has no plug_|


|Additional|Step|
|---|---|
|_next_to_(agent.65,<br>washing_machine.1001)<br>_plugged_in_(washing_m<br>achine.1001)<br>. . .|**Model:**<br>Claude-3 Opus<br>**Task Name:**<br>Wash clothes<br>**Task ID:**<br>scene_1_27_2|
|Current State<br>_plugged_in_(wm.1001)_= True_<br>Expected State<br>_plugged_in_(wm.1001)_ =False_|Current State<br>_plugged_in_(wm.1001)_= True_<br>Expected State<br>_plugged_in_(wm.1001)_ =False_|



(a) VirtualHome


**: Trajectory – Runtime Error**
































|holds_lh(soap.0)<br>not holds_lh(soap.0)<br>not dusty<br>(top_cabinet.25)<br>. . .|Model:<br>GPT-4o<br>Task Name:<br>Cleaning kitchen<br>cupboard|
|---|---|
|Precondition<br>_holding_(soap.0) =_False_<br>Historical State<br>_holding_(soap.0) =_True_|Precondition<br>_holding_(soap.0) =_False_<br>Historical State<br>_holding_(soap.0) =_True_|


|ontop(vidalia_onion<br>.67, countertop.26)<br>holds_rh(carving_k<br>nife.69)<br>. . .|Model:<br>GPT-4o<br>Task Name:<br>Chopping<br>vegetables|
|---|---|
|Precondition<br>_not inside_(vidalia_onion.67) =_False_<br>Historical State<br>_not inside_(vidalia_onion.67) =_False_|Precondition<br>_not inside_(vidalia_onion.67) =_False_<br>Historical State<br>_not inside_(vidalia_onion.67) =_False_|


|holds_lh(fridge.97)<br>not inside<br>(strawberry.0,<br>fridge.97)<br>sliced(carving_knife.0)|Model:<br>Llama3 70B<br>Task Name:<br>Bottling fruit|
|---|---|
|Precondition<br>fridge.97_ too big to grasp_<br>Precondition<br>carving_knife.0_not sliceable_|Precondition<br>fridge.97_ too big to grasp_<br>Precondition<br>carving_knife.0_not sliceable_|


|Additional|Step|
|---|---|
|_open_(carton.0)<br>_holds_rh_(candle.0)<br>_ontop_(candle.0,<br>coffee_table.33)<br>. . .|**Model:**<br>GPT-4o<br>**Task Name:**<br>Setting up<br>candles|
|Current State<br>_open_(carton.0)_ =True_<br>Expected State<br>_open_(carton.0)_ =False_|Current State<br>_open_(carton.0)_ =True_<br>Expected State<br>_open_(carton.0)_ =False_|



(b) BEHAVIOR


Figure 26: Trajectory runtime error examples for subgoal decomposition.


44


**E.3** **Action Sequencing**


The full results for the trajectory evaluation of action sequencing are presented in Table 12. Detailed
results for goal satisfaction are shown in Table 11. The results from two simulators are provided
separately, with _V_ representing VirtualHome and _B_ denoting BEHAVIOR .


**(1) Executable and Task Success Rate.** For VirtualHome, Mixtral Large achieved the best performance in both execution and task success rates, which is surprising as it outperforms several
LLMs considered more powerful, such as o1-preview, Cluade-3 Opus, GPT-4o, and Gemini-1.5 Pro.
Similarly, Cluade-3 Sonnet outperforms Cluade-3 Opus in execution success rate. We hypothesize
that this is because most tasks in VirtualHome require only 3-5 actions, though they may involve a
large number of objects in the scene. Therefore, it’s crucial to capture key information and focus
only on task-relevant objects. In such cases, some LLMs may "overthink" and act on irrelevant
objects, while Mixtral Large take a more straightforward approach, effectively capturing the essential
information.


For BEHAVIOR, the situation is the opposite: most tasks require long sequences of execution, with
an average of 14.6 steps as shown in Figure 60. Additionally, the preconditions for the actions in
BEHAVIOR are more complex and strict compared to VirtualHome, while the number of interactable
objects in the scene is relatively small. This requires stronger long-term planning and commonsense
reasoning abilities. As a result, LLMs that "think more" win, which explains why powerful, closedsource models achieved the much better performance, especially for o1-preview which uses reasoning
tokens to think much more before generating responses. .


In general, the performance of LLMs in VirtualHome is significantly better than in BEHAVIOR,
which is reasonable considering the task complexity. However, it is noteworthy that even the best
LLMs only completed half of the tasks in BEHAVIOR which indicates there’s still a long way to go
before LLMs can be effectively applied to embodied agents tasks.


**(2)** **Grammar** **Errors.** The occurrence of grammar errors for different LLMs generally aligns
with our overall understanding of their capabilities. That is, the larger the model, the fewer the
grammar errors; newer editions tend to perform better than older ones; and LLMs trained with
high-quality data excel. For instance, o1-preview, Claude-3 Opus, GPT-4o, and Mistral Large made
fewer grammar errors compared to their other family members, indicating advancements in their
language understanding and generation capabilities. This trend holds true for both BEHAVIOR and
VirtualHome tasks.


We studied the cases where LLMs made grammar errors, as shown in Figure 28. Common parsing
errors include unfinished actions or action sequences, often due to LLMs mistakenly stopping
generation early or producing overly long and incorrect action sequences that exceed the context
window. Additionally, LLMs sometimes confuse the length of arguments. For example, although
GPT-4o made very few grammar errors, it still confuses the parameters required for _rinse_ and _wash_ in
VirtualHome, leading to action-arg length errors. Interestingly, _rinse_ seems particularly challenging
for GPT-4o, as it also made another hallucination error involving _rinse_ .


We consider this metric a reflection of the LLMs’ instruction-following and trustworthiness abilities.
LLMs that can better follow instructions are likely to produce more grammatically correct outputs,







































(a) VirtualHome



(b) BEHAVIOR



Figure 27: Goal satisfaction error examples for subgoal decompositions


45


which is crucial for applications requiring high levels of precision and reliability. This underscores
the importance of continuous improvements in model size, training methodologies, and data quality
to enhance the reliability and accuracy of language LLMs.


**(3) Runtime Errors.** The runtime error rate in VirtualHome is lower than BEHAVIOR, the reason
of which has been previously discussed: 1) the action sequences required to accomplish tasks in
BEHAVIOR are longer, and 2) the preconditions for executing an action are more complex and strict.
For both simulators, the most frequent runtime errors are missing steps, as unsatisfied preconditions
are the primary cause of failed action execution.


Figure 29 shows examples of errors made by LLMs. The reasons for making a _missing step_ error are
mainly due to the lack of common sense reasoning ability to satisfy the preconditions for a desired
action, e.g., _getting to the sink_, _holding a soap_ before _washing hands_, or _soaking the brush_ before
_cleaning a stain_ . _Wrong temporal order_ errors often arise from deficiencies in planning abilities, such
as mistakenly _drinking_ after _putting a cup back_ or _grasping a tomato_ before _slicing it_, demonstrating
that LLMs still lack experience or common sense with daily life activities. Affordance errors occur
due to a lack of understanding about properties of objects, like attempting to _type on a mouse_ or
not _assuming a strawberry will become different parts after slicing it_ . For _additional step_ errors, it
may be an issue with in-context memory, where LLMs forget they have already opened a cabinet
and attempt to open it again, or it could be related to reasoning about the common situation of the
initial scene, like _the agent would be standing at the beginning_, therefore no need to _instruct it to_
_stand up again_ . In sum, the ability of commonsense reasoning is crucial for successfully predicting
action sequences since our prompts do not provide detailed information about the environment and
action instructions. LLMs must follow human conventions to reasonably guess the preconditions and
post-effects of the actions.


For the runtime error metrics, o1-preview takes a significant lead. Other commonly acknowledged
powerful LLMs, such as GPT-4-turbo, Gemini 1.5 Pro, and Claude-3 Opus, performed well in certain
areas, they still struggled with other aspects of the metrics.


**(4) Goal Satisfaction.** In general, in VirtualHome, LLMs perform better at satisfying state goals
than relation goals, while in BEHAVIOR, it is vice versa. The reason for this difference is that in
VirtualHome, there are more objects in the scene, and objects can have more complex relations (like
triple relations) compared to BEHAVIOR, where only binary relations exist. In BEHAVIOR, the
preconditions for state actions are more complex, as shown in Tables 28 and 27. Therefore, it is easier
for LLMs to achieve state goals in VirtualHome and relation goals in BEHAVIOR .


However, performance varies across different LLMs. In both simulators, o1-preview achieves
the highest state goal satisfaction rates, which are higher than its relation goal satisfaction rates.
Additionally, Claude-3 Sonnet and Mixtral Large performed exceptionally well in relation goals.


Figure 30 shows examples of errors that prevent LLMs from satisfying goals in a task. Generally, there are two cases where LLMs fail to satisfy a goal: 1) Missed goals: forgetting to
generate actions to achieve the goal, e.g., forgetting to _TOGGLE_ON(laptop.1000)_ to satisfy
_on(laptop.1000)_, which could be due to deficiencies in instruction-following or issues with incontext memory. 2) Wrong actions: mistakenly corresponding goals to incorrect actions, e.g.,
corresponding _onfloors(plywood_78)_ to _RIGHT_PLACE_NEXTTO(room_floor_living_room_0)_ instead of _RIGHT_PLACE_FLOOR(room_floor_living_room_0)_, which could be a lack of reasoning
ability.


46


**Model**



Table 12: Trajectory evaluation results (%) for _action sequencing_ . Full results.


**Goal Evaluation** **Trajectory Evaluation**


**Grammar Error (** _↓_ **)** **Runtime Error (** _↓_ **)**
_Task SR_ _Execution SR_

_Parsing_ _Hallucination_ _Predicate-Arg Num_ _Wrong Order_ _Missing Step_ _Affordance_ _Additional Step_


_V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_ _V_ _B_



Claude-3 Haiku 43.3 26.0 48.5 32.0 **0.0** **0.0** 4.9 6.0 0.3 **0.0** 1.6 7.0 43.3 54.0 1.3 1.0 3.3 1.0


Claude-3 Sonnet 62.9 44.0 67.2 57.0 **0.0** **0.0** 5.6 1.0 0.7 7.9 2.3 11.0 22.9 19.0 1.3 11.0 3.6 2.0


Claude-3 Opus 66.2 51.0 70.8 59.0 **0.0** **0.0** 14.1 **0.0** **0.0** **0.0** 0.7 3.0 14.1 35.0 0.3 3.0 6.2 2.0


Claude-3.5 Sonnet 72.8 60.0 75.4 69.0 **0.0** **0.0** 2.3 **0.0** **0.0** **0.0** 1.0 5.0 19.7 25.0 1.6 1.0 5.2 2.0


Gemini 1.0 Pro 34.4 27.0 45.9 32.0 0.3 7.0 9.2 3.0 2.0 6.0 1.3 13.0 38.7 35.0 2.6 4.0 7.2 4.0


Gemini 1.5 Flash 61.9 40.0 67.2 52.0 **0.0** **0.0** 2.0 **0.0** 0.3 **0.0** 0.3 5.0 29.8 42.0 0.3 1.0 4.3 2.0


Gemini 1.5 Pro 73.1 42.0 83.3 54.0 **0.0** **0.0** 1.6 **0.0** 0.3 **0.0** 0.3 6.0 13.1 39.0 1.3 1.0 5.6 2.0


GPT-3.5-turbo 14.7 16.0 31.8 20.0 35.1 4.0 1.6 7.0 1.3 23.0 0.3 1.0 28.2 36.0 1.6 8.0 2.0 1.3


GPT-4-turbo 57.0 38.0 65.6 45.0 **0.0** **0.0** 1.6 **0.0** 0.3 **0.0** **0.0** 7.0 32.1 47.0 0.3 1.0 3.6 **0.0**


GPT-4o 61.6 47.0 71.1 53.0 0.3 **0.0** **1.3** 1.0 0.3 **0.0** 0.3 9.0 25.2 36.0 1.3 1.0 4.9 **0.0**


Cohere Command R 24.6 16.0 37.7 19.0 0.7 5.0 29.8 13.0 2.0 **0.0** 3.0 8.0 25.2 43.0 2.0 12.0 4.3 4.0


Cohere Command R+ 63.3 27.0 70.2 35.0 **0.0** **0.0** 5.6 1.0 0.7 15.0 0.3 10.0 22.6 39.0 0.7 **0.0** 5.9 15.0


Mistral Large **73.4** 33.0 **83.6** 50.0 **0.0** **0.0** 2.6 **0.0** 0.3 **0.0** 0.3 8.0 12.8 35.0 0.3 6.0 4.9 7.0


Mixtral 8x22B MoE 46.2 30.0 49.5 40.0 **0.0** 3.0 13.1 6.0 0.7 **0.0** 0.7 10.0 34.7 32.0 1.3 9.0 3.0 2.0


Llama 3 8B 21.6 10.0 25.9 16.0 **0.0** **0.0** 41.6 15.0 1.0 9.0 0.3 6.0 31.1 44.0 **0.0** 9.0 **0.3** 5.0


Llama 3 70B 55.7 34.0 63.0 42.0 **0.0** **0.0** 23.3 2.0 1.0 **0.0** 2.0 15.0 **7.9** 38.0 3.0 3.0 7.9 6.0


o1-mini 65.9 56.0 68.9 65.0 0.3 **0.0** 5.2 3.0 3.3 **0.0** 0.3 7.0 21.6 17.0 0.3 6.0 5.9 5.0


o1-preview 71.1 **81.0** 78.4 **91.0** 2.0 **0.0** 8.2 **0.0** **0.0** **0.0** 0.3 **0.0** 34.1 **6.0** 0.3 2.0 8.9 3.0


**E.4** **Transition Modeling**


**E.4.1** **Logic Form Accuracy**


**VirtualHome** The results from VirtualHome (Table 13) illustrate the varied performance of models
across five distinct categories, demonstrating their ability to predict complex logic and predicates for
each action. The model Claude-3 Opus showcased superior performance in terms of all metrics in the
object states category, achieving an F1 score of 63.0%, highlighting its effectiveness in interpreting
complex object state transitions. Notably, Claude-3.5 Sonnet performed exceptionally well in
object orientation, achieving the highest F1 score of 90.9%, which suggests its strong capability in
understanding object orientations like ’facing’. Gemini 1.5 Pro also excels in the object affordance
category on the precision, recall, and f1 score with an F1 score of 77.7%, demonstrating its well
understanding of required object properties for different actions. However, across all models, the
non-spatial relations category displayed generally low scores, with Gemini 1.5 Flash performing best
at a modest F1 of 7.9%. This basically results from the complex logic and corner cases involved in
some non-spatial actions. For example, when predicting action ’grab’, few models consider ’not in a


47


**Wrong Order**



Figure 28: Grammar error examples for _action sequencing_ .


**VirtualHome: Trajectory – Runtime Error**

**Missing Step** **Affordance Error** **Additional Step**





















|…<br>PUTBACK(cup.100<br>0,sink.231)<br>DRINK(cup.1000)<br>…|Model:<br>Gemini 1.5 Flash<br>Task Name:<br>Drink<br>Task ID:<br>scene_1_171_2|
|---|---|
|Precondition<br>_holds_(cup.1000) =_False_<br> Historical State<br>_holds_(cup.1000) =_False_|Precondition<br>_holds_(cup.1000) =_False_<br> Historical State<br>_holds_(cup.1000) =_False_|


**Wrong Order**




|WALK(bathroom.1)<br>RINSE(hands_both<br>.1000)<br>. . .|Model:<br>Gemini 1.5 Flash<br>Task Name:<br>Wash hands<br>Task ID:<br>scene_1_813_2|
|---|---|
|Precondition<br>_next_to_(sink.42) =_False_<br>_holds_(soap.100) =_False_<br> Historical State<br>_next_to(sink.42) =False_<br>_holds(soap.100) =False_|Precondition<br>_next_to_(sink.42) =_False_<br>_holds_(soap.100) =_False_<br> Historical State<br>_next_to(sink.42) =False_<br>_holds(soap.100) =False_|


|WALK(home_office<br>.319)<br>FIND(mouse.413)<br>TYPE(mouse.413)<br>…|Model:<br>Mixtral 8x22b MO<br>Task Name:<br>Work<br>Task ID:<br>scene_1_670_2|
|---|---|
|Affordance<br>mouse.413 can’t be typed|Affordance<br>mouse.413 can’t be typed|


|STANDUP()<br>. . .|Model:<br>Mistral Large<br>Task Name:<br>Set up table<br>Task ID:<br>scene_1_93_1|
|---|---|
|Current State<br>_stand_up_(character.45)_= True_<br> Expected State<br>_stand_up_(character.45)_= False_|Current State<br>_stand_up_(character.45)_= True_<br> Expected State<br>_stand_up_(character.45)_= False_|



**Missing Step** **Affordance Error** **Additional Step**



**: Trajectory – Runtime Error**


























|RIGHT_GRASP(car<br>ving_knife_69)<br>LEFT_GRASP(toma<br>to_61)<br>SLICE(tomato_61)|Model:<br>GPT-4o<br>Task Name:<br>Chopping<br>Vegetables|
|---|---|
|Precondition<br>_not_in_hand(tomato_61)_= _False_<br> Historical State<br>_not_in_hand(tomato_61)_= _True_|Precondition<br>_not_in_hand(tomato_61)_= _False_<br> Historical State<br>_not_in_hand(tomato_61)_= _True_|


|stained(bathtub_35)<br>RIGHT_GRASP(scr<br>ub_brush_0)<br>CLEAN(bathtub_35)|Model:<br>GPT-4o<br>Task Name:<br>Cleaning<br>bathtubs|
|---|---|
|Precondition<br>_soaked_(scrub_brush_0) =_False_<br> Historical State<br>_soaked_(scrub_brush_0) =_False_|Precondition<br>_soaked_(scrub_brush_0) =_False_<br> Historical State<br>_soaked_(scrub_brush_0) =_False_|


|sliced(strawberry_<br>0)<br>RIGHT_TRANSFER_<br>CONTENTS_INSID<br>E(strawberry_0)|Model:<br>Claude-3 Sonnet<br>Task Name:<br>Bottling<br>fruit|
|---|---|
|Affordance<br>strawberry_0 is sliced and not<br>interactable. Should interact<br>with strawberry_0_part0 and<br>strawberry_0_part1|Affordance<br>strawberry_0 is sliced and not<br>interactable. Should interact<br>with strawberry_0_part0 and<br>strawberry_0_part1|


|OPEN(top_cabinet_27<br>…<br>OPEN(top_cabinet_27|Model:<br>Claude-3 Opus<br>)Task Name:<br>)Cleaning up<br>the kitchen|
|---|---|
|Current State<br>_open_(top_cabinet_27)_= True_<br> Expected State<br>_open_(top_cabinet_27)_= False_|Current State<br>_open_(top_cabinet_27)_= True_<br> Expected State<br>_open_(top_cabinet_27)_= False_|



Figure 29: Trajectory runtime error examples for _action sequencing_ .


48


Figure 30: Goal satisfaction error examples for _action sequencing_ .


Table 13: Full results of logic form accuracy for _transition modeling_ in VirtualHome

|Model|Object States<br>Precision Recall F1|Object Orientation<br>Precision Recall F1|Object Affordance<br>Precision Recall F1|Spatial Relations<br>Precision Recall F1|Non-Spatial Relations<br>Precision Recall F1|
|---|---|---|---|---|---|
|Claude-3 Haiku<br>Claude-3 Opus<br>Claude-3 Sonnet<br>Claude-3.5 Sonnet<br>Cohere Command R<br>Cohere Command R+<br>Gemini 1.0 Pro<br>Gemini 1.5 Flash<br>Gemini 1.5 Pro<br>GPT-3.5-turbo<br>GPT-4-turbo<br>GPT-4o<br>Llama 3 8b<br>Llama 3 70b<br>Mistral Large<br>Mixtral 8x22B MoE<br>o1-mini<br>o1-preview|76.0<br>40.1<br>52.5<br>**87.4**<br>**49.2**<br>**63.0**<br>76.6<br>37.4<br>50.3<br>86.1<br>46.7<br>60.5<br>18.0<br>6.8<br>9.9<br>44.9<br>19.0<br>26.3<br>68.4<br>12.3<br>20.4<br>82.3<br>37.6<br>51.6<br>45.3<br>11.9<br>18.8<br>63.5<br>21.9<br>32.5<br>79.3<br>44.2<br>56.7<br>80.2<br>41.5<br>54.6<br>30.8<br>13.7<br>18.9<br>63.5<br>21.9<br>32.5<br>30.0<br>8.0<br>13.0<br>72.0<br>33.0<br>45.0<br>82.5<br>45.9<br>59.0<br>83.0<br>45.1<br>58.5|19.0<br>34.4<br>24.4<br>46.3<br>**96.9**<br>62.6<br>48.1<br>78.1<br>59.5<br>**93.9**<br>**96.9**<br>**95.3**<br>38.7<br>90.6<br>54.2<br>34.6<br>68.8<br>45.9<br>16.3<br>62.5<br>27.9<br>2.0<br>3.1<br>2.5<br>88.2<br>93.8<br>90.9<br>11.4<br>15.6<br>13.2<br>10.1<br>31.3<br>15.3<br>48.0<br>59.4<br>52.8<br>0.0<br>0.0<br>0.0<br>49.0<br>66.3<br>56.6<br>48.0<br>88.0<br>62.0<br>43.0<br>83.0<br>57.0<br>51.3<br>62.5<br>56.3<br>69.0<br>90.6<br>78.4|67.8<br>73.9<br>70.7<br>76.8<br>74.3<br>75.5<br>60.7<br>74.3<br>66.8<br>77.7<br>**75.5**<br>76.6<br>40.2<br>23.0<br>29.2<br>51.0<br>62.1<br>56.0<br>55.3<br>20.1<br>29.6<br>54.4<br>74.7<br>62.9<br>**79.9**<br>**75.5**<br>**77.7**<br>57.2<br>53.1<br>54.9<br>65.9<br>71.0<br>68.4<br>76.2<br>73.7<br>74.9<br>1.6<br>3.2<br>2.1<br>65.0<br>50.0<br>57.0<br>72.0<br>29.0<br>41.0<br>64.0<br>74.0<br>69.0<br>59.8<br>57.1<br>58.5<br>84.7<br>71.4<br>77.5|37.7<br>38.7<br>38.2<br>37.6<br>39.9<br>38.7<br>32.3<br>39.9<br>35.7<br>45.3<br>39.8<br>42.4<br>12.6<br>6.7<br>8.8<br>30.1<br>34.8<br>32.4<br>45.0<br>16.5<br>24.3<br>**47.4**<br>**42.9**<br>**45.0**<br>42.2<br>35.8<br>38.7<br>35.2<br>21.7<br>26.8<br>31.8<br>34.2<br>32.9<br>40.8<br>40.7<br>40.8<br>15.5<br>18.2<br>16.8<br>27.0<br>27.0<br>27.0<br>35.0<br>18.0<br>24.0<br>40.0<br>38.0<br>39.0<br>32.1<br>32.8<br>32.5<br>39.8<br>37.8<br>38.8|2.0<br>1.5<br>1.7<br>10.4<br>**5.2**<br>7.0<br>6.2<br>4.1<br>4.9<br>7.1<br>5.1<br>5.9<br>3.3<br>0.9<br>1.4<br>7.6<br>3.1<br>4.4<br>7.7<br>2.5<br>3.8<br>16.3<br>**5.2**<br>7.9<br>15.5<br>**5.2**<br>7.8<br>1.7<br>0.3<br>0.6<br>3.8<br>1.0<br>1.6<br>14.8<br>5.1<br>7.5<br>0.0<br>0.0<br>0.0<br>5.0<br>2.0<br>3.0<br>3.0<br>1.0<br>1.0<br>12.0<br>4.0<br>6.0<br>5.0<br>4.1<br>4.5<br>**17.1**<br>9.0<br>**11.8**|



closed container’ or ’both hands of the robot is holding something’ as a precondition, and including
the logic ’when the robots hold something with one hand, hold the object with the other hand’ in the
effect is also too challenging for LLMs. This could point to a common challenge in modeling complex
relationships with real-world scenario concerns, requiring perhaps a more nuanced understanding or
a different approach in training.


**BEHAVIOR** In the BEHAVIOR environment (Table 14), the model Claude-3.5 Sonnet shows
outstanding performance, particularly in object states with an F1 score of 78.8% and in spatial
relations with an F1 of 58.6%. These results underscore its exceptional ability to handle both static
state transitions of objects and spatial relations between the robot and the objects. In the Non-Spatial
Relations category, the model o1-preview has significantly better performance than others with an F1
score of 83.5%.


**Overall Conclusions** Therefore, the consistently high performance of Claude-3.5 Sonnet models
across both environments suggests that these models have potentially benefited from training regimens
or architectural features that enhance their understanding of object-oriented and relational aspects of
embodied environments. There are also some divergences between the results of the two environments.
In the non-spatial relation category, Gemini 1.5 Flash performs better in VirtualHome while o1

49


Table 14: Full results of logic form accuracy for _transition modeling_ in BEHAVIOR

|Model|Object States<br>Precision Recall F1|Spatial Relations<br>Precision Recall F1|Non-Spatial Relations<br>Precision Recall F1|
|---|---|---|---|
|Claude-3.5 Sonnet<br>Claude-3 Haiku<br>Claude-3 Opus<br>Claude-3 Sonnet<br>Cohere Command R<br>Cohere Command R+<br>Gemini 1.0 Pro<br>Gemini 1.5 Flash<br>Gemini 1.5 Pro<br>GPT-3.5-turbo<br>GPT-4-turbo<br>GPT-4o<br>Llama 3 70b<br>Llama 3 8b<br>Mistral Large<br>Mixtral 8x22B MoE<br>o1-mini<br>o1-preview|83.3<br>**74.8**<br>**78.8**<br>64.1<br>55.2<br>59.3<br>74.6<br>69.4<br>71.9<br>66.2<br>68.7<br>67.5<br>59.7<br>43.9<br>50.6<br>58.0<br>58.4<br>58.2<br>67.2<br>55.2<br>60.6<br>73.9<br>57.2<br>64.5<br>69.6<br>46.7<br>55.9<br>67.1<br>46.1<br>54.6<br>58.2<br>59.4<br>58.8<br>73.1<br>69.6<br>71.3<br>68.1<br>64.6<br>66.3<br>40.3<br>32.4<br>35.9<br>67.5<br>66.5<br>67.0<br>60.2<br>60.0<br>60.1<br>46.3<br>37.2<br>41.3<br>**85.5**<br>72.3<br>78.3|**73.3**<br>**48.8**<br>**58.6**<br>54.7<br>37.4<br>44.4<br>70.4<br>44.6<br>54.6<br>62.8<br>39.8<br>48.7<br>29.1<br>11.6<br>16.6<br>54.2<br>33.6<br>41.5<br>47.5<br>35.3<br>40.5<br>54.5<br>40.7<br>46.6<br>52.9<br>27.2<br>35.9<br>57.6<br>31.6<br>40.9<br>50.3<br>27.8<br>35.8<br>63.9<br>35.8<br>45.9<br>60.3<br>38.8<br>47.2<br>29.6<br>22.7<br>25.7<br>54.9<br>32.3<br>40.7<br>53.2<br>39.9<br>45.6<br>71.1<br>42.3<br>53.1<br>72.4<br>46.1<br>56.3|82.9<br>66.2<br>73.6<br>63.3<br>51.4<br>56.7<br>68.5<br>69.1<br>68.8<br>68.8<br>52.0<br>59.2<br>27.2<br>15.3<br>19.6<br>53.0<br>56.6<br>54.7<br>43.8<br>48.3<br>45.9<br>60.7<br>53.8<br>57.0<br>59.6<br>47.4<br>52.8<br>40.8<br>36.1<br>38.3<br>58.5<br>38.4<br>46.4<br>84.7<br>64.2<br>73.0<br>65.1<br>53.8<br>58.9<br>48.9<br>43.9<br>46.2<br>59.7<br>44.6<br>51.1<br>57.9<br>55.8<br>56.8<br>80.1<br>58.3<br>67.5<br>**88.0**<br>**79.5**<br>**83.5**|



Table 15: Full results of planner success rate for _transition modeling_ (%)

|Model|Object States<br>V B|Object Orientation<br>V B|Object Affordance<br>V B|Spatial Relations<br>V B|Non-Spatial Relations<br>V B|
|---|---|---|---|---|---|
|Claude-3 Haiku<br>Claude-3 Opus<br>Claude-3 Sonnet<br>Claude-3.5 Sonnet<br>Cohere Command R<br>Cohere Command R+<br>Gemini 1.0 Pro<br>Gemini 1.5 Flash<br>Gemini 1.5 Pro<br>GPT-3.5-turbo<br>GPT-4-turbo<br>GPT-4o<br>Llama 3 8b<br>Llama 3 70b<br>Mistral Large<br>Mixtral 8x22B MoE<br>o1-mini<br>o1-preview|13.5<br>68.9<br>63.5<br>84.4<br>11.2<br>80.0<br>67.4<br>**86.7**<br>44.6<br>48.9<br>36.5<br>77.8<br>10.7<br>22.2<br>34.8<br>55.6<br>**94.4**<br>35.6<br>1.1<br>26.7<br>51.7<br>40.0<br>71.9<br>68.9<br>27.0<br>35.6<br>10.1<br>68.9<br>15.7<br>73.3<br>36.5<br>57.8<br>63.5<br>77.8<br>69.1<br>**86.7**|3.6<br>-<br>71.4<br>-<br>3.6<br>-<br>96.4<br>-<br>82.1<br>-<br>46.4<br>-<br>0.0<br>-<br>7.1<br>-<br>89.3<br>-<br>25.0<br>-<br>50.0<br>-<br>78.6<br>-<br>0.0<br>-<br>3.6<br>-<br>7.1<br>-<br>50.0<br>-<br>82.1<br>-<br>**100.0**<br>-|19.8<br>-<br>58.7<br>-<br>10.8<br>-<br>67.8<br>-<br>40.1<br>-<br>35.3<br>-<br>10.2<br>-<br>46.7<br>-<br>**95.8**<br>-<br>1.2<br>-<br>47.9<br>-<br>63.5<br>-<br>26.4<br>-<br>6.6<br>-<br>14.4<br>-<br>28.1<br>-<br>59.3<br>-<br>67.1<br>-|46.9<br>62.8<br>64.8<br>80.9<br>20.0<br>79.8<br>**96.6**<br>80.8<br>62.6<br>38.3<br>40.7<br>57.4<br>14.5<br>13.8<br>61.4<br>68.1<br>89.0<br>40.4<br>0.0<br>39.4<br>67.6<br>44.7<br>66.9<br>64.9<br>37.9<br>27.7<br>15.2<br>77.7<br>17.9<br>76.6<br>44.1<br>52.1<br>75.9<br>77.7<br>76.6<br>**89.4**|73.0<br>62.3<br>55.4<br>82.0<br>13.5<br>80.3<br>**91.9**<br>80.3<br>58.3<br>39.3<br>31.1<br>47.5<br>2.7<br>14.8<br>60.8<br>70.5<br>83.8<br>39.3<br>0.0<br>54.1<br>64.9<br>52.5<br>68.9<br>68.9<br>31.1<br>26.2<br>18.9<br>85.2<br>8.1<br>80.3<br>43.2<br>57.4<br>71.6<br>75.4<br>78.4<br>**90.2**|



preview performs better in BEHAVIOR . The performance disparity between different environments
also suggests that model training and optimization might need more customization toward specific
types of embodied interactions. Overall, high-performing models in certain categories, such as
Gemini 1.5 Pro in Object Orientation and Claude-3.5 Sonnet in Object States and Spatial Relations,
highlight areas where specific models excel and can be leveraged for tasks requiring high accuracy in
those domains. On the other hand, models like Llama 3 and Cohere Command R+ showed lower
performance, which may indicate limitations in their training datasets or model architectures for
these specific task requirements. Additionally, the generally lower scores in non-spatial Relations
across most models suggest a gap in current modeling capabilities, offering a potential area for future
research and model improvement.


50


|Missing|Object State|
|---|---|
|||
|**Ground Truth**<br>|**Ground Truth**<br>|


|Col1|Col2|
|---|---|
|**Ground Truth**<br>|**Ground Truth**<br>|


|Missing|Spatial Relation|
|---|---|
|<br>||
|**Ground Truth**<br>|**Ground Truth**<br>|









|Col1|Col2|
|---|---|
|**Ground Truth**|**Ground Truth**|


:action clean_stained_brush
:parameters (?scrub_brush ?obj ?agent)
:effects (not ( _stained_ ?obj))




|Col1|Col2|
|---|---|
|**Ground Truth**|**Ground Truth**|


|Col1|Col2|
|---|---|
|**Ground Truth**|**Ground Truth**|


|Col1|Col2|
|---|---|
|**Ground Truth**|**Ground Truth**|



















Figure 31: Transition modeling error examples


**E.4.2** **Planner Success Rate Analysis**


This assessment highlights how well models predict feasible transitions and achieve objectives from
initial states.


**Model Performance Highlights:** o1-preview stands out among all models, demonstrating robust
success rates across various categories and different environments. For example, o1-preview achieves
a 100% success rate in VirtualHome Object Orientation, and a 90.2% success rate in BEHAVIOR
Non-Spatial Relations reflects the internal consistency of its generated operators.


**Spatial vs.** **Non-Spatial Relations:** The data reveals a notable disparity in performance between
spatial and non-spatial Relations, with non-spatial Relations generally showing higher success rates.
This suggests that even though LLM may not cover corner cases in non-spatial relation actions,
models can still simplify it and get away with it compared with spatial relation actions, an insight that
could direct future training enhancements.


**Challenges and Strategic Insights:** Despite successes, persistent challenges in categories like object
orientation, where success rates are generally lower, highlight potential gaps in model training or
architectural design. The variability in success rates across models points to significant differences
in how they interpret and execute tasks within these dynamic environments. Even high-performing
models struggle with precise spatial interactions, a critical area for tasks requiring detailed physical
interactions. For example, GPT-4o struggles at non-spatial relation tasks and Claude-3 Opus is not
doing well at object states and object affordance tasks.


**Future Directions:** The findings suggest valuable pathways for model improvements, particularly
in enhancing relation understanding in physical task execution. Future research might focus on
diversifying training scenarios that incorporate a broader range of spatial and non-spatial interactions
and testing these models in more complex, real-world settings. Additionally, exploring hybrid models
that combine various strengths of current LLMs could yield more capable and flexible systems for
practical applications in embodied AI environments.


**E.4.3** **Error categorization**


In assessing the logical form accuracy of predicted PDDL action bodies by the LLM, we categorize
errors into two main types: missing predicates, and additional predicates as illustrated in figure 31.
Each category reflects specific discrepancies between the LLM outputs and the ground truth, affecting
the precision, recall, and F1 scores of the model’s predictions.


51


**Missing Predicates:** These errors occur when essential predicates are omitted in the LLM’s output,
leading to incomplete or incorrect action specifications. For instance, in the missing object affordance
error, the LLM omits the ‘(lieable ?obj)‘ predicate required for the action LIE, replacing it with
incorrect predicates like ‘(sittable ?obj)‘. Apparently, to lie on some object, the object should be
lieable instead of sittable. Such omissions can drastically affect the feasibility and correctness of the
generated plan, as they fail to capture the necessary conditions for action execution.


**Additional Predicates:** This error type is characterized by the inclusion of unnecessary or incorrect
predicates in the LLM output, which are not present in the ground truth. An example is seen in the
additional object state, where the LLM predicts ‘(not (stained ?scrub_brush))‘ under effects for the
action CLEAN_STAINED_BRUSH. To clean something with a brush, the clean object should be the
target object instead of the brush. These additional predicates can lead to over-constrained planning
scenarios, potentially preventing the execution of valid actions.


Another type of additional predicate is property hallucination. Property hallucination errors arise
when the LLM inaccurately attributes properties to objects that are not supported by the ground truth.
In the depicted error, the LLM predicts that an object is ‘(wearable ?obj)‘, while ‘wearable‘ is not
in the vocabulary dictionary. This type of error not only introduces factual inaccuracies but also
misleads the planning process by enforcing constraints that can never be true.





Figure 32: If we use a high-level planner (PDDL planner) over Ground Truth transition models of _plug_in_,
_walk_towards_ and _switch_on_, the output action sequence can be executed successfully.







52


Figure 33: If we use a high-level planner (PDDL planner) over GPT-4o predicted transition models of _plug_in_,
_walk_towards_ and _switch_on_, the output action sequence can be executed successfully.





Figure 34: If we use a high-level planner (PDDL planner) over the mixture of transtion models, i.e., GPT-4o
predicted transition models of _plug_in_ with ground truth _walk_towards_ and _switch_on_, the output action sequence
cannot be executed successfully.


**Consistency of predicted action space**


We observe the consistency of LLM predicted action space, that is, the complexity of predicted actions
is consistent. Such consistency facilitates the PDDL planner to find possible solutions from an initial
state to a goal state. It is worth noticing that interleaving LLM-predicted actions and ground truth
actions has a lower planner success rate than using only ground truth actions or only LLM-predicted
actions. Figure 32 demonstrates the success case of PDDL planning using ground truth actions, and
figure 33 shows that the actions predicted by GPT-4o also pass the PDDL planner test. However, if
we mix the two action spaces, using _plug_in_ from GPT-4o prediction and _walk_toward_ and _switch_on_
from ground truth, the PDDL planner cannot find a feasible solution for this task. This is because
_plug_in_ from GPT-4o prediction diverges with _switch_on_ from ground truth. In ground truth action
space, to _switch_on_ an object, it needs to be _plugged_in_ and has_switch. Ground truth _plug_in_ can
handle the cases when objects either _has_plug_ or _has_switch_ . In the GPT-4o predicted action space,
although _plug_in_ cannot handle the cases when objects _has_switch_, the LLM gets away with it by not
specifying _plug_in_ in the preconditions of _switch_on_ . Although the definition is not comprehensive,
it can pass the PDDL planner test. However, when mixing the two action spaces together, _switch_on_
requires precondition _plug_in_ . But _plug_in_ cannot handle the cases when objects only _has_switch_
but not _has_plug_ . Thus, the predicted action space maintains consistency, and it is crucial to provide
LLM with the context action definitions when we ask LLM to predict a single action.


53


**E.5** **Correlection with Action Length and Goal Complexity**


In this section, we analyze the factors that influence the goal success rates of GPT-4o’s performance in
action sequencing for BEHAVIOR . Generally, the goal success rate is influenced by task complexity,
as shown in Figure 35. The number of goals within a task, the number of state goals, and the number
of relation goals all adversely affect the success rates. The length of the ground-truth action sequence
follows the same trend, although there are some fluctuations.


Success rates for different actions and predicates are provided in Figure 36. The predicate with the
highest success rate is _open_, which corresponds to the action _OPEN_ that has a 1.00 execution success
rate. This high success rate is because GPT-4o can successfully reason about the preconditions for
this action, as shown in Table 27.


The complexity of action preconditions also adversely affects the success rate. Actions with lower
execution success rates often require tool usage, which is a challenge for LLMs. For example, _Soak_
requires placing the object in a toggled-on sink, _Slice_ requires holding a knife, and _Clean_ requires a
cleaning tool like a rag or a brush, and if cleaning a stain, the cleaning tool needs to be soaked.


Interestingly, the success rate of the same actions performed with the right hand is slightly higher
than those performed with the left hand. The action with the lowest success rate is _SLICE_ . There are
only two cases in BEHAVIOR that require this action, and GPT-4o failed both.





**F** **Sensitivity Analysis**


**F.1** **Motivation and Problem Formulation**


In the current setting of the transition modeling task, we use the entire space of LLM-predicted
actions combined with the PDDL planner to check whether there exists a feasible solution to fulfill
the goals. However, it still lacks finer-grid metrics to show which action fails if the PDDL planner
fails to find a solution. Therefore, we conduct sensitivity analysis to examine how sensitive task
success rates are to specific LLM-predicted actions. In the ground truth space, if after replacing a
specific action with the LLM predicted one, the PDDL planner fails to find a solution, we call the
task is _sensitive_ to this action, and vice versa. This sensitivity analysis provides insights into the


54


0.7


0.6


0.5


0.4


0.3


0.2


0.1


0.0


0.5


0.4


0.3


0.2


0.1


0.0



Action Length


(a) Success Rate vs Action Length


Relation Goals


(c) Success Rate vs Edge Goals



0.5


0.4


0.3


0.2


0.1


0.0


0.4


0.3


0.2


0.1


0.0



Total Goals


(b) Success Rate vs Total Goals


Total State Goals


(d) Success Rate vs Node Goals



Figure 35: BEHAVIOR Success Rate as a Function of Different Cofactors


per-action impact of LLM predictions on the success rate of specific actions within each task category,
highlighting areas where the LLM performs well and where it may require further improvement.
Sensitivity analysis also highlights the actions that are crucial in different task categories, providing
insights for fine-tuning and downstream tasks.


**F.2** **Implementation Details**


We start the sensitivity analysis by setting the action space as the ground truth space. Given the task,
for each relevant action, we replace one ground truth action with the LLM-predicted counterpart and
solve the task with a PDDL planner. Each task is categorized according to Appendix L.5 and we
report the overall sensitivity analysis results and the results by task categories.


**F.3** **Result Analysis**


The sensitivity analysis conducted on a large language model’s predictions for various tasks provides a
detailed view of the model’s robustness and areas necessitating improvement. This analysis evaluates
the impact of replacing specific actions predicted by the model while maintaining other actions as
ground truth across five different task categories: non-spatial relations, object affordance, object
orientation, object states, and spatial relations.


In the _non-spatial relations_ task, the model exhibited a nearly perfect success rate for most actions
except for "grab" and "walk_towards," with success rates of 0.97 and 0.65, respectively. The high
success rates indicate a strong model performance in scenarios where spatial relationships are not a


55


1.0


0.8


0.6


0.4


0.2


0.0


0.8


0.6


0.4


0.2


0.0



(a) Success Rate per Action



(b) Success Rate per Predicate


Figure 36: BEHAVIOR Success Rate Analysis for Actions and Predicates


56














Figure 37: VirtualHome sensitivity analysis per action


Figure 38: Sensitivity analysis for object states tasks in VirtualHome


Figure 39: Sensitivity analysis for object affordance tasks in VirtualHome


57


Figure 40: Sensitivity analysis for object orientation tasks in VirtualHome


Figure 41: Sensitivity analysis for spatial relations tasks in VirtualHome


Figure 42: Sensitivity analysis for non-spatial relations tasks in VirtualHome


58


factor, although the lower rate for "walk_towards" suggests difficulties in predicting movement-related
actions when the spatial context is absent.


For the _object affordance_ task, there were notable variances, with "plug_in" and "put_inside" having
significantly lower success rates of 0.09 and 0.75. This disparity suggests challenges in the model’s
understanding of actions involving complex interactions or manipulations, whereas actions directly
associated with straightforward affordances like "close" and "grab" achieved perfect success.


The _object orientation_ task showed high success rates for most actions, but "turn_to" and "walk_into"
had lower success rates of 0.89 and 0.77. This indicates that the model is proficient with simple
orientation changes yet slightly struggles with actions requiring precise orientation or movement
toward specific objects.


In the _object_ _states_ task, "plug_in" displayed extremely low success rates of 0.09. The action
"plug_in" likely presents conceptual challenges or ambiguities in interpretation, pointing to a critical
need for enhancing the model’s training on the diverse contexts and functionalities of objects.


The _spatial relations_ task results highlight significant challenges, as seen with "walk_towards" and
"walk_into" having success rates of 0.63 and 0.17. These outcomes underscore difficulties in handling
spatially complex predictions and interactions within a spatial context.


From Figure 37, generally, we notice "plug_in" and "walk_towards" as the most challenging actions
for LLM to predict. The preconditions of "plug_in" in VirtualHome contain two cases: either the
device "has_plug", or the device "has_switch". While most LLMs are able to predict "has_plug",
they can hardly catch the other case, which turns out to be quite common in the tasks. The difficulty
of "walk_towards" lies in its complex constraints on spatial relations. Many LLMs either predict
additional constraints or miss some necessary constraints, making the success rate of "walk_towards"
low.


Figure 43: BEHAVIOR sensitivity analysis per action


The analysis conducted on the BEHAVIOR dataset for the tasks of non-spatial relations, object states,
and spatial relations offers insights into the predictive capabilities of the LLM and delineates areas
that necessitate improvements.


In the _non-spatial relations_ task, the analysis revealed that the model performs exceptionally well
for most actions, achieving perfect success rates. However, deviations are observed in the actions
"release" and "slice_carvingknife" with success rates of 0.00 and 0.00, respectively. The low success
rate for "slice_carvingknife" suggests that the model struggles significantly with actions that involve
complex interactions, including tool use and precise movements, potentially due to inadequate
representation in the training data.


For the _object states_ task, similar trends are evident where actions such as "grasp" and "release"
exhibit lower success rates of 0.95 and 0.83, respectively, and "slice" again recorded a success


59


Figure 44: Sensitivity analysis for object states tasks in BEHAVIOR


Figure 45: Sensitivity analysis for spatial relations tasks in BEHAVIOR


Figure 46: Sensitivity analysis for non-spatial relations tasks in BEHAVIOR


60


rate of 0.00. These findings indicate that tasks requiring detailed manipulation of object states are
challenging for the model, pointing to a gap in training on nuanced and precise operations.


The _spatial relations_ task also highlights robust performances in most actions, yet "grasp" and "slice"
show lower success rates of 0.84 and 0.00. The consistent underperformance on "slice" across
different tasks underscores a significant deficiency in the model’s ability to handle precise tool-based
actions, while the challenges with "grasp" suggest difficulties in scenarios that demand fine motor
skills or specific spatial awareness.


Across all categories in the BEHAVIOR dataset, as shown in figure 43, the model’s proficiency
in straightforward actions contrasts with its struggles in more complex or nuanced actions. For
example, "place_inside" shows a rather low success rate in many tasks, probably because it requires
an understanding of both spatial relations like "inside", and non-spatial relations like "holding". The
agent also needs to be careful whether the robot currently is "handsfull". All of these decisions are
challenging for current LLMs. Generally, figure 43 underscores a limitation in the model’s current
training, which may not sufficiently encompass the complexity of real-world interactions or the
specificity required for precise task execution.


Overall, the model performs commendably on tasks involving basic manipulations but shows limitations in complex spatial judgments or detailed object interactions. To address these deficiencies,
it is recommended to enhance the model’s training with more diverse scenarios focusing on underperforming actions, fine-tune using specialized datasets, and conduct a thorough error analysis to
precisely identify and rectify the sources of errors. This strategy will not only improve the model’s
performance but also expand its applicability across varied tasks, reinforcing its utility in complex
real-world applications.


**G** **Pipeline-Based vs Modularized**


**G.1** **Motivation and Problem Formulation**


**Motivation** In previous sections, we examined four modularized ability modules in EMBODIED
AGENT INTERFACE. Despite covering all four abilities, it remains important to understand how
LLMs address complete tasks when provided with natural goals instead of symbolic ones. This is
crucial because, in most settings, only natural language goal descriptions are available, and symbolic


61


goals are often missing. Natural language descriptions can be ambiguous. For instance, the natural
language instruction _cook food_ can be as simple as _cook the chicken then eat it_, whereas its symbolic
goal should include detailed information such as cooked(chicken) and ontop(chicken,
plate) and ontop(plate, table). One advantage of our interface is the support for the
flexible composition of primitive modules. This allows us to first have LLMs parse natural language
goals into symbolic goals using the goal interpretation ( _G_ ) module before conducting further analysis.
Therefore, we implement pipeline-based methods to further investigate such ability of LLMs.


**Problem Formulation** Given an initial state _s_ 0 and a natural language goal _lg_, our objective is to
have LLMs generate a feasible action sequence ¯ _a_ to achieve _lg_ and to decompose _lg_ into a subgoal
trajectory _ϕ_ [¯] .


**G.2** **Implementation Details**


We evaluate LLM abilities through pipeline-based methods in BEHAVIOR environment. Using
the three methods—Goal Interpretation ( _G_ ), Action Sequencing ( _Q_ ), and Subgoal Decomposition
(Φ) provided in EMBODIED AGENT INTERFACE, we can establish two pipelines to obtain ¯ _a_ and _ϕ_ [¯]
respectively. Specifically, by leveraging the rule _G_ : _⟨s_ 0 _, lg⟩→_ _g_, we derive the symbolic goal _g_
corresponding to _lg_ . Then, we (1) apply the rule _Q_ : _⟨s_ 0 _, g⟩→_ _a_ ¯ to get the action sequence ¯ _a_, and
(2) apply the rule Φ : _⟨s_ 0 _, g⟩→_ _ϕ_ [¯] to derive the subgoal trajectory _ϕ_ [¯] .


**G.3** **Result Analysis**


Table 16 presents a comparison of modularized methods and pipeline-based methods for pipelines
_G_ + _Q_ and _G_ + Φ. Our observations indicate the following: (1) The trajectory executable rates are
similar between both methods. (2) Pipeline-based methods suffer from error accumulation due to the
composition of two modules.


These findings are consistent across both pipelines _G_ + _Q_ and _G_ + Φ. We attribute our observations
to two main reasons: First, achieving an executable trajectory requires an understanding of semantics,
which is largely related to the provided environment and vocabulary. Since these factors remain
unchanged between modularized and pipeline-based methods, Language Models (LLMs) generate
comparable executable trajectories for both. Second, pipeline-based methods tend to perform worse
in terms of goal success rate. Errors made by the goal interpretation model mislead the LLMs in
downstream modules, causing them to generate less accurate action and subgoal sequences aimed at
the goal. Nevertheless, despite these challenges, LLMs are still capable of generating feasible and
executable action sequences.


Furthermore, Table 16 demonstrates that while most state-of-the-art (SOTA) LLMs tend to avoid
grammar errors. However, this finding does not hold for less advanced models, including Gemini
1.0 Pro, GPT-3.5-turbo, and some open-sourced LLMs. We attribute this to the fact that most SOTA
LLMs are fine-tuned to follow human instructions more closely, whereas less advanced models either
have smaller sizes or undergo less rigorous tuning strategies. Regarding runtime errors, all LLMs,
regardless of their advancement, tend to miss the necessary steps in their generation process. This
observation also aligns with our findings discussed in Section E.2.


62


Table 16: Pipeline-based evaluation results for (1) _G_ + _Q_ and (2) _G_ + Φ in BEHAVIOR . _G_ : Goal Interpretation.
_Q_ : Action Sequencing. Φ: Subgoal Decomposition. In this table, M means ‘modularized’, whereas P means
‘pipeline-based’.


**Goal Evaluation** **Trajectory Evaluation**

**Grammar Error (** _↓_ **)** **Runtime Error (** _↓_ **)**

**Model** _Task SR_ _Execution SR_

_Parsing_ _Hallucination_ _Predicate-Arg Num_ _Wrong Order_ _Missing Step_ _Affordance_ _Additional Step_
_M_ _P_ _M_ _P_ _M_ _P_ _M_ _P_ _M_ _P_ _M_ _P_ _M_ _P_ _M_ _P_ _M_ _P_
_Goal Interpretation + Action Sequencing_
Claude-3 Haiku 26.0 21.0 32.0 29.0 **0.0** **0.0** 6.0 6.0 **0.0** **0.0** 7.0 6.0 54.0 52.0 1.0 7.0 1.0 17.0
Claude-3 Sonnet 44.0 41.0 57.0 53.0 **0.0** **0.0** 1.0 3.0 **0.0** **0.0** 11.0 14.0 **19.0** **21.0** 11.0 9.0 2.0 12.0
Claude-3 Opus **51.0** **46.0** **59.0** 54.0 **0.0** 1.0 **0.0** **1.0** **0.0** **0.0** 3.0 6.0 35.0 35.0 3.0 3.0 2.0 4.0
Gemini 1.0 Pro 27.0 26.0 32.0 35.0 7.0 5.0 3.0 3.0 6.0 6.0 13.0 14.0 35.0 38.0 4.0 2.0 4.0 11.0
Gemini 1.5 Flash 40.0 35.0 52.0 49.0 **0.0** **0.0** **0.0** 2.0 **0.0** **0.0** 5.0 10.0 42.0 41.0 1.0 **0.0** 2.0 7.0
Gemini 1.5 Pro 42.0 37.0 54.0 **55.0** **0.0** 1.0 **0.0** **1.0** **0.0** **0.0** 6.0 7.0 39.0 35.0 1.0 1.0 2.0 **0.0**
GPT-3.5-turbo 16.0 14.0 20.0 32.0 4.0 1.0 7.0 3.0 23.0 15.0 **1.0** **5.0** 36.0 39.0 8.0 6.0 1.0 3.0
GPT-4-turbo 38.0 32.0 45.0 47.0 **0.0** 1.0 **0.0** **1.0** **0.0** **0.0** 7.0 9.0 47.0 41.0 1.0 1.0 **0.0** **0.0**
GPT-4o 47.0 42.0 53.0 **55.0** **0.0** **0.0** 1.0 3.0 **0.0** **0.0** 9.0 6.0 36.0 35.0 1.0 1.0 **0.0** 4.0
Cohere Command R 16.0 5.0 19.0 9.0 5.0 3.0 13.0 38.0 **0.0** 1.0 8.0 8.0 43.0 31.0 12.0 12.0 4.0 8.0
Cohere Command R+ 27.0 15.0 35.0 29.0 **0.0** **0.0** 1.0 8.0 15.0 14.0 10.0 30.0 39.0 31.0 **0.0** 2.0 15.0 22.0
Mistral Large 33.0 31.0 50.0 38.0 **0.0** **0.0** **0.0** 3.0 **0.0** **0.0** 8.0 14.0 35.0 37.0 6.0 8.0 7.0 5.0
Mixtral 8x22B MoE 30.0 26.0 40.0 36.0 3.0 3.0 6.0 13.0 **0.0** **0.0** 10.0 14.0 32.0 **21.0** 9.0 13.0 2.0 15.0
Llama 3 8B 10.0 0.0 16.0 5.0 **0.0** 2.0 15.0 25.0 9.0 6.0 6.0 11.0 44.0 34.0 9.0 17.0 5.0 14.0
Llama 3 70B 34.0 26.0 42.0 40.0 **0.0** 1.0 2.0 3.0 **0.0** **0.0** 15.0 18.0 38.0 35.0 3.0 5.0 6.0 9.0
_Goal Interpretation + Subgoal Decomposition_
Claude-3 Haiku 29.0 21.0 35.0 40.0 **0.0** **0.0** **1.0** 5.0 **0.0** **0.0** 2.0 2.0 59.0 46.0 3.0 7.0 3.0 16.0
Claude-3 Sonnet 38.0 31.0 43.0 45.0 **0.0** **0.0** 2.0 **3.0** **0.0** **0.0** 3.0 2.0 51.0 47.0 1.0 3.0 3.0 18.0
Claude-3 Opus 39.0 35.0 47.0 45.0 **0.0** **0.0** 3.0 8.0 **0.0** **0.0** 5.0 4.0 45.0 42.0 **0.0** 1.0 5.0 7.0
Gemini 1.0 Pro 23.0 14.0 33.0 30.0 2.0 **0.0** 4.0 10.0 **0.0** 1.0 3.0 **1.0** 51.0 45.0 7.0 13.0 3.0 17.0
Gemini 1.5 Flash 34.0 32.0 42.0 44.0 2.0 1.0 **1.0** **3.0** **0.0** **0.0** 2.0 2.0 53.0 48.0 **0.0** 2.0 3.0 7.0
Gemini 1.5 Pro 31.0 26.0 37.0 38.0 **0.0** 1.0 **1.0** **3.0** **0.0** **0.0** 3.0 2.0 59.0 56.0 **0.0** **0.0** 2.0 **1.0**
GPT-3.5-turbo 24.0 14.0 36.0 27.0 2.0 **0.0** 3.0 12.0 **0.0** 22.0 3.0 **1.0** 52.0 32.0 4.0 6.0 3.0 5.0
GPT-4-turbo 37.0 37.0 47.0 49.0 **0.0** **0.0** 3.0 4.0 **0.0** **0.0** 9.0 8.0 40.0 37.0 1.0 2.0 6.0 6.0
GPT-4o **48.0** **38.0** **55.0** **52.0** **0.0** **0.0** 3.0 4.0 **0.0** **0.0** 5.0 6.0 **37.0** 35.0 **0.0** 3.0 5.0 9.0
Cohere Command R 15.0 8.0 25.0 15.0 21.0 13.0 11.0 32.0 **0.0** 1.0 **0.0** **1.0** 38.0 32.0 4.0 6.0 4.0 12.0
Cohere Command R+ 24.0 17.0 37.0 31.0 2.0 6.0 4.0 10.0 **0.0** 2.0 5.0 7.0 51.0 40.0 1.0 4.0 6.0 14.0
Mistral Large 30.0 22.0 38.0 29.0 1.0 1.0 3.0 12.0 **0.0** 1.0 4.0 5.0 52.0 50.0 2.0 2.0 1.0 5.0
Mixtral 8x22B MoE 27.0 22.0 33.0 29.0 **0.0** **0.0** 4.0 9.0 **0.0** 2.0 2.0 2.0 59.0 45.0 2.0 13.0 **0.0** 17.0
Llama 3 8B 21.0 3.0 29.0 14.0 2.0 7.0 11.0 29.0 **0.0** 2.0 6.0 3.0 44.0 **30.0** 8.0 15.0 7.0 7.0
Llama 3 70B 20.0 19.0 30.0 31.0 1.0 1.0 5.0 22.0 1.0 1.0 8.0 7.0 51.0 35.0 4.0 3.0 4.0 7.0


**H** **Replanning and Feedback**


**H.1** **Motivation and Problem Formulation**


In our current setting, the agent has only one chance to generate plans or make predictions regardless
of previous failures and warnings. However, in reality, it is an important ability for an agent to learn
from feedback and re-plan based on the failure and warning. It demonstrates the agent’s ability to
make agile adjustments based on the simulator execution and environment. Practically, replanning
and feedback also help to prevent the agent from making the same mistakes over and over again.
Therefore, we regard the model’s ability to re-plan from feedback as necessary.[9][81][82]


**H.2** **Implementation Details**


To evaluate the agent’s ability to re-plan from the feedback, for each unsuccessful task, either failed in
the execution (grammar error, runtime error, etc.), or failed in the goal satisfaction check (unsatisfied
state goals, relation goals, or action goals), we construct the feedback as the error message and
necessary information, and append them to the task prompt. We provide the previous agent’s plan as
_"At the [retry_cnt] retry, LLM predict the action sequence to be [predicted_action]"_, and detail the
reason why it fails. For example, for runtime error missing step, the error message will be _Action_

_[action] is not executable in the action sequence [actions]._ _It encounters an error:_ _MISSING STEP._
_Missing step means that action [action] needs some other necessary action before its execution."_ .
Another error message example for unsatisfied goals is _Action sequence [actions] does not satisfy_
_all_ _the_ _goals._ _Please_ _check_ _the_ _action_ _sequence_ _and_ _try_ _again._ _Specifically,_ _the_ _following_ _goals_
_are_ _not_ _satisfied:_ _Node_ _goals_ _not_ _satisfied:_ _[unsatisfied_node_goals]_ _Edge_ _goals_ _not_ _satisfied:_

_[unsatisfied_edge_goals] Action goals not satisfied:_ _[unsatisfied_action_goals]_ . We allow the agent


63


to re-plan at most 3 times, and each time, we append all previous attempts and error messages to the
feedback. We test this setting on the action sequencing task with model GPT-4o.


**H.3** **Result Analysis**


Table 17: Replanning evaluation results (%) for _action sequencing_


**Goal Evaluation** **Trajectory Evaluation**
**Model** **Grammar Error (** _↓_ **)** **Runtime Error (** _↓_ **)**

_Task SR_ _Execution SR_
_Parsing_ _Hallucination_ _Action-Arg Num_ _Wrong Order_ _Missing Step_ _Affordance_ _Additional Step_
GPT-4o 65.2 71.8 **0.0** **1.3** 0.7 **0.0** 25.3 1.0 **0.3**
GPT-4o w/ replanning **77.4** **83.3** **0.0** **1.3** **0.0** **0.0** **14.1** **0.3** 0.7


From table 17, we observe that with replanning, the model gains improvement by more than 10%
on the success rate and executable success rate. Other metrics also gain improvement except for the
additional steps error rate. A possible reason could be that the agent sometimes tries to iterate over
previous attempts and make modifications based on those attempts. Such a strategy makes agents
easily overstate some actions and cause additional step warnings.


**H.4** **Replanning with Stochastic Actions**


We also explore other failing cases, such as action failures in action sequencing. Specifically, we add
a new experiment to simulate stochastic actions with a certain failure probability and allow replanning
three times. The experiments are done on GPT-4o for action sequencing tasks in the BEHAVIOR
simulator.


Table 18: Replanning for Stochastic Actions on BEHAVIOR (Exp.6)


**Fail Probability** **Method** **Execution SR (%)** **Goal SR (%)**


0.05 w/o Replanning 60 50.0
w/ Replanning 85 (↑25) 65 (↑15)


0.1 w/o Replanning 25 15.0
w/ Replanning 70 (↑45) 55 (↑40)


0.2 w/o Replanning 10 5.0
w/ Replanning 65 (↑55) 45 (↑40)


From Table 18, we show that (1) replanning can be helpful; (2) the gap between with and without
replanning generally increases when the failure probability is larger.


64


**I** **Prompt and Analysis**


**I.1** **Prompt of Goal Interpretation**


We design the prompt template of goal interpretation as below:





Figure 47: Examples prompt of goal interpretation.


65


**I.2** **Prompt of Subgoal Decomposition**


We design the prompt template of subgoal decomposition as below:





66


Figure 48: Examples prompt of subgoal decomposition.


**I.3** **Prompt of Action Sequencing**


We design the prompt template of action sequencing as below:





67


68


69


Figure 49: Examples prompt of action sequencing.


**I.4** **Prompt of Transition Modeling**


We design the prompt template of transition modeling as below:







70


71


72


73


Figure 50: Examples prompt of transition modeling.


**I.5** **Prompt of Environment Representation**


We input the object-centric representation as the abstraction of the environment as below:







74


75


Figure 51: Examples prompt of object-centric representation for the embodied environment.


**I.6** **Prompt Analysis and Learned Lessons**


During our empirical experiments, we find many models struggle with outputting strictly formatted parsable output that can be accepted by our embodied agent interface, let alone the various
intricate formats accepted by different simulators. In addition, many models are inclined to output
accompanying explanations along with their answers despite explicit instructions forbidding such
behavior.


To address these problems, we design structured and easy-to-follow code output formats that consist
of JSON and Python list structures in place of long string output, leveraging popular LLMs’ superior
formal language (code) generation ability over natural language.


In order to standardize our evaluation, we use the same prompt for all models evaluated, following
the convention established by [83–85]. More details about this design choice are in Appendix I.7.


Below are some of the other LLM prompting strategies we used in designing our evaluation prompts.
Based on empirical qualitative experiments, prompts under these conditions generated the best results
for our embodied agent interface:


**I.7** **Further Consideration about Prompt Variability**


To ensure fairness in evaluating large language models (LLMs), we adopted a model-agnostic prompt
design. This approach prevents bias that could arise from using model-specific prompts. Through
iterative empirical testing across all models, we ensured that each model could accurately interpret
the prompts, with none exceeding a format error rate of 3.8%.


As discussed in the above section, our prompt design aligns with established evaluation methods [83],
ensuring a scalable and fair approach. Additionally, we conducted ablation studies demonstrating that
model-specific prompt tuning has minimal effect on performance. For example, models like GPT-4o
do not rely on explicit chain-of-thought prompts for reasoning tasks.


To further verify this, we tested model-specific prompt tuning on the VirtualHome goal interpretation
task across three models. The results are visualized in heatmaps (Fig. 52). In our experiments, we
test on "claude-3-opus-20240229", "gpt-4o-2024-05-13", "gemini-1.5-pro-preview-0409". We named
the prompt in the following way:


    - _base_, _general_, and _general_improved_ are the three versions of prompts we used in previous benchmark development. We started from _base_ and finalized the final version of
_general_improved_ prompt, based on which all our results are reported in the paper.


76


(a) Model Performance ranked by Claude-3 Opus.


(b) Model Performance ranked by GPT-4o.


(c) Model Performance ranked by Gemini-1.5 Pro.


Figure 52: Heatmap results showing minor differences between general and model-specific prompts across the
three tested models.


77


    - _{model name}_{number}_ are the model-specific prompts we tuned for each _{model name}_
with _TOP_{number}_ performance gain.


Our findings indicate that model-specific prompts do not lead to significant differences. For example,
in the figures, when Claude performs best with certain prompts, these prompts are also among the
top-performing prompts for other models. This suggests that prompt improvements can be generalized
across large models, supporting our initial point.


**J** **Human Performance Comparison**


To evaluate the performance of Large Language Models (LLMs) relative to human capabilities, we
conducted experiments comparing GPT-4 with human annotations across various tasks. The results
are summarized in Table 19.


Table 19: Comparison of GPT-4 and Human Performance on Different Tasks


**Goal Interpretation** **Action Sequencing** **Subgoal Decomposition** **Transition Modeling** **Average Perf.**
**Method**
_F1_ _Task SR_ _Execution SR_ _Task SR_ _Execution SR_ _F1_ _Planner SR_ _Module SR_
GPT-4o 37.6 42.9 57.1 **70.0** **90.0** 50.0 **100.0** 56.8
Human Annotation **80.6** **57.1** **85.7** 60.0 80.0 **52.9** 66.7 **64.4**


Our findings indicate that while humans outperform GPT-4o in tasks such as Goal Interpretation and
Action Sequencing Execution Success Rate, GPT-4o excels in Subgoal Decomposition and Transition
Modeling. Specifically, GPT-4o achieves a higher Goal Success Rate and Execution Success Rate in
Subgoal Decomposition, and a perfect Planner Success Rate in Transition Modeling. This suggests
that LLMs like GPT-4o may excel in tasks requiring long-context reasoning, such as long-horizon
logical reasoning and large-scale scene graph tracking, while humans perform better in tasks requiring
nuanced understanding and execution.


**K** **Further Discussion on Visual Information in Our Benchmark**


**K.1** **Integration of Visual Inputs in Long-Horizon Decision Making**


While our primary focus is on evaluating long-horizon decision-making capabilities using text-based
inputs and outputs, we acknowledge the critical role of visual information in practical embodied AI
scenarios. To address concerns regarding the assumption that all elements can be described using
text, we conducted additional experiments involving Vision-Language Models (VLMs) to explore
their potential in planning tasks.


**K.1.1** **Challenges with Existing VLMs**


Current VLMs are not specifically designed to process scene-level information from complex environments that include multiple rooms and numerous objects, such as those found in the BEHAVIOR and
VirtualHome simulators. Recent works evaluating VLMs for embodied and online scenarios [86–91]
primarily focus on perception tasks rather than long-horizon decision making.


**K.1.2** **Comparative Experiments with LLMs and VLMs**


To investigate the capabilities of VLMs in long-horizon decision-making, we conducted experiments
comparing Large Language Models (LLMs) and VLMs under various settings on the BEHAVIOR
dataset.


**Experiment Settings** We compare Llama 3 and LlaVA (with Llama 3 as the backbone model) for
LLM and VLM comparison. Below are experimental settings.


    - **Exp.0 (Baseline)** : Scene graph input to LLMs producing planning outputs.


78


    - **Exp.1** : Image input to VLMs producing planning outputs without intermediate scene graphs
(end-to-end approach).

    - **Exp.2** : Image and scene graph inputs to VLMs producing planning outputs.


**Results and Analysis** The results of these experiments are summarized in Table 20.


Table 20: Performance Comparison between LLMs and VLMs on BEHAVIOR


**Model** **Goal Interpretation (F1, %)** **Action Sequencing (Success Rate %)**


Llama 3 **31.5** **11.1**
LLaVA (w/o scene graph) 9.1 2.5
LLaVA (w/ scene graph) 25.8 11.0


These experiments reveal several key insights: (1) LLaVA shows significantly lower performance
than Llama 3 when used end-to-end (Exp.1). This indicates challenges in effectively utilizing visual
inputs for complex planning tasks. (2) Providing scene graphs as additional input to LLaVA (Exp.2)
improves its performance but does not match the level of Llama 3 using scene graphs (Exp.0). This
suggests that current VLMs may not fully leverage visual information for long-horizon reasoning.
(3) End-to-end approaches with VLMs (Exp.1) entangle perception and decision-making errors,
complicating the diagnosis of specific weaknesses in reasoning or planning abilities.


**K.1.3** **Why LLMs Benchmarking is Useful**


Many existing approaches leveraging foundation models include CodeAsPolicies [6], Voyager [35],
VIMA [92], VoxPoser [77]. Therefore, we aim for **standardized evaluation** for LLMs, which can
(1) guide **LLM researchers** for improvements to better support embodied AI; (2) provide **robotics**
**researchers** with selection and modularization of different LLMs and their roles. On top of that, our
experiments reveal a transferable pattern from LLMs to VLMs, shown as follows.


**Experiment Settings** Similar to Exp.0-Exp.2, we compare LLama 3 and LLaVA.


    - **Exp.3:** Scene graph and broader context input to LLMs improving planning output.

    - **Exp.4:** Images, scene graph, and broader input to VLMs to see if planning output is
improved.


Table 21: Insights from LLMs (Exp.3) can be applied to VLMs (Exp.4)


**Model** **Goal Interpretation (F1** _,_ **%) Improvement** **Action Sequencing (SR%) Improvement**

LLama 3 31.5 → 31.9 11.1 → 13.9
LLaVA (w/ scene graph) 25.8 → 25.9 11.0 → 12.3


**Result** **Analysis** Our experiments show that insights from LLM evaluations can be applied to
Vision-Language Models (VLMs) to improve their performance in planning tasks. In Experiment
3, adding scene graphs and broader context to Llama led to improved goal interpretation ( _F_ 1 score
increased from 31.5% to 31.9%) and action sequencing (success rate increased from 11.1% to 13.9%).
Similarly, in Experiment 4, applying the same approach to LLaVA showed smaller but notable
improvements, with the _F_ 1 score increasing from 25.8% to 25.9% and the action sequencing success
rate from 11.0% to 12.3%.


These findings suggest that techniques improving abstract planning in LLMs can also enhance VLM
reasoning, helping the development of more capable robotic systems for real-world tasks.


**K.1.4** **Implications for Future Research**


Our findings highlight the need for further development of VLMs to enhance their long-horizon
decision-making capabilities. Specifically:


79


1. **Improved Multimodal Integration** : VLMs require better mechanisms to integrate visual
and textual information effectively for planning tasks.


2. **Modular Evaluation Approaches** : Decomposed evaluations that isolate perception and
decision-making components can help accurately identify and address specific areas for
improvement.


3. **Methodological Transfer** : Strategies that improve LLM performance, such as providing
structured inputs or broader context, may be adapted to enhance VLM reasoning in embodied
AI tasks.


**K.2** **Impact of Perception and State Estimation Errors**


From the above results, we find that perception errors can significantly affect the planning performance
of models in embodied AI tasks. To assess this impact, we conducted experiments where scene graphs
generated by VLMs were used as input to LLMs for planning. Specifically, we use VLM-generated
scene graphs and then input to LLMs. We focus on subgoal decomposition since it is highly sensitive
to perception performance.


**K.2.1** **Experiment Setup (Exp.5)**


    - **Task** : Subgoal decomposition on the BEHAVIOR dataset.


    - **Input** : Scene graphs generated by VLMs from images.


    - **Models** : Different VLMs used for scene graph generation, followed by LLMs for planning.


**K.2.2** **Results Analysis**


The results are presented in Table 22.


Table 22: Effect of Perception Errors on Subgoal Decomposition


**VLM** **Object Error Rate (%)** **Predicate Error Rate (%)** **Steps Generated Num** **Success Rate (%)**


Claude-3.5-Sonnet **0.0** **8.0** 13 **75.0**
ChatGPT-4 **0.0** 20.0 8 50.0
LLaVA-Vicuna-13B **0.0** 83.0 6 12.5





















Figure 53: Possible VLM scene graph perception error types.


Figure 53 demonstrates four types of VLM perception errors: Unnecessary State, Missing State, Spatial Hallucination, and Object Hallucination. Here, we group missing state and spatial hallucination
as predicate errors and refer to object hallucination as object error. From table 22, the experiments
demonstrate that: (1) Higher predicate error rates in the scene graphs lead to lower success rates in
subgoal decomposition. (2) Accurate perception of object relationships (predicates) is crucial for
effective planning and task execution. (3) Claude-3.5 Sonnet is the strongest VLM among tested
VLMs, with the highest performance at 75.0%.


80


**K.3** **Assumptions on Scene Graphs in Our Benchmark**


Scene graphs are widely used in robotics to represent the environment’s semantic and relational
structure [93–96]. They enable robots to reason about objects and their relationships, which is
essential for complex task planning. Furthermore, generating relational graphs from real-world visual
data is an active research area [93–95]. Inspired by this insight, we adopt scene graphs as perception
results for our benchmark.


**L** **Dataset Statistics and Analysis**


**L.1** **Dataset Structure**


We define a data structure containing a comprehensive annotation for the evaluation of four ability
modules with different levels of planners. Each instance in the dataset represents a task goal, and
each task contains the following data:


1. Natural language task name

2. Natural language task instruction

3. Symbolic goal definition (including its LTL form)

4. Symbolic action trajectory

5. The transition models involved in the task


For tasks in the BEHAVIOR environment, the dataset also includes accompanying VR human
demonstration videos that showcase the execution of the ground truth action trajectories.


Please find our JSON data format in this link: [https://huggingface.co/datasets/](https://huggingface.co/datasets/Inevitablevalor/EmbodiedAgentInterface)
[Inevitablevalor/EmbodiedAgentInterface:](https://huggingface.co/datasets/Inevitablevalor/EmbodiedAgentInterface)


81


Figure 54: We release the task annotations in JSON format. Here is the data instance example for VirtualHome .


Figure 55: We release the task annotations in JSON format. Here is the data instance example for BEHAVIOR .


As shown in Figure 9, we extend the RobotHow [5] data structure, where each task goal includes a natural
language goal description, a VirtualHome action script, and the initial and final states of the scene. In addition,
we offer precise symbolic goals for each task, which enhances the accuracy of evaluation by mitigating the impact
of noisy final states. For instance, it is more precise to denote the final goal of the task “turn on light” as _on(light)_
rather than a noisier version like _facing(agent,_ _light)_ _and on(light)_ . Moreover, we provide transition model
annotations, which require accurate annotation of all the logical constraints of preconditions and post-effects.


For tasks in the BEHAVIOR environment, as shown in Figure 10, the dataset annotation focuses on the action
sequences and transition models. The dataset also includes accompanying demo videos that showcase the
execution of the ground truth action trajectories. For instance, the task “bottling fruit” includes a demo video
showing the agent picking up peaches and placing them inside a jar, along with the corresponding action
sequence and transition model annotations.


The designed data structure is general and can support a systematic evaluation and usage of the data. It enables
the exploration of different integration methods for various modules and can flexibly support downstream
applications. By providing a comprehensive set of annotations, including natural language descriptions, symbolic
goals, action trajectories, and transition models, researchers can investigate the interplay between different
components of embodied AI systems and develop novel approaches for integrating them effectively. Also, it is
not limited to specific simulators and can be expanded to other environments. It has the potential to become
a standardized data format for embodied AI tasks, facilitating the comparison and benchmarking of different
methods across various domains. The consistent representation of goals, actions, and transitions allows for a
unified evaluation framework and promotes the development of generalizable and transferable approaches.


The availability of natural language descriptions alongside symbolic representations enables the exploration of
language grounding and the development of more intuitive human-robot interaction methods. For example, an
agent can be trained to understand and execute natural language commands by grounding the language to the
corresponding symbolic goals and actions.


82


**L.2** **Data Statistics and Distribution**


To understand the datasets used for evaluating long-horizon decision making capabilities for complex goals, we
provide detailed statistics and distribution information in the following sections.


Figure 56: Action sequence length distribution in VirtualHome


Figure 57: Action counts distribution in VirtualHome


Figure 58: Object counts distribution in VirtualHome


**VirtualHome** Table 2 in the main paper presents the statistics of our annotated RobotHow dataset. We
identified a total of 801 goals, comprising 340 state goals, 299 relation goals, and 162 action goals. For
the provided task instructions (referred to as ‘trajectories’ in the table), the average length of action steps is
approximately 8.76, ranging from 2 to 54 steps. Notably, 21 out of 26 task categories include instructions with
action steps exceeding 10 in length, and one-third of the instructions have step lengths of more than 10. This
indicates the complexity of our annotated data.


83


Figure 59: Action object pair counts distribution in VirtualHome


**BEHAVIOR** As shown in Figure 60, the BEHAVIOR dataset has an average of 14.6 actions and 3.7
goals per task. The action length is determined by the number of actions in the ground truth action sequence. The number of goals is calculated by counting the expressions within the brackets of the outermost
connective _and_ in the BEHAVIOR goal, which is written in the Behavior Definition Description Language
(BDDL) format. For example, for a goal expression such as (and (contains ?hot_tub.n.02_1
?chlorine.n.01_1) (filled ?hot_tub.n.02_1 ?water.n.06_1)), we would consider it as
having two goals: the first one is (contains ?hot_tub.n.02_1 ?chlorine.n.01_1), and the
second one is (filled ?hot_tub.n.02_1 ?water.n.06_1).



25


20


15


10


5


0



|Col1|Col2|Col3|Col4|Col5|Avg: 14.6|Col7|Col8|Col9|Col10|Col11|Col12|Col13|Col14|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|<br>|<br>|||||||||||||
|<br>|<br>|||||||||||||
|<br>|<br>|||||||||||||
|<br>||||||||||||||
|<br>||||||||||||||
|<br>||||||||||||||
|<br>||||||||||||||
|<br>||||||||||||||
|<br>||||||||||||||
|<br>||||||||||||||


Action Lengths


(a) Action Lengths



20


15


10


5


0



|Col1|Col2|Col3|Col4|Avg: 3.7|Col6|Col7|Col8|Col9|Col10|Col11|
|---|---|---|---|---|---|---|---|---|---|---|
|<br>|<br>||||||||||
|<br>|<br>||||||||||
|<br>|||||||||||
|<br>|||||||||||
|<br>|||||||||||
|<br>|||||||||||
|<br>|||||||||||
|<br>|||||||||||


BDDL Goals


(b) Number of Goals



Figure 60: BEHAVIOR Task Complexity


**L.3** **Goal Complexity Analysis**


Goals in BEHAVIOR may contain quantifiers, such as forpairs( (?jar.n.01 - jar.n.01)
(?apple.n.01 - apple.n.01)) (inside ?apple.n.01 ?jar.n.01), which are referred to
as BDDL goals. These BDDL goals can be translated into multiple grounded goals. For example, the BDDL goal forpairs( (?jar.n.01 - jar.n.01) (?apple.n.01 - apple.n.01))
(inside ?apple.n.01 ?jar.n.01) could be translated into the grounded goals (and (inside
apple.n.01_1 jar.n.01_1) (inside apple.n.01_2 jar.n.01_2)). Different combinations of grounded goals can satisfy the same BDDL goal, which we refer to as goal options. For instance, (and
(inside apple.n.01_2 jar.n.01_1) (inside apple.n.01_1 jar.n.01_2)) also satisfies the same BDDL goal. Generally, one BDDL goal may have multiple goal options, each consisting of
several grounded goals.


Figure 61 illustrates the distribution of the number of grounded goals and goal options in BEHAVIOR . The
number of grounded goals is determined by sampling a goal option from the BDDL goal and counting the expressions within the brackets of the outermost connective _and_ . For example, (and (inside apple.n.01_2


84


15


10


5


0



|Col1|Col2|Col3|Col4|Col5|Col6|Col7|Avg: 6.7|Col9|Col10|Col11|
|---|---|---|---|---|---|---|---|---|---|---|
|<br>|<br>|<br>|<br>||||||||
|<br>|<br>||||||||||
|<br>|<br>||||||||||
|<br>|<br>||||||||||
|<br>|<br>||||||||||
|<br>|||||||||||
|<br>|||||||||||
|<br>|||||||||||


Grounded Goals


(a) Number of Grounded Goals



60


40


20


0



Goal Options


(b) Number of Goal Options



Figure 61: BEHAVIOR Goal Complexity


jar.n.01_1) (inside apple.n.01_1 jar.n.01_2)) contains two grounded goals. The number
of goal options is calculated by the different possible combinations of grounded goals that satisfy the BDDL
goal. On average, each task in BEHAVIOR has 6.7 grounded goals and 4164.4 goal options.


**L.4** **Task List**


Our dataset’s task selection criteria prioritize long-horizon tasks with complex goals, ensuring a challenging
and comprehensive evaluation of embodied agents. For BEHAVIOR, we choose all the tasks with human
demonstrations to ensure high-quality results. For VirtualHome, we select a typical scene and include all
the tasks that can be executed within this scene. The combination of simulated tasks from VirtualHome and
human-demonstrated tasks from BEHAVIOR allows us to evaluate the generalization capabilities of LLMs
across different domains and levels of complexity. The task lists in Table 23 and Table 24 offer a clear overview
of the selected tasks to understand the scope and variety of the evaluation benchmark.


Table 23: Task List on VirtualHome


Drink 28 Work 35


Turn on light 34 Pick up phone 28


Go to toilet 11 Get some water 1


Wash teeth 11 Read book 28


Make coffee 10 Listen to music 22


Wash dishes by hand 11 Relax on sofa 35


Wash clothes 21 Wash dishes with dishwasher 27


Browse internet 31 Put groceries in Fridge 23


Set up table 8 Write an email 21


Go to sleep 9


Brush teeth 5


The inclusion of a typical scene in VirtualHome enables a focused evaluation of an agent’s performance within
a specific environment. By providing all the executable tasks within this scene, researchers can thoroughly
investigate the agent’s ability to navigate, interact with objects, and complete goals in a constrained setting.


85


This setup facilitates the analysis of the agent’s behavior and helps identify strengths and weaknesses in its
decision-making process. The selected tasks in VirtualHome cover a wide range of household activities, such as
cooking, cleaning, and object manipulation. For instance, the task “cook some food” involves multiple steps,
including gathering ingredients, using kitchen appliances, and setting the table. Another example is the task
“wash dishes by hand”, which requires the agent to perform actions like picking up objects, wiping surfaces,
and putting things back in their designated places. These tasks showcase the complexity and diversity of the
scenarios.


In the BEHAVIOR dataset, the chosen tasks are based on real-world human demonstrations, ensuring the
authenticity and practicality of the tasks. These tasks encompass various domains, such as object manipulation,
tool use, and goal-directed behaviors. For example, the task “bottling fruit” requires the agent to grasp peaches,
cut them, and place them inside a jar, demonstrating object manipulation skills. The inclusion of these humandemonstrated tasks in BEHAVIOR provides valuable insights into real-world challenges and helps to evaluate
the performance of embodied agents in more realistic settings.


Table 24: Task List on BEHAVIOR . Length is the action trajectory length.


**Task Name** **Length** **Task Name** **Length**

assembling_gift_baskets 32 brushing_lint_off_clothing 13

boxing_books_up_for_storage 16 collecting_aluminum_cans 12

mopping_floors 15 preserving_food 20

re-shelving_library_books 8 polishing_silver 6

packing_boxes_for_household_move_or_trip 20 cleaning_freezer 15

installing_a_modem 3 cleaning_up_after_a_meal 31

putting_away_Christmas_decorations 17 setting_up_candles 12

cleaning_shoes 10 cleaning_sneakers 30

locking_every_window 4 washing_cars_or_other_vehicles 8

washing_dishes 11 putting_away_Halloween_decorations 15

collect_misplaced_items 11 cleaning_high_chair 4

washing_floor 8 sorting_mail 12

cleaning_bathrooms 12 cleaning_kitchen_cupboard 18

preparing_salad 34 putting_leftovers_away 18

preparing_a_shower_for_child 8 cleaning_stove 14

putting_dishes_away_after_cleaning 17 packing_child_s_bag 13

cleaning_windows 13 bottling_fruit 21

thawing_frozen_food 20 setting_mousetraps 8

washing_pots_and_pans 24 laying_wood_floors 8

putting_away_toys 16 cleaning_garage 19

watering_houseplants 14 installing_alarms 6

organizing_boxes_in_garage 12 putting_up_Christmas_decorations_inside 18

cleaning_bathtub 8 clearing_the_table_after_dinner 14

loading_the_dishwasher 13 filling_an_Easter_basket 26

cleaning_the_pool 8 opening_packages 6

packing_bags_or_suitcase 15 cleaning_up_refrigerator 23

making_tea 13 picking_up_trash 10

polishing_furniture 4 organizing_school_stuff 15

cleaning_cupboards 24 installing_a_printer 3

packing_food_for_work 12 storing_the_groceries 23


**L.5** **Task Categorization**


To have a better insight into LLM’s abilities in predicting transition modeling from different perspectives, we
group all the predicates in PDDL into five categories: object affordance, object states, object orientation, spatial
relations, and non-spatial relations.


86


In our approach, we categorize programs based on the most dominant predicate category, employing Inverse
Document Frequency (IDF) to measure the significance of predicates within each program. Given a set of
programs _P_ = _{p_ 1 _, p_ 2 _, . . ., pn}_ and a corresponding set of predicates _T_ = _{t_ 1 _, t_ 2 _, . . ., tm}_, each predicate
_ti_ is associated with a specific category _Cj_ within a predefined set of categories _C_ = _{c_ 1 _, c_ 2 _, . . ., ck}_ . Each
action _ai_ corresponds to a set of predicates _Ta_ _[′]_ _i_ [=][ �] _ti∈_ PDDL( _ai_ ) _[t][i]_ [according to its PDDL definition][ PDDL][(] _[a][i]_ [)][.]



action _ai_ corresponds to a set of predicates _Tai_ [=][ �] _ti∈_ PDDL( _ai_ ) _[t][i]_ [according to its PDDL definition][ PDDL][(] _[a][i]_ [)][.]

Each program _pi_ can be solved by a PDDL planner and has a corresponding action sequence _Ai_ . The predicate
set _Ti_ is defined as the unification of the predicate sets of all actions involved in action sequence _Ai_ . Formally,
_Ti_ = [�] _aj_ _∈Ai_ _[T]_ _a_ _[ ′]_ _j_ [=] _[ {][t]_ _i_ 1 _[, t]_ _i_ 2 _[, . . ., t]_ _il_ _[}]_ [, where] _[ l]_ [ can vary between programs.]


The task categorization process begins with the calculation of the IDF for each predicate across the corpus of
programs. The IDF for a predicate _t_ is computed as follows:



_aj_ _∈Ai_ _[T]_ _a_ _[ ′]_ _j_ [=] _[ {][t]_ _i_ 1 _[, t]_ _i_ 2 _[, . . ., t]_ _il_ _[}]_ [, where] _[ l]_ [ can vary between programs.]




    - _|P_ _|_
IDF( _t_ ) = log
df( _t_ )







where _|P_ _|_ denotes the total number of programs and df( _t_ ) is the number of programs that contain the predicate
_t_ . This metric quantifies the importance of a predicate relative to its frequency across all programs, thereby
enhancing the significance of rarer predicates.


Following the computation of the IDF values, each program _pi_ is scored for each category _cj_ by summing the
IDF values of the predicates belonging to _cj_ :


_S_ ( _cj, pi_ ) =              - IDF( _t_ )


_t∈pi,t∈cj_


Each program _pi_ is assigned to its top _K_ scoring categories based on the accumulated IDF scores. This is
formalized as follows:
Categories( _pi_ ) = top _K_ _{_ ( _cj, S_ ( _cj, pi_ )) _| cj_ _∈_ _C}_
where top _K_ selects the set of _K_ categories with the highest scores for each program.


This scoring and categorization method emphasizes the content-specific importance of predicates within programs, assigning categories based on the most informative predicate category present in each program. This
approach is particularly effective in distinguishing the dominant themes of programs when predicates are
unevenly distributed across categories, thus providing a robust basis for categorization in large-scale and diverse
datasets.


We categorize tasks with _K_ = 2. Table 25 illustrates the number of tasks under each task category in VirtualHome
and BEHAVIOR.


Table 25: Results on Task Categorization, showing task number in each task category.


**Object States** 178 45

**Object Orientation** 167               
**Object Affordance** 28              
**Spatial Relations** 145 94

**Non-spatial Relations** 74 61


**M** **Annotation Details**


**M.1** **Simulator Comparison and Selection**


The VirtualHome, BEHAVIOR, and AI2-THOR simulators each offer unique capabilities tailored to different
aspects of embodied agents. VirtualHome is ideal for long, complex household tasks, supporting a wide range of
scripted actions and detailed object states within home environments. BEHAVIOR provides a broad spectrum of
human activities with high goal complexity, realistic physics, and large, dynamic state space, making it suitable
for testing generalization across diverse scenarios. AI2-THOR focuses on photorealistic indoor environments,
emphasizing detailed perception and manipulation tasks with rich real-time interactions and substantial state
space.


In detail, we compare their key differences in terms of task length, goal complexity, task scenarios, actions, and
state space:


**Task Length and Goal Complexity**


87


     - VirtualHome is designed to simulate longer sequences of everyday activities, involving complex
multi-step goals with many objects. It involves sequences of 10-30 high-level actions on average.


     - **BEHAVIOR** has the longest task length and most complex multi-step trajectories. Its ranges from 50
to over 1000 low-level actions in length.


     - **AI2-THOR** tasks are generally shorter, centered around navigation and basic object interactions with
around 10 steps.


**Task Scenarios**


     - **VirtualHome** specializes in simulating complete residential apartments/houses with furnished rooms
to enable realistic household activity scenarios such as _making coffee_ or _brush teeth_ . It has around
20,000 unique household activity scenarios across 57 furnished residential environments.


     - **BEHAVIOR-100** provides 100 different scripted human behavior scenarios across various indoor
environments.


     - **AI2-THOR** focuses more on navigation tasks and low-level object interactions in diverse 3D indoor
scenes. It has over 120 unique room configurations across 8 different room types like bathrooms,
living rooms, etc.


**Action Space and State Space**


     - **VirtualHome** : Represents activities as high-level action sequences like " _<char0> [PutBack] <glass>_
_(1) <table>_ ", enabling simulation of complex multi-step activities. It has around 300 unique high-level
action types.


     - **AI2-THOR** : Has a larger state space with many possible environment configurations, i.e., allowing
over 30 different low-level interactions like open, pick-up, and toggle on/off across 200+ unique object
types. It has an estimated state space of over 10 [25] possible environment configurations.


     - **BEHAVIOR** : Provides low-level scripted actions like object grasp and movements. It provides over
100 unique low-level action types like grasps, movements, etc.


**Environments and Assets**


     - **VirtualHome** : Has 57 fully furnished residential environments with over 3000 unique 3D object
assets.


     - **BEHAVIOR** : Provides 50 different indoor environments without physics simulation.


     - **AI2-THOR** : Contains over 120 unique room configurations with over 1000 3D object assets.


Given our focus on embodied decision-making, which emphasizes the ability to navigate complex goals and
extended action sequences, we have selected VirtualHome and BEHAVIOR for our research. VirtualHome is
particularly suited for simulating long, intricate household tasks with its extensive high-level action sequences
and richly detailed environments. BEHAVIOR complements this by offering a diverse set of human activities
with detailed, low-level scripted actions, enabling comprehensive testing of decision-making capabilities across
various scenarios. Together, these platforms provide a robust framework for evaluating and enhancing the
decision-making abilities of embodied agents in complex, goal-oriented tasks.


**M.2** **BEHAVIOR**


For BEHAVIOR-100, we provide 2 additional sets of manual annotations: ground truth action sequences and
natural language task descriptions. The annotation is done based on observing the real demonstrations [4].


**M.2.1** **Annotating Action Sequence based on VR Human Demonstrations**


The BEHAVIOR dataset consists of 100 tasks defined using the Behavior Domain Definition Language (BDDL),
which is similar to the PDDL format. Each BDDL file has three parts: a list of objects, initial states, and goal
states. To build the transition model for our simulator, we annotated the ground-truth action sequence for each
task. The annotation process involves an automatic pipeline for objects’ state tracking and segmentation from
the demo, followed by manual effort for mapping state changes to action sequences.


**M.2.2** **Complicated Goals with Quantifiers**


BDDL goals may contain quantifiers, such as forpairs( (?jar.n.01 - jar.n.01)
(?apple.n.01 - apple.n.01)) (inside ?apple.n.01 ?jar.n.01), which need to be


88


translated into grounded goals, e.g., and ((inside apple.n.01_1 jar.n.01_1) (inside
apple.n.01_2 jar.n.01_2)). There can be different grounded goals that satisfy the same BDDL goal,
which we refer to as goal options. For example, ((inside apple.n.01_2 jar.n.01_1) (inside
apple.n.01_1 jar.n.01_2)) also satisfies the aforementioned BDDL goal. In general, one BDDL goal
may have a number of goal options, and each goal option has a number of grounded atomic goals.


**M.2.3** **Annotating Transition Models**


**PDDL Problem File Annotation.** We devised an automatic pipeline to generate PDDL problem files from
BDDL files defined for tasks in BEHAVIOR . Specifically, we would sample a solvable goal option from the
BDDL goals, and select max _num grounded goals from this option as the goal for the PDDL problem file,
then collect relevant objects and initial conditions for the grounded goals. This is done to ensure that the PDDL
planner can solve each problem within the desired time frame.


**PDDL Domain File Annotation.** The PDDL domain file is modified from an existing PDDL domain file
written for BEHAVIOR [§] . To make it compatible with our problem file and run in a PDDL planner, we modified
the naming for object types, added new predicates, and changed the preconditions and post-effects for some
operators.


**M.3** **VirtualHome**


We build our annotation of VirtualHome on top of RobotHow [5], which provides a categorized list of household
tasks, each with a varying number of instructions. Each instruction includes a natural language goal description,
a VirtualHome action script, and the initial and final states of the scene. However, RobotHow does not offer
precise symbolic goals for each task, which limits the accuracy of evaluation due to noisy final states. For
instance, it is more precise to denote the final goal of the task “ _turn on light_ ” as _on(light)_, rather than a noisier
version like _ontop(agent, chair) and on(light)_ .


To enhance the reliability and standardization of evaluation outcomes, we manually annotated 338 task instructions across 30 task categories. Our annotation process involved two main steps:


**M.3.1** **Wildcard Representations:** **Deriving Common Task-Related Final States**


We identified common final states for all instructions within the same task category by using wildcards and
object properties. This allows for multiple solutions for each task. For example, we use the property _clothes_ to
represent both _pants_ and _shirts_ . This step provides each task with a wildcard representation.


**M.3.2** **Extracting and Annotating Abstract Goals**


Based on the wildcard representations, we extracted all concrete goals and manually added any missing ones
(which are usually few). We annotated the following three types of goals:


     - State Goals: Represent object and agent states in the current scene, such as _plugged_in(TV)_ .


     - Relation Goals: Represent the spatial relationships that should be satisfied at the end of execution,
such as _ontop(agent, chair)_ .


     - Action Goals: Represent actions that must be performed as part of the task instruction, especially those
without post-effects. For example, in the task “ _pat cat_ ”, the agent is required to perform the action
_touch_ even though it has no post-effect in the final scene state.


**M.3.3** **Grounding to Concrete Goals from Abstract Goals**


In the VirtualHome simulator, each goal consists of an abstract goal and a concrete goal. An abstract goal is
designed to be general and applicable to all potential trajectories. It may contain wildcards to represent similar
objects, such as _book_, _notebook_, or _novel_ . On the other hand, concrete goals are grounded in specific objects
with their unique IDs, making them concrete and actionable.


The conversion between an abstract goal and a concrete goal involves mapping the wildcards to relevant objects
in the scene. This process transforms the abstract goals into specific, tangible objectives that can be directly
acted upon by the embodied agent. By replacing the wildcards with the appropriate object IDs, the abstract goals
are instantiated into concrete goals that are tailored to the specific environment and the available objects. For
example, an abstract goal like "pick up a _book_ " may be converted into a concrete goal such as "pick up _book_1_ "
or "pick up _novel_2_ ", depending on the specific objects present in the scene and their corresponding IDs. This


[§https://github.com/wmcclinton/downward/blob/main/behavior_full.pddl](https://github.com/wmcclinton/downward/blob/main/behavior_full.pddl)


89


conversion process allows the embodied agent to have a clear understanding of the exact objects it needs to
interact with to achieve the desired goal.


The mapping of wildcards to relevant objects is a crucial step in bridging the gap between the high-level, abstract
goals and the low-level, executable actions. It enables the embodied agent to ground the goals in the context of
the specific environment it operates in, making the goals more precise and achievable.


**M.3.4** **Annotating Transition Models**


In addition to the goals, we also annotate the transition model of preconditions and post-effects for each action
in VirtualHome using standard PDDL formulations. Since VirtualHome does not provide an existing PDDL
domain file, we define the predicate list and provide a PDDL implementation for each action.


The predicate list includes all relations (e.g., _inside_, _facing_ ), object states (e.g., _plugged_in_, _closed_ ), and object
properties related to actions (e.g., _grabbable_, _has_switch_ ). The PDDL version of the actions follows the same
preconditions and effects as their implementation in VirtualHome Github. The logic involves _and_, _or_, _not_, _when_,
_exists_, and _forall_ . Approximately 10 actions in VirtualHome simulate human body movement and have no effects
on the environment. For example, the action _read_ requires the robot to hold something readable but has no
effects. Such actions would possibly be left out in the success rate by planner metrics because they would not
contribute to the achievement of goals. However, we still provide their PDDL definitions and task the LLM to
predict them, which is evaluated by the logic form accuracy metric.


**M.4** **Quality Verification**


To verify the quality of the annotations, we employed both automated and human evaluation methods:


     - **Automated Verification** : We utilized the simulators (VirtualHome and BEHAVIOR) to automatically
check the consistency and correctness of the annotations. This included verifying that the annotated
actions and goals were executable and led to the desired outcomes within the simulation environments.


     - **Human** **Evaluation** : In addition to automated checks, we performed human evaluations of the
annotation quality. This involved assessing attributes such as action accuracy, action coverage, and
overall human preference for the annotations.


**M.4.1** **Annotation Quality Evaluation**


The quality of the annotations was quantitatively evaluated using the following metrics:


Table 26: Annotation Quality Evaluation Metrics


**Attribute** **Mean Score** **Weighted MSE**


Action Accuracy 3.73 0.4062

Action Coverage 4.07 1.8438

Human Preference 4.27 0.8125


**Explanation of Metrics:**


     - **Action Accuracy** : Measures how accurately the annotated actions correspond to the required actions
for task completion.


     - **Action Coverage** : Assesses the extent to which the annotations cover all necessary actions for task
execution.


     - **Human** **Preference** : Reflects the subjective preference of human evaluators for the quality and
naturalness of the annotations.


To assess the variability of scores across tasks and evaluators, we calculated the Weighted Mean Squared Error
(MSE):


     - For tasks evaluated by multiple annotators, we computed the MSE per attribute by averaging the
squared differences between individual scores and the mean score for that task.


90


     - We then computed a weighted mean of these MSEs across all tasks, with weights based on the
frequency of evaluations per task.


This method balances individual task variability with overall assessment consistency, accounting for varying
numbers of evaluations per task.


Figure 62: Score distribution visualization.


Our annotation process combines human expertise, iterative refinement, and automated checks to ensure a
high-quality and reliable dataset. The evaluation metrics indicate that the annotations are of high quality, with
mean scores above 3.7 out of 5 for all attributes and relatively low Weighted MSE values, demonstrating
consistency among annotators. We also visualize the score distribution in Figure 62.


**N** **Simulator Implementation Details**


**N.1** **BEHAVIOR Implementation Details**


We developed an environment for evaluating LLMs in behavioral tasks, which includes two types of simulators.
The first simulator is based on the iGibson framework [97], supporting visual rendering and physical simulations
via PyBullet. The second simulator is symbolic, recording activities in a graph-based format. Both simulators
share the same set of actions and object states.


**N.1.1** **Building the Symbolic Simulator and Transition Model Implementation**


Details about the arguments, preconditions, and post-effects of each action are presented in Table 27. An object’s
interactability is defined as (1) The object is not enclosed within another object, and (2) The object is within the
agent’s reach. However, as we currently allow auto-navigation for the agent, which automatically invokes the
NAVIGATE_TO action, this reachability requirement is not necessary for LLMs to fulfill.


Table 27: BEHAVIOR Symbolic Simulator Implementation: Action Transition Models.



**Action Name** **Arguments** **Preconditions** **Post Effects**

NAVIGATE_TO tar_obj1 tar_obj1 is interactable. The agent is next to
tar_obj1.





Continued on next page



91


Table 27 – continued from previous page

**Action Name** **Arguments** **Precondition** **Post Effects**

RIGHT_GRASP tar_obj1 tar_obj1 is interactable; The tar_obj1 is in the agent’s
agent’s right hand is empty; right hand.
tar_obj1 is small enough.



LEFT_RELEASE tar_obj1 tar_obj1 is interactable; The
agent’s left hand holds
tar_obj1.


RIGHT_RELEASE tar_obj1 tar_obj1 is interactable; The
agent’s right hand holds
tar_obj1.


LEFT_PLACE_ONTOP tar_obj1 tar_obj1 is interactable; The
agent’s left hand holds an
object.


RIGHT_PLACE_ONTOP tar_obj1 tar_obj1 is interactable; The
agent’s right hand holds an
object.



tar_obj1 is released to the
floor.


tar_obj1 is released to the
floor.


The object in the agent’s left
hand is on top of tar_obj1.



RIGHT_PLACE_ONTOP tar_obj1 tar_obj1 is interactable; The The object in the agent’s
agent’s right hand holds an right hand is on top of
object. tar_obj1.


LEFT_PLACE_INSIDE tar_obj1 tar_obj1 is interactable; The The object in the agent’s left
agent’s left hand holds an hand is inside tar_obj1.
object; tar_obj1 is big
enough and open if openable.



The object in the agent’s left
hand is inside tar_obj1.



RIGHT_PLACE_INSIDE tar_obj1 tar_obj1 is interactable; The
agent’s right hand holds
an object; tar_obj1 is big
enough and open if openable.


LEFT_PLACE_NEXTTO tar_obj1 tar_obj1 is interactable; The
agent’s left hand holds an
object.


RIGHT_PLACE_NEXTTO tar_obj1 tar_obj1 is interactable; The
agent’s right hand holds an
object.



The object in the agent’s
right hand is inside tar_obj1.


The object in the agent’s left
hand is next to tar_obj1.



RIGHT_PLACE_NEXTTO tar_obj1 tar_obj1 is interactable; The The object in the agent’s
agent’s right hand holds an right hand is next to
object. tar_obj1.


LEFT_PLACE_UNDER tar_obj1 tar_obj1 is interactable; The The object in the agent’s left
agent’s left hand holds an hand is under tar_obj1.
object.



The object in the agent’s left
hand is under tar_obj1.



RIGHT_PLACE_UNDER tar_obj1 tar_obj1 is interactable; The
agent’s right hand holds an
object.



LEFT_TRANSFER_
CONTENTS_INSIDE



tar_obj1 tar_obj1 is interactable; The
agent’s left hand holds an
object; tar_obj1 is big
enough and open if openable.



The object in the agent’s
right hand is under tar_obj1.


The contents inside the object in the agent’s left hand
are in tar_obj1.


The contents inside the object in the agent’s right hand
are in tar_obj1.



RIGHT_TRANSFER_ tar_obj1 tar_obj1 is interactable; The
CONTENTS_INSIDE agent’s right hand holds
an object; tar_obj1 is big
enough and open if openable.



LEFT_TRANSFER_
CONTENTS_ONTOP



LEFT_TRANSFER_ tar_obj1 tar_obj1 is interactable; The The contents inside the obCONTENTS_ONTOP agent’s left hand holds an ject in the agent’s left hand

object. are on top of tar_obj1.


RIGHT_TRANSFER_ tar_obj1 tar_obj1 is interactable; The The contents inside the obCONTENTS_ONTOP agent’s right hand holds an ject in the agent’s right hand
object. are on top of tar_obj1.



tar_obj1 tar_obj1 is interactable; The
agent’s left hand holds an
object.



RIGHT_TRANSFER_ tar_obj1 tar_obj1 is interactable; The The contents inside the obCONTENTS_ONTOP agent’s right hand holds an ject in the agent’s right hand
object. are on top of tar_obj1.









Continued on next page



92


Table 27 – continued from previous page

**Action Name** **Arguments** **Precondition** **Post Effects**

RIGHT_PLACE_ tar_obj1, tar_obj1 and tar_obj2 are in- The object
NEXTTO_ONTOP tar_obj2 teractable; The agent’s right
hand holds an object.



RIGHT_PLACE_ tar_obj1, tar_obj1 and tar_obj2 are in- The object in the agent’s
NEXTTO_ONTOP tar_obj2 teractable; The agent’s right right hand is next to tar_obj1
hand holds an object. and on top of tar_obj2.


TOGGLE_ON tar_obj1 tar_obj1 is interactable; One tar_obj1 is toggled on.
of the agent’s hands is
empty; tar_obj1 is toggled
off and closed.



tar_obj1 is toggled on.



TOGGLE_OFF tar_obj1 tar_obj1 is interactable; One
of the agent’s hands is
empty; tar_obj1 is toggled
on.


CLOSE tar_obj1 tar_obj1 is interactable; One
of the agent’s hands is
empty; tar_obj1 is open.


OPEN tar_obj1 tar_obj1 is interactable; One
of the agent’s hands is
empty; tar_obj1 is closed
and not toggled on.


CLEAN tar_obj1 tar_obj1 is interactable;
tar_obj1 is dusty or stained;
the agent has a cleaning
tool.



tar_obj1 is toggled off.


tar_obj1 is closed.


tar_obj1 is open.


tar_obj1 is not dusty or
stained (according to the
tool).



DRY tar_obj1 tar_obj1 is interactable; tar_obj1 is dry.
tar_obj1 is soaked.



SLICE tar_obj1 tar_obj1 is interactable;
tar_obj1 is not sliced; the
agent has a knife.


SOAK tar_obj1 tar_obj1 is interactable; One
of the agent’s hands is
empty; tar_obj1 is dry and
in a toggled on sink or a pot.


FREEZE tar_obj1 tar_obj1 is interactable; One
of the agent’s hands is
empty; tar_obj1 is unfrozen
and in the fridge.



tar_obj1 is sliced.


tar_obj1 is soaked.


tar_obj1 is frozen.



UNFREEZE tar_obj1 tar_obj1 is interactable; tar_obj1 is unfrozen.
tar_obj1 is frozen.



**N.1.2** **BEHAVIOR State Space and Action Space**





Our environment defines 15 object states, derived from a subset of iGibson’s state definitions, and offers 30
actions that agents can use to interact with the objects in a task scene. The names of these actions and object
states are provided in Table 28. Object states are categorized into unary states, describing conditions or attributes
of an object, and binary states, representing physical relationships between two objects. Actions are divided
into state actions, which modify the unary states of objects, and spatial actions, which modify the binary states
between two objects or between an object and the agent.


**N.2** **VirtualHome Implementation Details**


**N.2.1** **VirtualHome State Space and Action Space**


We have developed an evaluation environment for LLMs using the VirtualHome simulator as our foundation. At
the core of this environment is a MotionPlanner, which leverages the EnvironmentGraph from VirtualHome


93


**Action Sequencing**


**A1** LEFT_GRASP (object3)
**A2** LEFT_PLACE_INSIDE (object2)

















Figure 63: Implementation of Kinetics Tree for the Transition Model in BEHAVIOR .


Table 28: BEHAVIOR State Space and Action Space.


**State Space** **Action Space**
**Unary States** **Binary States** **State Actions** **Spatial Actions**
_Cooked_ _Inside_ TOGGLE_ON LEFT_GRASP
_Dusty_ _OnFloor_ TOGGLE_OFF RIGHT_GRASP
_Frozen_ _OnTop_ CLOSE LEFT_RELEASE
_Open_ _Under_ OPEN RIGHT_RELEASE
_Sliced_ _NextTo_ CLEAN LEFT_PLACE_ONTOP
_Soaked_ DRY RIGHT_PLACE_ONTOP
_Stained_ SLICE LEFT_PLACE_NEXTTO
_ToggledOn_ SOAK RIGHT_PLACE_NEXTTO
_Slicer_ FREEZE LEFT_PLACE_INSIDE
_CleaningTool_ UNFREEZE RIGHT_PLACE_INSIDE
COOK LEFT_PLACE_UNDER
RIGHT_PLACE_UNDER
LEFT_TRANSFER_CONTENTS_INSIDE
RIGHT_TRANSFER_CONTENTS_INSIDE
LEFT_TRANSFER_CONTENTS_ONTOP
RIGHT_TRANSFER_CONTENTS_ONTOP
LEFT_PLACE_NEXTTO_ONTOP
RIGHT_PLACE_NEXTTO_ONTOP
NAVIGATE_TO


to record the state of the environment. Additionally, the MotionPlanner incorporates runtime error analysis
for all 42 possible actions. To ensure accurate execution outcomes, we made necessary modifications to the
VirtualHome simulator, aligning its action executors with expected results. The current state space and action
space utilized in this environment are detailed in Table 29.


**O** **Evaluation Settings of LLMs**


**O.1** **Decoding Parameters**


To ensure standardization and consistency across all models, we utilized the same set of decoding parameters
for all the LLMs evaluated in this study. Specifically, we used a temperature of zero for all models, as our goal
was to use the arg max under the model’s distribution. Furthermore, since several of the models only support
temperature-based sampling and not other sampling methods, we limited ourselves to temperature scaling during
the sampling process. It is important to note that for a given prompt, the model’s completion involves sampling,
which introduces randomness in determining the specific completion decoded for each instance. However, in our
scenarios, this is not a significant factor since we are decoding the arg max through low-temperature decoding.


**O.2** **Evaluation Cost**


To perform a single run of all models on our benchmark, a total of 180 runs would be required. This involves
evaluating each specific model on each specific simulator ability module. The total cost of this process amounts


94


Table 29: VirtualHome State Space and Action Space.


**State Space** **Action Space**
**Unary States** **Binary States** **State Actions** **Spatial Actions**
_Closed_ _On_ OPEN TURN_TO
_Open_ _Inside_ SIT PUT_BACK
_On_ _Between_ STANDUP PUT_IN
_Off_ _Close_ SLEEP POUR
_Sitting_ _Facing_ WAKEUP PUT_ON
_Dirty_ _Holds_RH_ CLOSE FIND
_Clean_ _Holds_LH_ DRINK RUN
_Lying_ GRAB WALK
_Plugged_in_ LOOKAT POINT_AT
_Plugged_out_ LOOKAT_SHORT TOUCH
LOOKAT_LONG WATCH
SWITCH_OFF MOVE
SWITCH_ON RELEASE
TYPE DROP
PUSH
PULL
SQUEEZE
WASH
RINSE
SCRUB
EAT
PLUG_IN
PLUG_OUT
CUT
READ
LIE


to approximately 192,280,000 tokens and 84,960 queries across all models, which results in an API cost of
$1540.70. For open-source models, costs are based on the pricing of the Together AI API [¶] .


**O.3** **Model Cards**


Due to space limitations, we use shorthand model names in the main paper. Here we provide the details of the
models in Table 30.


Table 30: Model Cards for All Evaluated Large Language Models


**Model Name** **Creator** **Complete Model ID** **Release** **Hosting**

Claude-3 Haiku Anthropic claude-3-haiku-20240307 03/07/24 Anthropic

Claude-3 Sonnet Anthropic claude-3-sonnet-20240229 02/29/24 Anthropic

Claude-3 Opus Anthropic claude-3-opus-20240229 02/29/24 Anthropic

Claude-3.5 Sonnet Anthropic claude-3-5-sonnet-20240620 06/20/24 Anthropic

Cohere Command R Cohere command-r 03/11/24 Cohere

Cohere Command R+ Cohere command-r-plus 04/04/24 Cohere

Gemini 1.0 Pro Google gemini-pro 12/13/23 GCP Vertex

Gemini 1.5 Flash Google gemini-1.5-flash-preview-0514 05/14/24 GCP Vertex

Gemini 1.5 Pro Google gemini-1.5-pro-preview-0409 04/09/24 GCP Vertex

GPT-3.5-turbo OpenAI gpt-3.5-turbo-0125 01/25/24 OpenAI

GPT-4-turbo OpenAI gpt-4-turbo-2024-04-09 04/09/24 OpenAI

Continued on next page


¶https://www.together.ai/


95


Table 30: Model Cards for Various Models


**Model Name** **Release Org.** **Org.** **Model ID** **Release** **Host Org.**

GPT-4o OpenAI gpt-4o-2024-05-13 05/13/24 OpenAI

Llama 3 8B Instruct Meta meta-llama-3-8b-instruct 04/18/24 TogetherAI

Llama 3 70B Instruct Meta meta-llama-3-70b-instruct 04/18/24 TogetherAI

Mistral Large MistralAI mistral-large-2402 02/26/24 MistralAI

Mixtral 8x22B MoE MistralAI mixtral-8x22b-instruct-v0.1 04/17/24 TogetherAI

o1-mini OpenAI o1-mini-2024-09-12 09/12/24 OpenAI

o1-preview OpenAI o1-preview-2024-09-12 09/12/24 OpenAI


**P** **Extensive Related Work**


**P.1** **LLMs for Embodied Planning**


Existing works in embodied task and motion planning (TAMP) have used LLMs to perform varying tasks,
serving different ability modules defined within our embodied agent interface. Due to the page limit, we only
list a subset of such works and their categorization in the main paper. Here in Table 31 we provide an extended
list of such works with detailed categorization for your reference.


Table 31: Categorization of Existing Embodied Agent Planning Works’ Usage of Large Language Models: Each
“LLMs” refers to the usage of LLMs to perform an ability module. For example, Ada [8] uses LLMs for Action
Sequencing and Transition Modeling, while LLM+P [7] uses LLMs for Goal Interpretation.


SayCan [2] LLMs LLMs

Ada [8] LLMs LLMs

LLP+P [7] LLMs

AutoTAMP [18] LLMs LLMs

Code as Policies [6] LLMs LLMs LLMs

Voyager [35] LLMs LLMs

Demo2Code [13] LLMs LLMs LLMs

LM as ZeroShot Planner [3] LLMs LLMs

SayPlan [15] LLMs LLMs LLMs

Text2Motion [48] LLMs

LLMGROP [46] LLMs LLMs

REFLECT [66] LLMs LLMs

Generating Consistent PDDL [32] LLMs LLMs
Domains with LLMs


PlanSeqLearn [64] LLMs

COWP [70] LLMs LLMs LLMs

HumanAssisted Continual [65] LLMs
Robot Learning


DECKARD [34] LLMs LLMs

MOSAIC [63] LLMs

Interactive Task Planning [14] LLMs LLMs
with Language Models


RoCo [62] LLMs

Cook2LTL [42] LLMs

InnerMonologue [21] LLMs


Continued on next page


96


**Goal** **Action** **Subgoal** **Transition**
**Existing Works** **Ref.**
**Interpretation** **Sequencing** **Decomposition** **Modeling**

MLDT [20] LLMs

Learning to Reason over [61] LLMs LLMs LLMs
Scene Graphs


GRID [19] LLMs LLMs

LLMplanner [33] LLMs LLMs

DELTA [17] LLMs

Look Before You Leap [60] LLMs LLMs

CAPE [36] LLMs LLMs

HERACLEs [43] LLMs

RoboTool [16] LLMs LLMs

PROMST [59] LLMs

LLM3 [31] LLMs LLMs

Ghost in the Minecraft [27] LLMs

PlanBench [98] LLMs LLMs

TaPA [30] LLMs LLMs LLMs

ChatGPT Robot Control [29] LLMs

LLM World Models for Plan- [28] LLMs LLMs
ning


DEPS [81] LLMs LLMs

Grounded Decoding [25] LLMs

ProgPrompt [37] LLMs LLMs

DROC [24] LLMs LLMs

LMPC [58] LLMs LLMs

GPTPDDL [47] LLMs

RAP [57] LLMs

LEAGUE++ [38] LLMs LLMs

CoPAL [23] LLMs LLMs

SayCanPay [22] LLMs LLMs

LLMGenPlan [56] LLMs


**P.2** **LTL Agent Interface**


Initially proposed as a formal verification for computer programs [44], Linear Temporal Logic (LTL) expressions
have since then been extensively used in robotics [39–41] as a formalism that enables the extraction of guarantees
on robot performance given a robot model, a high-level description of robotic actions, and a class of admissible
environments. Recent work in embodied agent planning has adopted LTL expressions for their agent interface
due to their high expressiveness and formality, bridging the gap between natural language and robotic control
language [42, 43, 18]. Among these works [42] and [43] directly translate natural language actions and task goals
into LTL to perform planning tasks, while [18] utilizes an intermediary representation called Signal Temporal
Logic (STL) derived from LTL for action and subgoal planning. Due to the compactness and expressiveness of
LTL, our work leverages LTL as a unified interface between modules and agents to describe task specifications.

**P.3** **Embodied Agent Benchmarks**


Recent years have seen a proliferation of benchmarks for evaluating embodied simulation and decision-making,
each focusing on different aspects of the problem. PlanBench [99] provides an extensible framework for
evaluating LLMs on planning and reasoning about change but is limited to generating action sequences without
considering other crucial aspects of embodied decision-making. Similarly, LoTa-Bench [9] benchmarks languageoriented task planners for embodied agents but focuses primarily on action sequencing and lacks support for
fine-grained analysis of planning errors.


Several benchmarks are built on specific simulators. ALFRED [71] offers a benchmark for interpreting grounded
instructions for everyday tasks, while VirtualHome [5] simulates household activities via programs. However,
both lack a standardized interface for evaluating different LLM-based modules and don’t provide detailed metrics
for analyzing various types of errors in embodied decision-making tasks.


97


The HAZARD Challenge [100] evaluates embodied decision-making in dynamically changing environments,
but primarily emphasizes social metrics such as the value of rescued objects and damage, rather than providing a comprehensive evaluation of language-based reasoning and planning. ACT-Thor [101] provides a
controlled benchmark for embodied action understanding in simulated environments but focuses mainly on
action recognition and prediction rather than the full spectrum of embodied decision-making tasks.


In contrast to these existing benchmarks, our Embodied Agent Interface provides a more comprehensive
evaluation framework. It covers multiple decision-making modules (goal interpretation, subgoal decomposition,
action sequencing, and transition modeling), offers standardized interfaces for modular evaluation of LLM
capabilities, and includes fine-grained metrics for various types of errors. Moreover, by using LTL formulas
for goal specification, our framework enables the unified representation and evaluation of complex, temporally
extended goals, addressing a key limitation in many existing benchmarks.


**Q** **Maintenance Plan**


**Q.1** **Dataset URLs, License, and Hosting Plan**


The dataset is uploaded for public download under the CC-BY-4.0 license: [https://huggingface.co/](https://huggingface.co/datasets/Inevitablevalor/EmbodiedAgentInterface)
[datasets/Inevitablevalor/EmbodiedAgentInterface.](https://huggingface.co/datasets/Inevitablevalor/EmbodiedAgentInterface)


Our dataset is also hosted on [https://github.com/embodied-agent-interface/](https://github.com/embodied-agent-interface/embodied-agent-interface/)
[embodied-agent-interface/](https://github.com/embodied-agent-interface/embodied-agent-interface/) which will provide long-term support for hosting the dataset.
Contact information for the authors is available on our website. We can also be reached through GitHub issues
or via email for any inquiries or support related to the dataset and evaluation tool. We will maintain an erratum
on the GitHub issues to host any approved corrections or updates suggested by the authors or the broader video
research community.


**Q.2** **Long-term Preservation and DOI**


We provide a persistent dereferenceable identifier DOI: [https://datadryad.org/stash/share/](https://doi.org/10.5061/dryad.f4qrfj741)
[bLEj8IHqyKc9c_Ff0M8tTSZMTxOLxIup5zUWn9yq_j8.](https://doi.org/10.5061/dryad.f4qrfj741)


**Q.3** **URL of Croissant Metadata Record**


We provide structured metadata (schema.org standards) in [https://huggingface.co/datasets/](https://huggingface.co/datasets/Inevitablevalor/EmbodiedAgentInterface)
[Inevitablevalor/EmbodiedAgentInterface.](https://huggingface.co/datasets/Inevitablevalor/EmbodiedAgentInterface)


**Q.4** **Author Statement**


The authors bear all responsibility in case of violation of rights. All dataset annotations were collected by the
authors and we are releasing the dataset under CC-BY-4.0.


**Q.5** **URLs of Code and Re-productivity**


We have made the codebase publicly available on GitHub [(https://github.com/](https://github.com/embodied-agent-interface/embodied-agent-interface/)
[embodied-agent-interface/embodied-agent-interface/),](https://github.com/embodied-agent-interface/embodied-agent-interface/) along with detailed instructions. In this section, we provide the URLs for the VirtualHome evaluator, BEHAVIOR evaluator, and LLM
implementations. Further details regarding these implementations can be found in Appendix N.


**Q.6** **Code for VirtualHome Evaluator and Computation Resources**


We provide the code for the VirtualHome evaluator at [https://github.com/](https://github.com/embodied-agent-interface/embodied-agent-interface/)
[embodied-agent-interface/embodied-agent-interface/,](https://github.com/embodied-agent-interface/embodied-agent-interface/) along with detailed usage
instructions. This code facilitates goal interpretation, subgoal decomposition, action sequencing, and transition
modeling.


[To use our evaluator, ensure that Docker is installed in your environment.](https://www.docker.com/) For annotating the RobotHow dataset,
we also offer code for human interactive annotation of goals within VirtualHome, with usage instructions
available in the repository.


The experiment of subgoal decomposition is run on a Windows 11 system with an AMD Ryzen 7 5800H
processor and 16GB of memory. For goal interpretation, action sequencing, and transition modeling, the
experiments are run on a Linux system with 8 Intel(R) Xeon(R) Platinum 8481C CPU @ 2.70GHz and 32GB of
memory.


98


**Q.7** **Code for BEHAVIOR Evaluator and Computation Resources**


The code for the BEHAVIOR evaluator is available on GitHub at [https://github.com/](https://github.com/embodied-agent-interface/embodied-agent-interface/)
[embodied-agent-interface/embodied-agent-interface/.](https://github.com/embodied-agent-interface/embodied-agent-interface/) We provide easy-to-reproduce
scripts for prompt generation and evaluation of each module: _goal_ _interpretation_, _action_ _sequencing_, _sub-_
_goal_ _decomposition_, and _transition_ _modeling_ . The BEHAVIOR related experiments were conducted on a
Windows 11 system equipped with an AMD Ryzen 7 7800X3D processor and 32GB of RAM.


**Q.8** **Code for LLMs Implementations and Computation Resources**


We provide convenient and easy-to-reproduce batch LLM inference by directly integrating our evaluation
pipeline into the HELM [83] code base. In order to allow quick and easy access for all users with different
development environments and GPU setups, we provide the option to use Together AI [||] APIs for large opensource model inference. Users can easily set up their inference environment by following the instructions in
[https://github.com/embodied-agent-interface/embodied-agent-interface.](https://github.com/embodied-agent-interface/embodied-agent-interface)


To achieve standardization and consistency across all models, we used the same set of decoding parameters
for all LLMs evaluated. Specifically, we used temperature zero for all models because we wanted to use the
arg max under the model’s distribution. Also, because several of the models only support temperature-based
sampling, as opposed to other sampling methods, we restricted ourselves to temperature scaling during the
sampling process. For a given prompt, since the model’s completion involves sampling, there is randomness
involved in determining the specific completion decoded for each instance. For our scenarios, since we are
decoding the arg max through low-temperature decoding, this is not a significant factor.


To perform a single run of all models on our benchmark, a total of 180 runs would be required. This involves
evaluating each specific model on each specific simulator ability module. The total cost of this process amounts
to approximately 126,900,000 tokens and 50,760 queries across all models, which sums to a compute cost of
$879.24. For open-source models, the costs are calculated based on the pricing of the Together.ai API.


Due to space limitations, we use shortened model names for common commercial and open-source LLMs
evaluated in the main paper. However, shortened model names do not specify the precise release version and
hosting organization of the models being addressed. To support maximum reproducibility, we list detailed
information for each model in Table 30.


**R** **Datasheets for EMBODIED AGENT INTERFACE (EAI)**


**Motivation**


**For what purpose was the dataset created?** Was there a specific task in mind? Was there a specific gap that
needed to be filled? Please provide a description.


Please refer to Appendix B.1.


**Who created this dataset (e.g., which team, research group) and on behalf of which entity (e.g., company,**
**institution, organization)?**


The authors created the dataset within the Stanford Vision and Learning Lab at Stanford University. It is created
for public use and is not affiliated with or tied to any specific organization or institution.


**Who funded the creation of the dataset?** If there is an associated grant, please provide the name of the grantor
and the grant name and number.


The benchmark creation is funded by the 1287101-1-UBKNA.


**Any other comments?**


No.


**Composition**


**What do the instances that comprise the dataset represent (e.g., documents, photos, people, countries)?**
Are there multiple types of instances (e.g., movies, users, and ratings; people and interactions between them;
nodes and edges)? Please provide a description.


Each instance in the dataset represents a task with its corresponding ground truth plan. Specifically, each task
contains the following data: (1) natural language task name, (2) natural language task instruction, (3) symbolic
goal definition, (4) symbolic action trajectory, and (5) the transition models involved in the task. For tasks in the


||https://www.together.ai/


99


BEHAVIOR environment, the dataset also includes accompanying demo videos that showcase the execution of
the ground truth action trajectories. Further details regarding the dataset format can be found in Appendix L.1.


**How many instances are there in total (of each type, if appropriate)?**


We have released a dataset containing task plans and trajectories for two environments: VirtualHome and
BEHAVIOR . For VirtualHome, there are 338 task plans/trajectories with a total of 2,960 steps and 801 goal
conditions. For BEHAVIOR, there are 100 task plans/trajectories with a total of 1,460 steps and 673 goal
conditions. Additionally, we have annotated transition models for both environments. VirtualHome has 33
annotated transition models with 99 preconditions and 57 effects, while BEHAVIOR has 30 annotated transition
models with 84 preconditions and 51 effects.


More detailed information can be found in Table 2 of the main paper. Further statistics on the entire dataset
[are available on the website https://embodied-agent-interface.github.io/.](https://embodied-agent-interface.github.io/) We will continue
releasing more data and updating the information on the website in the future.


**Does the dataset contain all possible instances or is it a sample (not necessarily random) of instances from**
**a larger set?** If the dataset is a sample, then what is the larger set? Is the sample representative of the larger set
(e.g., geographic coverage)? If so, please describe how this representativeness was validated/verified. If it is not
representative of the larger set, please describe why not (e.g., to cover a more diverse range of instances, because
instances were withheld or unavailable).


The benchmark is built on top of two simulators VirtualHome [5] and BEHAVIOR -100 [4]. To better evaluate
the decision-making ability, we selectively focus on long-horizon tasks with complicated goals in VirtualHome
and BEHAVIOR-100. We select these two simulators due to their length of task plans and the number of goal
conditions, as detailed in Appendix M.1. Detailed statistics of the selected subset are discussed in Appendix L.2,
where we show that the selected subset is very diverse in tasks and environments.


**What data does each instance consist of?** Raw data (e.g., unprocessed text or images) or features? In either
case, please provide a description.


The benchmark contains task annotations along with an evaluator codebase for executing plans in the simulators
[https://github.com/embodied-agent-interface/embodied-agent-interface/.](https://github.com/embodied-agent-interface/embodied-agent-interface/) This
codebase is created by modifying the existing simulator backbones VirtualHome and BEHAVIOR. For example,
in the BEHAVIOR environment, we implemented a symbolic simulator. Detailed information about the evaluator
implementations and the revisions made to the simulators to support the evaluation can be found in Appendix N.1
for the BEHAVIOR environment and Appendix N.2 for the VirtualHome environment. Using the provided
simulator, users can visualize the plan execution process and capture screenshots. For tasks in the BEHAVIOR
environment, the dataset also includes demo videos that demonstrate the execution of the ground truth action
trajectories, complementing the annotated ground truth goals and plans.


**Is there a label or target associated with each instance?** If so, please provide a description.


Each instance is associated with a sequence of labels showing the ground truth action sequence.


**Is any information missing from individual instances?** If so, please provide a description, explaining why
this information is missing (e.g. because it was unavailable). This does not include intentionally removed
information but might include, e.g., redacted text.


All instances are complete.


**Are relationships between individual instances made explicit (e.g., users’ movie ratings, social network**
**links)?** If so, please describe how these relationships are made explicit.


Some instances may share the same task name or same goals, but different trajectories. It can be viewed as
alternative decision-making trajectories to achieve the goal. It will be indicated by a unique identifier indicating
the scene and the task.


**Are there recommended data splits (e.g., training, development/validation, testing)?** If so, please provide a
description of these splits, explaining the rationale behind them.


The benchmark presented in this work is specifically designed for zero-shot testing, with the primary goal of
assessing the out-of-the-box long-horizon embodied decision-making capabilities of LLMs.


**Are there any errors, sources of noise, or redundancies in the dataset?** If so, please provide a description.


The dataset was very carefully manually curated to mitigate any incidence of errors within the goal annotations,
trajectory annotations, and transition model annotations. For VirtualHome, we cross-examine the goal annotations by executing alternative action trajectories and summarizing the overlapping final states. After that, we
manually cleaned and removed any actions that were deemed unrelated. The annotation process, which utilized


100


a human-interactive annotation program, is discussed in further detail in Appendix M.3. For the BEHAVIOR
environment, we cross-examine the action sequence annotations by grounding them to multiple demonstration
videos. This process is described in more detail in Appendix M.2.


**Is the dataset self-contained, or does it link to or otherwise rely on external resources (e.g., websites, tweets,**
**other datasets)?** If it links to or relies on external resources, a) are there guarantees that they will exist, and
remain constant, over time; b) are there official archival versions of the complete dataset (i.e., including the
external resources as they existed at the time the dataset was created); c) are there any restrictions (e.g., licenses,
fees) associated with any of the external resources that might apply to a future user? Please provide descriptions
of all external resources and any restrictions associated with them, as well as links or other access points, as
appropriate.


The entire dataset will be made publicly available on our project website [https://](https://embodied-agent-interface.github.io/)
[embodied-agent-interface.github.io/.](https://embodied-agent-interface.github.io/) We will also provide a dockerized simulator evaluator tool for executing plans within the simulator environment. The text data will be released in JSON format
and hosted on our website. Detailed information about the dataset format can be found in Appendix L.1. The
benchmark will be released under the VirtualHome and BEHAVIOR licenses, which allow public use of the
simulators for both research and commercial purposes.


**Does the dataset contain data that might be considered confidential (e.g., data that is protected by legal**
**privilege or by doctor-patient confidentiality, data that includes the content of individuals’ non-public**
**communications)?** If so, please provide a description.


No.


**Does the dataset contain data that, if viewed directly, might be offensive, insulting, threatening, or might**
**otherwise cause anxiety?** If so, please describe why.


No.


**Does the dataset relate to people?** If not, you may skip the remaining questions in this section.


No.


**Does** **the** **dataset** **identify** **any** **subpopulations** **(e.g.,** **by** **age,** **gender)?** If so, please describe how these
subpopulations are identified and provide a description of their respective distributions within the dataset.


No.


**Is it possible to identify individuals (i.e., one or more natural persons), either directly or indirectly (i.e., in**
**combination with other data) from the dataset?** If so, please describe how.


No.


**Does the dataset contain data that might be considered sensitive in any way (e.g., data that reveals racial or**
**ethnic origins, sexual orientations, religious beliefs, political opinions or union memberships, or locations;**
**financial or health data;** **biometric or genetic data;** **forms of government identification, such as social**
**security numbers; criminal history)?** If so, please provide a description.


No.


**Any other comments?**


No.


**Collection Process**


**How was the data associated with each instance acquired?** Was the data directly observable (e.g., raw text,
movie ratings), reported by subjects (e.g., survey responses), or indirectly inferred/derived from other data (e.g.,
part-of-speech tags, model-based guesses for age or language)? If data was reported by subjects or indirectly
inferred/derived from other data, was the data validated/verified? If so, please describe how.


We annotate the data based on existing annotations from the VirtualHome and BEHAVIOR environments. To
ensure the accuracy of the annotations, we execute the plans in the simulator and verify that the goals are satisfied.


**What mechanisms or procedures were used to collect the data (e.g., hardware apparatus or sensor, manual**
**human curation, software program, software API)?** How were these mechanisms or procedures validated?


101


As mentioned above, the data is collected based on existing annotations from VirtualHome and BEHAVIOR .
We supplement these annotations with additional manual annotations about goals, trajectories, transition models,
and natural language task instructions, which are described in detail in Appendix M.


**If the dataset is a sample from a larger set, what was the sampling strategy (e.g., deterministic, probabilistic**
**with specific sampling probabilities)?**


To better evaluate the decision-making ability, we selectively focus on long-horizon tasks with complicated goals
in VirtualHome and BEHAVIOR-100. We select these two simulators due to their length of task plans and the
number of goal conditions, as detailed in Appendix M.1. Detailed statistics of the selected subset is discussed in
Appendix L.2, where we show that the selected subset is very diverse in tasks and environments.


**Who was involved in the data collection process (e.g., students, crowd workers, contractors) and how were**
**they compensated (e.g., how much were crowd workers paid)?**


The annotations are purely done by the authors in this paper, which are expert researchers.


**Over what timeframe was the data collected?** **Does this timeframe match the creation timeframe of the**
**data associated with the instances (e.g., the recent crawl of old news articles)?** If not, please describe the
timeframe in which the data associated with the instances was created.


The newly added annotations are done in 2024. VirtualHome was created in 2018, and BEHAVIOR was created
in 2022.


**Were any ethical review processes conducted (e.g., by an institutional review board)?** If so, please provide
a description of these review processes, including the outcomes, as well as a link or other access point to any
supporting documentation.


No.


**Does the dataset relate to people?** If not, you may skip the remaining questions in this section.


No.


**Did you collect the data from the individuals in question directly, or obtain it via third parties or other**
**sources (e.g., websites)?**


N/A.


**Were the individuals in question notified about the data collection?** If so, please describe (or show with
screenshots or other information) how notice was provided, and provide a link or other access point to, or
otherwise reproduce, the exact language of the notification itself.


N/A.


**Did the individuals in question consent to the collection and use of their data?** If so, please describe (or
show with screenshots or other information) how consent was requested and provided, and provide a link or
other access point to, or otherwise reproduce, the exact language to which the individuals consented.


N/A.


**If** **consent** **was** **obtained,** **were** **the** **consenting** **individuals** **provided** **with** **a** **mechanism** **to** **revoke** **their**
**consent in the future or for certain uses?** If so, please provide a description, as well as a link or other access
point to the mechanism (if appropriate).


N/A.


**Has an analysis of the potential impact of the dataset and its use on data subjects (e.g., a data protection**
**impact analysis) been conducted?** If so, please provide a description of this analysis, including the outcomes,
as well as a link or other access point to any supporting documentation.


N/A.


**Any other comments?**


No.


**Preprocessing/cleaning/labeling**


**Was any preprocessing/cleaning/labeling of the data done (e.g., discretization or bucketing, tokenization,**
**part-of-speech tagging, SIFT feature extraction, removal of instances, processing of missing values)?** If so,
please provide a description. If not, you may skip the remainder of the questions in this section.


102


The annotations were curated by humans. We execute the trajectories in the simulators to validate the goals,
trajectories, and transition models.


**Was the raw data saved in addition to the preprocessed/cleaned/labeled data (e.g., to support unanticipated**
**future uses)?** If so, please provide a link or other access point to the “raw” data.


Human annotations of missing goals, trajectories, and transition models are created from scratch, with the
process detailed in Appendix M. The raw demo videos used for annotation reference are publicly available from
BEHAVIOR [4].


**Is the software used to preprocess/clean/label the instances available?** If so, please provide a link or other
access point.


The code used to facilitate the dataset annotation process is available at [https://github.com/](https://github.com/embodied-agent-interface/embodied-agent-interface/)
[embodied-agent-interface/embodied-agent-interface/and](https://github.com/embodied-agent-interface/embodied-agent-interface/) is described in detail in Appendix M.


**Any other comments?**


No.


**Uses**


**Has the dataset been used for any tasks already?** If so, please provide a description.


The dataset presented in this work has been used as a benchmark for evaluating the performance of 15 large
language models (LLMs). More details about these LLMs can be found in Appendix O. Furthermore, the code
for conducting zero-shot evaluation using our dataset will be made available on our project’s GitHub repository.


**Is there a repository that links to any or all papers or systems that use the dataset?** If so, please provide a
link or other access point.


It will be made public on our website once more papers start to use our dataset.


**What (other) tasks could the dataset be used for?** Is there anything about the composition of the dataset or
the way it was collected and preprocessed/cleaned/labeled that might impact future uses? For example, is there
anything that a future user might need to know to avoid uses that could result in unfair treatment of individuals or
groups (e.g., stereotyping, quality of service issues) or other undesirable harms (e.g., financial harms, legal risks)
If so, please provide a description. Is there anything a future user could do to mitigate these undesirable harms?


There may be potential to benchmark vision-language foundation models (VLMs) using our benchmark.
However, we leave the exploration of these possibilities for future work.


**Is** **there** **anything** **about** **the** **composition** **of** **the** **dataset** **or** **the** **way** **it** **was** **collected** **and** **preprocessed/-**
**cleaned/labeled that might impact future uses?** For example, is there anything that a future user might need
to know to avoid uses that could result in unfair treatment of individuals or groups (e.g., stereotyping, quality of
service issues) or other undesirable harms (e.g., financial harms, legal risks) If so, please provide a description.
Is there anything a future user could do to mitigate these undesirable harms?


No.


**Are there tasks for which the dataset should not be used?** If so, please provide a description.


No.


**Any other comments?**


No.


**Distribution**


**Will the dataset be distributed to third parties outside of the entity (e.g., company, institution, organization)**
**on behalf of which the dataset was created?** If so, please provide a description.


The dataset will be made publicly available and can be used for both research and commercial purposes under
the VirtualHome and BEHAVIOR licenses.


**How will the dataset be distributed (e.g., tarball on the website, API, GitHub)** Does the dataset have a digital
object identifier (DOI)?


103


The dataset will be distributed as a JSON file that includes a unique identifier for each task goal, along with
its associated action trajectory, natural language task name, natural language task instructions, symbolic goal
definition, and the transition models involved in the task. Additionally, we will release the evaluator, which
enables the automatic calculation of fine-grained systematic metrics. Further details regarding the dataset format
can be found in Appendix L.1.


**When will the dataset be distributed?**


The full dataset will be made available upon the acceptance of the paper before the camera-ready deadline. We
release it on our website.


**Will the dataset be distributed under a copyright or other intellectual property (IP) license, and/or under**
**applicable terms of use (ToU)?** If so, please describe this license and/or ToU, and provide a link or other access
point to, or otherwise reproduce, any relevant licensing terms or ToU, as well as any fees associated with these
restrictions.


The dataset will be publicly released under the VirtualHome and BEHAVIOR licenses, which allow direct public
use for both research and commercial purposes.


**Have any third parties imposed IP-based or other restrictions on the data associated with the instances?**
If so, please describe these restrictions, and provide a link or other access point to, or otherwise reproduce, any
relevant licensing terms, as well as any fees associated with these restrictions.


No.


**Do any export controls or other regulatory restrictions apply to the dataset or to individual instances?** If
so, please describe these restrictions, and provide a link or other access point to, or otherwise reproduce, any
supporting documentation.


No.


**Any other comments?**


No.


**Maintenance**


**Who will be supporting/hosting/maintaining the dataset?**


The authors of the paper will maintain the dataset, and pointers to the dataset will be hosted on the GitHub repository [https://github.com/embodied-agent-interface/embodied-agent-interface/.](https://github.com/embodied-agent-interface/embodied-agent-interface/)
The repository will also include the code for downloading the dataset and the evaluation tool.


**How can the owner/curator/manager of the dataset be contacted (e.g., email address)?**


Contact information for the authors is available on our website. We can also be reached through GitHub issues
or via email for any inquiries or support related to the dataset and evaluation tool.


**Is there an erratum?** If so, please provide a link or other access point.


We will maintain an erratum on the GitHub repository to host any approved corrections or updates suggested by
the authors or the broader video research community.


**Will** **the** **dataset** **be** **updated** **(e.g.,** **to** **correct** **labeling** **errors,** **add** **new** **instances,** **delete** **instances)?** If
so, please describe how often, by whom, and how updates will be communicated to users (e.g., mailing list,
GitHub)?


Yes, we plan to host an erratum publicly on our GitHub. We also plan to expand the scale of the annotations, and
any updates or extensions will be announced on our website and GitHub repository.


**If the dataset relates to people, are there applicable limits on the retention of the data associated with the**
**instances (e.g., were individuals in question told that their data would be retained for a fixed period of**
**time and then deleted)?** If so, please describe these limits and explain how they will be enforced.


No.


**Will older versions of the dataset continue to be supported/hosted/maintained?** If so, please describe how.
If not, please describe how its obsolescence will be communicated to users.


N/A. There are no older versions at the current moment. All updates regarding the current version will be
communicated via our website and Github.


104


**If others want to extend/augment/build on/contribute to the dataset, is there a mechanism for them to do**
**so?** If so, please provide a description. Will these contributions be validated/verified? If so, please describe
how. If not, why not? Is there a process for communicating/distributing these contributions to other users? If so,
please provide a description.


Contributions to the dataset and evaluation tool will be made possible using standard open-source practices.
Interested parties can submit pull requests to the relevant GitHub repository, which will be reviewed and
incorporated by the authors.


**Any other comments?**


No.


**S** **Impact, Limitations and Future Directions**


**S.1** **Broader Impact**


The proposed EMBODIED AGENT INTERFACE and the comprehensive evaluation of Large Language Models
(LLMs) for embodied decision-making have significant implications for the development and deployment of
intelligent agents in various domains, including robotics, autonomous systems, and human-robot interaction.


Recent advancements in LLMs have revolutionized the field of artificial intelligence, particularly in the domains
of digital and embodied agents. LLMs have demonstrated remarkable capabilities in natural language understanding, generation, and reasoning, enabling the development of sophisticated AI systems that can interact with
humans and the environment in more natural and intuitive ways. The potential of LLMs extends beyond purely
digital interactions, as embodied agents that can perceive, reason, and act within physical environments have
also begun to benefit from the integration of LLMs. By combining the language understanding and generation
capabilities of LLMs with the perceptual and motor skills of embodied agents, researchers aim to create more
versatile and intelligent robots that can assist humans in various tasks.


However, despite the progress made in applying LLMs to embodied agents, significant challenges remain.
Embodied agents require the ability to ground natural language instructions in the physical world, reason
about object properties and relationships, and plan and execute actions in dynamic environments. While LLMs
have shown promise in these areas, they exhibit inconsistencies in their interactions with the physical world,
occasionally making correct decisions but frequently producing incorrect plans without reliable ways to control
their performance. This inconsistency leads to doubts regarding their suitability and readiness for robotic tasks.
Current evaluations of LLMs in embodied decision-making tasks often entangle various capabilities, making it
difficult to understand why LLMs sometimes work and sometimes fail. Existing benchmarks oversimplify the
problem by focusing only on high-level semantic failures, assuming that low-level physics can be automatically
fulfilled. This assumption results in unrealistic plans, such as a robot heating food in a microwave without first
ensuring the door is closed or the food is properly placed inside, which ignores crucial physical preconditions
and state changes.


The standardization of goal specifications, modules, and interfaces through the EMBODIED AGENT INTERFACE
framework enables a more systematic and rigorous evaluation of LLMs’ capabilities in embodied decisionmaking tasks. This standardization facilitates the comparison of different approaches and architectures, promoting
the development of more advanced and reliable LLM-based embodied agents. By identifying the strengths and
weaknesses of current LLMs through fine-grained metrics and error analysis, researchers and practitioners can
focus their efforts on addressing specific limitations and improving the overall performance of these agents.


Moreover, the insights gained from the evaluation of LLMs in embodied decision-making tasks can inform the
design of future LLMs and training strategies. The identified challenges, such as the difficulty in grounding
natural language instructions to environment-specific objects and states, the lack of reasoning abilities in handling
preconditions and post-effects of actions, and the reporting bias in goal interpretation, highlight the need for
more targeted training approaches. These findings can guide the development of LLMs that are better suited for
embodied decision-making, potentially leading to more effective and efficient agents in real-world applications.
The fine-grained evaluation results and the breakdown of LLM abilities into detailed tests can pinpoint the
strengths and weaknesses in LLM interactions with the physical world, offering a more robotics-centered
evaluation to uncover how much LLMs understand about the physical world and what knowledge can be
improved or added for robotics.


The EMBODIED AGENT INTERFACE framework and the fine-grained evaluation results can also contribute to the
development of safer and more trustworthy embodied agents. By providing a comprehensive understanding of
LLMs’ limitations and failure modes in embodied decision-making, this work can help in designing safeguards
and fallback mechanisms to mitigate potential risks associated with the deployment of these agents in real-world
scenarios. The fine-grained error analysis can inform the development of monitoring and intervention strategies
to ensure the safe and reliable operation of LLM-based embodied agents.


Furthermore, the EMBODIED AGENT INTERFACE framework and the evaluation methodology can be extended
to other domains beyond household tasks, such as industrial automation, healthcare robotics, and autonomous


105


vehicles. The standardization of goal specifications, modules, and interfaces can facilitate the application of
LLMs in these domains, enabling the development of more intelligent and adaptive agents that can understand
and execute complex tasks based on natural language instructions.


However, it is essential to consider the potential negative impacts and ethical implications of deploying LLMbased embodied agents. The evaluation results highlight the need for careful consideration of the limitations
and biases of these agents, particularly in safety-critical applications. It is crucial to establish guidelines and
best practices for the responsible development and deployment of LLM-based embodied agents, ensuring
transparency, accountability, and alignment with human values.


In conclusion, the EMBODIED AGENT INTERFACE framework and the comprehensive evaluation of LLMs for
embodied decision-making have the potential to advance the field of intelligent agents and enable the development
of more capable, reliable, and trustworthy embodied agents. By providing a standardized approach for evaluation
and identifying key challenges and opportunities, this work can guide future research and development efforts in
this area, ultimately leading to the realization of intelligent agents that can effectively understand and execute
complex tasks in real-world environments. As LLMs continue to evolve and be integrated into embodied agents,
it is essential to address the challenges and ethical considerations to ensure their responsible and beneficial
deployment in various domains.


**S.2** **Limitations**


While we currently evaluate the capabilities and limitations of LLMs in embodied decision-making tasks, our
approach has limitations as we abstract input environments using relational graphs of objects, which may not
adequately represent the rich multimodal information, low-level physical properties, and dynamics of real-world
environments. It does not cover the challenge of grounding natural language instructions in perceptual data and
executing actions that require fine-grained control and precise manipulation. Moreover, our current evaluation
primarily focuses on symbolic reasoning and decision-making, without directly incorporating sensory inputs or
actuation outputs. While this approach allows us to isolate and assess specific cognitive capabilities, it neglects
the crucial integration of perception and action, which is fundamental to embodied agents operating in physical
environments. Realistic embodied decision-making requires seamless interplay between language understanding,
sensory processing, and motor control, which our current framework does not fully capture.


**S.3** **Potential Negative Social Impact**


The development of embodied agents powered by large language models has significant potential social impacts
that must be carefully considered. On one hand, these intelligent agents could revolutionize numerous domains
and provide immense societal benefits. In healthcare, they could assist in elderly care, rehabilitation, and
medical procedures. In education, they could serve as personalized companions and learning companions. In
manufacturing and logistics, they could streamline operations, improve efficiency, and enhance workplace safety.


However, the deployment of such capable agents also raises concerns over privacy, security, and ethical
implications. There are risks of these systems being exploited for malicious purposes or exhibiting harmful
biases learned from training data. Additionally, their widespread adoption could disrupt certain job markets and
exacerbate economic inequalities if not managed responsibly. As we strive to unlock the full capabilities of
large language models for embodied decision-making, it is crucial to prioritize the development of robust safety
measures, ethical frameworks, and regulatory guidelines. Engaging diverse stakeholders, including policymakers,
domain experts, and the general public, will be vital in navigating the intricate societal impacts and ensuring
these powerful technologies are harnessed for the greater benefit of humanity.


**S.4** **Potential Future Directions**


We outline the future directions to represent a roadmap for advancing embodied decision making with large
foundation models.


     - **Multimodal Grounding and Integration:** Extend the evaluation to incorporate multimodal inputs
such as vision, audio, by integrating with Vision-Language Models (VLMs) and other multimodal
models. This includes assessing LLMs’ ability to ground language in rich sensory contexts, generate
low-level control commands, and reason about visual information like object keypoints, grasp poses,
and scene configurations. Additionally, explore the use of symbolic scene graph representations as
input to LLMs, enabling more effective reasoning about object relationships, spatial configurations,
and visual attributes.


     - **Vision and Low-Level Control:** Evaluate LLMs in scenarios that require tight integration with vision
systems and low-level control modules, such as tabletop object manipulation tasks. This could involve
predicting object keypoints, generating pick-and-place grasp poses, or generating goal configurations
for object arrangements.


     - **Episodic Memory and Scene Graph Memory:** Investigate the integration of memory systems into
LLMs, including episodic memory for storing and retrieving past experiences, state memory for
maintaining internal environment representations, and scene graph memory structures. Evaluating


106


memory-augmented LLMs can provide insights into leveraging past knowledge and rich environment
representations for improved decision-making.


     - **Physical and Geometric Reasoning:** Incorporate tasks that require physical and geometric reasoning
capabilities, such as spatial planning, object manipulation, collision avoidance, stability prediction,
and reasoning about object affordances, sizes, shapes, and attachments. Evaluate LLMs’ potential for
precise physical interactions and reasoning in real-world applications.


     - **Navigation and Exploration:** Include navigation tasks and scenarios that require LLMs to understand
navigational instructions, plan efficient routes, adapt to dynamic environments, and explore unknown
object states, physical properties, and environmental conditions crucial for robust decision-making.


     - **Forward, Backward, and Counterfactual Prediction:** Assess LLMs’ ability to perform forward
prediction (action _→_ state change), backward prediction (goal state _→_ necessary objects and subgoals),
and counterfactual reasoning (hypothetical "what-if" scenarios) to evaluate their capability in multi-step
planning and reasoning about the effects of actions or action sequences.


     - **Dataset, Simulation, and Benchmark Diversity:** Develop new datasets, simulation environments,
and integrate with existing robotics benchmarks to capture the complexity and diversity of real-world
scenarios. This includes creating environments with richer physics simulations, more diverse object
interactions, and more challenging task setups, as well as integrating with benchmarks focused on task
and motion planning, manipulation, and navigation.


     - **Fine-Tuning** **Decision** **Making** **Models:** Explore fine-tuning LLMs using the fine-grained error
analysis and feedback from the evaluation framework, enabling them to learn from their mistakes and
improve decision-making capabilities. Additionally, develop specialized models such as a decision
making GPT, trained on trajectories of embodied decision-making tasks, to generalize to unseen
trajectories and environments.


By addressing these directions, researchers and practitioners can overcome the current limitations, bridge the
gap between language models and embodied agents, and unlock the full potential of LLMs in real-world,
dynamic environments. Ultimately, these efforts will pave the way for more capable, adaptable, and trustworthy
embodied agents that can seamlessly understand natural language instructions, reason about physical properties
and constraints, and execute complex tasks with precision and robustness.


107


