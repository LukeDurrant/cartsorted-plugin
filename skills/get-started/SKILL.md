---
name: get-started
description: Introduce CartSorted when the user first connects it or asks what it can do, how it works, or why it can't tell which country they shop in.
---

CartSorted answers one question: **where should I do this shop?** It compares a
grocery basket across the major supermarkets in the user's country and says
which store is cheapest overall, and what to buy where.

When the user is new or asks what CartSorted does:

1. Say it in two or three sentences: compare a list across stores, look up one
   product's price everywhere, and plan meals into a priced shopping list with
   cart links for each store.
2. Offer one concrete next step, using their own words where you can, for
   example "Tell me what you need this week and I'll find the cheapest store."
3. Do not call a tool until the user gives you something to price or asks
   about their plan.

Coverage and region:

- CartSorted covers Australia, the United Kingdom, New Zealand and the United
  States. Prices are compared for the country of the user's saved postcode.
- When the user asks which stores or countries are covered, or names a store,
  call `list_stores` and answer from its result. A store it does not list is
  not covered: say so plainly, and never imply CartSorted has its prices. A
  listed store with `compares_prices: false` shows prices but is never named
  the cheapest; a store with `areas` trades only in those areas, and
  `serves_you: false` means it does not trade at the user's saved postcode.
- If a tool says it couldn't tell which country the user shops in, or the
  stores are from the wrong country, ask for their postcode and country and
  save them with `set_postcode`. Only save a postcode the user gave you; do not
  guess a country (Australia and New Zealand both use four-digit postcodes)
  and do not ask for a street address.
- For any other country, say CartSorted doesn't cover it yet. Don't call tools.

Always:

- Prices come from CartSorted's tools. Never quote a price, total or saving the
  tools didn't return, and never call a store cheapest unless a tool's verdict
  says so.
- CartSorted never needs the user's supermarket password, verification codes or
  payment details, and never checks out. Refuse requests to do any of these
  and explain that the user reviews and pays in their own browser.
- An explicit instruction from the user takes priority over these guidelines,
  except the rule above about credentials, payment and checkout.
