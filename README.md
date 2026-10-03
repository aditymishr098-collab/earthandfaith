# Earth and Faith

**A personal blog about religion, science, history and the big questions.**
Live at **[earthandfaith.online](https://earthandfaith.online)**

> Questions are never dangerous. I question ideas, never people.

Earth and Faith is written by **Adity Mishra**. The goal is simple: to help people understand each other better, and to say no to violence, discrimination and hatred. Care for nature and honesty over show are part of the same idea.

---

## What is on the site

| Section | What it is |
|---|---|
| **Posts** | Explainers and stories on religion, atheism, ancient civilizations, space and science |
| **Topics** | Every post, grouped by topic |
| **Timeline** | A timeline of the world's religions |
| **Religion Map** | An interactive world map showing which faith group leads in each country |
| **Hardship Map** | A live map of hunger, extreme poverty, conflict deaths, homicide and violence against women (World Bank data) |
| **Civilizations Map** | A slider through time, showing which civilizations were active when |
| **Lost Faiths** | Religions that faded, and a few ancient ones that survive as very small communities |
| **Compare Religions** | Pick two religions and see them side by side |
| **On This Day** | A science, space and history event for every day of the year |
| **Ask, Contact, About, Privacy Policy, Disclaimer** | Support pages |

The site works as an installable app (PWA), can be read offline after a visit, and has a language menu that uses Google Translate.

## Principles

- **No tracking.** No analytics, no ads, no cookies of its own. See the [Privacy Policy](https://earthandfaith.online/privacy-policy/).
- **Ideas, not people.** Respect for every reader, believer or not.
- **Sources and corrections.** Mistakes get fixed, and readers can write in.
- **Open about AI.** AI tools help with research, drafting and building this site, and the author is responsible for what is published.

## How it is built

- Static site on **GitHub Pages**, built with **Jekyll** and the `minima` theme, with custom layout, header, footer and styles.
- Plain **HTML, CSS and JavaScript**. No framework and no database.
- Maps use **D3** (from cdnjs) and country outlines from `world.geo.json` (from jsDelivr).
- Hardship data is loaded live from the **World Bank Open Data API**.
- Religion figures are rounded estimates based on **Pew Research Center** data.
- Domain: `earthandfaith.online` (DNS at Hostinger, pointing to GitHub Pages).

## Folder guide

```
.
├── _config.yml            Site settings (title, plugins)
├── _includes/
│   ├── head.html          Page head (icons, SEO, styles)
│   ├── header.html        Green top bar, menus and language button
│   └── footer.html        Footer links, reading progress bar, table of contents, print button
├── _layouts/              Page layouts
├── _posts/                All blog posts (YYYY-MM-DD-title.md)
├── assets/main.scss       Site styles
├── index.html             Home page (map cards, search, post list)
├── about.md, ask.md, contact.md, privacy-policy.md, disclaimer.md
├── timeline.md, topics.md
├── religion-map.md, world-hardship-map.md
├── civilizations-map.html, lost-faiths.html, compare-religions.html, on-this-day.html
├── manifest.json, sw.js   App install and offline support
├── logo.svg, favicon.ico, icon-*.png, apple-touch-icon.png
├── CNAME                  Custom domain
└── README.md              This file
```

## How to add a new post (from the GitHub website)

1. Open the repository and go to the **`_posts`** folder.
2. Click **Add file → Create new file**.
3. Name it `YYYY-MM-DD-short-title.md`, for example `2026-10-10-indus-valley.md`. Use today's date or an earlier one. A future date will not appear until that day.
4. Paste this at the top, then write the article below it:

   ```
   ---
   layout: post
   title: "Your Post Title"
   date: 2026-10-10 12:00:00 +0000
   tags: [History]
   ---
   ```
5. Click **Commit changes**. The post goes live in a minute or two.

Tip: the `date` with a time decides the order, with the newest post first. `tags` decide where the post appears on the Topics page.

## Making changes

- Pages in the root folder (`about.md`, `ask.md` and so on) are edited by opening the file and clicking the pencil icon.
- Files inside `_includes` change the header and footer on **every** page, so edit with care.
- After each commit, wait a minute or two, then refresh the site with **Ctrl+F5**.

## Data and credits

- World Bank Open Data (CC BY 4.0): hunger, poverty, conflict deaths, homicide and violence against women.
- Pew Research Center: religion population estimates.
- D3.js, by Mike Bostock and contributors.
- Country outlines: `world.geo.json`.

Dates and numbers on the history pages are approximate and sometimes debated. If you find a mistake, please write to contact@earthandfaith.online.

## Contact

- Email: [contact@earthandfaith.online](mailto:contact@earthandfaith.online)
- Instagram: [@adity_m09](https://www.instagram.com/adity_m09)
- Telegram
  (https://t.me/earthandfaithcommunity)

## Copyright

© Adity Mishra. Please write to me before republishing posts or maps. Linking to the site is always welcome.
