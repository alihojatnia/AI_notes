```markdown
# Wine Quality Prediction – MLflow + DVC Demo


A clean, professional, and fully reproducible machine learning project that predicts whether a red wine is high quality (rating ≥ 7) using physicochemical features.

Built with:
- **MLflow** – experiment tracking, model registry, metrics
- **DVC** – data & pipeline version control
- **scikit-learn** – simple yet powerful Random Forest model
- **Poetry** – dependency & environment management

---

##  Dataset

**Red Wine Quality** from [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/wine+quality)  
- 1,599 samples  
- 11 input features (acidity, sugar, pH, alcohol, etc.)  
- Binary target: `high_quality` (1 = rating ≥ 7)

> Small, meaningful, and real-world — ideal for demos.

---

## 🚀 Quick Start

```bash
# 1. Clone the repo
git clone https://github.com/yourusername/wine-quality-mlflow-dvc.git
cd wine-quality-mlflow-dvc

# 2. Install with Poetry
poetry install
poetry shell

# 3. Download raw data
mkdir -p data/raw
curl -L -o data/raw/winequality-red.csv \
  https://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/winequality-red.csv

# 4. Version the data with DVC
dvc add data/raw/winequality-red.csv
git add data/raw/winequality-red.csv.dvc
git commit -m "Add raw wine data"

# 5. Run the full pipeline
dvc repro
```

---

## 🔥 See Your Experiments

```bash
mlflow ui
```

Open [http://localhost:5000](http://localhost:5000) to explore:
- Parameters
- Metrics (accuracy, F1)
- Model artifacts
- Registered models (if F1 > 0.70)

---

## 📁 Project Structure

```text
├── data/
│   ├── raw/          # DVC-tracked raw CSV
│   └── processed/    # train/test split
├── src/
│   ├── data.py       # preprocessing logic
│   └── model.py      # training + MLflow logging
├── models/           # pickled model
├── metrics/          # JSON metrics (DVC-tracked)
├── params.yaml       # hyperparams & split config
├── dvc.yaml          # pipeline definition
├── pyproject.toml    # Poetry deps
└── README.md         # you're here!
```

---

## 🛠️ Reproducibility

- **Data**: Versioned with DVC (`data/raw/*.dvc`)
- **Pipeline**: `dvc repro` rebuilds everything
- **Environment**: Locked via `poetry.lock`
- **Experiments**: Tracked in `mlruns/`

Change a parameter → commit → `dvc repro` → new version, new experiment. Done.

---

## 🎯 Try It Yourself

```yaml
# params.yaml
train:
  max_depth: 12   # ← tweak me!
  n_estimators: 300
```

```bash
git commit -am "Try deeper forest"
dvc repro
```

Watch MLflow log a new run automatically.

---

## 🌍 Remote Storage (Optional)

Set up a DVC remote (S3, GCS, SSH, etc.):

```bash
dvc remote add -d myremote s3://my-bucket/wine-project
dvc push
```

Now your data lives safely outside Git.

---

## 🤝 Contributing

Pull requests welcome! Feel free to:
- Add new models (XGBoost, Logistic Regression)
- Improve feature engineering
- Add tests or CI

---

## 📜 License

[MIT](LICENSE) – free to use, modify, and show off.

---

Built with ❤️ and a glass of good red wine.

---
``` 

Just replace `yourusername` with your GitHub handle, push it, and you're good to go! This README is clear, friendly, and professional — exactly what recruiters and collaborators love to see. 🍷
```

