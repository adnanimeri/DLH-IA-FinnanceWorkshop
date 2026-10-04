# Exercise 2 – Part 2: Evaluating a Fraud Detection Workflow

## Purpose of this exercise

In the first part of Exercise 2, students design an AI decision workflow for a financial use case such as fraud review.

In this second part, students evaluate whether the AI model used in the workflow is useful in practice.

The goal is to understand why financial AI systems should not be evaluated only with accuracy, especially when the event of interest is rare, such as fraud, AML alerts, loan defaults, cyber incidents, or insurance fraud.

---

## 1. Context: Fraud Detection in Finance

Imagine a bank uses an AI model to detect suspicious card transactions in real time.

For each transaction, the model produces a **fraud-risk score** between 0 and 1.

If the score is above a defined threshold, the transaction may be:

- approved with additional monitoring,
- challenged using step-up authentication,
- temporarily blocked,
- or sent to a fraud analyst for review.

This is a typical **rare-event problem**.

Most transactions are legitimate, while only a small percentage are fraudulent. Because fraud is rare, a model can appear very accurate while still failing to detect the cases that matter most.

---

## 2. The Four Possible Outcomes

When an AI model predicts whether a transaction is fraud or not fraud, there are four possible outcomes.

| Outcome | Meaning | Finance example | Business consequence |
|---|---|---|---|
| **True Positive (TP)** | The model says fraud, and it is fraud. | A stolen-card transaction is correctly flagged. | Fraud loss is prevented or reduced. |
| **False Positive (FP)** | The model says fraud, but it is legitimate. | A real customer payment is blocked by mistake. | Customer frustration, support calls, analyst workload. |
| **False Negative (FN)** | The model says not fraud, but it is fraud. | A stolen-card transaction is approved. | Financial loss, customer harm, repeated attacks. |
| **True Negative (TN)** | The model says not fraud, and it is legitimate. | A normal card payment is approved. | Good customer experience and no analyst workload. |

---

## 3. Why False Positives and False Negatives Matter

### False positive example

A customer travels from Luxembourg to Paris and pays for a hotel.

The AI model sees:

- an unusual location,
- a higher-than-normal amount,
- a new merchant,
- a different spending pattern.

The model flags the transaction as suspicious.

However, the transaction is legitimate.

This is a **false positive**.

### Why false positives matter

False positives create operational and customer problems:

- the customer may be blocked or frustrated,
- the bank may receive support calls,
- analysts waste time reviewing legitimate transactions,
- customer trust may decrease.

A fraud model with too many false positives may become expensive and annoying, even if it catches some fraud.

---

### False negative example

A fraudster steals a customer’s card details and makes an online purchase.

The AI model does not flag the transaction.

The transaction is approved.

This is a **false negative**.

### Why false negatives matter

False negatives create direct financial and security risk:

- the bank may lose money,
- the customer may be harmed,
- the fraudster may continue using the account,
- the institution may fail to detect a broader fraud pattern.

A fraud model with too many false negatives does not protect the institution or the customer.

---

## 4. Why These Formulas Are Needed

In financial services, different errors have different costs.

A false positive may annoy a legitimate customer.

A false negative may allow fraud to pass.

Accuracy alone can hide the real performance of a fraud model because legitimate transactions usually dominate the dataset.

That is why we use:

- **precision**,
- **recall**,
- **F1 score**,
- and **AUPRC**.

These metrics help us evaluate whether the model:

- produces useful alerts,
- catches enough real fraud,
- avoids overwhelming analysts,
- and remains practical for operations.

---

## 5. Detailed Numerical Example

Assume we test a fraud model on **1,000 transactions**.

- **40 transactions** are actually fraud.
- **960 transactions** are legitimate.
- The model flags **50 transactions** as suspicious.
- After investigation, **30 flagged transactions** are real fraud.
- **20 flagged transactions** are legitimate.
- The model missed **10 fraud cases**.

| Metric element | Count | Explanation |
|---|---:|---|
| **True Positives (TP)** | 30 | Fraud cases correctly flagged by the model. |
| **False Positives (FP)** | 20 | Legitimate transactions wrongly flagged as fraud. |
| **False Negatives (FN)** | 10 | Fraud cases missed by the model. |
| **True Negatives (TN)** | 940 | Legitimate transactions correctly approved. |
| **Total transactions** | 1,000 | All transactions in the test set. |

---

## 6. Confusion Matrix View

|  | Actual fraud | Actual legitimate | Meaning |
|---|---:|---:|---|
| **Predicted fraud** | TP = 30 | FP = 20 | Flagged transactions |
| **Predicted not fraud** | FN = 10 | TN = 940 | Approved transactions |

---

## 7. Precision: How Clean Are the Alerts?

Precision answers the question:

> Of all transactions flagged as suspicious, how many were really fraud?

### Formula

```text
Precision = TP / (TP + FP)
```

### Calculation

```text
Precision = 30 / (30 + 20)
Precision = 30 / 50
Precision = 0.60 = 60%
```

### Interpretation

When the model raises an alert, it is correct **60% of the time**.

In practical terms, **40% of alerts are false alarms**.

### Why precision matters

Precision is important when false positives are expensive.

In fraud operations, low precision means:

- analysts spend too much time investigating legitimate customers,
- customers experience unnecessary friction,
- the fraud team may become overloaded.

---

## 8. Recall: How Much Fraud Do We Catch?

Recall answers the question:

> Of all real fraud cases, how many did the model detect?

### Formula

```text
Recall = TP / (TP + FN)
```

### Calculation

```text
Recall = 30 / (30 + 10)
Recall = 30 / 40
Recall = 0.75 = 75%
```

### Interpretation

The model catches **75% of all real fraud cases**.

It also misses **25% of fraud cases**.

### Why recall matters

Recall is important when false negatives are expensive.

A false negative means the bank allowed a fraudulent transaction to pass, potentially causing:

- financial loss,
- customer harm,
- repeated attacks,
- missed fraud patterns.

---

## 9. F1 Score: Balance Between Precision and Recall

F1 score combines precision and recall into one balanced metric.

It is useful when both false positives and false negatives matter.

### Formula

```text
F1 = 2 × (Precision × Recall) / (Precision + Recall)
```

### Calculation

```text
F1 = 2 × (0.60 × 0.75) / (0.60 + 0.75)
F1 = 2 × 0.45 / 1.35
F1 = 0.90 / 1.35
F1 = 0.67
```

### Interpretation

The model has a moderate balance between alert quality and fraud coverage.

A higher F1 score would mean a better balance between:

- catching fraud,
- and avoiding unnecessary alerts.

---

## 10. Business Trade-off: Precision vs. Recall

| Model behaviour | What happens | Business impact |
|---|---|---|
| **High precision, low recall** | The model flags fewer cases and is usually correct, but misses many fraud cases. | Lower analyst workload, but higher fraud loss risk. |
| **Low precision, high recall** | The model catches more fraud, but also flags many legitimate transactions. | Lower fraud loss, but more customer friction and analyst overload. |
| **Balanced precision and recall** | The model catches a useful share of fraud while keeping alerts manageable. | More practical for operational deployment. |

---

## 11. Second Example: AUPRC vs. Accuracy

Now imagine a larger fraud dataset with **10,000 transactions**.

Only **100 transactions are fraudulent**.

That means fraud is only **1%** of the dataset.

This is common in financial crime detection: the positive class is rare.

| Dataset element | Count |
|---|---:|
| Total transactions | 10,000 |
| Fraud transactions | 100 |
| Legitimate transactions | 9,900 |
| Fraud rate | 1% |

---

## 12. Model A: High Accuracy but Useless for Fraud

Model A predicts:

> “Not fraud” for every transaction.

| Result | Count |
|---|---:|
| Fraud detected | 0 |
| Fraud missed | 100 |
| Legitimate transactions correctly classified | 9,900 |
| Accuracy | 9,900 / 10,000 = 99% |

This model has **99% accuracy**, but it detects **zero fraud cases**.

It is operationally useless for fraud detection, even though it looks excellent according to accuracy.

---

## 13. Why Accuracy Can Be Misleading

Accuracy answers:

> How many predictions were correct overall?

That sounds useful, but it can be misleading when the event of interest is rare.

In this example:

- 99% of transactions are legitimate,
- the model predicts “not fraud” every time,
- so it gets 99% accuracy,
- but it finds none of the fraud cases.

This is why accuracy alone is not sufficient for rare-event financial AI.

---

## 14. Why AUPRC Is Better for Rare-Event Financial AI

AUPRC means:

> Area Under the Precision–Recall Curve

AUPRC evaluates how precision and recall change as the decision threshold changes.

Instead of asking only:

> How many predictions were correct overall?

AUPRC asks:

> Can the model rank rare positive cases, such as fraud, near the top of the review queue?

This is important because fraud teams often cannot review every transaction.

Students need the model to rank the most suspicious cases first.

---

## 15. Comparing Accuracy, Precision, Recall, F1, and AUPRC

| Metric | What it focuses on | When it is useful |
|---|---|---|
| **Accuracy** | Correct predictions across all classes. | Useful when classes are balanced. |
| **Precision** | Quality of the alerts. | Useful when false positives create cost or customer friction. |
| **Recall** | Ability to find real positive cases. | Useful when missed fraud, AML cases, or defaults are risky. |
| **F1 score** | Balance between precision and recall at one threshold. | Useful when both false positives and false negatives matter. |
| **AUPRC** | Precision-recall trade-off across thresholds. | Useful when the positive class is rare and ranking matters. |

---

## 16. Practical AUPRC Example

Assume an AML or fraud team can review only the **top 200 highest-risk transactions per day**.

The model ranks all **10,000 transactions** from most suspicious to least suspicious.

| Model | Accuracy | Fraud found in top 200 alerts | Operational interpretation |
|---|---:|---:|---|
| **Model A: Predicts no fraud** | 99% | 0 out of 100 | Looks accurate, but provides no useful investigation queue. |
| **Model B: Risk-ranking model** | Lower or not directly comparable | 60 out of 100 | More useful because many real fraud cases are placed near the top. |

AUPRC is useful here because the business problem is not simply classification.

The real problem is prioritization:

> Which transactions should analysts review first?

---

## 17. Student Exercise: Generate Random Data and Apply the Formulas

Students can now generate a simple random fraud-detection dataset and apply all metrics.

### Task

Generate a dataset with **200 transactions**.

Each transaction should have:

| Column | Meaning |
|---|---|
| Transaction ID | Unique transaction number |
| Amount | Random transaction amount |
| Actual label | 1 = fraud, 0 = legitimate |
| Predicted score | AI fraud-risk score between 0 and 1 |
| Predicted label | 1 = flagged as fraud, 0 = not flagged |

---

## 18. Suggested Data Setup

Use the following setup:

- Number of transactions: **200**
- Fraud rate: around **5%**
- Fraud cases: around **10 transactions**
- Legitimate cases: around **190 transactions**
- Fraud-risk score: random value between **0 and 1**
- Decision threshold: **0.70**

Decision rule:

```text
If fraud-risk score >= 0.70, predict fraud.
If fraud-risk score < 0.70, predict not fraud.
```

---

## 19. Important Note on Random Data

The data should not be completely random.

It should be **semi-realistic**.

For example:

- fraud cases should generally have higher risk scores,
- legitimate cases should generally have lower risk scores,
- some overlap should exist,
- the overlap creates false positives and false negatives.

This makes the exercise more realistic.

If all scores are random, the model performance may look meaningless.

---

## 20. What Students Should Calculate

Students should calculate:

1. True Positives (TP)
2. False Positives (FP)
3. False Negatives (FN)
4. True Negatives (TN)
5. Accuracy
6. Precision
7. Recall
8. F1 score
9. AUPRC

Students should then answer:

- Is the model useful?
- Does it create too many false positives?
- Does it miss too much fraud?
- Is accuracy misleading?
- Which metric is most relevant for fraud detection?

---

## 21. Optional Python Example for Students

Students can use the following Python code to generate a simple dataset and calculate the metrics.

```python
import numpy as np
import pandas as pd
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score, average_precision_score

np.random.seed(42)

# 1. Generate random transaction data
n_transactions = 200
fraud_rate = 0.05

transaction_id = np.arange(1, n_transactions + 1)
amount = np.round(np.random.uniform(5, 500, n_transactions), 2)

# 2. Generate actual labels: 1 = fraud, 0 = legitimate
actual_label = np.random.choice(
    [0, 1],
    size=n_transactions,
    p=[1 - fraud_rate, fraud_rate]
)

# 3. Generate semi-realistic fraud-risk scores
# Fraud cases generally receive higher scores, but there is overlap.
predicted_score = np.where(
    actual_label == 1,
    np.random.uniform(0.45, 1.00, n_transactions),
    np.random.uniform(0.00, 0.85, n_transactions)
)

# 4. Apply threshold
threshold = 0.70
predicted_label = (predicted_score >= threshold).astype(int)

# 5. Create dataset
transactions = pd.DataFrame({
    "transaction_id": transaction_id,
    "amount": amount,
    "actual_label": actual_label,
    "predicted_score": np.round(predicted_score, 3),
    "predicted_label": predicted_label
})

# 6. Calculate confusion matrix elements
TP = ((transactions["actual_label"] == 1) & (transactions["predicted_label"] == 1)).sum()
FP = ((transactions["actual_label"] == 0) & (transactions["predicted_label"] == 1)).sum()
FN = ((transactions["actual_label"] == 1) & (transactions["predicted_label"] == 0)).sum()
TN = ((transactions["actual_label"] == 0) & (transactions["predicted_label"] == 0)).sum()

# 7. Calculate metrics
accuracy = accuracy_score(transactions["actual_label"], transactions["predicted_label"])
precision = precision_score(transactions["actual_label"], transactions["predicted_label"], zero_division=0)
recall = recall_score(transactions["actual_label"], transactions["predicted_label"], zero_division=0)
f1 = f1_score(transactions["actual_label"], transactions["predicted_label"], zero_division=0)
auprc = average_precision_score(transactions["actual_label"], transactions["predicted_score"])

print("Confusion Matrix Elements")
print("TP:", TP)
print("FP:", FP)
print("FN:", FN)
print("TN:", TN)

print("\nMetrics")
print("Accuracy:", round(accuracy, 3))
print("Precision:", round(precision, 3))
print("Recall:", round(recall, 3))
print("F1 Score:", round(f1, 3))
print("AUPRC:", round(auprc, 3))

print("\nSample transactions")
print(transactions.head(10))
```

---

## 22. Optional Threshold Experiment

Students can repeat the calculation using different thresholds:

- 0.50
- 0.60
- 0.70
- 0.80
- 0.90

Then Students can observe:

- What happens to precision?
- What happens to recall?
- Does the model become more aggressive or more conservative?
- Which threshold is best for the business scenario?

### Expected interpretation

Lower threshold:

- more transactions flagged,
- higher recall,
- lower precision,
- more analyst workload.

Higher threshold:

- fewer transactions flagged,
- higher precision,
- lower recall,
- more missed fraud.

---



## 23. Key Takeaways for Students

- False positives create friction, cost, and analyst workload.
- False negatives create missed fraud, financial loss, and security risk.
- Precision measures alert quality.
- Recall measures fraud coverage.
- F1 balances precision and recall at a chosen threshold.
- Accuracy alone can be misleading when fraud is rare.
- AUPRC is preferred when positive cases are rare and the model is used to rank cases for review.
- Threshold choice is a business decision, not only a technical decision.

---

## 25. How This Connects to Exercise 2

In Exercise 2, students design an AI decision workflow.

This metrics exercise helps students to test whether the workflow is operationally useful.

For example, in a fraud review workflow, students should not only say:

> The model produces a fraud score.

Students should also explain:

- which threshold is used,
- how many cases are flagged,
- how many alerts are useful,
- how much fraud is missed,
- how analyst feedback is used,
- and which metrics are monitored after deployment.

This connects model evaluation to real financial decision-making.
