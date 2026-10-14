# 🫀 HRV-Based Cognitive Monitoring in Parkinson's Disease

![Python](https://img.shields.io/badge/Python-3.9+-blue)
![OpenCV](https://img.shields.io/badge/OpenCV-Image%20Processing-green)
![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-orange)
![Status](https://img.shields.io/badge/status-proof%20of%20concept-yellow)

## 📌 Overview

Cognitive decline is common in Parkinson's disease, but tracking it usually means repeated clinical assessments. This project explores a cheaper signal: **heart rate variability (HRV)**.

Parkinson's affects the autonomic nervous system, and reduced HRV has been linked to both autonomic dysfunction and cognitive impairment. The idea here is simple: extract HRV from a standard ECG printout or image, then use it to flag patients whose cognitive state may be declining.

The pipeline goes from a scanned ECG image to R-R intervals, then to HRV metrics (SDNN and RMSSD), and finally to a cognitive state prediction using a hybrid of **Support Vector Machine (SVM)** classification and **Linear Regression**.

## 🔄 Pipeline

```
ECG image ─> Preprocessing ─> R-peak detection ─> R-R intervals ─> HRV metrics ─> SVM + Linear Regression ─> Cognitive state
```

## 🧪 Methodology

### 1. ECG Image Processing
- Load the ECG image and convert RGBA to grayscale
- Gaussian blur to reduce noise
- Canny edge detection to isolate the waveform
- Crop the region of interest and extract contours

### 2. R-Peak Detection and R-R Intervals
- Identify R-peaks from the contour coordinates
- Compute R-R intervals from the distance between peaks
- Convert pixel distances to seconds using the ECG grid scale
- Export intervals to CSV

### 3. HRV Feature Extraction
- Missing value handling and outlier detection (values beyond 3 standard deviations)
- **SDNN**: standard deviation of R-R intervals, reflecting overall variability
- **RMSSD**: root mean square of successive differences, reflecting short-term (parasympathetic) variability
- Comparison against reference ranges for healthy adults

### 4. Cognitive State Modelling
- **SVM (linear kernel)** classifies cognitive state from SDNN and RMSSD
- **Linear Regression** models the relationship between SDNN and RMSSD
- Tested on 20 data points: 1 real patient ECG processed through the full pipeline, plus 19 synthetic HRV data points generated to test the classifier

## 📊 Results

**SVM classification**

| Metric | Score |
|--------|-------|
| Accuracy | 1.00 |
| Precision | 1.00 |
| Recall | 1.00 |
| F1-score | 1.00 |

**Linear regression (RMSSD from SDNN)**

| Metric | Value |
|--------|-------|
| Coefficient | 0.5071 |
| Intercept | 0.0215 |
| Mean Squared Error | 0.000011 |

> ⚠️ Only 1 of the 20 data points comes from a real patient ECG. The other 19 are synthetic, created to test whether the classifier and regression work end to end. The test split has just 4 samples, so these scores show the pipeline runs correctly, not that it is clinically accurate. Real patient data and cross-validation are the next step.

## 📁 Project Structure

```
├── Cognitive_Monitoring_HRV_SVM_LR.ipynb   # Full pipeline notebook
├── data/
│   ├── rr_intervals.csv                    # Extracted R-R intervals
│   └── SDNN&RMSSD.csv                      # HRV features (1 real patient + 19 synthetic)
├── images/                                 # Sample (anonymized) ECG images
├── requirements.txt
└── README.md
```

## 🚀 Getting Started

```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
pip install -r requirements.txt
jupyter notebook Cognitive_Monitoring_HRV_SVM_LR.ipynb
```

**requirements.txt**
```
numpy
pandas
opencv-python
pillow
matplotlib
seaborn
scipy
scikit-learn
```

## 🛠️ Tech Stack

| Area | Tools |
|------|-------|
| Language | Python |
| Image processing | OpenCV, Pillow |
| Data processing | Pandas, NumPy, SciPy |
| Modelling | scikit-learn (SVM, Linear Regression) |
| Visualization | Matplotlib, Seaborn |

## 🔭 Future Work

- Replace synthetic data with real patient ECGs, then use cross-validation
- More robust R-peak detection (e.g. Pan-Tompkins on digitized signals)
- Frequency-domain HRV features (LF, HF, LF/HF ratio)
- Validation against standard cognitive scores such as MoCA or MMSE
- Combining HRV with other biomarkers and brain imaging data

## 🔒 Data Privacy

All ECG images and patient data used in this repository are anonymized. No personally identifiable information is included.

## 👥 Authors

**Sushmitha K** 
**Devasena S** 
