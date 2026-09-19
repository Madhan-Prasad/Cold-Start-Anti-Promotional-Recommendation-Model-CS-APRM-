# Cold-Start Anti-Promotional Recommendation Model (CS-APRM)

A mathematical optimization framework formulated as an Integer Linear Program (ILP) to mitigate promotional bias and solve the new-item cold-start problem in digital streaming recommendation engines (e.g., Netflix, Prime Video, JioHotstar).

---

##  Project Overview
Mainstream streaming platforms frequently prioritize high-budget, commercially promoted titles on user interfaces, overshadowing organic cold-start titles that better match user preferences. 

This repository implements the **CS-APRM** model, which:
- Quantifies organic alignment via **Cosine Similarity** between user watch profiles and cold-start metadata vectors.
- Applies a soft regularized penalty ($\beta$) against artificial promotional intensity ($p_i$).
- Enforces hard rank-level constraints and an aggregate slate promotional budget ($P_{\max}$).
- Generates position-discounted recommendation slates ($K$) solved via `scipy.optimize.milp`.

---

##  Mathematical Model

$$\max_{\mathbf{x}} \sum_{u \in U} \sum_{k=1}^K \sum_{i \in I_{\text{new}}} w_k \left( \text{CosSim}(\mathbf{u}_u, \mathbf{v}_i) - \beta \cdot p_i \right) x_{u,i,k}$$

Subject to:
1. **Single Item Assignment:** $\sum_{i \in I_{\text{new}}} x_{u,i,k} = 1 \quad \forall k$
2. **Slate Uniqueness:** $\sum_{k=1}^K x_{u,i,k} \le 1 \quad \forall i$
3. **Promotional Budget Cap:** $\sum_{k=1}^K \sum_{i \in I_{\text{new}}} p_i \cdot x_{u,i,k} \le P_{\max}$

---

##  Key Findings & Simulation Results
- **Promotional Reduction:** Cuts total slate promotional footprint by **$54\%$** compared to standard unconstrained baselines.
- **High Organic Retention:** Sustains **$>97\%$** of user Discounted Cumulative Organic Gain (DCOG@10).
- **Long-Tail Discovery:** Increases screen exposure for low-budget, organic indie releases from $10\%$ to $50\%$ on top recommendation slates.

---

##  Repository Structure
```text
├── plots/                                 # Generated publication-quality figures
│   ├── plot_1_beta_sensitivity.png        # Penalty parameter sensitivity curve
│   ├── plot_2_pareto_frontier.png         # Organic alignment vs. promo budget
│   ├── plot_3_positional_intensity.png    # Slot-by-slot promotional scores
│   └── plot_4_organic_retention.png       # Cosine similarity retention across ranks
├── cs_aprm_simulation.py                  # Complete simulation, ILP solver, & plotting scripts                      
└── README.md
