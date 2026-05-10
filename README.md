# HierARC-GS
[cite_start]***HierARC-GS*** is a robust 3D Gaussian Splatting (3DGS) framework designed for high-fidelity scene reconstruction from unconstrained visual sequences (e.g., drone imagery, handheld captures). It addresses the pervasive "floater" artifacts and geometric inconsistencies caused by transient distractors and sparse-view constraints through hierarchical semantic priors and adaptive residual calibration[cite: 6, 7, 634, 635].

## 1. Project Features
* **Hierarchical Semantic Prior Acquisition**
  [cite_start]Leverages visual foundation models (SAM2 and SAM3) to automate semantic labeling, partitioning scene elements into hierarchical stability clusters (Dynamic, Suspected Static, Absolutely Forbidden, and others)[cite: 8, 60, 636, 688].

* **Instance-wise Residual Consensus Mechanism**
  [cite_start]Introduces an instance-level reasoning layer that distinguishes true physical motion from parallax-induced photometric errors by comparing instance residuals against group consensus[cite: 9, 61, 637, 689].

* **Semantics-Guided Adaptive Modulation**
  Dynamically modulates detection sensitivity based on regional stability profiles, effectively suppressing transient distractors while strictly preserving stable geospatial anchors[cite: 10, 62, 638, 690].

* **Robustness under Sparse-View Constraints**
  [cite_start]Engineered to maintain high structural fidelity and background purity even when viewpoints are drastically reduced (downsampled by 30%-50%)[cite: 63, 344, 345, 691, 966, 967].

## 2. Main Contributions
- [cite_start]**HierARC-GS Framework**: A semantics-conditioned residual calibration paradigm that decouples perspective noise from true object displacement for robust 3DGS optimization[cite: 57, 68, 685, 696].
- [cite_start]**Adaptive Calibration Strategy**: A stability-aware modulation approach that generates reliability-aware optimization masks to suppress inconsistent supervision[cite: 70, 698].
- [cite_start]**Comprehensive Evaluation**: Extensive experiments on 12 real-world sequences across low, medium, and high occlusion levels, demonstrating superior artifact suppression[cite: 11, 65, 639, 693].
- [cite_start]**New Dataset**: Introduction of a self-captured 3D reconstruction dataset featuring diverse architectural geometries and complex dynamic environments[cite: 71, 346, 699, 968].

## 3. Related Visualizations

### 3.1 Systematic Failure Analysis of 3DGS
![Fig 1](path/to/Fig1.jpg)
[cite_start]Dynamic distractors violate static assumptions, causing geometric inconsistency in the latent Gaussian space and resulting in floaters and textural blurring[cite: 27, 28, 655, 656].

### 3.2 HierARC-GS Technical Architecture
![Fig 3](path/to/Fig3.jpg)
[cite_start]The pipeline consists of three synergetic stages: (1) Hierarchical Semantic Acquisition, (2) Adaptive Residual Modulation, and (3) Masked 3DGS Optimization[cite: 153, 154, 781, 782].

### 3.3 Automated Labeling and Anomaly Extraction
![Fig 4](path/to/Fig4.jpg)
[cite_start]Illustration of semantic region segmentation and the anomaly extraction workflow where instance-level residuals are fused with semantic IDs to compute the relative anomaly ratio $\gamma_{(i,j)}$[cite: 198, 200, 826, 828].

### 3.4 Spatio-temporal Evolution of Optimization Masks
![Fig 5](path/to/Fig5.jpg)
[cite_start]Evolution of the mask $\mathcal{M}$ from initial warm-up noise ($t=200$) to precise isolation of dynamic agents at maturity ($t=3000$)[cite: 316, 935].

### 3.5 Qualitative Qualitative Comparison
![Fig 6](path/to/Fig6.jpg)
[cite_start]Visual comparison demonstrating our method's ability to eliminate distractors while preserving sharp geometric details of static backgrounds compared to baselines[cite: 447, 1168].

## 4. How to Run

### 4.1 Environment Configuration
- **OS**: Ubuntu 20.04 or higher
- **GPU**: NVIDIA RTX 3090 (24GB VRAM recommended)
- **Cuda Version**: 11.6+
- **Python**: 3.8+

### 4.2 Dependencies
```bash
pip install torch torchvision torchaudio
pip install -r requirements.txt

### 4.3 Installation
```bash
git clone [https://github.com/liu-kuana/hierarc-gs.git](https://github.com/liu-kuana/hierarc-gs.git)
cd hierarc-gs

# Install submodules for rasterization
pip install submodules/diff-gaussian-rasterization
pip install submodules/simple-knn

### 4.4 Dataset Preparation
- [cite_start]Place your image sequences in the `data/` directory. [cite: 6114, 6212]
- [cite_start]This project supports the **NeRF On-the-go** dataset and our self-captured **HierARC Dataset**. [cite: 5590, 5594]
- [cite_start]For sparse-view protocols (to simulate extreme informational scarcity as described in the paper), use our provided downsampling script to select approximately 30%-50% of the original frames: [cite: 5592, 6214]
```bash
python scripts/downsample.py --path data/Monument --ratio 0.3

### 4.5 Execution Commands

**Training with HierARC-GS:**
[cite_start]The hierarchical semantic modulation is activated at **500 iterations** to establish a stable baseline geometry[cite: 5602, 6225]. [cite_start]We recommend training for **3,000 iterations** to achieve high-fidelity convergence within the sparse-view regime as described in the paper[cite: 5598, 6220].
```bash
python train.py -s data/Monument --iterations 3000 --config configs/hierarc.yaml

## 5. Citation

[cite_start]If you find our work useful in your research, please cite our paper[cite: 5250, 5878]:

```bibtex
@article{liu2026hierarcgs,
  title={HierARC-GS: Semantic-Conditioned Residual Calibration for Robust 3D Gaussian Splatting from Unconstrained Visual Sequences},
  author={Liu, Kuan and Firkat, Eksan and Zhu, Bin and Zhu, Jihong and Li, Fengze and Hamdulla, Askar},
  journal={IEEE Transactions on Geoscience and Remote Sensing},
  year={2026}
}
## 6. Acknowledgement

[cite_start]This work was supported by the **National Natural Science Foundation of China** (Grant No. 62541328) and the **Tianshan Talents Cultivation Program - Leading Talents for Scientific and Technological Innovation** (No. 2024TSY-CLJ0002)[cite: 5267]. We would also like to express our gratitude to the authors of **3DGS**, **SAM 2**, and **SAM 3** for their outstanding open-source contributions that made this research possible.













