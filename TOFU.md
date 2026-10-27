# LittleLink on Tofu

This is Tofu's fork of [LittleLink](https://github.com/sethcottle/littlelink), kept for Tofu's app gallery. A person who
copies it gets their own link page, online at their own Tofu address, which they then change into theirs.

## What it is

A link-in-bio page, in place of Linktree: one simple page with a person's name, a line about them and big buttons to
their other places (a website, an email address, Instagram, YouTube, a shop, a booking page). It is plain HTML and CSS:
no scripts, no accounts, no database and no server. It looks right on a phone, which is where most people open such a
link.

## How Tofu runs it

| | |
|---|---|
| Preset | `static`: a finished site, published as it is |
| Root folder | the repository root (empty) |
| Install, build | none: there is no build step and no package manager |
| Output | the repository itself: `index.html` at the root, `css/`, `fonts/`, `images/`, `privacy.html` |
| Hosting | Tofu's Free static hosting takes it (135 published files of its 500; the repository's Markdown files are not published there) |
| Settings | none (no public settings, no address settings, no owner secrets) |
| Database, migrations | none |
| Accounts, sign-up | none: anyone with the address sees the page, which is the point of it |
| Source link | none needed: LittleLink is MIT-licensed; the page's footer keeps its credit to LittleLink |

## Making it yours

Editing a copy means changing its HTML. The simple way is Tofu's builder: open the app there and say what you want, for
example "Change the name to Mike, the line to 'Baker at the corner bakery', and add a button for my Facebook page". The
builder changes the files and puts the new version online.

By hand, everything is in `index.html`, and a note at its top says where: the name (in `<title>` and `<h1>`), the line
about you, the picture (`images/avatar.svg`, a placeholder outline), and one line per button, whose `href` is where it
goes. `BUTTONS.md` lists every button LittleLink comes with (119 styles, Facebook to Zoom), each as one line to copy in.
`class="theme-auto"` on the `<html>` line follows the phone's light or dark setting; `theme-light` and `theme-dark` fix
it.

## What we changed, and why

Each change is one commit on `tofu`, with its reason in the message.

1. **Removed other hosts' deploy files** (`docker/`, `wrangler.toml`, `.assetsignore`, `.dockerignore`, `.do/`). Tofu's
   static hosting serves files by their type, and `docker/Dockerfile`, `docker/compose.yaml` and `wrangler.toml` have
   types it does not serve, so with them the repository was not a site it could publish (Tofu's own check said so:
   `refused: typex3`). The others only configure Docker, Cloudflare Workers and DigitalOcean.
2. **A short starter page in place of the button catalogue.** Upstream's `index.html` is a catalogue of all 120 buttons
   (nearly all linking to `#`, one for an 18+ site), which a copy would have published as its owner's page. A copy now
   starts as a plain link page: "Your Name", a line about you, a placeholder picture (`images/avatar.svg`, a drawn
   outline, not a person) and five buttons (a website and an email address at `example.com`, and the home pages of
   Instagram, YouTube and TikTok). The canonical link to `https://yourwebsite.com` is gone: it told search engines that
   the page lives somewhere else, and a finished page cannot know its copy's address. The catalogue moved to
   `BUTTONS.md`, generated from upstream's page, so nothing a person picks buttons from is lost; `css/brands.css` and
   `images/icons/` are unchanged.
3. **Buttons fit narrow phones; Tofu's footer centred** (`css/style.css`). Buttons are 300px wide inside 24px of padding,
   so a 320px phone scrolled sideways by 4px; `max-width:100%` keeps them inside the page (measured: 324px wide before,
   320px after). On the Free plan Tofu adds a small "Shipped live by Tofu." footer to each page; it is centred like the
   page instead of sitting at the left edge.
4. **A privacy page that is true for a copy** (`privacy.html`). Upstream's is an example to fill in ("Example LLC" for
   analytics, embedded content and hosting). A copy has no analytics, no cookies and no scripts and is hosted by Tofu, so
   the page says that, links Tofu's privacy policy, and keeps a note telling the owner what to add if they add something
   that collects information.
5. **This file**, and a note at the top of `README.md` pointing here.

## What works differently on Tofu

- The page changes only when its files change and a new version goes online (through Tofu's builder, or an upload).
  There is no editing screen inside the page itself, as upstream has none.
- Nothing runs on a schedule and nothing sends email: LittleLink has neither.
- On the Free plan, each page ends with Tofu's small "Shipped live by Tofu." line.
- The page sets no cookies and runs no scripts. On Tofu's static hosting, the dashboard counts visits from the hosting's
  own records, with nothing added to the page. If the owner turns on Tofu's site analytics line, or adds anything that
  collects information, `privacy.html` should say so.
- There is no canonical link. Once the owner connects their own domain they can add one in `index.html`'s `<head>`.

## Upstream, licence and caveats

- Upstream: <https://github.com/sethcottle/littlelink>, branch `main`, commit
  `3c9162c9f8324d5e002259ed8b4b3cc377b08486` (2026-07-29, "Remove Kit"), LittleLink v3.11.0. The fork's `main` follows
  upstream untouched; `tofu` holds the commits above.
- Licence: MIT, `LICENSE.md` (Copyright 2019-2024 Seth Cottle), kept exactly; `css/style.css` and `css/brands.css` keep
  their MIT headers.
- The brand icons in `images/icons/` and the brand colours in `css/brands.css` are other companies' logos and trademarks
  (Instagram, YouTube, TikTok and the rest). Upstream includes them so a button can point at a person's page on that
  service; the MIT licence covers LittleLink's own work, not those marks.
- The font, Open Sans (`fonts/`, four weights), is Google Fonts' and published there under the SIL Open Font License
  1.1. Upstream ships the font files without a copy of that licence, and so does this fork.
- `images/avatar.png` (LittleLink's logo, upstream's) stays as the page's tab icon until the owner changes it.

## Verification

Run on 2026-10-27 (UTC) on the `tofu` branch, on a Mac, with nothing installed:

- **File modes (rule 1).** `git archive tofu | tar -x -C <dir>`, then `find <dir> -type f -exec chmod a-x {} +`: every
  file in the repository is already mode 100644, and nothing is executed (there is no build, no script, no container).
  The site was served from that tree.
- **Served locally.** `python3 -I -m http.server <port> --bind 127.0.0.1 --directory <dir>`: `/`, `/privacy.html`, the
  three stylesheets, the four fonts, the avatar and the icons the page uses all answered 200.
- **In a browser.** Headless Chromium (Playwright) at 320×640 and 390×844 (phone, touch) and 1280×800, light and dark,
  on `/` and `/privacy.html`: no page errors, no console errors, no failed requests, every image and font loaded,
  nothing wider than the screen (page width 320 at 320px; 324 with the button fix taken out), and the same with Tofu's
  footer added the way Tofu adds it (centred, inside the screen).
- **Live check, locally.** `live-check.py` (kept with the gallery's round 10 notes) against the local server:
  `RESULT PASS`.
- **Tofu's own check.** `lane_check_r9.mts` (Tofu's GitHub import, the static lane's rules and its safety check) on the
  pinned tag: the safety check passes, it is a finished site for the static lane, and it publishes 135 of the Free
  static hosting's 500 files.
- **Not applicable here:** port and user (no container), database pooler hosts, start races and the libpq certificate
  (no database), secrets (none), and files Tofu's upload refuses (no `.env`, `.npmrc`, key or credential file in the
  repository).

## What only a real deploy can verify

That a copy made from the gallery goes online on Tofu's Free static hosting as a finished site; the content types that
host sends for the fonts and icons; that Tofu's footer lands centred at the bottom of both pages; and `live-check.py`
against the copy's real address.
