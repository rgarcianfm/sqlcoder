# Defog SQLCoder
Defog's SQLCoder is a family of state-of-the-art LLMs for converting natural language questions to SQL queries.

[Interactive Demo](https://defog.ai/sqlcoder-demo/) | [🤗 HF Repo](https://huggingface.co/defog/sqlcoder-34b-alpha) | [♾️ Colab](https://colab.research.google.com/drive/1z4rmOEiFkxkMiecAWeTUlPl0OmKgfEu7?usp=sharing) | [🐦 Twitter](https://twitter.com/defogdata)

## TL;DR
SQLCoder is a family of large language models that outperforms `gpt-4` and `gpt-4-turbo` for natural language to SQL generation tasks on our [sql-eval](https://github.com/defog-ai/sql-eval) framework, and significantly outperform all popular open-source models.

SQLCoder-70B and SQLCoder-34B are fine-tuned on a base CodeLlama model.

## Results on novel datasets not seen in training (`num_beams`=4)
| model   | perc_correct |
|-|-|
| defog-sqlcoder-70b-alpha    | 93.0 |
| defog-sqlcoder-7b-2    | 87.0 |
| defog-sqlcoder-34b-alpha    | 84.0 |
| gpt4-2024-01-30       | 82.0 |
| gpt4-turbo-2024-01-30 | 78.0 |
| defog-sqlcoder2       | 77.5 |
| defog-sqlcoder-7b     | 71.0 |
| gpt-3.5-2024-01-30    | 65.0 |
| claude-2              | 64.5 |
| gpt-3.5-2023-08-28    | 61.0 |
| claude_instant_1      | 61.0 |
| text-davinci-003      | 52.5 |

![Percentage of correctly generated SQL queries on novel schemas not seen in training (n = 200) in SQL-Eval](https://github.com/defog-ai/sqlcoder/assets/5008293/839aa98f-101d-4baa-8e80-bb4fd5110012)]

## License
The code in this repo (what little there is of it) is Apache-2 licensed. The model weights have a `CC BY-SA 4.0` license. The TL;DR is that you can use and modify the model for any purpose – including commercial use. However, if you modify the weights (for example, by fine-tuning), you must open-source your modified weights under the same license terms.

## Training
Defog was trained on more than 20,000 human-curated questions. These questions were based on 10 different schemas. None of the schemas in the training data were included in our evaluation framework. 

You can read more about our [training approach](https://defog.ai/blog/open-sourcing-sqlcoder2-7b/) and [evaluation framework](https://defog.ai/blog/open-sourcing-sqleval/).

## Results by question category
We classified each generated question into one of 6 categories. The table displays the percentage of questions answered correctly by each model, broken down by category.
|               | date | group_by | order_by | ratio | join | where |
| ------------- | ---- | -------- | -------- | ----- | ---- | ----- |
| sqlcoder-70b  | 96   | 91.4     | 97.1     | 85.7  | 97.1 | 91.4  |
| sqlcoder-34b  | 80   | 94.3     | 85.7     | 77.1  | 85.7 | 80    |
| gpt-4         | 64   | 94.3     | 88.6     | 74.2  | 85.7 | 80    |
| sqlcoder2-15b | 76   | 80       | 77.1     | 60    | 77.1 | 77.1  |
| sqlcoder-7b   | 64   | 82.9     | 74.3     | 54.3  | 74.3 | 74.3  |
| gpt-3.5       | 68   | 77.1     | 74.2     | 34.3  | 65.7 | 71.4  |
| claude-2      | 52   | 71.4     | 74.3     | 57.1  | 65.7 | 62.9  |

## Using SQLCoder
You can use SQLCoder via the `transformers` library by downloading our model weights from the Hugging Face repo. We have added sample code for [inference](./inference.py) on a [sample database schema](./metadata.sql). 
```bash
python inference.py -q "Question about the sample database goes here"

# Sample question:
# Do we get more revenue from customers in New York compared to customers in San Francisco? Give me the total revenue for each city, and the difference between the two.
```

You can also use a demo on our website [here](https://defog.ai/sqlcoder-demo)

## Hardware Requirements
SQLCoder-34B has been tested on a 4xA10 GPU with `float16` weights. You can also load an 8-bit and 4-bit quantized version of the model on consumer GPUs with 20GB or more of memory – like RTX 4090, RTX 3090, and Apple M2 Pro, M2 Max, or M2 Ultra Chips with 20GB or more of memory.

## Todo

- [x] Open-source the v1 model weights
- [x] Train the model on more data, with higher data variance
- [ ] Tune the model further with Reward Modelling and RLHF
- [ ] Pretrain a model from scratch that specializes in SQL analysis
