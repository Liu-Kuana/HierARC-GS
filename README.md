# HierARC-GS

***HierARC-GS*** is a robust 3D Gaussian Splatting (3DGS) framework designed for high-fidelity scene reconstruction from unconstrained visual sequences (e.g., drone imagery, handheld captures). It addresses the pervasive "floater" artifacts and geometric inconsistencies caused by transient distractors and sparse-view constraints through hierarchical semantic priors and adaptive residual calibration.

## 1. Project Features

- **Hierarchical Semantic Prior Acquisition**

Leverages visual foundation models (SAM2 and SAM3) to automate semantic labeling, partitioning scene elements into hierarchical stability clusters (Dynamic, Suspected Static, Absolutely Forbidden, and others).

- **Instance-wise Residual Consensus Mechanism**

Introduces an instance-level reasoning layer that distinguishes true physical motion from parallax-induced photometric errors by comparing instance residuals against group consensus.

- **Semantics-Guided Adaptive Modulation**

Dynamically modulates detection sensitivity based on regional stability profiles, effectively suppressing transient distractors while strictly preserving stable geospatial anchors.

- **Robustness under Sparse-View Constraints**

Engineered to maintain high structural fidelity and background purity even when viewpoints are drastically reduced (downsampled by 30%-50%).

## 2. Main Contributions

- **HierARC-GS Framework** : A semantics-conditioned residual calibration paradigm that decouples perspective noise from true object displacement for robust 3DGS optimization.

- **Adaptive Calibration Strategy** : A stability-aware modulation approach that generates reliability-aware optimization masks to suppress inconsistent supervision.

- **Comprehensive Evaluation** : Extensive experiments on 12 real-world sequences across low, medium, and high occlusion levels, demonstrating superior artifact suppression.

- **New Dataset** : Introduction of a self-captured 3D reconstruction dataset featuring diverse architectural geometries and complex dynamic environments.

## 3. Related Visualizations

### 3.1 Systematic Failure Analysis of 3DGS

Dynamic distractors violate static assumptions, causing geometric inconsistency in the latent Gaussian space and resulting in floaters and textural blurring.

### 3.2 HierARC-GS Technical Architecture

The pipeline consists of three synergetic stages: (1) Hierarchical Semantic Acquisition, (2) Adaptive Residual Modulation, and (3) Masked 3DGS Optimization.

### 3.3 Automated Labeling and Anomaly Extraction

Illustration of semantic region segmentation and the anomaly extraction workflow where instance-level residuals are fused with semantic IDs to compute the relative anomaly ratio.

### 3.4 Spatio-temporal Evolution of Optimization Masks

Evolution of the mask from initial warm-up noise (t=200) to precise isolation of dynamic agents at maturity (t=3000).

### 3.5 Qualitative Comparison

Visual comparison demonstrating our method's ability to eliminate distractors while preserving sharp geometric details of static backgrounds compared to baselines.

## 4. How to Run

### 4.1 Environment Configuration

- **Operating System** : Ubuntu 20.04 or higher

- **GPU** : NVIDIA RTX 3090 (24GB VRAM recommended)

- **Cuda Version** : 11.6+

- **Python** : 3.8+

### 4.2 Dependencies

- **Python Packages** :

```bash
pip install torch torchvision torchaudio
pip install -r requirements.txt
```

### 4.3 Installation

```bash
git clone https://github.com/liu-kuana/hierarc-gs.git
cd hierarc-gs

# Install submodules for rasterization
pip install submodules/diff-gaussian-rasterization
pip install submodules/simple-knn
```

### 4.4 Dataset Preparation

- Place your image sequences in the `data/` directory.

- This project supports the **NeRF On-the-go** dataset and our self-captured **HierARC Dataset**.

- For sparse-view protocols (to simulate extreme informational scarcity as described in the paper), use our provided downsampling script to select approximately 30%-50% of the original frames:

```bash
python resize_images.py --path data/Monument --ratio 0.3
```

### 4.5 Execution Commands

**Training with HierARC-GS:**
The hierarchical semantic modulation is activated at **500 iterations** to establish a stable baseline geometry. We recommend training for **3,000 iterations** to achieve high-fidelity convergence within the sparse-view regime as described in the paper.

```bash
python yuyi1.py -s data/Monument --iterations 3000 --use_masks --mask_start_iter 500
```

**Variants for Ablation Studies:**

| Variant | Script | Description |
|---------|--------|-------------|
| **HierARC-GS (Ours)** | `yuyi1.py` | Full method with instance consensus |
| **Variant B** | `yuyi1_variant_b.py` | Hard semantic masking (no residual) |
| **Variant C** | `yuyi1_variant_c.py` | Pixel-level only (no instance averaging) |

**Run Variant B:**
```bash
python yuyi1_variant_b.py -s data/Monument --iterations 3000
```

**Run Variant C:**
```bash
python yuyi1_variant_c.py -s data/Monument --iterations 3000
```

**Generate Heatmaps & Masks:**
```bash
python heatmaps_binary2.py --model_path output/Monument
```

**Render Video:**
```bash
python render_video.py --model_path output/Monument
```

## 5. Citation

If you find our work useful in your research, please cite our paper:

```bibtex
@article{liu2026hierarcgs,
  title={HierARC-GS: Semantic-Conditioned Residual Calibration for Robust 3D Gaussian Splatting from Unconstrained Visual Sequences},
  author={Liu, Kuan and Firkat, Eksan and Zhu, Bin and Zhu, Jihong and Li, Fengze and Hamdulla, Askar},
  journal={IEEE Transactions on Geoscience and Remote Sensing},
  year={2026}
}
```

## 6. Acknowledgement

This work was supported by the **National Natural Science Foundation of China** (Grant No. 62541328) and the **Tianshan Talents Cultivation Program - Leading Talents for Scientific and Technological Innovation** (No. 2024TSY-CLJ0002). We would also like to express our gratitude to the authors of **3DGS**, **SAM 2**, and **SAM 3** for their outstanding open-source contributions that made this research possible.
