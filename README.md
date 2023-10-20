# graphbasedloading

## Introduction
This is a project from the CMU/DNAnexus Hackathon 2023, in which we are improving LLMs’ inferences for drug treatment recommendations by fine-tuning them with knowledge graphs.

## Research Motivation
Given that LLMs can answer queries quickly and efficiently, they could be useful in recommending drug treatments for various diseases, where time is of the essence. However, as their training data may be factually incorrect or outdated, LLMs are often unreliable in recommending drug treatments. 

Knowledge graphs can store factually correct relationships between data points, giving them the ability to answer complex queries. Hence, we believe that fine-tuning LLMs with knowledge graphs could aid LLMs in recommending drug treatments.

## Preliminary Workflow
```mermaid
graph LR;
  A["Small-scale experiments
  following LLM & KG paper"]
  B["Generate a few hundred
   relationship graph for a
   small scale experiment"]
  D["Load raw graph data (e.g. Drugbank)"]
  C["Encode entire graph in
    triple form as text
    LLM context"]
  X["Use KG based context for
fact based grounding"]
  E{"Graph fits in
context window?"}
  F["Generate query for
 relevant triples"]
  G["Extract triples from graph"]
  H{"Small enough for
context window?"}
  I["Hierarchically refine
query prompt with triples"]
  A-->D
  B-->D
  D-->E
  E-->|Yes|C
  C-->X
  E-->|No|F
  F-->G
  G-->H
  H-->|Yes|X
  H-->|No|I
  I-->F
```

## Methodology
Dataset: We compiled 44,663 drug-relationship-target triplets from the xxx database, which are in “drug_relationship_target.csv”.

Preprocessing: The triplets were preprocessed into a prompt-response format for LLAMA2, see “inputdata.txt”. Sample: <s>[INST] Tell me more about the drug with ID D07OAC. [/INST] Drug D07OAC is an inhibitor to target protein S5A2_HUMAN. </s>

Training and Inference: A LLAMA2-7b model was then fine-tuned on the preprocessed data.

## References
Pan, S., Luo, L., Wang, Y., Chen, C. et al. Unifying Large Language Models and Knowledge Graphs: A Roadmap. 20 June 2023, https://doi.org/10.48550/arXiv.2306.08302
