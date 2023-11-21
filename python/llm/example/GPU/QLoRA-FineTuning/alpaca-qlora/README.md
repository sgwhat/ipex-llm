# Alpaca QLoRA Finetuning (experimental support)

This example ports [Alpaca-LoRA](https://github.com/tloen/alpaca-lora/tree/main) to BigDL-LLM QLoRA on [Intel GPUs](../../README.md).

### 0. Requirements
To run this example with BigDL-LLM on Intel GPUs, we have some recommended requirements for your machine, please refer to [here](../../README.md#requirements) for more information.

### 1. Install

```bash
conda create -n llm python=3.9
conda activate llm
# below command will install intel_extension_for_pytorch==2.0.110+xpu as default
# you can install specific ipex/torch version for your need
pip install --pre --upgrade bigdl-llm[xpu] -f https://developer.intel.com/ipex-whl-stable-xpu
pip install transformers==4.34.0
pip install fire datasets peft==0.5.0
pip install oneccl_bind_pt==2.0.100 -f https://developer.intel.com/ipex-whl-stable-xpu # necessary to run distributed finetuning
pip install accelerate==0.23.0
```

### 2. Configures OneAPI environment variables
```bash
source /opt/intel/oneapi/setvars.sh
```

### 3. Finetune

Here, we provide example usages on different hardware. Please refer to the appropriate script based on your device:

#### Finetuning LLaMA2-7B on single Arc A770

```bash
bash finetune_llama2_7b_arc_1_card.sh
```

#### Finetuning LLaMA2-7B on two Arc A770

```bash
bash finetune_llama2_7b_arc_2_card.sh
```

#### Finetuning LLaMA2-7B on single Data Center GPU Flex 170

```bash
bash finetune_llama2_7b_flex_170_1_card.sh
```

#### Finetuning LLaMA2-7B on three Data Center GPU Flex 170

```bash
bash finetune_llama2_7b_flex_170_3_card.sh
```

#### Finetuning LLaMA2-7B on single Intel Data Center GPU Max 1100

```bash
bash finetune_llama2_7b_pvc_1100_1_card.sh
```

#### Finetuning LLaMA2-7B on four Intel Data Center GPU Max 1100

```bash
bash finetune_llama2_7b_pvc_1100_4_card.sh
```

#### Finetuning LLaMA2-7B on single Intel Data Center GPU Max 1550

```bash
bash finetune_llama2_7b_pvc_1550_1_card.sh
```

#### Finetuning LLaMA2-7B on four Intel Data Center GPU Max 1550

```bash
bash finetune_llama2_7b_pvc_1550_4_card.sh
```

### 4. Sample Output
```log
{'loss': 1.9231, 'learning_rate': 2.9999945367033285e-05, 'epoch': 0.0}                                                                                                                            
{'loss': 1.8622, 'learning_rate': 2.9999781468531096e-05, 'epoch': 0.01}                                                                                                                           
{'loss': 1.9043, 'learning_rate': 2.9999508305687345e-05, 'epoch': 0.01}                                                                                                                           
{'loss': 1.8967, 'learning_rate': 2.999912588049185e-05, 'epoch': 0.01}                                                                                                                            
{'loss': 1.9658, 'learning_rate': 2.9998634195730358e-05, 'epoch': 0.01}                                                                                                                           
{'loss': 1.8386, 'learning_rate': 2.9998033254984483e-05, 'epoch': 0.02}                                                                                                                           
{'loss': 1.809, 'learning_rate': 2.999732306263172e-05, 'epoch': 0.02}                                                                                                                             
{'loss': 1.8552, 'learning_rate': 2.9996503623845395e-05, 'epoch': 0.02}                                                                                                                           
  1%|█                                                                                                                                                         | 8/1164 [xx:xx<xx:xx:xx, xx s/it]
```