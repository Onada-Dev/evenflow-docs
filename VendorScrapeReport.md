# Evenflow Interiors — Vendor Scrape Report

**Date:** February 16, 2026
**Total Products in Database:** 139,405
**Vendors with Products:** 58 of 68 registered
**AI Search Status:** Live and searchable at the Evenflow Product Search chatbot

---

## How to Read This Report

| Column | What It Means |
|--------|---------------|
| **Products** | Total number of products pulled from this vendor |
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

| Vendor | Products | Desc% | Img% | Dims% | Price% | Mat% | Fin% | Stock% |
|--------|----------|-------|------|-------|--------|------|------|--------|
| Four Hands | 17,911 | 100 | 100 | 100 | 100 | 100 | 100 | 88 |
| Jonathan Charles | 429 | 100 | 100 | 100 | 100 | 100 | 88 | 100 |
| Caracole | 1,125 | 100 | 100 | 100 | 100 | 57 | 88 | 100 |
| Currey & Company | 3,102 | 100 | 100 | — | 100 | 100 | 100 | — |
| Theodore Alexander | 2,440 | 100 | 100 | — | 100 | 100 | 86 | 59 |
| ELK Home | 2,432 | 100 | 100 | — | 100 | 100 | 71 | 100 |
| Cyan Design | 2,303 | 100 | 100 | — | 100 | 100 | 82 | 100 |
| Renwil | 1,559 | 100 | 100 | — | 100 | 99 | 62 | 100 |
| Uttermost | 4,719 | 100 | 100 | — | 100 | 86 | 10 | 100 |
| Old Biscayne Designs | 3,323 | 100 | 100 | 72 | 100 | 89 | 90 | — |
| Loloi Rugs | 5,089 | 98 | 98 | — | 100 | 5 | — | 100 |
| Eastern Accents | 9,501 | 100 | 87 | 12 | 100 | 32 | — | 100 |
| Jaipur Living | 2,740 | 100 | 100 | — | 100 | — | — | 100 |
| Wendover Art Group | 10,000 | 100 | 100 | — | 100 | 72 | 56 | 100 |
| Shadow Catchers Art | 4,193 | 100 | 100 | — | 100 | 14 | 12 | 100 |
| Paragon PG | 4,038 | 100 | 100 | — | 100 | — | — | 100 |
| Propac Images | 1,292 | 100 | 100 | — | 100 | — | — | 100 |
| Made Goods | 1,484 | 100 | 100 | — | 100 | — | — | 100 |
| Savoy House | 2,099 | 100 | 100 | — | 100 | — | — | 100 |
| Jamie Young Co | 870 | 100 | 100 | — | 100 | — | — | 100 |
| Maitland-Smith | 768 | 100 | 99 | 100 | 100 | 6 | 5 | 90 |
| Ambella Home | 708 | 100 | 100 | 100 | 100 | — | — | 100 |
| Artistica Home | 612 | 100 | 100 | 100 | 100 | 22 | 20 | 72 |
| Universal Furniture | 1,097 | 100 | 100 | 99 | 100 | — | — | — |
| Summer Classics | 1,027 | 98 | 96 | — | 100 | 84 | 20 | 100 |
| Interlude Home | 1,076 | 77 | 100 | — | 100 | — | — | 100 |
| Eloquence | 398 | 100 | 100 | — | 100 | — | — | 100 |
| Padma's Plantation | 213 | 100 | 100 | — | 100 | — | — | 100 |
| Selamat Designs | 235 | 98 | 98 | — | 100 | — | — | 100 |
| James Martin Vanities | 498 | 100 | 100 | — | 100 | 6 | 6 | 100 |
| Hekman | 419 | 100 | 100 | — | 100 | — | — | 100 |
| Greenhouse Fabrics | 89 | 100 | 100 | — | — | 100 | — | — |
| Sarreid Ltd | 585 | 100 | 100 | — | 100 | — | — | 100 |
| Swaim Inc. | 1,317 | 100 | 91 | — | 100 | 6 | — | 100 |

> **Dimensions gap:** Lisa reports that these vendors should have dimensions available on their sites, but we are not currently capturing them: **Loloi Rugs, Eastern Accents, Jaipur Living, Jamie Young Co, Savoy House, Made Goods, Uttermost, Padma's Plantation, Eloquence, Selamat Designs, James Martin Vanities, Hekman, Cyan Design, Shadow Catchers Art**. This is a scraper improvement item.

---

## Scraped Vendors — Data Needs Improvement

These vendors are in the database but have data gaps that may affect how well the AI can find and describe their products.

| Vendor | Products | Issue | Impact |
|--------|----------|-------|--------|
| Surya | 19,761 | Product names are inventory codes (e.g. "AAM-008"). Vision enrichment has added descriptions. | Largest vendor by count. AI uses enriched descriptions for search. |
| Bernhardt | 5,871 | 14% missing descriptions. 14% missing photos. | One of the biggest furniture vendors — data is thinner than ideal. |
| Fairfield Chair | 6,515 | No dimensions. Material/finish data only ~50%. | Basic search works but product detail could be richer. |
| Regina Andrew | 2,175 | 34% missing descriptions and photos. | Lighting vendor — some products won't have images in results. |
| Gabriella White | 2,550 | No material or finish data. | AI matches by name/description only — no structured attribute filtering. |
| Maxwell Fabrics | 7,449 | Only 6% have descriptions or images (listing-level data only). | Large catalog but very thin data. Needs re-scrape with detail pages. |
| Rowe Furniture | 1,840 | 26% missing photos. No material/finish data. | Some products won't display images. |
| Tomlinson Companies | 1,770 | Only 16% have descriptions or images. | Products captured at listing level. Needs detail-page scrape. |
| Essentials for Living | 561 | Zero product photos. | Products won't display images in search results. |
| Precedent Furniture | 299 | Zero product photos. No material data. | Products searchable by name only, no images. |
| Tuuci | 135 | Only 22% have photos. No pricing. | Umbrella/shade vendor — limited display quality. |

---

## Scraped But Incomplete — Need Re-Scraping

These vendors technically have products in the database, but the data is so sparse or the count is so low that they need to be redone.

| Vendor | Products | Problem |
|--------|----------|---------|
| Artesia Collections | 480 | Broken — virtually no usable data (0% descriptions, images, or pricing) |
| Lexington Home Brands | 26 | Major vendor with thousands of products — only captured 26 |
| Sherrill Furniture | 18 | Major upholstery vendor — only 18 category pages captured, not individual products |
| McKinley Leather | 10 | Only 10 products captured |
| Style Upholstering | 11 | Only 11 products captured |
| Thibaut Design | 4 | Major fabric/wallcovering vendor — only 4 products |
| Duralee / Robert Allen | 50 | Large fabric house with thousands of products — only 50 captured |
| Christopher Guy | 63 | Luxury vendor — likely has hundreds more products |
| Gracie Studio | 50 | Likely has more — needs full catalog scrape |
| Peyton Webster | 57 | Small catalog — may be complete, needs verification |
| Rowley Company | 15 | Workroom supplies — likely has more, low priority |
| Phillips Scott | 1 | Only 1 product captured |
| Horizon Shades | 3 | Only 3 products captured |

---

## Needs Scraping — Newly Confirmed Accessible

Lisa has confirmed these vendors are accessible. We previously marked them as blocked but they need to be scraped.

**Lisa — please send us the correct website URLs for these vendors so we can begin scraping them.**

| Vendor | Previous Status | Notes |
|--------|----------------|-------|
| HVL Group (Hudson Valley) | Was marked "login-gated" | Lisa confirms totally accessible |
| A&B Home | Was marked "password-protected" | Lisa confirms website is fine |
| Chelsea House | Was marked "Cloudflare blocked" | Lisa confirms fine |
| Noir Furniture LA | Was marked "Cloudflare blocked" | Lisa confirms fine |
| Southern Home Inc. | Was marked "Cloudflare blocked" | Lisa confirms fine |
| Woodbridge Furniture | Was marked "DNS error" | Lisa confirms fine |
| Worlds Away | Was marked "Cloudflare blocked" | Lisa confirms fine |

---

## Not Scrapeable — Still Blocked

These vendors still cannot be scraped with automated tools.

| Vendor | Reason |
|--------|--------|
| Vanguard Furniture | Requires trade account login to access catalog |
| Global Views | Cloudflare security blocks automated access |
| Bramble Co | Complex website — needs specialized scraper |

---

## Vendor Rankings by Category

**Lisa — please fill in the Rank column (1 = most important) for each vendor within each category. This will help us weight search results so the most important vendors appear first.**

A vendor can appear in multiple categories. That's expected.

### Furniture

| Rank | Vendor | Products |
|------|--------|----------|
| | Four Hands | 17,911 |
| | Bernhardt | 5,871 |
| | Uttermost | 4,719 |
| | Old Biscayne Designs | 3,323 |
| | Gabriella White | 2,550 |
| | Theodore Alexander | 2,440 |
| | ELK Home | 2,432 |
| | Tomlinson Companies | 1,770 |
| | Made Goods | 1,484 |
| | Swaim Inc. | 1,317 |
| | Caracole | 1,125 |
| | Universal Furniture | 1,097 |
| | Interlude Home | 1,076 |
| | Maitland-Smith | 768 |
| | Ambella Home | 708 |
| | Artistica Home | 612 |
| | Sarreid Ltd | 585 |
| | Essentials for Living | 561 |
| | Artesia Collections | 480 |
| | Jonathan Charles | 429 |
| | Eloquence | 398 |
| | Selamat Designs | 235 |
| | Padma's Plantation | 213 |
| | Christopher Guy | 63 |
| | Lexington Home Brands | 26 |
| | Phillips Scott | 1 |
| | Vanguard Furniture | 0 |
| | A&B Home | 0 |
| | Bramble Co | 0 |
| | Noir Furniture LA | 0 |
| | Southern Home Inc. | 0 |
| | Woodbridge Furniture | 0 |
| | Worlds Away | 0 |

### Upholstery / Seating

| Rank | Vendor | Products |
|------|--------|----------|
| | Fairfield Chair | 6,515 |
| | Bernhardt | 5,871 |
| | Gabriella White | 2,550 |
| | Theodore Alexander | 2,440 |
| | Rowe Furniture | 1,840 |
| | Swaim Inc. | 1,317 |
| | Caracole | 1,125 |
| | Precedent Furniture | 299 |
| | Christopher Guy | 63 |
| | Sherrill Furniture | 18 |
| | Style Upholstering | 11 |
| | McKinley Leather | 10 |
| | Vanguard Furniture | 0 |

### Lighting

| Rank | Vendor | Products |
|------|--------|----------|
| | Uttermost | 4,719 |
| | Currey & Company | 3,102 |
| | ELK Home | 2,432 |
| | Cyan Design | 2,303 |
| | Regina Andrew | 2,175 |
| | Savoy House | 2,099 |
| | Renwil | 1,559 |
| | Made Goods | 1,484 |
| | Jamie Young Co | 870 |
| | Chelsea House | 0 |
| | HVL Group (Hudson Valley) | 0 |
| | Worlds Away | 0 |

### Rugs

| Rank | Vendor | Products |
|------|--------|----------|
| | Surya | 19,761 |
| | Loloi Rugs | 5,089 |
| | Jaipur Living | 2,740 |
| | Peyton Webster | 57 |

### Art / Wall Decor

| Rank | Vendor | Products |
|------|--------|----------|
| | Wendover Art Group | 10,000 |
| | Uttermost | 4,719 |
| | Shadow Catchers Art | 4,193 |
| | Paragon PG | 4,038 |
| | Renwil | 1,559 |
| | Propac Images | 1,292 |
| | Christopher Guy | 63 |

### Decor / Accessories

| Rank | Vendor | Products |
|------|--------|----------|
| | Surya | 19,761 |
| | Uttermost | 4,719 |
| | Currey & Company | 3,102 |
| | Cyan Design | 2,303 |
| | Renwil | 1,559 |
| | Made Goods | 1,484 |
| | Interlude Home | 1,076 |
| | Jamie Young Co | 870 |
| | Sarreid Ltd | 585 |
| | A&B Home | 0 |
| | Chelsea House | 0 |
| | Global Views | 0 |

### Fabrics / Textiles

| Rank | Vendor | Products |
|------|--------|----------|
| | Eastern Accents | 9,501 |
| | Maxwell Fabrics | 7,449 |
| | Greenhouse Fabrics | 89 |
| | Duralee / Robert Allen | 50 |
| | Thibaut Design | 4 |

### Outdoor

| Rank | Vendor | Products |
|------|--------|----------|
| | Gabriella White | 2,550 |
| | Summer Classics | 1,027 |
| | Tuuci | 135 |
| | Lexington Home Brands | 26 |

### Bedding / Pillows

| Rank | Vendor | Products |
|------|--------|----------|
| | Surya | 19,761 |
| | Eastern Accents | 9,501 |

### Wallcoverings

| Rank | Vendor | Products |
|------|--------|----------|
| | Gracie Studio | 50 |
| | Thibaut Design | 4 |

### Bathroom

| Rank | Vendor | Products |
|------|--------|----------|
| | Ambella Home | 708 |
| | James Martin Vanities | 498 |

### Window Treatments

| Rank | Vendor | Products |
|------|--------|----------|
| | Horizon Shades | 3 |

---

## Progress Since Original Report

Comparing the current state to the original vendor scrape report.

### Vendors That Improved Significantly

| Vendor | Previous Status | Previous Products | Current Status | Current Products |
|--------|----------------|-------------------|---------------|-----------------|
| Theodore Alexander | Incomplete | 7 | Fully Scraped | **2,440** |
| Ambella Home | Not Yet Scraped | 0 | Fully Scraped | **708** |
| Paragon PG | Not Yet Scraped | 0 | Fully Scraped | **4,038** |
| Sarreid Ltd | Not Yet Scraped | 0 | Fully Scraped | **585** |
| Propac Images | Not Scrapeable | 0 | Fully Scraped | **1,292** |
| Interlude Home | Not Scrapeable | 0 | Scraped | **1,076** |
| Tomlinson Companies | Not Yet Scraped | 0 | Scraped (thin) | **1,770** |
| Maxwell Fabrics | Not Yet Scraped | 0 | Scraped (thin) | **7,449** |
| Precedent Furniture | Not Yet Scraped | 0 | Scraped | **299** |
| Peyton Webster | Not Yet Scraped | 0 | Scraped | **57** |
| Gracie Studio | Not Yet Scraped | 0 | Incomplete | **50** |

### New Vendors Added (not in original report)

| Vendor | Products | Category |
|--------|----------|----------|
| ELK Home | 2,432 | Lighting, furniture, decor |
| Renwil | 1,559 | Decor, mirrors, lighting |
| Swaim Inc. | 1,317 | Upholstery, custom furniture |
| Summer Classics | 1,027 | Outdoor furniture |
| Tuuci | 135 | Outdoor umbrellas, shade |
| Greenhouse Fabrics | 89 | Fabrics, textiles |
| Rowley Company | 15 | Workroom supplies |
| Horizon Shades | 3 | Window treatments |

### Data Quality Improvements

| Vendor | What Changed |
|--------|-------------|
| Surya (19,761 products) | Previously "no descriptions" — now 100% have AI-generated descriptions via vision enrichment |
| Wendover Art Group (10,000) | Previously "no descriptions" — now 100% have descriptions |
| Cyan Design (2,303) | Description coverage: 79% → 100%. Image coverage: 83% → 100% |
| Four Hands (17,911) | Description coverage: 97% → 100% |
| Gabriella White (2,550) | Previously "85% missing descriptions" — now 98% have descriptions |

### By the Numbers

| Metric | Original Report | Current Report | Change |
|--------|----------------|----------------|--------|
| Fully scraped vendors | 22 | 34 | **+12** |
| "Not yet scraped" vendors | 8 | 0 | **All done** |
| "Not scrapeable" vendors that were scraped | 0 | 3 | Propac, Interlude, Signature Pillows |
| New vendors added | — | 8 | ELK Home, Renwil, Swaim, Summer Classics, etc. |
| Products with AI-enriched descriptions | 0 | 53,347 | Via GPT-4o-mini vision pipeline |
| Vendors deleted (no longer needed) | 0 | 10 | Per Lisa's direction |
| Total vendors | 78 | 68 | -10 deleted |
| Total products | 140,512 | 139,405 | -1,107 (deleted vendor products) |

### What's Still Stuck

- **Incomplete high-priority vendors** — Lexington (26), Sherrill (18), Thibaut (4), Duralee (50), Christopher Guy (63) still have minimal data
- **Cloudflare-blocked** — Global Views still returns 0 products
- **Login-gated** — Vanguard Furniture still requires trade credentials
- **Complex site** — Bramble Co needs specialized scraper
- **Newly accessible vendors** — 7 vendors Lisa confirmed are accessible need scraping (HVL Group, A&B Home, Chelsea House, Noir, Southern Home, Woodbridge, Worlds Away)

---

## Summary

| Status | Vendors | Products |
|--------|---------|----------|
| Fully scraped (strong data) | 34 | 106,611 |
| Scraped (data needs improvement) | 11 | 29,493 |
| Incomplete (need re-scraping) | 13 | 769 |
| Newly accessible (need scraping) | 7 | 0 |
| Blocked / not accessible | 3 | 0 |
| **Total** | **68** | **139,405** |

### Key Highlights

- **77% of products** (106,611) come from vendors with strong, complete data
- **Top 5 vendors by product count:** Surya (19,761), Four Hands (17,911), Wendover Art Group (10,000), Eastern Accents (9,501), Maxwell Fabrics (7,449)
- **Vision enrichment** has been run on 53,347 products using GPT-4o-mini to generate descriptions, colors, materials, and style tags from product images
- **Cohere Rerank** cross-encoder model is now used to improve search result relevance
- **Vendor diversity** is enforced in search results — max 2 products per vendor to ensure variety
- **10 vendors deleted** per Lisa's direction — no longer needed for Evenflow's vendor network

### Recommended Next Steps

1. **Scrape newly accessible vendors** — HVL Group, A&B Home, Chelsea House, Noir, Southern Home, Woodbridge, Worlds Away (Lisa to provide URLs)
2. **Re-scrape incomplete vendors** — Lexington, Sherrill, Thibaut, Duralee, and Christopher Guy are high-priority vendors with minimal data
3. **Capture dimensions** — 14 vendors have dimensions available that we're not currently scraping
4. **Improve Maxwell Fabrics** — 7,449 products but only listing-level data; need to scrape individual detail pages
5. **Improve Tomlinson** — Same issue as Maxwell; listing data only
6. **Consider headless browser** for Global Views (still Cloudflare-blocked)
7. **Trade account access** — Vanguard Furniture requires login credentials to scrape

---

## Action Items for Lisa

1. **Fill in the Rank column** in the "Vendor Rankings by Category" section above (1 = most important vendor in that category)
2. **Send us the website URLs** for these vendors you confirmed are accessible: HVL Group, A&B Home, Chelsea House, Noir Furniture LA, Southern Home Inc., Woodbridge Furniture, Worlds Away
