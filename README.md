# K2 Vendor Verifier

## What's K2VV

Since the release of the Kimi K2 model, we have received numerous feedback on the precision of Kimi K2 in toolcall. Given that K2 focuses on the agentic loop, the reliability of toolcall is of utmost importance.

We have observed significant differences in the toolcall performance of various open-source solutions and vendors. When selecting a provider, users often prioritize lower latency and cost, but may inadvertently overlook more subtle yet critical differences in model accuracy.

These inconsistencies not only affect user experience but also impact K2's performance in various benchmarking results.
To mitigate these problems, we launch K2 Vendor Verifier to monitor and enhance the quality of all K2 APIs.

We hope K2VV can help ensuring that everyone can access a consistent and high-performing Kimi K2 model.


## Evaluation Results

**Test Time**: 2025-09-22

<table>
<thead>
<tr>
<th rowspan="2">Model Name</th>
<th rowspan="2">Providers</th>
<th colspan="6" style="text-align: center;">Tool calls test</th>
</tr>
<tr>
<th>Count of Finish Reason stop</th>
<th>Count of Finish Reason Tool calls</th>
<th>Count of Finish Reason others</th>
<th>Schema Validation Error Count</th>
<th>Successful Tool Call Count</th>
<th>Similarity compared to the official Implementation</th>
</tr>
</thead>
<tbody>
<tr>
<td rowspan="12">kimi-k2-0905-preview</td>
<td><a href="https://platform.moonshot.cn/">MoonshotAI</a></td>
<td>1437</td>
<td>522</td>
<td>41</td>
<td>0</td>
<td>522</td>
<td>-</td>
</tr>
<tr>
<td><a href="https://platform.moonshot.cn/">Moonshot AI Turbo</a></td>
<td>1441</td>
<td>513</td>
<td>46</td>
<td>0</td>
<td>513</td>
<td>99.29%</td>
</tr>
<tr>
<td><a href="https://openrouter.ai/provider/novita">NovitaAI</a></td>
<td>1483</td>
<td>514</td>
<td>3</td>
<td>10</td>
<td>504</td>
<td>96.82%</td>
</tr>
<tr>
<td><a href="https://openrouter.ai/provider/siliconflow">SiliconFlow</a></td>
<td>1408</td>
<td>553</td>
<td>39</td>
<td>46</td>
<td>507</td>
<td>96.78%</td>
</tr>
<tr>
<td><a href="https://www.volcengine.com/">Volc</a></td>
<td>1423</td>
<td>516</td>
<td>61</td>
<td>40</td>
<td>476</td>
<td>96.70%</td>
</tr>
<tr>
<td><a href="https://openrouter.ai/provider/deepinfra">DeepInfra</a></td>
<td>1455</td>
<td>545</td>
<td>0</td>
<td>42</td>
<td>503</td>
<td>96.59%</td>
</tr>
<tr>
<td><a href="https://openrouter.ai/provider/fireworks">Fireworks</a></td>
<td>1483</td>
<td>511</td>
<td>6</td>
<td>39</td>
<td>472</td>
<td>95.68%</td>
</tr>
<tr>
<td><a href="https://cloud.infini-ai.com/">Infinigence</a></td>
<td>1484</td>
<td>467</td>
<td>49</td>
<td>0</td>
<td>467</td>
<td>95.44%</td>
</tr>
<tr>
<td><a href="https://groq.com/">Groq (99 samples, normalized)</a></td>
<td>67</td>
<td>30</td>
<td>1</td>
<td>0</td>
<td>30</td>
<td>92.65%</td>
</tr>
<tr>
<td><a href="https://openrouter.ai/provider/baseten">Baseten</a></td>
<td>1777</td>
<td>217</td>
<td>6</td>
<td>9</td>
<td>208</td>
<td>72.23%</td>
</tr>
<tr>
<td><a href="https://openrouter.ai/provider/together">Together</a></td>
<td>1866</td>
<td>134</td>
<td>0</td>
<td>8</td>
<td>126</td>
<td>64.89%</td>
</tr>
<tr>
<td><a href="https://openrouter.ai/provider/atlas-cloud">AtlasCloud</a></td>
<td>1906</td>
<td>94</td>
<td>0</td>
<td>4</td>
<td>90</td>
<td>61.55%</td>
</tr>
</tbody>
</table>

The detailed evaluation metrics are as follows:

| Metric Name | Description |
|-------------|-------------|
| Count of Finish Reason: stop | Number of responses where finish_reason is "stop". |
| Count of Finish Reason: tool_calls | Number of responses where finish_reason is "tool_calls". |
| Count of Finish Reason: others | Number of responses where finish_reason is neither "stop" nor "tool_calls". |
| Schema Validation Error Count | Among "tool_calls" responses, the number that failed schema validation. |
| Successful Tool Call Count | Among "tool_calls" responses, the number that passed schema validation. |
| Similarity to Official API | 1-Euclidean distance between a provider's metric values and those of the official Moonshot AI API/estimated_max_distance(2000). |

## How we do the test

We test toolcall's response over a set of 2,000 requests. Each provider's responses are collected and compared against the official Moonshot AI API.
K2 providers are periodically evaluated. If you are not on the list and would like to be included, feel free to contact us.

**Sample Data**: We have provided detailed sample data in samples.jsonl.

## Verify by yourself

To run the evaluation tool with sample data, use the following command:

```bash
python tool_calls_eval.py samples.jsonl \
    --model kimi-k2-0905-preview \
    --base-url https://api.moonshot.cn/v1 \
    --api-key YOUR_API_KEY \
    --concurrency 5 \
    --output results.jsonl \
    --summary summary.json
```

- `samples.jsonl`: Path to the test set file in JSONL format
- `--model`: Model name (e.g., kimi-k2-0905-preview)
- `--base-url`: API endpoint URL
- `--api-key`: API key for authentication (or set OPENAI_API_KEY environment variable)
- `--concurrency`: Maximum number of concurrent requests (default: 5)
- `--output`: Path to save detailed results (default: results.jsonl)
- `--summary`: Path to save aggregated summary (default: summary.json)
- `--timeout`: Per-request timeout in seconds (default: 600)
- `--retries`: Number of retries on failure (default: 3)
- `--extra-body`: Extra JSON body as string to merge into each request payload (e.g., '{"temperature":0.5}')
- `--incremental`: Incremental mode to only rerun failed requests


For testing other providers via OpenRouter:

```bash
python tool_calls_eval.py samples.jsonl \
    --model kimi-k2-0905-preview \
    --base-url https://openrouter.ai/api/v1 \
    --api-key YOUR_OPENROUTER_API_KEY \
    --concurrency 5 \
    --extra-body '{"provider": {"only": ["YOUR_DESIGNATED_PROVIDER"]}}'
```

## Contact Us
If you have any questions or concerns, please reach out to us at shijuanfeng@moonshot.cn.
