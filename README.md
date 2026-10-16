# Gemini Nano Banana 2.1 API (`gemini-nano-banana-2.1`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-hero-perfume.jpg" width="820" alt="Sample render via the APIMart image API">
</p>

> **from $0.0137 per image** on the APIMart route — un único endpoint compatible con OpenAI en `https://api.apimart.ai/v1`, sin plan mensual. *(observado el 2026-10-12)*

**[Obtener una clave de API](https://go.apimart.ai/k-dcbdb7)** · **[Precios en vivo](https://go.apimart.ai/k-97cb3c)** · **[Página del modelo](https://go.apimart.ai/k-aec9bb)** · [⚡ Inicio rápido en 60 segundos](#quickstart)

**Por qué los equipos usan Gemini Nano Banana 2.1 (`gemini-nano-banana-2.1`) a través de APIMart**

- **Una clave, todo el catálogo.** La misma base URL `https://api.apimart.ai/v1` y la misma cabecera `Authorization` dan acceso a Gemini Nano Banana 2.1 (`gemini-nano-banana-2.1`) y a más de 300 modelos de imagen, vídeo y lenguaje: cambia el campo `model`, no tu cliente.
- **Mínimo de $1, pago por uso.** Sin suscripción ni plan prepago que dimensionar por adelantado: recarga desde $1 y úsalo en llamadas.
- **El cargo vuelve en la respuesta.** Cada tarea completada informa del importe facturado (`cost` / `credits_cost`); una tarea marcada como `failed` se reembolsa.
- **Asíncrono por diseño.** Envía, toma el `task_id`, consulta `GET /v1/tasks/{id}`: el batching y los reintentos son trabajo de cola normal, no una integración a medida.

<!-- /conv-kit:v1 -->



Gemini Nano Banana 2.1 is Google's image model on APIMart: text-to-image and reference-image editing on the official or Ext route with 1K / 2K / 4K output.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Per-image | `gemini-nano-banana-2.1` | per delivered image, by resolution | see the [model page](https://go.apimart.ai/k-aec9bb) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/images/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Output | List price | Effective price |
| --- | --- | --- |
| per generated second | — | see the live pricing page |

Token rates (effective): `text_input` $1.2/M, `image_input` $1.2/M, `text_output` $6/M, `image_output` $24/M.
<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-97cb3c) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

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

The table lists the fields accepted by the APIMart `images/generations` endpoint. Check the [model page](https://go.apimart.ai/k-aec9bb) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/images/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"gemini-nano-banana-2.1","prompt":"A bamboo forest at sunrise, slow push-in","size":"1:1","resolution":"1K"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/images/generations", headers=HEADERS, timeout=60, json={
    "model": "gemini-nano-banana-2.1", "prompt": "A bamboo forest at sunrise, slow push-in",
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

**¿Cuál es el id del modelo Gemini Nano Banana 2.1?**

`gemini-nano-banana-2.1`. Consulta la tabla de rutas de arriba.

**¿Cómo se factura?**

**from $0.0137 per image** on the APIMart route. La tarea completada informa del importe exacto en su campo `cost`; una tarea fallida se reembolsa.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-dcbdb7) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [nano-banana-2-1-prompts-es](https://github.com/rx5umw-ict13/nano-banana-2-1-prompts-es)
- **Same model, other languages** — 9 more language builds: 中文 [nano-banana-2-1-api-cn](https://github.com/g8yg5t1oq/nano-banana-2-1-api-cn) · Deutsch [nano-banana-2-1-api-de](https://github.com/tn-gr0cr/nano-banana-2-1-api-de) · Français `nano-banana-2-1-api-fr` · Relay/gateway `nano-banana-2-1-api-gateway` · Bahasa [nano-banana-2-1-api-id](https://github.com/a3g9df6h4jh/nano-banana-2-1-api-id) · 日本語 `nano-banana-2-1-api-ja` · 한국어 `nano-banana-2-1-api-ko` · Português [nano-banana-2-1-api-pt](https://github.com/ulpgjzie6fwa/nano-banana-2-1-api-pt) · Русский [nano-banana-2-1-api-ru](https://github.com/jqhc31it1vme/nano-banana-2-1-api-ru)
- **More models in this language** — [midjourney-api-es](https://github.com/xbuu0/midjourney-api-es) · [gemini-2-5-flash-image-preview-api-es](https://github.com/ekw3w/gemini-2-5-flash-image-preview-api-es) · [nano-banana-api-es](https://github.com/gbm5mfl1/nano-banana-api-es) · [gemini-3-1-flash-image-preview-api-es](https://github.com/x3kcczm8l29xn/gemini-3-1-flash-image-preview-api-es) · [nano-banana-2-api-es](https://github.com/d63hy9t-4pp/nano-banana-2-api-es) · [gemini-3-pro-image-preview-api-es](https://github.com/uv9d8/gemini-3-pro-image-preview-api-es)
- **Topic hubs** — every build of this model, all languages: [nano-banana-2-1](https://github.com/topics/nano-banana-2-1) · [nano-banana-2-1-api](https://github.com/topics/nano-banana-2-1-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-aec9bb> | `apimart.ai/es/model/nano-banana-2-1` |
| Live pricing | <https://go.apimart.ai/k-97cb3c> | `apimart.ai/es/pricing` |
| Get an API key | <https://go.apimart.ai/k-dcbdb7> | `apimart.ai/es/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Gemini Nano Banana 2.1 is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
