## Overview

### Relevant Source Files

- `main.py` (lines 6–9)  
- `requirements.txt` (line 45)  
- `data_model.py` (lines 1–13)

---

## Purpose and Scope

The **FDS (Frontier Data Science)** system is a production-ready **water potability prediction service**. It determines whether water samples are safe for human consumption based on physicochemical quality indicators.

The system provides a **REST API** built with FastAPI that:

- Accepts water quality parameters as input  
- Returns binary classification results using a pre-trained machine learning model  

This document offers a high-level overview of:

- System architecture  
- Core components  
- Data flow  

For more details, refer to:

- **API Reference** – for endpoint-level documentation  
- **Machine Learning Model** – for model internals and training logic  
- **Development Guide** – for local setup, testing, and deployment

---

## System Architecture

The FDS system is designed using a **layered architecture** that separates concerns across three main domains:

1. **API Layer**  
   Handles HTTP requests and responses using FastAPI.

2. **Data Validation Layer**  
   Uses Pydantic models to enforce schema validation for all incoming requests.

3. **Inference Layer**  
   Applies a pre-trained machine learning model to classify water safety.

This separation ensures a clean, maintainable codebase and enables independent upgrades to each component.

---

## Core Components Architecture

*Section pending detail. You can expand this with diagrams, flowcharts, or a breakdown of modules like `main.py`, `data_model.py`, and the model loading logic.*

<img width="905" height="820" alt="image" src="https://github.com/user-attachments/assets/5460153d-b35b-4af4-9add-97c559505de1" />
## Sources

- `main.py` (lines 1–44)  
- `data_model.py` (lines 4–13)

---

## Request Processing Data Flow

The system processes water quality predictions through a structured pipeline that:

1. **Validates Input Data**  
   Incoming JSON requests are validated using Pydantic models to ensure data integrity and schema compliance.

2. **Transforms Data for ML Inference**  
   Validated input is converted into a `pandas.DataFrame` format suitable for consumption by the machine learning model.

3. **Executes Model Prediction**  
   The pre-trained model performs binary classification on the input features to predict potability.

4. **Formats the Response**  
   The raw model output is converted into a human-readable string indicating whether the water is *potable* or *not potable*.

---

## Prediction Pipeline Flow

*Section pending detail. Add a flow diagram, numbered steps, or pseudocode to illustrate how data flows from API input to final response.*

<img width="1705" height="140" alt="image" src="https://github.com/user-attachments/assets/c5425d4a-6abe-487a-9c34-16f3da25d57c" />
## Sources

- `main.py` (lines 6–12, 14–17, 19–43, 35)  
- `data_model.py` (lines 4–13)  
- `requirements.txt` (lines 1–158, 45, 145)

---

## Key Components

### FastAPI Application

The main application is defined in:

- `main.py` (lines 6–9)  
  A `FastAPI` instance is created with descriptive metadata (title, description).

The application exposes two endpoints:

- **Root Endpoint** (`@app.get("/")`)  
  - Location: `main.py` (lines 14–17)  
  - Function: Returns a welcome message.

- **Prediction Endpoint** (`@app.post("/predict")`)  
  - Location: `main.py` (lines 19–43)  
  - Function: Accepts `Water` objects and returns binary potability predictions.

---

### Water Data Model

The `Water` class in `data_model.py` (lines 4–13) extends `Pydantic`'s `BaseModel` and defines the schema for incoming water quality data.

| Parameter           | Type   | Description                        |
|---------------------|--------|------------------------------------|
| `ph`                | float  | pH level of water                  |
| `Hardness`          | float  | Water hardness measurement         |
| `Solids`            | float  | Total dissolved solids             |
| `Chloramines`       | float  | Chloramine content                 |
| `Sulfate`           | float  | Sulfate concentration              |
| `Conductivity`      | float  | Electrical conductivity            |
| `Organic_carbon`    | float  | Total organic carbon               |
| `Trihalomethanes`   | float  | Trihalomethane levels              |
| `Turbidity`         | float  | Water turbidity measurement        |

---

### Machine Learning Model

- The model is **loaded using `pickle.load()`** at application startup.  
  - Location: `main.py` (lines 11–12)  
- It expects input in the form of a `pandas.DataFrame` containing the nine water quality parameters.
- The model performs **binary classification**:
  - `0` = Non-potable  
  - `1` = Potable

---

## Extended Ecosystem

The system leverages a wide range of dependencies to support both core functionality and advanced data science workflows.

### Core Dependencies

- `fastapi`: Web framework for defining API routes  
- `pydantic`: Data validation and serialization  
- `pandas`: Data manipulation and DataFrame operations  
- `pickle`: Model serialization/loading  
- `uvicorn`: ASGI server for FastAPI deployment

### Extended Capabilities

- **Asynchronous Processing:** `celery`, `aiohttp` for background tasks  
- **Data Version Control:** `dvc`, `dvc-data` for managing datasets and models  
- **Advanced ML Libraries:** `scikit-learn`, `xgboost`, `lightgbm`, `AutoTS`  
- **Interactive Dashboards:** `streamlit`, `altair` for visualizing model results and data  
- **Database Connectivity:** `mysql-connector`, `mysqlclient` for persistence and querying  
- **Data Profiling:** `pandas-profiling`, `ydata-profiling` for data quality analysis

---

## System Initialization

The application follows a straightforward initialization pattern:

1. **Model Loading**  
   - The pre-trained model is loaded from `model.pkl` using `pickle.load()`  
   - Source: `main.py` (lines 11–12)

2. **FastAPI Instance**  
   - Application created with metadata (title, description)  
   - Source: `main.py` (lines 6–9)

3. **Endpoint Registration**  
   - Root and prediction endpoints registered using FastAPI decorators  
   - Source: `main.py` (lines 14–17, 19–43)

4. **Dependency Injection**  
   - Request validation is automatically handled by the `Water` Pydantic model  
   - Source: `data_model.py` (lines 4–13)

The system is designed for immediate deployment using `uvicorn` and can be easily extended through its dependency ecosystem.
