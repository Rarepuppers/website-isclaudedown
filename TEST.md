# isclaudedown.com — one-month SEO test

**Started:** 2026-08-23
**Decision date:** 2026-09-23

## What this is

A clone of `website-isclaudeup` served on the exact-match domain
`isclaudedown.com`, to test whether an exact-match domain can capture the
"is claude down" query family that `isclaudeup.com` currently loses.

## The measured position going in

Same 28-day window, Search Console API, 2026-08-23:

| site | "down" queries | "up" queries |
|---|---|---|
| iscodexup.com | 2,240 impr, 35 clicks, **pos 8.3** | 25 impr, 3 clicks, pos 5.3 |
| isclaudeup.com | 79 impr, **0 clicks, pos 30.9** | 465 impr, 8 clicks, pos 7.0 |

Demand is real and isclaudeup is not capturing it. Specifics:
`is claude down right now` → **position 71.7**; `claude down` → **position 39.1**.

## The case against this test, recorded honestly

Worth writing down so the result is judged against the prediction rather than
re-argued later:

1. **Google already refuses to crawl isclaudeup's own pages.** URL Inspection
   on 2026-08-23 returned `Discovered - currently not indexed`, with **no crawl
   date**, for all 8 subpages. A brand-new domain starts with less authority
   than that, not more.
2. **The domain name is demonstrably not the ranking factor.** iscodexup.com
   ranks position 8.3 for "is codex down" from a domain literally named
   "...up", with 2 of 3 subpages indexed. The two sites are near-identical
   twins — same page count, same word count, isclaudeup has one MORE page — so
   the only real difference is that iscodexup cleared Google's crawl-priority
   threshold.
3. **Duplicate content.** Two sites reporting the same live status of the same
   product is the textbook case for Google picking one and filtering the other,
   and it splits already-thin link equity.

Mitigation applied: title, meta description and OG copy are genuinely
rewritten rather than copied, and the canonical is self-referential (pointing
it at isclaudeup would guarantee it never ranks and make the test meaningless).

## How to judge it on 2026-09-23

Check in Search Console:

- Is `isclaudedown.com` **indexed at all**? If URL Inspection still says
  `Discovered - currently not indexed`, the test has answered itself.
- Impressions/clicks for "down" queries vs isclaudeup's baseline above.
- Has isclaudeup.com **lost** anything? A drop in its "up" query performance
  would mean the two are cannibalising each other, which is the main risk.

**If it has not been indexed, or isclaudeup has regressed: revert.** Point
`isclaudedown.com` back at the 301 in Porkbun, delete the Pages site, and
remove the property from Search Console.

## The verdict inversion — do not break this

This domain asks "Is Claude **down**?", so the answer word is the opposite of
the sibling's for the same real state:

| real state | isclaudeup.com | isclaudedown.com |
|---|---|---|
| Claude up | YES | **NO** |
| Claude degraded | KINDA | KINDA |
| Claude down | NO | **YES** |

It is implemented **entirely** in `config.js` → `copy.*.verdict`. `script.js`
only does `els.verdict.textContent = verdict`, and the mascot art is keyed off
the real state, so the shared JS stays byte-identical across forks as FORK.md
requires.

**If you ever copy `config.js` from the sibling repo, re-apply the swap.** A
status page that prints "YES" while Claude is working tells people it is broken
during the exact minutes they are checking whether it is broken.

Both directions were verified in a browser before launch, against the live
status page and against a simulated major outage.

## Deliberately shared with isclaudeup

Not oversights:

- **Brevo list and the recovery notifier.** Same product, so a subscriber
  wanting "email me when Claude is back" wants the identical email. FORK.md's
  warning about mixing lists is about mixing *products* (Claude vs Codex).
  `badge.html` therefore still points at `isclaudeup-notifier…workers.dev`, and
  no separate worker is deployed for this site.
- **Stripe link.** Same business entity.

## NOT shared

- **Cloudflare Web Analytics token.** Currently the placeholder
  `REPLACE_WITH_ISCLAUDEDOWN_CF_TOKEN`. Reusing isclaudeup's token would file
  this site's traffic against isclaudeup's property and corrupt both datasets.
- **The `utm_source=` tag** on arcade links, now `isclaudedown`, so referrals to
  snackpackuniverse.com can be told apart.
