# 🧠 VQA-Med 2025: Visual Question Answering on Skin Conditions using ViT + LBP + BERT

Welcome to repository for the **ImageCLEF VQA-Med 2025 Challenge**, where we developed a **multi-modal deep learning pipeline** to solve **medical Visual Question Answering (VQA)** for dermatological cases. This repository contains the full codebase, methodology, and training scripts for our submission.

<div align="center">
  <img src="https://img.shields.io/badge/Model-ViT%2BLBP%2BBERT-blue.svg" />
  <img src="https://img.shields.io/badge/Task-Medical%20VQA-red.svg" />
  <img src="https://img.shields.io/badge/Challenge-ImageCLEF%20VQA--Med%202025-brightgreen.svg" />
  <img src="https://img.shields.io/badge/Rank-Top%2010%25-yellow.svg" />
</div>

---

## 🚀 Project Overview

Medical VQA is a challenging intersection of computer vision and natural language understanding. Our task was to answer clinical questions based on dermatological images and associated questions.

We approached this with a **multi-stage pipeline** combining powerful **image features from ViT and LBP** with a **pre-trained text generator (T5)** to generate accurate and contextually aware answers.

---

## 📦 Dataset

- **Source**: [ImageCLEF VQA-Med 2025 dataset](https://ai4media-bench.aimultimedialab.ro/competitions/62/)
- **Structure**:
  - 4,000+ dermatology cases
  - Each case includes:
    - An image of the skin lesion or body part
    - A set of natural language clinical questions (e.g., *"What color is the lesion?"*)
    - A ground truth answer for evaluation

- **Annotations**:
  - Multi-label format
  - JSON-based
  - Questions include classification, region-based, and semantic reasoning types

---

## 🧠 Model Architecture

We used a **hybrid deep learning architecture** combining:

### 🖼️ Image Feature Extractor
1. **Vision Transformer (ViT-B/32)**
   - Captures global features and attention across patches
   - Pre-trained on ImageNet
2. **Local Binary Patterns (LBP)**
   - Captures micro-texture and contrast patterns in skin
   - Enhances lesion region encoding

Final image embedding = Concatenation of `ViT` features + `LBP` histogram vector.

### 📖 Text Feature Extractor
## 3. BERT (Bidirectional Encoder Representations from Transformers)

- **Task:** "Question → Answer" generation (typically via classification or span prediction)
- **Input:** `[Question] + [Image Features]` (concatenated or fused as a joint representation)
- **Output:** Text answer derived from the most relevant span or token-level prediction

> **Note:** In multimodal setups (e.g., VisualBERT, LXMERT), BERT is extended to handle visual inputs by fusing image features with textual tokens. The model jointly encodes both modalities to generate the most appropriate natural language answer.



---

## 🛠️ Training Strategy

- **Loss Function**: CrossEntropy Loss on tokenized answer sequences
- **Optimizer**: AdamW
- **Learning Rate**: \( 1 \times 10^{-4} \)
- **Batch Size**: 16
- **Epochs**: 10
- **Augmentations**:
  - Random rotations, flips, normalization on images
- **Training Flow**:
  1. Extract image features using ViT + LBP
  2. Prepend these to tokenized questions
  3. Feed into T5 for training on answer sequences

---

## 📊 Evaluation Metrics

Evaluation is done via **soft accuracy** on partial matches:

**Soft Accuracy** = `|Prediction ∩ Ground Truth| / max(|Prediction|, |Ground Truth|)`

A score like `0.625` indicates partial correctness (e.g., 5 correct labels out of 8 expected).

Metrics reported:
- Per-question accuracy (CQID010, CQID011, ..., CQID036)
- Average accuracy across all clinical question types
- Total number of evaluated cases

---

## 🏆 Results

| Metric                    | Score |
|---------------------------|-------|
| Overall Soft Accuracy     | 56.78% |
| Best CQID Performance     | CQID020 ("label of affected area") |
| Rank                | 12|

---




## 💡 Key Highlights

✅ Hybrid feature fusion (ViT + LBP)  
✅ Natural language generation with T5  
✅ Clinical insight-driven question filtering  
✅ Domain-specific post-processing on answers

---

## 🧪 Run it Yourself

```bash
# Clone this repo
git clone https://github.com/yourusername/imageclef-vqa-med-2025.git
cd imageclef-vqa-med-2025

# Install dependencies
pip install -r requirements.txt

# Run notebook or training script
jupyter notebook Imageclef_vqa_2025.ipynb

```





## 📚 Citation

If you use this work or the dataset, please cite the official CLEF proceedings:

```bibtex
@inproceedings{imageclef2025,
  title     = {Overview of the ImageCLEF 2025 Visual Question Answering Task},
  author    = {Abacha, Asma Ben and Hasan, Sadid A. and Demner-Fushman, Dina and Müller, Henning and Vasuki, V. and Liu, Xiaoling and others},
  booktitle = {Working Notes of CLEF 2025 – Conference and Labs of the Evaluation Forum},
  year      = {2025},
  address   = {Avignon, France},
  publisher = {CEUR Workshop Proceedings}
}

