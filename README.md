# ecommerce scraping: how to get product data past the anti-bot walls, with full plan pricing and the credit math behind Amazon scraping

Most "ecommerce scraping" tutorials teach you parsing. That's the easy part. The 2026 Imperva Bad Bot Report puts automated traffic at more than 53% of all web traffic, and retailers answer it with anti-bot layers that check TLS fingerprints, IP reputation, cookies, browser signals and request patterns — your request gets blocked before a parser ever runs. So the real question behind this keyword is: how do you reliably pull product prices, stock and seller data from sites like Amazon and Walmart at volume, without building your own proxy pool and CAPTCHA-solving pipeline?

This article covers the three things that actually decide whether your project works: what a scraping API does for you, what it costs (including the per-request credit math most reviews gloss over), and where ScraperAPI fits compared with alternatives like Oxylabs and Bright Data.

> **Quick pick:** for most price-monitoring and competitor-tracking projects, ScraperAPI's Hobby plan at **$49/month** (100,000 credits — roughly 20,000 Amazon product pages) is the sensible entry point, dropping to **$44.10/month** on annual billing. Every plan comes with a 7-day free trial of 5,000 credits, no credit card required. 👉 [Compare all plans and start your free trial](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## Why ecommerce scraping fails, and what it takes to fix it

Ecommerce scraping fails at the access layer, long before data extraction. Amazon, Walmart, Nike and the big marketplaces run layered defenses that evaluate who you are before serving a page: a plain HTTP request from a datacenter IP typically returns a bot-detection page, not product data. On top of that, prices shift with location, login state and time — the same product can show $49 from one IP and $54 from another — so even a successful request can give you data you can't compare across runs.

Doing this yourself means maintaining a rotating proxy pool, a headless browser fleet, CAPTCHA solving, retry logic and session management. That's a real engineering project. A scraping API like ScraperAPI packages all of it behind one API call: you send the target URL, the service handles proxies, JavaScript rendering, CAPTCHA and retries, and returns the HTML or parsed data. The company behind it says it serves over 10,000 data-driven businesses and processes roughly 36 billion API requests a month — scale figures worth knowing, though self-reported. Whether the trade is worth it depends on volume and target difficulty, which is exactly what the credit math below is for.

## ScraperAPI plans: the full current lineup

ScraperAPI's pricing page currently shows eight tiers. All paid plans include JavaScript rendering, premium residential and mobile IPs, anti-bot bypass, the structured data endpoints, DataPipeline access and priority support — the differences are credits, concurrency, geo-targeting, billing options and analytics history.

| Plan | Monthly / annual (per mo) | API credits / mo | Concurrency | Key differences | Get it |
| --- | --- | --- | --- | --- | --- |
| Free | $0 | 1,000 | 5 | No geo-targeting; enough for small tests | [Sign up free](https://www.scraperapi.com/?fp_ref=coupons) |
| Hobby | $49 / $44.10 | 100,000 | 20 | Geo-targeting limited to US & EU | [Get Hobby](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Startup | $149 / $134.10 | 1,000,000 | 50 | Geo-targeting still US & EU only | [Get Startup](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Business | $299 / $269.10 | 3,000,000 | 100 | Country-level geo-targeting unlocks | [Get Business](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Scaling | $475 / $427.50 | 5,000,000 | 200 | Marked "most popular" on site; pay-as-you-go available | [Get Scaling](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Professional | $975 / $877.50 | 10,500,000 | 300 | Country-level targeting + pay-as-you-go | [Get Professional](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Advanced | $1,975 / $1,777.50 | 21,500,000 | 500 | Large-scale collection tier | [Get Advanced](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Enterprise | Custom quote | 22,000,000+ | 500+ | Beyond this scale goes through sales | [Contact sales](https://www.scraperapi.com/pricing/?fp_ref=coupons) |

A few plan details that don't fit in the table but matter:

- **Geographic targeting is gated.** Hobby and Startup can only target US and EU. If you need localized prices on Amazon Germany, Japan or other country-specific storefronts, you need Business ($299/month) or above. This is the single most common reason to skip the two cheapest tiers for international price monitoring.
- **Analytics history is limited on the low tiers.** The dashboard shows only the last 30 days of usage data on the Free, Hobby and Startup plans; Business and up get unlimited history.
- **Annual billing saves 10%** across all plans. In real money: $58.80/year on Hobby, $178.80/year on Startup, $358.80/year on Business, and $570/year on Scaling.
- **There's currently a first-month promo code, START10**, good for 10% off any paid plan's first month — confirmed by multiple coupon aggregators as still active as of an August 2026 update. It stacks with the free trial (trial first, code when you upgrade). Ignore the "28% off" and "65% off" codes floating around coupon sites; aggregators flag those as unverifiable.
- **Running out of credits** pauses scraping until renewal by default. You can enable auto-upgrade (Free through Professional) or pay-as-you-go continuation at a fixed per-credit rate with a monthly cap (Scaling and above). Third-party pricing breakdowns also note that credits don't roll over between billing cycles — another reason to right-size rather than overbuy.

## The credit math most reviews skip

This is where ecommerce scraping budgets get blown. ScraperAPI doesn't charge per request flat — its documentation prices each request **by domain type**:

| Target type | Cost per request |
| --- | --- |
| Regular websites (blogs, news, general HTML) | 1 credit |
| E-commerce domains (Amazon, Walmart, eBay) | 5 credits |
| Search engines (Google, Bing, including subdomains) | 25 credits |
| Social media (LinkedIn) | 30 credits |
| Cloudflare / Datadome / PerimeterX / Akamai bypass (when triggered) | +10 credits per request |

Amazon's 5-credit rate is the one that matters here. A Hobby plan's 100,000 credits buys you about **20,000 Amazon product pages per month** — respectable for monitoring a few hundred ASINs daily. The same 100,000 credits would only cover 4,000 Google searches if SERP tracking sneaks into your workflow, so mixed-target projects burn credits faster than the plan name suggests.

Add-on parameters stack more costs. Per the official documentation, `render=true` (JavaScript rendering) adds 10 credits, `premium=true` adds 10, and `ultra_premium=true` adds 30 per request — and combinations are billed as combo rates (premium + render costs +25, ultra premium + render +75), not simple sums. If you run ultra-premium rendering against a heavily protected target at 75+ credits per request, that same 100,000-credit Hobby plan shrinks to roughly 1,300 pages. Worth knowing before you enable flags indiscriminately.

Four billing details from the documentation that almost no roundup mentions:

1. Each response carries a `sa-credit-cost` header showing exactly what that request consumed — useful for auditing spend in real time.
2. Only successful requests (HTTP 200 and 404) are billed, plus requests you deliberately cancel before the 70-second timeout. Failed blocks and 5xx errors don't cost credits.
3. The **Domain Cost Estimator** in your dashboard shows the exact credit cost of any URL before you scale up, and there's also an account-level URL cost endpoint you can call programmatically.
4. The `max_cost` parameter caps what any single request can consume, so one misconfigured call can't eat a day's budget.

👉 [Check your target domain's exact credit cost on the pricing page](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## Structured data endpoints: skip the parser entirely

The parsing half of ecommerce scraping — cleaning raw HTML into price, stock, SKU and seller fields — is where most DIY projects spend their weeks. ScraperAPI's structured data endpoints remove that step for the big targets by returning clean JSON directly. Credit cost matches the domain's normal rate, so Amazon endpoints run at 5 credits per request like any other Amazon call.

For ecommerce specifically, the Amazon endpoints are the deep part of the catalog:

- **Amazon Product API**: product name, price, availability, shipping price, ratings and review counts, images, feature bullets, best-seller rank, coupon flags and seller info — 18+ fields in one call, across 21 international Amazon storefronts (.com, .co.uk, .de, .co.jp, .com.au and more). It also supports US ZIP-code-level targeting, so you can pull the localized price and shipping a customer in a specific ZIP would see.
- **Amazon Search API**: full search and category pages as structured JSON — positions, ASINs, names, prices, prime flags, best-seller and Amazon's Choice badges, plus the "1K+ bought in past month" style signals.
- **Amazon Offers API**: third-party seller offers with seller name, condition, price and shipping — the data you need for Buy Box monitoring, since the visible price on Amazon is only the winning offer out of possibly 15+ sellers.

Walmart, eBay and Etsy have dedicated endpoints at the standard 5-credit e-commerce rate. Google Shopping runs at the 25-credit SERP rate. Beyond ecommerce, the same structured catalog covers Trustpilot, G2, app stores and more — and as of late 2025, ScraperAPI also publishes an MCP server so you can wire the scraping API into Claude for ad-hoc extraction from protected, JavaScript-heavy pages.

On reliability: one third-party write-up circulating in early 2026 claims a 100% success rate on Amazon product pages with an average response time around 11.8 seconds. ScraperAPI itself publishes a guide pointing out that published scraping success rates across the industry run from 40% to 100%, and tells users to benchmark their own targets rather than trust vendor averages. The reasonable read: test your specific domains during the trial before committing to a plan.

👉 [See the Amazon scraper endpoints and sample responses](https://www.scraperapi.com/solutions/ecommerce-data-collection/amazon-scraper/?fp_ref=coupons)

## How ScraperAPI compares with Oxylabs and Bright Data

For a "which tool" decision, here's the honest picture using third-party data from mid-2026 (these numbers come from comparison sites, not the vendors' own pages, so verify current pricing before committing):

| Dimension | ScraperAPI | Oxylabs E-commerce Scraper API | Bright Data |
| --- | --- | --- | --- |
| Entry paid price | $49/mo (100,000 credits) | Micro $49/mo (up to 98,000 results) | Growth $499/mo |
| Billing model | Per credit / request | Per result | Per result or record |
| Free trial | 7-day, 5,000 credits + permanent 1,000 credits/mo free tier | Up to 2,000 results, no card | Pay-as-you-go trial |
| E-commerce specialization | Structured endpoints for Amazon, Walmart, eBay, Etsy | Dedicated e-commerce scraper API | 600+ pre-built scrapers incl. Amazon, Walmart, eBay, Shein, Shopify, returning normalized JSON |

The billing model difference is the crux. On paper, Oxylabs' $49 Micro tier delivering up to 98,000 results looks far cheaper than ScraperAPI's 20,000 Amazon pages at the same price. But per-result plans and per-request credit plans count differently — a "result" and a "page request" aren't the same unit, and the real comparison depends on whether you'd otherwise pay developer time to build parsers. ScraperAPI's structured endpoints return that parsed JSON as part of the request, which is where its premium over raw proxy bandwidth gets justified. Third-party reviewers broadly land in the same place: ScraperAPI is a solid choice when your targets are e-commerce and real-estate domains, while its per-credit model gets expensive against ultra-protected targets like LinkedIn or Instagram at high volume.

If your need is breadth — normalized product data from hundreds of retailers with zero parser work — Bright Data's pre-built scraper catalog is built for exactly that, at a higher entry price. If your need is depth on Amazon specifically, with ZIP-level price localization and offer-level data, ScraperAPI's endpoint set is the more targeted fit.

## When ScraperAPI isn't the right choice

Being straight about the failure cases:

- **SERP-heavy projects.** At 25 credits per Google request, search engine monitoring on ScraperAPI costs five times an Amazon page. If your workflow is mostly rank tracking, a SERP-specialized provider will usually cost less.
- **Social media scraping.** LinkedIn at 30 credits per request is the most expensive category on the platform; reviewers consistently flag this as the case where dedicated residential proxies beat a credit-based API.
- **Very large volumes on heavily protected targets.** If every request needs ultra-premium rendering, do the math at 75+ credits per request before buying a plan — bandwidth-billed proxies may win. Scaling tier and above gets pay-as-you-go pricing, which is designed for exactly this calculation.
- **Over 22 million requests per month.** That's custom-quote territory via the sales team, not self-serve plans.

## Getting started without wasting credits

The trial makes it cheap to validate before paying. A workable sequence:

1. Sign up for the 7-day trial (5,000 credits, no credit card) and add your target URLs.
2. Run each target through the Domain Cost Estimator first. A "small" monitoring project on a Cloudflare-protected retail site costs 11 credits per request (1 + 10 bypass), which changes plan math considerably.
3. Set `max_cost` on your requests so a single call can't run past your limit.
4. Use the structured endpoints for Amazon and Walmart instead of parsing HTML yourself.
5. If the project continues, decide between annual billing (10% cheaper) and monthly, apply START10 on the first month if you go monthly, and enable auto-upgrade only if interruption would hurt more than an occasional overage.

👉 [Start your free 7-day trial with 5,000 credits](https://www.scraperapi.com/?fp_ref=coupons)

## FAQ

**How many Amazon pages can each plan scrape per month?** At 5 credits per Amazon request: Hobby ≈ 20,000 pages, Startup ≈ 200,000, Business ≈ 600,000, Scaling ≈ 1,000,000. JavaScript rendering adds 10 credits per request and cuts those figures by two thirds.

**Do unused credits roll over?** According to third-party pricing breakdowns, credits expire at the end of each billing cycle with no rollover. Annual plans can renew early for a fresh batch if you need a top-up mid-cycle.

**What happens when I hit the concurrency limit?** Requests return a 429 and are rejected, not queued — you're not charged for them, but your scraper needs to handle retries. Hobby allows 20 concurrent connections, Scaling 200.

**Is ecommerce scraping legal?** Public product and category pages carry lower risk than pages behind logins, and some US court decisions have found that accessing publicly available information may not violate the CFAA in certain circumstances — but terms of service, privacy law (GDPR in the EU, CCPA in California), copyright and state law can all still apply. Skip personal data fields you don't need, keep request rates moderate, and consult a lawyer for commercial multi-site projects; this isn't legal advice.

**Does the free plan really exist?** Yes — 1,000 credits per month with 5 concurrent connections, indefinitely, on top of the 7-day/5,000-credit trial.

## Picking a plan, case by case

If you're monitoring under 10,000 Amazon pages a month and only need US/EU prices, Hobby at $49 (or $44.10 annual) covers it with headroom — don't pay for tiers whose geo-targeting you can't use. If localized pricing across Amazon's international storefronts is the whole point of your project, Business at $299 is where country-level targeting unlocks, and no cheaper plan gets you there. If your targets mix Amazon with Google Shopping or SERP data, run the Domain Cost Estimator before choosing a tier, because the 25-credit SERP rate will dominate your budget. And if you're genuinely unsure whether an API beats self-built proxies for your targets, the 7-day trial answers that question for the cost of nothing: 👉 [test ScraperAPI on your own target sites with the free trial](https://www.scraperapi.com/?fp_ref=coupons).
