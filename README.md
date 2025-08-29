# LLM 微調對多跳推理能力的影響研究

本專案目的是透過**LoRA 微調 (Fine-tuning)**方法，使 **[MediaTek-Research/Llama-Breeze2-3B-Instruct](https://huggingface.co/MediaTek-Research/Llama-Breeze2-3B-Instruct)** 模型處理複雜、多步驟的「多跳推理」能力。

---

## 專案簡介

原生模型在處理複雜推理問題時，無法掌握問題核心並且沒有推理能力。本專案透過兩種微調方式進行研究：

* **微調答案模型（FT-AO）**：只提供正確答案進行微調。
* **微調思維鏈模型（FT-CoT）**：提供包含完整推理過程的數據進行微調。

研究核心發現：**CoT 微調不僅能顯著提高模型的答案準確率，更能賦予其真正的多步驟推理能力**。

---

## 專案流程

整體分為三大階段：

1.  **資料集生成**
    * 利用gpt-4o模型作為生成引擎，系統性地產出包含多跳推理、基礎知識與增強題型的**合成資料集**。

2.  **模型微調**
    * 使用生成的資料集，分別對模型進行兩種微調：
        * **FT-AO**：僅提供最終答案進行訓練。
        * **FT-CoT**：提供完整的思維鏈（推理過程）進行訓練。

3.  **模型評估與結果分析**
    * 比較原生模型、FT-AO 模型與 FT-CoT 模型在處理多跳推理問題時的表現。
    * 分析各模型輸出結果的準確率與推理過程，以驗證 CoT 微調的有效性。

---


## 專案結構

```

LLM\_Breeze2-CoT/
├── /data
│   └── /dataset
│       ├── train\_chatml.jsonl
│       ├── train\_chatml\_cot.jsonl
│       ├── val\_chatml.jsonl
│       └── test\_chatml.jsonl
├── /results
│   ├── ft\_ao\_outputs\_02.jsonl
│   ├── ft\_cot\_outputs\_02.jsonl
│   └── base\_model\_outputs\_02.jsonl
├── README.md
└── requirements.txt

````

---

##  環境需求

* Python 3.10 以上
* 支援 CUDA 的 NVIDIA GPU（建議 11GB VRAM 以上）
* 主要套件：請參考 `requirements.txt`

###  安裝環境

建議使用虛擬環境，並安裝所有相依套件：

```bash
pip install -r requirements.txt
````

-----

##  執行步驟

### 1\. 資料集準備

原始訓練與測試資料集已整理並放在 `data/dataset` 資料夾中。

### 2\. 模型微調

使用 `lora2_02.ipynb` 筆記本中的程式碼，分別對模型進行 FT-AO 和 FT-CoT 微調。請務必確認筆記本中的檔案路徑已設定正確。

### 3\. 結果分析

微調後的模型輸出結果會存放於 `/results` 資料夾。

-----

## 結論與發現

  * **原生模型**：表現出「假性推理」，答案籠統且缺乏邏輯，準確率最低。
  * **微調 AO 模型**：準確度有提升，但推理能力並未顯著提升。
  * **微調 CoT 模型**：在所有模型中表現最佳，其清晰的多步驟推理過程證明了 CoT 微調能賦予模型真正的推理能力。

<!-- end list -->

```
```