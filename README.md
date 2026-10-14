# Dice Roller Casino

An incremental dice game. Roll for points, buy more dice and rollers, then open
dice cups, craps tables and a high roller lounge that keep earning while you
watch.

**Play it:** https://cyraaryc.github.io/CMPM121-D1-/

## How to play

- Click the 🎲 to roll all your dice. You get points equal to the total, and
  you'll hear a dice throw.
- **add dice** adds another die to every roll.
- **add dice roller** hires a roller who rolls all your dice once a second.
- The shop below the dice has upgrades that earn points every second:
  - **dice cup**: +5 points/sec
  - **craps table**: +25 points/sec
  - **high roller lounge**: +120 points/sec
- Stronger upgrades stay locked (???) until you've held enough points at once.
- Anything you can't afford is faded out, and shop buttons show how many more
  points you need. Each purchase makes the next one of that kind cost more.
- The line under your points shows your dice, rollers and about how many points
  you're making per second.

## Running it

You need [Deno](https://docs.deno.com/runtime/getting_started/installation/).

```sh
deno task setup    # once per clone, runs the checks before every commit
deno task dev      # dev server at http://localhost:3000
deno task ci       # format check, lint, type check and production build
deno task preview  # serve the production build
```

Pushing to `main` builds and deploys to GitHub Pages through
`.github/workflows/deploy.yml`.

## Where things are

- `src/main.ts`: the dice button, add dice and roller buttons, dice sound, the
  game loop, stats line, purchase messages and the casino theme.
- `src/upgrades.ts`: the shop upgrades as plain data (name, description, cost,
  cost growth, points/sec, unlock amount).
- `src/shop.ts`: shared code that unlocks, buys, earns income for and draws a
  button for every entry in `upgrades.ts`.
- `src/assets/`: the logo (`tile.svg`) and dice sounds, imported through Vite
  so they get the right URL on GitHub Pages.

**Adding an upgrade:** add one object to the `upgrades` array in
`src/upgrades.ts`. Nothing else needs to change. The high roller lounge was
added this way.

**Timing:** the game loop in `main.ts` measures real seconds between frames
and passes them to the shop, so income doesn't depend on frame rate.

## Design notes

The shop is split into three parts, each with one job:

- `upgrades.ts` only describes what can be bought.
- `shop.ts` only handles the rules for buying, unlocking and earning.
- `main.ts` only connects the shop to the page through a small wallet (get,
  spend, earn).

Because of that, adding or rebalancing an upgrade is a data change, and the
shop rules can change without touching the page code.

Relevant commits:

- Upgrade shop and shared model: ADD COMMIT LINK
- High roller lounge added through the data: ADD COMMIT LINK

## Credits

Tooling adapted from the
[CMPM 121 Fall 2026 S01 starter](https://github.com/ucsc-cmpm-121-f26/s01-starter/tree/8974888efb6537be82e8eb069db139ea885039da),
which has no explicit repository license. No new license is assigned to that
material. The D1 scaffold and original SVG were authored with OpenAI GPT-6
Astra assistance. The upgrade shop, elapsed-time loop, purchase feedback,
unlocks, casino theme and this README were written with Claude (Anthropic)
assistance. Dice sound effects: ADD SOURCE. Dependencies retain their
respective licenses.

Docs used:
[MDN requestAnimationFrame](https://developer.mozilla.org/en-US/docs/Web/API/Window/requestAnimationFrame),
[Vite static asset handling](https://vite.dev/guide/assets).