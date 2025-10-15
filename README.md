# WHOOP Data Platform

Production-grade health analytics platform demonstrating enterprise data engineering capabilities.

## Overview
Automated ETL pipeline that ingests WHOOP biometric data, transforms it through a medallion architecture (Bronze → Silver → Gold), and delivers actionable insights via interactive dashboards.

## Tech Stack
- **Cloud Platform:** Azure (Functions, Data Lake, Data Factory, Key Vault)
- **Data Warehouse:** Snowflake
- **Transformation:** dbt, SQL, Python
- **Orchestration:** Azure Data Factory, Snowflake Tasks
- **Analytics:** Power BI, Snowsight
- **ML/AI:** Python (scikit-learn, Prophet), Snowpark
- **DevOps:** Git, Azure DevOps, Terraform

## Project Status
🚧 In Development

## Architecture
[Architecture diagram coming soon]

## Setup Instructions
[Documentation coming soon]

## Project Structure
```
whoop-data-platform/
├── docs/              # Documentation
├── infrastructure/    # Terraform IaC
├── ingestion/        # Azure Functions for data extraction
├── dbt_project/      # dbt transformations
├── ml_models/        # Machine learning models
├── powerbi/          # Power BI dashboards
└── snowflake/        # Snowflake scripts
```

## License
MIT License
```

**.gitignore:**
```
# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
env/
venv/
ENV/
build/
dist/
*.egg-info/

# Azure Functions
.vscode/
local.settings.json
.python_packages/
.funcignore

# dbt
dbt_project/target/
dbt_project/dbt_packages/
dbt_project/logs/

# Jupyter
.ipynb_checkpoints/
*.ipynb

# Environment variables
.env
*.env

# Secrets
*.pem
*.key
secrets/
credentials/

# IDE
.idea/
*.swp
*.swo
.DS_Store

# Terraform
*.tfstate
*.tfstate.*
.terraform/
*.tfvars

# Power BI
*.pbix

# Snowflake
*_private_key.p8
