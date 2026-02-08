# CLAUDE.md — AI Assistant Guide for kergi-shop

## Project Overview

**kergi-shop** is a single-page e-commerce website for KER-GI, a natural health supplement sold in Senegal. The entire application lives in a single `index.html` file (~2100 lines) with no build tools, no backend server, and no package manager. Orders are processed via WhatsApp.

- **Language**: French (targeting Senegalese customers)
- **Currency**: FCFA (West African CFA franc)
- **Contact**: WhatsApp +221 77 335 14 14

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Markup | HTML5 (single file) |
| Styling | CSS3 (inline `<style>` block, CSS variables, media queries) |
| Logic | Vanilla JavaScript (no frameworks) |
| Icons | Font Awesome 6.4.0 (CDN) |
| Storage | Browser `localStorage` (cart persistence) |
| Orders | WhatsApp Web API (pre-formatted messages) |
| Backend | None — fully client-side |

## Repository Structure

```
kergi-shop/
├── index.html      # Entire application (HTML + CSS + JS)
├── README.md       # Short project description
└── CLAUDE.md       # This file
```

There is no `package.json`, no `node_modules`, no build pipeline, and no test suite.

## index.html Layout (line ranges are approximate)

| Section | Lines | Description |
|---------|-------|-------------|
| CSS styles | 1–1150 | All styling, CSS variables, animations, responsive breakpoints |
| HTML body | 1151–1668 | Page structure: header, hero, products, testimonials, FAQ, cart modal |
| JavaScript | 1670–2109 | Product data, `ShoppingCart` class, event listeners, helpers |

### Key HTML Sections

- **Header**: Fixed sticky nav with logo, menu links, cart button
- **Welcome Banner**: Animated marquee with promotional text
- **Hero**: Main CTA with SVG product graphic
- **Problems**: Pain-point cards for target customers
- **Products**: Grid of 3 product cards (Classique, Premium, Pack)
- **Benefits**: Feature highlights (natural, halal, ancestral formula)
- **Testimonials**: Auto-rotating carousel (8 testimonials, 5s interval)
- **Spiritual Services**: Istikhara and invocation services
- **FAQ**: Collapsible accordion (4 items)
- **Contact**: Form with WhatsApp integration
- **Cart Modal**: Full checkout flow with form validation
- **WhatsApp Float**: Fixed floating button (bottom-right)

### Key JavaScript Components

- **Product data array** (~line 1672): Defines 3 products with id, name, price, badge, stock, features
- **`ShoppingCart` class** (~line 1717): Core cart logic — add/remove/update items, localStorage sync, price formatting, phone validation, checkout via WhatsApp
- **Event listeners** (~line 1930): Navigation, mobile menu toggle, testimonial carousel, FAQ accordion, form submissions

## Development Workflow

### Running Locally

No install or build steps required. Just open `index.html` in a browser:

```bash
# Any of these work:
open index.html
xdg-open index.html
python3 -m http.server 8000  # then visit http://localhost:8000
```

### Making Changes

Since everything is in one file, edit `index.html` directly:
- **CSS changes**: Modify the `<style>` block (top of file)
- **Content/layout changes**: Edit the HTML body (middle of file)
- **Behavior changes**: Edit the `<script>` block (bottom of file)

### Testing

There is no automated test suite. Test manually by:
1. Opening `index.html` in a browser
2. Checking responsive behavior at different viewport widths
3. Testing cart add/remove/checkout flow
4. Verifying WhatsApp message formatting
5. Checking navigation and scroll behavior

## Conventions and Patterns

### CSS

- **Color scheme** defined via CSS variables in `:root`:
  - Primary background: black (`#000000`)
  - Accent/CTA: gold (`#FFD700`)
  - Error/notification: red (`#DC2626`)
  - Success: green (`#22C55E`)
  - Card backgrounds: dark grays (`#1F2937`, `#374151`)
- **Responsive breakpoints**: 768px (tablet), 480px (mobile)
- **Animations**: `bannerGlow` (3s), `pulse` (2s), `slideIn` for notifications

### JavaScript

- No modules or imports — all code in a single `<script>` block
- `ShoppingCart` is the main class, instantiated as a global
- Cart data stored in `localStorage` under key `'kergi_cart'`
- Phone validation regex for Senegalese numbers: `/^(77|78|76|75|70|33)[0-9]{7}$/`
- Prices formatted with thousand separators and "FCFA" suffix
- Checkout generates a WhatsApp `wa.me` URL with encoded message

### Hardcoded Data

The following are embedded directly in the code (no environment variables):
- WhatsApp number: `221773351414`
- Product catalog (3 items with prices in FCFA)
- 14 Senegalese delivery cities
- Payment methods: Cash, Wave, Orange Money, Free Money
- Testimonial content (8 entries)

## Important Constraints

1. **Single-file architecture**: All changes must go in `index.html`. Do not split into separate files without explicit instruction.
2. **No build tools**: Do not introduce webpack, vite, or similar without explicit instruction.
3. **No backend**: The site is purely static. Order processing happens via WhatsApp redirect.
4. **French language**: All user-facing text must be in French.
5. **Senegal-specific**: Phone validation, city lists, currency, and delivery details are Senegal-specific.
6. **WhatsApp dependency**: The checkout flow relies on WhatsApp Web — do not replace without instruction.
7. **CDN dependency**: Font Awesome is loaded from cdnjs.cloudflare.com — no local copy.

## Common Tasks

### Adding a new product
Add an entry to the `products` array (~line 1672) with: `id`, `name`, `price`, `badge`, `stock`, `features[]`, and optionally `bonus`. The product grid renders dynamically from this array.

### Changing the color scheme
Update CSS variables in the `:root` selector at the top of the `<style>` block.

### Adding a new delivery city
Find the `<select id="city">` in the cart modal HTML and add a new `<option>`.

### Modifying the checkout WhatsApp message
Edit the `checkout()` method in the `ShoppingCart` class (~line 1870).
