# llm-phishing-adversarial

# LLM 驅動的釣魚郵件對抗式生成與偵測研究
# Adversarial Phishing Email Generation & Detection with LLMs

碩士論文專案：以強化學習探討生成式 AI 於資安場景的濫用風險，
建構「生成端 vs 偵測端」的對抗式框架，並提供完整的模型評估與校準。

A master's thesis project studying the misuse risk of generative AI in
cybersecurity, via an adversarial generator-vs-detector framework with
rigorous model evaluation and calibration.

## 專案架構 | Architecture

- **偵測端 (Detector):** 微調 DeBERTa-v3 文本分類器，判別釣魚 / 正常郵件
- **生成端 (Generator):** Vicuna-7B，經 LoRA-SFT 領域微調 → PPO (RLHF) 對抗訓練
- **對抗目標:** 生成端學習規避偵測端，量化生成式 AI 的資安風險

## 技術重點 | Highlights

**偵測端 (`01_deberta_detector.ipynb`)**
- DeBERTa-v3 二元分類，F1 = 96.54%
- Stratified K-Fold 交叉驗證
- 類別權重處理資料不平衡 (class weighting)
- Temperature Scaling 機率校準
- 評估指標：Precision / Recall / F1 / ROC-AUC

**生成端 (`02_vicuna_sft_ppo.ipynb`)**
- QLoRA 4-bit 量化微調 (nf4 + double quant)
- 兩階段訓練：SFT → PPO (RLHF)，使用 TRL 框架
- KL 散度約束 + 自適應 KL 控制，抑制 Reward Hacking
- 以凍結的 DeBERTa 作為 reward model
- Perplexity 驗證生成穩定度 (標準差 2.91 → 0.27)
- Base / SFT / PPO 三組 Ablation Study

## 主要結果 | Results

- 偵測模型 F1 達 96.54%
- 對抗訓練後，規避偵測成功率較基線提升 8.67 個百分點
- 實證生成式 AI 於社交工程攻擊的濫用風險，為企業導入 AI 的資安辨識提供依據

## 技術棧 | Tech Stack

Python · PyTorch · Hugging Face Transformers · PEFT (LoRA) · TRL (PPO) ·
bitsandbytes · scikit-learn

## 備註 | Note

本專案為學術研究用途，旨在揭露並防範生成式 AI 的資安風險，
不提供可直接濫用的攻擊工具或資料。
