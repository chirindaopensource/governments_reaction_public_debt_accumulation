# README.md


# A Cross-Country Analysis of Government Fiscal Reaction Functions

<!-- PROJECT SHIELDS -->
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Python Version](https://img.shields.io/badge/python-3.9%2B-blue.svg)](https://www.python.org/downloads/)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
[![Imports: isort](https://img.shields.io/badge/%20imports-isort-%231674b1?style=flat&labelColor=ef8336)](https://pycqa.github.io/isort/)
[![Type Checking: mypy](https://img.shields.io/badge/type_checking-mypy-blue)](http://mypy-lang.org/)
[![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=flat&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![NumPy](https://img.shields.io/badge/numpy-%23013243.svg?style=flat&logo=numpy&logoColor=white)](https://numpy.org/)
[![Statsmodels](https://img.shields.io/badge/Statsmodels-150458.svg?style=flat&logo=python&logoColor=white)](https://www.statsmodels.org/stable/index.html)
[![Linearmodels](https://img.shields.io/badge/Linearmodels-013243.svg?style=flat&logo=python&logoColor=white)](https://bashtage.github.io/linearmodels/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-%23ffffff.svg?style=flat&logo=Matplotlib&logoColor=black)](https://matplotlib.org/)
[![Seaborn](https://img.shields.io/badge/seaborn-%233776AB.svg?style=flat&logo=python&logoColor=white)](https://seaborn.pydata.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-%23F37626.svg?style=flat&logo=Jupyter&logoColor=white)](https://jupyter.org/)
[![arXiv](https://img.shields.io/badge/arXiv-2507.13084-b31b1b.svg)](https://arxiv.org/abs/2507.13084)
[![DOI](https://img.shields.io/badge/DOI-10.48550/arXiv.2507.13084-blue)](https://doi.org/10.48550/arXiv.2507.13084)
[![Research](https://img.shields.io/badge/Research-Public%20Finance-green)](https://github.com/chirindaopensource/governments_reaction_public_debt_accumulation)
[![Discipline](https://img.shields.io/badge/Discipline-Macroeconometrics-blue)](https://github.com/chirindaopensource/governments_reaction_public_debt_accumulation)
[![Methodology](https://img.shields.io/badge/Methodology-Panel%20Data%20Analysis-orange)](https://github.com/chirindaopensource/governments_reaction_public_debt_accumulation)
[![Data Source](https://img.shields.io/badge/Data%20Source-IMF%2C%20WB%2C%20OECD-lightgrey)](https://data.imf.org/)
[![Year](https://img.shields.io/badge/Year-2025-purple)](https://github.com/chirindaopensource/governments_reaction_public_debt_accumulation)

**Repository:** `https://github.com/chirindaopensource/governments_reaction_public_debt_accumulation`

**Owner:** 2025 Craig Chirinda (Open Source Projects)

This repository contains an **independent**, professional-grade Python implementation of the research methodology from the 2025 paper entitled **"Do Governments React to Public Debt Accumulation? A Cross-Country Analysis"** by:

*   Paolo Canofari
*   Alessandro Piergallini
*   Marco Tedeschi

The project provides a complete, end-to-end pipeline for estimating sovereign fiscal reaction functions from panel data. It replicates the paper's advanced econometric techniques, including diagnostics for cross-sectional dependence and slope heterogeneity, and features a from-scratch implementation of the Dynamic Common Correlated Effects Mean Group (DCCEMG) estimator. The goal is to provide a transparent, robust, and extensible tool for researchers, policymakers, and financial analysts to assess fiscal sustainability across countries.

## Table of Contents

- [Introduction](#introduction)
- [Theoretical Background](#theoretical-background)
- [Features](#features)
- [Methodology Implemented](#methodology-implemented)
- [Core Components (Notebook Structure)](#core-components-notebook-structure)
- [Key Callable: create_fiscal_credibility_report](#key-callable-create_fiscal_credibility_report)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Input Data Structure](#input-data-structure)
- [Usage](#usage)
- [Output Structure](#output-structure)
- [Project Structure](#project-structure)
- [Customization](#customization)
- [Contributing](#contributing)
- [License](#license)
- [Citation](#citation)
- [Acknowledgments](#acknowledgments)

## Introduction

This project provides a Python implementation of the methodologies presented in the 2025 paper "Do Governments React to Public Debt Accumulation? A Cross-Country Analysis." The core of this repository is the iPython Notebook `governments_reaction_public_debt_accumulation_draft.ipynb`, which contains a comprehensive suite of functions to replicate the paper's findings, from initial data validation to the final generation of results tables, interpretations, and visualizations.

The central question in public finance is whether governments act to ensure their long-term solvency. This project provides the tools to answer this question by estimating a government's fiscal reaction function—a rule that describes how the primary budget balance responds to changes in the level of public debt. A positive and significant response is a key indicator of a sustainable, or "Ricardian," fiscal policy.

This codebase enables users to:
-   Rigorously validate and clean macroeconomic panel data.
-   Perform state-of-the-art panel data diagnostic tests.
-   Estimate dynamic fiscal reaction functions using the advanced DCCEMG estimator, which accounts for global shocks and country-specific behaviors.
-   Calculate and test the significance of the long-run fiscal response to debt.
-   Systematically conduct robustness and sensitivity analyses to ensure the credibility of the findings.
-   Replicate and extend the empirical results of the original research paper.

## Theoretical Background

The implemented methods are grounded in modern panel data econometrics and the theory of fiscal sustainability.

**The Fiscal Reaction Function:** The analysis centers on estimating a dynamic fiscal policy rule, as proposed by Bohn (1998). The core equation is:
$$
s_{it} = \phi_i s_{i,t-1} + \rho_i b_{i,t-1} + \boldsymbol{\beta_i' x_{it}} + \epsilon_{it}
$$
where `s` is the primary surplus-to-GDP ratio, `b` is the debt-to-GDP ratio, and `x` is a vector of control variables (e.g., output gap, spending gap). The key hypothesis is that **`ρ > 0`**, which implies that governments raise their primary surplus in response to higher debt, ensuring solvency.

**The Long-Run Response (LRR):** The ultimate measure of sustainability is the long-run response, which accounts for policy inertia (`φ`). It is calculated as:
$$
LRR = \frac{\rho}{1 - \phi}
$$

**Econometric Challenges:** Estimating this relationship in a multi-country panel is challenging due to:
1.  **Cross-Sectional Dependence (CSD):** Global shocks (e.g., financial crises, pandemics) affect all countries simultaneously, correlating the error terms `ε_it`.
2.  **Slope Heterogeneity:** The policy rule parameters (`φ_i`, `ρ_i`) are likely different for each country.

**The DCCEMG Estimator:** To address these challenges, the paper uses the Dynamic Common Correlated Effects Mean Group (DCCEMG) estimator (Chudik & Pesaran, 2015). This method augments each country's regression with cross-sectional averages of the variables, which act as proxies for the unobserved global shocks. By averaging the resulting coefficients across countries, it provides a consistent estimate of the average policy rule while respecting country-specific heterogeneity.

## Features

The provided iPython Notebook (`governments_reaction_public_debt_accumulation_draft.ipynb`) implements the full research pipeline, including:

-   **Data Validation:** A rigorous validation module to check data schema, dimensions, and consistency.
-   **Data Cleansing:** Systematic handling of missing values and economically-informed outlier clipping.
-   **Panel Data Diagnostics:** A full suite of tests for slope homogeneity (F-test for poolability), cross-sectional dependence (Pesaran's CD test), and panel unit roots (CIPS test).
-   **From-Scratch DCCEMG Estimator:** A complete and heavily commented implementation of the DCCE Mean Group algorithm.
-   **Long-Run Effects with Delta Method:** Precise calculation of the long-run response and its standard error using the Delta Method.
-   **Automated Robustness & Sensitivity Suite:** A framework to automatically re-run the entire pipeline under alternative specifications (e.g., different lags, GFC definitions, data winsorization, HP filter parameters).
-   **Programmatic Reporting:** Automated generation of a publication-quality results table, a detailed textual interpretation of the findings, and key visualizations.

## Methodology Implemented

The core analytical steps directly implement the methodology from the paper:

1.  **Data Preparation (Tasks 1-3):** The pipeline ingests raw panel data, validates its structure, cleanses it, and sets a proper `MultiIndex` for panel analysis.
2.  **Diagnostics (Tasks 4-5):** It generates descriptive statistics (replicating Table 1) and runs the full suite of panel diagnostic tests to justify the choice of the DCCEMG estimator.
3.  **Estimation (Task 6):** It estimates the 10 different model specifications from Table 2 using the from-scratch DCCEMG estimator, paying strict attention to the correct, subsample-specific calculation of cross-sectional averages.
4.  **Inference (Tasks 7-9):** It calculates the long-run response to debt, computes its standard error via the Delta Method, assigns significance stars, and compiles all results into a formatted table replicating Table 2.
5.  **Validation (Tasks 10-11):** It runs a comprehensive set of robustness and sensitivity checks to confirm the stability of the main findings.
6.  **Reporting (Tasks 12-14):** It synthesizes all quantitative outputs into a human-readable textual interpretation and a set of key visualizations.

## Core Components (Notebook Structure)

The `governments_reaction_public_debt_accumulation_draft.ipynb` notebook is structured as a logical pipeline with modular functions for each task:

-   **Task 1: `validate_inputs`**: The initial quality gate for all inputs.
-   **Task 2: `clean_and_prepare_data`**: Handles data quality issues.
-   **Task 3: `set_panel_structure`**: Prepares the DataFrame for panel analysis.
-   **Task 4: `generate_descriptive_statistics`**: Computes summary tables.
-   **Task 5: `run_diagnostic_tests`**: Performs key econometric tests.
-   **Task 6: `run_dccemg_estimation`**: The core estimation engine.
-   **Task 7: `calculate_long_run_effects`**: Computes the key sustainability metric.
-   **Task 8: `add_significance_indicators`**: Formats results for significance.
-   **Task 9: `compile_results_table`**: Generates the final publication-quality table.
-   **Task 10: `run_robustness_checks`**: Tests stability against specification changes.
-   **Task 11: `run_sensitivity_analysis`**: Tests stability against parameter changes.
-   **Task 12: `generate_results_interpretation`**: Creates a textual summary of findings.
-   **Task 13: `generate_visualizations`**: Creates plots of key results.
-   **Task 14: `create_fiscal_credibility_report`**: The top-level orchestrator that runs the entire workflow.

## Key Callable: create_fiscal_credibility_report

The central function in this project is `create_fiscal_credibility_report`. It orchestrates the entire analytical workflow from raw data to a final, comprehensive report object.

```python
def create_fiscal_credibility_report(
    raw_df: pd.DataFrame,
    analysis_parameters: Dict[str, Any],
    enable_intensive_visuals: bool = False
) -> Dict[str, Any]:
    """
    Executes the complete, end-to-end fiscal credibility analysis and compiles a master report.

    This grand orchestrator function serves as the single entry point to run the
    entire research project. It sequentially executes the baseline analysis,
    a series of robustness and sensitivity checks, and finally generates
    interpretive text and visualizations.

    Args:
        raw_df (pd.DataFrame): The raw input panel data.
        analysis_parameters (Dict[str, Any]): A comprehensive dictionary containing all
            parameters required for every stage of the analysis.
        enable_intensive_visuals (bool): Flag to enable computationally expensive
            visualizations. Defaults to False.

    Returns:
        Dict[str, Any]: A master dictionary containing the complete project results.
    """
    # ... (implementation is in the notebook)
```

## Prerequisites

-   Python 3.9+
-   Core dependencies: `pandas`, `numpy`, `statsmodels`, `linearmodels`, `scipy`, `matplotlib`, `seaborn`.

## Installation

1.  **Clone the repository:**
    ```sh
    git clone https://github.com/chirindaopensource/governments_reaction_public_debt_accumulation.git
    cd governments_reaction_public_debt_accumulation
    ```

2.  **Create and activate a virtual environment (recommended):**
    ```sh
    python -m venv venv
    source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
    ```

3.  **Install Python dependencies:**
    ```sh
    pip install pandas numpy statsmodels linearmodels scipy matplotlib seaborn
    ```

## Input Data Structure

The primary input is a `pandas.DataFrame` with a specific schema. See the example usage section for a detailed breakdown and a script to generate a structurally correct synthetic dataset. The key columns are:
-   `country_iso`, `year`
-   `s_it` (primary surplus/GDP), `b_it` (debt/GDP), `a_it` (current account/GDP)
-   `y_tilde_it` (output gap), `g_tilde_it` (spending gap)
-   `s_it_lag1`, `b_it_lag1`
-   `d_gfc_t`, `high_debt_indicator`, `industrial_indicator`
-   **For sensitivity analysis:** `y_real` (log real GDP), `g_real` (log real gov't spending)

## Usage

The `governments_reaction_public_debt_accumulation_draft.ipynb` notebook provides a complete, step-by-step guide. The core workflow is:

1.  **Prepare Inputs:** Load your raw data into a `pandas.DataFrame` and define the `analysis_parameters` dictionary.
2.  **Execute Pipeline:** Call the master orchestrator function:
    ```python
    master_report = create_fiscal_credibility_report(
        raw_df=your_dataframe,
        analysis_parameters=your_parameters
    )
    ```
3.  **Inspect Outputs:** Programmatically access any result from the returned `master_report` dictionary. For example, to view the main results table:
    ```python
    final_table = master_report['baseline_analysis']['final_formatted_table']
    print(final_table)
    ```

## Output Structure

The `create_fiscal_credibility_report` function returns a single, comprehensive dictionary with the following top-level keys:

-   `baseline_analysis`: Contains all artifacts from the main pipeline run (validation reports, analysis data, estimation results, final table).
-   `robustness_checks`: Contains the final results tables from the various robustness scenarios.
-   `sensitivity_analysis`: Contains the final results tables from the sensitivity scenarios.
-   `textual_interpretation`: A markdown string summarizing the key findings from the baseline analysis.
-   `visualizations`: A dictionary of `matplotlib` Figure objects for the key plots.

## Project Structure

```
governments_reaction_public_debt_accumulation/
│
├── governments_reaction_public_debt_accumulation_draft.ipynb  # Main implementation notebook
├── requirements.txt                                             # Python package dependencies
├── LICENSE                                                      # MIT license file
└── README.md                                                    # This documentation file
```

## Customization

The pipeline is highly customizable via the master `analysis_parameters` dictionary. Users can easily modify:
-   The `dcce_lags` for the main estimator.
-   The exact list of `regressors` for any of the 10 models.
-   The lists of countries in the `subsample_definitions`.

## Contributing

Contributions are welcome. Please fork the repository, create a feature branch, and submit a pull request with a clear description of your changes. Adherence to PEP 8, type hinting, and comprehensive docstrings is required.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## Citation

If you use this code or the methodology in your research, please cite the original paper:

```bibtex
@article{canofari2025do,
  title={Do Governments React to Public Debt Accumulation? A Cross-Country Analysis},
  author={Canofari, Paolo and Piergallini, Alessandro and Tedeschi, Marco},
  journal={arXiv preprint arXiv:2507.13084},
  year={2025}
}
```

For the implementation itself, you may cite this repository:
```
Chirinda, C. (2025). A Python Implementation of "Do Governments React to Public Debt Accumulation?". 
GitHub repository: https://github.com/chirindaopensource/governments_reaction_public_debt_accumulation
```

## Acknowledgments

-   Credit to Paolo Canofari, Alessandro Piergallini, and Marco Tedeschi for their rigorous and insightful research.
-   Thanks to the developers of the `pandas`, `statsmodels`, `linearmodels`, and other scientific Python libraries that make this work possible.

--

*This README was generated based on the structure and content of `governments_reaction_public_debt_accumulation_draft.ipynb` and follows best practices for research software documentation.*
