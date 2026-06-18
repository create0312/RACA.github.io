<div align="center">

# RACA: Rule-Aligned Collaborative Agents for Evidence-Grounded Breast Ultrasound Classification

**A rule-aligned multi-agent framework for transparent and clinically grounded breast ultrasound diagnosis.**

[![Paper](https://img.shields.io/badge/Paper-MICCAI%202026%20manuscript-2f6f9f)](#citation)
[![Task](https://img.shields.io/badge/Task-Breast%20Ultrasound%20Classification-bf5b2c)](#results)
[![Method](https://img.shields.io/badge/Method-Collaborative%20Agents-6a4c93)](#method-overview)
[![Code](https://img.shields.io/badge/Code-coming%20soon-lightgrey)](#release-plan)

Lingyu Chen<sup>&#42;</sup>, Yue Wang<sup>&#42;</sup>, Cheng Li, Lin Jin, Daoqiang Zhang, Hongen Liao, Fang Chen<sup>&dagger;</sup>, Qingjie Meng<sup>&dagger;</sup>

<sup>&#42;</sup> Equal contribution &nbsp;&nbsp; <sup>&dagger;</sup> Co-corresponding authors

[Paper](#citation) | [Method](#method-overview) | [Results](#results) | [Citation](#citation)

</div>

![RACA framework](Images/Fig1.PNG)


## Highlights

- **Rule-aligned diagnosis**: RACA anchors breast ultrasound (BUS) classification to standardized clinical descriptors such as shape, margin, echogenicity, and posterior features.
- **Collaborative agents**: The framework decomposes diagnosis into evidence-grounded perception, rule-aligned risk modeling, and critic-guided decision calibration.
- **Traceable evidence chain**: Each prediction is supported by auditable diagnostic evidence, consistency checks, malignancy risk estimates, and prior-case retrieval.
- **Memory-guided calibration**: A critic agent retrieves similar prior decision states and selectively refines uncertain predictions under reliability constraints.
- **Strong empirical performance**: RACA improves overall classification performance across BUSI, BUV, and BUSBRA benchmarks while preserving clinically interpretable decision patterns.

## Abstract

Breast cancer is highly prevalent worldwide. While breast ultrasound (BUS) serves as a primary screening modality, its reliable diagnostic interpretation remains inherently challenging. Most existing BUS classification methods prioritize direct image-to-label mapping and overall accuracy, neglecting the alignment with clinical diagnostic rules and decision traceability. Such limitations hinder their integration into rule-governed clinical workflows and diminish the trustworthiness of automated diagnostic decisions. Although agent-based frameworks offer a modular paradigm for organizing complex decision processes, their potential for rule-aligned and fully traceable BUS diagnosis remains largely unexplored. To address these challenges, we introduce **RACA, a Rule-Aligned Collaborative Agent framework for evidence-grounded BUS classification**. Our RACA introduces **a novel, three-stage framework driven by explicit clinical rules**. First, a perception module consolidates morphological and semantic evidence into **structured diagnostic evidence**. Then, a risk modeling agent aggregates the evidence under explicit clinical constraints to derive **a rule-aligned risk estimate** and an initial prediction. Finally, **a critic-guided calibration agent refines predictions through memory-based retrieval**. Extensive experiments on three BUS benchmarks  **demonstrate the superiority of RACA over representative classification and medical agent-based frameworks**. Comprehensive inter pretability analyses and case demonstrations **validate that its traceable decision pathway aligns with clinical diagnosis**.

## Method Overview

RACA contains three collaborative stages.

### 1. Evidence-Grounded Perception

Given a breast ultrasound image, RACA first localizes the lesion ROI using detection and segmentation tools. The perception stage then converts the ROI into structured diagnostic evidence through three agents:

- **Morphological Measurement Agent**: extracts quantitative and acoustic features such as compactness, circularity, margin sharpness, echo contrast, and posterior ratio.
- **Semantic Interpretation Agent**: uses a vision-language model to generate diagnostic descriptions, then parses them into clinical attributes.
- **Auditable Diagnostic-Attribute Fusion Agent**: fuses structural and semantic evidence according to attribute consistency and ROI reliability.

The resulting evidence is organized in a diagnostic space:

```text
D = {shape, margin, echogenicity, posterior}
```

### 2. Rule-Aligned Risk Modeling

The risk modeling agent maps fused evidence into a continuous malignancy risk estimate. High-risk clinical attributes, consistency penalties, and evidence instability are explicitly incorporated into the risk computation. This produces:

- an initial malignancy risk score,
- an initial benign/malignant prediction,
- a compact decision state for downstream calibration.

### 3. Critic-Guided Decision Calibration

The critic agent retrieves top-k prior cases from a memory bank using decision-state similarity. It computes a memory-based malignancy risk and admits it only when the neighborhood evidence is reliable. Multiple calibration agents then perform rule-constrained updates, including:

- dual-threshold veto/rescue,
- top-k rescue for likely false negatives,
- optional plug-in re-check using auxiliary confidence signals.

The final prediction is selected by a rule-constrained policy that favors clinically consistent updates.

## Interpretability

![RACA interpretability](Images/Fig3.PNG)

RACA is designed to expose how each decision is made. The paper analyzes whether diagnostic evidence aligns with guideline-based features, whether benign and malignant cases rely on clinically plausible cues, and whether evidence patterns remain stable across sampling ratios.

Key observations:

- Guideline-based evidence closely aligns with the structured evidence learned by RACA.
- Malignant predictions are mainly driven by invasive morphology such as eccentricity, edge, and margin-related cues.
- Benign predictions rely more on acoustic intensity and posterior-related evidence.
- The hierarchical evidence layout maps low-level perception signals to high-level decision gates, making each prediction traceable.

## Results

RACA is evaluated on three public breast ultrasound datasets: **BUSI**, **BUV**, and **BUSBRA**. All methods use the same 7:2:1 train/validation/test split for fair comparison.

### Main Comparison

| Dataset | Best competing ACC | RACA ACC | RACA PRE | RACA REC | RACA F1 |
| --- | ---: | ---: | ---: | ---: | ---: |
| BUSI | 0.7500 | **0.8088** | 0.7742 | 0.8000 | **0.7869** |
| BUV | 0.7647 | **0.8235** | 0.7143 | **0.8333** | **0.7692** |
| BUSBRA | 0.7979 | **0.8085** | 0.7273 | 0.6557 | 0.6897 |

RACA shows the strongest overall accuracy across all three datasets and achieves large gains over general medical agent baselines, highlighting the importance of explicitly encoding domain-specific diagnostic rules.

### Ablation Study

| Configuration | BUSI ACC | BUSI F1 | BUSBRA ACC | BUSBRA F1 |
| --- | ---: | ---: | ---: | ---: |
| EGP + rule-aligned risk modeling | 0.6176 | 0.5000 | 0.5212 | 0.3571 |
| + top-k rescue calibration | 0.7054 | 0.6374 | 0.6223 | 0.3860 |
| + dual-threshold calibration | 0.7941 | 0.7667 | 0.7713 | 0.5474 |
| Full RACA | **0.8088** | **0.7869** | **0.8085** | **0.6897** |

The step-wise improvement supports the central design: memory-based calibration improves rule-based inference rather than replacing it.

## Datasets

The experiments use publicly available breast ultrasound datasets:

- **BUSI**
- **BUV**
- **BUSBRA**

Please download each dataset from its official source and follow the corresponding license and usage terms. Dataset preparation scripts will be added after code release.

## Release Plan

- [ ] Inference code
- [ ] Data split and preprocessing scripts
- [ ] Memory bank construction
- [ ] Training and validation configuration
- [ ] Model checkpoints
- [ ] Demo examples

## Project Structure

The planned repository layout is:

```text
RACA/
|-- assets/
|   |-- raca_framework.png
|   |-- raca_interpretability.png
|-- configs/
|-- datasets/
|-- raca/
|   |-- agents/
|   |-- perception/
|   |-- risk_modeling/
|   |-- calibration/
|-- scripts/
|-- tools/
|-- README.md
```

## Citation

If RACA is useful for your research, please cite the paper. The official BibTeX will be updated after publication.

```bibtex
@misc{chen2026raca,
  title  = {RACA: Rule-Aligned Collaborative Agents for Evidence-Grounded Breast Ultrasound Classification},
  author = {Chen, Lingyu and Wang, Yue and Li, Cheng and Jin, Lin and Zhang, Daoqiang and Liao, Hongen and Chen, Fang and Meng, Qingjie},
  year   = {2026},
  note   = {Manuscript}
}
```

## Acknowledgements

This work was supported by the National Natural Science Foundation of China Grants 62271246 and 82572314, and the Science and Technology Commission of Shanghai Municipality Grant 24511104100.

## Disclaimer

This repository is intended for research use only. RACA is not a certified medical device and should not be used for clinical diagnosis without appropriate validation, regulatory review, and expert supervision.

