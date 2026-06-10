# Datasets for Inductors — 电感损耗预测数据集

本项目提供用于**电感器损耗预测**的机器学习数据集和训练代码，涵盖两种常见磁芯类型：**EC 型**和 **PQ 型**。

## 项目结构

```
├── EC core/                        # EC 型磁芯
│   ├── Train.py                    # PyTorch 神经网络训练脚本
│   ├── Datasets/
│   │   ├── filtered_samples_20000.csv   # 20,000 条训练样本
│   │   └── Test_5000.csv                # 5,000 条测试样本
│   └── 电感结构.png                 # 电感结构示意图
│
├── PQ core/                        # PQ 型磁芯
│   ├── dataset/
│   │   ├── train.csv               # 训练集
│   │   ├── test.csv                # 测试集
│   │   └── val.csv                 # 验证集
│   └── 磁芯数据手册/                # PQ 磁芯规格书 (PDF)
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

## EC 型磁芯

### 数据集说明

EC 型磁芯数据集通过 **Maxwell 有限元仿真** 生成，包含在不同几何参数和激励条件下的电感仿真结果。

**输入特征 (9 维)**：

| 特征 | 含义 | 单位 |
|------|------|------|
| `c` | 磁芯中柱宽度 | mm |
| `dc1` | 窗口深度 | mm |
| `dc2` | 边柱宽度 | mm |
| `f` | 频率 | kHz |
| `ht` | 窗口高度 | mm |
| `i` | 激励电流 | A |
| `lg1` | 气隙长度 | mm |
| `Nx` | x 方向绕组匝数 | — |
| `Ny` | y 方向绕组匝数 | — |

**输出目标 (3 维)**：

| 目标 | 含义 | 单位 |
|------|------|------|
| `L` | 电感值 | μH |
| `Pw` | 绕组损耗 | W |
| `Pc` | 磁芯损耗 | W |

### 训练代码

`Train.py` 使用 **PyTorch** 实现了一个**多分支前馈神经网络**：
- **共享层**：9 → 79 全连接 + ReLU
- **分支层**：分别预测 L、Pw、Pc
- **损失函数**：加权 MSE（权重 [0.5, 1.0, 1.5] 对应 L、Pw、Pc）
- **数据预处理**：对数变换 + StandardScaler 标准化

运行训练：

```bash
cd "EC core"
python Train.py
```

## PQ 型磁芯

### 数据集说明

PQ 型磁芯数据集同样由 Maxwell 有限元仿真生成，涵盖 PQ 16/11.6 至 PQ 65/60 多种规格。

**输入特征**：

| 特征 | 含义 |
|------|------|
| `StrandDiameter` | 利兹线单股直径 |
| `Strands` | 利兹线股数 |
| `A` / `B` / `C` / `D` / `E` | 磁芯几何尺寸 |
| `AirGap` | 气隙长度 |
| `Frequency` | 频率 |
| `Current` | 电流 |
| `Turns` | 匝数 |
| `phi_A` / `phi_B` | 相位角 |

**输出目标**：

| 目标 | 含义 |
|------|------|
| `L` | 电感值 |
| `StrandedLossAC` | 交流绕组损耗 |
| `CoreLoss` | 磁芯损耗 |
| `CoreType` | 磁芯型号标签 |

### 磁芯数据手册

`磁芯数据手册/` 目录包含 PQ 系列各型号磁芯的原始规格书（PDF），提供磁芯的机械尺寸、磁参数等参考信息。

## 引用与致谢

数据由 Maxwell 有限元仿真软件生成，用于电力电子领域中高频电感器的损耗建模研究。

如果您在研究中使用了本数据集，请引用：

```
@misc{TJU-CAPS-inductors,
  author       = {TJU-CAPS},
  title        = {Datasets for Inductors: EC and PQ Core Loss Prediction},
  year         = {2025},
  publisher    = {GitHub},
  url          = {https://github.com/TJU-CAPS/Datasets-for-inductors}
}
```

## License

本项目采用**双重许可**：

- **代码** (`Train.py` 等源码) — [MIT License](LICENSE)
- **数据集** (CSV 文件) — [CC BY 4.0](LICENSE-DATA)

使用数据时请务必**署名**，标明数据来源为 TJU-CAPS。磁芯数据手册 (PDF) 为各厂商公开发布的技术文档，版权归原厂商所有。
