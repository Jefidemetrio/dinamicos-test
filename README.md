# Dinamicos.pt - Presentation Website

Responsive, deploy-ready presentation website for dinamicos.pt with four company fronts.

## Files

- `index.html` — Semantic HTML5 structure (nav, hero, main grid of 4 sections, footer)
- `style.css` — Mobile-first CSS with responsive breakpoints at 640px and 1024px
- `assets/` — Empty folder reserved for images/icons (ready to populate)

## Directus Data Structure

Full Directus collection schema available at:

- **`docs/DIRECTUS-SCHEMA.md`** — Complete schema documentation with field definitions
- **`directus-collections.json`** — JSON schema for Directus API/schema import
- **`directus-schema.json`** — Full collection definitions with seed data
- **`migrations/001_directus_schema.sql`** — SQL migration script for database setup

### Collections

| Collection | Type | Purpose |
|------------|------|---------|
| `company_info` | Singleton | Global company information (name, tagline, contact) |
| `fronts` | Collection | 4 company sections (Web Dev, Admin, Publicidade, Modular) |
| `services` | Collection | Service offerings linked to fronts (N:1) |
| `process_steps` | Collection | 4-step process (Escuta, Planeamento, Execução, Acompanhamento) |
| `case_studies` | Collection | Before/after client success stories |
| `contact_info` | Singleton | Global contact information |
| `navigation_menu` | Collection | Main navigation menu items |

### Key Features

- **Color-coded fronts**: Each front has its own color (#3B82F6, #10B981, #E8733A, #6B7280)
- **CTA types**: B2B or B2C differentiation for appropriate call-to-action buttons
- **Value propositions**: Short text above-the-fold for each front
- **Before/After case studies**: Transformation narratives for social proof
- **Process visualization**: 4-step workflow across all fronts

## Structure

```
dinamicos-pt/
├── index.html
├── style.css
├── README.md
├── assets/
│   └── [images go here]
├── docs/
│   └── DIRECTUS-SCHEMA.md
├── migrations/
│   └── 001_directus_schema.sql
├── directus-collections.json
├── directus-schema.json
├── directus-statistics.md
└── SEED_DATA.md
```

## Responsive Breakpoints

| Breakpoint | Layout change |
|------------|---------------|
| < 640px   | Single column, centered nav stack |
| ≥ 640px   | Navigation spreads, larger hero text |
| ≥ 1024px  | Main grid becomes 2×2 columns |

## Deployment

The site is a static, fully self contained website. To deploy:

1. Place `index.html` and `style.css` in the web root (e.g., `/var/www/dinamicos.pt`).
2. Serve via any static file server (nginx, Apache, Caddy, or Vercel/Netlify drop).
3. Add assets as needed to `assets/`.

No build step required — just copy and serve.

## Directus Integration

To connect the frontend to Directus:

1. Install Directus and configure the database
2. Import the schema from `directus-schema.json` via Directus CLI or API
3. Upload media assets (logos, hero images, case study thumbnails)
4. Seed initial data via Directus Admin UI or API

### API Endpoints

```bash
# Get all active fronts with color info
GET /items/fronts?filter[is_active][_eq]=true&sort=order

# Get services for a specific front
GET /items/services?filter[front_id][_eq]=1&sort=order

# Get case studies for a front
GET /items/case_studies?filter[front_id][_eq]=1&sort=-is_featured

# Get process steps
GET /items/process_steps?sort=order

# Get navigation menu
GET /items/navigation_menu?filter[is_active][_eq]=true&sort=order

# Get global settings
GET /items/company_info
GET /items/contact_info
```

## Color Palette

| Front | Color | Hex Code | Usage |
|-------|-------|----------|-------|
| Web Development | Azul (Blue) | #3B82F6 | Tech/digital focus |
| Company Administration | Verde (Green) | #10B981 | Growth/professionalism |
| Advertising/Publicity | Laranja (Orange) | #E8733A | Creativity/energy |
| Modular Construction | Cinza (Gray) | #6B7280 | Modern/minimalist |