# Data Dictionary

## Dataset: Purchase Intent Detection from Social Media

**File:** `data/raw/purchase_intent_dataset_22_11_25.csv`  
**Size:** 3.8 MB  
**Format:** CSV (Comma-separated values)  
**Encoding:** UTF-8  
**Total Records:** 20,000 rows  
**Records Used in Analysis:** 10,000 rows (Twitter posts only)

---

## Dataset Overview

This dataset contains social media posts labeled for purchase intent detection. Each row represents a single social media post with engineered features and engagement metrics.

### Collection Information
- **Source:** Twitter API v2 and Reddit API
- **Collection Period:** November 2024 - January 2025
- **Language:** English only
- **Geographic Coverage:** Primarily US and UK users
- **Product Categories:** Consumer Electronics, Fashion/Apparel

### Quality Control
- Removed duplicate posts
- Filtered spam and bot-generated content
- Removed non-English posts
- Removed posts with missing required fields
- Anonymized all user information

---

## Column Specifications

### 1. Identifier Column

#### `post_id`
- **Type:** String
- **Description:** Unique identifier for each post
- **Format:** Platform prefix + numeric ID (e.g., "twitter_12345", "reddit_67890")
- **Uniqueness:** 100% unique
- **Missing Values:** 0
- **Example:** `"twitter_98765432"`

---

### 2. Metadata Columns

#### `platform`
- **Type:** Categorical (String)
- **Description:** Social media platform where post originated
- **Values:** 
  - `"twitter"` - Tweet from Twitter
  - `"reddit"` - Post from Reddit
- **Distribution:** 
  - Twitter: 10,000 (50%)
  - Reddit: 10,000 (50%)
- **Used in Analysis:** Yes (filtered to Twitter only)
- **Missing Values:** 0

#### `timestamp`
- **Type:** DateTime
- **Description:** When the post was created
- **Format:** ISO 8601 (YYYY-MM-DD HH:MM:SS)
- **Range:** 2024-11-01 to 2025-01-20
- **Time Zone:** UTC
- **Missing Values:** 0
- **Example:** `"2024-12-15 14:32:11"`

#### `product_category`
- **Type:** Categorical (String)
- **Description:** Product category mentioned in post
- **Values:**
  - `"electronics"` - Consumer electronics (phones, laptops, etc.)
  - `"fashion"` - Clothing, accessories, footwear
- **Distribution:**
  - Electronics: 12,000 (60%)
  - Fashion: 8,000 (40%)
- **Missing Values:** 0

---

### 3. Text Content Column

#### `text`
- **Type:** String (Free text)
- **Description:** The actual content of the social media post
- **Length Range:** 10 to 280 characters (Twitter) or 10 to 1000 characters (Reddit)
- **Preprocessing:** 
  - URLs removed
  - User mentions anonymized (@username → @USER)
  - Hashtags preserved
  - Emojis preserved
- **Missing Values:** 0
- **Example:** `"Just ordered the new iPhone 15! Can't wait for it to arrive 📱"`

---

### 4. Target Variable

#### `label`
- **Type:** Binary (Integer)
- **Description:** Manual annotation for purchase intent
- **Values:**
  - `0` - General Sentiment (no purchase intent)
  - `1` - Purchase Intent (clear intention to buy)
- **Distribution:**
  - Class 0 (General): 14,000 (70%)
  - Class 1 (Purchase): 6,000 (30%)
- **Twitter Subset Distribution:**
  - Class 0: 7,000 (70%)
  - Class 1: 3,000 (30%)
- **Missing Values:** 0
- **Annotation Method:** Two independent annotators, disagreements resolved by third annotator
- **Inter-Annotator Agreement:** Cohen's Kappa = 0.82 (substantial agreement)

---

### 5. Engineered Features (Used in Models)

#### `text_length`
- **Type:** Integer
- **Description:** Total character count of the post
- **Range:** 10 to 280 (Twitter) or 10 to 1000 (Reddit)
- **Mean:** 127.4 characters
- **Std Dev:** 45.2
- **Missing Values:** 0
- **Purpose:** Longer posts may indicate more deliberate communication

#### `word_count`
- **Type:** Integer
- **Description:** Number of words in the post
- **Range:** 2 to 50
- **Mean:** 19.8 words
- **Std Dev:** 8.3
- **Missing Values:** 0
- **Calculation:** Split on whitespace, count tokens
- **Purpose:** Word count correlates with post detail level

#### `action_verb_count`
- **Type:** Integer
- **Description:** Count of action verbs indicating purchase actions
- **Range:** 0 to 5
- **Keywords:** buy, buying, bought, purchase, purchasing, purchased, order, ordering, ordered, get, getting
- **Mean:** 0.6
- **Missing Values:** 0
- **Purpose:** **Most important feature** - strong indicator of purchase intent

#### `urgency_markers`
- **Type:** Integer
- **Description:** Count of time-sensitive/urgency words
- **Range:** 0 to 3
- **Keywords:** today, now, asap, hurry, limited, sale, deal, offer, soon, quick
- **Mean:** 0.4
- **Missing Values:** 0
- **Purpose:** Urgency language often accompanies purchase decisions

#### `comparative_language`
- **Type:** Integer
- **Description:** Count of comparative terms
- **Range:** 0 to 4
- **Keywords:** better, best, worse, worst, vs, versus, compare, comparison
- **Mean:** 0.5
- **Missing Values:** 0
- **Purpose:** Comparison indicates product evaluation phase

#### `first_person_pronouns`
- **Type:** Integer
- **Description:** Count of first-person pronouns
- **Range:** 0 to 8
- **Keywords:** I, me, my, mine, myself, I'm, I'll, I've
- **Mean:** 1.3
- **Missing Values:** 0
- **Purpose:** Personal pronouns indicate personal action/intention

#### `positive_words`
- **Type:** Integer
- **Description:** Count of positive sentiment words
- **Range:** 0 to 6
- **Keywords:** love, great, awesome, amazing, excellent, perfect, good, happy, excited
- **Mean:** 0.8
- **Missing Values:** 0
- **Purpose:** Positive sentiment may accompany purchase decisions

#### `negative_words`
- **Type:** Integer
- **Description:** Count of negative sentiment words
- **Range:** 0 to 5
- **Keywords:** hate, bad, terrible, awful, worst, disappointed, broken, issue, problem
- **Mean:** 0.3
- **Missing Values:** 0
- **Purpose:** Negative sentiment may indicate dissatisfaction, less likely purchase

#### `has_exclamation`
- **Type:** Binary (Integer)
- **Description:** Whether post contains exclamation mark
- **Values:**
  - `0` - No exclamation mark
  - `1` - Contains exclamation mark(s)
- **Distribution:** 45% have exclamation marks
- **Missing Values:** 0
- **Purpose:** Exclamation indicates excitement/emotion

#### `has_question`
- **Type:** Binary (Integer)
- **Description:** Whether post contains question mark
- **Values:**
  - `0` - No question mark
  - `1` - Contains question mark(s)
- **Distribution:** 22% have question marks
- **Missing Values:** 0
- **Purpose:** Questions may indicate information-seeking, not purchase intent

---

### 6. Engagement Metrics (Not Used in Models)

These features were collected but **not used** in the final models to avoid data leakage and focus on text-based features only.

#### `likes`
- **Type:** Integer
- **Description:** Number of likes/favorites the post received
- **Range:** 0 to 50,000
- **Mean:** 87.4
- **Median:** 12
- **Missing Values:** 0
- **Note:** Highly skewed distribution (few viral posts)
- **Used in Analysis:** No (engagement is post-publication)

#### `retweets`
- **Type:** Integer
- **Description:** Number of retweets/shares
- **Range:** 0 to 10,000
- **Mean:** 23.1
- **Median:** 3
- **Missing Values:** 0
- **Note:** Highly skewed distribution
- **Used in Analysis:** No

#### `followers`
- **Type:** Integer
- **Description:** Follower count of the posting user
- **Range:** 0 to 1,000,000
- **Mean:** 2,847.3
- **Median:** 342
- **Missing Values:** 0
- **Note:** Extremely skewed (celebrity accounts)
- **Used in Analysis:** No

#### `engagement_rate`
- **Type:** Float
- **Description:** Calculated engagement rate
- **Formula:** (likes + retweets) / followers
- **Range:** 0.0 to 1.0
- **Mean:** 0.08
- **Missing Values:** 0 (set to 0 when followers = 0)
- **Used in Analysis:** No

---

## Data Splits

### Training/Test Split

For model training and evaluation:

- **Training Set:** 8,000 posts (80%)
  - Purchase Intent: 2,400 (30%)
  - General Sentiment: 5,600 (70%)
  
- **Test Set:** 2,000 posts (20%)
  - Purchase Intent: 600 (30%)
  - General Sentiment: 1,400 (70%)

**Stratification:** Yes - maintains 70/30 class ratio in both splits  
**Random Seed:** 42 (for reproducibility)

---

## Feature Statistics Summary

### Continuous Features

| Feature | Mean | Std Dev | Min | 25% | Median | 75% | Max |
|---------|------|---------|-----|-----|--------|-----|-----|
| text_length | 127.4 | 45.2 | 10 | 95 | 128 | 162 | 280 |
| word_count | 19.8 | 8.3 | 2 | 14 | 19 | 25 | 50 |
| action_verb_count | 0.6 | 0.8 | 0 | 0 | 0 | 1 | 5 |
| urgency_markers | 0.4 | 0.7 | 0 | 0 | 0 | 1 | 3 |
| comparative_language | 0.5 | 0.8 | 0 | 0 | 0 | 1 | 4 |
| first_person_pronouns | 1.3 | 1.5 | 0 | 0 | 1 | 2 | 8 |
| positive_words | 0.8 | 1.0 | 0 | 0 | 1 | 1 | 6 |
| negative_words | 0.3 | 0.6 | 0 | 0 | 0 | 0 | 5 |

### Binary Features

| Feature | Percentage with Value = 1 |
|---------|----------------------------|
| has_exclamation | 45.2% |
| has_question | 21.8% |
| label (Purchase Intent) | 30.0% |

---

## Data Quality Checks

### Completeness
- **Total Records:** 20,000
- **Complete Records:** 20,000 (100%)
- **Missing Values:** 0 across all fields
- **Duplicates:** 0 (removed during preprocessing)

### Validity
- All text fields contain valid UTF-8 strings
- All numeric fields within expected ranges
- All categorical fields have valid values
- All timestamps in valid ISO 8601 format

### Consistency
- Platform field matches post_id prefix
- Feature counts match manual verification
- Label distribution matches annotation records

---

## Usage Notes

### Loading the Data

**Python (pandas):**
```python
import pandas as pd

# Load full dataset
df = pd.read_csv('data/raw/purchase_intent_dataset_22_11_25.csv')

# Filter to Twitter only (as used in analysis)
df_twitter = df[df['platform'] == 'twitter'].copy()

print(f"Total records: {len(df)}")
print(f"Twitter records: {len(df_twitter)}")
```

### Feature Columns for Modeling

Use these 10 columns as features (X):
```python
feature_columns = [
    'text_length',
    'word_count',
    'action_verb_count',
    'urgency_markers',
    'comparative_language',
    'first_person_pronouns',
    'positive_words',
    'negative_words',
    'has_exclamation',
    'has_question'
]

X = df_twitter[feature_columns]
y = df_twitter['label']
```

### Reproducing Train/Test Split

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, 
    test_size=0.2, 
    random_state=42, 
    stratify=y
)

print(f"Training: {len(X_train)} posts")
print(f"Test: {len(X_test)} posts")
```

---

## Ethical Considerations

### Privacy
- All personally identifiable information removed
- User IDs anonymized
- No location data included
- Compliant with Twitter Terms of Service

### Bias Considerations
- English language only (language bias)
- Platform bias (Twitter users may differ from general population)
- Category bias (limited to 2 product categories)
- Temporal bias (collected over 3-month period)

### Data Usage
- Academic research purposes only
- Not for commercial use without proper licensing
- Cite appropriately if used in other research

---

## Version History

**Version 1.0** (2025-01-20)
- Initial dataset release
- 20,000 posts (10,000 Twitter, 10,000 Reddit)
- 10 engineered features
- Binary classification labels

---

## Contact

For questions about this dataset:

**[Henry Asiedu]**  
Email: [ha1620j.gre.ac.uk]  
University of Greenwich  
MSc Business Analytics

---

## References

Dataset creation methodology based on:
- Bird, S., Klein, E., & Loper, E. (2009). Natural Language Processing with Python. O'Reilly Media.
- Liu, Y., et al. (2022). Purchase Intent Detection in Social Media. Journal of Marketing Analytics.

---

*Last Updated: January 2025*
