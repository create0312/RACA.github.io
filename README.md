<div align="center">

# RACA: Rule-Aligned Collaborative Agents for Evidence-Grounded Breast Ultrasound Classification

**A rule-aligned multi-agent framework for transparent and clinically grounded breast ultrasound diagnosis.**

[![Paper](https://img.shields.io/badge/Paper-MICCAI%202026%20manuscript-2f6f9f)](#citation)
[![Task](https://img.shields.io/badge/Task-Breast%20Ultrasound%20Classification-bf5b2c)](#results)
[![Method](https://img.shields.io/badge/Method-Collaborative%20Agents-6a4c93)](#method-overview)
[![Code](https://img.shields.io/badge/Code-repository-lightgrey)](https://github.com/MedAI-26/RACA.github.io)

Lingyu Chen<sup>&#42;</sup>, Yue Wang<sup>&#42;</sup>, Cheng Li, Lin Jin, Daoqiang Zhang, Hongen Liao, Fang Chen<sup>&dagger;</sup>, Qingjie Meng<sup>&dagger;</sup>

<sup>&#42;</sup> Equal contribution &nbsp;&nbsp; <sup>&dagger;</sup> Co-corresponding authors

[Paper](#citation) | [Method](#method-overview) | [Experimental Setup](#experimental-setup) | [Results](#results) | [Citation](#citation)

</div>

<a id="fig1"></a>

![RACA framework](Images/Fig1.PNG)

<p align="justify">
<b>Fig. 1.</b> Overview of the proposed RACA. (a) Lesion ROI is localized using a combination of existing detection and segmentation tools. (b-d) In Stage I, three perception agents extract and consolidate morphological and semantic evidence into structured diagnostic evidence. (e) In Stage II, diagnostic evidence is evaluated to derive a continuous malignancy risk estimate and an initial prediction via a rule-aligned risk modeling agent. (f) In Stage III, a critic-guided decision calibration agent further refines the prediction through prior-case retrieval.
</p>

## Highlights

- **Rule-aligned diagnosis**: RACA anchors breast ultrasound (BUS) classification to standardized clinical descriptors such as shape, margin, echogenicity, and posterior features.
- **Collaborative agents**: The framework decomposes diagnosis into evidence-grounded perception, rule-aligned risk modeling, and critic-guided decision calibration.
- **Traceable evidence chain**: Each prediction is supported by auditable diagnostic evidence, consistency checks, malignancy risk estimates, and prior-case retrieval.
- **Memory-guided calibration**: A critic agent retrieves similar prior cases and selectively refines uncertain predictions under reliability constraints.
- **Strong empirical performance**: RACA improves overall classification performance across BUSI, BUV, and BUSBRA benchmarks while preserving clinically interpretable decision patterns.

## Abstract

Breast cancer is highly prevalent worldwide. While breast ultrasound (BUS) serves as a primary screening modality, its reliable diagnostic interpretation remains inherently challenging. Most existing BUS classification methods prioritize direct image-to-label mapping and overall accuracy, neglecting the alignment with clinical diagnostic rules and decision traceability. Such limitations hinder their integration into rule-governed clinical workflows and diminish the trustworthiness of automated diagnostic decisions.

Although agent-based frameworks offer a modular paradigm for organizing complex decision processes, their potential for rule-aligned and fully traceable BUS diagnosis remains largely unexplored. To address these challenges, we introduce **RACA, a Rule-Aligned Collaborative Agent framework for evidence-grounded BUS classification**. Our RACA introduces **a novel, three-stage framework driven by explicit clinical rules**.

First, a perception module consolidates morphological and semantic evidence into **structured diagnostic evidence**. Then, a risk modeling agent aggregates the evidence under explicit clinical constraints to derive **a rule-aligned risk estimate** and an initial prediction. Finally, **a critic-guided calibration agent refines predictions through memory-based retrieval**.

Extensive experiments on three BUS benchmarks **demonstrate the superiority of RACA over representative classification and medical agent-based frameworks**. Comprehensive interpretability analyses and case demonstrations **validate that its traceable decision pathway aligns with clinical diagnosis**.

## Method Overview

As demonstrated in [Fig. 1](#fig1), given a BUS image, RACA formulates diagnosis as a structured, three-stage decision process governed by standardized clinical rules.

### 1. Evidence-Grounded Perception

Stage I extracts clinically meaningful imaging evidence to form structured diagnostic evidence. Perception agents A<sub>str</sub>, A<sub>sem</sub>, and A<sub>fusion</sub> extract structural and semantic evidence from the image and produce fused diagnostic evidence.

### 2. Rule-Aligned Risk Modeling

Stage II performs risk modeling. A rule-aligned risk modeling agent A<sub>risk</sub> maps the fused evidence to an initial malignancy risk and an initial prediction under explicit clinical constraints.

### 3. Critic-Guided Decision Calibration

Stage III conducts decision calibration. A critic-guided decision calibration agent A<sub>critic</sub> retrieves top-k prior cases to estimate a memory-based malignancy risk, refines the initial prediction, and generates the final prediction through selective calibration.

## Experimental Setup

### Datasets

RACA is evaluated on three public breast ultrasound datasets: **BUSI**, **BUV**, and **BUSBRA**. All methods use the same **7:2:1 train/validation/test split** for fair comparison.

- **BUSI** [1]
- **BUV** [15]
- **BUSBRA** [5]

> **Dataset references**
>
> [1] Al-Dhabyani, W., Gomaa, M., Khaled, H., Fahmy, A. Dataset of breast ultrasound images. *Data in Brief*, 28:104863, 2020.
>
> [15] Lin, Z., Lin, J., Zhu, L., Fu, H., Qin, J., Wang, L. A new dataset and a baseline model for breast lesion detection in ultrasound videos. In *MICCAI*, pp. 614-623, 2022.
>
> [5] Gomez-Flores, W., Gregorio-Calas, M. J., Coelho de Albuquerque Pereira, W. BUS-BRA: A breast ultrasound dataset for assessing computer-aided diagnosis systems. *Medical Physics*, 51(4):3110-3123, 2024.

### Implementation Details

We use **Auto-GPT [23]** to build our agent framework. The calibration **A<sub>k-plug</sub> employs an EfficientNet [26] pretrained on BUSC [21]**, which is independent of the target datasets, **ensuring no data leakage**. Each dataset is randomly divided into training, validation, and test sets with a **7:2:1 ratio**.

RACA uses the validation split exclusively for **hyperparameter selection and memory construction**, while all baselines adopt **identical data splits for fair comparison**. Runtime is **31.40 s/image on one RTX 4090 24GB GPU**, with most latency from the **VLM component (31.38 s/image)**.

> **Implementation references**
>
> [23] Significant Gravitas. AutoGPT, 2023. [Project page](https://github.com/Significant-Gravitas/AutoGPT).
>
> [26] Tan, M., Le, Q. EfficientNet: Rethinking model scaling for convolutional neural networks. In *ICML*, pp. 6105-6114, 2019.
>
> [21] Rodrigues, P. S. Breast ultrasound image. *Mendeley Data*, 2017.

## Results

BUS classification performance is evaluated by standard metrics: **Accuracy (ACC)**, **Precision (PRE)**, **Recall (REC)**, and **F1-score (F1)**.

### Comparison Results

We compare RACA against representative fully supervised methods (**LKA [32]** and **DiffMICv2 [30]**), semi-supervised methods (**OPENSSC [7]** and **PEAT [31]**), and general medical agent frameworks (**Medagents [27]** and **MDAgents [12]**) across the three datasets.

Table 1 shows that RACA achieves superior overall performance. Compared with the strongest fully supervised baselines, RACA obtains substantial accuracy improvements, indicating that explicit rule-aligned decision modeling improves precision over conventional implicit feature learning. Against semi-supervised methods, RACA maintains a more balanced precision-recall trade-off. Crucially, RACA outperforms general medical agents by margins exceeding **19% in accuracy**, demonstrating the necessity of explicitly integrating domain-specific diagnostic rules.

<table>
  <caption><b>Table 1.</b> Quantitative comparison on BUSI, BUV, and BUSBRA datasets.</caption>
  <thead>
    <tr>
      <th rowspan="2">Category</th>
      <th rowspan="2">Method</th>
      <th colspan="4">BUSI</th>
      <th colspan="4">BUV</th>
      <th colspan="4">BUSBRA</th>
    </tr>
    <tr>
      <th>ACC</th>
      <th>PRE</th>
      <th>REC</th>
      <th>F1</th>
      <th>ACC</th>
      <th>PRE</th>
      <th>REC</th>
      <th>F1</th>
      <th>ACC</th>
      <th>PRE</th>
      <th>REC</th>
      <th>F1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Fully-supervised</td>
      <td>LKA [32]</td>
      <td>0.7353</td>
      <td>0.6765</td>
      <td>0.7667</td>
      <td>0.7188</td>
      <td>0.7647</td>
      <td>0.7500</td>
      <td>0.5000</td>
      <td>0.6000</td>
      <td>0.7979</td>
      <td>0.7447</td>
      <td>0.5738</td>
      <td>0.6481</td>
    </tr>
    <tr>
      <td>Fully-supervised</td>
      <td>DiffMICv2 [30]</td>
      <td>0.7500</td>
      <td>0.7485</td>
      <td>0.7518</td>
      <td>0.7486</td>
      <td>0.7647</td>
      <td>0.7596</td>
      <td>0.7045</td>
      <td>0.7167</td>
      <td>0.7606</td>
      <td>0.7419</td>
      <td>0.6780</td>
      <td><b>0.6915</b></td>
    </tr>
    <tr>
      <td>Semi-supervised</td>
      <td>OPENSSC [7]</td>
      <td>0.7206</td>
      <td>0.7540</td>
      <td>0.7395</td>
      <td>0.7191</td>
      <td>0.7059</td>
      <td><b>0.8438</b></td>
      <td>0.5833</td>
      <td>0.5503</td>
      <td>0.7394</td>
      <td>0.7188</td>
      <td>0.6410</td>
      <td>0.6500</td>
    </tr>
    <tr>
      <td>Semi-supervised</td>
      <td>PEAT [31]</td>
      <td>0.6765</td>
      <td>0.7381</td>
      <td>0.6404</td>
      <td>0.6211</td>
      <td>0.6471</td>
      <td>0.3235</td>
      <td>0.5000</td>
      <td>0.3929</td>
      <td>0.7447</td>
      <td><b>0.7512</b></td>
      <td>0.6321</td>
      <td>0.6382</td>
    </tr>
    <tr>
      <td>Medical agent-based</td>
      <td>Medagents [27]</td>
      <td>0.6119</td>
      <td>0.6423</td>
      <td>0.6297</td>
      <td>0.6077</td>
      <td>0.5294</td>
      <td>0.5208</td>
      <td>0.5227</td>
      <td>0.5143</td>
      <td>0.5260</td>
      <td>0.5271</td>
      <td>0.5312</td>
      <td>0.5103</td>
    </tr>
    <tr>
      <td>Medical agent-based</td>
      <td>MDAgents [12]</td>
      <td>0.5588</td>
      <td>0.5000</td>
      <td><b>0.8000</b></td>
      <td>0.6154</td>
      <td>0.7059</td>
      <td>0.6000</td>
      <td>0.5000</td>
      <td>0.5455</td>
      <td>0.6330</td>
      <td>0.4524</td>
      <td>0.6230</td>
      <td>0.5241</td>
    </tr>
    <tr>
      <td><b>Ours</b></td>
      <td><b>RACA</b></td>
      <td><b>0.8088</b></td>
      <td><b>0.7742</b></td>
      <td><b>0.8000</b></td>
      <td><b>0.7869</b></td>
      <td><b>0.8235</b></td>
      <td>0.7143</td>
      <td><b>0.8333</b></td>
      <td><b>0.7692</b></td>
      <td><b>0.8085</b></td>
      <td>0.7273</td>
      <td>0.6557</td>
      <td>0.6897</td>
    </tr>
  </tbody>
</table>

RACA shows the strongest overall accuracy across all three datasets and achieves large gains over general medical agent baselines, highlighting the importance of explicitly encoding domain-specific diagnostic rules.

> **Comparison baseline references**
>
> [32] Zhu, Z., Lu, S. Y., Huang, T., Liu, L., Liu, Z. LKA: Large Kernel Adapter for enhanced medical image classification. In *MICCAI*, pp. 394-404, 2025.
>
> [30] Yang, Y., Fu, H., Aviles-Rivero, A. I., Xing, Z., Zhu, L. DiffMIC-v2: Medical image classification via improved diffusion network. *IEEE Transactions on Medical Imaging*, 44(5):2244-2255, 2025.
>
> [7] He, A., Li, T., Zhao, Y., Zhao, J., Fu, H. Open-set semi-supervised medical image classification with learnable prototypes and outlier filter. In *MICCAI*, pp. 492-501, 2024.
>
> [31] Zeng, Q., Xie, Y., Lu, Z., Xia, Y. PEFAT: Boosting semi-supervised medical image classification via pseudo-loss estimation and feature adversarial training. In *CVPR*, pp. 15671-15680, 2023.
>
> [27] Tang, X., Zou, A., Zhang, Z., Li, Z., Zhao, Y., Zhang, X., Cohan, A., Gerstein, M. MedAgents: Large language models as collaborators for zero-shot medical reasoning. In *Findings of ACL*, pp. 599-621, 2024.
>
> [12] Kim, Y., Park, C., Jeong, H., Chan, Y. S., Xu, X., McDuff, D., Lee, H., Ghassemi, M., Breazeal, C., Park, H. W. MDAgents: An adaptive collaboration of LLMs for medical decision-making. *NeurIPS*, 37:79410-79452, 2024.

### Ablation Study

We evaluate the contribution of each component in RACA on the BUSI and BUSBRA datasets. In Table 2, while the baseline (**EGP + A<sub>risk</sub>**) establishes an evidence-grounded and rule-aligned foundation, sequentially integrating multiple calibration agents (**A<sub>k-top</sub>**, **A<sub>k-thre</sub>**, and **A<sub>k-plug</sub>**) achieves substantial, step-wise performance gains. This steady upward trajectory validates our core design philosophy: decision calibration with prior-case retrieval acts as a powerful, structured enhancement of rule-based inference rather than a replacement.

<table>
  <caption><b>Table 2.</b> Ablation study on BUSI and BUSBRA datasets.</caption>
  <thead>
    <tr>
      <th rowspan="2">EGP</th>
      <th rowspan="2">A<sub>risk</sub></th>
      <th rowspan="2">A<sub>k-top</sub></th>
      <th rowspan="2">A<sub>k-thre</sub></th>
      <th rowspan="2">A<sub>k-plug</sub></th>
      <th colspan="4">BUSI</th>
      <th colspan="4">BUSBRA</th>
    </tr>
    <tr>
      <th>ACC</th>
      <th>PRE</th>
      <th>REC</th>
      <th>F1</th>
      <th>ACC</th>
      <th>PRE</th>
      <th>REC</th>
      <th>F1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>&#10003;</td>
      <td>&#10003;</td>
      <td></td>
      <td></td>
      <td></td>
      <td>0.6176</td>
      <td>0.5909</td>
      <td>0.4333</td>
      <td>0.5000</td>
      <td>0.5212</td>
      <td>0.3164</td>
      <td>0.4098</td>
      <td>0.3571</td>
    </tr>
    <tr>
      <td>&#10003;</td>
      <td>&#10003;</td>
      <td>&#10003;</td>
      <td></td>
      <td></td>
      <td>0.7054</td>
      <td>0.6744</td>
      <td>0.6042</td>
      <td>0.6374</td>
      <td>0.6223</td>
      <td>0.4151</td>
      <td>0.3607</td>
      <td>0.3860</td>
    </tr>
    <tr>
      <td>&#10003;</td>
      <td>&#10003;</td>
      <td>&#10003;</td>
      <td>&#10003;</td>
      <td></td>
      <td>0.7941</td>
      <td>0.7667</td>
      <td>0.7667</td>
      <td>0.7667</td>
      <td>0.7713</td>
      <td><b>0.7647</b></td>
      <td>0.4262</td>
      <td>0.5474</td>
    </tr>
    <tr>
      <td>&#10003;</td>
      <td>&#10003;</td>
      <td>&#10003;</td>
      <td>&#10003;</td>
      <td>&#10003;</td>
      <td><b>0.8088</b></td>
      <td><b>0.7742</b></td>
      <td><b>0.8000</b></td>
      <td><b>0.7869</b></td>
      <td><b>0.8085</b></td>
      <td>0.7273</td>
      <td><b>0.6557</b></td>
      <td><b>0.6897</b></td>
    </tr>
  </tbody>
</table>

## Diagnostic Interpretability

Moving beyond overall classification accuracy, we further assess whether the proposed RACA forms a stable, traceable, and clinically consistent decision-making process. We conduct a systematic experimental analysis of the structured feature representations within the evidence acquired by RACA. Specifically, we investigate the relationship between established BI-RADS guideline-based features and the explicit diagnostic evidence constructed by RACA.

For each sample, we compute the normalized frequency of each feature within the top-9 ranked evidence, which reflects its relative contribution to the final decision-making process. Fig. 2(a) shows that the distribution of guideline-based evidence tightly aligns with the overall structured evidence, demonstrating that RACA strictly preserves clinical rules while robustly expanding its representational capacity to capture subtle diagnostic evidence. Fig. 2(b) shows a stark contrast: malignant predictions are heavily driven by invasive morphological features, such as eccentricity and edge, whereas benign predictions rely predominantly on acoustic intensity evidence, such as lesion brightness and posterior ratio, proving that the model naturally replicates real-world clinical diagnosis.

![RACA interpretability](Images/Fig3.PNG)

<p align="justify">
<b>Fig. 2.</b> Comprehensive analysis of the proposed RACA across consistency, discrimination, and hierarchical representation on the BUSI dataset.
</p>

## Case Demonstration

To explicitly demonstrate the diagnostic traceability of RACA, we show the inference process using a randomly selected test sample. Fig. 3 presents the step-by-step intermediate output of each stage. In Stage I, quantitative morphological measurements and VLM-derived semantic descriptions are converted into clinically aligned diagnostic evidence. In Stage II, the fused evidence is mapped to attribute-wise contributions and aggregated into a malignancy risk estimate. In Stage III, the critic-guided calibration agent verifies rule consistency, confirms malignancy gate activation, and routes the final decision according to the calibrated risk and rule-level evidence.

![Traceable rule-aligned inference process of RACA](Images/Fig2.PNG)

<p align="justify">
<b>Fig. 3.</b> Traceable rule-aligned inference process of RACA on a test sample.
</p>

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
