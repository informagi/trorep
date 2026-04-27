# The Relevance of (Relevant) Entity Presence
This repository provides the data and analysis suite for our reproduction paper "The Relevance of (Relevant) Entity Presence". \
Specifically:
- A reproduction of ["Query-specific Document and Entity Representations" (QDER)](https://github.com/shubham526/SIGIR2025-QDER) (SIGIR '25).
- An analysis of ["Document Re-Ranking Using Entity-based Query Understanding" (DREQ)](https://github.com/shubham526/ECIR2024-DREQ) (ECIR '24).

> [!WARNING]
> This reproduction analyzes the [QDER implementation](https://github.com/shubham526/SIGIR2025-QDER) at commit [`[ab2cd49153b2abdebcdfb7df5b77b7850f52a190]`](https://github.com/shubham526/SIGIR2025-QDER/tree/ab2cd49153b2abdebcdfb7df5b77b7850f52a190).
>
> Although the original repository has been updated since our reproduction/analysis, the **entity filtering and relevance leakage issues** identified in our findings remain present in the current codebase.
>
> Note that several scripts have been renamed in the new version:
> | Our Analysis (Commit `ab2cd4`) | Current QDER Master (Commit `c6e4a0`) |
> | :--- | :--- |
> | `make_entity_qrels.py` | `make_entity_ranking_qrels.py` |
> | `make_entity_data.py` | `make_entity_ranking_data.py` |
> | `create_rerank_data.py` | `make_doc_ranking_data.py` |

## Abstract / Findings
Query-Specific Document and Entity Representations (QDER) has
shown state-of-the-art effectiveness on multiple information retrieval benchmarks. This work attempts to reproduce this promising approach. In an initial attempt, we were unable to find results
that resemble the convincing effects measured in the original work.
A deep dive in the published artifacts, plus a comparative study
to similar work by the same authors show 
1. inconsistencies between the descriptions in the paper and their implementation in
the published artifacts, 
2. selectively reporting metrics computed
over the ranked judged documents only, and, unfortunate in combination with the previous observation, 
3. a leakage of relevance
assessments due to oversampling entities that are known to appear in relevant documents only. 

Due to many dependencies in
the ranking pipeline, the final ranking models may have gained
knowledge about relevance of entities by accident.
We conclude that knowing the relevance of entities for an information need can
be very valuable, but that the methods described are insufficient
to determine this relevance information from the documents and
queries alone.

## Technical Details
- Recommended Python version: `3.13`
- Data Artifacts: Refer to [DATA.md](DATA.md) for details on model checkpoints and experimental results/rankings.
- Analysis Tools: Refer to [USAGE.md](USAGE.md) for documentation on the analysis and evaluation scripts.

## Reproduction Pipelines

| Entity Ranker Pipeline                        | Document Re-Ranker Pipeline                       |
|-----------------------------------------------|---------------------------------------------------|
| <img src="graphs/entity-ranker_pipeline.png"> | <img src="graphs/document-reranker_pipeline.png"> |

We provide both our entity ranker and document re-ranker pipelines used for our reproduction in the form of bash scripts.
Each script chains the data preparation, training, and evaluation scripts provided in the official QDER implementation:
- Document Re-Ranker Pipeline: [document-ranker_train_evaluate](pipelines/document-ranker_train_evaluate.sh)
- Entity Ranker Pipeline: [entity-ranker_train_evaluate](pipelines/entity-ranker_train_evaluate.sh)

_(Note: We also provide adjusted versions of the `create_rerank_data.py` and `make_entity_data.py` scripts used in these pipelines. These modifications allow the pipelines to generate training and evaluation data specifically for a provided fold index.)_
