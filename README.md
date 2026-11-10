# 🌊 Lake Pollution & Foaming Risk Prediction

A simple, low-cost early-warning tool that predicts **lake water state** and **foaming risk** from just four sensors on an ESP32 node.

Give it a pH, turbidity, TDS and temperature reading, and it tells you whether the water is clean or polluted and how likely foaming is.

---

## ✨ Features

- **Water State Classification:** Labels water as `Clean` or `Moderately Polluted`, with a confidence score.
- **Foaming Risk Prediction:** Returns `Low`, `Medium` or `High` risk along with a 0–100 Foaming Risk Index.
- **Sensor-Only Inputs:** The models use only what the node can measure (pH, conductivity from TDS, turbidity), while labels are built from the full 8-parameter lab dataset, so the model learns to infer hidden nutrient and organic load.
- **Temperature-Aware:** Warmer water raises the foaming risk through a simple, transparent rule.
- **Out-of-Range Warnings:** Flags readings that fall outside the data the model was trained on.
- **Two Ways to Run:** Command-line prediction or an interactive prompt for manual input.

---

## 🔌 Hardware

| Sensor | Model | Measures |
|---|---|---|
| pH sensor kit | E-201-C | pH |
| Turbidity sensor | R-0913 | Turbidity (NTU) |
| TDS sensor | R-0228 | TDS (ppm) |
| Temperature sensor | DS18B20 | Temperature (°C) |

All sensors connect to an **ESP32**.

---

## 🛠️ Tech Stack

- **Language:** Python
- **ML:** scikit-learn (Random Forest)
- **Data:** pandas, NumPy
- **Model Storage:** joblib
- **Dataset:** Luan et al., Figshare (2025)

---

## 🚀 Getting Started

1. **Clone the repository:**
   ```bash
   git clone https://github.com/sejal-0905/lake-foaming-risk-prediction.git
   cd lake-foaming-risk-prediction
   ```

2. **Install dependencies:**
   ```bash
   pip install numpy pandas scikit-learn joblib
   ```

3. **Train the models:**
   ```bash
   python lake_sensor_model.py train --csv lake_2023_12.csv
   ```

4. **Predict from the command line:**
   ```bash
   python lake_sensor_model.py predict --temp 29 --tds 320 --tur 4.5 --ph 8.3
   ```

5. **Or use the interactive version:**
   ```bash
   python predict_lake.py
   ```

---

## 🧠 How It Works

1. Pollution and foaming labels are created from all 8 lab parameters (TN, TP, CODMn, DOC, DO, turbidity, EC, pH).
2. Two Random Forest models are trained using only the sensor-measurable features.
3. For a live reading, TDS is converted to conductivity and both models output class probabilities.
4. The probabilities become a Foaming Risk Index (0–100), adjusted slightly for temperature.

---

## ⚠️ Limitations

- Labels are rule-based, so "Clean" means cleaner than the median sample in the dataset, not a regulatory standard.
- The temperature adjustment is a heuristic, since temperature isn't in the training data.
- Low-cost analog sensors need regular calibration.

---

## 🔮 Future Work

- Retrain on real data logged from the ESP32 node
- Add a web dashboard for live readings
