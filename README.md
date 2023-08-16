# ChiMed

## Introduction

**ChiMed: An Open Source Chinese Medical Large Language Model **

This project utilized a Chinese medical instruction dataset to fine-tune the **[KnowLM](https://github.com/zjunlp/KnowLM)**-13B model, resulting in a significant improvement in the performance of the large language model in the context of Chinese medical knowledge question-answering scenarios.

## Dataset Construction (Instruction tuning)

Currently, most open-source ChatLLM (Conversational Large Language Model) projects utilize instruction data generated by other models like ChatGPT. Inevitably, this gives rise to the issue of data hallucination, which can significantly impact the practical application and extension of LLMs in real-world scenarios. Consequently, in this project, to enhance the accuracy of knowledge question-answering in the medical domain, we adopted the following approach for constructing the instruction dataset:

1. A compilation of genuine medical patient Q&A data (covering topics such as diseases, medications, diagnostic tests, surgeries, prognoses, dietary information, etc.) resulted in a total of 560k instruction data entries.
2. Drug knowledge data: Based on the drug text knowledge of the Medical Knowledge base, by setting specific question templates for semi-structured data (e.g. : "What is the adaptive disease of {drug}?") Construct instruction data set, a total of 180K instruction data；
3. Disease knowledge data: Based on the disease text knowledge of the Medical Knowledge Base, by setting specific question templates for semi-structured data (such as: "What are the typical symptoms of {disease}?" )Construct instruction data set, a total of 298K instruction data;

## Training Details

ChiMed was trained on six A800 GPUs (80GB). The currently released open-source content is the LoRA weights from the training process at the 12,400th step (total training duration of 114 hours and 46 minutes)；

### Download

[Lora Weights](https://pan.baidu.com/s/1wfMoMNVLWMgxY_n8CipVgg?pwd=xcbt)

## Performance Evaluation

### Drug Knowledge Question-Answering Evaluation 

We randomly selected 94 drug data for evaluation. Instructions were formed in the format of "{Drug}'s indications," and ChatGPT (gpt3.5), ChatGLM, and ChiMed were prompted to provide answers. Professional medical experts then compared the answers from the three models against the drug's official prescribing information and assigned scores based on the following three criteria:

- Criterion 1: The model answer is correct if it matches at least one indication in the drug's prescribing information.

- Criterion 2: The model answer is correct if the number of indications it matches is greater than or equal to half the total number of indications listed in the drug's prescribing information.

- Criterion 3: The model answer is correct if the number of indications it matches is greater than or equal to two-thirds of the total number of indications listed in the drug's prescribing information.

| Model      | Criterion 1 | Criterion 2 | Criterion 3 |
| ---------- | ----------- | ----------- | ----------- |
| ChatGLM    | 39.36%      | 23.16%      | 14.74%      |
| ChatGPT    | 47.87%      | 30.85%      | 15.96%      |
| **ChiMed** | **91.49%**  | **82.98%**  | **72.34%**  |

### Disease Knowledge Question-Answering Evaluation

We randomly selected 100 diseases data for evaluation. Instructions were constructed in the form of "Which drugs can treat {Disease}?", "{Disease} requires what tests?", and "What are the clinical manifestations of {Disease}?" to form the "Treatment Drugs," "Diagnostic Tests," and "Clinical Manifestations" instructions. ChatGPT (gpt3.5), ChatGLM, and ChiMed were prompted to provide answers. Professional medical experts then compared the answers from the three models against the Medical Knowledge Base's disease knowledge and assigned scores based on the following three criteria:

- Criterion 1: The model answer is correct if it matches at least one "Treatment Drug" ("Diagnostic Test," "Clinical Manifestation").

- Criterion 2: The model answer is correct if the number of "Treatment Drugs" ("Diagnostic Tests," "Clinical Manifestations") it matches is greater than or equal to half the total number of indications listed in the disease's medical knowledge.

- Criterion 3: The model answer is correct if the number of "Treatment Drugs" ("Diagnostic Tests," "Clinical Manifestations") it matches is greater than or equal to two-thirds of the total number of indications listed in the disease's medical knowledge.

| Model      | Clinical Manifestation Criterion 1 | Clinical Manifestation Criterion 2 | Clinical Manifestation Criterion 3 | Diagnostic Tests Criterion 1 | Diagnostic Tests Criterion 2 | Diagnostic Tests Criterion 3 | Treatment Drugs Criterion 1 | Treatment Drugs Criterion 2 | Treatment Drugs Criterion 3 |
| ---------- | ---------------------------------- | ---------------------------------- | ---------------------------------- | ---------------------------- | ---------------------------- | ---------------------------- | --------------------------- | --------------------------- | --------------------------- |
| chatglm    | 90.00%                             | 6.00%                              | 3.00%                              | 93.00%                       | 11.00%                       | 6.00%                        | 60.00%                      | 10.00%                      | 5.00%                       |
| chatgpt    | 94.00%                             | 11.00%                             | 4.00%                              | **97.00%**                   | 8.00%                        | 5.00%                        | 62.00%                      | 11.00%                      | 4.00%                       |
| **ChiMed** | **95.00%**                         | **15.00%**                         | **7.00%**                          | **97.00%**                   | **20.00%**                   | **7.00%**                    | **75.00%**                  | **36.00%**                  | **23.00%**                  |

## Quick Start

1. Environment Configuration

```bash
pip install -r requirements.txt
```

2. Download **[KnowLM](https://github.com/zjunlp/KnowLM)**-13B
3. Download LoRA weights and place them in the lora directory
4. Modify the model position parameters in the `gradio_demo.py` 
5. Launch demo

```bash
python gradio_demo.py
```

## Acknowledgment

We are very grateful to the following open source projects for their help:

- [Meta AI LLaMA](https://arxiv.org/abs/2302.13971v1)
- [Huggingface Transformers Llama](https://github.com/huggingface/transformers/tree/main/src/transformers/models/llama)
- [Alpaca](https://crfm.stanford.edu/2023/03/13/alpaca.html) and [Alpaca-LoRA](https://github.com/tloen/alpaca-lora)
- [KnowLM](https://github.com/AetherCortex/Llama-X)

