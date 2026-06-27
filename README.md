# How Thirsty Is Your AI Session?
### An AI Resource Consumption Calculator

**By [Kaneda Consulting](https://www.kanedaconsulting.com) · Human leadership for the AI era**

---

A free, single-file web calculator that estimates the water and energy consumed by an AI conversation based on your session's total token counts. Built to make the environmental cost of AI tangible — not abstract.

Enter your input and output token totals (from a tool like [ccusage](https://github.com/ryoppippi/ccusage)) and get an estimated water and energy figure for your session, with per-query averages if you supply a query count.

---

## Why this exists

Every AI query draws from physical resources — water for cooling data centers, energy from the grid. Those costs are real, local, and largely invisible. If you're in the Pacific Northwest, the data center processing your query may be 50–120 miles away.

This calculator is designed for educators, researchers, and anyone who wants to put a number on that cost in a conversation rather than in the abstract.

---

## What it calculates

- **Water consumed** (mL) — on-site cooling + off-site electricity generation water
- **Energy drawn** (Wh) — GPU + infrastructure overhead per query

Results are shown as session totals, with optional per-query averages.

---

## Supported models

| Provider | Model basis | Data quality |
|----------|------------|--------------|
| Gemini (Google) | Fleet median — Gemini Apps | ✅ Directly measured |
| Claude (Anthropic / AWS) | Claude 3.7 Sonnet ceiling / Oviedo floor | ⚠️ Estimated range |
| ChatGPT (OpenAI / Azure) | GPT-4o ceiling / Oviedo floor | ⚠️ Estimated range |

Gemini figures come from Google's own full-stack production instrumentation. Claude and ChatGPT figures are derived from independent peer-reviewed research — neither Anthropic nor OpenAI has published directly measured per-query production data.

---

## Methodology

**Energy + Water — Gemini:**
```
E     = 0.24 Wh  × (output_tokens / 300)
Water = 0.26 mL  × (output_tokens / 300)
```
Both directly measured. Source: Elsworth et al. (2025) — arXiv:2508.15734

**Energy — Claude / ChatGPT floor:**
```
E_floor = 0.34 Wh × (output_tokens / 300)
```
Source: Oviedo et al. (2026) — *Joule* DOI: 10.1016/j.joule.2026.102430

**Energy — Claude / ChatGPT ceiling:**
```
E_ceil = Jegham et al. (2025) Table 4, interpolated to output_tokens
```
Source: Jegham et al. (2025) — arXiv:2505.09598v6

**Water — Claude / ChatGPT:**
```
Water (mL) = [(E_kwh / PUE) × WUE_site + E_kwh × WUE_source] × 1000
```
Source: Jegham et al. (2025) Eq. 4. WUE_site values from provider disclosures (AWS: 0.12 L/kWh; Azure: 0.27 L/kWh). PUE and WUE_source from Jegham et al. Table 1.

---

## Data sources

- **Elsworth et al. (2025)** — "Measuring the environmental impact of delivering AI at Google Scale." arXiv:2508.15734. The only directly measured, published, production-scale figure for any major AI assistant.
- **Oviedo et al. (2026)** — "Energy Use of AI Inference: Efficiency Pathways and Test-Time Scaling." *Joule.* DOI: 10.1016/j.joule.2026.102430. Peer-reviewed production-scale H100 benchmark.
- **Jegham et al. (2025)** — "How Hungry is AI? Benchmarking Energy, Water, and Carbon Footprint of LLM Inference." arXiv:2505.09598v6. Model-specific estimates for Claude 3.7 Sonnet and GPT-4o.
- **AWS WUE/PUE:** PUE 1.14, WUE_source 5.11 from Jegham et al. (2025) Table 1. WUE_site updated to 0.12 L/kWh: Amazon (2025) "Amazon data center water usage." aboutamazon.com/news/sustainability/amazon-data-center-water-usage. Self-reported 2025 figure, a 52% improvement since 2021.
- **Azure WUE/PUE:** PUE 1.12, WUE_source 4.35 from Jegham et al. (2025) Table 1. WUE_site updated to 0.27 L/kWh: Microsoft (2026) "Inside Microsoft's two-decade push to cut water intensity while scaling for growth." blogs.microsoft.com, June 24 2026. Self-reported 2025 figure for owned datacenter fleet.

### Data gap

Published benchmarks cover models available through late 2025. Current models — Claude 4.x, GPT-5, Gemini 2.x — have not yet been independently benchmarked in peer-reviewed literature. Since models continue to improve, actual current consumption is likely lower than these figures.

---

## Usage

This is a single self-contained HTML file. No dependencies, no build step, no server required.

1. Download `ai_consumption_calculator.html`
2. Open it in any browser
3. Enter your total input and output token counts
4. Optionally enter the number of queries in your session for per-query averages

To get your token counts, run [ccusage](https://github.com/ryoppippi/ccusage) on your conversation, or check your API usage logs.

---

## How to get your token counts

If you're using Claude via the API, ccusage provides per-conversation token breakdowns:

```bash
npx ccusage
```

The calculator uses **total output tokens** as the primary driver of energy and water estimates, since output generation dominates inference cost.

---

## Built with AI

This calculator was built entirely through a conversational session with **Claude** (Anthropic). No code was written by hand.

| Metric | Value |
|--------|-------|
| Assistant | Claude (Anthropic) |
| Total input tokens | ~80,000 |
| Total output tokens | ~76,600 |
| Queries in session | 72 |
| Session focus | Research, methodology, design iteration |

The session covered: sourcing and evaluating peer-reviewed research, building and iterating the calculator methodology, and designing the interface through ~30 rounds of visual feedback. The token counts above are estimates.

---

## Have more current data?

The biggest limitation of this calculator is that the underlying research moves slower than the models. If you have access to more recent published figures for water or energy consumption — whether from a new paper, a provider disclosure, or a life-cycle assessment — please [submit an issue](../../issues) to this repo.

Useful contributions include:
- Peer-reviewed papers published with data collected after mid-2025 with per-query energy or water figures
- Official provider disclosures with methodology (not just aggregate sustainability reports)
- Corrections to WUE, PUE, or CIF values for any of the three providers
- Benchmarks covering Claude 4.x, GPT-5, or Gemini 2.x or later.

Please include a link to the source and note whether the figures are directly measured or estimated.

---

## License

MIT License — Copyright (c) 2026 Kaneda Consulting

Permission is hereby granted, free of charge, to any person obtaining a copy of this file to use, copy, modify, merge, publish, or distribute it, provided the above copyright notice and this permission notice appear in all copies.

---

## About Kaneda Consulting

Kaneda Consulting provides human-centered AI leadership education for executives, organizations, and educators navigating the AI era.

[kanedaconsulting.com](https://www.kanedaconsulting.com)
