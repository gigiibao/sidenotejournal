# side note

Static website (plain HTML/CSS/JS, no build step).

## Deploy to Netlify
**Drag and drop:** go to https://app.netlify.com/drop and drop this folder (or the unzipped project).
**From Git:** push this folder to a repo, then "Add new site → Import an existing project". Leave the build command empty and set the publish directory to `.` (netlify.toml already does this).

## Structure
- `index.html` — the whole site. Every page is a `<div class="page" data-page="...">` section, and the router at the bottom of the file shows one of them based on the URL path (`/`, `/blog`, `/blog/<article>`, `/our-story`, `/resources`, `/student-voices`).
- `404.html` — shown for any address that is not a real route
- `sitemap.xml` — the list of pages given to search engines
- `assets/img/` — illustrations and the dog cursor
- `netlify.toml` — publish settings, route list, cache and security headers
- `favicon.svg`, `robots.txt`

## Routing
The site uses real paths, not `#` fragments. That matters for Google: anything after a `#` is discarded by search engines, so hash routes are invisible to them.

Links are written as ordinary paths (`<a href="/blog">`). The router intercepts clicks so navigation stays instant, and falls back to a normal page load if JavaScript is off. Old `#` links such as `/#/blog` still work — they are rewritten to `/blog` on arrival.

## Adding a page
Five places, all required:

1. A new `<div class="page" data-page="your-page" hidden>` section in `index.html`
2. An entry in the `M` map (for a top-level page) in the router script, so the path resolves
3. An entry in the `META` map in the same script, giving the page its `<title>` and description
4. A `[[redirects]]` block in `netlify.toml`, above the catch-all `/*` rule
5. A `<url>` entry in `sitemap.xml`

Skipping step 4 makes the page return 404 in production. Skipping step 3 leaves it with the home page's title.

## Fonts
Inter Tight and Gaegu load from Google Fonts (see the `<link>` in `index.html`). The main text falls back to Helvetica Neue / Helvetica / Arial, and the handwriting to a cursive font, if they can't load.

## Language and settings
English / Portuguese / Chinese is built in. The chosen language is remembered in the browser (`localStorage`). Page titles and descriptions are in English, which is what the `<html lang="en">` default declares and what search engines index.
