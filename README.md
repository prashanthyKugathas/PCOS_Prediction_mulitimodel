# InnoX — Multimodal AI System for PCOS Prediction

## Overview
InnoX is a decision-level multimodal AI system for predicting Polycystic Ovary Syndrome (PCOS) by combining ultrasound image analysis and tabular clinical data. The design prioritizes transparency, reproducibility, bias mitigation, and clinical applicability. Image and tabular models are trained independently and fused at inference time due to the absence of patient-wise paired multimodal datasets.

## Key Features
- Deep learning for ultrasound image classification using EfficientNet-B0 (ImageNet pretrained).
- Classical machine learning models (Logistic Regression, Random Forest, Gradient Boosting) for clinical/tabular features.
- Decision-level fusion: weighted combination of unimodal predictions at inference time.
- Emphasis on reproducible preprocessing, clear validation, and bias/risk discussion.

## Abstract
Polycystic Ovary Syndrome (PCOS) is a common endocrine disorder with diagnostic challenges caused by subjective ultrasound interpretation and heterogeneous clinical presentations. InnoX integrates ultrasound image analysis and clinical data at the decision level, using an EfficientNet-B0 image model and classical tabular classifiers. The system avoids automated AutoML pipelines, focuses on interpretability, and follows strict data-handling practices.

## Datasets (Open-source)
Datasets referenced and used for experiments (public):

- PCOS clinical dataset (tabular): https://www.kaggle.com/datasets/michaelmendiolasy/pcos-clinical-dataset
- PCOS tabular alternative: https://www.kaggle.com/datasets/vishnuvamsi05799/polycystic-ovary-syndromepcos?select=PCOS_Dataset
- Ultrasound images (1): https://www.kaggle.com/datasets/shnotweta/2000-images-of-ultrasound-for-pcos
- Ultrasound images (2): https://www.kaggle.com/datasets/anaghachoudhari/pcos-detection-using-ultrasoundimages

All datasets used are public and open-source. Users must follow each dataset's license and citation requirements when reusing data.

## Methodology (Summary)

Data preprocessing
- Image: speckle reduction (Gaussian blur), CLAHE contrast enhancement, edge sharpening, resize to 224×224, normalization to [0,1], and augmentation for training only.
- Tabular: remove identifiers, median imputation for numerics, mode imputation for categoricals, one-hot encoding, and StandardScaler fit on training data.

Models and training
- Image model: EfficientNet-B0 pretrained on ImageNet with a custom classification head (GAP, batch norm, dropout, dense layers, sigmoid output). Two-phase training: frozen backbone then conservative fine-tuning (last 10 layers).
- Tabular models: Logistic Regression baseline, Random Forest (constrained depth), Gradient Boosting. Trained with stratified 5-fold cross-validation.
- Class imbalance handled via class weights and stratified sampling. Early stopping and learning-rate scheduling applied where relevant.

Decision-level fusion
- Independently trained unimodal models are combined at inference by weighted averaging of prediction probabilities. Weights can be tuned on validation data.

## Validation & Metrics
- Strict separation of train/validation/test sets with stratified sampling.
- Metrics tracked: Accuracy, Precision, Recall, F1-score, ROC-AUC, confusion matrices, and ROC curves.
- Final reported metrics are generated from the provided notebook during execution.

## Results and Error Analysis
Quantitative metrics and plots are generated in the notebook. Common error modes observed include false positives from noise-like cyst patterns and false negatives in early-stage PCOS.

## Limitations
- Unpaired multimodal datasets (no patient-wise image+clinical pairs).
- Limited dataset diversity and equipment variability.
- No external multicenter validation yet.
- Explainability tools (Grad-CAM, SHAP) are not included in the current submission but recommended for future work.

## Future Work
- Collect patient-wise paired multimodal datasets and perform early/feature-level fusion experiments.
- Add explainability (Grad-CAM for images, SHAP for tabular models).
- Perform multi-center external validation and prospective clinical trials.

## Usage
1. Open and run the notebook `InnoX7_Notebook.ipynb` to reproduce data processing, training, and evaluation pipelines.
2. Ensure required Python packages are installed (see `requirements.txt` in the notebook or environment). Typical packages: `torch`/`tensorflow` (depending on implementation), `efficientnet` (or `timm`), `scikit-learn`, `pandas`, `numpy`, `opencv-python`, `matplotlib`/`seaborn`.

Example commands (adjust environment manager as needed):

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
jupyter lab
```

## Reproducibility Notes
- Random seeds should be fixed for dataset splits, augmentation, and model initialization.
- Preprocessing and scaler objects must be serialized and re-used for inference to prevent data leakage.
- Reported metrics should always be computed on untouched test sets.

## References
See the project report and the following selected references included in the report:

1. Yazhini Karthik et al., "Polycystic Ovary Syndrome Prediction through CNN Based Image Analysis"
2. G. Mohan et al., "Harnessing Deep Neural Networks for Accurate PCOS Diagnosis from Medical Images"
3. Poonam Moral et al., "CystNet: An AI Driven Model for PCOS Detection..."

## Contact
For questions or collaboration, open an issue in this repository or contact the project lead.

---
Generated from report and notebook: see `InnoX7_Notebook.ipynb` for executable experiments.
