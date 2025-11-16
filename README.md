# Citation Graph Construction & Link Prediction

Course project for **CS768: Learning With Graphs** (Spring 2025, IIT Bombay) under **Prof. Abir De**.

The project builds a **directed citation graph** from BibTeX-style metadata and then performs **link prediction** to suggest likely missing citations for a given paper.

---

## Overview

The work is split into two main tasks:

- **Task 1 – Citation Graph Construction**
  - Parse `.bib` / `.bbl` files for a collection of papers.
  - Normalize and fuzzy-match titles to build a clean mapping from titles to paper folders.
  - Enforce **temporal sanity checks** to remove:
    - Future citations (citing a paper that appears to be published later in time).
    - Self-citations based on folder/title mapping.
  - Construct a directed **NetworkX** graph with:
    - Nodes = papers.
    - Edges = “paper A cites paper B”.
  - Run basic structural analysis (degree distributions, components, diameter, etc.) and save summaries.

- **Task 2 – Link Prediction on the Citation Graph**
  - Use text embeddings to represent node abstracts.
  - Compute **in-neighbor similarity features** between pairs of papers.
  - Train a **Random Forest** model to predict whether a citation edge should exist.
  - Compare against a **similarity-weighted citation voting** baseline.
  - Evaluate models using **Recall@20 ≈ 0.40** on a small held-out set of test papers.

---

## Repository Structure

```text
CS768ASS-main/
├── CS768_Assignment_Report.pdf      # Project report
├── TASK1/
│   ├── parallelized.py              # Main script for citation graph construction
│   ├── README.txt                   # Short note about the saved run
│   └── SAVED_RUN/
│       ├── citation_graph.pkl       # Saved directed citation graph
│       ├── nodes_parallelized.txt   # List of nodes from the saved run
│       ├── edges_parallelized.txt   # List of edges from the saved run
│       ├── summary_directed.txt     # Analysis of directed graph
│       ├── summary_undirected.txt   # Analysis of undirected version
│       ├── directed_citation_graph_degree_histogram*.png
│       ├── undirected_citation_graph_degree_histogram*.png
│       └── plotter.py               # Utility for graph analysis and plots
└── TASK2/
    ├── Q2.ipynb                     # Notebook with experiments / training
    ├── citation_graph.pkl           # Graph copy used for link prediction
    ├── link_predictor_model.pkl     # Trained Random Forest model
    ├── utils.py                     # Text normalization & similarity features
    ├── evaluation.py                # CLI entrypoint for link prediction
    └── run_evaluation.py            # Helper wrapper around evaluation.py
