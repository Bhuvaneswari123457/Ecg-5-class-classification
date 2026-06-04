# Ecg-5-class-classification



# Five-Class Classification of ECG Signals Using TQWT and Machine Learning

This repository contains the implementation of a machine learning framework to automatically categorize electrocardiogram (ECG) signals into five distinct cardiac conditions using **Tunable Q-factor Wavelet Transform (TQWT)** for feature extraction.

The model classifies ECG signals into:

* **NORM:** Normal Rhythm
* **CD:** Cardiomyopathy
* **HYP:** Hypertrophy
* **MI:** Myocardial Infarction
* **STTC:** ST-T changes

---

## 📌 Project Overview

Manual interpretation of multi-lead ECG signals is highly subjective, time-consuming, and prone to clinical error. This project introduces an automated processing pipeline that extracts localized time-frequency characteristics from individual ECG leads using TQWT. These features are then classified using both a custom **1D Convolutional Neural Network (CNN)** and an ensemble **Random Forest Classifier** to assess comparative diagnostic performance.

---

## 🛠️ Methodology & System Architecture

The pipeline consists of the following consecutive stages:

1. **Preprocessing:** Segmenting individual leads from the raw 12-lead ECG signals.
2. **Feature Extraction (TQWT):** Applying Tunable Q-factor Wavelet Transform to decompose each signal lead into frequency sub-bands, capturing subtle temporal and spectral patterns.
3. **Data Splitting:** Partitioning the dataset into training, validation, and testing sets.
4. **Model Training:** Training both a deep 1D-CNN and a Random Forest Classifier on the extracted features.
5. **Evaluation:** Testing the models on isolated test sets using metrics like Precision, Recall, F1-Score, and Accuracy.

### System Block Diagram

```text
[Load Dataset & Preprocess] ──> [Individual Lead Feature Extraction: TQWT]
                                                   │
                                                   ▼
[Results Analysis] <── [Model Evaluation] <── [Model Training] <── [Data Splitting] <── [Model Building]

```

---

## 📊 Dataset Description

* **Source:** Based on the benchmark **PTB-XL** dataset.
* **Size:** Evaluated utilizing a large-scale subset of **21,799 samples**.
* **Signal Layout:** Standard 12-lead ECG layouts containing comprehensive multi-angle cardiac electrical pathways.

---

## 🧠 Model Architectures

### 1D Convolutional Neural Network (CNN)

The custom sequential CNN design consists of alternating 1D convolutional layers, batch normalization, max pooling, and dropout regularization layers:

| Layer (type) | Output Shape | Param # |
| --- | --- | --- |
| **Conv1D** (256 filters, kernel=5, ReLU) | (None, 1000, 256) | 1,792 |
| **BatchNormalization** | (None, 1000, 256) | 1,024 |
| **MaxPooling1D** | (None, 500, 256) | 0 |
| **Conv1D** (128 filters, kernel=5, ReLU) | (None, 500, 128) | 196,736 |
| **BatchNormalization** | (None, 500, 128) | 512 |
| **MaxPooling1D** | (None, 250, 128) | 0 |
| **Conv1D** (64 filters, kernel=5, ReLU) | (None, 250, 64) | 49,216 |
| **BatchNormalization** | (None, 250, 64) | 256 |
| **MaxPooling1D** | (None, 125, 64) | 0 |
| **Flatten** | (None, 8000) | 0 |
| **Dense** (ReLU) | (None, 64) | 512,064 |
| **Dropout** (0.5) | (None, 64) | 0 |
| **Dense** (ReLU) | (None, 64) | 4,160 |
| **Dropout** (0.5) | (None, 64) | 0 |
| **Dense** (Softmax) | (None, 5) | 325 |

* **Optimizer:** Adam (Learning Rate = $0.0001$)
* **Loss Function:** Categorical Cross-Entropy

### Random Forest Classifier

* Built as an ensemble of Decision Trees trained on bootstrapped feature sets.
* Evaluated both on individual leads and combined-lead matrices.

---

## 📈 Performance Results

### Lead-Wise Classification Accuracy Comparison

The models were evaluated lead-by-lead, as well as on combined configurations:

| Lead No. | Lead Name | CNN Accuracy | Random Forest Accuracy |
| --- | --- | --- | --- |
| 0 | I | 56.23% | 61.19% |
| 1 | II | 57.22% | 63.96% |
| 2 | III | 56.39% | 63.34% |
| 3 | aVR | 59.25% | 62.60% |
| 4 | aVL | 55.68% | 61.90% |
| 5 | aVF | 55.19% | 62.91% |
| 6 | V1 | 57.19% | 62.76% |
| 7 | V2 | 57.71% | 61.96% |
| 8 | V3 | 58.70% | **66.94%** |
| 9 | V4 | 56.82% | 66.76% |
| 10 | V5 | 59.80% | 66.48% |
| 11 | V6 | **60.42%** | 65.99% |
| **All Leads** | **ALL** | **44.88%** | **71.22%** |
| **Selected** | **II, aVR, aVF, V6** | **53.61%** | **66.20%** |

### Key Insights

* **Ensemble Learning Superiority:** The Random Forest Classifier trained on all leads achieved the peak classification accuracy of **71.22%**.
* **Deep Learning Challenges:** The CNN alone struggled to capture the comprehensive 12-lead multi-signal context simultaneously, resulting in a lower accuracy of **44.88%** when utilizing all 12 leads together.
* **TQWT Impact:** Introducing TQWT features significantly improved the baseline raw performance of both networks by capturing robust time-frequency information.

---

## 👥 Contributors (Group 5)

* **Harsh Benuskar** (Roll No: 121CS0149) — PPT Making
* **Chodisetty Bhuvaneswari** (Roll No: 121CS0151) — Lead Coding & Development
* **Akash Kumar Biswal** (Roll No: 121CS0152) — Report Structure & Writing
* **Gourav Kumar Biswal** (Roll No: 121CS0153) — Coding ,PPT, & Report

**Submitted To:** Dr. Puneet Kumar Jain, Department of Computer Science & Engineering, NIT Rourkela
