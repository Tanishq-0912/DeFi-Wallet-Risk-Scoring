# 📊 Wallet Risk Scoring Using Compound Protocol

## 🔍 Objective

The goal of this project is to analyze a set of Ethereum wallet addresses and assign each a **risk score from 0 to 1000** based on their on-chain behavior using data from the **Compound Protocol via the Covalent API**.

---

## 📦 Deliverables

- `wallet_scores.csv`: Final file with wallet addresses and their risk scores.
- `README.md`: Explanation of the process, decisions, and scoring logic.

---

## 📈 Process Overview

### 1. ✅ Data Collection

- Used the **Covalent API** to fetch Ethereum Mainnet transaction history for each wallet.
- Endpoint used:  
  `https://api.covalenthq.com/v1/1/address/{wallet_address}/transactions_v2/`
- Retrieved up to 10,000 transactions per wallet.

---

### 2. 🧪 Feature Selection

The following behavioral indicators were extracted:

| Feature Name      | Description |
|------------------|-------------|
| `total_txns`      | Total number of transactions made by the wallet. |
| `successful_txns` | Number of successful transactions. |
| `success_ratio`   | Ratio of successful to total transactions. |
| `total_gas`       | Total gas spent across all transactions. |

These features were chosen to capture both **activity** and **reliability**.

---

### 3. ⚙️ Scoring Methodology

- Applied **MinMax normalization** to scale all features to a 0–1 range.
- Assigned custom weights to reflect risk indicators:

| Feature          | Weight |
|------------------|--------|
| total_txns       | 30%    |
| successful_txns  | 30%    |
| success_ratio    | 30%    |
| total_gas        | 10%    |

- Final Score Calculation:  
  `score = weighted_sum(normalized_features) × 1000`

- Resulting scores are rounded to the nearest integer in the 0–1000 range.

---

### 4. 📤 Output Format

The final CSV file contains:

| wallet_id | score |
|-----------|-------|
| 0xabc...  | 732   |
| 0xdef...  | 589   |

---

## 🧠 Why These Indicators?

- High transaction volume can indicate heavy usage, but may also increase risk if success ratio is low.
- High success ratio implies reliability and fewer failed or spammy attempts.
- Gas usage shows economic engagement — users spending more gas are likely interacting with smart contracts.

---

## ✅ Tools & Technologies

- Python (Jupyter Notebook)
- Covalent API
- Pandas, Requests, Scikit-learn (MinMaxScaler)
- tqdm for progress bar

---

## 🚀 Notes

- Only wallets with non-empty transaction histories were considered.
- Model can be extended with lending/borrowing behavior from Compound V2/V3 or Aave for deeper risk profiling.
