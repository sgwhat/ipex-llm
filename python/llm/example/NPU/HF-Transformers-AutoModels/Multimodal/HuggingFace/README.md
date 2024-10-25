# Run Large Multimodal Model on Intel NPU
In this directory, you will find examples on how you could apply IPEX-LLM INT4 or INT8 optimizations on Large Multimodal Models on [Intel NPUs](../../../README.md). See the table blow for verified models.

## Verified Models

| Model      | Model Link                                                    |
|------------|----------------------------------------------------------------|
| Phi-3-Vision | [microsoft/Phi-3-vision-128k-instruct](https://huggingface.co/microsoft/Phi-3-vision-128k-instruct) |

## 0. Requirements
To run these examples with IPEX-LLM on Intel NPUs, make sure to install the newest driver version of Intel NPU.
Go to https://www.intel.com/content/www/us/en/download/794734/intel-npu-driver-windows.html to download and unzip the driver.
Then go to **Device Manager**, find **Neural Processors** -> **Intel(R) AI Boost**.
Right click and select **Update Driver** -> **Browse my computer for drivers**. And then manually select the unzipped driver folder to install.

## Example: Predict Tokens using `generate()` API
In the example [generate.py](./generate.py), we show a basic use case for a phi-3-vision model to predict the next N tokens using `generate()` API, with IPEX-LLM INT4 optimizations on Intel NPUs.
### 1. Install
#### 1.1 Installation on Windows
We suggest using conda to manage environment:
```bash
conda create -n llm python=3.10 libuv
conda activate llm

# install ipex-llm with 'npu' option
pip install --pre --upgrade ipex-llm[npu]
pip install torchvision
```

### 2. Runtime Configurations
For optimal performance, it is recommended to set several environment variables. Please check out the suggestions based on your device.
#### 2.1 Configurations for Windows

> [!NOTE]
> For optimal performance, we recommend running code in `conhost` rather than Windows Terminal:
> - Press <kbd>Win</kbd>+<kbd>R</kbd> and input `conhost`, then press Enter to launch `conhost`.
> - Run following command to use conda in `conhost`. Replace `<your conda install location>` with your conda install location.
> ```
> call <your conda install location>\Scripts\activate
> ```

**Following envrionment variables are required**:

```cmd
set BIGDL_USE_NPU=1
```

### 3. Running examples

```
python ./generate.py
```

Arguments info:
- `--repo-id-or-model-path REPO_ID_OR_MODEL_PATH`: argument defining the huggingface repo id for the Phi-3-vision model (e.g. `microsoft/Phi-3-vision-128k-instruct`) to be downloaded, or the path to the huggingface checkpoint folder. It is default to be `'microsoft/Phi-3-vision-128k-instruct'`, and more verified models please see the list in [Verified Models](#verified-models).
- `--lowbit-path LOWBIT_MODEL_PATH`: argument defining the path to save/load lowbit version of the model. If it is an empty string, the original pretrained model specified by `REPO_ID_OR_MODEL_PATH` will be loaded. If it is an existing path, the lowbit model in `LOWBIT_MODEL_PATH` will be loaded. If it is a non-existing path, the original pretrained model specified by `REPO_ID_OR_MODEL_PATH` will be loaded, and the converted lowbit version will be saved into `LOWBIT_MODEL_PATH`. It is default to be `''`, i.e. an empty string.
- `--image-url-or-path IMAGE_URL_OR_PATH`: argument defining the image to be infered. It is default to be `'http://farm6.staticflickr.com/5268/5602445367_3504763978_z.jpg'`.
- `--prompt PROMPT`: argument defining the prompt to be infered (with integrated prompt format for chat). It is default to be `'What is in the image?'`.
- `--n-predict N_PREDICT`: argument defining the max number of tokens to predict. It is default to be `32`.
- `--load_in_low_bit`: argument defining the `load_in_low_bit` format used. It is default to be `sym_int8`, `sym_int4` can also be used.

#### Sample Output
##### [microsoft/Phi-3-vision-128k-instruct](https://huggingface.co/microsoft/Phi-3-vision-128k-instruct)

```log
Inference time: xxxx s
-------------------- Prompt --------------------
Message: [{'role': 'user', 'content': '<|image_1|>\nWhat is in the image?'}]
Image link/path: http://farm6.staticflickr.com/5268/5602445367_3504763978_z.jpg
-------------------- Output --------------------


What is in the image?
 The image shows a young girl holding a white teddy bear. She is wearing a pink dress with a heart on it. The background includes a stone
```

The sample input image is (which is fetched from [COCO dataset](https://cocodataset.org/#explore?id=264959)):

<a href="http://farm6.staticflickr.com/5268/5602445367_3504763978_z.jpg"><img width=400px src="http://farm6.staticflickr.com/5268/5602445367_3504763978_z.jpg" ></a>