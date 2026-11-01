# PossumStock Post-Festival Site: Handoff for Claude Code

Date: 2026-11-01 · Repo: `heykara/possumstock` · Netlify project: `possumstock` · Event: PossumStock 2026 (Aug 20-22, Red River Canoe, Adams TN)

## Goal
The festival is over. `index.html` was replaced with a single-page **thank-you / farewell** page. The owner is **building a new site off this design**, so keep the page clean and reusable.

## Branch and deploy
- Work branch: `post-festival-thank-you` (cut from `main`). `main` still holds the old pre-event site (schedule, tickets, countdown, guide-app links). Don't delete it; the old `CFG` and `FESTIVAL_SCHEDULE` data live there.
- Static site, **no build step, no dependencies, no package.json** (see README.TXT §2). Netlify publishes the repo root.
- Check the branch deploy preview before merging to `main`.

## Files changed
- `index.html`: new thank-you page. All CSS and JS inline. No JS libraries.
- `privacy.html`: section 6 now discloses the Instagram clips (loaded only on tap). LAST UPDATED set to November 1, 2026.
- Unchanged: `privacy.css`, `netlify.toml`, `assets/`.
- Do not add `thanks.html`. It was a duplicate draft.

## Page structure
1. Nav (logo link + Instagram button) and a dark marquee. The marquee reads "POSSUMS NEVER DIE" three times, then "NEWS DROPPING SOON".
2. Hero: logo, a possum playing dead, "THANK YOU, SWAMP", and the festival dates.
3. Three stages: thank-you blurbs for Sun Daddy, Eclipse Flow Lounge, and Moon Mama.
4. **FROM THE FIELD**: shows one random taped photo card from a random category (Possum Camp, Photo Stock, Videos). Category chips switch; SEE MORE reveals the rest. Lineup has day-jump pills, and acts become links when added to `LINKS`.
5. **WHO MADE IT HAPPEN**: credits for sound, production, behind the scenes, other crew, partners, and photos.
6. Closing card: "POSSUMS NEVER DIE." with a "SOCIAL BATTERY: CHARGING" badge and an Instagram follow button.
7. Footer with a privacy link.

## Voice and tone (from the organizer's own post, @el.parson)
Loud, grateful, informal, emoji-heavy (😭🥹🫶😈😘). Signature lines: "POSSUMS NEVER DIE", "POSSUMS DON'T HIBERNATE" (replaces the retired "THE SEASON AIN'T OVER"), "more news coming at you with the quickness". Claims used: biggest PossumStock to date and the wildest weekend in years. Keep the energy and avoid corporate language. Don't repeat the organizer's text word for word.

## Decisions already made
- **PLUR removed** from this page. Do not reintroduce the PLUR block or the "PEACE · LOVE · UNITY · RESPECT" line.
- **Skydaddy removed** from credits and partners. (It may still appear in the old `main` site.)
- The organizer's @el.parson post is no longer embedded; its thank-you text stays in the hero.
- No date for next year is announced. Do not invent one.
- The "Back to the event site" button was removed because the page is now the site.

## Credits (exact handles, as given by the organizer)
- Sound: @lawton_vierkant, @joe_tellmann_isdacoolest
- Production: @consolidatedpresents
- Behind the scenes: @flameseductress, @_karacomputer, @matt.tv
- Other crew: @william_wilyard, @randallsaez, @yonky_mcyonkerface, @fancy_boy_clothes, @malfunktion365, @aberration7_77, @msdawnmarie, @glitter.d3mons, @flame_therapist
- Photos: @lane.c.media
- Partners row (not from the post): Red River Canoe & Campground, Moon Rooster, Revel House Pub & Eatery, &Vinyl Records, Ville Magazine (@villerockandroll), Kim Cargill Realty (realtor.com page), food vendors

## Instagram clips: how they work
- Data is the `CLIPS` object at the bottom of `index.html`: `stage: [type, shortcode, creditHandle?]`, where type is `reel` or `p`.
- Nothing from Instagram loads until a visitor taps **TAP TO PLAY** (or TAP TO READ) on that card. Then that card's iframe at `https://www.instagram.com/{type}/{code}/embed/` is created. Several cards can be loaded, each only by its own tap.
- Each card has an "Open ↗" link and a "📸 @handle" credit when a handle is set. Cards pop in on scroll (IntersectionObserver, skipped for reduced motion).
- Categories in `CLIPS`: camp (1), photo (5), video (5). Posts were sorted by type: `p` posts -> Photo Stock, reels -> Videos. Move an entry between arrays to recategorize.
- Carousel posts always open on slide 1. The original `img_index` slides can't be preserved.
- If a post turns out to be private or deleted, remove its entry from `CLIPS`.

## Open items
1. **Clip credits:** the third item in each `CLIPS` entry (posting account handle, no `@`) is empty for all but the Possum Camp post. The page already renders "📸 @handle" under a clip when the handle exists. The owner still needs to supply the accounts. Some posts may be collabs with two handles, which the current code doesn't support.
2. **Test every embed** on the deployed preview. The embeds were not tested (Instagram blocks automated access, so none of the posts were ever opened).
3. **Open Graph URLs:** the three OG/canonical URLs assume `https://possumstock.netlify.app/`. Update them if a custom domain is used (a PossumStock custom domain became available in Oct 2026).
4. **README.TXT is stale.** Sections 1, 3, 4, 8, and 9 describe the old site (CFG, FESTIVAL_SCHEDULE, four-place gate-time rule, guide-app links). Rewrite it once the new site's shape is known. Add a note that Instagram embeds load only on tap, which is an exception to "no external requests beyond Google Fonts".
5. **privacy.html §6** still has a paragraph about the on-site guide app, which this page no longer links to. Delete it or restore the link.
6. A site map image (`assets/festival-map.jpg`) was a TODO for the separate guide project. It is not needed here.

## Rules to keep (from README.TXT, still valid)
- Zero build, zero dependencies, hand-editable from a phone.
- Only the existing CSS custom properties in `:root`. Don't introduce new hex values.
- Fonts via the Google Fonts `@import` only.
- Tap targets of at least 44px, `prefers-reduced-motion` respected, text contrast of at least 4.5:1 (use the `-dark` variants for text on white), skip link kept.
- **If anything that collects personal data is ever added** (mailing list, SMS, push, analytics IDs, or more third-party embeds), update `privacy.html` and its LAST UPDATED line before it ships.
- Don't touch the guide project (`possumstock2026.netlify.app`). It is a separate repo and Netlify project.

## Retention ideas (not built)
A concrete save-the-date once there is a date; an optional feedback link; a self-hosted photo/video gallery to replace Instagram embeds (works offline, fewer privacy concerns); and email/SMS signup (needs a privacy update first).

## Aesthetic pass 2 (latest)
- Hero possum is now `assets/custom/possum-heart-paws.svg` (upright, heart paws, `possumBop` animation). The upside-down "playing dead" possum and its `flop` CSS are gone.
- The organizer post has no label sticker (`CATS.org.label` is empty); the embedded card stays and is still tap-to-load.
- THREE STAGES section removed.
- Lineup is folded behind a **SEE THE LINEUP ▼** button (`#lineupToggle` / `#lineupBody`). The nav LINEUP link, `#lineup` and `#day-*` hashes open it.
- WHO MADE IT HAPPEN is one wall of band-style name blocks built from the `CREW` array (name + Instagram handle or https link). Joe Tellmann, Consolidated Presents, Kody Luck, kara computer and Matt TV have names; the rest still show handles until real names are supplied. Partners are folded into the same wall.
- Shared helpers: `styleBlock`, `nameBlock`, `igUrl`.

## Aesthetic pass 3
- All section cards and the hero title card now use the closing-card look: ink background, slime border and offset shadow, white text. Inner stickers stay white with ink text; their shadows use `var(--sh)` (slime inside cards).
- Ticker and OG description no longer say "THE SEASON AIN'T OVER" (ticker now says POSSUMS DON'T HIBERNATE).
- Partner links confirmed: Kim Cargill Realty -> realtor.com agent page; Ville Magazine -> instagram.com/villerockandroll.
- Lineup stays click-to-expand. No A-Z finder.
- `.day-jump` is a `<nav>`, so it carries explicit resets so the global `nav` styles don't leak onto it.

## Aesthetic pass 4
- Removed the @el.parson Instagram embed (code, `org` category, CSS). Hero text stays. Only FROM THE FIELD loads Instagram, and only on tap.
