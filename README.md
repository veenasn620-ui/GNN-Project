# ECG Signal Imputation using MIT-BIH Arrhythmia Database

This project explores different ways to reconstruct missing parts of ECG signals using machine learning. The main goal is to understand how well different models can recover important signal patterns, especially in clinically meaningful regions like QRS complexes.

The work is structured into multiple stages, starting from data preprocessing and baseline methods, and moving towards a custom transformer-based model with additional experiments and analysis.

---

## Project Structure

The raw dataset is not included since it’s publicly available on PhysioNet. The notebook downloads it automatically when you run it.

---

## What this project does

### 1. Data Preprocessing

The ECG data is taken from the MIT-BIH Arrhythmia Database and processed before training:

- Bandpass filtering to remove noise
- Removing low-quality signal segments
- Splitting data by patient to avoid leakage
- Normalizing signals per patient
- Applying data augmentation (noise, shifts, etc.)
- Creating realistic missing-data masks

---

### 2. Baseline Models

I tested a range of approaches for imputing missing ECG values:

- Simple methods like interpolation and mean filling
- Deep learning models like BiLSTM and BiGRU
- BRITS for time-series imputation

These models were evaluated only on the missing parts of the signal.

One key observation:  
Traditional interpolation methods struggle with sharp ECG features like QRS peaks, even if they perform okay on flat regions.

---

### 3. Transformer Model (Main Contribution)

I implemented a custom transformer inspired by SAITS, designed specifically for this task.

Some key ideas:

- Multi-scale feature extraction to capture both fine and coarse signal patterns
- A loss function that gives more importance to high-amplitude regions (like QRS peaks)
- A training strategy that gradually increases masking difficulty

This model achieved the best performance overall, especially in reconstructing important signal regions.

---

### 4. Ablation Studies

To understand what actually matters, I tested different design choices:

- Masking difficulty
- Model depth
- Patch sizes
- Loss weighting

This helped identify which parts of the model contribute most to performance.


## Setup

Install dependencies:

```bash
pip install torch numpy pandas matplotlib seaborn scipy wfdb scikit-learn tqdm
pip install pypots==0.4.0
