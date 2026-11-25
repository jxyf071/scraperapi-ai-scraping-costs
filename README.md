# Web Scraping for AI: Markdown, JSON, Real Credit Costs, and Which ScraperAPI Plan Fits

Web scraping for AI comes down to two separate questions. Can the API actually retrieve the page, including JavaScript rendering, proxies, and bot protection? And is the output something a model can ingest, meaning clean Markdown or schema-validated JSON? ScraperAPI competes on the first question at the budget end of the market: a 7-day trial with 5,000 credits, a free tier of 1,000 credits a month, and paid plans that start around $49/month. Its fit for the second question, structured AI-ready output, needs a closer look before you build around it.

This article covers the full plan lineup, what a rendered and unblocked request actually costs in credits (not 1), why independent benchmarks disagree by 20 to 60 points on the same tool, and where Firecrawl, Zyte, Bright Data, or Crawl4AI may be the better-shaped tool for your pipeline.

> **Quick pick:** Most individuals and small teams running a normal site list belong on Hobby at $49/month; the free 7-day, 5,000-credit trial tells you whether it gets through your targets before you pay. Move up to Business ($299/month) only when you need country-level geotargeting, which Hobby and Startup don't include.
> 👉 [Start the free trial and compare plans](https://www.scraperapi.com/pricing?fp_ref=coupons)

## ScraperAPI's plan lineup at a glance

The official docs list eight tiers. Per-plan credit allotments and the middle-tier prices sit on the pricing page, so the table below fills in what has been verified and flags the rest rather than guessing.

| Plan | Best for | Credits & concurrency | Price | Get started |
| --- | --- | --- | --- | --- |
| Free | Testing after the trial | 1,000 credits/mo, 5 threads | $0 | [Sign up free](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Hobby | Small lists on normal pages | See pricing page | from $49/mo | [Check Hobby](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Startup | Growing lists | See pricing page | See pricing page | [Check Startup](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Business | Country-level geotargeting | See pricing page | $299/mo | [Check Business](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Scaling | Higher volume + pay-as-you-go | See pricing page | See pricing page | [Check Scaling](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Professional | Pay-as-you-go + auto-upgrade | See pricing page | See pricing page | [Check Professional](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Advanced | Custom configuration | See pricing page | See pricing page | [Check Advanced](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Enterprise | Custom volume and SLAs | Custom | Custom | [Talk to sales](https://www.scraperapi.com/pricing?fp_ref=coupons) |

A few things this table hides. Hobby and Startup route traffic through US or EU locations only; country codes start at Business. Pay-as-you-go, which lets you keep scraping past your plan limit at a fixed per-credit rate up to a cap you set, is available on Scaling, Professional, Advanced, and Enterprise, not on Free, Hobby, Startup, or Business. Auto-upgrade, which bumps you to the next tier when credits run out, is available on Hobby, Startup, Business, Scaling, and Professional.

## What AI scraping actually needs from an API

A working AI scrape runs through three stages: fetch the page (plain HTTP for static pages, a headless browser for anything JavaScript-rendered), convert the HTML into a shorter intermediate format, and map that format to a schema the model can return. Markdown is the usual middle format because it keeps headings, lists, links, and tables while cutting the scripts and attributes that burn tokens without adding meaning.

For a RAG or agent pipeline, the stages extend: fetch, render JS, extract, convert to Markdown or JSON, chunk, embed, retrieve. Production adds freshness, deduplication, and change detection on top. So when you evaluate an API for AI work, look at JS rendering (does it wait for dynamic content), Markdown output (does it preserve tables and headings), schema-constrained extraction (does it return validated JSON, not free text), and proxy plus anti-bot handling. A tool that gets all four right costs more than one that gets one right.

Two failure modes show up again and again. Content comes back empty because the page needs JS rendering. Or retrieval returns the wrong chunk because the pipeline split text arbitrarily instead of by heading. Both are fixable: render before extraction, and chunk by document structure.

## Why the same tool's success rate swings 40 points

The ScraperAPI success rate you read depends on which benchmark you read. A Proxyway test put Zyte first with 93% success on heavily protected sites; the same tool landed 11th in String's benchmark at 68%. Firecrawl managed 33.69% in Proxyway on heavily protected pages but 80% in String's. ScraperAPI went from 64% to 84% once it was configured at its strongest setting, and a separate October 2026 benchmark across 11 sites measured 60% overall, ranking it fifth of six APIs tested, at an average response time of 10.6 seconds.

The numbers disagree that much because the target URLs differ, the configs differ, and "success" means different things across testers (a 200 response versus actually getting the target data). None of these numbers tell you how ScraperAPI will do on your URLs. A small test does.

Pick 20 to 50 URLs that match the sites you actually need, fix the config across tools, and record success rate, latency, and credits burned per request. That data is worth more than any vendor homepage claim. If you go that route, ScraperAPI's `sa-credit-cost` response header and its per-URL cost estimator let you see the credits a given page burned, and a `max_cost` cap stops a single request from exceeding the ceiling you set.

## What a rendered request actually costs

ScraperAPI prices in credits, and the headline number is 1 credit per standard page. For AI scraping, 1 is almost never what you pay. Add the two features AI work relies on and it compounds fast.

| Request type | Credit cost |
| --- | --- |
| Standard page | 1 |
| E-commerce (Amazon, Walmart, eBay) | 5 |
| SERP (Google, Bing) | 25 |
| Social (LinkedIn) | 30 |
| Bot-protection bypass (Cloudflare, DataDome, PerimeterX, Akamai) | +10 |
| JS rendering (`render=true`) | +10 |
| Premium proxies | +10 |
| Screenshot | +10 |
| Premium proxies + JS rendering | 25 |
| Ultra Premium | 30 |
| Ultra Premium + JS rendering | 75 |

A page that needs both JS rendering and bot-protection bypass costs 21 credits, not 1. A benchmark that logged every request found an average of about 25 credits per request across its test set. If you're budgeting an AI or RAG pipeline against the "1 credit" line on the marketing page, you're off by roughly 20×.

That changes which plan is right. A 1,000-credit free tier buys around 47 fully rendered, unblocked pages. That's a test, not a workload. If your targets are mostly static documentation or product pages, the 1-credit rate applies and the free tier goes a lot further.

## Matching the tool to the job

The market splits by what a tool is actually optimized for.

- **Clean Markdown and schema output:** Firecrawl returns Markdown or JSON from a single call and is the most often recommended for LLM-ready output, at the cost of the lowest success on heavily protected pages in Proxyway's test. Pricing from a 2026 API ranking starts around $19/month.
- **Heavily protected sites:** Zyte's team comes from Scrapy, auto-selects proxy and render configs, and offers AI extraction into typed schemas (products, articles, jobs). Its Proxyway result of 93% is the strongest on the list; its String result of 68% shows the ceiling is not universal. Starts from a $5 free credit, then around $0.13 per 1,000 requests.
- **Scale and ready-made data:** Bright Data runs Web Unlocker for proxy, CAPTCHA, and rendering, plus SERP API and 100+ prebuilt scrapers. 5,000 free requests per month, then $1.50 per 1,000 successful requests pay-as-you-go; Scale at $499/month, then $1.30 per 1,000. Failed requests are billed too when custom Web Unlocker features are on.
- **Self-hosted on a budget:** Crawl4AI is Apache 2.0 Python, built on Playwright, outputs LLM-ready Markdown and JSON, and is free. Around 87,000 GitHub stars, but you own the rendering infrastructure and the proxy problem.

The practical rule: if the output format is your bottleneck, pay for Firecrawl or run Crawl4AI. If getting through bot protection is the bottleneck, Zyte and Bright Data have the best numbers. If you want a general-purpose unblocker that handles most normal pages for less, ScraperAPI is the budget option, and the config matters enough that it's worth 64% versus 84% on the same tool.

## Hidden limits to price in before you buy

The credit table above is only half the cost story. Three limitations that appear in independent reviews of ScraperAPI and in its own docs will affect how a pipeline behaves.

**Geotargeting.** Hobby and Startup are US or EU only. If your workload needs per-country IPs, country codes start at Business at $299/month.

**Cache.** Independent testing reports a 10-minute result cache forced on difficult targets, which means a "re-fetch" inside that window may return stale data. For change-detection pipelines, that breaks your freshness assumption unless you build around it.

**Credit rollover.** Credits don't carry month to month. That's typical for the category, but it matters if your crawl volume is spiky.

On the AI side, a third-party October 2026 review recorded a hosted MCP server, LangChain and LlamaIndex integrations, and Markdown or text output at no extra credit cost. Those are worth testing on the trial, since they're features of the integration layer rather than the core pricing page.

## Compliance is part of availability

Cloudflare now blocks many AI crawlers by default on ad-supported pages. That's a live constraint on any pipeline that expects to fetch open web pages in 2026; a high success rate on normal pages doesn't mean much if half your targets sit behind a default block.

Crawlers fall into three groups with different user-agent and robots.txt behavior. Training crawlers (GPTBot, ClaudeBot) collect pages for model training. Retrieval crawlers (OAI-SearchBot, Claude-SearchBot, ExaSearchBot) fetch pages for AI answers. Infrastructure tools like Firecrawl and Crawl4AI crawl on behalf of their users, with UA and robots.txt behavior that vary by tool and configuration. Before you buy, check how the tool identifies itself and whether it respects robots.txt and terms of service. Robots.txt, terms, and copyright are hard limits, not obstacles to route around.

One concrete data point on ScraperAPI: an independent test logged a 403 ToS-denylist on every tier for Instagram and X, which matches those platforms' published scraping prohibitions. Expect to focus on retail, search, reviews, and documentation targets instead.

## FAQ

**Is web scraping for AI legal?**
Scraping public data is generally lawful in many jurisdictions, but robots.txt, terms of service, and copyright set hard boundaries, and scraping private data, overloading servers, or republishing content verbatim are the common problem cases. The practitioner carries the compliance responsibility.

**Do I need a headless browser?**
Only for dynamic content. Static pages should be fetched with a plain HTTP request first; move to a browser when you confirm the content is rendered after load. In one internal test of SaaS documentation pages, 34% needed headless rendering because the main content arrived after the initial HTML.

**Markdown or JSON?**
Markdown is the safer intermediate format for retrieval because it keeps headings, lists, links, and tables. Use JSON when the model must return specific typed fields, and constrain it with a JSON or Zod schema so missing fields surface before ingestion.

**How many credits does a JS-rendered page cost on ScraperAPI?**
Ten credits for `render=true`, plus the base 1 credit for a standard page. Add bot-protection bypass and you're at 21. Premium proxies plus rendering is priced at 25. Check your specific target with the in-dashboard cost estimator before you commit.

**Is ScraperAPI the right choice for an AI agent stack?**
It's fine as a general unblocker if your sites are normal pages and you're prepared to configure it. If structured extraction and clean Markdown are the primary need, Firecrawl or Crawl4AI cover that layer more directly. If heavy bot protection is the blocker, Zyte and Bright Data have stronger published numbers.

## The decision

Run the free 7-day, 5,000-credit trial against your real target list before you pick a paid plan. If your pages are static text, you may not need ScraperAPI at all, since Crawl4AI runs free in your own infrastructure. If your pages are behind Cloudflare and DataDome, expect Zyte or Bright Data to clear more of them, at a higher per-request price. If your targets are ordinary product, search, or review pages and you want a budget unblocker to configure, Hobby at $49/month is the starting point.

Two things to confirm before checkout: the current monthly price on the pricing page, since ScraperAPI's pricing has changed through 2026, and the real credit cost of your top 10 target URLs. A page that costs 21 credits is a different product from one that costs 1.

👉 [Compare ScraperAPI's current plans and start the free trial](https://www.scraperapi.com/pricing?fp_ref=coupons)
