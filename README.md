# Grok Imagine 2.0 API (`grok-imagine-2.0-ext`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-hero-perfume.jpg" width="820" alt="Sample render via the APIMart image API">
</p>

> **from $0.015 per image** (region-edit $0.015) — one OpenAI-compatible endpoint at `https://api.apimart.ai/v1`, no monthly plan required. *(observed 2026-10-12)*

**[Get an API key](https://go.apimart.ai/k-2b63be)** · **[Live pricing](https://go.apimart.ai/k-e15913)** · **[Model page](https://go.apimart.ai/k-fb0aa5)** · [⚡ 60-second quickstart](#quickstart)

**Why teams call Grok Imagine 2.0 (`grok-imagine-2.0-ext`) through APIMart**

- **One key, entire catalog.** The same `https://api.apimart.ai/v1` base URL and `Authorization` header reach Grok Imagine 2.0 (`grok-imagine-2.0-ext`) and 300+ other image, video and language models — switch the `model` field, not your client.
- **$1 minimum, pay as you go.** No subscription and no prepaid plan to size up front: top up from $1 and spend it on calls. There is no free quota to burn through first, so the price in this table is the price you pay.
- **The charge comes back in the response.** Every call reports the amount billed (`cost` / `credits_cost`), so a spend number is read per call instead of guessed at month end.
- **Async by design.** Submit, take the `task_id`, poll `GET /v1/tasks/{id}` — batching and retries are ordinary queue work, not a bespoke integration.

<!-- /conv-kit:v1 -->



Grok Imagine 2.0 is xAI's image generation and region-edit model, served per image through the APIMart endpoint.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Per-image | `grok-imagine-2.0-ext` | per delivered image, by resolution | see the [model page](https://go.apimart.ai/k-fb0aa5) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/images/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Output | List price | Effective price |
| --- | --- | --- |
| default | 0.06 | 0.015 |
| region-edit | 0.01875 | 0.015 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-e15913) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | what to generate, or what to change when editing |
| `size` | string | `auto` / `1:1` | aspect ratio, or `auto` where the route supports it |
| `resolution` | string | `1K` | output resolution tier — cost scales with it |
| `n` | integer | `1` | number of images per task, where the route allows more than one |
| `image_urls` | string[] | — | reference images as public HTTP(S) URLs or base64 data URLs |
| `nsfw_check` | boolean | `false` | runs `omni-moderation-latest` before submitting |

The table lists the fields accepted by the APIMart `images/generations` endpoint. Check the [model page](https://go.apimart.ai/k-fb0aa5) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/images/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"grok-imagine-2.0-ext","prompt":"A bamboo forest at sunrise, slow push-in","size":"1:1","resolution":"1K"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/images/generations", headers=HEADERS, timeout=60, json={
    "model": "grok-imagine-2.0-ext", "prompt": "A bamboo forest at sunrise, slow push-in",
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["images"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**What is the Grok Imagine 2.0 model id?**

`grok-imagine-2.0-ext`. See the route table above.

**How is it billed?**

**from $0.015 per image** (region-edit $0.015). The completed task reports the exact amount in its `cost` field; a failed task is refunded.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-2b63be) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [grok-imagine-2-0-prompts](https://github.com/tv8s57ojjaw/grok-imagine-2-0-prompts)
- **Same model, other languages** — 10 more language builds: 中文 [grok-imagine-2-0-api-cn](https://github.com/q7zvwq45nz481l/grok-imagine-2-0-api-cn) · Deutsch [grok-imagine-2-0-api-de](https://github.com/ridq8qs/grok-imagine-2-0-api-de) · Español [grok-imagine-2-0-api-es](https://github.com/rkrzj6t/grok-imagine-2-0-api-es) · Français [grok-imagine-2-0-api-fr](https://github.com/xr7eak0ma/grok-imagine-2-0-api-fr) · Relay/gateway `grok-imagine-2-0-api-gateway` · Bahasa [grok-imagine-2-0-api-id](https://github.com/pqwyoiqhp/grok-imagine-2-0-api-id) · 日本語 [grok-imagine-2-0-api-ja](https://github.com/yx0gie/grok-imagine-2-0-api-ja) · 한국어 [grok-imagine-2-0-api-ko](https://github.com/ignd9ttac/grok-imagine-2-0-api-ko) · Português [grok-imagine-2-0-api-pt](https://github.com/et3yo6leiak/grok-imagine-2-0-api-pt) · Русский [grok-imagine-2-0-api-ru](https://github.com/c3lmgfyau8zz/grok-imagine-2-0-api-ru)
- **More models in this language** — [grok-imagine-1-5-api](https://github.com/igu3s5e5x90wx/grok-imagine-1-5-api) · [grok-image-api](https://github.com/hpcsd0apt/grok-image-api) · [grok-imagine-image-quality-api](https://github.com/xt5chw51l/grok-imagine-image-quality-api) · [imagen-4-api](https://github.com/lrk0lbrti/imagen-4-api) · [qwen-image-2-0-pro-api](https://github.com/usen4/qwen-image-2-0-pro-api) · [seedream-4-0-api](https://github.com/qela34r8sb7/seedream-4-0-api)
- **Topic hubs** — every build of this model, all languages: [grok-imagine-2-0](https://github.com/topics/grok-imagine-2-0) · [grok-imagine-2-0-api](https://github.com/topics/grok-imagine-2-0-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->
## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-fb0aa5> | `apimart.ai/model/grok-imagine-2-0` |
| Live pricing | <https://go.apimart.ai/k-e15913> | `apimart.ai/pricing` |
| Get an API key | <https://go.apimart.ai/k-2b63be> | `apimart.ai/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Grok Imagine 2.0 is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
