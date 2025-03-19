# Income Prediction API

This repository contains a Flask API that serves a machine learning model predicting whether an individual's income exceeds $50K/year based on census data attributes.

## Overview

This project implements a production-ready API for an income prediction model with the following features:

- RESTful API endpoints for making predictions and updating data
- Persistent storage in SQL database (PostgreSQL in production, SQLite for local development)
- Automatic database creation and schema migration
- Docker containerization for consistent deployment
- Ready for deployment on Railway

## Tech Stack

- **Framework**: Flask
- **Database ORM**: Peewee
- **Machine Learning**: Scikit-learn (pipeline with joblib serialization)
- **Data Processing**: Pandas
- **Containerization**: Docker
- **Deployment**: Railway

## API Endpoints

### Make a Prediction

```
POST /predict
```

Request body:
```json
{
  "observation_id": "unique-id-for-observation",
  "data": {
    "age": 39,
    "sex": "male",
    "race": "White",
    "workclass": "State-gov",
    "education": "Bachelors",
    "marital-status": "Never-married",
    "capital-gain": 0,
    "capital-loss": 0,
    "hours-per-week": 40
  }
}
```

Response:
```json
{
  "observation_id": "unique-id-for-observation",
  "prediction": true,
  "probability": 0.75
}
```

Error response:
```json
{
  "observation_id": "unique-id-for-observation",
  "error": "Error message detailing what went wrong"
}
```

### List All Predictions

```
GET /list-db-contents
```

Returns a list of all predictions stored in the database.

## Local Development

### Prerequisites

- Python 3.8+
- pip

### Setup

1. Clone the repository
```bash
git clone <repository-url>
cd <repository-directory>
```

2. Install dependencies
```bash
pip install -r requirements.txt
```

3. Run the application
```bash
python app.py
```

The API will be available at http://localhost:5000

### Running Tests

```bash
pytest
```

## Deployment on Railway

### Prerequisites

- Railway account
- Railway CLI (optional)

### Deployment Steps

1. Create a new project on Railway
2. Connect your GitHub repository
3. Add a PostgreSQL database service
4. Set the `DATABASE_URL` environment variable to the PostgreSQL connection string
5. Deploy the application

Railway will automatically build and deploy your application.

## Project Structure

```
.
├── app.py                # Main application file
├── columns.json          # Feature columns for the model
├── dtypes.pickle         # Data types for model input
├── pipeline.pickle       # Trained ML pipeline
├── Dockerfile            # Docker configuration
├── requirements.txt      # Python dependencies
└── README.md             # This file
```

## Feature Constraints

The API validates input data against these constraints:

- **age**: 0-120 years
- **sex**: "male" or "female"
- **race**: "White", "Black", "Asian-Pac-Islander", "Amer-Indian-Eskimo", or "Other"
- **capital-gain**: Non-negative value
- **capital-loss**: Non-negative value
- **hours-per-week**: 0-168 hours
