# Internal note — brand decision needed before WP rebuild

**Send to:** brand / marketing leadership, the website project owner,
optionally the original prototype designer
**From:** Jessica Cheshire
**Decide by:** before any WordPress development hours are committed

---

## Subject lines (pick one)

- "Quick decision needed before WP rebuild — website vs. brand guidelines"
- "Heads up: website prototype doesn't match our brand guide — need a call"
- "Brand decision before website dev starts"

---

## Email / Slack draft

> Hi all —
>
> As we get ready to hand the new website prototype off to the
> WordPress developer, I want to flag a decision that needs to
> happen first. It's small to fix now and expensive to fix later.
>
> **The prototype doesn't match our official brand guidelines.**
>
> | | Sentral brand guide | Website prototype |
> |---|---|---|
> | Palette | Forest, Coral, Cloud, Off-black, Cream | Black, Cream, Gold |
> | Typography | Tobias + Matter (Georgia / Open Sans fallbacks) | Cormorant Garamond + DM Sans |
> | Wordmark | Sen+ral | Sen+ral *(unchanged)* |
>
> The prototype is a *different* visual identity that shares only
> the wordmark and the cream-on-dark contrast direction.
>
> Three paths forward:
>
> 1. **Adopt the prototype's identity for digital.** Officially
>    endorse it as the website's visual variant. Keep brand-guide
>    palette + typography for print, decks, and physical signage.
>    *Implication:* fastest to launch. We end up with two visual
>    identities that need to be reconciled at some point.
>
> 2. **Rebuild the prototype to match our brand guide.** Swap the
>    palette to forest / coral / cloud / off-black, swap fonts to
>    Tobias / Matter. Keep all the structure, copy, and component
>    patterns — those are good.
>    *Implication:* extra design hours before WP starts (probably
>    one week with our designer), but we ship one consistent brand.
>
> 3. **Update the brand guide to match the prototype.** Treat the
>    prototype as the new standard and revise our brand book to
>    match. Marketing, sales, and design assets all migrate over time.
>    *Implication:* biggest change, longest timeline, but resolves the
>    divergence at the source.
>
> My recommendation is **option 2** if our brand guidelines still
> reflect where we want Sentral to sit visually, or **option 3** if
> the brand guide itself is overdue for an update. Option 1 is
> defensible short-term but creates technical and brand debt.
>
> Whichever way we go, the WP developer needs the answer before
> they start building, so the theme is set up correctly from day
> one. Could we get a 15-minute call this week to lock it in?
>
> Reference docs are all in the project repo:
> [github.com/Sentral2026/sentral-website](https://github.com/Sentral2026/sentral-website)
> — see `STYLE_GUIDE.md` §0 for the full side-by-side and
> `ADA_BRIEF.md` for accessibility requirements that apply
> regardless of which option we pick.
>
> Thanks —
> Jess

---

## Slack-shorter version

> Quick one for whoever owns the website redesign / brand —
>
> The prototype we're handing to the WP dev doesn't match our
> official brand guide. Different palette (black/cream/gold vs.
> forest/coral/cloud), different fonts (Cormorant + DM Sans vs.
> Tobias + Matter). Wordmark is the same.
>
> We need to decide before WP build starts:
> 1. Keep the prototype's identity (digital variant of the brand)
> 2. Rebuild prototype to match the brand guide (1 week of design work)
> 3. Update the brand guide to match the prototype
>
> Side-by-side + my recommendation in `STYLE_GUIDE.md` §0:
> https://github.com/Sentral2026/sentral-website/blob/main/STYLE_GUIDE.md
>
> Can we 15-min this week?

---

## Things to bring to the call

- Side-by-side mockup of one page (e.g. Live With Us) in both
  identities, if there's time to put one together. Visual makes the
  decision faster than words.
- A printout of the official brand guide (`reference_sentral_brand.md`)
  so the room can see what option 2 / 3 actually looks like.
- The ADA brief — the accessibility requirements apply regardless,
  so they're not on the table to negotiate.
- A rough estimate from the WP dev for how much extra work option 2
  vs. option 1 represents, if you can get one quickly.

---

## Once a decision is made

Update `STYLE_GUIDE.md` §0 to remove the divergence callout and
record the chosen direction. The WP developer reads this guide as
their canonical reference, so it needs to reflect the final answer,
not the open question.
