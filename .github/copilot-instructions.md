# Claude Code Workspace Guidelines

## Overview
This repository contains course materials teaching Claude Code best practices through practical examples, with the primary implementation being a professional e-commerce analytics dashboard (Lesson 7) built from a Jupyter notebook.

## Code Style

- **Language**: Python 3.8+
- **Imports**: Organize as stdlib → third-party → local imports; use conditional imports for optional dependencies
- **Type Hints**: Always use `typing` module annotations (e.g., `Dict[str, pd.DataFrame]`, `Optional[int]`)
- **Docstrings**: Use structured format with `Args`, `Returns`, `Raises` sections
- **Class Design**: Use classes for data processing (e.g., `EcommerceDataLoader`, `BusinessMetricsCalculator`)
- **Error Handling**: Include try/except blocks for file I/O; print informative messages on failures
- **Data Validation**: Implement `_validate_data()` or similar in `__init__` to verify required columns and data structure

## Architecture

**modular design** with clear separation of concerns:

- **data_loader.py**: Handles CSV ingestion and data merging (`load_raw_data()`, `process_data()`)
- **business_metrics.py**: Calculates KPIs and generates visualizations using Plotly/Matplotlib
- **dashboard.py**: Streamlit application that orchestrates data loading and metrics display
- **Jupyter Notebooks**: Exploratory analysis (EDA.ipynb) with production refactor (EDA_Refactored.ipynb)

**Key Patterns**:
1. Data flows: CSV → DataLoader → processed DataFrames → Metrics calculation → Visualization
2. Metrics classes take processed DataFrames in `__init__`, expose calculation methods
3. Visualizations return Plotly figures for dashboard reusability
4. Dashboard uses Streamlit layout controls (`st.columns()`, `st.metric()`) with custom CSS

## Build and Test

**Installation**:
```bash
pip install -r lesson7_files/requirements.txt
```

**Run Applications**:
```bash
# Streamlit Dashboard
cd lesson7_files
streamlit run dashboard.py

# Jupyter Notebook Analysis
jupyter notebook lesson7_files/EDA_Refactored.ipynb
```

**Data Setup**: Ensure CSV files exist in `lesson7_files/ecommerce_data/` (6 datasets: orders, order_items, products, customers, reviews, payments)

## Project Conventions

- **DataFrame Processing**: Use copy() to avoid SettingWithCopyWarning; preserve date columns as datetime64
- **Metric Calculations**: Accept `current_year` and optional `previous_year` for year-over-year analysis
- **Visualization Defaults**: Use consistent color schemes (e.g., Plotly template); include proper axis labels and legends
- **Configuration**: Parameters expose time periods and filters without requiring code changes (see dashboard filters)
- **Warnings**: Suppress non-critical warnings with `warnings.filterwarnings('ignore')` but log data quality issues

## Integration Points

- **Data Source**: CSV files in `ecommerce_data/` directory (e-commerce dataset with 7 entities)
- **Output Formats**: Plotly figures (interactive), Matplotlib plots (static), Streamlit renders directly
- **Cross-Module Communication**: Dashboard imports both DataLoader and BusinessMetricsCalculator; avoid circular imports

## Testing Strategy

- Validate data loading completes without missing files
- Verify metric calculations produce consistent results across time periods
- Check Streamlit page renders without errors when data is present
- Ensure Jupyter notebook cells execute sequentially without dependency issues

## Key Files to Reference

- [lesson7_files/data_loader.py](lesson7_files/data_loader.py) - Modeling class structure and type hints
- [lesson7_files/business_metrics.py](lesson7_files/business_metrics.py) - Visualization and metric patterns
- [lesson7_files/dashboard.py](lesson7_files/dashboard.py) - Streamlit layout best practices
- [lesson7_files/README.md](lesson7_files/README.md) - Feature specifications and usage guide
