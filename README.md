# Named Entity Recognition - FIGER Dataset

---

## Dataset
**FIGER Data Split**:
- Train: 1,248,761 samples
- Test: 278 samples
- Dev: 9,967 samples

---
## Folder Structure

- The preprocessed data is stored in the folder: /preprocessed data which contains the test_data_preprocessed.json, dev_data_preprocessed.json etc.

- The preprocessing scripts are in the folder: /preprocessing

**Code for LSTM and BERT**:
- **LSTM:**

- Folder: /code/LSTM_NER
- File: lstm_ner.py

    - To train call the train() function.
    - The test() function is also included to test for one example.
    - All the predefined parameters are included. (epochs etc.)
    - Make sure the files used in this code, train_data_preprocessed.json etc. exist in the same folder.

-**BERT:**

- Folder: /code/BERT_NER
- File: bert_ner.py

    - To train call the train() function.
    - The test() function is also included to test for one example.

## Implementation Details

### Data Preprocessing
- For tokens with multiple tags, the tag with maximum length was selected as the gold label.
- Preprocessed files:
  - `train_data_preprocessed.json`
  - `test_data_preprocessed.json`
  - `dev_data_preprocessed.json`  
  (Located in `/preprocessed data` folder)

**Evaluation Metrics**:
- Loose Macro
- Loose Micro  
  (Implementation in `/preprocessing/evaluation_of_ner.py`)

---

## Model Performances

### 1. Bi-directional LSTM
- **Hyperparameters**:
  - Loss: Categorical cross-entropy
  - Optimizer: Adam (LR=0.001)
  - Batch Size: 128
  - Epochs: 200

**Results**:
| Metric       | Loose Micro       | Loose Macro       |
|--------------|-------------------|-------------------|
| Precision    | 0.871             | 0.979             |
| Recall       | 0.689             | 0.959             |
| F1           | 0.769             | 0.969             |
- **Accuracy**: 0.95 (6598/6944 tags matched)

---

### 2. BERT (DistilBERT)
- **Model**: `distilbert-base-uncased`
- **Hyperparameters**:
  - Learning Rate: 1e-4
  - Weight Decay: 1e-5
  - Batch Size: 16
  - Epochs: 30

**Results**:
| Metric       | Loose Micro       | Loose Macro       |
|--------------|-------------------|-------------------|
| Precision    | 0.776             | 0.970             |
| Recall       | 0.759             | 0.968             |
| F1           | 0.767             | 0.969             |
- **Accuracy**: 0.98 (6871/6944 tags matched)

---

## Error Analysis
**Common Issues**:
1. **Multilabel Misclassification**:
   - `/organization/educational_institution` → `/organization`
   - `/person/author` → `/person`
2. **OOV (Out-of-Vocabulary) Handling**:
   - Treated as `<OOV>` token; rule-based approaches could improve performance.

---

## Key Findings
1. **Metric Selection**:
   - Macro F1 (95%+) better reflects performance than Micro F1 (76%) due to class imbalance (86% "O" class).
2. **Model Comparison**:
   - **BERT** outperforms LSTM with fewer epochs (30 vs 200).

---

## Challenges Faced
| Model  | Major Challenges                          |
|--------|-------------------------------------------|
| LSTM   | High system requirements (15GB RAM)       |
| BERT   | Long training times (2-3 days)            |

---

## Conclusion
BERT demonstrated superior performance for NER tasks, achieving high accuracy with fewer training epochs compared to LSTM. Future work could focus on:
- Better OOV handling
- Class imbalance mitigation

**Input and Output Available**: `/preprocessed data` and `/output` folders contain preprocessed data and predictions.
