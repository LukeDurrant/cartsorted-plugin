---
name: weekly-shop
description: Plan the week's meals and turn the plan into a priced shopping list and per-store cart links. Use when the user asks about their meal plan, wants dinner ideas for the week, or wants their shopping in a supermarket cart.
---

Use this skill for the weekly cycle: plan meals, price the list, fill carts.

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
- `food_cost` is what the plan's food costs at a store and is the figure
  stores are ranked on; `till` is what whole packs ring up when that
  differs. Don't present one as the other.

## Fill carts → `get_cart_handoff`

1. There are two ways to do the shop. `plan: "split"` (the default) buys each
   item at the store where the plan found it cheapest, one cart per store.
   `plan: "single_shop"` puts the whole list in one cart at one store. If the
   user has not said which they want, call it once, then tell them what
   `alternative` says the other way would cost and let them choose.
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
