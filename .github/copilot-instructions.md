# Optimal Power Flow (OPF) Project Instructions

This project implements Optimal Power Flow (OPF) calculations using Python and Pyomo, solving via the NEOS Server.

## Architecture & Key Components

- **`opf_starter.ipynb`**: The primary workspace. Contains model initialization, data loading logic, and constraint definitions.
- **`opf_utils.py`**: Helper library. Handles:
  - Solving models via NEOS (`solve_and_print`).
  - Result visualization (`print_result`).
  - Exporting results (`create_results_json`).
- **Data**: Located in `data/OPFData.xlsx`.

## Development Workflow

1.  **Setup**:
    -   Set `os.environ['NEOS_EMAIL']` in the notebook to a valid email for NEOS access.
    -   Load data using `pandas` (e.g., `pd.read_excel`).

2.  **Modeling (Pyomo)**:
    -   Use `pyomo.environ` (aliased as `pyo`).
    -   Define a `pyo.ConcreteModel()`.
    -   Populate model parameters from the loaded data.

3.  **Execution**:
    -   Use `opf_utils.solve_and_print(m, AC=True)` for AC OPF (uses `conopt` solver).
    -   Use `opf_utils.solve_and_print(m, AC=False)` for DC OPF (uses `cplex` solver).

## Project Conventions

### Model Attributes (Required by `opf_utils.py`)
The `opf_utils.py` functions expect specific attribute names on the Pyomo model `m`. Ensure these are defined:

**Variables:**
-   `m.x_p`: Active Power Generation (indexed by node).
-   `m.x_q`: Reactive Power Generation (indexed by node) [AC only].
-   `m.x_v`: Voltage Magnitude (indexed by node) [AC only].
-   `m.x_v_angle`: Voltage Angle (indexed by node).

**Parameters (Typical Usage):**
-   `m.nodes`: Set/List of node indices.
-   `m.a`, `m.b`, `m.c`: Cost coefficients for generation.
-   `m.P_load`, `m.Q_load`: Active and Reactive load at nodes.
-   `m.G`, `m.B`: Real and Imaginary parts of the admittance matrix.
-   `m.V_max`, `m.V_min`: Voltage limits.
-   `m.P_G_max`, `m.P_G_min`: Active power generation limits.

### Data Loading Pattern
When implementing `add_node_data` or admittance functions:
1.  Read the Excel file using `pd.read_excel(filename, sheet_name=...)`.
2.  Extract columns to dictionaries or lists.
3.  Assign to model attributes (e.g., `m.P_load = dict(zip(df['Node'], df['P_load']))`).

## Common Issues
-   **NEOS Error**: Ensure the email is set and valid.
-   **AttributeError**: Check that your Pyomo model variable names match exactly what `opf_utils.py` expects (`x_p`, `x_v`, etc.).
