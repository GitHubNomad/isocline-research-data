# Isocline Research Data

Raw research data published by [The Isocline Index](https://isocline.ai) alongside each article.

Each folder corresponds to a published article and contains the quantitative data dossier compiled by our research pipeline — STAT_ROW figures, RISK_TABLE assessments, trend projections, patent references, and raw source citations.

## Articles

| Article | Folder |
|---------|--------|
| [Small Modular Reactors: Solving the AI Data Center Power Crisis](https://hestia-media-group.com/articles/small-modular-reactors-ai-data-center-power-crisis/) | [`small-modular-reactors-ai-data-center-power-crisis/`](small-modular-reactors-ai-data-center-power-crisis/) |
| [The Grid Problem: How AI Data Centers Are Reshaping Power Infrastructure](https://hestia-media-group.com/articles/ai-data-centers-power-infrastructure/) | [`ai-data-centers-power-infrastructure/`](ai-data-centers-power-infrastructure/) |
| [Neuromorphic Chips and the Future of Edge AI](https://hestia-media-group.com/articles/neuromorphic-chips-and-the-future-of-edge-ai/) | [`neuromorphic-chips-and-the-future-of-edge-ai/`](neuromorphic-chips-and-the-future-of-edge-ai/) |
| [RISC-V in Safety-Critical Systems: Navigating Certification Challenges](https://hestia-media-group.com/articles/risc-v-safety-critical-certification/) | [`risc-v-safety-critical-certification/`](risc-v-safety-critical-certification/) |
| [CXL 3.0 and the End of the Memory Wall](https://hestia-media-group.com/articles/cxl-30-and-the-end-of-the-memory-wall/) | [`cxl-30-and-the-end-of-the-memory-wall/`](cxl-30-and-the-end-of-the-memory-wall/) |

## What's in Each Dossier

Each `heimdall-data-dossier.md` is the raw output of our Heimdall data-analysis agent, run before the article is drafted. It contains:

- **STAT_ROW** — verified quantitative data points with sources and dates
- **RISK_TABLE** — technical risk assessments with severity ratings and barrier descriptions
- **TREND_PROJECTIONS** — near/mid/long-term forecasts with confidence levels and citations
- Topic-specific sections (patent landscape, market data, regulatory timelines, etc.)

Sources are drawn from arXiv, IEEE Xplore, ACM Digital Library, Google Patents, USPTO, and IETF.

## Why We Publish This

Developers share useful repositories faster than they share articles. If you find the raw data useful — for your own research, to verify our claims, or to build on — fork it. If you spot errors, open an issue.

## License

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — Attribution: Hestia Media Group (hestia-media-group.com)
