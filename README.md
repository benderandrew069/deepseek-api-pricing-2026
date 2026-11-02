# DeepSeek API Pricing: Compare Providers and Estimate Your Costs

DeepSeek API pricing starts at $0.15 per million uncached input tokens and $0.60 per million output tokens for the official Flash API off-peak. Other providers set their own rates, so the cost of using DeepSeek also depends on where you access it and which configuration you choose.

Start with the question you need to answer:

- **Checking the official price?** The table below separates input, cached input and output, including peak rates.
- **Choosing a provider?** Compare DeepSeek direct, Morph, UnificAlly and Moduck using their published model prices.
- **Planning a monthly budget?** Use the formula and two worked examples to price your own token mix.

**Prices checked November 2, 2026.** This guide is prepared by Moduck, an AI API aggregator for developers and product teams.

These are **DeepSeek’s direct API prices in USD per 1 million tokens**, from [DeepSeek’s official rate card](https://api-docs.deepseek.com/quick_start/pricing/):

| Official API model | Input, cache miss: off-peak / peak | Input, cache hit: off-peak / peak | Output: off-peak / peak |
| --- | --- | --- | --- |
| `deepseek-flash` — V4.1-Flash-1017 | $0.15 / $0.30 | $0.003 / $0.006 | $0.60 / $1.20 |
| `deepseek-v4-pro` — V4-Pro-0813 | $0.66 / $1.32 | $0.022 / $0.044 | $1.98 / $3.96 |

Peak pricing applies **01:00–04:00 and 06:00–10:00 UTC, Monday through Friday, excluding Chinese public holidays**. Every other hour is off-peak, including weekends and those holidays.

The current model behind `deepseek-flash` is DeepSeek-V4.1-Flash-1017. DeepSeek says the older `deepseek-v4-flash` and `deepseek-v4-flash-vision-exp` names are still accepted, but now reach that model at Flash prices. Use the current served version when comparing a rate card with an older article.
![Conceptual quote cards for model, cache and timing leading to a budget calculation.](assets/deepseek-api-pricing-cover-20261102.png)

*Conceptual illustration: the conditions behind a token quote belong in the budget.*

## What DeepSeek API providers charge

A provider’s input price answers only part of the buying question. Cached input and generated output have their own rates, and a time-based surcharge can change which option makes sense for your application.

Here are the providers’ published Flash prices, checked November 2, 2026. **All prices are USD per 1M tokens; input means cache-miss input.**

| Provider | Model / configuration | Rate period | Input | Cached input | Output |
| --- | --- | --- | ---: | ---: | ---: |
| [DeepSeek](https://api-docs.deepseek.com/quick_start/pricing/) | Flash 1017 | Off-peak | $0.15 | $0.003 | $0.60 |
| [DeepSeek](https://api-docs.deepseek.com/quick_start/pricing/) | Flash 1017 | Peak | $0.30 | $0.006 | $1.20 |
| [Morph](https://www.morphllm.com/pricing) | Flash 1017 | All hours | $0.12 | $0.008 | $0.80 |
| [UnificAlly](https://unifically.com/zh/blogs/deepseek-api) | Flash | Off-peak | $0.1125 | $0.00225 | $0.45 |
| [UnificAlly](https://unifically.com/zh/blogs/deepseek-api) | Flash | Peak | $0.225 | $0.0045 | $0.90 |
| [Moduck](https://moduck.ai/models/c/deepseek/deepseek-v4.1-flash) | Flash · FP4 | All hours | $0.03776 | $0.00128 | $0.096 |

Model IDs: DeepSeek `deepseek-flash`; Morph `morph-dsv41flash-1017`; UnificAlly `deepseek/deepseek-flash`; Moduck `deepseek/deepseek-v4.1-flash`.

DeepSeek’s peak schedule is defined above. UnificAlly publishes peak/off-peak prices; Moduck’s model page gives the FP4 rates above without a separate time-band table.

Morph shows why a single headline rate can mislead. Its uncached input costs less than DeepSeek direct off-peak, while its cached input and output cost more. An application that reads large amounts of new text has a different comparison from one that generates long answers.

Moduck’s Flash prices are for **FP4**, a model precision/quantization configuration. Keep that specification beside the price when you build a shortlist. Compare the cost of the configuration you plan to use, then judge its answers against your application’s requirements. The budget table compares token costs; it does not claim every host and configuration produces identical results.

## Turn token rates into your own monthly estimate

Start with monthly billable token totals, not the number of people using your app. Ten short extraction calls can cost less than one long agent run, even when both end in a one-paragraph answer.

For each provider and each applicable billing period:

**Token cost = (cache-miss input ÷ 1,000,000 × miss rate) + (cache-hit input ÷ 1,000,000 × hit rate) + (billable output ÷ 1,000,000 × output rate).**

Add the periods together, then add separately billed tools and other applicable fees. If you start from requests, monthly input is requests × average input tokens, and monthly output is requests × average billable output tokens. Split input into hit and miss totals using measured usage. For thinking workloads, use the service’s billable output total, not just the visible final answer.

For DeepSeek direct, the response fields `prompt_cache_hit_tokens` and `prompt_cache_miss_tokens` distinguish the two input categories. [DeepSeek’s caching guide](https://api-docs.deepseek.com/guides/kv_cache) describes matching persisted prompt prefixes and best-effort cache behavior. Reusing a document is not a promise that every token will hit the cache. A cache discount affects qualifying input; it does not discount newly generated output. In the assistant example below, losing all assumed hits would raise direct off-peak cost from $24.36 to $42.00 at the same total input and output volumes.

If your logs do not yet separate peak usage, calculate an all-off-peak and an all-peak budget as bounds for the same token volumes. For a more specific estimate, split actual tokens into the correct time bins. The official price page does not settle every long-request boundary case; confirm how a request crossing a boundary is assigned before relying on a last-minute scheduling trick.

There is another line item for research agents. DeepSeek lists Search and Fetch at **$0.75 per 1,000 successful billable requests each**. Thus 10,000 billable searches plus 20,000 billable fetches add $7.50 + $15.00 = **$22.50**, separately from model tokens. The [Search & Fetch billing rules](https://api-docs.deepseek.com/guides/search) exclude failed requests and define a billable success. The page still says certain server-side calls are “currently free” while also stating billing starts November 1, 2026, Beijing time—a date already past at this check. Budget using the stated charging schedule and verify account treatment rather than assuming those calls remain free.

These estimates exclude taxes, payment or currency-conversion charges, any separate gateway fees, and extra retry/fallback usage. Hosted API token fees also remain separate from any chat-app offer or promotional credit. Do not assume a free balance applies to your account.
![Cached and fresh input enter a model; generated output remains a separate billed category.](assets/deepseek-cache-output-20261102.png)

*Conceptual billing view. Cache pricing applies to qualifying input; generated output uses its own rate.*

## The cheaper route changes with the workload

Consider two applications over a 30-day month. These are planning examples using published prices, not customer bills or performance benchmarks. Each provider is assigned the **same token volumes and cache share** to isolate the rate differences. Actual cache hits depend on the workload and host.

| Assumption | Input-heavy assistant | Output-heavy generator |
| --- | --- | --- |
| Requests per month | 100,000 | 50,000 |
| Average input tokens per request | 2,000 | 1,000 |
| Average billable output tokens per request | 200 | 2,000 |
| Total input / output | 200M / 20M | 50M / 100M |
| Cache-hit share of input | 60% | 10% |
| Share of billable tokens assigned to peak rates | 25% of each input/output category | 25% of each input/output category |

The assistant has 120M cached input, 80M uncached input and 20M output tokens. On DeepSeek direct, all-off-peak token cost is:

**120 × $0.003 + 80 × $0.15 + 20 × $0.60 = $24.36.**

With 25% of each category charged at double rates, the mixed-period estimate is **$24.36 × (0.75 + 0.25 × 2) = $30.45**. At 100% peak, the same tokens cost **$48.72**. Nothing about the model, token volume or cache ratio changed in that timing comparison.

Here are the monthly token costs using each provider’s published rates, rounded to the nearest cent after calculating the full total:

| Provider-rate calculation | Input-heavy assistant | Output-heavy generator |
| --- | ---: | ---: |
| DeepSeek direct, all off-peak | $24.36 | $66.77 |
| DeepSeek direct, 25% peak | $30.45 | $83.46 |
| DeepSeek direct, all peak | $48.72 | $133.53 |
| Morph, listed all-hours rates | $26.56 | $85.44 |
| UnificAlly, published rates with 25% peak | $22.84 | $62.59 |
| Moduck FP4, published model rates | $5.09 | $11.31 |

For the assistant, Morph’s calculation is **80 × $0.12 + 120 × $0.008 + 20 × $0.80 = $26.56**. It falls below the mixed-period direct estimate, but above direct all-off-peak. A nightly assistant job can wait for the lower rate; a customer waiting for an answer usually cannot. Those two applications should not inherit the same buying decision.

Now look at the generator. Direct all-off-peak cost is **45 × $0.15 + 5 × $0.003 + 100 × $0.60 = $66.765**. At 25% peak that becomes **$83.45625**, while Morph totals **45 × $0.12 + 5 × $0.008 + 100 × $0.80 = $85.44**. The output-heavy mix has reversed that particular mixed-period comparison. For UnificAlly, the generator calculation is **(45 × $0.1125 + 5 × $0.00225 + 100 × $0.45) × 1.25 = $62.5921875**.

The Moduck row uses its published FP4 rates. The model page does not list separate peak and off-peak prices, so no time adjustment has been added. The next section shows both Moduck calculations and what they mean for these applications.

## DeepSeek pricing on Moduck

Moduck is an AI API aggregator for developers and product teams. Its model page brings the DeepSeek configuration, token prices, specifications and quick-start information together, giving you a practical path from comparing costs to planning an integration.

For `deepseek/deepseek-v4.1-flash` in FP4, **Moduck’s prices are $0.03776 per 1M input tokens, $0.00128 per 1M cached input tokens and $0.096 per 1M output tokens**. The model information also lists a 1M-token context window and a maximum output of 384K tokens. The context and output limits are capacity ceilings. Costs depend on the tokens actually processed.

Apply these prices to the input-heavy assistant: 80M uncached input tokens, 120M cached input tokens and 20M output tokens per month.

**80 × $0.03776 + 120 × $0.00128 + 20 × $0.096 = $5.0944, or about $5.09 per month.**

For the output-heavy generator, the assumptions are 45M uncached input tokens, 5M cached input tokens and 100M output tokens:

**45 × $0.03776 + 5 × $0.00128 + 100 × $0.096 = $11.3056, or about $11.31 per month.**

The assistant spends about $3.02 on fresh input, $0.15 on cached input and $1.92 on output. That makes repeated context useful, but it also shows why you should count the uncached portion carefully. If the assistant receives mostly new documents, replace the 60% cache assumption with the hit share your workload achieves.

For the generator, output accounts for $9.60 of the $11.31 estimate. Reducing answer length or unnecessary generation attempts can therefore matter more than increasing its relatively small cached-input share. A support assistant and a report generator should have separate budgets even when they use the same model.

You can use Moduck’s model information and pricing to assess the configuration, calculate a workload budget, and then use the model page’s OpenAI-style quick-start example and [Moduck’s Flash specification](https://docs.moduck.ai/en/models/library/text-models-llm/deepseek-ai/deepseek-flash) to prepare your integration. Check the model identifier and supported request fields as part of setup, rather than copying an unrelated model’s example.

[Moduck’s public pricing page](https://moduck.ai/pilot/pricing) sets a $10 minimum top-up and no fixed monthly fee. Keep the amount you fund separate from expected token consumption: the assistant’s $5.09 estimate is a usage budget, while the minimum top-up determines the initial funding amount. The examples use standard model rates without applying promotional credit.

## Choose a route that fits your workload

DeepSeek direct is a straightforward baseline when you want its current hosted model and can work with its cache and time-based pricing. Schedulable jobs deserve an off-peak calculation; interactive workloads should use the time distribution their users actually create.

Compare other providers using the part of the bill that matters to your application. Morph’s input price may suit some reading-heavy workloads, but its output rate changes the calculation for long answers. UnificAlly has its own peak/off-peak rate card. Moduck’s FP4 rates give the two example workloads token budgets of about $5.09 and $11.31, alongside model information and integration documentation in the same product.

**Consider Moduck when you want a unified API platform and its DeepSeek FP4 configuration fits the work you are building.** Start with your expected input, output and cache usage, then evaluate whether the model’s answers meet your application’s acceptance criteria. A useful budget should describe the work you need done.

To start your DeepSeek integration with Moduck, get an API key and follow the documentation for your selected model.

[Get API Key](https://moduck.ai/dashboard/api-keys) — Sign-in required. Setup details are in the [Moduck documentation](https://docs.moduck.ai/).
