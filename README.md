# Optimal Power Flow

This repository contains a starter project for solving an optimal power flow (OPF) problem using Python and Pyomo. The goal is to model a power system, enforce network and generation constraints, and optimize generator dispatch while minimizing operating cost.

The project is structured around a notebook workflow and a small utility module for solving and exporting results.

## Project overview

Optimal power flow is a core optimization problem in power systems. In this project, the model includes:

- bus-level data such as demand, generator limits, voltage bounds, and cost coefficients
- AC and DC OPF formulations
- generator dispatch decisions
- voltage angle and magnitude variables
- result printing and JSON export helpers

The cost model follows a quadratic generator cost function:

$$
C_i(p_i) = a_i p_i^2 + b_i p_i + c_i
$$

where $p_i$ is the real power output at node $i$.

## Repository contents

- `opf_starter.ipynb` – main notebook with the OPF setup, data interpretation, and solve workflow
- `opf_utils.py` – helper functions for cost calculation, solving the model, printing results, and creating output JSON
- `data/OPFData.xlsx` – example input data used to build the optimization model
- `Stage0.csv` – additional dataset included in the repo

## Requirements

This project relies on the following Python packages:

- `pyomo`
- `pandas`
- `numpy`
- `openpyxl`

A typical environment setup is:

```bash
python -m venv .venv
source .venv/bin/activate
pip install pyomo pandas numpy openpyxl
```

## Getting started

1. Open the notebook in `opf_starter.ipynb`.
2. Set your NEOS email address before solving remotely:

```python
import os
os.environ['NEOS_EMAIL'] = 'you@example.com'
```

3. Run the notebook cells to load the data, build the model, and solve the AC or DC OPF problem.

## Core workflow

The utilities in `opf_utils.py` provide the main operations:

- `calculate_costs(m)` – computes the total objective value
- `print_result(m, AC=True)` – prints node values for voltage and power variables
- `solve_and_print(m, AC=True)` – solves the model and prints the solution
- `create_results_json(m, team_name, AC=True)` – writes a JSON summary of the optimized results

## Example usage

```python
from pyomo import environ as pyo
from opf_utils import solve_and_print, create_results_json

AC = True
model = load_model('data/OPFData.xlsx', pyo.ConcreteModel(), AC)
solve_and_print(model, AC)
# create_results_json(model, 'Team_A', AC)
```

## Notes

- Solver access may depend on NEOS or a local optimizer available on your machine.
