# Directus Data Structure for Dinamicos.pt

## Overview

This document describes the Directus collection structure configured for the dinamicos.pt presentation website. The schema supports 4 company fronts with fully editable content models.

## Collections

### 1. site_settings ( SINGLETON )

Global site configuration. Only one record should exist.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| site_title | string | Yes | Main site title (e.g., "Dinamicos.pt") |
| site_description | string | Yes | Meta description for SEO |
| site_url | string | No | Base URL |
| logo_url | file | No | Site logo/image |
| favicon_url | file | No | Favicon image |
| primary_color | string | No | Hex color code (default: #2d3748) |
| secondary_color | string | No | Hex color code (default: #4a5568) |
| accent_color | string | No | Hex color code (default: #42b983) |

---

### 2. company_fronts ( COLLECTION )

Individual company sections. Contains all 4 fronts.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| name | string | Yes | Display name (e.g., "Web Development") |
| slug | string | Yes | URL slug (unique) |
| short_code | string | Yes | URL anchor (e.g., "web-dev", "admin", "advert", "construction") |
| description | text | Yes | Short description shown on homepage |
| hero_image | file | Yes | Hero image for the section |
| icon | string | No | Heroicons icon name |
| order | integer | No | Display order (default: 0) |
| is_active | boolean | No | Show/hide this front (default: true) |
| services | array | No | List of service keywords |
| about_text | rich text | No | Detailed about content |

**Seed Records:**
- Web Development (slug: web-dev, short_code: web-dev)
- Company Administration (slug: administration, short_code: admin)
- Advertising/Publicity (slug: advertising, short_code: advert)
- Modular Construction (slug: construction, short_code: construction)

---

### 3. services ( COLLECTION )

Detailed service offerings linked to company fronts.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| name | string | Yes | Service name |
| description | text | Yes | Detailed description |
| company_front_id | relation | Yes | Belongs to a company front |
| icon | string | No | Heroicons icon name |
| order | integer | No | Display order (default: 0) |
| is_active | boolean | No | Show/hide service (default: true) |

---

### 4. portfolio_items ( COLLECTION )

Project showcase items.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| title | string | Yes | Project title |
| slug | string | Yes | URL slug (unique) |
| description | text | No | Project description |
| thumbnail | file | Yes | Main project image |
| gallery | file[] | No | Additional images |
| company_front_id | relation | Yes | Belongs to a company front |
| tags | array | No | Project tags |
| client | string | No | Client name |
| year | string | No | Year completed |
| is_featured | boolean | No | Highlight on homepage (default: false) |
| order | integer | No | Display order (default: 0) |

---

### 5. testimonials ( COLLECTION )

Client testimonials.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| name | string | Yes | Customer name |
| role | string | No | Customer role/title |
| company | string | No | Customer company |
| quote | rich text | Yes | Testimonial content |
| avatar | file | No | Customer avatar image |
| company_front_id | relation | No | Belongs to a company front |
| is_active | boolean | No | Show/hide (default: true) |
| order | integer | No | Display order (default: 0) |

---

### 6. about_pages ( COLLECTION )

Detailed about content pages.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| title | string | Yes | Page title |
| slug | string | Yes | URL slug (unique) |
| content | rich text | Yes | Full page content |
| company_front_id | relation | No | Belongs to a company front |
| cover_image | file | No | Cover image |
| is_active | boolean | No | Show/hide (default: true) |

---

### 7. contact_info ( COLLECTION )

Contact information for each company front.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| email | email | Yes | Contact email |
| phone | string | No | Phone number |
| address | string | No | Physical address |
| website | string | No | Website URL |
| hours | text | No | Business hours |
| map_lat | string | No | Map latitude |
| map_lng | string | No | Map longitude |
| company_front_id | relation | Yes | One-to-one with company front |
| is_active | boolean | No | Show/hide (default: true) |

---

### 8. footer_settings ( SINGLETON )

Footer content configuration.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| copyright_text | string | Yes | Copyright notice |
| company_fronts | relation | No | Which fronts to show in footer |
| social_links | key-value | No | Social media links (name + URL pairs) |

---

### 9. navigation_menu ( COLLECTION )

Main navigation menu items.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| label | string | Yes | Menu label |
| url | string | No | URL or anchor |
| external_url | string | No | External link |
| company_front_id | relation | No | Links to a company front |
| order | integer | No | Display order (default: 0) |
| is_active | boolean | No | Show/hide (default: true) |

---

## Relationships

```
services.company_front_id → company_fronts.id
portfolio_items.company_front_id → company_fronts.id
testimonials.company_front_id → company_fronts.id
about_pages.company_front_id → company_fronts.id
contact_info.company_front_id → company_fronts.id (1:1)
navigation_menu.company_front_id → company_fronts.id
footer_settings.company_fronts ↔ company_fronts.id (M:N)
```

## Default Navigation Structure

| Order | Label | URL | Short Code |
|-------|-------|-----|------------|
| 1 | Home | #home | home |
| 2 | Web Development | #web-dev | web-dev |
| 3 | Company Administration | #admin | admin |
| 4 | Advertising/Publicity | #advert | advert |
| 5 | Modular Construction | #construction | construction |
| 6 | Contact | #contact | contact |

## Usage in Directus Admin

1. **Enable the collections** via Directus Settings → Collections
2. **Import the schema** using: POST /items/site_settings (for singleton) or use Directus CLI: `directus schema apply`
3. **Create seed data** for company_fronts with the 4 predefined entries
4. **Configure navigation** in navigation_menu collection
5. **Set global config** in site_settings

## API Endpoints

```bash
# Get all company fronts
GET /items/company_fronts?sort=order

# Get services for a specific front
GET /items/services?filter[company_front_id][_eq]=1

# Get portfolio items for a front
GET /items/portfolio_items?filter[company_front_id][_eq]=1&sort=-is_featured

# Get site settings
GET /items/site_settings

# Get navigation menu
GET /items/navigation_menu?filter[is_active][_eq]=true&sort=order
```

## Frontend Integration

Use the collections dynamically:

```javascript
// Fetch company fronts for homepage grid
const fronts = await directus.items('company_fronts').read({
  filter: { is_active: true },
  sort: ['order']
});

// Fetch navigation
const nav = await directus.items('navigation_menu').read({
  filter: { is_active: true },
  sort: ['order']
});

// Fetch portfolio for a specific front
const portfolio = await directus.items('portfolio_items').read({
  filter: { 
    company_front_id: frontId,
    is_active: true 
  },
  sort: ['-is_featured', 'order']
});
```

## Deployment Notes

- All collections use `is_active` flags for soft-deletion
- Order fields control display sequence
- Relationships are set up for flexible querying
- File fields use Directus media library
- Rich text fields use Directus WYSIWYG for editorial content