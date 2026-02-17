# Evenflow Interiors — User Stories

**Version:** 1.0
**Date:** February 16, 2026
**Author:** Onada Development

---

## Story Map Overview

Stories are organized by **epic** and tagged with implementation status:

- **Deployed** — Live in production
- **Partial** — Partially implemented
- **Planned** — Approved for Phase 2
- **Future** — Phase 3 or backlog

---

## Epic 1: Product Search

> *As a designer, I need to find products from our vendor network quickly and accurately so I can source items for my projects without browsing dozens of websites.*

### US-1.1: Natural Language Product Search — `Deployed`

**As a** designer
**I want to** type a natural language description of what I'm looking for
**So that** I can find matching products without knowing which vendor carries what

**Acceptance Criteria:**
- User can type queries like "traditional dark wood king bed" or "modern brass table lamp"
- System returns relevant products from across all active vendors
- Results include product name, vendor name, image, price, and link
- Response time is under 60 seconds including LLM processing
- Empty results display a helpful message suggesting alternative queries

**Technical Notes:**
- Query is sent to Claude Sonnet via tool_use
- Claude calls `search_products` tool with extracted query, categories, and filters
- Search pipeline: vector search (pgvector) → Cohere rerank → vendor diversity filter

---

### US-1.2: Category-Filtered Search — `Deployed`

**As a** designer
**I want to** search within specific product categories
**So that** I can narrow results to the type of product I need

**Acceptance Criteria:**
- Claude automatically infers category from the query (e.g., "king bed" → bedroom)
- Supported categories: bedroom, living room, dining, seating, lighting, rugs, art, decor, outdoor, fabrics, accessories, bathroom
- Category filter is applied at the database level for performance
- User can explicitly specify category (e.g., "show me lighting options")

**Technical Notes:**
- Claude's system prompt lists available categories
- The `search_products` tool accepts a `categories` array parameter
- Database uses GIN index on `categories` TEXT[] for fast array containment queries

---

### US-1.3: In-Stock Filtering — `Deployed`

**As a** designer
**I want to** filter search results to only show in-stock items
**So that** I can find products available for immediate delivery

**Acceptance Criteria:**
- User can say "in stock" or "available now" to trigger the filter
- Claude passes `in_stock: true` to the search tool
- Only products with `in_stock = true` are returned
- Products with unknown stock status (`null`) are excluded when filtering

**Technical Notes:**
- Partial index on `in_stock = TRUE` for fast filtering
- Stock status display: green dot (in stock), red dot (out of stock), gray dot (unknown/contact vendor)

---

### US-1.4: Hybrid Search (Vector + Attributes) — `Deployed`

**As a** designer
**I want to** search by specific colors, materials, or finishes
**So that** I can find products matching my design specifications

**Acceptance Criteria:**
- Queries mentioning colors (e.g., "white", "navy", "walnut") trigger attribute matching
- Queries mentioning materials (e.g., "leather", "marble", "velvet") trigger attribute matching
- Attribute matches are merged with vector search results
- Color/material matches are boosted in the reranking step

**Technical Notes:**
- 40+ color words and 32+ material words detected via set intersection
- Attribute search runs ILIKE queries on `color`, `finish`, `material`, `name`, `description` columns in parallel with vector search
- Results merged with `similarity = 0.55` baseline for attribute matches

---

### US-1.5: Cross-Encoder Reranking — `Deployed`

**As a** designer
**I want to** see the most relevant products at the top of my results
**So that** I spend less time scrolling through irrelevant items

**Acceptance Criteria:**
- Results are reranked using a cross-encoder model after initial retrieval
- Reranking considers the full product description, not just the embedding similarity
- If reranking service is unavailable, results fall back to attribute-based scoring
- Reranking does not add more than 2–3 seconds to response time

**Technical Notes:**
- Cohere `rerank-english-v3.0` cross-encoder
- Over-fetch 5x from vector search, rerank 3x, then diversity filter to final count
- Builds rich document strings: name + vendor + description + color/finish/material/size
- Falls back to attribute-based scoring if Cohere API key is missing or call fails

---

### US-1.6: Vendor Diversity — `Deployed`

**As a** designer
**I want to** see products from a variety of vendors in my results
**So that** I'm not shown 10 products from the same vendor when I want options

**Acceptance Criteria:**
- No more than 2 products per vendor in the default result set
- If fewer than the requested number of results after diversity filtering, backfill from remaining candidates
- Vendor diversity is applied after reranking to preserve relevance within each vendor's slots

**Technical Notes:**
- `_enforce_vendor_diversity(rows, max_results, max_per_vendor=2)` in `search.py`
- First pass: take up to 2 per vendor; second pass: backfill from overflow

---

### US-1.7: Direct API Search — `Deployed`

**As a** developer or power user
**I want to** search products directly without going through the chat interface
**So that** I can build integrations or run bulk queries

**Acceptance Criteria:**
- `GET /api/products/search?q=...&in_stock=bool&limit=int` returns products directly
- No LLM processing — pure vector search + rerank
- Response includes product array, total count, and echoed query
- Supports the same filtering parameters as the chat search

---

## Epic 2: Conversational Experience

> *As a designer, I want to have a natural conversation with the search assistant so I can refine my searches without starting over.*

### US-2.1: Conversational Follow-ups — `Deployed`

**As a** designer
**I want to** refine my search with follow-up messages
**So that** I can narrow or broaden results without repeating my original query

**Acceptance Criteria:**
- "Now show me those in white" works after a furniture search
- "Do you have anything cheaper?" considers the previous results
- "Show me 3 more from Bernhardt" understands the vendor context
- Conversation context is maintained for up to 20 messages
- Context expires after 1 hour of inactivity

**Technical Notes:**
- In-memory conversation store keyed by `conversation_id` (UUID)
- Full message history (including tool calls and results) sent to Claude
- Auto-cleanup of expired conversations on each request

---

### US-2.2: New Conversation — `Deployed`

**As a** designer
**I want to** start a fresh search without previous context
**So that** old queries don't interfere with new searches

**Acceptance Criteria:**
- Omitting `conversation_id` from the request starts a new conversation
- Each new conversation gets a fresh UUID
- The UUID is returned in the response for subsequent follow-ups

---

### US-2.3: Multi-Tool Search — `Deployed`

**As a** designer
**I want to** ask complex queries that require multiple searches
**So that** I can compare different product types in a single response

**Acceptance Criteria:**
- "Show me king beds and matching nightstands" triggers two separate searches
- Claude can call `search_products` multiple times in a single response
- Products from all tool calls are aggregated in the response
- Text response summarizes findings from multiple searches

**Technical Notes:**
- Claude's tool_use loop continues until all tool calls are resolved
- Products are accumulated across tool calls in `chat_with_tools()`

---

## Epic 3: Product Display

> *As a designer, I need to see products presented clearly with all the details I need to make sourcing decisions.*

### US-3.1: Product Cards — `Deployed`

**As a** designer
**I want to** see product results as visual cards with key details
**So that** I can quickly evaluate products without clicking through to vendor sites

**Acceptance Criteria:**
- Each product shows: image, name, vendor, price, stock status, material/finish/size
- Product name links to the vendor's product page (opens in new tab)
- Clicking the product image also opens the vendor page
- Missing images are handled gracefully (hidden, not broken)
- Prices show the vendor's price text, or "Trade pricing" when unavailable
- Attributes (material, finish, size) are dot-separated in a compact line

---

### US-3.2: Vendor-Grouped Layout — `Deployed`

**As a** designer
**I want to** see products grouped by vendor
**So that** I can easily see which vendor offers which products

**Acceptance Criteria:**
- Products are grouped by `vendor_name` in the display
- Each vendor group has a label showing the vendor name
- Products within a vendor group are displayed in a horizontal scrollable row
- Vertical card layout (image on top, details below)
- Card width is fixed at 176px (w-44)

---

### US-3.3: Stock Status Badges — `Deployed`

**As a** designer
**I want to** see stock availability at a glance
**So that** I know which products are available for immediate ordering

**Acceptance Criteria:**
- **Green dot + text** — Product is in stock
- **Red dot + text** — Product is out of stock
- **Gray dot + "Contact vendor"** — Stock status unknown
- Badge displays the vendor's stock text when available (e.g., "Ships in 8–10 weeks")

---

### US-3.4: Products Before Text — `Deployed`

**As a** designer
**I want to** see product cards before the text response
**So that** I can immediately start browsing products instead of reading text first

**Acceptance Criteria:**
- When the response includes products, product cards render above the text bubble
- Text response is concise (1–2 sentences) since the cards convey the details
- Text response does not list products again (no duplication)

---

### US-3.5: Markdown Bot Responses — `Deployed`

**As a** designer
**I want to** see well-formatted bot responses
**So that** information is easy to read and scan

**Acceptance Criteria:**
- Bot messages render Markdown (bold, italic, lists, links)
- Formatting is clean and consistent with the design system
- User messages are plain text (no Markdown rendering)

---

## Epic 4: Chat Interface

> *As a designer, I want a clean, professional chat interface that matches our brand and is easy to use.*

### US-4.1: Chat Input — `Deployed`

**As a** designer
**I want to** type my query and send it easily
**So that** I can search quickly without friction

**Acceptance Criteria:**
- Text input is fixed at the bottom of the screen
- **Enter** sends the message; **Shift+Enter** adds a newline
- Input auto-resizes vertically as I type (up to 120px max)
- Input is disabled (grayed out) while the bot is processing
- Gold "Send" button appears when there is text to send
- Send button is disabled when input is empty or while loading

---

### US-4.2: Typing Indicator — `Deployed`

**As a** designer
**I want to** see a visual indicator while the bot is thinking
**So that** I know my query is being processed

**Acceptance Criteria:**
- Three animated gold bouncing dots appear in the message area during processing
- Indicator disappears when the response arrives
- Chat auto-scrolls to keep the indicator visible

---

### US-4.3: Error Handling — `Deployed`

**As a** designer
**I want to** see clear error messages if something goes wrong
**So that** I know to try again or report the issue

**Acceptance Criteria:**
- Network errors show a red inline alert in the message area
- Error message is human-readable (not a raw stack trace)
- User can send a new message after an error (input is re-enabled)

---

### US-4.4: Welcome State — `Deployed`

**As a** designer
**I want to** see a welcome message when I first open the chatbot
**So that** I understand what I can do and how to use it

**Acceptance Criteria:**
- Empty chat shows a branded welcome message
- Welcome explains the chatbot's capabilities and vendor coverage
- Suggests example queries the user can try

---

### US-4.5: Responsive Layout — `Deployed`

**As a** designer
**I want to** use the chatbot on my desktop or tablet
**So that** I can search for products from any device

**Acceptance Criteria:**
- Chat column is max-width 768px, centered on large screens
- Tables and product rows scroll horizontally on narrow screens
- Header and input bar adapt to mobile widths
- Touch-friendly on tablets

---

### US-4.6: Branding — `Deployed`

**As a** designer
**I want to** see a professional, branded interface
**So that** the tool feels like a part of the Evenflow ecosystem

**Acceptance Criteria:**
- "Evenflow" wordmark in Great Vibes script font
- "INTERIORS" in tracked uppercase
- "Product Search Assistant" subtitle
- Gold accent color (#b8963e) on buttons and highlights
- Navy (#1c2741) user message bubbles
- Cream/warm white background
- Playfair Display for headings, Inter for body text

---

## Epic 5: Data Pipeline — Scraping

> *As the system, I need to scrape vendor websites regularly to keep the product database current and comprehensive.*

### US-5.1: Generic Scraper — `Deployed`

**As a** system administrator
**I want to** scrape products from any standard e-commerce site using a generic strategy
**So that** new vendors can be added without custom code

**Acceptance Criteria:**
- Discovers product URLs via sitemap.xml crawling
- Extracts product data using JSON-LD, then OG tags, then CSS patterns
- Rate-limited to 1 request/second per domain
- Uses descriptive User-Agent string
- Handles pagination and sub-sitemaps

---

### US-5.2: Custom Scrapers — `Deployed`

**As a** system administrator
**I want to** have specialized scrapers for vendors with non-standard platforms
**So that** I can maximize product coverage across all vendors

**Acceptance Criteria:**
- Shopify scraper handles 11 vendors via `/products.json` API
- Magento 2 GraphQL scraper handles 9 vendors
- Custom API scrapers for Surya, Currey & Company, SuiteCommerce, WooCommerce
- HTML scrapers for vendors without APIs (Maitland-Smith, Artistica, Tomlinson, etc.)
- Each scraper produces standardized product data matching the `products` schema

**Current Coverage:**
| Scraper Type | Vendors | Products |
|---|---|---|
| Shopify | 11 | ~25,000 |
| Magento GraphQL | 9 | ~30,000 |
| Custom APIs | 6 | ~35,000 |
| SuiteCommerce | 2 | ~3,500 |
| WooCommerce | 2 | ~3,800 |
| HTML Parsing | 5 | ~15,000 |
| Generic Sitemap | ~10 | ~27,000 |

---

### US-5.3: Embedding Generation — `Deployed`

**As a** system
**I want to** generate vector embeddings for every product
**So that** semantic search can find products by meaning, not just keywords

**Acceptance Criteria:**
- Every product gets a 1536-dimension embedding via OpenAI `text-embedding-3-small`
- Embedding is generated from the `search_text` field (concatenation of name, vendor, description, categories, style tags, material, color, finish, size)
- Embeddings are generated during scraping and stored in the `embedding` column
- Products without embeddings are excluded from vector search results

---

### US-5.4: Vision Enrichment — `Partial`

**As a** system administrator
**I want to** enrich products with poor descriptions using AI image analysis
**So that** products with only SKU-style names or empty descriptions become searchable

**Acceptance Criteria:**
- Products with missing/short/SKU-like descriptions are identified for enrichment
- GPT-4o-mini vision analyzes the product image and generates: description, color, material, finish, style_tags
- Enriched products have their `search_text` rebuilt and embeddings regenerated
- Progress is tracked via `vision_enriched` column ('true', 'failed', null)
- Pipeline is resume-safe — can be stopped and restarted without duplicating work

**Current Status:**
- 53,347 products enriched (38%)
- ~87,000 products remaining (many already have good descriptions)

---

### US-5.5: Manual Scrape Trigger — `Deployed`

**As a** system administrator
**I want to** manually trigger scraping for specific vendors
**So that** I can refresh data on-demand or add new vendors

**Acceptance Criteria:**
- Individual scrape scripts exist per vendor in `backend/scripts/`
- Each script can be run independently from the command line
- Script updates `last_scraped_at` and `scrape_success` on the vendor record
- Failed scrapes don't corrupt existing data (new products are inserted, not upserted destructively)

---

### US-5.6: Automated Scrape Scheduling — `Planned`

**As a** system administrator
**I want to** schedule automatic re-scraping on a weekly cadence
**So that** the product database stays current without manual intervention

**Acceptance Criteria:**
- High-priority vendors are re-scraped weekly
- Medium-priority vendors are re-scraped bi-weekly
- Low-priority vendors are re-scraped monthly
- Scrape failures are logged and reported
- Scrape runs don't overlap or conflict

---

## Epic 6: Vendor Management

> *As a system administrator, I need to manage the vendor network — adding, removing, and monitoring vendor data.*

### US-6.1: Vendor Registry — `Deployed`

**As a** system administrator
**I want to** maintain a registry of all approved vendors
**So that** I know which vendors to scrape and their current status

**Acceptance Criteria:**
- `vendors.json` file contains all vendor entries with name, URL, categories, priority, and status
- Vendors can be seeded into the database via `seed_vendors.py`
- Vendor list is accessible via `GET /api/vendors`
- Each vendor record tracks `last_scraped_at` and `scrape_success`

---

### US-6.2: Vendor Deletion — `Deployed`

**As a** system administrator
**I want to** remove vendors from the system
**So that** discontinued or unwanted vendors don't appear in search results

**Acceptance Criteria:**
- Deleting a vendor cascades to delete all its products
- Vendor is removed from `vendors.json` registry
- Deleted vendor's products no longer appear in search results

---

### US-6.3: Vendor Scrape Report — `Deployed`

**As a** company owner
**I want to** see a detailed report of vendor scrape status and data quality
**So that** I can track progress and identify vendors that need attention

**Acceptance Criteria:**
- Report shows all vendors with product counts and data quality percentages
- Data quality columns: Desc%, Img%, Dims%, Price%, Mat%, Fin%, Stock%
- Vendors are grouped by status: fully scraped, needs improvement, incomplete, newly accessible, blocked
- Report includes progress comparison from original to current state
- Report is published as a GitHub Pages site for easy sharing

---

### US-6.4: Category Rankings — `Partial`

**As a** company owner
**I want to** rank vendors by importance within each product category
**So that** search results can be weighted toward preferred vendors

**Acceptance Criteria:**
- 12 consolidated categories are defined (Furniture, Upholstery, Lighting, Rugs, Art, Decor, Fabrics, Outdoor, Bedding, Wallcoverings, Bathroom, Window Treatments)
- Each category shows all relevant vendors with their product counts
- A Rank column allows manual ranking input (1 = most important)
- Vendors can appear in multiple categories
- Rankings will be used to weight search result ordering (Phase 2)

**Current Status:** Category tables published, awaiting Lisa's ranking input.

---

## Epic 7: Authentication & Access Control — `Future`

> *As a business owner, I want to control who can access the chatbot and vendor data.*

### US-7.1: User Login — `Future`

**As a** designer
**I want to** log in to the chatbot
**So that** my conversations and preferences are saved

**Acceptance Criteria:**
- Email + password authentication
- Session persists across browser refreshes
- Logout clears session

---

### US-7.2: Role-Based Access — `Future`

**As a** company owner
**I want to** assign roles (designer, admin) to users
**So that** I can control who can manage vendors vs. who can only search

**Acceptance Criteria:**
- **Designer role:** Can search products, view conversations
- **Admin role:** Can manage vendors, trigger scrapes, view reports
- Admin can create/deactivate user accounts

---

## Epic 8: Saved Searches & Collections — `Future`

> *As a designer, I want to save and organize products I've found for my projects.*

### US-8.1: Save Products to Collection — `Future`

**As a** designer
**I want to** save individual products to a named collection
**So that** I can build product boards for my design projects

**Acceptance Criteria:**
- "Save" button on each product card
- Products can be saved to named collections (e.g., "Smith Living Room Project")
- Collections are persisted per user
- Saved products include a snapshot of the product data at save time

---

### US-8.2: View & Share Collections — `Future`

**As a** designer
**I want to** view my saved collections and share them with colleagues
**So that** I can present options to clients or collaborate with my team

**Acceptance Criteria:**
- Collection view shows all saved products in a grid
- Collections can be shared via URL (read-only for recipients)
- Shared collections include product images, names, vendors, and links

---

## Epic 9: Analytics & Insights — `Future`

> *As a business owner, I want to understand how the tool is being used to make data-driven decisions.*

### US-9.1: Search Analytics — `Future`

**As a** company owner
**I want to** see what products designers are searching for
**So that** I can identify demand patterns and vendor gaps

**Acceptance Criteria:**
- Dashboard showing: top search queries, most-viewed vendors, search volume over time
- Identify queries with zero results (potential vendor gaps)
- Track which vendors' products are clicked most often

---

### US-9.2: Vendor Usage Report — `Future`

**As a** company owner
**I want to** see which vendors are most/least used in search results
**So that** I can evaluate vendor relationships and data quality ROI

**Acceptance Criteria:**
- Report showing vendor appearance frequency in search results
- Correlation between data quality and search result inclusion
- Recommendations for vendors to prioritize for data improvement

---

## Epic 10: Performance & Reliability — `Planned`

> *As a user, I expect the system to be fast, reliable, and handle errors gracefully.*

### US-10.1: Response Streaming — `Planned`

**As a** designer
**I want to** see the bot's response appear word-by-word as it's generated
**So that** I get faster perceived response times

**Acceptance Criteria:**
- Chat responses stream in real-time (Server-Sent Events or WebSocket)
- Products appear as soon as the tool call returns (before text is complete)
- Graceful fallback to batch response if streaming fails

---

### US-10.2: Persistent Conversations — `Planned`

**As a** designer
**I want to** resume a conversation after closing the browser
**So that** I don't lose my search context

**Acceptance Criteria:**
- Conversations are stored server-side (database, not in-memory)
- Conversations survive server restarts and redeployments
- Conversation history is loaded when a user returns with their conversation ID

---

### US-10.3: Health Monitoring — `Partial`

**As a** system administrator
**I want to** monitor the system's health
**So that** I can detect and respond to issues quickly

**Acceptance Criteria:**
- `GET /health` endpoint returns system status
- Monitors: API availability, database connectivity, LLM API status
- Alerts on: scrape failures, API errors, high latency

**Current Status:** Basic `/health` endpoint deployed. Advanced monitoring planned.

---

## Story Status Summary

| Status | Count | Description |
|--------|-------|-------------|
| **Deployed** | 23 | Live in production |
| **Partial** | 3 | Partially implemented |
| **Planned** | 3 | Approved for Phase 2 |
| **Future** | 6 | Phase 3 or backlog |
| **Total** | **35** | |
