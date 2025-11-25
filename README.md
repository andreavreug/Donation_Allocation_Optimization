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

- **Weighted Sum Optimization**: Maximizes total weighted impact based on funding ratios and project characteristics
- **Budget Constraints**: Respects total budget allocation and charity-specific minimums
- **Project-Level Controls**: Handles iteration limits, minimum funding requirements, and cost caps
- **Dependency Management**: Enforces prerequisite relationships between projects
- **Multi-Criteria Objectives**: Balances impact across education, community, and health sectors

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

## Model Structure

### Decision Variables
- `x[p]`: Amount (in dollars) allocated to project p
- `y[p]`: Number of iterations for project p

### Constraints
- Budget constraint (total allocation ≤ $50,000)
- Charity minimum allocations ($10,000 to JMDM, $15,000 to CSM)
- Long-term project minimum (≥75% of total allocation)
- Project-specific constraints (cost caps, iteration limits, minimum funding)
- Dependency constraints (prerequisite validation)

### Objective Function

$$\text{Maximize} \quad \sum_p \frac{x_p}{\text{cost\_cap}[p]} \times \text{total\_weight}[p]$$

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

## Team

- Sofia Berumen Ramos
- Romane Lucas-Girardville
- Andrea Vreugdenhil
- Gina Yassin

**Course**: MGSC 662 - Final Project | McGill University

## License

This project is created as part of an academic assignment.
