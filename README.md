# Container Chaos: Energy Production Forecasting Challenge

## Overview

This competition challenges participants to build a resilient machine learning system that forecasts energy production while handling data drift through continuous learning. You'll fetch data, train models, deploy containerized APIs, detect drift, and retrain models in a production-like environment.

## Problem Statement

Predict energy production output based on weather conditions, time factors, and operational parameters. As climate patterns shift and equipment ages, your model must adapt to distribution changes in the data.

## Folder Structure

**Note**: The folder structure provided below is a recommended optimal organization, but you are free to use any structure you prefer. The competition workflow and requirements remain the same regardless of how you organize your files.

## Competition Workflow

**Note**: The phases below simulate a CI/CD pipeline workflow for continuous model improvement and deployment.

### Phase 1: Data Acquisition & Initial Training

- Fetch train and test datasets from the provided endpoint (shared on competition day)
- Split your data using `training/split.py` (train/validation sets)
- Train your initial model with `training/train.py` (runs locally, not containerized)
- Save your trained model as `models/model_v1.pkl`

### Phase 2: Deployment & Inference

- Implement preprocessing in `backend/preprocess.py`
- Containerize your inference API using `docker/Dockerfile.backend`
- Deploy your service and make it publicly accessible
- Expose your prediction endpoint: `POST <your-website-url>/predict` (e.g., `https://your-domain.com/predict`)
- Your API will receive requests from the competition server and return predictions

### Phase 3: Drift Detection & Logging

- Implement drift detection in `backend/drift.py` to catch distribution shifts
- Log drifted inputs with features to `data/drift_log.csv`
- Track metrics using `backend/metrics.py` (Prometheus monitoring)

### Phase 4: Label Acquisition & Retraining

- Send drifted features to the label endpoint using `scripts/request_labels.py` (endpoint provided on competition day)
- Store labeled drift data in `data/labeled_drift.csv`
- Retrain your model with the new labeled data
- Save as `models/model_v2.pkl` (or increment version number)
- Redeploy your containerized backend with the improved model

### Phase 5: Accuracy Checking & Load Testing

- Submit your deployed application URL to competition hosts
- Hosts will evaluate model accuracy on hidden test data
- Your frontend will be stress-tested for performance and reliability
- Optional: Repeat phases 3-4 to improve before final evaluation

## Folder Structure

```
energy-production-forecast/
├── backend/
│   ├── app.py                # Flask/FastAPI inference service (containerized)
│   ├── preprocess.py         # Data preprocessing logic (YOU IMPLEMENT)
│   ├── drift.py              # Drift detection algorithm (YOU IMPLEMENT)
│   ├── metrics.py            # Prometheus metrics exporter
│
├── training/
│   ├── fetch_data.py         # Fetch train/test data from endpoint (YOU IMPLEMENT)
│   ├── split.py              # Train/validation splitting logic (YOU IMPLEMENT)
│   ├── train.py              # Model training script - LOCAL ONLY (YOU IMPLEMENT)
│
├── frontend/
│   ├── app.py                # Monitoring dashboard (use any frontend stack you prefer)
│
├── data/
│   ├── raw.csv               # Initial mixed train + test data
│   ├── drift_log.csv         # Unlabeled drifted inputs with features
│   ├── labeled_drift.csv     # Labeled drift data from server
│
├── models/
│   ├── model_v1.pkl          # Initial trained model
│   ├── model_v2.pkl          # Retrained model (post-drift)
│
├── scripts/
│   └── request_labels.py     # Send drift features to labeling endpoint
│
├── docker/
│   ├── Dockerfile.backend    # Backend API container definition
│   ├── Dockerfile.frontend   # Frontend dashboard container (optional)
│
├── docker-compose.yml        # Orchestrate all services
├── requirements.txt          # Python dependencies
└── README.md
```

## Output Feature

- Production

## Key Endpoints (Provided on Competition Day)

- **Data Fetch Endpoint**: `GET /data` - Returns train and test datasets
- **Label Request Endpoint**: `POST /labels` - Submit drifted features, receive ground truth labels

## Implementation Requirements

### Preferably Containerize

- Backend inference API (`backend/app.py`)
- All dependencies should be specified in `requirements.txt`
- Use provided Dockerfiles in `docker/` directory or create your own

### API Contract

Your backend must expose:

- `POST <your-website-url>/predict` - Accept input features, return predictions (e.g., `https://your-domain.com/predict`)

## Evaluation Criteria

1. **Model Performance**: Accuracy on test set and drifted data
2. **Stress Test**: API response time and reliability under load
3. **Phase Completion**: Judges will evaluate how many phases you've completed and how far you've progressed through the workflow

## Tips

- Consider time-series features and temporal patterns
- Handle weather data carefully (missing values, sensor errors)
- Monitor for seasonal changes and climate pattern shifts
- Account for equipment degradation over time
- Start simple with your drift detection algorithm (statistical tests, distribution comparison)
- Log comprehensive metrics for debugging
- Version your models clearly
- Test your containerized API locally before submission

## Rules

- Training scripts run **locally only** (not containerized)
- Inference API **must be containerized**
- You may use any ML framework (scikit-learn, PyTorch, TensorFlow, etc.)
- External data sources are not allowed
- **AI Code Editor Usage**: Using AI-powered code editors (e.g., GitHub Copilot, Cursor, etc.) will result in mark deductions from your final score
- **Monitoring**: Invigilators will continuously monitor your work throughout the competition
- **Point Deduction Notification**: Any point deductions will only be disclosed at the end of the session during the final presentation

## Support

For questions during the competition, contact the organizers through the official channel.

Good luck! 🚀
