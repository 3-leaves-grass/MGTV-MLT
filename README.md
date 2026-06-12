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
user_content =  "你是一个影视剧语言翻译专家，擅长根据影视剧对白并结合语境把中文翻译成其他语言\n\n"
    "给定影视剧的一段上下文对白，结合当前影视剧的语境,将指定的对白翻译成英语\n\n"
    上下文对白如下：
    [上文]\n皇上已经病入膏肓了\n他病了?\n说了不见就不见\n病死也不见\n娘娘\n\n
    [当前文本] \n'''json{\n  \"1\": \"皇后你也不该太没样了\",\n  \"2\": \"皇上重病\",\n  \"3\": \"你怎么也是个皇后     \"\n}'''\n\n
    [下文] \n应该照应他不是\n你这样的话\n成何体统啊?\n真有这么严重啊?\n给太后请安"

    "## 格式要求\n"
    "- 输入输出格式必须完全一致：JSON格式，键为字幕编号，值为翻译内容\n"
    "- 确保翻译后的条目数量与原始字幕完全一致\n"
    "- 对专有名词、术语严格按照术语表执行\n"
    "- 给定上下文，翻译当前文本，不要遗漏任何字幕\n\n"
    "## 输出格式\n"
    "严格按照以下JSON格式输出，不得有任何额外内容:\n"
    "```json\n"
    "{{\n"
    '  "1": "翻译内容1",\n'
    '  "2": "翻译内容2",\n'
    "  ...\n"
    "}}\n"
    "```"
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
infer_request = InferRequest(messages=[{
    "role": "user",
    "content": 'who are you?',
}])
request_config = RequestConfig(max_tokens=128, temperature=0)
resp_list = engine.infer([infer_request], request_config=request_config)
response = resp_list[0].choices[0].message.content
print(response)
```

## SGLang Server Usage

## vLLM Server Usage

## KTransformers Server Usage

## API Usage


# Evaluation

## Performance of our benchmark

## Performance of Flores200

