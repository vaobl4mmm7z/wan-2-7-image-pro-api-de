# Wan 2.7 Image Pro API (`wan2.7-image-pro`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-hero-perfume.jpg" width="820" alt="Sample render via the APIMart image API">
</p>

> **from $0.0544 per image** on the APIMart route — ein einziger OpenAI-kompatibler Endpoint unter `https://api.apimart.ai/v1`, kein Monatsabo nötig. *(erfasst am 2026-10-12)*

**[API-Key erhalten](https://go.apimart.ai/k-c93474)** · **[Live-Preise](https://go.apimart.ai/k-f81cfe)** · **[Modellseite](https://go.apimart.ai/k-42480b)** · [⚡ Quickstart in 60 Sekunden](#quickstart)

**Warum Teams Wan 2.7 Image Pro (`wan2.7-image-pro`) über APIMart nutzen**

- **Ein Key, der gesamte Katalog.** Dieselbe Base-URL `https://api.apimart.ai/v1` und derselbe `Authorization`-Header erreichen Wan 2.7 Image Pro (`wan2.7-image-pro`) und über 300 weitere Bild-, Video- und Sprachmodelle — Sie wechseln das `model`-Feld, nicht Ihren Client.
- **$1 Minimum, Pay-as-you-go.** Kein Abo und kein Prepaid-Tarif, den Sie vorab dimensionieren müssen: ab $1 aufladen und in Calls verbrauchen.
- **Die Abrechnung kommt in der Antwort zurück.** Jede abgeschlossene Aufgabe meldet den berechneten Betrag (`cost` / `credits_cost`); eine als `failed` markierte Aufgabe wird erstattet.
- **Asynchron by Design.** Absenden, `task_id` entgegennehmen, `GET /v1/tasks/{id}` pollen — Batching und Retries sind gewöhnliche Queue-Arbeit, keine Sonderintegration.

<!-- /conv-kit:v1 -->



Wan 2.7 Image Pro (wan2.7-image-pro) is Wan's flagship text-to-visual model in Alibaba's Wan lineup. Create stunning outputs with a top-tier Wan engine. The Wan API supports fast Wan 2.7 Image Pro generation, and pricing keeps Wan 2.7 Image Pro generation workflows affordable for creators across the Wan community.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Per-image | `wan2.7-image-pro` | per delivered image, by resolution | see the [model page](https://go.apimart.ai/k-42480b) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/images/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Output | List price | Effective price |
| --- | --- | --- |
| default | 0.068 | 0.0544 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-f81cfe) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

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

The table lists the fields accepted by the APIMart `images/generations` endpoint. Check the [model page](https://go.apimart.ai/k-42480b) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/images/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"wan2.7-image-pro","prompt":"A bamboo forest at sunrise, slow push-in","size":"1:1","resolution":"1K"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/images/generations", headers=HEADERS, timeout=60, json={
    "model": "wan2.7-image-pro", "prompt": "A bamboo forest at sunrise, slow push-in",
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

**Wie lautet die Modell-id von Wan 2.7 Image Pro?**

`wan2.7-image-pro`. Siehe Routentabelle oben.

**Wie wird abgerechnet?**

**from $0.0544 per image** on the APIMart route. Die abgeschlossene Aufgabe meldet den genauen Betrag im Feld `cost`; eine fehlgeschlagene Aufgabe wird erstattet.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-c93474) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [wan-2-7-image-pro-prompts-de](https://github.com/vaobl4mmm7z/wan-2-7-image-pro-prompts-de)
- **Same model, other languages** — 10 more language builds: English [wan-2-7-image-pro-api](https://github.com/u3cdzay/wan-2-7-image-pro-api) · 中文 `wan-2-7-image-pro-api-cn` · Español [wan-2-7-image-pro-api-es](https://github.com/pht10ui5daic/wan-2-7-image-pro-api-es) · Français `wan-2-7-image-pro-api-fr` · Relay/gateway `wan-2-7-image-pro-api-gateway` · Bahasa [wan-2-7-image-pro-api-id](https://github.com/g8aowr0uot/wan-2-7-image-pro-api-id) · 日本語 [wan-2-7-image-pro-api-ja](https://github.com/jhz35cfz/wan-2-7-image-pro-api-ja) · 한국어 `wan-2-7-image-pro-api-ko` · Português [wan-2-7-image-pro-api-pt](https://github.com/gvq5ox1pycj/wan-2-7-image-pro-api-pt) · Русский [wan-2-7-image-pro-api-ru](https://github.com/jsgotg/wan-2-7-image-pro-api-ru)
- **More models in this language** — [midjourney-api-de](https://github.com/h5fd0cofy/midjourney-api-de) · [gemini-2-5-flash-image-preview-api-de](https://github.com/kn6f69s2b8j9/gemini-2-5-flash-image-preview-api-de) · [nano-banana-api-de](https://github.com/uh6cob8op/nano-banana-api-de) · [gemini-3-1-flash-image-preview-api-de](https://github.com/k8wq8pd46e5ku/gemini-3-1-flash-image-preview-api-de) · [nano-banana-2-api-de](https://github.com/mocpl3/nano-banana-2-api-de) · [gemini-3-pro-image-preview-api-de](https://github.com/cmpxx2w/gemini-3-pro-image-preview-api-de)
- **Topic hubs** — every build of this model, all languages: [wan-2-7-image-pro](https://github.com/topics/wan-2-7-image-pro) · [wan-2-7-image-pro-api](https://github.com/topics/wan-2-7-image-pro-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-42480b> | `apimart.ai/de/model/wan-2-7-image-pro` |
| Live pricing | <https://go.apimart.ai/k-f81cfe> | `apimart.ai/de/pricing` |
| Get an API key | <https://go.apimart.ai/k-c93474> | `apimart.ai/de/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Wan 2.7 Image Pro is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
