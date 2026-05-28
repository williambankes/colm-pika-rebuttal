# Colm-Pika-Rebuttal

This anon repo provides plots and additional results for the reviewers interest and hopefully in an easy to read format. These will be added to the appendix of the updated manuscript.

## Analysis of Probe Performance Across Layers

To better understand how the selected layer affects probe performance we trained a probe across every other layer i.e. {1,3,5,...27} of Qwen2.5-Math-1.5B-Instruct and Qwen/Qwen2.5-Math-7B-Instruct on the Math dataset - which has large validation and test sizes, 1.5k and 5k samples respectively. The results are shown in the table and figures below: 

<p align="center">
  <img src="https://github.com/williambankes/colm-pika-rebuttal/blob/main/figures/Qwen_Qwen2.5-Math-1.5B-Instruct__DigitalLearningGmbH_MATH-lighteval_layer_sweep.png?raw=true" width="49%" />
  <img src="https://github.com/williambankes/colm-pika-rebuttal/blob/main/figures/Qwen_Qwen2.5-Math-7B-Instruct__DigitalLearningGmbH_MATH-lighteval_layer_sweep.png?raw=true" width="49%" />
</p>

| Layer | Qwen/Qwen2.5-Math-1.5B-Instruct | Qwen/Qwen2.5-Math-7B-Instruct |
|---:|---:|---:|
| 1 | 0.7552 | 0.7663 |
| 3 | 0.7818 | 0.7780 |
| 5 | 0.7882 | 0.7869 |
| 7 | 0.7937 | 0.7974 |
| 9 | 0.8053 | 0.8064 |
| 11 | 0.8077 | 0.8189 |
| 13 | 0.8156 | 0.8232 |
| 15 | 0.8286 | 0.8427 |
| 17 | **0.8434** | **0.8515** |
| 19 | 0.8417 | 0.8504 |
| 21 | 0.8355 | 0.8465 |
| 23 | 0.8386 | 0.8453 |
| 25 | 0.8368 | 0.8379 |
| 27 | 0.8368 | 0.8436 |

We further analyse the logs of our experiments in Table 2 and present their best layer and best position ids in the table below, here best_pos_idx is the position relative to the end of the prompt. We note that out of 28 layers Qwen 2.5 models consistently find the best representation in the later layers of the network (>20) with the exception of DeepSeek-R1-Distill-Qwen-7B on the AIME dataset at layer 13. In the GPT-OSS model the average best layer decreases as the reasoning level increases 17.50, 15.75, 14.50 for low, medium, and high respectively. We do not find a consistent pattern across datasets e.g. AIME best layers vary from 13 to 28.

| model | dataset | best_layer_idx | best_pos_idx | best_val_score | test_score |
|---|---|---|---|---|---|
| Qwen2.5-Math-7B-Instruct | MATH | 21 | 4 | 0.891 | 0.846 |
| Qwen2.5-Math-7B-Instruct | AMC | 22 | 4 | 0.881 | 0.848 |
| Qwen2.5-Math-7B-Instruct | AIME | 28 | 1 | 0.867 | 0.679 |
| Qwen2.5-Math-7B-Instruct | GSM8K | 20 | 4 | 0.754 | 0.767 |
| DeepSeek-R1-Distill-Qwen-7B | MATH | 27 | 2 | 0.793 | 0.739 |
| DeepSeek-R1-Distill-Qwen-7B | AIME | 13 | 1 | 0.753 | 0.701 |
| DeepSeek-R1-Distill-Qwen-7B | GSM8K | 23 | 1 | 0.647 | 0.640 |
| gpt-oss-20b_high | MATH | 15 | 0 | 0.850 | 0.860 |
| gpt-oss-20b_high | AMC | 16 | 2 | 0.757 | 0.691 |
| gpt-oss-20b_high | AIME | 10 | 2 | 0.761 | 0.286 |
| gpt-oss-20b_high | GSM8K | 17 | 0 | 0.701 | 0.686 |
| gpt-oss-20b_low | MATH | 17 | 0 | 0.845 | 0.845 |
| gpt-oss-20b_low | AMC | 12 | 0 | 0.810 | 0.793 |
| gpt-oss-20b_low | AIME | 24 | 0 | 0.825 | 0.778 |
| gpt-oss-20b_low | GSM8K | 17 | 0 | 0.790 | 0.761 |
| gpt-oss-20b_medium | MATH | 15 | 0 | 0.840 | 0.850 |
| gpt-oss-20b_medium | AMC | 13 | 1 | 0.784 | 0.735 |
| gpt-oss-20b_medium | AIME | 19 | 2 | 0.778 | 0.568 |
| gpt-oss-20b_medium | GSM8K | 16 | 1 | 0.921 | 0.629 |

## Analysis of Probe Generalization Across Datasets

Here we analyse how our success probes generalise when trained on one dataset and evaluated on another. We note that training on the Math dataset leads to the best average AUROC results for the majority of models. Whilst AIME appears to be the worst for generalization. In specific instances the Math trained probes achieve better performance on AIME than those trained on the dataset itself, see Qwen2.5-Math-7B-Instruct and gpt-oss-20b-high. Both GSM8K and AIME often fail to generalize to the MATH dataset. 

| Model | Trained on | Eval: MATH | Eval: GSM8K | Eval: AIME | Avg ± SD |
|---|---|---:|---:|---:|---:|
| **Qwen2.5-1.5B-Instruct** | MATH | **0.844** | 0.727 | 0.931 | 0.834 ± 0.084 |
|  | GSM8K | 0.796 | **0.762** | 1.000 | 0.853 ± 0.105 |
|  | AIME | 0.783 | 0.709 | **0.931** | 0.808 ± 0.092 |
| **Qwen2.5-Math-1.5B-Instruct** | MATH | **0.841** | 0.730 | 0.654 | **0.742 ± 0.077** |
|  | GSM8K | 0.760 | **0.758** | 0.538 | 0.685 ± 0.104 |
|  | AIME | 0.740 | 0.605 | **0.712** | 0.686 ± 0.058 |
| **Qwen2.5-Math-7B-Instruct** | MATH | **0.846** | 0.720 | 0.901 | **0.822 ± 0.076** |
|  | GSM8K | 0.794 | **0.767** | 0.877 | 0.813 ± 0.047 |
|  | AIME | 0.745 | 0.640 | **0.679** | 0.688 ± 0.043 |
| **DeepSeek-R1-Distill-Qwen-7B** | MATH | **0.739** | 0.515 | 0.643 | **0.632 ± 0.092** |
|  | GSM8K | 0.589 | **0.640** | 0.502 | 0.577 ± 0.057 |
|  | AIME | 0.634 | 0.511 | **0.701** | 0.615 ± 0.079 |
| **gpt-oss-20b-low** | MATH | **0.845** | 0.710 | 0.611 | **0.722 ± 0.096** |
|  | GSM8K | 0.662 | **0.761** | 0.361 | 0.595 ± 0.170 |
|  | AIME | 0.650 | 0.614 | **0.778** | 0.681 ± 0.070 |
| **gpt-oss-20b-medium** | MATH | **0.850** | 0.725 | 0.504 | **0.693 ± 0.143** |
|  | GSM8K | 0.540 | **0.629** | 0.504 | 0.558 ± 0.053 |
|  | AIME | 0.558 | 0.577 | **0.568** | 0.568 ± 0.008 |
| **gpt-oss-20b-high** | MATH | **0.860** | 0.692 | 0.679 | **0.744 ± 0.082** |
|  | GSM8K | 0.656 | **0.686** | 0.214 | 0.519 ± 0.216 |
|  | AIME | 0.594 | 0.522 | **0.286** | 0.467 ± 0.132 |


## Performance of Probe Across Base/Instruct/Math-Instruct/Reasoning Models

To understand how the probe performance changes across Base, Instruct, Math-Instruct, and reasoning models we run probe training on variants of Qwen2.5-7B. The results are reported in the table below. The Base model probe achieves the lowest performance across the MATH and AIME datasets failing to answer any questions in AIME. The Math and Instruct models perform the best with the probe performance dropping again on the Reasoning R1-Distill model. 

| Qwen 2.5-7B | MATH | AIME | GSM8K |
|:---|:---:|:---:|:---:|
| Base | 0.7694 | NaN | 0.6661 |
| Math | 0.8463 | 0.6790 | 0.7665 |
| Instruct | 0.8375 | 0.7284 | 0.7766 |
| Reasoning (R1-Distill) | 0.7391 | 0.7014 | 0.6401 |

## Analysis of the Routing Splits

The table below shows the cost, accuracy, and the break down of how each router assigns questions to each model on the GSM8K, AIME, and Math datasets. In the GSM8K dataset the router uses the Math-7B model and GPT-OSS-low models to reduce the cost identifying that both are capable of answering the GSM8K questions. In AIME, a much harder dataset, our router divides the questions mostly between the Medium and High GPT-OSS models reflecting the need for extensive reasoning when answering these questions. 

| Strategy | Cost ($) | GSM8K Accuracy | Math-7B | R1-Qwen-7B | gpt-oss-20b-low | gpt-oss-20b-medium | gpt-oss-20b-high |
|---|---|---|---|---|---|---|---|
| Oracle | $0.4223 | 97.8% | 6% | 1% | 92% | 1% | 0% |
| Probe (λ=1.00) | $0.4318 | 94.1% | 39% | 0% | 60% | 1% | 0% |
| Probe (λ=0.59) | $0.4588 | 94.4% | 47% | 0% | 51% | 2% | 0% |
| Probe (λ=0.53) | $0.4713 | 94.5% | 47% | 0% | 49% | 3% | 0% |
| Probe (λ=0.37) | $0.5170 | 94.5% | 49% | 0% | 43% | 8% | 0% |
| Random | $0.9986 | 92.0% | 20% | 21% | 19% | 21% | 19% |
| SC-Entropy | $5.2471 | 95.8% | 94% | 3% | 1% | 2% | 1% |
| MIRT Router | $2.0130 | 94.1% | 14.4% | 1.5% | 0.3% | 6% | 77.8% |

| Strategy | Cost ($) | AIME Accuracy | Math-7B | R1-Qwen-7B | gpt-oss-20b-low | gpt-oss-20b-medium | gpt-oss-20b-high |
|---|---|---|---|---|---|---|---|
| Oracle | $0.5490 | 93.3% | 17% | 17% | 30% | 27% | 10% |
| Random | $0.5187 | 43.3% | 30% | 30% | 17% | 10% | 13% |
| SC-Entropy (best) | $2.7740 | 93.3% | 0% | 30% | 3% | 33% | 33% |
| Probe (λ=0.98) | $0.4216 | 66.7% | 3% | 10% | 30% | 57% | 0% |
| Probe (λ=0.27) | $0.5401 | 83.3% | 0% | 13% | 3% | 77% | 7% |
| Probe (λ=0.08) | $0.8607 | 86.7% | 0% | 7% | 0% | 57% | 37% |
| Probe (λ=0.04) | $1.1288 | 93.3% | 0% | 3% | 0% | 43% | 53% |
| MIRT Router | $0.9460 | 80.0% | 0% | 0% | 3% | 70% | 27% |


## Analysis of Routing Costs

We provide updated results for our routing experiments factoring in the additional cost of evaluating the probe across all 

![routing_figure](https://github.com/williambankes/colm-pika-rebuttal/blob/main/figures/GSM8K_routing_with_costs.png?raw=true)


