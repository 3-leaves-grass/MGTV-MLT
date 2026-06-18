# 影视字幕翻译背景

## 1. 面向内容出海的多语种字幕需求

随着影视剧、综艺、纪录片等长视频内容持续拓展海外传播渠道，多语种字幕已成为内容国际化分发的重要基础能力。字幕不仅承担语言转换功能，还直接影响海外用户对剧情推进、人物关系、情绪表达、文化信息和节目风格的理解。对于长视频平台而言，字幕翻译质量已经从单一语言服务问题，发展为影响内容出海效率、海外用户体验和多语种内容资产建设的关键环节。

在实际生产中，字幕翻译还与审核分发、成片回填、AI 配音、多平台复用等后续环节紧密相关。译文如果存在条目错位、时间轴破坏、漏译错译或表达不自然等问题，不仅会增加人工校对成本，也会影响整集内容的交付稳定性。因此，面向影视字幕场景建设可规模化、可追踪、可持续优化的智能翻译能力，具有明确的业务必要性。

## 2. 影视字幕翻译的场景特殊性

影视字幕不同于普通通用文本。普通机器翻译通常以独立句子为基本单位，而影视字幕天然具有“短条目、强上下文、严结构”的特点。单条字幕往往只是完整语义的一部分，人物称谓、代词指代、语气变化和剧情承接都需要依赖前后文才能准确判断。

同时，字幕翻译必须遵守原始字幕条目和时间轴约束。译文既要准确表达原意，又要适合屏幕阅读节奏，不能因译文过长影响观看体验，也不能因合并、拆分或漏译破坏字幕回填。对于综艺、影视对白等内容，还存在大量口语表达、节目化用语、文化梗、人名别称和固定术语，通用翻译模型容易出现语义理解不足、风格不一致和专名翻译波动等问题。

因此，影视字幕翻译不是简单的文本替换，而是一个同时受语义、结构、时间轴、术语、口语风格和交付规范约束的复杂生产任务。

## 3. 传统翻译方式的不足

传统逐句机翻或直接调用通用大模型的方式，在短文本翻译场景中具备一定可用性，但难以充分满足长视频字幕生产要求。其主要不足体现在：一是孤立处理单条字幕，容易忽略上下文中的人物关系和剧情承接；二是对字幕条目结构缺少稳定约束，批量翻译时容易发生漏译、错位或条目数量不一致；三是对影视专名、称谓和节目术语缺少持续一致的约束机制；四是缺少面向大规模交付的质量评估与问题回流流程。

这些问题在单条样例中可能不明显，但在整集、整季、多语种批量翻译中会持续放大，最终表现为质量不稳定、人工返工成本高和上线交付不可控。

## 4. 建设必要性与应用价值

围绕上述业务痛点，面向影视字幕翻译场景建设智能化翻译能力，目标是形成“结构可靠、语义准确、质量可控、可规模化交付”的技术体系。该体系以字幕结构保持为基础，以上下文增强翻译为核心，以术语约束、质量评估和异常校正为保障，支撑长视频多语种内容的稳定生产。

能力建设的重点不在于简单调用翻译接口，而在于将影视字幕的真实生产约束纳入统一流程：在译前完成字幕结构识别和语义组织，在译中结合上下文和术语约束提升表达一致性，在译后通过自动检查和人工复核形成质量闭环。通过这一体系，字幕翻译能力能够从“文本可翻译”提升为“字幕可回填、可审核、可播出、可复用”。

该字幕翻译能力能够为长视频内容出海提供稳定的多语种字幕生产基础。一方面，可提升海外内容本地化效率，缩短字幕生产周期，降低人工校对和返工成本；另一方面，可沉淀影视垂类语料、术语资产和质量反馈，为后续模型训练、语种扩展和 AI 配音等应用提供持续支撑。

从平台能力建设角度看，该体系有助于形成面向影视字幕生产的智能翻译闭环，增强多语种内容资产的可复用性和可扩展性，对网络视听平台建设自主可控的多语种译制能力具有重要意义。



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

### Benchmark Introduction

The goal of film and television subtitle translation evaluation is not merely to measure the similarity between model translations and human reference translations; more importantly, it is to verify whether translated outputs satisfy the practical delivery standards for long-form video content going global. Subtitle translations are required to simultaneously meet multiple criteria including semantic accuracy, natural wording, stable segmentation, valid timestamps, and traceable quality. For this reason, a single automated metric cannot fully capture the overall quality of subtitle translations. To address this gap, we propose a new benchmark for audiovisual translation. Within this benchmark, we construct a novel evaluation framework alongside a high-quality evaluation dataset.
Centered on automated assessment, the evaluation framework enables large-scale, reproducible measurement of the overall performance of diverse models and translation batches. When necessary, random sampling manual review is incorporated to provide supplementary judgments on subtle dimensions of subtitles such as tone, character dynamics, culturally specific expressions, and audience viewing experience. We have released the [benchmark datasets](https://huggingface.co/datasets/xxx) in hugging face.

#### Benchmark dataset

The evaluation set is constructed based on real-world film and television subtitle scenarios, covering 11 languages with a total of 8019 sentence-level samples.

| Language | Sample Count |
| --- | ---: |
| Indonesian(印尼语) | 1077 |
| English(英语) | 762 |
| Vietnamese(越南语) | 657 |
| Malay(马来语) | 489 |
| Thai(泰语) | 457 |
| Korean(韩语) | 699 |
| Japanese(日语) | 665 |
| Arabic(阿语) | 388 |
| French(法语)  | 1026|
| Spanish(西语)  | 523|
| Russian(俄语)  | 1026|
| All | 8019 |

#### Evaluation Criteria

Automated evaluation is conducted around two core objectives: semantic quality and subtitle structural compliance. For semantic quality, reference-based COMET, semantic similarity, and BLEU-2 metrics are employed to evaluate translation quality from multiple dimensions including neural assessment, semantic proximity, and surface n-gram matching. In terms of subtitle structure, we inspect the count and order of subtitle segments, empty translations, missing translations, misalignment, timestamp validity, and format compliance to determine whether the translated subtitles can be used for backfilling and formal delivery

Let $x_i$ denote the Chinese source text of the the i-th subtitle segment, $y_i$ the model-generated translation, and $r_i$ the human reference translation. The reference-based COMET score is defined as:

$$
C_i = COMET(x_i, y_i, r_i)
$$

As a neural evaluation model, COMET integrates source text, human reference translation and machine translation output to predict the consistency between translations and human quality judgments. It is suitable for measuring whether the semantics of the source text are accurately preserved in translations.

Semantic similarity quantifies the semantic proximity between model translations and reference translations based on text embeddings, as formulated below:

$$
E_i = \frac{v(y_i) \cdot v(r_i)}{\|v(y_i)\| \|v(r_i)\|}
$$

where，$v(\cdot)$ represents the text embedding vector. This metric serves as a supplementary indicator for semantic consistency amid paraphrasing, word order adjustments and divergent expressions. BLEU-2 measures bigram overlap between model translations and reference translations, denoted as:

$$
B_i = BLEU_2(y_i, r_i)
$$

Since film and television subtitles mostly consist of short utterances, BLEU-2 provides supplementary information on local surface matching and phrase consistency. Thus, its weight is doubled in the weighted composite score.

To enable horizontal comparison of different models and translation batches on the same evaluation set, we derive a weighted composite score for automated evaluation with the formula as follows:

$$
S_i = 0.20E_i + 0.35C_i + 0.25 \times B_i + 0.10T_i + 0.10F_i
$$

In the formula, $S_i$ refers to the weighted composite score, $T_i$ stands for timestamp accuracy, $F_i$ represents format compliance. Timestamp accuracy evaluates whether subtitle segments maintain stable alignment with video timecodes; format compliance verifies that punctuation, line breaks, layout structure and final subtitle delivery format conform to specifications.



### Steps
#### Step 1
Download benchmark [eval datasets] (https://huggingface.co/datasets/xxx)


#### Step 2

```
xxxx
```

#### Evaluate 

```
python evaluate.py
```

### Performance 
We evaluate our Mango-MT against three commercial large models (DeepSeek-v4-pro, Gemini-3-pro, GPT-5.4) across 11 languages with five core translation metrics: Semantic similarity, BLEU-2, COMET, Timeline, Format

Following  evaluation protocols, we adopt five core translation metrics  to quantify translation quality. On translation benchmark, Mango-MT outperforms Gemini, DeepSeek and GPT across all 11 evaluated languages. This demonstrates that our model delivers powerful multilingual translation capabilities for film and drama content, and possesses significant commercial deployment value.

 Mango-MT model:

| Language| Sample Count | Semantic similarity  | BLEU-2   | COMET    | Timeline | Format | Overall Score |
|--------|--------|------------|----------|----------|--------|------|----------|
| Overall   | 8020   | 0.859403   | 0.449146 | 0.900888 | 1.0    | 1.0  | **0.911764** |
| Malay | 490    | 0.816903   | 0.393724 | 0.916367 | 1.0    | 1.0  | **0.880971** |
| Thai   | 457    | 0.836646   | 0.481828 | 0.905263 | 1.0    | 1.0  | **0.925085** |
| English   | 762    | 0.862242   | 0.440677 | 0.933643 | 1.0    | 1.0  | **0.919562** |
| Indonesian | 1077   | 0.848496   | 0.501265 | 0.940874 | 1.0    | 1.0  | **0.949637** |
| Vietnamese | 657    | 0.858959   | 0.505468 | 0.939634 | 1.0    | 1.0  | **0.953398** |
| Russian   | 1276   | 0.875878   | 0.457478 | 0.884543 | 1.0    | 1.0  | **0.913505** |
| French   | 1026   | 0.879256   | 0.472997 | 0.860401 | 1.0    | 1.0  | **0.913490** |
| Japanese   | 665    | 0.858157   | 0.426881 | 0.889264 | 1.0    | 1.0  | **0.896314** |
| Korean   | 699    | 0.860051   | 0.326254 | 0.878718 | 1.0    | 1.0  | **0.842689** |
| Spanish   | 523    | 0.874014   | 0.500081 | 0.891235 | 1.0    | 1.0  | **0.936776** |
| Arabic   | 388    | 0.839917   | 0.357656 | 0.868953 | 1.0    | 1.0  | **0.850945** |

DeepSeek-v4-pro:

| Language   | Sample Count | Semantic Similarity | BLEU-2   | COMET    | Timeline | Format | Overall Score |
|------------|--------------|---------------------|----------|----------|----------|--------|---------------|
| Overall    | 8020         | 0.806946            | 0.312899 | 0.881785 | 1.0      | 1.0    | 0.826464      |
| Malay      | 490          | 0.772937            | 0.269850 | 0.900785 | 1.0      | 1.0    | 0.804787      |
| Thai       | 457          | 0.780416            | 0.351148 | 0.885932 | 1.0      | 1.0    | 0.841733      |
| English    | 762          | 0.835910            | 0.372428 | 0.920924 | 1.0      | 1.0    | 0.875720      |
| Indonesian | 1077         | 0.745968            | 0.247460 | 0.929048 | 1.0      | 1.0    | 0.798091      |
| Vietnamese | 657          | 0.791891            | 0.385748 | 0.929119 | 1.0      | 1.0    | 0.876444      |
| Russian    | 1276         | 0.824543            | 0.294146 | 0.854199 | 1.0      | 1.0    | 0.810952      |
| French     | 1026         | 0.833093            | 0.343295 | 0.831537 | 1.0      | 1.0    | 0.829304      |
| Japanese   | 665          | 0.828781            | 0.344643 | 0.869252 | 1.0      | 1.0    | 0.842316      |
| Korean     | 699          | 0.814358            | 0.207775 | 0.846163 | 1.0      | 1.0    | 0.762916      |
| Spanish    | 523          | 0.811760            | 0.375534 | 0.880326 | 1.0      | 1.0    | 0.858233      |
| Arabic     | 388          | 0.834742            | 0.295438 | 0.875921 | 1.0      | 1.0    | 0.821240      |

GPT-5.4:

|Language|Sample Count|Semantic Similarity|BLEU\-2|COMET|Timeline|Format|Overall Score|
|---|---|---|---|---|---|---|---|
|Overall|8020|0\.810158|0\.313546|0\.877375|1\.0|1\.0|0\.825886|
|Malay|490|0\.768620|0\.265427|0\.897863|1\.0|1\.0|0\.800690|
|Thai|457|0\.803542|0\.382755|0\.889516|1\.0|1\.0|0\.863416|
|English|762|0\.832979|0\.360511|0\.913667|1\.0|1\.0|0\.866635|
|Indonesian|1077|0\.750451|0\.258435|0\.925707|1\.0|1\.0|0\.803305|
|Vietnamese|657|0\.795900|0\.384080|0\.925085|1\.0|1\.0|0\.875000|
|Russian|1276|0\.828267|0\.298187|0\.851815|1\.0|1\.0|0\.812882|
|French|1026|0\.843068|0\.347100|0\.827232|1\.0|1\.0|0\.831695|
|Japanese|665|0\.827816|0\.358832|0\.858844|1\.0|1\.0|0\.845575|
|Korean|699|0\.822028|0\.209970|0\.842269|1\.0|1\.0|0\.764185|
|Spanish|523|0\.807804|0\.362856|0\.875232|1\.0|1\.0|0\.849320|
|Arabic|388|0\.820404|0\.238392|0\.865532|1\.0|1\.0|0\.786213|

Multilingual Subtitle Translation Evaluation Results based on gemini-3-pro:
|Language|Sample Count|Semantic Similarity|BLEU\-2|COMET|Timeline|Format|Overall Score|
|---|---|---|---|---|---|---|---|
|Overall|8019|0\.821596|0\.346600|0\.889810|1\.0|1\.0|0\.849053|
|Malay|490|0\.787010|0\.304621|0\.906635|1\.0|1\.0|0\.827035|
|Thai|457|0\.823896|0\.438264|0\.908024|1\.0|1\.0|0\.901719|
|English|762|0\.845588|0\.389542|0\.926590|1\.0|1\.0|0\.888195|
|Indonesian|1077|0\.772867|0\.305272|0\.934462|1\.0|1\.0|0\.834271|
|Vietnamese|657|0\.810758|0\.410032|0\.933933|1\.0|1\.0|0\.894044|
|Russian|1276|0\.838988|0\.328368|0\.866350|1\.0|1\.0|0\.835204|
|French|1026|0\.847074|0\.377884|0\.841096|1\.0|1\.0|0\.852740|
|Japanese|665|0\.835018|0\.368309|0\.874487|1\.0|1\.0|0\.857229|
|Korean|699|0\.830636|0\.237857|0\.853538|1\.0|1\.0|0\.783794|
|Spanish|523|0\.801747|0\.386711|0\.888592|1\.0|1\.0|0\.864712|
|Arabic|387|0\.831975|0\.296359|0\.875409|1\.0|1\.0|0\.820967|



## Evaluation of FLORES
We evaluate the multilingual translation performance of our model on FLORES+ which is based on FLORES-200. This dataset was originally released by FAIR researchers at Meta under the name FLORES.  The data is now being managed by OLDI, [the Open Language Data Initiative](https://oldi.org/). The + has been added to the name to disambiguate between the original datasets and this new actively developed version. For newer versions of this dataset, Please see [FLORES+ HuggingFace repo ](https://huggingface.co/datasets/openlanguagedata/flores_plus).
The data consists of translations primarily from English into over 200 language varieties. The original English sentences were sampled in equal amounts from [Wikinews](https://en.wikinews.org/wiki/Main_Page) (an international news source), [Wikijunior](https://en.wikibooks.org/wiki/Wikijunior) (a collection of age-appropriate non-fiction books), and [Wikivoyage](https://en.wikivoyage.org/wiki/Main_Page) (a travel guide).

For each language, the dataset has 997 sentences for the dev split and 1012 sentences for the devtest split. The separate blind test set, originally developed by Meta, is not managed by OLDI.

Following official FLORES evaluation protocols, we adopt two core universal metrics to quantify translation quality:

- spBLEU: SentencePiece-normalized BLEU with a unified 256k multilingual SentencePiece tokenizer, eliminating tokenization bias to enable equitable score comparison across all 200 languages.

- chrF++: Character-level F-score optimized for agglutinative, non-Latin, and low-orthography languages, complementing spBLEU to capture fine-grained lexical and character-level alignment.

We measure translation performance on FLORES-200 using BLEU and chrF++ on 11 languages (Russian, Indonesian, Japanese, French, Thai, English, Spanish, Vietnamese, Arabic, Korean, Malay) following official FLORES-200 evaluation rules.


### Dependencies

```
pip install sacrebleu sentencepiece
```
### Steps
#### Step 1
Download the devtest datasets of following languages from [flores devtest](https://huggingface.co/datasets/openlanguagedata/flores_plus) and put them into reference_data/. The description of 11 languages in flores are following：

| Code | Script | lang | file name |
|------|--------|------|-------------------|
| rus  | Cyrl   | Russian | rus_Cyrl.jsonl |
| ind  | Latn   | Indonesian | ind_Latn.jsonl |
| jpn  | Jpan   | Japanese | jpn_Jpan.jsonl |
| fra  | Latn   | French | fra_Latn.jsonl |
| tha  | Thai   | Thai | tha_Thai.jsonl |
| eng  | Latn   | English | eng_Latn.jsonl |
| spa  | Latn   | Spanish (Latin American) | spa_Latn.jsonl |
| vie  | Latn   | Vietnamese | vie_Latn.jsonl |
| ara  | Arab   | Modern Standard Arabic | arb_Arab.jsonl |
| kor  | Hang   | Korean | kor_Hang.jsonl |
| zsm  | Latn   | Standard Malay | zsm_Latn.jsonl |
| cmn  | Hans   | Mandarin Chinese (Standard Beijing) | cmn_Hans.jsonl |

#### Step 2

Put the downloaded cmn_Hans.jsonl into the src_data/, translate Chinese into 11 languages by using following .py based on our model, the translated results will be saved into the translated_data/:

```
cd flores200 && python translate_flores_with_Mango_MT.py
```

Similarly, translate Chinese into 11 languages by using following .py based on gpt, deepseek, gemini, the translated results will be saved into the translated_data/:

```
cd flores200 && python translate_flores_with_api.py
```
#### Evaluate 
we adopt two core universal metrics (BLEU, chrF++ ) to quantify translation quality by using following .py:


```
python evaluate.py
```

### Performance 
We evaluate our Mango-MT against three commercial large models (DeepSeek-v4-pro, Gemini-3-pro, GPT-5.4) across 11 languages with two core translation metrics: BLEU (short utterance fluency, core metric for subtitles) and chrF++ (character-level semantic matching).

Evaluation Results on  BLEU:
|Language|DeepSeek|Gemini|GPT|Mango\-MT|
|---|---|---|---|---|
|Russian|21\.11|22\.84|20\.26|**23\.33**|
|Indonesian|27\.80|28\.85|24\.98|**31\.75**|
|Japanese|29\.68|**31\.64**|27\.71|30\.99|
|French|31\.15|32\.84|28\.07|**33\.83**|
|Thai|10\.40|9\.48|8\.38|**10\.52**|
|English|33\.76|34\.09|30\.99|**40\.49**|
|Spanish|21\.10|22\.41|21\.39|**24\.57**|
|Vietnamese|32\.55|32\.73|30\.08|**34\.23**|
|Arabic|**16\.12**|17\.30|13\.24|16\.10|
|Korean|24\.91|**25\.22**|23\.11|24\.48|
|Malay|23\.43|24\.09|21\.12|**25\.52**|
|**Macro Average**|24\.73|25\.59|22\.67|**26\.89**|

Evaluation Results on chrF++:
|Language|DeepSeek|Gemini|GPT|Mango\-MT|
|---|---|---|---|---|
|Russian|**48\.23**|49\.85|**48\.23**|48\.30|
|Indonesian|56\.65|**58\.65**|55\.82|57\.97|
|Japanese|**30\.40**|30\.08|29\.01|28\.64|
|French|56\.15|**58\.36**|55\.21|57\.33|
|Thai|43\.53|**45\.24**|43\.49|42\.83|
|English|60\.00|61\.00|58\.84|**63\.78**|
|Spanish|48\.07|49\.28|48\.48|**49\.60**|
|Vietnamese|53\.08|**54\.42**|52\.55|53\.55|
|Arabic|45\.37|**47\.26**|43\.59|43\.99|
|Korean|31\.12|**32\.52**|30\.15|30\.22|
|Malay|54\.19|**55\.99**|53\.95|54\.25|
|**Macro Average**|47\.89|**49\.33**|47\.21|48\.23|


1. **BLEU (subtitle-focused metric)**
   Mango-MT achieves the highest macro average score (26.89), outperforming Gemini (25.59), DeepSeek (24.73) and GPT (22.67). It ranks first on 8 out of 11 languages, especially showing dominant advantages on English and Southeast Asian languages (Indonesian, Thai, Vietnamese, Malay), which fits our subtitle translation scenario perfectly.

2. **chrF++ (fine-grained semantic metric)**
   Gemini leads the macro average (49.33), followed by Mango-MT (48.23), DeepSeek (47.89) and GPT (47.21). Our model matches or outperforms commercial models across multiple languages.


