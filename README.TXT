POSSUMSTOCK 2026 - PROMOTIONAL SITE
Development Standards
Last updated: August 21, 2026


1. WHAT THIS REPO IS
--------------------
The public promotional site for Possumstock 2026, deployed on Netlify as the
project "possumstock". It is a static site with no build step and no
dependencies. netlify.toml publishes the repository root as-is.

Files:
  index.html      The entire site. Markup, CSS, and JS are all inline.
  privacy.html    Privacy policy.
  privacy.css     Stylesheet for privacy.html only.
  assets/         Logo, custom SVG artwork, Twemoji PNGs.
  netlify.toml    Two lines: publish = "."

The separate on-site attendee guide (offline PWA, favorites, reminders) is NOT
in this repo. It lives in its own repository and its own Netlify project, so
that this site stays stable and unaffected by the guide's service worker,
functions, and deploy lifecycle.

The guide is live at https://possumstock2026.netlify.app/ and this site now
links out to it from five places: the nav ("APP", the first item), the
announcement bar, the GET THE APP section, the festival-app FAQ answer, and the
footer. Every one of those links reads CFG.appUrl at runtime, so the URL is
changed in one place.

GET THE APP is the first section on the page, sitting directly below the hero
countdown and above the hero-to-lineup divider. Nav order follows page order:
APP, LINEUP, SCHEDULE, TICKETS, FAQ, LOCATION. If sections are reordered, move
the nav item to match.

A PWA cannot be installed from another origin. No button on this site can
trigger the install prompt for the guide, because browsers only offer
"Install" / "Add to Home Screen" while the visitor is on the guide's own
address. The GET THE APP section therefore sends people to the app and spells
out the per-platform save steps. Do not replace it with a download button or
an app-store badge; there is no package to download and no store listing.


2. NO BUILD STEP, NO DEPENDENCIES
---------------------------------
Do not add a package.json, a bundler, a framework, or an npm dependency to this
repo. The site is edited by hand and deployed by file copy. This is deliberate:
it keeps deploys instant, eliminates install failures during the festival
weekend, and means anyone can fix a typo from a phone.

Do not run build commands. There is nothing to build.


3. SINGLE SOURCE OF TRUTH FOR EVENT DATA
----------------------------------------
Two structures in index.html hold all event data. Edit these, not the rendered
markup.

  CFG (index.html ~line 2320)
    Festival name, tagline, dates, gates-open countdown target, venue, address,
    maps link, Instagram, guide app URL, day labels, and ticket tiers. The
    ticket cards are rendered from CFG.tickets by the ticket renderer
    immediately below it, and every link carrying the data-app-link attribute
    is pointed at CFG.appUrl by the app-links block.

  FESTIVAL_SCHEDULE (index.html ~line 2590)
    Three days, each with three stages, each with an ordered array of
    [time, artist] pairs. The lineup section and the schedule tabs are both
    rendered from this array by renderFestivalSchedule().

Never hand-edit a rendered schedule row or ticket perk in the markup. It will
be overwritten on the next render and will drift from the data.


4. TIME AND DATE RULES
----------------------
Event timezone is America/Chicago (UTC-05:00 in August).

Confirmed times, which must agree everywhere they appear:
  Gates open   Thursday, August 20, 2026 at 2:00 PM
  First music  Thursday, August 20, 2026 at 3:00 PM
  Festival     August 20-22, 2026

Gate time currently appears in four places. If it changes, change all four:
  - CFG.gatesOpen (the countdown target)
  - The first entry of CFG.tickets[0].perks
  - The Thursday line in the "at a glance" list (~line 1900)
  - The gates FAQ answer (~line 1960)

Times in FESTIVAL_SCHEDULE are bare display strings with no AM/PM. They are
listed in program order per stage and are read by humans in context. Two rules
govern them:

  a) Within one stage's list, times always move forward. When the clock value
     drops, the day has rolled over - either morning into afternoon, or evening
     into the following calendar day.

  b) A set that starts after midnight still belongs to the program day it is
     listed under. Friday's Sun Daddy list runs from 1:30 PM Friday through
     5:00 AM Saturday, and every one of those sets is part of Friday's program.
     Do not move late sets to the next day's tab.

Some artists appear more than once across the weekend (BUCK, Flowstar w Flame,
Fire Safety w Peach). This is intentional. Do not deduplicate them.


5. STYLE AND ASSETS
-------------------
Design tokens are the CSS custom properties in the :root block at the top of
index.html (~line 11). Use them. Do not introduce new hex values.

  --ink #1a1228        --slime #8ee820      --slime-dark #5aaa00
  --slime-mid #72c010  --cyan #00c8e8       --cyan-dark #0090b8
  --pink #f07898       --pink-dark #d04868  --yellow #f8d800
  --yellow-dark #c8a800  --orange #f07820   --purple #9858c8
  --bg-top #dbb8e8     --bg-mid #c4a8e0     --bg-bot #8ec8f0

Visual language: thick ink borders, hard offset shadows, slight card rotations,
dashed dividers, sticker-collage layering.

Fonts are loaded from Google Fonts via @import at the top of index.html. That
is acceptable for this promotional site because it is always used with signal.
It is NOT acceptable in the guide project, which must work offline.

Artwork lives in assets/custom/ as standalone SVGs, catalogued in
assets/custom/README.md. Reuse those files rather than drawing new ones or
re-embedding SVG inline.

Emoji graphics in assets/emojis/ are Twemoji, licensed CC BY 4.0. The
attribution in assets/emojis/ATTRIBUTION.md must stay with them wherever they
are used or copied.

assets/possumstock-2026-logo.png is roughly 1.9 MB and is not square. Compress
and crop it before reusing it as an icon anywhere.


6. ACCESSIBILITY
----------------
  - Tap targets at least 44px.
  - Body text contrast at least 4.5:1. The pink and yellow tokens fail on
    white; use the -dark variants for text.
  - Honor prefers-reduced-motion: no ticker scroll, no card transitions.
  - Keep the skip link, aria-current on nav, and tabpanel semantics intact.
  - Assume outdoor daylight viewing. No light gray on white.


7. PRIVACY
----------
The site collects no personal information. privacy.html describes only the
standard technical data that hosting providers receive.

If anything is ever added that collects personal data - a mailing list, SMS
signup, push notification subscription, or analytics with identifiers -
privacy.html must be updated to disclose what is collected, why, how long it
is kept, who it is shared with, and how to opt out. Update the LAST UPDATED
line when you do. Do this before the collection ships, not after.


8. BEFORE YOU DEPLOY
--------------------
  - Confirm gate and music times agree in all four places listed in section 4.
  - Confirm the countdown targets the correct date and time.
  - Check the lineup and schedule tabs render every day and stage.
  - Check the layout at 320px wide.
  - Confirm no external request has been added beyond the existing Google
    Fonts import. Links to the guide app are navigation, not requests, and are
    fine; a QR-code image built by a third-party API would not be.
  - Confirm the guide app link still resolves and that the install steps match
    what current iOS Safari and Android Chrome actually show.
  - Do not commit build artifacts, node_modules, or editor files.


9. TODO
-------
  - No site map image exists in assets/. The guide project needs one at
    assets/festival-map.jpg. Add it when artwork is ready.
  - star-doodle.svg lives at assets/custom/star-doodle.svg, not assets/. Any
    reference to the shorter path is wrong.
