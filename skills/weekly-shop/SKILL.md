---
name: weekly-shop
description: Plan the week's meals, compare the ingredient shopping list using CartSorted prices, and get cart links for the cheapest eligible retailer or a split across stores. Use when the user wants a weekly meal plan, dinner ideas, or the ingredients in a supermarket cart.
---

Use this skill for the weekly cycle: plan meals, price the list, fill carts.

## Plan a week and shop for it

1. Read `get_meal_plan` for the requested week. Keep the meals already saved
   unless the user asks to change them. Use the same `week_start` for the
   plan, shopping list and cart links.
2. Use `suggest_meals`, `get_weekly_menu` or `list_recipes` to offer meals for
   the days and meal types the user wants. Use recipe IDs from the results.
   Show the proposed days and meals. Do not invent recipes, prices or claims
   about a recipe's diet suitability.
3. Add meals with `add_to_plan` when the user has chosen them or has asked you
   to choose and save the plan. Pass the servings the user gives. Otherwise,
   omit `servings` to use their saved household size. Read `get_meal_plan`
   again to confirm what was saved.
4. Call `get_shopping_list` to price the ingredients for the saved plan.
   Use its products, pack counts, prices and store decision. Do not build a
   second basket from recipe names or calculate ingredient totals yourself.
5. When the user asks to get the ingredients from the cheapest retailer,
   call `get_cart_handoff` with `plan: "single_shop"` and omit `retailer`.
   CartSorted then selects the store. Present its cart links and the items
   that need manual action. The user opens the links and checks out.

## Plan

- `get_meal_plan` shows what is planned for a week, in cooking order.
  `week_start` is any date in the week and defaults to this week. Each meal
  has an `entry_id`.
- `get_weekly_menu` is the specials-led menu: each meal's cost per serve and
  the specials it uses. `suggest_meals` ranks recipes for the next free
  dinner and gives the reasons; it has no costs. `list_recipes` finds
  recipes in the user's library — pass `query` ("chicken", "vegetarian
  pasta") instead of reading the whole library.
- `add_to_plan` adds a recipe to a day (0 = Monday). Only add what the user
  chose or clearly asked for, then confirm the day and meal.
- `remove_from_plan` takes one meal off the plan by its `entry_id`. Use it
  only when the user asks to remove a meal or undo an add, and say which
  meal you are removing first.
- Supermarket specials run on their own weekly cycle, so the day of the shop
  decides which specials a plan can use. `get_meal_plan` returns the shop
  day and its specials; relay `shop_notice` when it is there. When the user
  says which day they shop, save it with `set_shop_day`.

## Price

- `get_shopping_list` prices the plan: every compared store's total
  (`retailers`), the items to buy at each store (`stores`), the whole list at
  one shop as an alternative (`single_shop`), and what could not be priced.
  Give the headline and the verdict as the tool wrote them.
- Only call a store cheapest when the result's verdict or a retailer's
  `cheapest` field says so. A `single_shop` result can select a store for
  coverage; its presence alone is not a cheapest claim. Show its `missing`
  items, the list's `unpriced` items and any `stale_note`.
- `food_cost` is what the plan's food costs at a store and is the figure
  stores are ranked on; `till` is what whole packs ring up when that
  differs. Don't present one as the other.

## Fill carts → `get_cart_handoff`

1. Match the user's choice of shop:
   - For "the cheapest retailer", "one store" or "the whole shop", pass
     `plan: "single_shop"`. Omit `retailer` so CartSorted selects the store,
     unless the user has named one. Use `list_stores` for a named store's slug.
   - For the cheapest items across stores, pass `plan: "split"`. CartSorted
     assigns each item to a store and returns a sheet for each store.
   - If the user has not said which they want, call it once with the default
     split, then show the `alternative` and let them choose.
2. It can take up to ~40 seconds for a large plan.
3. The card shows one section per store with a button per cart link. In the
   reply, tell the user:
   - to sign in to each store in their own browser first;
   - links marked as adding to the cart must be opened **once** — opening one
     twice doubles the items;
   - items under "add by hand" have a search link each;
   - items marked "you have it" should not be bought;
   - for a store using the browser extension, the CartSorted extension fills
     the whole sheet on the store's own page.
4. Follow the result's `instructions`, and each sheet's own `instructions`,
   exactly. Stop at the cart: the user reviews it and pays themselves.
5. If `sheets` is empty, read `note`: it says why.

## Never

- Never ask for, type or store a supermarket password, verification code or
  payment details.
- Never open a link for the user, reproduce an extension's request, or go
  past the cart review page.
- Never buy an item the sheet says the user already has.
- Never remove a meal the user did not ask you to remove.
