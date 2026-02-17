# Evenflow Interiors — Vendor Scrape Report

**Date:** February 16, 2026
**Total Products in Database:** 140,512
**Vendors with Products:** 61 of 78 registered
**AI Search Status:** Live and searchable at the Evenflow Product Search chatbot

---

## How to Read This Report

| Column | What It Means |
|--------|---------------|
| **Products** | Total number of products pulled from this vendor |
| **Priority** | How important this vendor is to Evenflow's business (High / Medium / Low) |
| **Desc%** | Percentage of products with a written description (helps the AI understand and match products) |
| **Img%** | Percentage of products with a photo |
| **Dims%** | Percentage of products with dimensions (W x D x H) |
| **Price%** | Percentage of products with pricing info (many trade vendors hide prices) |
| **Mat%** | Percentage of products listing their material (wood, marble, iron, etc.) |
| **Fin%** | Percentage of products listing their finish (e.g. dark walnut, polished brass) |
| **Stock%** | Percentage of products with stock/availability info |

Dashes (—) mean the vendor's website doesn't publish that information, which is normal for trade-only vendors.

---

## Fully Scraped Vendors — Strong Data Quality

These vendors have rich, complete product data. Search results from these will look great.

| Vendor | Products | Priority | Desc% | Img% | Dims% | Price% | Mat% | Fin% | Stock% |
|--------|----------|----------|-------|------|-------|--------|------|------|--------|
| Four Hands | 17,911 | High | 100 | 100 | 100 | 100 | 100 | 100 | 88 |
| Jonathan Charles | 429 | High | 100 | 100 | 100 | 100 | 100 | 88 | 100 |
| Caracole | 1,125 | High | 100 | 100 | 100 | 100 | 57 | 88 | 100 |
| Currey & Company | 3,102 | High | 100 | 100 | — | 100 | 100 | 100 | — |
| Theodore Alexander | 2,440 | High | 100 | 100 | — | 100 | 100 | 86 | 59 |
| ELK Home | 2,432 | Med | 100 | 100 | — | 100 | 100 | 71 | 100 |
| Cyan Design | 2,303 | Med | 100 | 100 | — | 100 | 100 | 82 | 100 |
| Renwil | 1,559 | Med | 100 | 100 | — | 100 | 99 | 62 | 100 |
| Uttermost | 4,719 | High | 100 | 100 | — | 100 | 86 | 10 | 100 |
| Old Biscayne Designs | 3,323 | Med | 100 | 100 | 72 | 100 | 89 | 90 | — |
| Loloi Rugs | 5,089 | High | 98 | 98 | — | 100 | 5 | — | 100 |
| Eastern Accents | 9,501 | Med | 100 | 87 | 12 | 100 | 32 | — | 100 |
| Jaipur Living | 2,740 | High | 100 | 100 | — | 100 | — | — | 100 |
| Wendover Art Group | 10,000 | High | 100 | 100 | — | 100 | 72 | 56 | 100 |
| Shadow Catchers Art | 4,193 | Med | 100 | 100 | — | 100 | 14 | 12 | 100 |
| Paragon PG | 4,038 | Med | 100 | 100 | — | 100 | — | — | 100 |
| Propac Images | 1,292 | Med | 100 | 100 | — | 100 | — | — | 100 |
| Made Goods | 1,484 | High | 100 | 100 | — | 100 | — | — | 100 |
| Savoy House | 2,099 | Med | 100 | 100 | — | 100 | — | — | 100 |
| Jamie Young Co | 870 | Med | 100 | 100 | — | 100 | — | — | 100 |
| Maitland-Smith | 768 | High | 100 | 99 | 100 | 100 | 6 | 5 | 90 |
| Ambella Home | 708 | Med | 100 | 100 | 100 | 100 | — | — | 100 |
| Artistica Home | 612 | Med | 100 | 100 | 100 | 100 | 22 | 20 | 72 |
| Universal Furniture | 1,097 | High | 100 | 100 | 99 | 100 | — | — | — |
| Summer Classics | 1,027 | High | 98 | 96 | — | 100 | 84 | 20 | 100 |
| Interlude Home | 1,076 | Med | 77 | 100 | — | 100 | — | — | 100 |
| Eloquence | 398 | Med | 100 | 100 | — | 100 | — | — | 100 |
| Padma's Plantation | 213 | Med | 100 | 100 | — | 100 | — | — | 100 |
| Selamat Designs | 235 | Med | 98 | 98 | — | 100 | — | — | 100 |
| James Martin Vanities | 498 | Med | 100 | 100 | — | 100 | 6 | 6 | 100 |
| Hekman | 419 | Low | 100 | 100 | — | 100 | — | — | 100 |
| Greenhouse Fabrics | 89 | Med | 100 | 100 | — | — | 100 | — | — |
| Sarreid Ltd | 585 | Med | 100 | 100 | — | 100 | — | — | 100 |
| Swaim Inc. | 1,317 | Med | 100 | 91 | — | 100 | 6 | — | 100 |

---

## Scraped Vendors — Data Needs Improvement

These vendors are in the database but have data gaps that may affect how well the AI can find and describe their products.

| Vendor | Products | Priority | Issue | Impact |
|--------|----------|----------|-------|--------|
| Surya | 19,761 | High | Product names are inventory codes (e.g. "AAM-008"). Vision enrichment has added descriptions. | Largest vendor by count. AI uses enriched descriptions for search. |
| Bernhardt | 5,871 | High | 14% missing descriptions. 14% missing photos. | One of the biggest furniture vendors — data is thinner than ideal. |
| Fairfield Chair | 6,515 | Med | No dimensions. Material/finish data only ~50%. | Basic search works but product detail could be richer. |
| Regina Andrew | 2,175 | High | 34% missing descriptions and photos. | Lighting vendor — some products won't have images in results. |
| Gabriella White | 2,550 | High | No material or finish data. | AI matches by name/description only — no structured attribute filtering. |
| Maxwell Fabrics | 7,449 | Med | Only 6% have descriptions or images (listing-level data only). | Large catalog but very thin data. Needs re-scrape with detail pages. |
| Rowe Furniture | 1,840 | Med | 26% missing photos. No material/finish data. | Some products won't display images. |
| Tomlinson Companies | 1,770 | Med | Only 16% have descriptions or images. | Products captured at listing level. Needs detail-page scrape. |
| Wesley Hall | 396 | Med | Only 34% have descriptions. No stock info. | AI relies on name matching for most products. |
| Essentials for Living | 561 | Med | Zero product photos. | Products won't display images in search results. |
| Precedent Furniture | 299 | Med | Zero product photos. No material data. | Products searchable by name only, no images. |
| Castelle Furniture | 675 | Med | No descriptions available from site. | Outdoor furniture — AI matches by product name only. |
| Tuuci | 135 | Low | Only 22% have photos. No pricing. | Umbrella/shade vendor — limited display quality. |

---

## Scraped But Incomplete — Need Re-Scraping

These vendors technically have products in the database, but the data is so sparse or the count is so low that they need to be redone.

| Vendor | Products | Priority | Problem |
|--------|----------|----------|---------|
| Artesia Collections | 480 | Med | Broken — virtually no usable data (0% descriptions, images, or pricing) |
| Lexington Home Brands | 26 | High | Major vendor with thousands of products — only captured 26 |
| Sherrill Furniture | 18 | High | Major upholstery vendor — only 18 products captured |
| McKinley Leather | 10 | Med | Only 10 products captured |
| Style Upholstering | 11 | Med | Only 11 products captured |
| Thibaut Design | 4 | High | Major fabric/wallcovering vendor — only 4 products |
| Duralee / Robert Allen | 50 | High | Large fabric house with thousands of products — only 50 captured |
| Christopher Guy | 63 | High | Luxury vendor — likely has hundreds more products |
| Gracie Studio | 50 | Med | Likely has more — needs full catalog scrape |
| Peyton Webster | 57 | Med | Small catalog — may be complete, needs verification |
| LAFVB | 16 | Med | Marketing site only — sells custom window treatments, no product catalog |
| Signature Pillows | 20 | Low | WizCommerce SPA — only best sellers captured |
| Rowley Company | 15 | Low | Workroom supplies — likely has more, low priority |
| Phillips Scott | 1 | Med | Only 1 product captured |
| Horizon Shades | 3 | Med | Only 3 products captured |

---

## Not Yet Scraped — Blocked or Inaccessible

These vendors cannot be scraped with automated tools due to security, login requirements, or site issues.

| Vendor | Priority | Reason |
|--------|----------|--------|
| Vanguard Furniture | High | Requires trade account login to access catalog |
| HVL Group (Hudson Valley) | High | Product data not publicly accessible |
| Fine Art Handcrafted Lighting | High | Cloudflare security blocks automated access |
| Global Views | High | Cloudflare security blocks automated access |
| Leftbank Art | Med | Requires trade account login |
| Bramble Co | Med | Complex website — needs specialized scraper |
| Noir Furniture LA | Med | Cloudflare security blocks automated access |
| Chelsea House | Med | Cloudflare security blocks automated access |
| Southern Home Inc. | Med | Cloudflare security blocks automated access |
| Worlds Away | Med | Cloudflare/WAF blocks automated access |
| Stout Textiles | Med | SSL certificate error — site may be down |
| Woodbridge Furniture | Med | Domain DNS error — site appears to be down |
| A&B Home | Med | Password-protected WooCommerce site |
| Newport Cottages | Low | Password-protected Shopify site |
| AFK Furniture | Low | Cloudflare blocks access |
| LIW Furniture | Low | Site appears to be down |

---

## Summary

| Status | Vendors | Products |
|--------|---------|----------|
| Fully scraped (strong data) | 34 | 107,682 |
| Scraped (data needs improvement) | 13 | 30,239 |
| Incomplete (need re-scraping) | 15 | 843 |
| Blocked / not accessible | 16 | 0 |
| **Total** | **78** | **140,512** |

### Key Highlights

- **77% of products** (107,682) come from vendors with strong, complete data
- **Top 5 vendors by product count:** Surya (19,761), Four Hands (17,911), Wendover Art Group (10,000), Eastern Accents (9,501), Maxwell Fabrics (7,449)
- **Vision enrichment** has been run on 53,347 products using GPT-4o-mini to generate descriptions, colors, materials, and style tags from product images
- **Cohere Rerank** cross-encoder model is now used to improve search result relevance
- **Vendor diversity** is enforced in search results — max 2 products per vendor to ensure variety

### Recommended Next Steps

1. **Re-scrape incomplete vendors** — Lexington, Sherrill, Thibaut, Duralee, and Christopher Guy are high-priority vendors with minimal data
2. **Improve Maxwell Fabrics** — 7,449 products but only listing-level data; need to scrape individual detail pages
3. **Improve Tomlinson** — Same issue as Maxwell; listing data only
4. **Consider headless browser** for Cloudflare-blocked vendors (Fine Art HL, Global Views, Noir, etc.)
5. **Trade account access** — Vanguard and HVL Group require login credentials to scrape
