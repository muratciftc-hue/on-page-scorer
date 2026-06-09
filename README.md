# On-Page SEO Scorer — Claude Code Skill

AI-powered on-page SEO scoring system for Claude Code. Analyzes any web page against a target keyword across 9 weighted categories and returns a prioritized action list.

## Installation

Copy the `on-page-scorer` folder into your Claude Code skills directory:

```bash
cp -r on-page-scorer ~/.claude/skills/
```

## Usage

```
/on-page-scorer https://example.com/page target-keyword
```

### Batch Analysis

```
/on-page-scorer https://example.com/page1 https://example.com/page2 target-keyword
```

## Scoring Categories

| Category | Weight | What It Measures |
|----------|--------|-----------------|
| Keyword Coverage | 20% | Keyword placement in title, H1, meta desc, first 100 words, URL, alt text, density |
| Content Depth | 20% | Word count, H2 count, paragraphs, images, schema, E-E-A-T signals |
| Heading Structure | 10% | H1 uniqueness, keyword in headings, hierarchy, heading density |
| Meta Title | 10% | Length (50-55 chars/580px), keyword position (first 30-35 chars), power words |
| Meta Description | 10% | Length (120-155 chars, mobile-first), keyword in first 120 chars, CTA |
| Internal Linking | 8% | Link density (2-5/1000 words), anchor text quality, contextual placement |
| CTA | 7% | CTA presence, button format, placement, message variety |
| Readability | 8% | Sentence length, paragraph length, scanability, heading density |
| AI Content Quality | 7% | Naturalness, originality, keyword integration, search intent, actionability |

## Score Ranges

| Score | Status | Action |
|-------|--------|--------|
| 80-100 | Excellent | Review quarterly |
| 60-79 | Good / Optimizable | Optimize within 30 days |
| 40-59 | Weak | Rewrite within 14 days |
| 0-39 | Critical | Immediate intervention |

## Methodology Sources

- [theStacc Content Scoring Framework (2026)](https://thestacc.com/blog/content-scoring-guide/)
- [Surfer SEO Content Score](https://docs.surferseo.com/en/articles/5700317-what-is-content-score)
- [Semrush On-Page SEO Checklist (2026)](https://www.semrush.com/blog/on-page-seo-checklist/)
- [Google Helpful Content Guidelines](https://developers.google.com/search/docs/fundamentals/creating-helpful-content)
- [Zyppy Meta Title Length Research](https://zyppy.com/title-tags/meta-title-tag-length/)
- [Shopify Internal Linking Guide (2026)](https://www.shopify.com/blog/internal-links-seo)
- [Portent Readability Study](https://portent.com/blog/content/study-how-content-readability-affects-seo-and-rankings.htm)
- [WiserNotify CTA Statistics (2026)](https://wisernotify.com/blog/call-to-action-stats/)

## Requirements

- Claude Code CLI or Claude Code Desktop
- WebFetch tool access (for fetching page HTML)

## License

MIT
