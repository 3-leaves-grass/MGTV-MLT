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

# 影视字幕翻译数据构造方法

## 1. 字幕数据规整与结构校验

数据构造以多来源字幕资产的统一治理为起点，覆盖原始 SRT 字幕、平台批量翻译结果、人工精翻字幕和多语种评测数据等类型。由于各类数据在剧名标注、集数表达、语种标识和译文版本上存在差异，需要先建立统一的剧集、集数、语种和版本关联规则，使中文原文与目标语种译文形成稳定、可追溯的条目级对应关系，为后续上下文构造、质量筛选和模型训练提供可靠前提。

## 2. 上下文增强的数据构造

影视对白具有明显的上下文依赖，单条字幕往往不足以表达完整语义。为提升模型对连续语境的学习能力，数据构造采用上下文窗口化方式，将连续若干字幕条目组织为训练样本，并保留片段前后的语境信息。

这种构造方式能够覆盖单句、短段和较长对白段等多种字幕形态，使数据更接近真实翻译任务。通过引入上下文，模型能够更好地学习人物称谓、代词指代、语气延续、剧情承接和综艺口语表达等影视字幕关键现象，减少孤立逐句翻译造成的语义偏差。

同时，窗口化构造仍然保持字幕条目之间的结构边界，不改变原始字幕的基本对齐关系。该设计兼顾了语义完整性和字幕结构稳定性，使训练数据既能提供充分上下文，又能符合字幕回填和质量检查要求。

## 3. 训练样本输入输出构造

在完成字幕窗口化后，进一步将每个窗口组织为面向模型训练的标准样本。单条训练样本由 system prompt、input 和 output 三部分构成，分别用于限定模型角色、描述翻译任务和提供目标译文。

system prompt 用于明确模型的任务身份和能力边界，核心定位为“影视剧语言翻译专家”，强调模型需要结合影视剧对白和上下文语境完成翻译。其内容如下：

```text
你是一个影视剧语言翻译专家，擅长根据影视剧对白并结合语境进行翻译
```

input 由目标语种、翻译要求、术语约束、上文、当前字幕片段和下文共同组成。其中，当前字幕片段按字幕编号组织，编号用于保持原始字幕条目的顺序和边界；上文和下文提供连续语境，用于辅助模型判断人物关系、代词指代、语气延续和剧情承接。input 模板如下：

````text
给定影视剧的一段上下文对白，结合当前影视剧的语境，将指定的对白段落翻译成{目标语种}

## 翻译要求
- 输出为 JSON 格式，键为字幕编号，值为翻译内容，确保翻译后的条目数量与原始字幕完全一致，每个编号的字幕保证原文和翻译一一对应
- 对专有名词、术语严格按照术语表执行
- 结合上下文语境，翻译当前文本段落

## 术语表
{术语表}

[上文]
{上文字幕}

[当前文本]
{
    "0": "待翻译字幕1",
    "1": "待翻译字幕2"
}

[下文]
{下文字幕}

## 输出格式
严格按照以下 JSON 格式输出，不得有任何额外内容：
```json
{
  "0": "翻译内容1",
  "1": "翻译内容2"
}
```
````

output 为当前字幕片段对应的目标语种译文。译文按与 input 中当前字幕片段一致的编号输出，保证译文条数、条目顺序和字幕边界与原文保持一致。output 模板如下：

```json
{
    "0": "翻译内容1",
    "1": "翻译内容2"
}
```

以一组中文译日语训练样本为例，system prompt 保持不变，input 可构造为：

````text
给定影视剧的一段上下文对白，结合当前影视剧的语境，将指定的对白段落翻译成日语

## 翻译要求
- 输出为 JSON 格式，键为字幕编号，值为翻译内容，确保翻译后的条目数量与原始字幕完全一致，每个编号的字幕保证原文和翻译一一对应
- 对专有名词、术语严格按照术语表执行
- 结合上下文语境，翻译当前文本段落

## 术语表

[上文]
我不理解
她为什么要破坏别人家庭
为什么要把我生下来
为什么我从一出生开始
就没有爸爸

[当前文本]
{
    "0": "直到她去年生病住院",
    "1": "我连她最后一面都没见到"
}

[下文]
我才明白
成年人的世界是复杂的
不是非黑即白的
你爸他真的很担心你
他刚刚还在问我

## 输出格式
严格按照以下 JSON 格式输出，不得有任何额外内容：
```json
{
  "0": "翻译内容1",
  "1": "翻译内容2"
}
```
````

对应 output 为：

```json
{
    "0": "去年彼女が入院するまで",
    "1": "最後の顔も見られなかった"
}
```

通过上述构造，训练样本不仅学习源文到译文的语义转换，也同步学习字幕编号对齐、上下文利用、术语约束和结构化输出能力。

## 4. 基于无参考 COMET 的质量感知筛选

为控制低质量译文对训练数据的影响，构造流程引入无参考 COMET 质量估计，对候选字幕逐条打分，并以分数作为过滤和隔离依据。

无参考 COMET 不依赖人工参考译文，只输入中文原文和机器译文，由质量估计模型预测译文对原文的语义忠实度和可用性。该方法适合用于大规模候选译文筛选，因为相关数据通常规模较大，且并非每条候选译文都具备人工参考译文。

设第 $i$ 条字幕的中文原文为 $x_i$，机器译文为 $y_i$，无参考 COMET 质量估计模型为 $Q$，则单条字幕质量分为：

$$
q_i = Q(x_i, y_i)
$$

其中，$q_i$ 表示该条译文的质量分，分值越高，说明译文与原文在语义和表达上的一致性越好。构造时以约定阈值 $\tau$ 进行低分隔离，当前口径采用 $0.4$ 作为低分切分线。设低分标记为 $L_i$，则：

$$
L_i =
\begin{cases}
1, & q_i < \tau \\
0, & q_i \ge \tau
\end{cases}
$$

对于连续高质量字幕片段，进一步按上下文窗口构造训练样本。设一个窗口包含 $n$ 条字幕，其最低分和平均分分别为：

$$
q_{\min} = \min(q_1, q_2, \ldots, q_n)
$$

$$
\bar{q} = \frac{1}{n}\sum_{i=1}^{n} q_i
$$

窗口准入优先关注最低分，即窗口内不得混入低于阈值的字幕条目；平均分用于观察片段整体质量。低分条目不与相邻高分片段直接合并，而是单独隔离或进入复核范围；连续高分片段则用于构造上下文样本。

通过上述机制，数据构造实现了从“全量候选数据”到“高可信训练数据”的转化。该方法在扩大数据规模的同时控制噪声，提升多语种字幕训练数据的整体质量。

# 影视字幕翻译评估体系

## 1. 评估定位

影视字幕翻译评估的目标不仅是判断译文与参考译文是否相似，更重要的是判断翻译结果是否满足长视频内容出海的实际交付要求。字幕译文需要同时满足语义准确、表达自然、条目稳定、时间轴可用和质量可追踪等要求，因此单一自动指标难以完整反映字幕翻译质量。

评估体系以自动评估为主体，用于大规模、可复现地衡量不同模型和不同批次译文的整体表现；必要时结合抽样复核，对影视字幕中的语气、人物关系、文化表达和观看体验进行补充判断。

## 2. 评估数据基础

评估集围绕真实影视字幕场景构建，覆盖 8 个语种，共 24,217 条句子级数据。

| 语种 | 数据量 |
| --- | ---: |
| 印尼语 | 5,049 |
| 英语 | 3,576 |
| 越南语 | 3,079 |
| 马来语 | 2,280 |
| 泰语 | 2,144 |
| 韩语 | 3,161 |
| 日语 | 3,113 |
| 阿语 | 1,815 |
| 合计 | 24,217 |

## 3. 自动评估方法

自动评估围绕语义质量和字幕结构两类目标展开。语义质量方面，采用有参考 COMET、语义相似度和 BLEU-2 等指标，从神经评估、语义接近度和词面匹配多个角度衡量译文质量。字幕结构方面，检查字幕条目数量、顺序、空译、漏译、错位、时间轴和格式合规性，判断译文是否具备回填和交付基础。

设第 $i$ 条字幕的中文原文为 $x_i$，模型译文为 $y_i$，人工参考译文为 $r_i$。有参考 COMET 评分记为：

$$
C_i = COMET(x_i, y_i, r_i)
$$

COMET 通过神经评估模型综合源文、参考译文和模型译文，预测译文与人工质量判断的一致程度，适合衡量译文是否准确还原原文语义。

语义相似度基于向量表示计算模型译文与参考译文的语义接近程度，公式为：

$$
E_i = \frac{v(y_i) \cdot v(r_i)}{\|v(y_i)\| \|v(r_i)\|}
$$

其中，$v(\cdot)$ 表示文本向量。该指标可补充判断同义改写、语序变化和表达差异下的语义一致性。

BLEU-2 用于衡量模型译文与参考译文在二元词组层面的重合度，记为：

$$
B_i = BLEU_2(y_i, r_i)
$$

影视字幕多为短句，BLEU-2 对局部词面匹配和短语一致性具有补充价值，因此在加权评价值中采用加倍处理。

为便于同一评估集内的模型和批次横向比较，自动评估进一步形成加权评价值，计算公式如下：

$$
S_i = 0.20E_i + 0.35C_i + 0.25 \times B_i + 0.10T_i + 0.10F_i
$$

其中，$S_i$ 为加权评价值，$T_i$ 为时间轴准确度，$F_i$ 为格式合规度。时间轴准确度用于判断字幕句段与视频时间信息是否保持稳定；格式合规度用于判断标点、换行、结构和字幕交付格式是否满足要求。

自动评估结果按总体和语种分别统计，形成平均评价值、分语种评价值和结构异常统计。通过统一数据、统一指标和统一口径，可以较为客观地比较不同翻译方案在影视字幕场景下的适用性。




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




