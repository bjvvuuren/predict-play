# predict-play

Predictive Relational Evaluation of Demand for In-licensing Content Titles

Student Name: Brandon Janse Van Vuuren

Student Number: 26304758

## Business Motivation (Part A)

StadioChoice (SC) is navigating an ever changing entertainment industry. With the influx of international competitors like Netflix, Disney+ and Prime Video SC has seen a steady decline in their traditional satellite television customer base. They have seen growth in their streaming service but the service is currently making a loss. 

The problem I have identified is to approach the loss from a cost cutting strategy. The briefing packet highlights that content an sports rights is costing the business R34 billion annually, however a large portion of this investment goes unwatched and content renewal decision are made based on intuition instead of data driven decision making. 

This project aims to address the gap in the content buying. It focuses on one key strategic pillar: spend on content that earns its keep. The project aims to build a model that utilises internal data like viewing logs, search query demand, external IMDb metadata and catalogue data that will help the business understand the likelihood that a specific title will be watched in the current licensing year. 

PREDICT-Play will enable acquisition teams to make data-driven decision about content which will eliminate wasteful expenditure and become the backbone of the "Spend on content that earns its keep" strategic goal.

It’s important to note that there is a heavy skew towards sport related content and that the content we have viewing data on is only based on streaming content. The recommendation here is to identify non-sports content and optimise that side of the content buying first. I also recommend the deployment of a recommendation system either in tandem or shortly after. 

## Problem Statement (Part B)


StadioChoice is facing growth challenges with a flat year on year revenue due to a declining satellite subscriber base and the current operating loss in the steaming service.The current R34 billion expenditure on content acquisition at StadioChoice, which is largely spent on unwatched content, is creating wasteful expenditure. 

This wasteful expenditure is further exacerbated by licensing decisions that are not rooted in data. 

Establishing a method to optimise content acquisition will both cut unnecessary expenditure and tackle monthly subscriber churn as users find more meaningful content that they are likely to watch. 

This project aims to build a machine-learning tool that will overlay a predicted likelihood to watch a specific title in the acquisition catalogue.

## Repository Structure (Part D)

```text
predict-play/
├── docs/
│   ├── Data_Request.pdf
│   └── guides/
├── modelling/
│   ├── datasets/
│   ├── evaluation/
│   │   ├── experimental_setup.ipynb
│   │   ├── experimental_results.ipynb
│   │   └── statistical_helpers.ipynb
│   ├── models/
│   └── tuning/
│       └── visualisation_notebook.ipynb
├── predict-play/
│   ├── __init__.py
│   └── inference.py
├── .gitignore
└── README.md

```
### Artifact Mapping & Folder Descriptions

* **`docs/`**: Houses all project documentation, operational guides, and the data request documentation.
* **`modelling/`**: The core research, experimentation, and machine learning workspace:
  * **`datasets/`**: Accommodates **Datasets** (raw streaming logs, processed search queries, and warehoused external tmdb metadata).
  * **`models/`**: Accommodates **Models**.
  * **`evaluation/`**: Accommodates testing, validation, and benchmarking:
    * **Experimental setup**: Experiment configurations, train/validation split definitions, and testing scipts.
    * **Experimental results**: Logged performance metrics, evaluation run outputs, and comparative benchmark data.
    * **Statistical helper and comparison scripts**: Utility functions, hypothesis testing routines, and statistical evaluation scripts comparing model performance.
  * **`tuning/`**: Accommodates hyper-parameter optimization workflows:
    * **Visualisation scripts**: Diagnostic plotting, EDA generation and relational graph visualization scripts.
* **`predict-play/`**: Houses the deployable inference solution, service application code, and production pipelines.


## RAAIDD Log (Part E)

| RAAIDD | Description |
| :--- | :--- |
| RISKS | There are three  major risks I have identified. Firstly is an issue with data matching. The unavailability of accurate external TMDB metadata matching internal STADIOchoice title naming conventions poses a risk to getting related titles. The second is that even with aggregated events, training for this type of nonlinear data on 3 years of data may cause memory issues. Three NLP data is notorious for bad data quality for instance variance or slang in search queries may bypass a standard spelling correction pipeline.|
| ACTIONS | We will take the following actions: implement a robust spelling correcting on the search queries before matching them to the TMDB data, construct a relational network that maps watches, corrected searched and metadata. Train and compare two methods (R-GCN and Gradient Boosting) to evaluate the best trade-off between accuracy and compute costs.  |
| ASSUMPTIONS | Here are the assumptions we are making: The TMDB data is accurate and relevant to the markets SC serves. The portion of unwatched content is due to a lack of interest not platform problems. Viewers use the search bar enough for us to estimate demand from that.  |
| ISSUES | There are potential rate limits and API key acquisition works with TMDB |
| DECISIONS | We will aggregate the event streaming by title and day. The way keeping it efficient while still having a point in time reference for the activity. We will evaluate a neural network (R-GCN) as well as decision tree (XGBoost) and compare them |
| DEPENDENCIES | We will need access to the data per the data request document for the time periods specified therein.  Matching search queries to teh TMDB database will rely heavily on the accuracy of the spelling corrections.|

## Modelling Information. 
1. [Preprocessing](https://github.com/bjvvuuren/predict-play/blob/main/docs/preprocessing.MD)
2. [Feature Engineering](https://github.com/bjvvuuren/predict-play/blob/main/modelling/datasets/feature-engineering/FeatureEngineering.MD)

## Modelling Notebooks
1. [01_preprocessing.ipynb](https://github.com/bjvvuuren/predict-play/blob/main/modelling/datasets/preprocessed_data/01_preprocessing.ipynb)
2. [02_feature_engineering.ipynb](https://github.com/bjvvuuren/predict-play/blob/main/modelling/datasets/feature-engineering/02_feature_engineering.ipynb)
