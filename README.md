# MT App Launcher

One page that lists every app and sub-page across your GitHub Pages sites. It reads your repos from GitHub each time it refreshes, so new pages (like a Recipe Box player or an LE version of the calendar) show up without you adding anything.

## Setup (about 5 minutes)

1. On GitHub, create a new **public** repo named `launcher`.
2. Upload `index.html` (and `links.json` if you want extra manual links).
3. Go to **Settings → Pages**. Under "Build and deployment", pick **Deploy from a branch**, branch **main**, folder **/ (root)**, then **Save**.
4. Wait a minute or two, then open **https://mountytechwv.github.io/launcher/**
5. Bookmark it. On your phone, use "Add to Home Screen" so it works like an app.

## How it finds links

- Every public repo with GitHub Pages turned on gets a card.
- Every `.html` file in that site becomes a clickable link. `index.html` files become folder links (for example `player/index.html` → `.../player/`).
- If a page has a `<title>`, that's used as its name. Otherwise the file name is cleaned up (`le-calendar.html` → "Le Calendar").
- If a repo's "Website" field (the gear icon next to About on the repo page) points somewhere else, that link is shown too.
- Repos with no website are listed at the bottom so you can still get to them.

**Tip:** give each page a clear `<title>` and the launcher will show nice names automatically, like "MT Economic Calendar – LE Trading Academy".

## Settings

At the top of the script in `index.html` there's a `CONFIG` block:

- `hideRepos` – repo names to leave out
- `hideFiles` – pages to leave out (test pages, 404 page, etc.)
- `cacheMinutes` – how often it re-checks GitHub (default 30)
- `showOtherRepos` – show or hide the "Repos without a website" list

## Apps hosted on Vercel (or anywhere outside GitHub Pages)

GitHub doesn't know the web addresses of your Vercel sites, so the launcher can't find those on its own. To add them, put a `launcher.json` file in the root of **that app's repo** (not this one):

```json
{
  "name": "MT Economic Calendar",
  "links": [
    { "name": "LE Trading Academy version", "url": "https://....vercel.app", "note": "short note" },
    { "name": "Kingdom Traders version", "url": "https://....vercel.app" }
  ],
  "hide": ["test.html"]
}
```

- `name` (optional) – the card title
- `links` – extra links shown on that app's card. Add `"main": true` to tag one as the main link.
- `hide` (optional) – page paths to leave off the card

The launcher reads this file automatically. You don't need to change anything in the launcher repo.

Also set each repo's **Website** field (gear icon next to "About" on the repo page) to its main Vercel URL. The launcher picks that up as well.

## Extra links (optional)

For anything that doesn't belong to one of your repos, add it to `links.json` in this launcher repo:

```json
[
  { "group": "Discord tools", "name": "Something", "url": "https://...", "note": "short note" }
]
```

## Good to know

- It only sees **public** repos. Put links for private ones in `links.json`.
- GitHub allows about 60 checks an hour without logging in. The page saves the results and only re-checks every 30 minutes, so you won't hit that in normal use. If you do, it shows the saved list and tells you when it'll work again.
- Press `/` to jump to the search box.
