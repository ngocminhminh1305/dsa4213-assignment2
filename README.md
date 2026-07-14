# Language Modeling using RNN and LSTM  
**DSA4213 Assignment 2 – National University of Singapore**

## Overview

This repository contains the implementation for **Assignment 2** of the NUS **DSA4213** module.  
The objective of this assignment is to build and evaluate **recurrent neural network (RNN)–based language models** for next-token prediction.

Two recurrent architectures are implemented and compared:
- **Vanilla RNN**
- **Long Short-Term Memory (LSTM)**

The models are trained on *An Inquiry into the Nature and Causes of the Wealth of Nations* by **Adam Smith**, and evaluated using **perplexity**. The assignment also includes ablation studies on **dropout** and **context length**, as well as qualitative text generation analysis.

---

## Repository Structure
├── code.ipynb # Jupyter / Google Colab notebook with all experiments

├── report.pdf # Written report (PDF)

└── README.md # This file


All experiments are implemented entirely in the notebook.

---

## Dataset

- **Source:** Project Gutenberg  
- **Text:** *An Inquiry into the Nature and Causes of the Wealth of Nations* – Adam Smith  
- **Task:** Word-level language modeling (next-token prediction)

### Preprocessing
The following preprocessing steps are performed in the notebook:
- Removal of license and metadata text
- Text normalization (spacing and punctuation)
- Word-level tokenization
- Introduction of special tokens:
  - `<pad>` – padding
  - `<unk>` – unknown words
  - `<bos>` – beginning of sequence
  - `<eos>` – end of sequence
- Train / validation / test split:
  - 80% training
  - 10% validation
  - 10% test  
  (order preserved to reflect sequential prediction)

The final processed corpus contains:
- **441,551 tokens**
- **10,359 unique words**

---

## Models

### Vanilla RNN
- Processes tokens sequentially with a hidden state
- Simple and computationally efficient
- Prone to vanishing gradients on long sequences

### LSTM
- Enhanced recurrent architecture with gating mechanisms
- Designed to capture longer-range dependencies
- Higher model capacity and computational cost

---

## Training Setup

All models are implemented in **PyTorch** and trained with mini-batch optimization.

**Common hyperparameters:**
- Batch size: 64  
- Embedding size: 128  
- Hidden size: 256  
- Number of layers: 2  
- Dropout: 0.2 (unless stated otherwise)  
- Optimizer: AdamW  
- Learning rate: 0.001  
- Gradient clipping: 1.0  
- Maximum epochs: 10  

**Early stopping:**
- Patience = 2
- Monitored on validation perplexity

**Sequence length:** 128 tokens (default)

All experiments were run on **Google Colab (Tesla T4 GPU)**.

---

## Evaluation Metric

- **Perplexity (PPL)**  
Lower perplexity indicates better predictive performance and greater confidence in the model’s output distribution.

Both **validation** and **test** perplexity are reported.

---

## Results

### Validation and Test Perplexity

| Model | Validation PPL | Test PPL |
|------|---------------|----------|
| RNN | 1515.10 | 1065.74 |
| LSTM | 9376.26 | 6937.49 |

Despite its simplicity, the RNN outperformed the LSTM on this dataset. The added complexity of the LSTM led to overfitting and unstable convergence, likely due to the limited and homogeneous nature of the data.

---

## Ablation Studies

### Dropout Ablation

Dropout values tested: **0.0** and **0.2**

Introducing dropout significantly improved generalization for both models. Without dropout, validation and test perplexities increased dramatically, indicating severe overfitting. The RNN benefited the most from dropout regularization.

---

### Context Length Ablation

Sequence lengths tested: **128** and **256**

Increasing the context length worsened performance and doubled training time for both models. Shorter contexts (128 tokens) provided a better balance between stability, efficiency, and generalization.

---

## Text Generation

Both models were used to generate text using different sampling temperatures (T = 0.7, 1.0, 1.3).

**Observations:**
- Generated text was grammatically plausible but often incoherent
- The RNN produced shorter, repetitive outputs that remained loosely on-topic
- The LSTM generated longer sentences but frequently drifted into memorized or nonsensical fragments
- Neither model successfully captured Adam Smith’s core economic principles

---

## How to Reproduce

All experiments are contained in **`code.ipynb`**.

1. Open `code.ipynb` in **Google Colab**
2. Enable GPU runtime
3. Run all cells from top to bottom

The notebook will:
- Preprocess the dataset
- Train RNN and LSTM models
- Perform ablation studies
- Generate evaluation metrics, plots, and sample text outputs

No additional setup is required.

---

## Notes

- All quantitative results and figures are reported in `report.pdf`
- Early stopping was essential to prevent overfitting
- Validation metrics, rather than training loss, guided model selection
- This assignment demonstrates that simpler models can outperform more complex ones in low-data settings
