# 🏨 Hotel Reservation Cancellation Predictor

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=for-the-badge&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-3.0.0-000000?style=for-the-badge&logo=flask&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.3.2-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)
![LightGBM](https://img.shields.io/badge/LightGBM-4.1.0-00B300?style=for-the-badge)
![MLflow](https://img.shields.io/badge/MLflow-2.9.2-0194E2?style=for-the-badge&logo=mlflow&logoColor=white)
![GCP](https://img.shields.io/badge/Google%20Cloud-Cloud%20Run-4285F4?style=for-the-badge&logo=googlecloud&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Container-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-D24939?style=for-the-badge&logo=jenkins&logoColor=white)

---

## 📌 Overview

**Hotel Reservation Cancellation Predictor** is a production-grade Machine Learning project that predicts whether a hotel reservation is likely to be cancelled. Built with a modular ML pipeline, the application is served via a **Flask** web interface, containerized with **Docker**, and deployed on **Google Cloud Run** through a fully automated **Jenkins CI/CD** pipeline. The trained model is stored and loaded from **Google Cloud Storage (GCS)**, enabling seamless cloud-native model versioning and serving.

---

## ✨ Features

- 🤖 **ML-Powered Predictions** — Uses LightGBM to classify hotel reservations as cancelled or not
- 🌐 **Flask Web App** — User-friendly form interface to input booking details and get instant predictions
- ☁️ **GCS Model Storage** — Model artifacts stored and loaded directly from Google Cloud Storage
- 🐳 **Dockerized** — Fully containerized for consistent, portable deployments
- 🔁 **Jenkins CI/CD Pipeline** — Automated 3-stage pipeline: Clone → Build & Push → Deploy to Cloud Run
- 📊 **MLflow Experiment Tracking** — Tracks training runs, metrics, and model versions
- ⚖️ **Imbalanced Data Handling** — Uses `imbalanced-learn` to address class imbalance in reservation data
- 📓 **EDA Notebooks** — Exploratory data analysis available in `notebook/`

---

## 🗂️ Project Structure

```
Hotel_reservation_using-GCP-jenkins/
├── artifacts/                  # Saved model artifacts and pipeline outputs
├── config/
│   └── paths_config.py         # GCS bucket path and model output path config
├── custom_jenkins/             # Custom Jenkins configuration files
├── notebook/                   # Jupyter notebooks for EDA and experimentation
├── pipeline/                   # ML training pipeline scripts
├── src/
│   └── model_loader.py         # Logic to load model from GCS or local path
├── static/                     # Static assets (CSS, JS) for Flask UI
├── templates/
│   └── index.html              # Jinja2 HTML template for prediction UI
├── utils/                      # Utility/helper functions
├── application.py              # Flask app entry point
├── Dockerfile                  # Docker image definition
├── Jenkinsfile                 # Jenkins declarative pipeline (3 stages)
├── requirements.txt            # Python dependencies
└── setup.py                    # Package setup
```

---

## 🧰 Tech Stack

| Category | Technology |
|---|---|
| Language | Python 3.8+ |
| ML Model | LightGBM 4.1.0 |
| ML Framework | scikit-learn 1.3.2 |
| Imbalance Handling | imbalanced-learn 0.11.0 |
| Web Framework | Flask 3.0.0 |
| Experiment Tracking | MLflow 2.9.2 |
| Cloud Platform | Google Cloud Platform (GCP) |
| Model Storage | Google Cloud Storage (GCS) |
| Deployment | Google Cloud Run |
| Containerization | Docker |
| CI/CD | Jenkins |
| Serialization | Joblib 1.3.2 |

---

## 🔮 Prediction Features

The model takes the following 10 input features to make a prediction:

| Feature | Description |
|---|---|
| `lead_time` | Days between booking and arrival |
| `no_of_special_requests` | Number of special requests made |
| `avg_price_per_room` | Average price per room (in USD) |
| `arrival_month` | Month of arrival (1–12) |
| `arrival_date` | Date of arrival (1–31) |
| `market_segment_type` | Market segment (encoded as integer) |
| `no_of_week_nights` | Number of weeknights booked |
| `no_of_weekend_nights` | Number of weekend nights booked |
| `type_of_meal_plan` | Meal plan type (encoded as integer) |
| `room_type_reserved` | Reserved room type (encoded as integer) |

**Output:** `1` = Reservation likely to be **cancelled** | `0` = Reservation likely to be **kept**

---

## ⚙️ Installation & Setup

### 1. Clone the Repository

```bash
git clone https://github.com/PremnathAnbu/Hotel_reservation_using-GCP-jenkins.git
cd Hotel_reservation_using-GCP-jenkins
```

### 2. Create a Virtual Environment

```bash
python -m venv venv
source venv/bin/activate       # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
pip install -e .
```

### 4. Configure GCS Path

Update `config/paths_config.py` with your GCS bucket path:

```python
MODEL_OUTPUT_PATH = "gs://your-bucket-name/path/to/model.pkl"
# Or for local testing:
# MODEL_OUTPUT_PATH = "artifacts/model.pkl"
```

Set up GCP credentials:

```bash
export GOOGLE_APPLICATION_CREDENTIALS="/path/to/your/service-account-key.json"
```

---

## 🚀 Usage

### Run the Flask Application Locally

```bash
python application.py
```

The app runs at `http://0.0.0.0:5000`. Open your browser and fill in the booking form to get a cancellation prediction.

> **Note:** If `MODEL_OUTPUT_PATH` starts with `gs://`, the model is loaded from GCS. Otherwise, it loads from the local path.

---

## 🐳 Docker Deployment

### Build the Image

```bash
docker build -t hotel-reservation-app .
```

### Run the Container

```bash
docker run -p 5000:5000 \
  -e GOOGLE_APPLICATION_CREDENTIALS=/app/key.json \
  -v /path/to/key.json:/app/key.json \
  hotel-reservation-app
```

---

## 🔁 Jenkins CI/CD Pipeline

The `Jenkinsfile` defines a 3-stage automated pipeline:

```
┌──────────────────────┐     ┌──────────────────────────┐     ┌──────────────────────┐
│  Stage 1             │────▶│  Stage 2                 │────▶│  Stage 3             │
│  Clone GitHub Repo   │     │  Build & Push Docker Img │     │  Deploy to Cloud Run │
│                      │     │  to Google Container Reg │     │  (us-central1)       │
└──────────────────────┘     └──────────────────────────┘     └──────────────────────┘
```

### Jenkins Setup Requirements

Configure the following credentials in Jenkins:

| Credential ID | Type | Description |
|---|---|---|
| `github-token` | Username/Password | GitHub personal access token |
| `gcp-key` | Secret File | GCP service account JSON key |

### Environment Variables in Jenkinsfile

```groovy
GCP_PROJECT = "your-gcp-project-id"
GCLOUD_PATH = "/var/jenkins_home/google-cloud-sdk/bin"
```

### What the Pipeline Does

1. **Clone** — Pulls the latest code from the `main` branch
2. **Build & Push** — Authenticates with GCP, builds the Docker image (which also trains the model), and pushes it to Google Container Registry (`gcr.io`)
3. **Deploy** — Deploys the container to **Google Cloud Run** with public unauthenticated access

---

## ☁️ GCP Cloud Run Deployment

The app is deployed as a managed serverless container on Cloud Run:

```bash
gcloud run deploy ml-project \
  --image=gcr.io/YOUR_PROJECT_ID/ml-project:latest \
  --platform=managed \
  --region=us-central1 \
  --allow-unauthenticated \
  --set-env-vars CLOUD_RUN=true
```

After deployment, Cloud Run provides a public HTTPS URL for the application.

---

## 📊 MLflow Experiment Tracking

Training runs are tracked with MLflow. To view the dashboard:

```bash
mlflow ui
```

Navigate to `http://localhost:5000` to explore metrics, parameters, and registered models.

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m 'Add your feature'`
4. Push to the branch: `git push origin feature/your-feature`
5. Open a Pull Request

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

## 👤 Author

**Premnath Anbu**
- GitHub: [@PremnathAnbu](https://github.com/PremnathAnbu)

---

> ⭐ If you found this project useful, please give it a star!
