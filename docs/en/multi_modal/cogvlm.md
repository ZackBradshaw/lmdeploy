# cogvlm

## Introduction

CogVLM is a powerful open-source visual language model (VLM). LMDeploy supports CogVLM-17B models like [THUDM/cogvlm-chat-hf](https://huggingface.co/THUDM/cogvlm-chat-hf) and CogVLM2-19B models like [THUDM/cogvlm2-llama3-chat-19B](https://huggingface.co/THUDM/cogvlm2-llama3-chat-19B) in PyTorch engine.

## Quick Start

### Prepare model

Download CogVLM models using huggingface-cli.

```shell
# install huggingface-cli
pip install -U "huggingface_hub[cli]"
# download model
huggingface-cli download THUDM/cogvlm-chat-hf --local-dir ./cogvlm-chat-hf --local-dir-use-symlinks False
# cogvlm-chat-hf uses the tokenizer from lmsys/vicuna-7b-v1.5, we should download the files into the model directory. Skip this step for CogVLM2.
huggingface-cli download lmsys/vicuna-7b-v1.5 special_tokens_map.json tokenizer.model tokenizer_config.json --local-dir ./cogvlm-chat-hf --local-dir-use-symlinks False
```

### Offline inference pipeline

The following sample code shows the basic usage of VLM pipeline. For more examples, please refer to [VLM Offline Inference Pipeline](https://lmdeploy.readthedocs.io/en/latest/inference/vl_pipeline.html#vlm-offline-inference-pipeline)

```python
from lmdeploy import pipeline, PytorchEngineConfig
from lmdeploy.vl import load_image

pipe = pipeline('cogvlm-chat-hf', model_name='cogvlm', backend_config=PytorchEngineConfig(tp=1, max_prefill_token_num=4096, cache_max_entry_count=0.8))

image = load_image('https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/tests/data/tiger.jpeg')
response = pipe(('describe this image', image))
print(response)
```