# AasaMedChem Inventory Management System

Next.js 15 + Neon PostgreSQL + Vercel — Inventory & Order Management for chemicals/lab supplies.

## Live Demo
- Admin: admin@aasa.com / admin123
- Seller: seller@aasa.com / seller123

## Tech Stack
- **Frontend/Backend**: Next.js 15 (App Router)
- **DB**: Neon PostgreSQL (serverless)
- **Auth**: JWT (jose) + bcryptjs, httpOnly cookies
- **Deployment**: Vercel

## Unit Storage Strategy

| Dimension | Base Unit Stored | Supported Display Units |
|-----------|-----------------|------------------------|
| weight    | g (grams)       | g, kg                  |
| volume    | mL (millilitres)| mL, L                  |
| count     | item            | item                   |

- base_price = INR per 1 base_unit (e.g. ₹2.50/g)
- price_per_kg = base_price × 1000
- All DB fields: NUMERIC(20,6) — exact decimal, 6dp precision
- Conversions in lib/units.ts; applied before saving & before display

## Database Schema (key tables)

**products**: id, name, sku, dimension, base_unit, display_unit, quantity NUMERIC(20,6), base_price NUMERIC(20,6)

**orders**: id, user_id FK, status, total_amount NUMERIC(20,6)

**order_items**: order_id FK, product_id FK, ordered_unit, ordered_quantity NUMERIC(20,6), base_quantity NUMERIC(20,6), unit_price NUMERIC(20,6), line_total NUMERIC(20,6)

## Local Setup

```bash
git clone <repo> && cd medchem-inventory
npm install
cp .env.example .env.local  # add DATABASE_URL + JWT_SECRET
npm run dev
curl http://localhost:3000/api/init  # seed DB
```

## Vercel Deploy

```bash
vercel
# Set DATABASE_URL + JWT_SECRET in Vercel dashboard
curl https://your-app.vercel.app/api/init
```
