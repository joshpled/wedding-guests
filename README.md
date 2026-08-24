# Wedding — guest pages

Public pages for guests. GitHub Pages, repo `wedding-guests`.

| File | URL | Who gets it |
|---|---|---|
| `index.html` | `/` | **Everyone.** Sunday only. |
| `weekend-21bb7a5b.html` | `/weekend-21bb7a5b.html` | **Family and wedding party.** Both days, including Saturday. |

## Why the odd filename

Some guests are not invited to Saturday. The family page must not be
discoverable by anyone who was not sent the link.

Three rules make that work, and all three matter:

1. **Neither page links to the other.** No menu, no landing page, no
   "see also". Verified: zero internal links in either file.
2. **The root is the ordinary guest page.** Someone who trims
   `/weekend-21bb7a5b.html` back to `/` lands on the normal Sunday page and learns
   nothing. There is no directory listing on GitHub Pages.
3. **The family filename is random.** Not `family.html` or `weekend.html`,
   which anyone could guess.

Both pages carry `noindex, nofollow` so neither appears in search.

The Saturday page also avoids language implying other guests got less. It reads
as "both days in one place", not "you got the special version".

**Do not add a link between these pages, a nav bar, or a sitemap.**

## Sending the links

- Everyone: `https://joshpled.github.io/wedding-guests/`
- Family and wedding party: `https://joshpled.github.io/wedding-guests/weekend-21bb7a5b.html`

Send the second one privately — text or individual email, never a group thread
that includes people not invited to Saturday.

## Why this is a separate repository

The planning dashboard is in `wedding-dashboard` and carries a Supabase key
granting read and write on vendor contracts, payment history and every guest's
contact details. Anything published beside it is one URL edit from that key.

**Nothing that touches money or the guest table may be added here.**

## How the form works

No Supabase key in this repo. The dietary form posts to an Edge Function:

```
https://ixtynzfktgfwjwlvdaho.supabase.co/functions/v1/guest-info
```

- **lookup** — replies with which fields are *missing*. Never returns a stored
  email or phone, so it cannot harvest contact details.
- **submit** — writes to a `guest_submissions` review queue. Nothing reaches
  the guest list until approved on the dashboard.

## Deploy

```bash
git init && git add . && git commit -m "guest pages"
git branch -M main
git remote add origin git@github.com:joshpled/wedding-guests.git
git push -u origin main
```

Settings → Pages → Source `main` / root.

## Before sharing

1. Open `/` — confirm it is the Sunday page and links nowhere.
2. Open `/weekend-21bb7a5b.html` — confirm it loads and links nowhere.
3. Submit the form as yourself; check it lands on the dashboard.
4. `grep -r "sb_publishable\|createClient" .` should return nothing.
