---
name: cheapest-store
description: Find the cheapest supermarket for a list of groceries, or compare one product's price across stores. Use when the user asks where to shop, which store is cheaper, or how much something costs at each store.
---

Use this skill when the user wants to know where a shop, or one product, is
cheapest.

## A list of items → `compare_basket`

1. Collect the items. Each item is free text ("plain flour 1kg"), a retailer
   product URL, or a barcode. Keep the user's sizes and brands; set `qty` only
   when they give a count. If the list is empty or unclear, ask for it.
2. Call `compare_basket` once with the whole list (up to 100 items). Choose
   `match_mode` from how the items were given:
   - `substitutes` when the list is everyday items typed as words ("milk,
     bread, eggs"). Each store's own version of an item then counts, which is
     what makes the stores comparable. Use it for any generic list.
   - the default when the user named specific brands, or gave product URLs or
     barcodes: one exact product per line, at any size.
   - `exact` only when they want the same product in the same size.
3. The card shows the stores, totals and items. In the reply, give the verdict
   in one or two sentences instead of repeating the table:
   - If `verdict.cheapest` is set, name that store and the saving the tool
     returned.
   - If it is missing, do not name a cheapest store. Say why, from
     `verdict.cheapest_withheld`: `coverage` means no store stocks enough of
     the list (name `verdict.most_complete`); `zone` means some prices are
     from another price zone.
   - Mention the store plan's split ("buy these there, those here") when it
     uses more than one store, and any items no store priced.
   - A line's `substitutes` are different products on offer for it. They are
     in no total: offer one as a swap, never as the line's price.
   - `stores_without_prices` were compared and had no current price for the
     list. Say that; do not say they are not covered.
   - If a default-mode list of plain items comes back with each line priced
     at one store only, run it again with `match_mode: "substitutes"`.
4. Mention stale prices only when `stale_line_count` is above zero.

## A place → `postcode`

Prices come from the user's saved postcode. If they ask about somewhere else
("in Perth", "near 6012", "at my parents' place"), pass that postcode or ZIP as
`postcode` to `compare_basket` or `search_product`. It applies to that call
only. Use `set_postcode` only when they want the change kept. A search echoes
the postcode it used as `postcode`, and a basket echoes it as
`location.postcode`. Say which place the prices are for when it is not their
saved one.

## One product → `search_product`

1. Call `search_product` with the product as the user named it, or its
   product URL. Use `limit`, `max_price` or `sort` only when they ask for
   more results, a budget or an order, and `postcode` only when they name a
   place (see "A place" above).
2. Read what kind of answer came back:
   - `verdict` is the store comparison of one product, named in
     `verdict.subject`. Give its headline as being about that product.
   - With no verdict, `best_per_store` lists the best-value offer at each
     store (ids into `offers`). Give those; the first is the best value.
   - `kinds` means the search covers several kinds of product. Ask which the
     user meant, or answer for the kind they clearly asked about.
   - An offer marked `differs` is another size, or a variant (organic, lite)
     the user did not ask for. Say so if you mention it.
   - `note` explains an empty or partial result. Relay it.
3. Offers are already ordered; don't re-rank them. A unit price reads
   `unit_price` per `unit` ("$1.65/L").

## A store by name → `list_stores`

If the user asks about a specific store ("is it cheaper at X?") and X is not
in the comparison result, call `list_stores`. If X is not listed, say
CartSorted doesn't cover it and give the comparison for the stores it does.

## Don't

- Don't say or imply a store is covered unless `list_stores` or a comparison
  result names it.

- Don't compute, round or re-rank prices, totals or savings yourself.
- Don't compare stores across countries.
- Don't claim a price is current if the tool marked it older.
