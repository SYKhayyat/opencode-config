# OpenCode — free-tier setup

OpenCode is installed globally (`npm i -g opencode-ai`, v1.18.18).
This folder holds its config. The `OPENCODE_CONFIG` environment variable points here,
so this file is used no matter which directory you run `opencode` from.

## Step 1 — get the API keys

Sign up for these and paste each key into `opencode.json`, replacing the
matching `PASTE_..._HERE` placeholder. All are free and none require a credit card.

| Provider | Where to get the key | Free allowance |
|---|---|---|
| Z.ai (GLM) | https://z.ai/manage-apikey/apikey-list | GLM-4.7-Flash and GLM-4.5-Flash, free with no expiry |
| NVIDIA Build | https://build.nvidia.com | ~5,000 inference credits, 40 requests/min, 100+ models |
| Mistral | https://console.mistral.ai/api-keys | 1 billion tokens/month, 1 req/sec, 500k tokens/min |
| Google AI Studio | https://aistudio.google.com/apikey | 1,500 Flash requests/day (Pro is limited and may be paid-only) |
| ModelScope | https://modelscope.cn/my/myaccesstoken | 2,000 calls/day, 500 per model/day |

You do not need all five. One key is enough to start — Z.ai is the one to get first.
Providers whose keys are still placeholders will simply fail if you select them;
you can delete those blocks from `opencode.json` if you prefer a shorter list.

## Step 2 — run it

    cd C:\Users\Administrator\Videos\opencode
    opencode

Inside OpenCode, `/models` opens the model picker to switch providers mid-session.

## Which model to use

All strings below were verified against `opencode models` on this machine.

| Task | Model | Why |
|---|---|---|
| Default / everyday | `zai/glm-4.7-flash` | Free with no expiry, 200k context, tuned for coding and agents |
| Hard problems (Rust) | `nvidia/qwen/qwen3-coder-480b-a35b-instruct` | Much larger model, 262k context |
| Frontier-class | `nvidia/z-ai/glm-5.2` | 1M token context |
| Very long context | `nvidia/minimaxai/minimax-m3` | 1M token context |
| DeepSeek | `nvidia/deepseek-ai/deepseek-v4-pro` | Strong reasoning, no separate signup needed |
| High volume | `mistral/codestral-latest` | Mistral's coding model, covered by the 1B tokens/month free tier |
| Second opinion | `google/gemini-3.7-flash` | Different model family, different failure modes |
| Qwen direct | `modelscope/Qwen/Qwen3-Coder-30B-A3B-Instruct` | 262k context, free |

## Notes

- There is no automatic failover between providers. When one hits its quota,
  switch models manually with `/models`.
- Pricing shown in OpenCode's model picker comes from the models.dev catalog and
  reflects each provider's list price. It does not know about your free tier —
  Mistral models show a per-token price, but the free "Experiment" tier covers
  them up to 1B tokens/month.
- NVIDIA's models are free to call but draw down a shared credit pool. Once the
  credits are gone the flagship models stop; smaller ones keep a daily quota.
- OpenCode bundles a pinned copy of the models.dev catalog, so a few models listed
  on models.dev (Devstral, Kimi) are not selectable here. Run `opencode models`
  to see the authoritative list for your installed version.
- `opencode.json` holds API keys in plain text. It is committed here with
  placeholder values only. If you paste real keys into it, untrack it first:
  `git rm --cached opencode.json && echo opencode.json >> .gitignore`.
- Free tiers in this space change often. Re-check the table above every few months:
  https://github.com/amardeeplakshkar/awesome-free-llm-apis
