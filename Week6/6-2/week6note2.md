# Crack Open (Neural) Nets: Can We Make ML-Based Networked Systems More Trustworthy?

## Speech Description

Marco Canini

Machine learning (ML) solutions to challenging networking problems are a promising avenue but the lack of interpretability and the behavioral uncertainty affect trust and hinder adoption. A key advantage of ML algorithms and architectures, such as deep neural networks and reinforcement learning, is that they can discover solutions that are attuned to specific problem instances. As an example, consider a video bit rate adaptation logic that is tuned specifically for Netflix clients in the United States. Yet, there is a general fear that ML systems are black boxes. This creates uncertainty about why learning systems work, whether they will continue to work in conditions that are different from those seen during training or whether they will fall off performance cliffs. The lack of interpretability of ML models is widely recognized as a major hindrance to adoption.

This raises a crucial question: **How do we ensure that learned models behave reliably and as intended?** ML solutions that cannot be trusted to do so are brittle and may not be deployed despite their performance benefits. <u>**We propose an approach to enhance the trustworthiness of ML solutions for networked systems. Our approach builds on innovations in interpretable ML tools.**</u> Given a black-box ML model, <u>interpretable ML methods offer explanations on any given input instance</u>. By integrating the explanations from these tools with operator’s domain knowledge, **our approach can verify that the ML model behaves as per operator expectations, detect misbehaviors and identify corrective actions**. To demonstrate our approach, we performed an in-depth case study on **Pensieve** (a recent neural video rate adaptation system) and identified four classes of undesired behaviors.

Bio: Marco Canini is an assistant professor in Computer Science at KAUST. Marco obtained his Ph.D. in computer science and engineering from the University of Genoa in 2009 after spending the last year as a visiting student at the University of Cambridge, Computer Laboratory. He was a postdoctoral researcher at EPFL from 2009 to 2012 and after that a senior research scientist for one year at Deutsche Telekom Innovation Labs & TU Berlin. Before joining KAUST, he was an assistant professor at the Université catholique de Louvain. He also held positions at Intel, Microsoft, and Google.

## Pre-read of some crucial articles

### **In-Network Computation is a Dumb Idea Whose Time Has Come**

#### 1-2. Introduction & Problem Articulation

**Crux problem**: What should network computer? What kinds of computation should be delegated to the network?

**Solution**: (user-defined) [Aggregation Function](https://en.wikipedia.org/wiki/Aggregate_function) (like MapReduce, ML, graph processing, stream processing) with several properties:

- Reduce the ammount of data (reduce congestion)
- Simple Arithemtic/logic operations (parallelizable)
- commutative and associative functions (can be applied seperately)
- often readily availble (transparent)

**Challenge**: 

1. Limited compute power
2. Stringent constrains on packet processing time

**Similar studies and differences**:

1. Change network architecture [8, 21]
2. Build switch chip [15]
3. (Ours) flexible, programmable data planes

**Model**: [DAIET](https://mcanini.github.io/papers/daiet.p-socc17.pdf)

**Focus**: Judicious Network Computing 

#### 4. Solution Sketch

**Aggregation Tree (in a map-reduce environment)**: spanning tree covering all paths from all the mappers to reducer



---



## Talk

## Rise of ML-based Solution

**New interest of ML in network**: Machine learning to solve congestion network, etc.

**Good**:

1. Have rich domain knowledge
2. tradition for well-principled solution

**Hmm:**

1. Many problem don't have closed form solution
2. Hard to scale heuristics to wide dynamic senario



### The problem

**The problem**: How do we know the learning model behave reliably and as inteded ?

1. ML is black box with black magic. 
2. Uncertainty: why they work? performance? 

**Hope:** 

1. Performance/accuracy (black box) VS trustworthy (white box) is not a fundamental trade-off
2. A continuum of solution exists
   1. interpretable ML tools (DAIET)
   2. integrating domain knowledge

**Narrowing the scope: Neural Policy, ordered sequence, assume intellegible feature & domain knowledge**

**Interest**

**Neural Policy**: mapping states to action

**Order sequences** of decisions (input, output)

Assume **intelligible features** and **domain knowlege**

- **Measureable Quantities**: What is the buffer? What is the bandwidth?

Test the behaviour of the **system ( the model and the environment it performs on) **  to a high level spectrum

- **System**: composition of model and environment

**Not Interest**

- Fine Grain
- Robustness
- Safe exploration



### Typical source of policies

**Reinforcement Learning** well fits control of systems

- Train an agent
- Make the agent action
- Acting to the environment
- React to the environment and adjust its behaviour

For example: an agent act to the network, observe the traffic and all the related metrics and learn from those.

adaptive reconfigurate

**Basic Methods**

1. Extract information: extract information from the ML network
2. Interpret ML: Tools for interpreting ML -- offer insight into ML model 
   1. **==(What we picked)== Instance-wise feature selection**: What is the contribution for a specific feature for a particular input in this model? (quantify the contribution for each instance -- make sense of the explanation) ==Maybe they want to fit into their domain knowledge to have an explanation for this ... output. Best fit for the research!!==
   2. **Detecting Neuron Activations**: How did the network learnt? (we want the human is an expert to the domain knowledge, not to ML)
   3. **Extracting high-level rules**: (e.g., from neural policy we want to extract the ) ==?I didn't understnad this part==

**Instance-wise feature selection**

(post-hoc interpretation of why a model give a particular output on a given input, is importance of feature in line with the domain knowledge)

**Example: Job scheduler**.  ==?????==

Job name is related to the performance: ?????

But by analysis, we find there is some relation that maybe differnet jobs are concurrent with each other, and by showing job name, we know who are concurrent.

**Model are not entirely black-box**

1. **Detection step**: Flag in *misbehaviour*  that will be detected
   1. What is *misbehaviour* and how they detected: 
2. **Correction step (box) -- Debugger, Retrainer**: visit the dataset, change the policy, etc. (not a one-shot approach)

**On Properties**

- **(High level) Assertions on System Behaviour**: expressed in temporal logic / some code
- **Facts**
  - Model input, output
  - Observed state, reward
  - Contribution of features

**Architectutre [Figure]**

```
Input -> Model -> Output
interpretable ML
explanation -> Detection -> Correction
Domain knoledge -> properties /conditions
```



### Concrete Case: [Pensieve](https://people.csail.mit.edu/hongzi/content/publications/Pensieve-Sigcomm17.pdf)

**What it did:** a vedio rate adaptation system based on a shallow neural network trained via reinformcement learning

**Why we use it**: becasue author kindly made code and models availble :)

**When you switch betwee nbit raates, how smooth is the bits transformed?**

**Measures:** 

1. what is the network bandwidth? 
2. previow decision for each bit rate? 
3. what is the current buffer level in client side? ...

**Objectives & Methodologies**

1. Despite improved performace, we observe that Pensieve has **unwanted behaviour**. We used **LIME (locally interpretable ml explaniation)** to answer: **How much does a feature contribute to the decision?**
2. **Collecte Data from Pensieve**. 
   1. Run the model
   2. Colelct system traces (state of the system, inpput s to the agent, decisions... traces enriched with contribution of each feature -> contribution of the feature)
3. **Calculate the Contribution**
   1. a simple model might suffice
   2. Property: each input feature contributes in the decision
      1. Compute the average contribution
      2. Feature with lower contribution might not be useful
      3. ==Why is the Throughput [T-7] so high? -- Q by remzi==
         1. Correction box has not built yet
         2. It's hard to determine which kind of feature / element cuase the behaviour
      4. ==Are the metrics independent?==
         1. No. They might have some correlation
         2. Observation: Decision are made by round. We thinks that the decisions.
         3. Pretend the anyother features are not important... Let's avoid complexity for now.
      5. ==Why are we using interpretable ML method to explain ML model?==
         1. IML is not ML: it's just some LA method. We should rather say it's a statistical analysis tool.
4. **Ignored classes**: QoE might not be optimal ==???? Questions1: Idon't get it...==
   1. Properties: all admissible decisions are useful.
   2. [Figure: CDF of speed in test trace]
   3. [Figure: Frequency of each class selection]
   4. **Why do you want to select these classes of bitrates** ?
      1. The **optimal** is the encoding at network speed. Ignored classes means suboptimal QoE.
      2. [Figure: Difference with optimal ...] -- fine-grained decision might never made in some of the regions.
5. **Decision Instability**: traces of non-smooth video experience ==???? Questions2: Idon't get it...==
   1. Property: The selected bit rate is stable.
   2. **Experiment**: Force the system to go back in time. Force the system to select the same bit rate. Measure the class changes.
6. **Decision Uncertainty**
   1. Property: the agent is **confident** in its decision
      1. Confident: know what it's going to pick -- (1/2, 1/2) is not good
      2. [Figure: Confidence of the top bit rates]



### Summary

We want to understand why ml methods are doing the decision. Are they misbehaving?

Introduce: **Instance-wise feature selection** (false-positive checker, interpretable ML method)

Apply: Pensieve



### Question after talk

==How do we trust the analysis?==

1. Detection - more robust methodology
2. Why not simpler model? Why neural network?
3. We could do verification-based method? 