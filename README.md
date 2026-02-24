# 🛒 Amazon Co-Purchasing Network Analysis

<div align="center">

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![NetworkX](https://img.shields.io/badge/NetworkX-Graph%20Analysis-orange?style=for-the-badge)
![Gephi](https://img.shields.io/badge/Gephi-Visualization-brightgreen?style=for-the-badge)
![NumPy](https://img.shields.io/badge/NumPy-Vectorized-013243?style=for-the-badge&logo=numpy&logoColor=white)

**DATA 620 — Week 3: Graph Visualization**

*Candace Grant | CUNY School of Professional Studies | February 2026*

</div>

---

## 📌 Project Overview

This project analyzes Amazon's **"Customers Who Bought This Item Also Bought"** co-purchasing network. The dataset contains **403,394 products** and **3,387,388 co-purchase relationships**, collected on June 1, 2003.

I performed graph analysis in Python and created an interactive network visualization in Gephi to explore how products cluster together through purchasing behavior.

---

## 📊 Dataset

| Metric | Value |
|:-------|------:|
| 🔵 **Nodes (Products)** | 403,394 |
| 🔗 **Edges (Co-purchases)** | 3,387,388 |
| 📐 **Diameter** | 21 |
| 🔺 **Avg Clustering Coefficient** | 0.4177 |
| 🌐 **Largest WCC Coverage** | 99.9% |

> **Source:** [SNAP Stanford — Amazon0601](https://snap.stanford.edu/data/amazon0601.html)
>
> *Citation: J. Leskovec, L. Adamic & B. Adamic. The Dynamics of Viral Marketing. ACM TWEB, 1(1), 2007.*

---

## 🔬 Analysis Pipeline

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  📥 LOAD     │────▶│  🎯 SAMPLE   │────▶│  📏 METRICS  │────▶│  🎨 VISUALIZE│
│  403K nodes  │     │  10K nodes   │     │  Diameter &  │     │  500 nodes   │
│  3.4M edges  │     │  Snowball    │     │  Clustering  │     │  in Gephi    │
│  (vectorized)│     │  BFS         │     │  (hand-coded)│     │  (.gexf)     │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
```

### Step 1 — Load the Graph
- Downloads the compressed dataset directly from SNAP Stanford
- Uses **vectorized NumPy loading** (`np.loadtxt()`) for 5–10x faster performance over line-by-line reading

### Step 2 — Snowball Sampling
- Full graph (400K+ nodes) is too large for exact metric computation
- **BFS snowball sampling** from a high-degree seed node extracts 10,000 nodes
- Preserves local network structure: clustering patterns, communities, and hub-spoke topology

### Step 3 — Connected Components
- Identifies weakly and strongly connected components
- Extracts the **largest connected component** as the primary analysis graph

### Step 4 — Diameter (Hand-Coded ✍️)
- Implemented **BFS eccentricity** from scratch to estimate the network diameter
- Runs BFS from 100 sampled nodes, tracks the maximum shortest path found
- ✅ Verified against NetworkX's approximation algorithm

### Step 5 — Clustering Coefficient (Hand-Coded ✍️)
- Implemented the **triangle-counting formula** from scratch:
  `C(v) = 2 × triangles(v) / (degree(v) × (degree(v) − 1))`
- Computed across 2,000 sampled nodes
- ✅ Verified against NetworkX's `average_clustering()`

### Step 6 — Gephi Export & Visualization
- Exported a **500-node subgraph** as `.gexf` with node attributes:
  degree, clustering coefficient, betweenness centrality, eigenvector centrality, and community labels
- Loaded into **Gephi** for interactive network visualization

---

## 📁 Repository Contents

| File | Description |
|:-----|:------------|
| `Copy_of_DATA620WK3.ipynb` | 📓 Main analysis notebook (Python) |
| `gephi_amazon_500.gexf` | 🎨 Gephi visualization file (500-node subgraph) |
| `README.md` | 📄 This file |

> 💡 The Amazon dataset is **not included** in this repo — the notebook automatically downloads it from SNAP Stanford when you run it.

---

## 🚀 How to Run

```bash
# 1. Clone the repo
git clone https://github.com/YOUR_USERNAME/DATA620-Week3.git
cd DATA620-Week3

# 2. Install dependencies
pip install networkx numpy pandas matplotlib

# 3. Run the notebook
jupyter notebook Copy_of_DATA620WK3.ipynb

# 4. Open the Gephi file (requires Gephi installed)
#    File → Open → gephi_amazon_500.gexf
```

---

## 🔑 Key Findings

🔹 **The network is highly connected** — 99.9% of all products belong to the same weakly connected component

🔹 **Products form tight co-purchase clusters** — The clustering coefficient of ~0.42 means that if customers buy Product A with both B and C, there's a 42% chance B and C are also co-purchased

🔹 **Any two products are at most ~21 hops apart** — The diameter of 21 shows the network is compact despite its massive size

🔹 **Both metrics were implemented by hand** before verifying with NetworkX, demonstrating understanding of the underlying BFS and triangle-counting algorithms

---

## 🛠️ Technologies

- **Python 3.10+** — Core language
- **NetworkX** — Graph construction and verification
- **NumPy** — Vectorized data loading and array operations
- **Pandas** — Data export and tabular operations
- **Matplotlib** — Plotting
- **Gephi** — Interactive network visualization

---

<div align="center">

*Built for DATA 620: Web Analytics — CUNY School of Professional Studies*

</div>
