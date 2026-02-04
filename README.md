# Donation Allocation Optimization

An optimization model for allocating charitable donations to maximize impact across multiple organizations and project types.

## Project Overview

This project develops mathematical optimization models to determine optimal donation allocation for the **Fondation Pierre et Yolande Gingras**. The model allocates a fixed budget across multiple charitable projects while considering:

- **Multiple charities** with different minimum allocation requirements
- **Project constraints** including cost caps, iteration limits, and minimum funding thresholds
- **Multi-dimensional impact** measured across education, community, and health dimensions
- **Project dependencies** where some projects can only be funded if prerequisites are met
- **Term-based constraints** ensuring minimum allocation to long-term initiatives

## Key Features

- **Two Optimization Approaches**: Implements both weighted sum and goal programming models
- **Budget Constraints**: Respects total budget allocation and charity-specific minimums
- **Project-Level Controls**: Handles iteration limits, minimum funding requirements, and cost caps
- **Dependency Management**: Enforces prerequisite relationships between projects
- **Multi-Criteria Objectives**: Balances impact across education, community, and health sectors
- **Sensitivity Analysis**: Tests weight variations to understand model robustness

## Technical Stack

- **Optimization Engine**: Gurobi (Python API)
- **Data Processing**: Pandas, NumPy
- **Environment**: Python 3.x, Jupyter Notebook

## Data Requirements

The model requires an Excel file (`project_data.xlsx`) with the following sheets:
- **Data sheet**: Project information including:
  - Project ID and charity assignment
  - Cost caps, minimum funding, maximum iterations
  - Impact scores (education, community, health)
  - Project dependencies and term classification
  
- **Weights sheet**: Charity-specific impact weights

## Mathematical Model

### Sets and Indices

| Symbol | Description |
|--------|-------------|
| $P$ | Set of all projects |
| $P_{\text{JMDM}}$ | Projects belonging to JMDM charity |
| $P_{\text{CSM}}$ | Projects belonging to CSM charity |
| $P_{\text{LT}}$ | Long-term projects |
| $p$ | Index for projects |

### Parameters

| Symbol | Description | Value |
|--------|-------------|-------|
| $B$ | Total budget | 50,000 |
| $A_{\text{JMDM}}$ | Minimum allocation to JMDM | 10,000 |
| $A_{\text{CSM}}$ | Minimum allocation to CSM | 15,000 |
| $L$ | Minimum proportion for long-term projects | 0.75 |
| $C_p$ | Cost cap (per iteration) for project $p$ | — |
| $M_p$ | Minimum funding required if project $p$ is funded | — |
| $N_p$ | Maximum number of iterations for project $p$ | — |
| $w_p$ | Total impact weight for project $p$ | — |
| $e_p, c_p, h_p$ | Education, community, health impact scores | — |

### Decision Variables

| Variable | Type | Description |
|----------|------|-------------|
| $x_p$ | Continuous | Amount (in dollars) allocated to project $p$ |
| $y_p$ | Integer | Number of iterations for project $p$ |

### Constraints

**Budget Constraint:**

$$\sum_{p \in P} x_p \leq B$$

**Charity Minimum Allocations:**

$$\sum_{p \in P_{\text{JMDM}}} x_p \geq A_{\text{JMDM}}$$

$$\sum_{p \in P_{\text{CSM}}} x_p \geq A_{\text{CSM}}$$

**Long-Term Project Requirement:**

$$\sum_{p \in P_{\text{LT}}} x_p \geq L \cdot \sum_{p \in P} x_p$$

**Allocation-Iteration Linkage:**

$$x_p = y_p \cdot C_p \quad \forall p \in P$$

**Iteration Limits:**

$$y_p \leq N_p \quad \forall p \in P$$

**Minimum Funding Threshold:**

$$x_p \geq M_p \cdot \mathbb{1}_{[x_p > 0]} \quad \forall p \in P$$

**Dependency Constraints:**

$$x_p \leq x_q \cdot \frac{C_p}{M_q} \quad \forall p \text{ where } p \text{ depends on } q$$

---

### Model 1: Weighted Sum Approach

**Objective:** Maximize total weighted impact based on funding efficiency.

$$\text{Maximize} \quad Z = \sum_{p \in P} \frac{x_p}{C_p} \cdot w_p$$

This approach prioritizes projects that deliver the most impact per dollar spent.

---

### Model 2: Goal Programming Approach

**Additional Variables:**

| Variable | Description |
|----------|-------------|
| $S_{c,k}$ | Calibrated score for charity $c$ in dimension $k$ |
| $T_c$ | Total calibrated score for charity $c$ |
| $d^+_{c,k}, d^-_{c,k}$ | Positive/negative deviation from target proportion |
| $d^-_B$ | Budget underutilization deviation |

**Calibrated Scores:**

$$S_{c,k} = \sum_{p \in P_c} \frac{x_p}{C_p} \cdot s_{p,k} \quad \forall c \in \{\text{JMDM}, \text{CSM}\}, \; k \in \{e, c, h\}$$

$$T_c = S_{c,e} + S_{c,c} + S_{c,h} \quad \forall c$$

**Goal Constraints:**

$$S_{c,k} + d^-_{c,k} - d^+_{c,k} = \pi_{c,k} \cdot T_c \quad \forall c, k$$

where $\pi_{c,k}$ is the target proportion for charity $c$ in dimension $k$.

**Objective:** Minimize deviations while maximizing impact.

$$\text{Minimize} \quad W_B \cdot d^-_B + W_P \sum_{c,k} \left( d^+_{c,k} + d^-_{c,k} \right) - W_I \cdot Z$$

where:
- $W_B$ = weight for budget utilization
- $W_P$ = weight for proportional deviations
- $W_I$ = weight for total impact

This approach ensures proportional representation of impact goals while respecting all constraints.

## Usage

1. Prepare your `project_data.xlsx` file with project and weight information
2. Run the Jupyter notebook `Final Project Base Model.ipynb`
3. Review the optimization results and solution analysis

## Results Output

The model generates:
- Optimal allocation amounts for each project
- Number of iterations per project
- Total impact scores by dimension
- Constraint satisfaction verification
- Budget utilization summary


## License

This project is created as part of an academic assignment.
