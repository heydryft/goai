---
title: Runcrate Provider
description: "Run open models such as DeepSeek, GLM, Qwen, gpt-oss and Kimi on Runcrate in Go with GoAI, through its OpenAI-compatible API with chat, streaming and tool calling."
---

# Runcrate

[Runcrate](https://www.runcrate.ai/) inference provider using the OpenAI-compatible Chat Completions API. Runcrate serves open models such as DeepSeek, GLM, Qwen, gpt-oss and Kimi.

## Setup

```bash
go get github.com/zendev-sh/goai@latest
```

```go
import "github.com/zendev-sh/goai/provider/runcrate"
```

Set the `RUNCRATE_API_KEY` environment variable, or pass `WithAPIKey()` directly.

## Models

- `deepseek-ai/DeepSeek-V4.1-Flash`
- `deepseek-ai/DeepSeek-V4-Pro`
- `zai-org/GLM-5.3`
- `zai-org/GLM-5.3-Flash`
- `Qwen/Qwen3-Coder-480B-A35B-Instruct-Turbo`
- `openai/gpt-oss-120b`
- `moonshotai/Kimi-K2.6`

Model IDs use the `org/model` prefix format and are case-sensitive. `GET https://api.runcrate.ai/v1/models` lists the full catalog and needs no API key.

## Tested Models

**Unit tested** (mock HTTP server, 2026-10-27): `deepseek-ai/DeepSeek-V4.1-Flash`, `zai-org/GLM-5.3`

**Live tested** (real API, 2026-10-24): `deepseek-ai/DeepSeek-V4.1-Flash` (`GenerateText`), `zai-org/GLM-5.3-Flash` (`StreamText`)

## Usage

```go
model := runcrate.Chat("deepseek-ai/DeepSeek-V4.1-Flash")

result, err := goai.GenerateText(ctx, model, goai.WithPrompt("Hello"))
if err != nil {
    log.Fatal(err)
}
fmt.Println(result.Text)
```

## Options

| Option | Type | Description |
|--------|------|-------------|
| `WithAPIKey(key)` | `string` | Set a static API key |
| `WithTokenSource(ts)` | `provider.TokenSource` | Set a dynamic token source |
| `WithBaseURL(url)` | `string` | Override the default `https://api.runcrate.ai/v1` endpoint |
| `WithHeaders(h)` | `map[string]string` | Set additional HTTP headers |
| `WithHTTPClient(c)` | `*http.Client` | Set a custom `*http.Client` |

## Notes

- Declares text-only chat capability with tool calling.
- Reasoning from earlier assistant turns is sent back as `reasoning_content`, and streaming requests set `stream_options.include_usage` so the final chunk carries token usage.
- The package provides `Chat` only. The catalog also lists image, video, speech and embedding models, which this provider does not wrap.
- Environment variable `RUNCRATE_BASE_URL` can override the default endpoint.
