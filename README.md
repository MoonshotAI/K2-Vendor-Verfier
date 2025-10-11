# K2 Vendor Verifier

## What's K2VV

Since the release of the Kimi K2 model, we have received numerous feedback on the precision of Kimi K2 in toolcall. Given that K2 focuses on the agentic loop, the reliability of toolcall is of utmost importance.

We have observed significant differences in the toolcall performance of various open-source solutions and vendors. When selecting a provider, users often prioritize lower latency and cost, but may inadvertently overlook more subtle yet critical differences in model accuracy.

These inconsistencies not only affect user experience but also impact K2's performance in various benchmarking results.
To mitigate these problems, we launch K2 Vendor Verifier to monitor and enhance the quality of all K2 APIs.

We hope K2VV can help ensuring that everyone can access a consistent and high-performing Kimi K2 model.


## Evaluation Results

**Test Time**: 2025-10-10

<table>
  <thead>
    <tr>
      <th rowspan="2">Model Name</th>
      <th rowspan="2">Providers</th>
      <th colspan="6">Tool calls test</th>
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
      <td rowspan="15">kimi-k2-0905-preview</td>
      <td><a href="https://platform.moonshot.ai/">MoonshotAI</a></td>
      <td>2679</td>
      <td>1286</td>
      <td>35</td>
      <td>0</td>
      <td>1286</td>
      <td>-</td>
    </tr>
    <tr>
      <td><a href="https://platform.moonshot.ai/">Moonshot AI Turbo</a></td>
      <td>2659</td>
      <td>1301</td>
      <td>40</td>
      <td>0</td>
      <td>1301</td>
      <td>99.26%</td>
    </tr>
    <tr>
      <td><a href="https://openrouter.ai/provider/novita">NovitaAI</a></td>
      <td>2717</td>
      <td>1279</td>
      <td>4</td>
      <td>195</td>
      <td>1084</td>
      <td>92.87%</td>
    </tr>
    <tr>
      <td><a href="https://openrouter.ai/provider/groq">Groq</a></td>
      <td>2739</td>
      <td>989</td>
      <td>26</td>
      <td>0</td>
      <td>989</td>
      <td>89.39%</td>
    </tr>
    <tr>
      <td><a href="https://openrouter.ai/provider/fireworks">Fireworks</a></td>
      <td>2546</td>
      <td>1443</td>
      <td>8</td>
      <td>347</td>
      <td>1096</td>
      <td>88.83%</td>
    </tr>
    <tr>
      <td><a href="https://openrouter.ai/provider/baseten">Baseten</a></td>
      <td>2598</td>
      <td>1396</td>
      <td>6</td>
      <td>358</td>
      <td>1038</td>
      <td>88.57%</td>
    </tr>
    <tr>
      <td><a href="https://openrouter.ai/provider/deepinfra">DeepInfra</a></td>
      <td>2552</td>
      <td>1446</td>
      <td>2</td>
      <td>374</td>
      <td>1072</td>
      <td>88.05%</td>
    </tr>
    <tr>
      <td><a href="https://www.volcengine.com/">Volc</a></td>
      <td>2572</td>
      <td>1368</td>
      <td>60</td>
      <td>376</td>
      <td>992</td>
      <td>87.59%</td>
    </tr>
    <tr>
      <td><a href="https://openrouter.ai/provider/together">Together</a></td>
      <td>2725</td>
      <td>1271</td>
      <td>1</td>
      <td>369</td>
      <td>902</td>
      <td>86.60%</td>
    </tr>
    <tr>
      <td><a href="https://openrouter.ai/provider/siliconflow">SiliconFlow</a></td>
      <td>2626</td>
      <td>985</td>
      <td>389</td>
      <td>1</td>
      <td>984</td>
      <td>86.08%</td>
    </tr>
    <tr>
      <td><a href="https://cloud.infini-ai.com/">Infinigence</a></td>
      <td>2729</td>
      <td>860</td>
      <td>411</td>
      <td>2</td>
      <td>858</td>
      <td>82.17%</td>
    </tr>
    <tr>
      <td><a href="https://nebius.ai/">Nebius</a></td>
      <td>3327</td>
      <td>633</td>
      <td>37</td>
      <td>82</td>
      <td>551</td>
      <td>70.49%</td>
    </tr>
    <tr>
      <td><a href="https://openrouter.ai/provider/chutes">Chutes</a></td>
      <td>3866</td>
      <td>131</td>
      <td>0</td>
      <td>27</td>
      <td>104</td>
      <td>49.12%</td>
    </tr>
    <tr>
      <td><a href="https://openrouter.ai/provider/atlas-cloud">AtlasCloud</a></td>
      <td>3867</td>
      <td>128</td>
      <td>2</td>
      <td>33</td>
      <td>95</td>
      <td>48.93%</td>
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
| Similarity to Official API | 1-Euclidean distance between a provider's metric values and those of the official Moonshot AI API/estimated_max_distance(4000). |

## How we do the test

We test toolcall's response over a set of 4,000 requests. Each provider's responses are collected and compared against the official Moonshot AI API.
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
- `--extra-body`: Extra JSON body as string to merge into each request payload (e.g., '{"temperature":0.6}')
- `--incremental`: Incremental mode to only rerun failed requests


For testing other providers via OpenRouter:

```bash
python tool_calls_eval.py samples.jsonl \
    --model moonshotai/kimi-k2-0905 \
    --base-url https://openrouter.ai/api/v1 \
    --api-key YOUR_OPENROUTER_API_KEY \
    --concurrency 5 \
    --extra-body '{"provider": {"only": ["YOUR_DESIGNATED_PROVIDER"]}}'
```

## Contact Us
**We're preparing the next benchmark round and need your input.**

If there's any **metric or test case** you care about, please drop a note in [issue](https://github.com/MoonshotAI/K2-Vendor-Verifier/issues/9)

And welcome to drop the name of any vendor you’d like to see in in [issue](https://github.com/MoonshotAI/K2-Vendor-Verifier/issues/10)

---
If you have any questions or concerns, please reach out to us at shijuanfeng@moonshot.cn.
