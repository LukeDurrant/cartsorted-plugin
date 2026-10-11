# CartSorted

CartSorted answers one question: **where should I do this shop?** Give your
assistant a grocery list and it compares the whole basket across the major
supermarkets in your country, using fresh prices from CartSorted's own
catalogue, and tells you which store is cheapest and what to buy where.

You can also plan the week's meals from your recipe library. CartSorted
prices the ingredients and returns a shopping list and cart links for one
store or a split across stores. Ask for the cheapest retailer to get the
whole-shop option. The answer shows missing items and only names a cheapest
store when the price comparison supports it. You review the cart and pay on
the store's site.

For shoppers in Australia, the United Kingdom, New Zealand and the United
States.

## What you can ask

- "Which supermarket is cheapest for milk, bread, eggs, chicken and rice?"
- "How much is a 2kg bag of rice at each store?"
- "Which supermarkets do you compare in my country?"
- "Suggest a cheap dinner for Wednesday and add it to my plan."
- "Help me plan this week's dinners, then get the ingredients from the cheapest retailer."
- "Turn this week's meal plan into cart links for each store."

## Install

**Claude Code**

```bash
claude plugin marketplace add LukeDurrant/cartsorted-plugin
```

```bash
claude plugin install cartsorted@cartsorted
```

Then type `/mcp` in a session, choose the CartSorted server and sign in.

**Claude (web, desktop, mobile) and ChatGPT:** add CartSorted from the
plugin directory once it is listed. Until then, add
`https://cartsorted.com/api/mcp` as a custom connector (Claude) or under
Plugins in Developer mode (ChatGPT). Steps for each app are at
[cartsorted.com/ai](https://cartsorted.com/ai).

## What is in the plugin

- **A connector** to CartSorted's hosted MCP server at
  `https://cartsorted.com/api/mcp`. It provides the tools: basket
  comparison, product price lookup, the list of stores covered, your recipes
  and meal plan, a priced shopping list, and each store's own cart links.
  Price comparisons and cart links are shown as cards where the app supports
  them.
- **Three skills** that tell the assistant how to use those tools:
  `get-started`, `cheapest-store` and `weekly-shop`.

The plugin runs nothing on your computer. It contains no scripts, hooks or
executables.

## Signing in

The first time a tool runs you are sent to CartSorted's own consent page to
sign in and approve the connection. Create an account at
[cartsorted.com](https://cartsorted.com) first. Your assistant never
sees your CartSorted password, and you can disconnect at any time in your
assistant's connector settings.

Prices are compared for the country of the postcode saved in your CartSorted
settings. If the assistant says it can't tell which country you shop in, save
your postcode there and ask again.

## What it sends and receives

Your assistant sends CartSorted the items, products or week you ask about.
CartSorted returns prices, store comparisons, your recipes, meal plans and
shopping lists, and cart links. It does not return your email address,
password or payment details. See the
[privacy policy](https://cartsorted.com/privacy).

## What it can change

Four things in your CartSorted account, each only when you ask: your saved
postcode, the day you do the week's shop, adding a recipe to your meal plan,
and taking a meal off it. Taking a meal off is the one action that removes
anything, so your assistant asks you to confirm it first; the recipe stays in
your library.

## What it will not do

- It never asks for, or handles, your supermarket login, verification codes
  or payment details.
- It never checks out. Cart links open in your own browser, where you review
  the cart and pay yourself.
- It only compares the stores it lists. Ask which stores are covered.

## Licence

The files in this repository are released under the [MIT licence](LICENSE).
The CartSorted service they connect to has its own
[terms](https://cartsorted.com/terms).

## Help

[Support](https://cartsorted.com/support) ·
[Terms](https://cartsorted.com/terms) ·
[How to connect an assistant](https://cartsorted.com/ai)
