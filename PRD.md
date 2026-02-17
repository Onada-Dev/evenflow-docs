# Evenflow Interiors — Product Requirements Document

**Version:** 1.0
**Date:** February 16, 2026
**Author:** Onada Development
**Status:** In Development (MVP Deployed)

---

## 1. Executive Summary

Evenflow Interiors is an interior design and construction company that works with 68+ pre-approved trade vendors across furniture, lighting, rugs, fabrics, art, and decor. Their designers currently search for products by manually browsing dozens of vendor websites — a time-consuming process that fragments their workflow.

**Evenflow Product Search** is an AI-powered chatbot that consolidates all vendor catalogs into a single conversational search interface. Designers describe what they need in plain English, and the system returns matching products from across the entire vendor network with images, pricing, stock status, and direct links to vendor pages.

### Key Value Proposition

- **One search, 60+ vendors** — Eliminates the need to browse vendor sites individually
- **Natural language queries** — "Find me 5 traditional dark wood king beds in stock" instead of clicking through category trees
- **Conversational refinement** — Follow-up with "now show me those in white leather" to narrow results
- **Trade-aware** — Handles trade pricing, vendor-specific availability, and industry terminology

---

## 2. Problem Statement

### Current State

Interior designers at Evenflow spend significant time sourcing products:

1. They maintain relationships with 68+ trade vendors, each with their own website
2. Finding the right product requires browsing multiple vendor sites, navigating inconsistent category structures, and mentally comparing options across tabs
3. There's no unified way to search "dark wood king bed" across all vendors simultaneously
4. Designers rely on memory and bookmarks to know which vendors carry which product types
5. Trade vendor sites often lack modern search — no semantic search, poor filtering, and sometimes no search at all

### Desired State

A single chat interface where a designer types a natural language description of what they need and instantly sees relevant products from across all approved vendors — complete with images, pricing (where available), stock status, and direct links to the vendor page for ordering.

---

## 3. Goals & Success Metrics

### Primary Goals

| Goal | Metric | Target |
|------|--------|--------|
| Reduce product sourcing time | Time to find 5 relevant options | < 30 seconds (vs. 15–30 min manual) |
| Comprehensive vendor coverage | Vendors with searchable products | 60+ vendors, 100K+ products |
| Search relevance | Users find what they're looking for | 80%+ of queries return relevant results |
| Adoption | Active usage by design team | Daily use by Evenflow designers |

### Secondary Goals

| Goal | Metric | Target |
|------|--------|--------|
| Data freshness | Products updated regularly | Weekly re-scrapes of high-priority vendors |
| Data quality | Products with descriptions + images | 90%+ of products have both |
| Conversation depth | Multi-turn refinement works | Follow-up queries narrow results correctly |

---

## 4. User Personas

### Primary: Interior Designer (Lisa, Sarah, etc.)

- **Role:** Interior designer at Evenflow Interiors
- **Context:** Designing residential and commercial spaces; needs to source furniture, lighting, rugs, fabrics, and decor from approved trade vendors
- **Pain points:**
  - Browses 10+ vendor websites per project
  - Spends hours finding the right piece in the right size, finish, and price range
  - Loses track of which vendors carry which categories
  - Trade sites are clunky and hard to search
- **Needs:**
  - Fast product discovery across all vendors
  - Visual results (photos are essential)
  - Direct links to vendor pages for ordering
  - Stock/availability information
  - Ability to refine searches conversationally

### Secondary: Company Owner / Manager (Bryan)

- **Role:** Business owner managing the vendor network and technology
- **Context:** Maintains vendor relationships, adds/removes vendors, monitors system health
- **Needs:**
  - Vendor scrape status reporting
  - Ability to add/remove vendors from the system
  - Data quality visibility
  - Category ranking to weight search results

---

## 5. Product Scope

### In Scope (MVP — Current Release)

| Feature | Description | Status |
|---------|-------------|--------|
| AI Chat Interface | Conversational product search using Claude | Deployed |
| Natural Language Search | Vector similarity + keyword hybrid search | Deployed |
| Multi-Vendor Product Database | 139,405 products from 58 active vendors | Deployed |
| Product Cards | Visual results with image, price, stock, attributes | Deployed |
| Vendor Grouping | Results grouped by vendor in horizontal scroll rows | Deployed |
| Conversation Context | Multi-turn follow-up queries (20 messages, 1hr TTL) | Deployed |
| Cohere Reranking | Cross-encoder reranking for better relevance | Deployed |
| Vendor Diversity | Max 2 products per vendor to prevent dominance | Deployed |
| Stock Status Display | Green/red/gray indicator per product | Deployed |
| Vision Enrichment | GPT-4o-mini image analysis for sparse descriptions | 38% complete |
| Vendor Scrape Report | GitHub Pages dashboard for Lisa | Deployed |

### Out of Scope (Future Phases)

| Feature | Description | Phase |
|---------|-------------|-------|
| User Authentication | Login, roles, permissions | Phase 2 |
| Saved Searches / Collections | Save and share product boards | Phase 2 |
| Automated Scrape Scheduling | Cron-based re-scraping | Phase 2 |
| Streaming Responses | Real-time token streaming for chat | Phase 2 |
| Persistent Conversations | Server-side conversation storage | Phase 2 |
| Vendor Ranking Weights | Priority-weighted search results | Phase 2 |
| Price Comparison | Compare pricing across vendors | Phase 3 |
| Order Integration | Place orders directly from chat | Phase 3 |
| Mobile App | Native mobile experience | Phase 3 |
| Analytics Dashboard | Search analytics, popular queries, vendor usage | Phase 3 |

---

## 6. System Architecture

### High-Level Architecture

```
┌─────────────────┐     ┌───────────────────┐     ┌───────────────────┐
│   React Chat    │────▶│   FastAPI Backend  │────▶│   Claude Sonnet   │
│   Frontend      │◀────│   (Railway)        │◀────│   (Tool Use)      │
│   (SPA)         │     └────────┬──────────┘     └────────┬──────────┘
└─────────────────┘              │                          │
                          ┌──────▼──────────┐       ┌──────▼──────────┐
                          │   Supabase      │       │   search_       │
                          │   Postgres +    │       │   products()    │
                          │   pgvector      │       │   Tool Call     │
                          └──────┬──────────┘       └─────────────────┘
                                 │
                          ┌──────▼──────────┐
                          │   Scraper       │
                          │   Pipeline      │
                          │   (Manual)      │
                          └─────────────────┘
```

### Component Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| Frontend | React 18 + TypeScript + Vite + Tailwind CSS | Chat UI with product cards |
| Backend API | Python 3.12 + FastAPI | REST API, search orchestration |
| LLM | Claude Sonnet 4.5 (Anthropic API) | Natural language understanding, tool use |
| Search | pgvector (cosine similarity) + Cohere Rerank | Semantic + cross-encoder ranking |
| Embeddings | OpenAI text-embedding-3-small (1536 dims) | Product vectorization |
| Database | Supabase Postgres + pgvector extension | Product storage, vector search |
| Scraper | Python (httpx, BeautifulSoup, custom APIs) | Vendor data extraction |
| Enrichment | GPT-4o-mini Vision | Image-based description generation |
| Hosting | Railway (auto-deploy from GitHub) | Single-container deployment |

### Search Pipeline

```
User Query
    │
    ▼
┌─────────────────────────────────────────────┐
│ 1. ATTRIBUTE DETECTION                       │
│    Extract color words (40+) and material    │
│    words (32+) from the query                │
└─────────────────┬───────────────────────────┘
                  │
    ┌─────────────┴─────────────┐
    ▼                           ▼
┌──────────────┐    ┌────────────────────┐
│ 2a. VECTOR   │    │ 2b. ATTRIBUTE      │
│ SEARCH       │    │ SEARCH (if colors  │
│ 5x overfetch │    │ or materials found)│
│ via pgvector │    │ ILIKE matching     │
└──────┬───────┘    └────────┬───────────┘
       │                     │
       └──────────┬──────────┘
                  ▼
┌─────────────────────────────────────────────┐
│ 3. COHERE RERANK                             │
│    Cross-encoder re-scoring of 3x candidates │
│    (falls back to attribute scoring)         │
└─────────────────┬───────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────┐
│ 4. VENDOR DIVERSITY                          │
│    Max 2 products per vendor, backfill from  │
│    overflow to maintain result count         │
└─────────────────┬───────────────────────────┘
                  │
                  ▼
            Final Results
```

---

## 7. Data Model

### Vendors

| Field | Type | Description |
|-------|------|-------------|
| id | UUID | Primary key |
| name | TEXT | Vendor display name |
| url | TEXT | Vendor website URL (unique) |
| categories | TEXT[] | Product categories (furniture, lighting, rugs, etc.) |
| status | TEXT | active / inactive |
| priority | TEXT | high / medium / low |
| last_scraped_at | TIMESTAMPTZ | Last successful scrape time |
| scrape_success | BOOLEAN | Whether last scrape succeeded |
| notes | TEXT | Free-form notes |

### Products

| Field | Type | Description |
|-------|------|-------------|
| id | UUID | Primary key |
| vendor_id | UUID | FK → vendors (CASCADE delete) |
| vendor_name | TEXT | Denormalized vendor name |
| name | TEXT | Product name |
| description | TEXT | Product description |
| price | DECIMAL(10,2) | Numeric price (when available) |
| price_text | TEXT | Display price ("$4,200", "Trade pricing") |
| product_url | TEXT | Direct link to vendor product page |
| image_url | TEXT | Product image URL |
| categories | TEXT[] | Category tags (bedroom, lighting, rugs, etc.) |
| style_tags | TEXT[] | Style descriptors (traditional, modern, etc.) |
| material | TEXT | Primary material |
| dimensions | TEXT | Raw dimension string |
| color | TEXT | Product color |
| finish | TEXT | Product finish |
| in_stock | BOOLEAN | Stock status (nullable — null = unknown) |
| stock_text | TEXT | Human-readable stock status |
| size | TEXT | Size descriptor |
| collection | TEXT | Product collection name |
| sku | TEXT | Vendor SKU |
| embedding | VECTOR(1536) | Semantic search vector |
| search_text | TEXT | Concatenated searchable text |
| vision_enriched | TEXT | 'true' / 'failed' / null |

---

## 8. API Specification

### POST /api/chat

Primary chat endpoint. Sends user message to Claude with tool_use capability.

**Request:**
```json
{
  "message": "Find me 5 traditional dark wood king beds in stock",
  "conversation_id": "uuid (optional — omit for new conversation)"
}
```

**Response:**
```json
{
  "response": "Here are some beautiful traditional king beds in dark wood finishes from your vendors.",
  "products": [
    {
      "id": "uuid",
      "vendor_name": "Bernhardt",
      "name": "Sutton House Panel Bed",
      "description": "Classic panel design with carved detail...",
      "price_text": "$4,200",
      "product_url": "https://bernhardt.com/...",
      "image_url": "https://...",
      "categories": ["bedroom", "beds"],
      "in_stock": true,
      "stock_text": "In Stock",
      "material": "Walnut",
      "finish": "Dark Walnut",
      "size": "King",
      "similarity": 0.82
    }
  ],
  "conversation_id": "uuid"
}
```

### GET /api/products/search

Direct search endpoint bypassing the LLM layer.

**Parameters:**
- `q` (string, required) — Search query
- `in_stock` (boolean, optional) — Filter for in-stock items
- `limit` (integer, optional, default 10) — Max results

**Response:**
```json
{
  "products": [...],
  "total": 10,
  "query": "dark wood king bed"
}
```

### GET /api/vendors

List all vendors with scrape status.

**Parameters:**
- `status` (string, optional) — Filter by status (active/inactive)

### GET /health

Health check endpoint.

---

## 9. Frontend Specification

### Design System

| Element | Specification |
|---------|--------------|
| Script Font | Great Vibes (wordmark "Evenflow") |
| Serif Font | Playfair Display (headings, section titles) |
| Body Font | Inter (all body text, UI elements) |
| User Bubble | Navy (#1c2741) background, white text |
| Bot Bubble | White background, stone border, subtle shadow |
| Accent Color | Gold (#b8963e) — buttons, highlights, branding |
| Background | Cream/warm white (#faf9f7) |
| Max Width | 768px centered column |

### Chat Interface

- **Header:** "Evenflow" wordmark (Great Vibes) + "INTERIORS" subtitle + "Product Search Assistant"
- **Empty state:** Welcome message describing the chatbot's capabilities
- **Messages:** User messages right-aligned (navy), bot messages left-aligned (white)
- **Bot messages render Markdown** via `react-markdown`
- **Loading state:** Three animated gold bouncing dots
- **Input bar:** Auto-resizing textarea, Enter to send, Shift+Enter for newline, gold Send button

### Product Display

- **Products render before the text response** (cards first, concise text after)
- **Grouped by vendor** — Each vendor gets a labeled horizontal scroll row
- **Product card layout (w-44):**
  - Product image (160px tall, click opens vendor URL in new tab)
  - Product name (navy, linked to vendor page, 2-line clamp)
  - Price or "Trade pricing" (italic)
  - Stock badge (green dot = in stock, red = out of stock, gray = contact vendor)
  - Material / Finish / Size attributes (dot-separated, 1-line clamp)

---

## 10. Scraper Pipeline

### Overview

The scraper pipeline extracts product data from 68 vendor websites using a combination of generic and custom scrapers. Each scraper discovers product URLs, extracts structured data, generates embeddings, and upserts into the database.

### Scraper Types

| Type | Strategy | Vendor Count |
|------|----------|-------------|
| Shopify | `/products.json` API + pagination | 11 vendors |
| Magento 2 GraphQL | GraphQL product queries | 9 vendors |
| Custom REST APIs | Vendor-specific API endpoints | 6 vendors |
| SuiteCommerce REST | NetSuite commerce API | 2 vendors |
| WooCommerce | WP REST/Store API | 2 vendors |
| HTML Scraping | Sitemap crawl + HTML parsing | 5 vendors |
| Generic Sitemap | Sitemap → JSON-LD/OG extraction | Remaining vendors |

### Data Extraction Priority

1. **JSON-LD** (`application/ld+json` with `@type: Product`) — Most reliable
2. **Shopify product JSON** — Embedded in page or via API
3. **Open Graph meta tags** — `og:title`, `og:image`, `og:price:amount`
4. **CSS selectors** — Common patterns (`.product-title`, `.price`)
5. **LLM extraction** — Last resort for non-standard pages

### Vision Enrichment

Products with missing or poor descriptions are enriched using GPT-4o-mini vision analysis:

- **Input:** Product image URL
- **Output:** Enhanced `description`, `color`, `material`, `finish`, `style_tags`
- **Progress:** 53,347 products enriched (38% of 140K total)
- **Resume-safe:** Tracks `vision_enriched` status per product

### Scraping Rules

- Rate limit: 1 request/second per domain
- User-Agent: `EvenflowBot/1.0 (product-search; contact@evenflowinteriors.com)`
- Respect `robots.txt`
- Cache aggressively — don't re-scrape unchanged products
- Store raw data in `raw_data` JSONB column for debugging

---

## 11. Deployment

### Infrastructure

| Component | Service | Details |
|-----------|---------|---------|
| Application | Railway | Single Docker container, auto-deploy from GitHub `master` |
| Database | Supabase | Free tier, Postgres + pgvector, hosted |
| DNS/CDN | Railway | Automatic HTTPS |
| Docs Site | GitHub Pages | Vendor scrape report at `onada-dev.github.io/evenflow-docs` |

### Docker Build

Multi-stage build:
1. **Stage 1 (Node 20):** Build React frontend → `dist/`
2. **Stage 2 (Python 3.12):** Install backend deps, copy frontend `dist/` to `backend/static/`, run FastAPI with uvicorn

FastAPI serves both the API (`/api/*`, `/health`) and the React SPA (all other routes) from a single container.

### Environment Variables

| Variable | Purpose |
|----------|---------|
| `ANTHROPIC_API_KEY` | Claude API access |
| `SUPABASE_URL` | Database connection |
| `SUPABASE_SERVICE_KEY` | Service role key (bypasses RLS) |
| `OPENAI_API_KEY` | Embedding generation |
| `COHERE_API_KEY` | Reranking (optional, falls back gracefully) |

---

## 12. Current Status & Roadmap

### Current State (February 2026)

- **139,405 products** from **58 active vendors** in the database
- **Search pipeline** fully operational with vector search + Cohere rerank + vendor diversity
- **Chat interface** deployed on Railway with conversation context
- **38% of products** enriched with GPT-4o-mini vision
- **7 vendors** newly confirmed accessible, pending scraping
- **3 vendors** still blocked (Vanguard login-gated, Global Views Cloudflare, Bramble Co complex)

### Phase 2 Roadmap

1. **Scrape newly accessible vendors** (7 vendors confirmed by Lisa)
2. **Re-scrape incomplete vendors** (13 vendors with low product counts)
3. **Complete vision enrichment** (62% of products remaining)
4. **Capture dimensions** for 14 vendors where they're available on-site
5. **Implement vendor ranking weights** (after Lisa fills in category rankings)
6. **Add response streaming** for faster perceived chat response times
7. **Persistent conversation storage** (survive server restarts)
8. **Automated scrape scheduling** (weekly cron for high-priority vendors)

### Phase 3 Roadmap

1. **User authentication** and role-based access
2. **Saved searches and product boards** for project organization
3. **Search analytics** — popular queries, vendor usage, conversion tracking
4. **Mobile-optimized interface**
5. **Price comparison** across vendors for similar products

---

## 13. Risks & Mitigations

| Risk | Impact | Mitigation |
|------|--------|-----------|
| Vendor sites change structure | Scraper breaks, stale data | Multiple extraction strategies, fallback chain, manual re-scraping scripts |
| Cloudflare/WAF blocking | Can't scrape vendor | Flag as blocked, ask Lisa for alternative access methods |
| Claude API latency | Slow chat responses (5–15s) | 60s client timeout, typing indicator, future: streaming |
| Supabase free tier limits | DB size/connection limits | Monitor usage, upgrade tier if needed |
| Data quality varies by vendor | Poor search results for some vendors | Vision enrichment pipeline, data quality reporting |
| Trade pricing hidden | Can't show prices | Display "Trade pricing — contact vendor" gracefully |
| In-memory conversations | Lost on redeploy | Acceptable for MVP; Phase 2 adds persistence |

---

## 14. Appendices

### A. Vendor Categories

The system organizes vendors into 12 consolidated categories:

1. **Furniture** — Case goods, tables, storage, decorative furniture (33 vendors)
2. **Upholstery / Seating** — Sofas, chairs, custom upholstery (13 vendors)
3. **Lighting** — Chandeliers, lamps, sconces, outdoor lighting (14 vendors)
4. **Rugs** — Area rugs, runners, outdoor rugs (4 vendors)
5. **Art / Wall Decor** — Framed art, wall sculptures, mirrors (7 vendors)
6. **Decor / Accessories** — Vases, objects, bookends, trays (13 vendors)
7. **Fabrics / Textiles** — Drapery, upholstery fabric, trim (5 vendors)
8. **Outdoor** — Outdoor furniture, umbrellas, fire features (4 vendors)
9. **Bedding / Pillows** — Luxury bedding sets, decorative pillows (2 vendors)
10. **Wallcoverings** — Wallpaper, hand-painted panels (2 vendors)
11. **Bathroom** — Vanities, bath furniture (2 vendors)
12. **Window Treatments** — Shades, blinds, custom drapery (1 vendor)

### B. Repository Structure

- **`Onada-Dev/evenflow-chatbot`** — Main application (backend + frontend + scrapers)
- **`Onada-Dev/evenflow-docs`** — Documentation and GitHub Pages site
