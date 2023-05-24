

# GPTQLoRA: Efficient Finetuning of Quantized LLMs with GPTQ

QLoRA uses [AutoGPTQ](https://github.com/PanQiWei/AutoGPTQ) for quantization

## License and Intended Use
We release the resources associated with QLoRA finetuning in this repository under MIT license.

## Installation
To load models in 4bits with transformers and bitsandbytes, you have to install accelerate and transformers from source and make sure you have the latest version of the bitsandbytes library (0.39.0). You can achieve the above with the following commands:
```bash
git clone https://github.com/PanQiWei/AutoGPTQ.git && cd AutoGPTQ
pip install .[triton]
cd ..
pip install -r requirements.txt
pip install bitsandbytes
pip install git+https://github.com/huggingface/transformers.git
pip install git+https://github.com/qwopqwop200/peft.git
pip install git+https://github.com/huggingface/accelerate.git
```

## Getting Started
The `qlora.py` code is a starting point for finetuning and inference on various datasets.
Basic command for finetuning a baseline model on the Alpaca dataset:
```bash
python qlora.py --model_path <path>
```

For models larger than 13B, we recommend adjusting the learning rate:
```bash
python qlora.py –learning_rate 0.0001 --model_path <path>
```

The file structure of the model checkpoint is as follows:
```
(bnb) root@/root/qlora-main# ls llama-7b/
config.json             gptq_model-4bit-128g.bin  special_tokens_map.json  tokenizer_config.json
generation_config.json  quantize_config.json      tokenizer.model
```
## Quantization
Quantization is based on AutoGPTQ. Also, to run the code, you first need a model converted to GPTQ.

## Paged Optimizer
You can access the paged optimizer with the argument `--optim paged_adamw_32bit`

## Known Issues and Limitations
Here a list of known issues and bugs. If your issue is not reported here, please open a new issue and describe the problem.

1. Resuming a LoRA training run with the Trainer currently runs on an error
2. Make sure that `tokenizer.bos_token_id = 1` to avoid generation issues.
3. Sometimes the loss is 0.0 in the first output. In this case you need to rerun the code.

## Acknoledgements
This code is based on [QLoRA](https://github.com/artidoro/qlora).

This repo builds on the [Stanford Alpaca](https://github.com/tatsu-lab/stanford_alpaca) and [LMSYS FastChat](https://github.com/lm-sys/FastChat) repos.
