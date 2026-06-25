# Mango-MT: A 9B Model Bridging the Gap with Closed-Source Audiovisual MT

<img width="6303" height="1881" alt="f0" src="https://github.com/user-attachments/assets/68f72a09-0d9b-4e95-b9e6-f2a5a4d420e4" />



# Introduction

Global long-form video expansion requires robust multilingual subtitle translation, but generic MT fails at fragmented lines, timeline constraints and plot context for mass production. We introduce Mango-MT, an 11-language audiovisual subtitle translator paired with the FT-MT benchmark. Evaluations across all languages show it outperforms GPT, Gemini and DeepSeek with steady timeline compliance, consistent semantics and industrial scalability. Our system holds four key advantages over prior work:

- **Scenario-oriented & Multilingual Optimization**: Professionally optimized for 11 different languages, perfectly adapting to complex industrial rules of video subtitle translation.
- **Context-aware Translation Mechanism**: Leverages global context modeling to stabilize plot logic and consistent character appellation translation.
- **Structural Robustness**: Rigidly retains original subtitle numbers and timestamps to ensure stable batch translation delivery.
- **Industrial-grade evaluation benchmark** : Unlike single-metric schemes, our benchmark integrates a full framework and high-quality dataset, evaluating subtitles against real delivery standards for reliable all-round quality assessment.


## News 🚀🚀🚀


- **2026/06/26** : 🚀 We introduce Mango-MT, an advanced machine learning large language model (MT) that demonstrates superior overall translation performance on film and television translation for 11 languages. Mango-MT achieves SoTA performance on FTT-MT (overall score: **0.91**), and our model matches or outperforms commercial models across most languages on Flores-200.
  
- **2026/06/22** : 🔥 We open-source the benchamark (FTT-MT) constructed on professional film and television translation data.




## Installation
```
conda create -n mlt python==3.11
conda activate mlt
pip install -r requirements.txt
```

## Model Weights
Model checkpoints are accessible from xxx

## Usage

### Transformers Usage
```
python examples/infer/transformers_infer.py
```


### Swift Usage
[ms-swift](https://github.com/modelscope/ms-swift) is a large model and multimodal large model fine-tuning and deployment framework provided by the ModelScope community.
```
pip install ms-swift -U
```
see the [documents](https://swift.readthedocs.io/en/latest/) for more details
```
python examples/infer/swift_infer.py
```

### SGLang Server Usage
[SGLang](https://github.com/sgl-project/sglang) is a fast serving framework for large language models and vision language models. Please use the following command in a fresh environment:
```
uv pip install 'git+https://github.com/sgl-project/sglang.git#subdirectory=python&egg=sglang[all]'
```
See its [documentation](https://docs.sglang.io/docs/get-started/install) for more details.

The following will create API endpoints at http://localhost:8000/v1:

```
python -m sglang.launch_server --model-path model_dir --port 8000 --tp-size 1 --mem-fraction-static 0.8 --context-length 4096 --reasoning-parser qwen3
```

### vLLM Server Usage
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

### API Usage
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
## Evaluation

### Benchmark Introduction

Audiovisual subtitle translation evaluation prioritizes practical delivery standards for global long-form videos rather than mere similarity to human references. Qualified subtitles require simultaneous compliance with semantic accuracy, natural expression, standardized segmentation, valid timestamps and traceable quality, which cannot be fully assessed by single automatic metrics. To mitigate this limitation, we build a dedicated audiovisual translation benchmark with a high-quality dataset and an automated-centric evaluation framework. It supports scalable, reproducible comprehensive model evaluation, with supplementary random manual sampling to assess subtle subtitle characteristics including character tone, cultural adaptability and viewing experience. We have released the [benchmark datasets](https://huggingface.co/datasets/xxx) in hugging face.

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

#### Criteria

To enable horizontal comparison of different models and translation batches on the same evaluation set, we derive a weighted composite score for automated evaluation with the formula as follows:

$$
S = 0.20E + 0.35C + 0.25 \times B + 0.10T + 0.10F
$$

In the formula, $C$ denotes subtitle COMET score, $E$ denotes semantic similarity score,  $S$ refers to the weighted composite score, $T$ stands for timestamp accuracy, $F$ represents format compliance. 


1. **Semantic similarity：**
   Semantic similarity quantifies the semantic proximity between model translations and reference translations based on text embeddings.
   
3. **COMET：**
   As a neural evaluation model, COMET integrates source text, human reference translation and machine translation output to predict the consistency between translations and human quality judgments. It is suitable for measuring whether the semantics of the source text are accurately preserved in translations.
   
5. **BLEU-2：**
   BLEU-2 provides supplementary information on local surface matching and phrase consistency. Thus, its weight is doubled in the weighted composite score.

6. **Timestamp：**
   Timestamp accuracy evaluates whether subtitle segments maintain stable alignment with video timecodes.
   
7. **Format：**
   Format compliance verifies that punctuation, line breaks, layout structure and final subtitle delivery format conform to specifications.

### Steps
#### Step 1
Download [eval data](https://huggingface.co/datasets/xxx) and put in /benchmark/data/test_corpus.xlsx


#### Step 2
Translate with Mango-MT model by using following .py :

```
python benchmark/translate_with_Mango_MT.py --input /benchmark/data/test_corpus.xlsx --output /benchmark/result_1.xlsx
```

#### Step 3
Translate with DeepSeek-v4-pro, Gemini-3-pro, GPT-5.4 by using following .py :


```
python benchmark/translate_with_api.py --input /benchmark/data/result_1.xlsx --output /benchmark/result_2.xlsx
```

#### Step 4
Evaluate the translated data to get the final evaluation score by using:

```
python benchmark/evaluate.py --input /benchmark/result_2.xlsx
```

### Performance 
We evaluate our Mango-MT against three commercial large models (DeepSeek-v4-pro, Gemini-3-pro, GPT-5.4) across 11 languages with five core translation metrics: Semantic similarity, BLEU-2, COMET, Timeline, Format
Following  evaluation protocols, we adopt five core translation metrics  to quantify translation quality. On translation benchmark, Mango-MT outperforms Gemini, DeepSeek and GPT across all 11 evaluated languages. This demonstrates that our model delivers powerful multilingual translation capabilities for film and drama content, and possesses significant commercial deployment value. 

 Evaluation Results on Mango-MT model:

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

Evaluation Results on DeepSeek-v4-pro:

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

Evaluation Results on GPT-5.4:

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

Evaluation Results on Gemini-3-pro:
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



### FLORES
We evaluate the multilingual translation performance of our model on FLORES+ which is based on FLORES-200. This dataset was originally released by FAIR researchers at Meta under the name FLORES.  The data is now being managed by OLDI, [the Open Language Data Initiative](https://oldi.org/). The + has been added to the name to disambiguate between the original datasets and this new actively developed version. For newer versions of this dataset, Please see [FLORES+ HuggingFace repo ](https://huggingface.co/datasets/openlanguagedata/flores_plus).
The data consists of translations primarily from English into over 200 language varieties. The original English sentences were sampled in equal amounts from [Wikinews](https://en.wikinews.org/wiki/Main_Page) (an international news source), [Wikijunior](https://en.wikibooks.org/wiki/Wikijunior) (a collection of age-appropriate non-fiction books), and [Wikivoyage](https://en.wikivoyage.org/wiki/Main_Page) (a travel guide).

Following official FLORES evaluation protocols, we adopt two core universal metrics to quantify translation quality:

- spBLEU: SentencePiece-normalized BLEU with a unified 256k multilingual SentencePiece tokenizer, eliminating tokenization bias to enable equitable score comparison across all 200 languages.

- chrF++: Character-level F-score optimized for agglutinative, non-Latin, and low-orthography languages, complementing spBLEU to capture fine-grained lexical and character-level alignment.


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
#### Step 3
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


1. **BLEU (subtitle-focused metric)：**
   Mango-MT achieves the highest macro average score (26.89), outperforming Gemini (25.59), DeepSeek (24.73) and GPT (22.67). It ranks first on 8 out of 11 languages, especially showing dominant advantages on English and Southeast Asian languages (Indonesian, Thai, Vietnamese, Malay), which fits our subtitle translation scenario perfectly.

2. **chrF++ (fine-grained semantic metric)：**
   Gemini leads the macro average (49.33), followed by Mango-MT (48.23), DeepSeek (47.89) and GPT (47.21). Our model matches or outperforms commercial models across multiple languages.


