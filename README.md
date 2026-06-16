# Installation
```
conda create -n mlt python==3.11
conda activate mlt
pip install -r requirements.txt
```

# Model Weights
Model checkpoints are accessible from xxx

# Usage

## Transformers Usage

```
from transformers import AutoModelForCausalLM, AutoTokenizer

# loading model
model = AutoModelForCausalLM.from_pretrained(
    model_dir, # Model checkpoints 
    torch_dtype=torch.float16, 
    trust_remote_code=True,
    device_map="auto"
)


#loading tokenizer
tokenizer = AutoTokenizer.from_pretrained(
    model_dir,
    trust_remote_code=True
)

# construct message
user_content =  """你是一个影视剧语言翻译专家，擅长根据影视剧对白并结合语境把中文翻译成其他语言给定影视剧的一段上下文对白，结合当前影视剧的语境,将指定的对白翻译成英语
## 格式要求
- 输入输出格式必须完全一致：JSON格式，键为字幕编号，值为翻译内容
- 确保翻译后的条目数量与原始字幕完全一致
- 对专有名词、术语严格按照术语表执行
- 给定上下文，翻译当前文本，不要遗漏任何字幕

## 输出格式
严格按照以下JSON格式输出，不得有任何额外内容:
'''json
{
  "1": "翻译内容1",
  "2": "翻译内容2",
  ...
}
'''
[上文]
启禀太后
皇上病重
但不见任何人
臣妾们担心的紧
不见任何人?

[当前文本] 
'''json{
  "1": "母后",
  "2": "既然皇上不见人",
  "3": "要不咱们还是走吧?"
}'''

[下文] 
你给我回来
皇上传皇后觐见
"""

messages = [{"role": "user", "content": user_content}]

text = tokenizer.apply_chat_template(
    messages,
    tokenize=False,
    add_generation_prompt=True
)
inputs = tokenizer(text, return_tensors="pt").to(device)

with torch.no_grad():
    generated_ids = model.generate(**inputs, max_new_tokens=1024)

input_len = len(inputs.input_ids[0])	
generated_text = tokenizer.decode(generated_ids[0][input_len:], skip_special_tokens=True)

print(f"translated_text: {generated_text}")

```

## Swift Usage
[ms-swift](https://github.com/modelscope/ms-swift) is a large model and multimodal large model fine-tuning and deployment framework provided by the ModelScope community.
```
pip install ms-swift -U
```
see the [documents](https://swift.readthedocs.io/en/latest/) for more details
```
import os
# os.environ['SWIFT_DEBUG'] = '1'
os.environ['CUDA_VISIBLE_DEVICES'] = '0'
os.environ['IMAGE_MAX_TOKEN_NUM'] = '1024'
os.environ['VIDEO_MAX_TOKEN_NUM'] = '128'
os.environ['FPS_MAX_FRAMES'] = '16'

from swift import get_model_processor, get_template
from swift.infer_engine import TransformersEngine, InferRequest, RequestConfig
model_dir = ""

enable_thinking = False

model, processor = get_model_processor(model_dir)  # attn_impl='flash_attention_2'
template = get_template(processor, enable_thinking=enable_thinking)
engine = TransformersEngine(model, template=template)
user_content =  """你是一个影视剧语言翻译专家，擅长根据影视剧对白并结合语境把中文翻译成其他语言给定影视剧的一段上下文对白，结合当前影视剧的语境,将指定的对白翻译成英语
## 格式要求
- 输入输出格式必须完全一致：JSON格式，键为字幕编号，值为翻译内容
- 确保翻译后的条目数量与原始字幕完全一致
- 对专有名词、术语严格按照术语表执行
- 给定上下文，翻译当前文本，不要遗漏任何字幕

## 输出格式
严格按照以下JSON格式输出，不得有任何额外内容:
'''json
{
  "1": "翻译内容1",
  "2": "翻译内容2",
  ...
}
'''
[上文]
启禀太后
皇上病重
但不见任何人
臣妾们担心的紧
不见任何人?

[当前文本] 
'''json{
  "1": "母后",
  "2": "既然皇上不见人",
  "3": "要不咱们还是走吧?"
}'''

[下文] 
你给我回来
皇上传皇后觐见
"""
infer_request = InferRequest(messages=[{
    "role": "user",
    "content": user_content,
}])
request_config = RequestConfig(max_tokens=128, temperature=0)
resp_list = engine.infer([infer_request], request_config=request_config)
response = resp_list[0].choices[0].message.content
print(response)
```

## SGLang Server Usage
[SGLang](https://github.com/sgl-project/sglang) is a fast serving framework for large language models and vision language models. Please use the following command in a fresh environment:
```
uv pip install 'git+https://github.com/sgl-project/sglang.git#subdirectory=python&egg=sglang[all]'
```
See its [documentation](https://docs.sglang.io/docs/get-started/install) for more details.

The following will create API endpoints at http://localhost:8000/v1:

```
python -m sglang.launch_server --model-path model_dir --port 8000 --tp-size 1 --mem-fraction-static 0.8 --context-length 4096 --reasoning-parser qwen3
```

## vLLM Server Usage
[vLLM](https://github.com/vllm-project/vllm) is a high-throughput and memory-efficient inference and serving engine for LLMs.  Please use the following command in a fresh environment:
```
uv pip install vllm --torch-backend=auto --extra-index-url https://wheels.vllm.ai/nightly
```
See its [documentation]([https://docs.sglang.io/docs/get-started/install](https://docs.vllm.ai/en/stable/getting_started/installation/index.html)) for more details.

For detailed usage guide, see the [vLLM Qwen3.5 recipe](https://docs.vllm.ai/projects/recipes/en/latest/Qwen/Qwen3.5.html).

The following will create API endpoints at http://localhost:8000/v1:
```
vllm serve model_dir --port 8000 --tensor-parallel-size 1 --max-model-len 4096 --reasoning-parser qwen3 --language-model-only
```

## API Usage
The chat completions API is accessible via standard HTTP requests or OpenAI SDKs. Here, we show examples using the OpenAI Python SDK.

Before starting, make sure it is installed and the API key and the API base URL is configured, e.g.:
```
pip install -U openai
```
Set the following accordingly
```
export OPENAI_BASE_URL="http://localhost:8000/v1"
export OPENAI_API_KEY="EMPTY"
```
An usage example
```
from openai import OpenAI
# Configured by environment variables
client = OpenAI()

user_content =  """你是一个影视剧语言翻译专家，擅长根据影视剧对白并结合语境把中文翻译成其他语言给定影视剧的一段上下文对白，结合当前影视剧的语境,将指定的对白翻译成英语
## 格式要求
- 输入输出格式必须完全一致：JSON格式，键为字幕编号，值为翻译内容
- 确保翻译后的条目数量与原始字幕完全一致
- 对专有名词、术语严格按照术语表执行
- 给定上下文，翻译当前文本，不要遗漏任何字幕

## 输出格式
严格按照以下JSON格式输出，不得有任何额外内容:
'''json
{
  "1": "翻译内容1",
  "2": "翻译内容2",
  ...
}
'''
[上文]
启禀太后
皇上病重
但不见任何人
臣妾们担心的紧
不见任何人?

[当前文本] 
'''json{
  "1": "母后",
  "2": "既然皇上不见人",
  "3": "要不咱们还是走吧?"
}'''

[下文] 
你给我回来
皇上传皇后觐见
"""
messages = [
    {"role": "user", "content": user_content},
]

chat_response = client.chat.completions.create(
    model=model_dir,  # Model checkpoints 
    messages=messages,
    max_tokens=4096,
    extra_body={
        "top_k": 20,
        "chat_template_kwargs": {"enable_thinking": False},
    }, 
)
print("Chat response:", chat_response)
```
# Evaluation

## Performance of our benchmark

## Performance of FLORES

We evaluate the multilingual translation performance of our model on FLORES+ which is based on FLORES-200. This dataset was originally released by FAIR researchers at Meta under the name FLORES.  The data is now being managed by OLDI, [the Open Language Data Initiative](https://oldi.org/). The + has been added to the name to disambiguate between the original datasets and this new actively developed version. For newer versions of this dataset, Please see [FLORES+ HuggingFace repo ](https://huggingface.co/datasets/openlanguagedata/flores_plus).
The data consists of translations primarily from English into over 200 language varieties. The original English sentences were sampled in equal amounts from [Wikinews](https://en.wikinews.org/wiki/Main_Page) (an international news source), [Wikijunior](https://en.wikibooks.org/wiki/Wikijunior) (a collection of age-appropriate non-fiction books), and [Wikivoyage](https://en.wikivoyage.org/wiki/Main_Page) (a travel guide).

For each language, the dataset has 997 sentences for the dev split and 1012 sentences for the devtest split. The separate blind test set, originally developed by Meta, is not managed by OLDI.

Following official FLORES evaluation protocols, we adopt two core universal metrics to quantify translation quality:

- spBLEU: SentencePiece-normalized BLEU with a unified 256k multilingual SentencePiece tokenizer, eliminating tokenization bias to enable equitable score comparison across all 200 languages.

- chrF++: Character-level F-score optimized for agglutinative, non-Latin, and low-orthography languages, complementing spBLEU to capture fine-grained lexical and character-level alignment.

We measure translation performance on FLORES-200 using BLEU and chrF++ on 11 languages  following official FLORES-200 evaluation rules.

Dependencies

```
pip install sacrebleu sentencepiece
```

