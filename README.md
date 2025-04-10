# 🧬 AI-based Respiratory Aging Predictor

This project predicts a person’s **biological  age** based on their **air quality exposure** and **personal health factors**, using machine learning models trained on synthetic CpG methylation site data.

---

## 🔍 Project Overview

The project consists of **two machine learning models**:

1. **CpG Site Predictor (Model 1)**  
   - Predicts 353 CpG methylation site values from user input (AQI, stress, physical activity, etc.)
   - Trained on a **synthetically generated dataset** with realistic biological patterns
   - Uses **chunked regression models** for memory efficiency and speed

2. **Epigenetic Clock Predictor (Model 2)**  
   - Takes predicted CpG site values as input  
   - Outputs the **biological age** of the individual, serving as an indicator for respiratory aging

---

## 🧠 Input Features (FIRST MODEL)

Users provide the following input:

| Feature               | Type     | Values                   |
|-----------------------|----------|--------------------------|
| `aqi`                 | Categorical | `low`, `medium`, `high` |
| `stress`              | Categorical | `low`, `medium`, `high` |
| `physical_activity`   | Categorical | `low`, `medium`, `high` |
| `asthma`              | Categorical | `yes`, `no`             |
| `gender`              | Categorical | `male`, `female`        |
| `chronological_age`   | Numeric  | Any integer (e.g., 45)   |

### 🔁 Output (Model 1)

A vector of **353 CpG site methylation values**:

---

## ⚙️ How Model 1 Works (CpG Site Predictor)

1. Input data is one-hot encoded to match the training format.
2. The 353 CpG values are **split into chunks of 50**.
3. For each chunk, a `MultiOutputRegressor` with `RandomForestRegressor` is trained.
4. At prediction time:
   - All models are loaded using `predict_all_cpg()`
   - Each model predicts its chunk
   - Chunks are concatenated to form the full CpG vector

---

## 🧠 Input Features (SECOND MODEL - Epigenetic Age Predictor)

The second model takes **353 CpG methylation values** as input.

| Feature     | Type     | Description                         |
|--------------|----------|-------------------------------------|
| `CpG1`–`CpG353` | Numeric  | Predicted methylation beta values |

### 🔁 Output (Model 2)

A single **biological age** prediction


---

## ⚙️ How Model 2 Works (Epigenetic Clock)

1. A regression model (e.g., `RandomForestRegressor` or `LinearRegression`) is trained using:
   - Input: 353 CpG methylation values
   - Target: biological age label
2. The model learns patterns linking CpG patterns to biological aging.
3. At inference time:
   - Pass the predicted CpG vector into the model
   - Model outputs estimated epigenetic age

---

## 🖥️ How to Run the Full Pipeline

```python
# 1. Provide input
sample_input = pd.DataFrame([{
    'aqi': 'high',
    'stress': 'low',
    'physical_activity': 'medium',
    'asthma': 'no',
    'gender': 'male',
    'chronological_age': 28
}])

# 2. Predict CpG values
cpg_values = predict_all_cpg(sample_input)

# 3. Predict Epigenetic Age
predicted_age = predict_epigenetic_age(cpg_values)
print("Your predicted epigenetic age is:", predicted_age)




