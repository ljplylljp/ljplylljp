# ResFed: Residual-based Robust Federated Aggregation

基于更新残差的联邦学习拜占庭鲁棒聚合方法（代码包）

## 项目结构

```
ResFed_Code/
├── client/
│   └── flower_client.py      # Flower客户端（支持投毒攻击）
├── server/
│   └── strategy.py           # ResidualRobustStrategy 核心聚合策略
├── models/
│   └── cnn.py                # SimpleCNN / FashionCNN
├── utils/
│   ├── data_utils.py         # 数据加载与Non-IID划分
│   └── attack_utils.py       # 标签翻转、模型替换、后门攻击
├── configs/
│   └── default.yaml          # 默认实验配置
├── main.py                   # 主运行脚本
├── requirements.txt
└── README.md
```

## 环境安装

```bash
conda create -n resfed python=3.10
conda activate resfed
pip install -r requirements.txt
```

## 快速运行

```bash
python main.py --config configs/default.yaml
```

## 主要配置说明（configs/default.yaml）

| 参数            | 说明                                      | 推荐值     |
| --------------- | ----------------------------------------- | ---------- |
| dataset         | cifar10 / fashionmnist                    | cifar10    |
| num_clients     | 客户端总数                                | 50~100     |
| malicious_ratio | 恶意比例                                  | 0.1~0.4    |
| attack_type     | label_flip / model_replacement / backdoor | label_flip |
| alpha           | Dirichlet非IID参数（越小越非IID）         | 0.1 / 0.5  |
| contamination   | IsolationForest污染率                     | 0.2        |
| use_dp          | 是否开启差分隐私                          | false      |

## 支持的攻击类型

1. **label_flip**：标签翻转
2. **model_replacement**：模型替换（放大更新）
3. **backdoor**：后门触发器攻击

## 核心方法

服务器端对客户端更新提取残差特征（L2范数、均值、标准差、最大绝对值、与均值更新的余弦相似度），使用Isolation Forest进行异常检测，仅聚合被判定为良性的更新。

## 扩展建议

- 差分隐私：在client中集成Opacus（已预留接口）
- 边缘部署：将client代码移植到树莓派/Jetson，配合Flower真实客户端模式
- 更多基线：可在strategy中添加Krum、Median等对比

## 引用

如果本代码对你的研究有帮助，请引用相关论文（待发表后补充）。

## License

MIT License（仅供学术研究使用）
