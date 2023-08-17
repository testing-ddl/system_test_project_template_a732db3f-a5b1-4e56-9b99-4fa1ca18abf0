*Disclaimer - Domino Reference Projects are starter kits built by Domino researchers. They are not officially supported by Domino. Once loaded, they are yours to use or modify as you see fit. We hope they will be a beneficial tool on your journey!

# OSS LLM Inference Reference Project
Project to show how to generate text output from LLMs using different inference frameworks

* [ft_falcon7b_8bit_lora.ipynb](ft_falcon7b_8bit_lora.ipynb) : This notebook contains code to fine tune a LoRA adapter for the Falcon-7b model to perform summarization

* [convert_hf_ct.ipynb](convert_hf_ct.ipynb) : : This notebook contains code to convert a Huggingface model to a `ctranslate2` model. `ctranslate2` does not support adapters out of the box so we merge it with the model and export it for subsequent use

* [bench_ct2.ipynb](bench_ct2.ipynb) : This notebook contains code that loads a `ctranslate2` model and generates output from it. 

* [bench_vllm.ipynb](bench_vllm.ipynb) :  This notebook contains code that uses `vLLM` to generate output from a Huggingface model that has the summarization adapter attached to it
  
* [app.sh](app.sh) : The shell script needed to run the chat app

* [app.py](app.py) : Streamlit app code for the summarization app. This app uses the `ctranslate2` model to generate responses

* [model.py](model.py) :
  
* ![Summarization app](summarization_app.png)

## Setup instructions

This project requires the following [compute environments](https://docs.dominodatalab.com/en/latest/user_guide/f51038/environments/) to be present:

### PromptEngineering
**Environment Base** 

`nvcr.io/nvidia/pytorch:22.12-py3`

**Dockerfile Instructions**

```
# System-level dependency injection runs as root
USER root:root

# Validate base image pre-requisites
# Complete requirements can be found at
# https://docs.dominodatalab.com/en/latest/user_guide/a00d1b/automatic-adaptation-of-custom-images/#_pre_requisites_for_automatic_custom_image_compatibility_with_domino
RUN /opt/domino/bin/pre-check.sh

# Configure /opt/domino to prepare for Domino executions
RUN /opt/domino/bin/init.sh

# Validate the environment
RUN /opt/domino/bin/validate.sh

RUN pip uninstall --yes torch torchvision torchaudio

RUN pip install torch  --index-url https://download.pytorch.org/whl/cu118

RUN pip uninstall -y protobuf
RUN pip install "protobuf==3.20.3" "mlflow==2.6.0"
RUN pip install -q -U bitsandbytes==0.39.1 "datasets>=2.10.0,<3" "ipywidgets" "ctranslate2==3.17.1"
RUN pip install -q -U py7zr einops tensorboardX transformers peft accelerate deepspeed
RUN pip install --no-cache-dir Flask Flask-Compress Flask-Cors jsonify uWSGI streamlit streamlit-chat "vllm==0.1.3"
RUN pip uninstall --yes transformer-engine

RUN pip uninstall -y apex
```
