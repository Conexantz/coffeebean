# Café stock tracker

A single-page stock tracker for three café outlets (Jurong, Orchard, Tampines). It reads stock from Supabase and saves stock and reorder levels back to it.

- Outlet tabs, with items grouped by category
- Steppers and a Save button per item for stock on hand and reorder level
- Items at or below their reorder level are marked **Low**
- A combined "Order from suppliers" list that totals the shortfall across outlets, with a Copy button

## Deploy on Vercel

It is a static site with no build step.

1. In Vercel, choose **Add New → Project** and import this repository.
2. Leave **Framework Preset** as **Other**, with no build command and the root directory as `./`.
3. Deploy. `index.html` is served at the site root.

## Supabase

The page talks to the Supabase REST API directly using the project URL and publishable key at the top of the script in `index.html`. The publishable key is meant to be public; access is controlled in the database:

- `outlets`, `items` and `stock_counts` are readable by anyone with the key (read-only RLS policies).
- Writes go only through the `save_stock_level(p_outlet_code, p_sku, p_qty_on_hand, p_reorder_level)` function, which accepts values from 0 to 100,000 and records the count for today's date (Singapore time).
- The page reads the `latest_stock` view: the most recent count per outlet and item.

Anyone who can open the deployed site can change stock and reorder levels. Add Supabase Auth before sharing the link beyond your team.
