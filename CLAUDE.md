# PossumStock Site

Static site for PossumStock (camping music festival, Red River Canoe, Adams TN). The 2026 event (Aug 20-22) is over. `index.html` is a post-festival thank-you page, and the owner is building a new site off its design. See `HANDOFF-post-festival-site.md` for full context and open items.

## Stack and workflow
- Plain HTML/CSS/JS. **No build step, no dependencies, no package.json.** Netlify publishes the repo root.
- CSS and JS stay inline in each page. Edit by hand; keep it phone-editable.
- Work on a branch (`post-festival-thank-you`), never push straight to `main`. Check the Netlify branch deploy before merging.
- Preview locally with `python3 -m http.server 8000`.

## Design rules
- Use only the CSS custom properties in `:root`. Do not add new hex values.
- Fonts come from the Google Fonts `@import` only.
- Tap targets of at least 44px, `prefers-reduced-motion` respected, text contrast of at least 4.5:1 (use the `-dark` variants on white), keep the skip link and visible focus.
- Every section card uses the dark ink + slime-border look (same as the closing card). White sticker items sit on top. Keep the sticker and collage look: thick ink borders, offset shadows, slightly rotated cards.

## Voice
Loud, grateful, informal, emoji-friendly (😭🥹🫶😈😘). Signature lines: "POSSUMS NEVER DIE", "POSSUMS DON'T HIBERNATE". Don't use "THE SEASON AIN'T OVER" (retired). No corporate language. Never invent dates or announcements. There is no 2027 date yet.

## Hard rules
- **Privacy:** if anything that collects personal data is added (mailing list, SMS, push, analytics, or more third-party embeds), update `privacy.html` and its LAST UPDATED line *before* it ships.
- **Instagram posts** (the organizer post under the heading, and FROM THE FIELD) load from Instagram only after a visitor taps that card's own button. Never load on scroll or page load, and don't add the Instagram embed script.
- **Removed on purpose:** PLUR copy and the Skydaddy credit. Do not reintroduce them.
- **Credits:** use handles exactly as given. Photos by @lane.c.media. Don't alter the `CREW` list in `index.html` without being asked.
- Don't edit the separate guide-app project (`possumstock2026.netlify.app`).

## Open items
1. Fill in the posting account handle (3rd item) for each `CLIPS` entry in `index.html` (only the Possum Camp post has one).
   Lineup links: add acts to `LINKS` (act name -> Instagram handle) and they become tappable. Partner links are confirmed (Kim Cargill -> realtor.com page, Ville -> @villerockandroll). Handle-only `CREW` blocks still need real names.
2. Test every embed on the deploy preview.
3. Update the three Open Graph and canonical URLs if a custom domain is used.
4. Rewrite `README.TXT` (it still describes the old pre-event site) and delete the guide-app paragraph in `privacy.html` §6 if the homepage still doesn't link to it.
