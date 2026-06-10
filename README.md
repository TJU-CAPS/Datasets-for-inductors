# Datasets for Inductors — Core Loss Prediction

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20619694.svg)](https://doi.org/10.5281/zenodo.20619694)

Machine learning datasets and training code for **inductor loss prediction**, covering two common core types: **EC core** and **PQ core**.

## Project Structure

```
├── EC core/                        # EC-type magnetic core
│   ├── Train.py                    # PyTorch neural network training script
│   ├── Datasets/
│   │   ├── filtered_samples_20000.csv   # 20,000 training samples
│   │   └── Test_5000.csv                # 5,000 test samples
│   └── 电感结构.png                 # Inductor structure diagram
│
├── PQ core/                        # PQ-type magnetic core
│   ├── dataset/
│   │   ├── train.csv               # Training set
│   │   ├── test.csv                # Test set
│   │   └── val.csv                 # Validation set
│   └── 磁芯数据手册/                # PQ core datasheets (PDF)
│       ├── pq_16_11_6.pdf
│       ├── pq_20_16.pdf
│       ├── pq_20_20.pdf
│       ├── pq_26_20.pdf
│       ├── pq_26_25.pdf
│       ├── pq_32_20.pdf
│       ├── pq_32_30.pdf
│       ├── pq_35_35.pdf
│       ├── pq_40_30.pdf
│       ├── pq_40_40.pdf
│       ├── pq_50_40.pdf
│       ├── pq_50_50.pdf
│       ├── pq_65_60.pdf
│       └── pq_107_87.pdf
│
├── .gitignore
└── README.md
```

## EC Core

### Dataset Description

The EC core dataset was generated using **Maxwell Finite Element Method (FEM) simulations**, covering inductor performance under various geometric parameters and excitation conditions.

**Input Features (9 dimensions)**:

| Feature | Description | Unit |
|---------|-------------|------|
| `c` | Center leg width | mm |
| `dc1` | Window depth | mm |
| `dc2` | Outer leg width | mm |
| `f` | Frequency | kHz |
| `ht` | Window height | mm |
| `i` | Excitation current | A |
| `lg1` | Air gap length | mm |
| `Nx` | Turns in x-direction | — |
| `Ny` | Turns in y-direction | — |

**Output Targets (3 dimensions)**:

| Target | Description | Unit |
|--------|-------------|------|
| `L` | Inductance | μH |
| `Pw` | Winding loss | W |
| `Pc` | Core loss | W |

### Training Code

`Train.py` implements a **multi-branch feedforward neural network** using **PyTorch**:
- **Shared layer**: 9 → 79 fully connected + ReLU
- **Branch layers**: Separate prediction heads for L, Pw, and Pc
- **Loss function**: Weighted MSE (weights [0.5, 1.0, 1.5] for L, Pw, Pc)
- **Preprocessing**: Log transformation + StandardScaler normalization

To run training:

```bash
cd "EC core"
python Train.py
```

## PQ Core

### Dataset Description

The PQ core dataset was also generated via Maxwell FEM simulations, spanning multiple core sizes from PQ 16/11.6 to PQ 65/60.

**Input Features**:

| Feature | Description |
|---------|-------------|
| `StrandDiameter` | Litz wire strand diameter |
| `Strands` | Number of litz wire strands |
| `A` / `B` / `C` / `D` / `E` | Core geometric dimensions |
| `AirGap` | Air gap length |
| `Frequency` | Frequency |
| `Current` | Current |
| `Turns` | Number of turns |
| `phi_A` / `phi_B` | Phase angles |

**Output Targets**:

| Target | Description |
|--------|-------------|
| `L` | Inductance |
| `StrandedLossAC` | AC winding loss |
| `CoreLoss` | Core loss |
| `CoreType` | Core model label |

### Core Datasheets

The `磁芯数据手册/` directory contains original manufacturer datasheets (PDF) for various PQ core sizes, providing mechanical dimensions and magnetic parameters for reference.

## Citation

The data was generated using Maxwell FEM simulations for high-frequency inductor loss modeling in power electronics.

If you use this dataset in your research, please cite:

```
@misc{TJU-CAPS-inductors,
  author       = {TJU-CAPS},
  title        = {Datasets for Inductors: EC and PQ Core Loss Prediction},
  year         = {2025},
  publisher    = {Zenodo},
  doi          = {10.5281/zenodo.20619694},
  url          = {https://doi.org/10.5281/zenodo.20619694}
}
```

## Funding

This work is supported by **TECH SEED**.

## License

This project uses a **dual license**:

- **Code** (`Train.py` and other source files) — [MIT License](LICENSE)
- **Datasets** (CSV files) — [CC BY 4.0](LICENSE-DATA)

**Attribution is required** when using the datasets — please credit TJU-CAPS. The core datasheets (PDF) are publicly available manufacturer documents; copyright belongs to their respective owners.
