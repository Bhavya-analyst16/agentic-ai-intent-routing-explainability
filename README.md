# Evaluating Agentic AI in Business Analysis: Intent Routing Accuracy and Error Explainability

MSc Business Analytics dissertation, Robert Gordon University, Aberdeen (2026)
Author: Bhavya Bhardwaj | Supervisor: Professor Nirmalie Wiratunga

## Overview

When an AI agent gives a wrong answer to a business question, users can't tell whether it **misunderstood the question** (a routing error) or **understood it but analysed it badly** (an execution error). Its explanation may also sound convincing without being honest.

This project builds and compares three agentic AI architectures that route natural language business queries to specialist agents. It measures:

1. **Intent routing accuracy**: does the query reach the right workflow?
2. **Error explainability**: when routing fails, can the system flag that honestly?

## Dataset

[Olist Brazilian E-Commerce dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (Kaggle, CC BY-NC), around 100,000 anonymised orders. The raw data is **not included** in this repository because of its licence. Download it from Kaggle to rerun the notebooks.

Three analytical workflows were used:

| Workflow | Focus | Example query |
|---|---|---|
| W1 | Customer Behaviour Analytics | "Will this customer come back and buy again?" |
| W2 | Marketing Funnel & Campaign Analysis | "Did this lead convert into a paying customer?" |
| W3 | Product & Seller Performance Analysis | "Is this seller getting good or bad feedback?" |

## Evaluation set

30 labelled queries, 10 per workflow: 18 straightforward, 6 ambiguous and 6 multi-intent (`evaluation/labelled_queries.csv`).

## Architectures

| Architecture | Design pattern | What it does |
|---|---|---|
| 1. Pure Routing | Routing | Keyword-based orchestrator sends each query to one specialist agent. No fallback. |
| 2. Orchestrator-Workers | Orchestrator-Workers | Scores all workflows, then applies a SINGLE, MULTI or FALLBACK strategy. |
| 3. Reflection + Dual Prompting | Reflection | Adds a reflection agent that writes a factual and a contrastive explanation for each decision and issues a CONFIRM, UNCERTAIN or REVISE verdict. |

## Results

| Metric | Arch 1 | Arch 2 | Arch 3 |
|---|---|---|---|
| Overall accuracy | 83.3% (25/30) | 90.0% (27/30) | 90.0% (27/30) |
| Straightforward | 100% | 100% | 100% |
| Ambiguous | 66.7% | 100% | 100% |
| Multi-intent | 50.0% | 50.0% | 50.0% |
| W1 / W2 / W3 | 100 / 80 / 70% | 100 / 90 / 80% | 100 / 90 / 80% |

Key findings:

- **Fallback handling fixed ambiguous queries.** Ambiguous accuracy rose from 66.7% to 100%, a 33.3 percentage-point gain.
- **Multi-intent queries stayed at 50%** across all three architectures, which is a ceiling for keyword-based routing.
- **Architecture 3 matched Architecture 2 on accuracy but added honest uncertainty.** It gave CONFIRM on all 27 correct routings and UNCERTAIN on all 3 misrouted ones.

## Repository structure

- `notebooks/` holds the Google Colab notebooks (01 data exploration, 02 to 04 the three architectures, 05 comparison and results)
- `results/` holds per-architecture results (CSV, JSON), architecture diagrams and comparison charts
- `evaluation/` holds the 30 labelled queries

## Running the notebooks

The notebooks were developed in Google Colab with data stored in Google Drive.

1. Download the Olist dataset from Kaggle.
2. Place the CSV files in `MyDrive/Dissertation/Data` and `labelled_queries.csv` in `MyDrive/Dissertation/Evaluation`.
3. Run the notebooks in order.

Tools: Python, pandas, matplotlib, Google Colab.

## Limitations

- The evaluation set is small (30 queries).
- Routing is keyword-based, which limits performance on queries that span two workflows.
- Faithfulness is assessed at output level only, not by inspecting the system's internal reasoning.

## Licence

Code is released under the MIT Licence. The Olist dataset is subject to its own CC BY-NC licence.
