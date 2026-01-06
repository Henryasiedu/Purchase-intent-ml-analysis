# Purchase Intent Detection from Social Media Posts

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-Academic-green)](LICENSE)

**MSc Business Analytics Project**  
University of Greenwich | BUSI1783 – Business Analytics Project  
Term 3, 2024-25

---

## 📋 Project Overview

This repository contains the complete analysis code and data for detecting purchase intent in social media posts using machine learning techniques. The project analyzes 10,000 Twitter posts to distinguish between posts expressing genuine purchase intent versus general sentiment about products.

**Research Question:** Can machine learning models accurately detect purchase intent from social media text using linguistic features?

**Key Finding:** Random Forest classifier achieved F1-score of 0.794, demonstrating that purchase intent can be detected with reasonable accuracy using linguistic patterns.

---

## 👤 Author

**[Henry Asiedu]**  
MSc Business Analytics  
University of Greenwich  
Email: [ha1620j@.gre.ac.uk]  

**Supervisor:** Dr. Martina Testori  
**Module:** BUSI1783 – Business Analytics Project

---

## 📁 Repository Structure

```
purchase-intent-detection/
│
├── README.md                          # This file
├── requirements.txt                   # Python dependencies
├── LICENSE                           # License information
│
├── data/                             # Dataset directory
│   ├── raw/                          # Raw, unprocessed data
│   │   └── purchase_intent_dataset_22_11_25.csv
│   ├── processed/                    # Processed data (generated)
│   └── DATA_DICTIONARY.md           # Data documentation
│
├── code/                             # Python scripts
│   ├── feature_extraction_pipeline.py    # Feature engineering code
│   ├── train_twitter_models.py          # Model training code
│   └── run_complete_analysis.py         # Complete analysis pipeline
│
├── notebooks/                        # Jupyter notebooks
│   └── Purchase_Intent_Analysis_FINISHED.ipynb  # Main analysis notebook
│
└── results/                          # Analysis outputs
    ├── figures/                      # Visualizations (generated)
    └── models/                       # Trained models (generated)
```

---

## 📊 Dataset Information

### Source
- **Platform:** Twitter (via Twitter API v2)
- **Collection Period:** November 2024 - January 2025
- **Total Posts:** 20,000 social media posts
  - 10,000 Twitter posts (used in analysis)
  - 10,000 Reddit posts (not used in current analysis)

### Data File
- **Location:** `data/raw/purchase_intent_dataset_22_11_25.csv`
- **Size:** ~3.8 MB
- **Format:** CSV with comma delimiter
- **Encoding:** UTF-8

### Dataset Characteristics
- **Posts Analyzed:** 10,000 Twitter posts
- **Features:** 10 engineered linguistic features
- **Target Variable:** Binary classification (Purchase Intent vs General Sentiment)
- **Class Distribution:** 
  - Purchase Intent: 30% (3,000 posts)
  - General Sentiment: 70% (7,000 posts)
- **Product Categories:** Consumer Electronics and Fashion

### Features

#### Engineered Features (10 total):
1. **text_length** - Total character count
2. **word_count** - Number of words in post
3. **action_verb_count** - Count of action verbs (buy, purchase, order, get)
4. **urgency_markers** - Time-sensitive language (today, now, hurry)
5. **comparative_language** - Comparative terms (better, best, vs)
6. **first_person_pronouns** - Use of I, me, my, mine
7. **positive_words** - Positive sentiment indicators
8. **negative_words** - Negative sentiment indicators
9. **has_exclamation** - Binary: contains exclamation mark
10. **has_question** - Binary: contains question mark

#### Additional Features (not used in models):
- **likes** - Number of likes/favorites
- **retweets** - Number of retweets/shares
- **followers** - Follower count of poster
- **engagement_rate** - Calculated engagement metric

### Data Collection Methodology
1. Twitter API v2 used for post collection
2. Manual annotation for purchase intent labels
3. Two annotators with Cohen's Kappa = 0.82 (substantial agreement)
4. Quality control: removed duplicates, spam, non-English posts
5. Feature engineering applied using custom Python pipeline

### Annotation Guidelines
**Purchase Intent (Label = 1):** Posts expressing clear intention to purchase
- Example: "Just ordered the new iPhone 15! Can't wait!"
- Example: "Going to buy this dress tomorrow"

**General Sentiment (Label = 0):** Posts about products without purchase intent
- Example: "The new iPhone looks amazing"
- Example: "I love this dress design"

---

## 🔬 Analysis Methodology

### Models Evaluated
Four machine learning classifiers were trained and evaluated:

1. **Random Forest** ⭐ Best Performer
   - Trees: 100
   - Class weights: Balanced
   - Random state: 42

2. **Logistic Regression**
   - Solver: lbfgs
   - Class weights: Balanced
   - Max iterations: 1000

3. **Support Vector Machine (SVM)**
   - Kernel: Linear
   - Class weights: Balanced
   - Probability: True

4. **Naive Bayes (Gaussian)**
   - No hyperparameters (uses default)
   - Features scaled using StandardScaler

### Data Split
- **Training Set:** 8,000 posts (80%)
- **Test Set:** 2,000 posts (20%)
- **Stratification:** Yes (maintains 70/30 class ratio)
- **Random State:** 42 (for reproducibility)

### Evaluation Metrics
- **Primary:** F1-Score (harmonic mean of precision and recall)
- **Secondary:** Precision, Recall, ROC-AUC
- **Rationale:** F1-score chosen due to class imbalance (70/30 split)

---

## 📈 Key Results

### Model Performance Summary

| Model | F1-Score | Precision | Recall | ROC-AUC |
|-------|----------|-----------|--------|---------|
| **Random Forest** | **0.794** | **0.799** | **0.788** | **0.920** |
| Logistic Regression | 0.747 | 0.777 | 0.720 | 0.875 |
| Naive Bayes | 0.737 | 0.781 | 0.697 | 0.844 |
| SVM (Linear) | 0.712 | 0.791 | 0.648 | 0.869 |

### Feature Importance (Random Forest)

Top 5 most important features for purchase intent detection:

1. **action_verb_count** (0.185) - Strongest predictor
2. **first_person_pronouns** (0.142) - Second strongest
3. **urgency_markers** (0.128)
4. **word_count** (0.112)
5. **positive_words** (0.098)

### Key Findings
- ✅ Random Forest achieved best performance (F1 = 0.794)
- ✅ Action verbs are the strongest indicator of purchase intent
- ✅ First-person pronouns highly correlate with purchase intent
- ✅ Model performance close to 0.80 threshold (industry acceptable)
- ✅ ROC-AUC of 0.920 indicates excellent class separation

---

## 🚀 Reproduction Instructions

### Prerequisites

**System Requirements:**
- Python 3.8 or higher
- 4GB RAM minimum
- 500MB disk space

**Required Libraries:**
- pandas >= 1.3.0
- numpy >= 1.21.0
- scikit-learn >= 0.24.0
- matplotlib >= 3.4.0
- seaborn >= 0.11.0

### Installation

1. **Clone the repository:**
```bash
git clone https://github.com/[YOUR-USERNAME]/purchase-intent-detection.git
cd purchase-intent-detection
```

2. **Create virtual environment (recommended):**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install dependencies:**
```bash
pip install -r requirements.txt
```

### Running the Analysis

#### Option 1: Jupyter Notebook (Recommended)

The easiest way to reproduce the analysis:

```bash
jupyter notebook notebooks/Purchase_Intent_Analysis_FINISHED.ipynb
```

Then run all cells in order (Cell → Run All).

**Expected Runtime:** 2-3 minutes

#### Option 2: Python Scripts

Run the complete analysis pipeline:

```bash
python code/run_complete_analysis.py
```

This will:
1. Load and validate the dataset
2. Extract features
3. Split data into train/test
4. Train all 4 models
5. Evaluate and compare models
6. Generate visualizations
7. Save results to `results/` directory

**Expected Runtime:** 2-3 minutes

#### Option 3: Individual Scripts

Run specific parts of the analysis:

```bash
# Feature extraction only
python code/feature_extraction_pipeline.py

# Model training only
python code/train_twitter_models.py
```

### Verifying Results

After running the analysis, you should see:

**Console Output:**
```
Random Forest - F1: 0.794, Precision: 0.799, Recall: 0.788
Logistic Regression - F1: 0.747, Precision: 0.777, Recall: 0.720
Naive Bayes - F1: 0.737, Precision: 0.781, Recall: 0.697
SVM - F1: 0.712, Precision: 0.791, Recall: 0.648
```

**Generated Files:**
- `results/figures/` - Confusion matrices, ROC curves, feature importance plots
- `results/models/` - Trained model files (pickle format)

---

## 📦 Dependencies

See `requirements.txt` for complete list. Core dependencies:

```
pandas>=1.3.0          # Data manipulation
numpy>=1.21.0          # Numerical computing
scikit-learn>=0.24.0   # Machine learning
matplotlib>=3.4.0      # Plotting
seaborn>=0.11.0        # Statistical visualization
```

Optional but recommended:
```
jupyter>=1.0.0         # For running notebooks
ipython>=7.0.0         # Enhanced Python shell
```

---

## 📖 Documentation

- **`README.md`** (this file) - Project overview and reproduction guide
- **`data/DATA_DICTIONARY.md`** - Detailed data documentation
- **`notebooks/Purchase_Intent_Analysis_FINISHED.ipynb`** - Annotated analysis with explanations
- **Dissertation PDF** - Complete research report (submitted separately)

---

## 🔗 Related Links

- **University:** [University of Greenwich](https://www.gre.ac.uk/)
- **Programme:** [MSc Business Analytics](https://www.gre.ac.uk/business/programmes/msc-business-analytics)
- **Module:** BUSI1783 – Business Analytics Project
- **Twitter API:** [Twitter API v2 Documentation](https://developer.twitter.com/en/docs/twitter-api)

---

## 📊 Project Timeline

- **November 2024:** Data collection and annotation
- **December 2024:** Feature engineering and model development
- **January 2025:** Analysis, evaluation, and dissertation writing
- **Submission:** January 5, 2026

---

## 🙏 Acknowledgments

- **Dr. Martina Testori** - Project supervisor and module leader
- **University of Greenwich** - MSc Business Analytics Programme
- **Twitter API** - Data source for social media posts
- **Scikit-learn community** - Machine learning implementation

---

## 📝 Citation

If you use this work, please cite:

```bibtex
@mastersthesis{[Asiedu]2025,
  author  = {[Henry Asiedu]},
  title   = {Purchase Intent Detection from Social Media Posts Using Machine Learning},
  school  = {University of Greenwich},	
  year    = {2025},
  type    = {MSc Business Analytics Project},
  url     = {https://github.com/[YOUR-USERNAME]/purchase-intent-detection}
}
```

---

## 📧 Contact

**[Henry Asiedu]**  
MSc Business Analytics Student  
University of Greenwich

- **Email:** [ha1620j@.gre.ac.uk]
- **GitHub:** [@your-username](https://github.com/your-username)

For questions about this project, please email or open an issue on GitHub.

---

## 📄 License

This project is submitted as part of academic coursework at the University of Greenwich.

**Academic Use:** This work is provided for educational and research purposes.

**Data:** The dataset is derived from publicly available social media posts. All personally identifiable information has been removed.

**Code:** Feel free to use and adapt the code for educational purposes with appropriate citation.

---

## ⚠️ Important Notes

### Data Privacy
- All tweets have been anonymized
- No personal information included
- Compliant with Twitter's Terms of Service
- GDPR considerations addressed

### Reproducibility
- Fixed random seed (42) ensures reproducibility
- All data preprocessing steps documented
- Complete code provided
- Environment specifications in requirements.txt

### Limitations
- Analysis limited to English language posts
- Product categories: Electronics and Fashion only
- Twitter platform only (no cross-platform analysis)
- Binary classification (could be extended to multi-class)

---

## 🔮 Future Work

Potential extensions of this research:

1. **Cross-platform Analysis:** Include Reddit, Facebook, Instagram data
2. **Deep Learning:** Experiment with BERT, RoBERTa transformers
3. **Multi-class Classification:** Distinguish intent types (immediate, future, considering)
4. **Real-time Detection:** Deploy model as API for live monitoring
5. **Product-specific Models:** Separate models for different product categories

---

## 📚 Additional Resources

### Related Research
- Liu et al. (2022) - Purchase Intent Detection using NLP
- Zhang & Singh (2021) - Social Media Analytics for E-commerce
- Smith (2020) - Machine Learning for Consumer Behavior

### Learning Resources
- [Scikit-learn Documentation](https://scikit-learn.org/)
- [Pandas Documentation](https://pandas.pydata.org/)
- [Feature Engineering Guide](https://www.kaggle.com/learn/feature-engineering)

---

**Last Updated:** December 2024  
**Version:** 1.0  
**Status:** Final Submission

---

*This README provides all information necessary to understand and reproduce the analysis. For detailed methodology and results discussion, please refer to the full dissertation report.*
