# Formula 1 Project Azure Data Factory:

## Table of Contents

- [1) Introduction](#1-introduction)
- [2) Code Structure](#2-code-structure)
  
# 1) Introduction: 

This is the export resource from Azure Data Factory of michael-formula1-adf. The Pipeline was used mainly to trigger and run the notebooks in Azure Databricks. The main process included Test Connection with Azure Data Lake, pull data from api.jolpi.ca/ergast/f1, convert to csv format and process files before saving it in Delta Lake.

The reason that I used Azure Data Factory (ADF) instead of relying purely on in-notebook orchestration (dbutils.notebook.run chains) because ADF gives me production-grade pipeline management that Databricks notebooks alone don't provide: scheduled triggers so the pipeline runs automatically without manually opening and executing a notebook, built-in retry policies and failure handling per activity, centralized monitoring where I can see exactly which step (import, convert, or a specific process file) succeeded or failed and why and integration with Azure Monitor for email alerts on failure. None of which a bare notebook orchestrator provides out of the box. It also decouples orchestration from execution: ADF tells Databricks what to run and when, while Databricks focuses purely on the Spark computation, a cleaner separation of concerns than having one notebook responsible for both running its own logic and managing the control flow of every other notebook in the pipeline.

# 2) Code Structure:


```
.
├── factory/                                      # datafactory.
│   └── michael-formula1-adf.json    
├── linkedService/                                # link service to Azure Databricks.
│   └── michaelFormula1Databricks.json
├── pipeline/                                     # all processing pipeline.
│   └── pl_formula1_pipeline.json                 # my main formula 1 pipeline
└── trigger/                                      # trigger the pipeline.          
    └── tr_formula1_pl.json                        
```
