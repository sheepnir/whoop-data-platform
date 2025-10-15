# Project Context: WHOOP Data Platform

## Project Overview
I'm building a production-grade, end-to-end data analytics platform that ingests personal health data from the WHOOP API, transforms it through a medallion architecture, and delivers actionable insights via dashboards and predictive models. This is a portfolio project to demonstrate enterprise data engineering, governance, and analytics capabilities for a VP of Enterprise Data role.

## Primary Goal
Create an automated data pipeline showcasing modern data stack technologies (Azure, Snowflake, dbt, Power BI) with enterprise-grade practices including data governance, master data management, CI/CD, and machine learning.

## Technical Stack

### Cloud & Infrastructure
- **Azure Services:**
  - Azure Functions (serverless data extraction)
  - Azure Data Lake Storage Gen2 (raw data storage)
  - Azure Data Factory (orchestration)
  - Azure Key Vault (secrets management)
  - Azure Databricks (optional for advanced ML)
  - Azure DevOps (CI/CD)

### Data Platform
- **Snowflake:** Cloud data warehouse
  - Bronze layer: Raw JSON data
  - Silver layer: Cleansed, typed data
  - Gold layer: Business-ready dimensional model
  - Snowflake-specific tools: Snowsight, Snowpipe, Tasks, Streams, Snowpark

### Development Tools
- **Cursor IDE:** Primary development environment
- **Git/GitHub:** Version control (repo: whoop-data-platform)
- **dbt:** SQL-based transformations with testing
- **Python:** Data engineering, ML, automation
- **SQL:** Core transformation logic

### Analytics & Visualization
- **Power BI:** Primary dashboarding tool
- **Snowsight:** Quick data exploration
- **Python Libraries:** pandas, scikit-learn, Prophet for ML

## Data Source: WHOOP API v2
The WHOOP API provides biometric data including:
- **Sleep Metrics:** stages (light, deep, REM, awake), duration, efficiency, respiratory rate, sleep performance score
- **Recovery Metrics:** recovery score (0-100%), HRV (heart rate variability in ms), resting heart rate (bpm), SpO2
- **Strain Metrics:** daily strain score (0-21), workout strain, calories burned
- **Workout Details:** activity type, duration, average/max heart rate, heart rate zones, GPS data (if available)

API Authentication: OAuth 2.0 with access/refresh tokens

## Architecture: Medallion Design Pattern

### Bronze Layer (Raw)
- Raw JSON data from WHOOP API stored as-is
- Stored in both Azure Data Lake and Snowflake
- No transformations, just ingestion
- Includes audit columns (load_timestamp, source_file)

### Silver Layer (Cleansed)
- Parsed JSON into structured tables
- Data type conversions and standardization
- Deduplication logic
- Data quality checks (nulls, ranges, referential integrity)
- Clean column names and consistent formatting

### Gold Layer (Business-Ready)
- Dimensional model (star schema)
- **Fact Tables:**
  - fact_daily_recovery
  - fact_sleep
  - fact_strain
  - fact_workouts
- **Dimension Tables:**
  - dim_date
  - dim_activity_type
  - dim_sleep_stage
- Aggregate tables for performance
- Business logic and calculated metrics

## Data Model (Gold Layer)

### fact_daily_recovery
- recovery_id (PK)
- date_key (FK to dim_date)
- recovery_score (0-100)
- hrv_rmssd_ms (heart rate variability)
- resting_heart_rate_bpm
- spo2_percentage
- skin_temp_celsius
- cycle_id
- load_timestamp

### fact_sleep
- sleep_id (PK)
- date_key (FK to dim_date)
- sleep_start_timestamp
- sleep_end_timestamp
- total_sleep_minutes
- light_sleep_minutes
- deep_sleep_minutes
- rem_sleep_minutes
- awake_minutes
- sleep_efficiency_pct
- sleep_performance_pct
- respiratory_rate
- sleep_needed_minutes
- sleep_debt_minutes
- load_timestamp

### fact_strain
- strain_id (PK)
- date_key (FK to dim_date)
- day_strain_score (0-21)
- calories_burned
- avg_heart_rate_bpm
- max_heart_rate_bpm
- load_timestamp

### fact_workouts
- workout_id (PK)
- date_key (FK to dim_date)
- activity_type_key (FK to dim_activity_type)
- workout_start_timestamp
- workout_end_timestamp
- duration_minutes
- strain_score
- avg_heart_rate_bpm
- max_heart_rate_bpm
- calories_burned
- distance_meters (if available)
- altitude_gain_meters (if available)
- zone_duration_minutes (array for 5 HR zones)
- load_timestamp

### dim_date
- date_key (PK)
- full_date
- year, quarter, month, day
- day_of_week, day_name
- week_of_year
- is_weekend
- is_holiday (optional)

### dim_activity_type
- activity_type_key (PK)
- activity_type_id (from WHOOP)
- activity_name
- activity_category

### dim_sleep_stage
- sleep_stage_key (PK)
- sleep_stage_name (Light, Deep, REM, Awake)
- sleep_stage_description

## Machine Learning Components

### Predictive Models to Build:
1. **Recovery Score Prediction:** Predict tomorrow's recovery based on today's sleep quality, strain, and historical patterns (regression model)
2. **Sleep Quality Forecasting:** Predict sleep performance based on bedtime, previous day strain, and trends (time-series)
3. **Anomaly Detection:** Identify unusual patterns in HRV, resting heart rate, or sleep metrics (isolation forest or z-score based)
4. **Correlation Analysis:** Find relationships between strain, sleep quality, recovery, and external factors

### ML Pipeline:
- Feature engineering in Python/Snowpark
- Model training with scikit-learn or Prophet
- Model evaluation and versioning
- Store predictions back to Snowflake (fact_predictions table)
- Retrain models periodically with new data

## Project Modules (Development Phases)

### Module 1: Foundation & Environment Setup
- Azure account and resource group
- Snowflake trial account
- WHOOP Developer API access
- Cursor IDE configuration
- Git repository structure
- Architecture diagram
- Dimensional model design

### Module 2: Manual Data Exploration
- Test WHOOP API authentication
- Manual data export and analysis
- Load sample data into Snowflake
- Explore data quality
- Document data dictionary
- Create staging tables

### Module 3: Bronze Layer - Automated Ingestion
- Azure Function for WHOOP API extraction
- OAuth token management with refresh
- Azure Data Lake Storage setup
- Daily automated triggers
- Error handling and logging
- Raw JSON storage in Snowflake

### Module 4: Silver Layer - Data Cleansing
- dbt project initialization
- JSON parsing with LATERAL FLATTEN
- Data type conversions
- Deduplication logic
- dbt tests for data quality
- Data lineage documentation

### Module 5: Gold Layer - Dimensional Model
- Implement star schema in Snowflake
- Create fact and dimension tables
- Build dbt models for Gold layer
- Aggregate tables
- Business logic and calculated metrics
- Comprehensive testing

### Module 6: Orchestration & Automation
- Azure Data Factory pipelines
- End-to-end orchestration
- Snowflake Tasks as alternative
- Data quality gates
- Monitoring and alerting
- Pipeline documentation

### Module 7: Analytics & Machine Learning
- Feature engineering pipeline
- Train predictive models
- Model evaluation
- Store predictions in Snowflake
- Anomaly detection logic
- Correlation analysis

### Module 8: Visualization & Dashboards
- Power BI connection to Snowflake
- Build semantic model
- Create 5 interactive dashboards:
  1. Recovery Overview
  2. Sleep Analysis
  3. Strain & Performance
  4. Predictive Insights
  5. Data Quality Monitoring
- Snowsight dashboards for exploration

### Module 9: DevOps & Production Readiness
- CI/CD pipeline setup
- Automated testing (unit, integration, dbt)
- Infrastructure as Code (Terraform)
- Comprehensive documentation
- Data catalog with metadata
- Cost optimization
- Monitoring dashboards

### Module 10: Advanced Features (Optional)
- Real-time streaming with Snowpipe
- Advanced ML models
- Mobile habit tracking integration
- Data sharing capabilities
- Performance tuning

## Key Learning Objectives

### Technical Skills to Demonstrate:
- Cloud data engineering (Azure ecosystem)
- Modern data warehouse design (Snowflake)
- ETL/ELT pipeline development
- SQL and Python proficiency
- Data modeling (dimensional design)
- Data governance and quality frameworks
- Master data management principles
- Machine learning for time-series and predictions
- Analytics and visualization
- DevOps practices (Git, CI/CD, IaC)
- Cost optimization and performance tuning

### Snowflake-Specific Skills:
- Snowsight interface and Worksheets
- Semi-structured data handling (VARIANT, FLATTEN)
- Snowpipe for continuous ingestion
- Tasks and Streams for orchestration
- Time Travel and Zero-Copy Cloning
- Snowpark for Python processing
- Query optimization and performance
- Role-based access control (RBAC)
- Resource monitors and cost management

## Success Criteria

### Technical Metrics:
- Pipeline reliability: 95%+ success rate
- Data latency: < 12 hours from WHOOP sync
- Data quality: Zero issues in Gold layer
- Query performance: < 3 seconds for dashboards
- Code coverage: 100% for critical functions

### Business Value:
- Accurate recovery predictions (MAE < 10%)
- Actionable health insights
- Self-service analytics capability

### Portfolio Quality:
- Professional documentation
- Clean, well-commented code
- Demonstrates job requirements
- Deployable via IaC

## Repository Structure
```
whoop-data-platform/
├── README.md
├── docs/
│   ├── architecture.md
│   ├── data_model.md
│   ├── setup_guide.md
│   └── snowflake_learnings.md
├── infrastructure/
│   ├── terraform/
│   │   ├── azure/
│   │   └── snowflake/
│   └── azure-pipelines.yml
├── ingestion/
│   ├── azure_functions/
│   │   ├── whoop_extractor/
│   │   └── requirements.txt
│   └── tests/
├── dbt_project/
│   ├── models/
│   │   ├── bronze/
│   │   ├── silver/
│   │   └── gold/
│   ├── tests/
│   ├── macros/
│   └── dbt_project.yml
├── ml_models/
│   ├── notebooks/
│   ├── src/
│   └── snowpark/
├── powerbi/
│   ├── dashboards/
│   └── semantic_model/
├── snowflake/
│   ├── setup_scripts/
│   ├── stored_procedures/
│   └── tasks/
└── .github/
    └── workflows/
```

## Budget Considerations
- Azure: ~$20-50/month
- Snowflake: Free trial (30 days), then ~$25-40/month
- Power BI: Desktop (free) or Pro ($10/month)
- Cursor IDE: Free or Pro ($20/month)
- **Total: ~$50-120/month after trials**

## Target Job Requirements Addressed
This project directly demonstrates:
✅ Modern data platforms (Snowflake, Azure)
✅ Cloud infrastructure and scalable architectures
✅ Data engineering (Python, SQL, Spark concepts)
✅ Master data management and governance
✅ Predictive models and optimization tools
✅ Production-grade data products
✅ CI/CD pipelines
✅ Self-service analytics tools
✅ Data lakes and real-time processing
✅ Leadership through architecture and documentation

## How to Help Me

### I Need Help With:
1. **Code Development:** Write Python scripts, SQL queries, dbt models, Terraform configs
2. **Architecture Decisions:** Best practices for data modeling, pipeline design, tool selection
3. **Troubleshooting:** Debug errors in Azure Functions, Snowflake queries, dbt runs, API calls
4. **Documentation:** Create clear README files, architecture diagrams, setup guides
5. **Optimization:** Performance tuning for queries, cost optimization strategies
6. **Best Practices:** Enterprise-grade patterns for error handling, logging, testing, security
7. **Snowflake Expertise:** Teach me Snowflake features and optimization techniques
8. **Learning:** Explain concepts, provide examples, suggest resources

### Communication Preferences:
- Provide clear explanations with examples
- Show me best practices and why they matter
- Include code comments explaining logic
- Suggest alternative approaches when applicable
- Point out potential issues or improvements
- Help me understand trade-offs in decisions
- Teach me Snowflake and Azure features as we go

### Code Style Preferences:
- Clean, readable, well-commented code
- Follow PEP 8 for Python
- Use meaningful variable names
- Include docstrings for functions
- Error handling with specific exceptions
- Logging for debugging and monitoring
- Type hints where appropriate

## Current Status
Just initialized the GitHub repository. Ready to start Module 1: Foundation & Environment Setup.

## Questions to Consider
- Should I use Snowflake Tasks or Azure Data Factory for orchestration? (or both?)
- What's the best way to handle OAuth token refresh in Azure Functions?
- How should I structure my dbt project for scalability?
- What clustering keys should I use in Snowflake for optimal performance?
- How do I implement incremental models in dbt for large fact tables?
- What's the best way to version and deploy ML models?
- Should I use Snowpark or external Python for ML processing?
