# 🌊 Lake Pollution & Foaming Risk Prediction

<p align="center">
  <strong>An IoT and Machine Learning-Based Early Warning System for Lake Water Quality and Foaming Risk</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.x-blue?logo=python" />
  <img src="https://img.shields.io/badge/ESP32-IoT%20Hardware-red?logo=espressif" />
  <img src="https://img.shields.io/badge/Scikit--learn-Machine%20Learning-orange?logo=scikitlearn" />
  <img src="https://img.shields.io/badge/Random%20Forest-Prediction-green" />
</p>

A low-cost, sensor-driven early-warning system that predicts **lake water quality and foaming risk** using four environmental parameters measured through an ESP32-based IoT node.

By combining pH, turbidity, Total Dissolved Solids (TDS), and temperature readings with machine learning, the system classifies water conditions and estimates the likelihood of foaming.

---

## ✨ Features

- 💧 **Water Quality Classification** — Classifies water as `Clean` or `Moderately Polluted`, along with a prediction confidence score.
- 🌊 **Foaming Risk Prediction** — Estimates `Low`, `Medium`, or `High` foaming risk and generates a Foaming Risk Index (0–100).
- 🔌 **Sensor-Based Inputs** — Uses only parameters measurable by the ESP32 node: pH, TDS, turbidity, and temperature.
- 🧠 **Machine Learning Inference** — Uses Random Forest models trained on labels derived from a broader eight-parameter laboratory dataset.
- 🌡️ **Temperature-Aware Prediction** — Applies a transparent temperature-based adjustment to the predicted foaming risk.
- ⚠️ **Out-of-Range Detection** — Flags sensor readings outside the model's training-data ranges.
- 💻 **Two Prediction Modes** — Supports command-line predictions and interactive manual input.

## 🔌 Hardware Requirements

All four sensors interface with an **ESP32 microcontroller** to collect water-quality measurements.

| Component | Model | Measurement |
|---|---|---|
| Microcontroller | ESP32 | Sensor data acquisition |
| pH Sensor | E-201-C | pH level |
| Turbidity Sensor | R-0913 | Turbidity (NTU) |
| TDS Sensor | R-0228 | Total Dissolved Solids (ppm) |
| Temperature Sensor | DS18B20 | Water temperature (°C) |

## 🛠️ Tech Stack

| Category | Technology | Purpose |
|---|---|---|
| Programming Language | Python | Model development and prediction |
| Machine Learning | scikit-learn | Random Forest classification |
| Data Processing | pandas, NumPy | Dataset processing and feature preparation |
| Model Persistence | joblib | Saving and loading trained models |
| Hardware Platform | ESP32 | Environmental sensor integration |
| Dataset | Luan et al., Figshare (2025) | Lake and reservoir water-quality data |

## 🚀 Getting Started

Follow these steps to set up and run the prediction system locally.

### Prerequisites

- Python 3
- pip
- Water-quality dataset CSV file

### Installation

**1. Clone the repository**

```bash
git clone https://github.com/sejal-0905/lake-foaming-risk-prediction.git
```

**2. Navigate to the project directory**

```bash
cd lake-foaming-risk-prediction
```

**3. Install dependencies**

```bash
pip install numpy pandas scikit-learn joblib
```

## ▶️ Usage

### 1. Train the Machine Learning Models

Train the water-state and foaming-risk prediction models using the dataset.

```bash
python lake_sensor_model.py train --csv lake_2023_12.csv
```

### 2. Command-Line Prediction

Provide the four sensor readings directly as command-line arguments.

```bash
python lake_sensor_model.py predict --temp 29 --tds 320 --tur 4.5 --ph 8.3
```

### 3. Interactive Prediction

Alternatively, run the interactive prediction script to enter sensor readings manually.

```bash
python predict_lake.py
```

## 🧠 How It Works

The system uses a combination of laboratory-derived labels and sensor-measurable features to train two separate Random Forest models.

### Model Development Pipeline

1. **Dataset Preparation:** Uses eight laboratory parameters — Total Nitrogen (TN), Total Phosphorus (TP), CODMn, Dissolved Organic Carbon (DOC), Dissolved Oxygen (DO), turbidity, Electrical Conductivity (EC), and pH.
2. **Label Generation:** Derives water-state and foaming-risk labels using rules based on the full laboratory dataset.
3. **Feature Selection:** Restricts model inputs to sensor-measurable parameters: pH, conductivity (converted from TDS), and turbidity.
4. **Model Training:** Trains two Random Forest classifiers for water-state classification and foaming-risk prediction.
5. **Prediction:** Uses live or manually entered sensor readings to generate class probabilities.
6. **Risk Index Calculation:** Converts prediction probabilities into a 0–100 Foaming Risk Index, with a temperature-based adjustment.

### System Workflow

```text
       ESP32 Sensor Node
               |
               v
      Environmental Readings
     (pH, TDS, Turbidity,
         Temperature)
               |
               v
       Feature Preparation
      (TDS → Conductivity)
               |
          +----+----+
          |         |
          v         v
     Water State   Foaming Risk
     Classifier    Classifier
          |         |
          v         v
     Clean /       Low / Medium /
     Moderately    High
     Polluted      |
          |         v
          |    Foaming Risk Index
          |       (0–100)
          +----+----+
               |
               v
        Prediction Output
```

## ⚠️ Limitations

- **Rule-Based Labels:** Water-state labels are derived from the dataset median. Therefore, `Clean` represents a relatively cleaner sample within the dataset, not compliance with an established regulatory water-quality standard.
- **Indirect Feature Inference:** The models use a limited set of sensor inputs to infer patterns associated with laboratory parameters that are not directly measured by the IoT node.
- **Temperature Heuristic:** Temperature is not included in the model's training features. Its influence on foaming risk is handled through a separate rule-based adjustment.
- **Sensor Calibration:** Low-cost analog sensors require regular calibration to maintain measurement reliability.
- **Dataset Generalisation:** Model performance on real-world lake conditions requires validation using independently collected sensor and laboratory measurements.

## 🔮 Future Work

- Integrate the trained prediction pipeline with the ESP32 for real-time sensor-based inference.
- Collect and evaluate water-quality readings from actual IoT deployments.
- Retrain and validate the models using real-world sensor and laboratory measurements.
- Develop a web dashboard for live sensor monitoring, water-state classification, and foaming-risk visualisation.

## 🎯 Project Highlights

- Combines IoT-based environmental monitoring with machine learning.
- Develops two Random Forest models for water-state and foaming-risk prediction.
- Uses four low-cost sensors to estimate water-quality conditions.
- Applies feature selection to bridge the gap between laboratory measurements and practical sensor availability.
- Incorporates temperature-aware risk adjustment and out-of-range warnings.
- Provides a foundation for low-cost, real-time lake pollution monitoring and early warning.

---

<p align="center">
  <strong>👩‍💻 Developed as an IoT and Machine Learning academic project</strong>
  <br />
  <sub>Internet of Things • Environmental Monitoring • Machine Learning • Smart Cities</sub>
</p>
