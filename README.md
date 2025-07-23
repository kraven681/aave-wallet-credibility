# aave-wallet-credibility

project/
├── app.py
├── model_training.py
├── feature_engineering.py
├── score_wallets.py
├── wallet_credit_model.pkl
├── requirements.txt
├── README.md
├── analysis.md
├── credit_score_distribution.png
└── sample_data.json


---

# README.md

## Wallet Credit Scoring

### Overview
This project provides a machine learning-based scoring system that evaluates DeFi wallets based on their historical transaction behavior on Aave V2. The model assigns a credit score between 0 and 1000 to each wallet, with higher scores indicating more reliable and responsible usage.

### Architecture
- **Data Input**: JSON transaction data from Aave V2
- **Feature Engineering**: Extracts behavior-based features from transaction logs
- **Model**: Regression-based ML model (e.g., Random Forest) trained to predict wallet reliability
- **API**: Flask-based server for live scoring
- **Outputs**: Credit scores with optional behavior analysis

### Files
- `app.py`: Flask API to score new data
- `model_training.py`: Training script for the ML model
- `feature_engineering.py`: Functions to extract features from raw transaction data
- `score_wallets.py`: Scales raw model predictions to 0-1000
- `wallet_credit_model.pkl`: Pretrained model
- `analysis.md`: Distribution and behavior analysis
- `credit_score_distribution.png`: Visual of score distribution

### Setup
```bash
pip install -r requirements.txt
python model_training.py  # Train the model
python app.py             # Start the API server
```

### API Usage
**Endpoint:** `POST /score`

**Input:**
```json
[
  {
    "userWallet": "0x123...",
    "action": "deposit",
    "timestamp": "2021-08-01T00:00:00Z",
    ...
  },
  ...
]
```

**Output:**
```json
[
  {
    "userWallet": "0x123...",
    "credit_score": 865
  },
  ...
]
```

---

# analysis.md

## Wallet Credit Score Analysis

### Score Distribution
Credit scores were binned into ranges:
- 0–100
- 101–200
- ...
- 901–1000

**Distribution Plot:**
![Score Distribution](credit_score_distribution.png)

### Key Observations

#### Low Range Wallets (0–300):
- **High borrow-to-repay ratios**
- **Frequent liquidation calls**
- **Short activity lifespan (burst usage)**
- Likely to be exploiters, bots, or distressed users

#### High Range Wallets (700–1000):
- **Consistent deposit behavior**
- **On-time repayments and fewer liquidations**
- **Diverse token usage and long-term activity**
- Likely long-term users, whales, or bots mimicking responsible behavior

### Conclusion
This credit scoring system can assist lending protocols in risk modeling, identifying Sybil wallets, and allocating rewards responsibly. Future improvements may include incorporating more chains, external credit data, and community governance signals.
