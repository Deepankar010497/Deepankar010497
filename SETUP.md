# Profile README — setup & customisation

Everything here is for your **GitHub profile overview page** (`github.com/Deepankar010497`),
which is a different thing from your portfolio site. This README is what recruiters and
other engineers see when they click your avatar.

---

## 1. The one rule that makes it work

GitHub renders a profile README on your overview page **only** if you own a **public**
repository whose name is **exactly your username**:

```
Deepankar010497  /  Deepankar010497
└── username          └── repository name must be identical
```

If the repo is named anything else (`profile`, `profile-readme`, `me`, …) it will just be a
normal repo and nothing appears on your profile. It must also be **public** — a private
repo is ignored.

The README must be at the **root** of that repo, named `README.md`.

---

## 2. Create the repository

1. Go to <https://github.com/new>
2. **Repository name:** `Deepankar010497` — exactly that, all lowercase/uppercase matching your username (`Deepankar010497`)
3. **Description:** `Profile README` (optional)
4. **Visibility:** ✅ **Public** — this is required
5. **Do NOT** tick "Add a README file", and set no `.gitignore` or licence — you already have files
6. Click **Create repository**

GitHub should now show a hint like *"You found a secret! `Deepankar010497/Deepankar010497` is a special repository"* — that's the confirmation you got it right.

---

## 3. Push these files

This folder is already laid out as the repo. From this directory:

```bash
cd "d:/Github Works/Github_branch_code/Deepankar010497"

git init -b main
git add -A
git commit -m "Add profile README with custom banner artwork"

git remote add origin https://github.com/Deepankar010497/Deepankar010497.git
git push -u origin main
```

### ⚠️ About the push authentication

Your GitHub token expired recently, so the first push may fail with:

```
remote: Invalid username or token. Password authentication is not supported for Git operations.
```

GitHub **retired password auth entirely**. When that happens:

- **Do NOT** set `GIT_TERMINAL_PROMPT=0` — it suppresses the browser sign-in window and turns
  a recoverable expiry into a hard failure.
- Just run `git push` and **complete the browser window** Git Credential Manager opens. The
  push resumes automatically.
- If a stale credential keeps getting reused, clear it first:

  ```bash
  printf 'protocol=https\nhost=github.com\n' | git credential reject
  git push -u origin main
  ```

- **Never** paste a Personal Access Token into a chat or a prompt someone else controls. The
  browser flow exists precisely so you don't have to.

---

## 4. The snake animation (optional — currently switched off)

A workflow is already included at `.github/workflows/snake.yml`. It works, and it has already
run — it writes `snake.svg` and `snake-dark.svg` to an **`output`** branch. Its automatic
triggers are currently **commented out** (see step 2 below).

**It is not referenced in `README.md` right now, and that's deliberate.** The snake traces your
contribution graph; because private contributions aren't counted yet (§7), the graph is nearly
empty and a snake tracing three squares looks worse than no snake at all.

**Turn it on once your graph has real activity:**

1. First enable private contributions — see §7, *"My private contributions aren't counted"*
2. Uncomment the `schedule:` and `push:` triggers in `snake.yml`. They're commented out so the
   workflow doesn't churn in the background while nothing displays its output — `workflow_dispatch`
   is left on, so you can always run it by hand in the meantime
3. Confirm the `output` branch exists: <https://github.com/Deepankar010497/Deepankar010497/tree/output>
   — if it doesn't, open **Actions** → **Generate contribution snake** → **Run workflow**
4. Paste this into `README.md` wherever you want the snake to sit:

```html
<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Deepankar010497/Deepankar010497/output/snake-dark.svg" />
    <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Deepankar010497/Deepankar010497/output/snake.svg" />
    <img src="https://raw.githubusercontent.com/Deepankar010497/Deepankar010497/output/snake.svg" alt="Contribution snake" width="100%" />
  </picture>
</div>
```

**To stop running it entirely:** delete `.github/workflows/snake.yml`. The automatic triggers are
commented out already, so nothing runs on its own; the `output` branch is harmless and can be
left alone.

**If the snake shows a broken image:** the `output` branch doesn't exist yet, or your default
branch isn't `main`. Fix the branch name in `snake.yml` under `on.push.branches` if yours is
`master`.

**If Actions is disabled:** Settings → Actions → General → *Allow all actions and reusable
workflows*. The workflow only needs the default `GITHUB_TOKEN`; no secrets to configure.

---

## 5. Customisation map

| What you want to change | Where |
| :--- | :--- |
| **Name / colours / tagline in the header artwork** | `assets/banner.svg` — open in any editor; the text is inside `<text>` tags, colours are in `<defs>` |
| **Footer artwork** | `assets/footer.svg` |
| **Rotating headline** | `README.md` → the `readme-typing-svg` URL, edit the `lines=` parameter (`;` separates lines, `+` is a space, `%C2%B7` is `·`) |
| **Text around the badges** | Everything under `## Hey, I'm Deepankar` |
| **Tech badges** | The shields.io blocks. Copy any line and change `label`/`logo`/`color`. Logo names come from [Simple Icons](https://simpleicons.org) |
| **Icon strip** | `skillicons.dev/icons?i=...` — valid names at <https://skillicons.dev> |
| **Adding a stats/graph section back** | See §7 → *"Bringing back a stats section"*. Every card is a standalone `<img>`, so add only the ones you've confirmed render |
| **Snake colours** | `snake.yml` → the `palette` on the `snake-dark.svg` output, or add `color_snake=`/`color_dots=` — only relevant once you re-enable the snake (§4) |

### Brand palette (matches your portfolio)

```
cyan    #6DD3FF   primary
lime    #C8F169   accent
mint    #8CE6C6   mid-tone
ink     #0D1117   card background
```

---

## 6. What powers each widget

These are third-party services that render an image on every page load. Nothing is stored in
your repo except the two local SVGs.

| Widget | Service | Notes |
| :--- | :--- | :--- |
| Banner, footer | **local SVG in this repo** | No dependency, can't rate-limit, fully branded |
| Rotating headline | `readme-typing-svg.demolab.com` | Formerly on Heroku; the `demolab.com` domain is the current one |
| Badges | `img.shields.io` | Very reliable |
| Icon strip | `skillicons.dev` | One invalid icon name degrades that icon only |

That's the complete list — the README now depends on exactly two external services. Deliberately
**absent**: profile-view counters, stats / streak / language cards, contribution charts and the
snake. They're all straightforward to add back, but they were removed because they were showing
near-zero numbers, and a row of zeros reads worse than showing nothing. See §7.

---

## 7. Troubleshooting

**Bringing back a stats section.**
Every card is just an `<img>`, so add back only what renders. Check the URL in a browser tab
first — an image means it works; JSON, `402` or `429` means it doesn't. These were verified
good when this README was built:

| Card | URL |
| :--- | :--- |
| Stats | `https://github-readme-stats.vercel.app/api?username=Deepankar010497&show_icons=true&theme=tokyonight&hide_border=true&include_all_commits=true&count_private=true&title_color=6DD3FF&icon_color=C8F169&text_color=c9d1d9&bg_color=0d1117` |
| Top languages | `https://github-readme-stats.vercel.app/api/top-langs/?username=Deepankar010497&layout=compact&theme=tokyonight&hide_border=true&langs_count=8&title_color=6DD3FF&text_color=c9d1d9&bg_color=0d1117` |
| Streak | `https://streak-stats.demolab.com?user=Deepankar010497&theme=tokyonight&hide_border=true&background=0d1117&stroke=6DD3FF&ring=C8F169&fire=C8F169&currStreakLabel=6DD3FF` |
| Summary / extra stats | `https://github-profile-summary-cards.vercel.app/api/cards/profile-details?username=Deepankar010497&theme=tokyonight` |
| Contribution chart | `https://ghchart.rshah.org/6DD3FF/Deepankar010497` |

⚠️ **`streak-stats.demolab.com` is the correct domain.** The older
`github-readme-streak-stats.demolab.com` no longer resolves at all (a DNS failure, not a typo you
made), and the `…herokuapp.com` one is gone.

⚠️ **Fix the data before you fix the design.** The reason this section was cut is that
`count_private=true` does nothing until you enable private contributions, so the language card
read *Jupyter Notebook 99.99%* and the stats read four zeros. Turn that setting on first.

**A stats card shows "Something went wrong" / a rate-limit message.**
`github-readme-stats.vercel.app` is a free shared deployment and does get throttled. Options:
- Reload after a minute — it's usually transient
- Self-host it in one click: <https://github.com/anuraghazra/github-readme-stats#deploy-on-your-own-vercel-instance> and swap the domain
- GitHub also **caches** these images for a few hours, so a broken image can persist after the
  service has recovered — append `&v=2` to the URL to bust the cache, change it each time

**My private contributions aren't counted.**
On your GitHub profile → *Contribution settings* → tick **"Include private contributions on my
profile"**. The `count_private=true` parameter in the stats URL only works after that.

**Trophies or the activity graph — why aren't they here?**
Both `github-profile-trophy.vercel.app` and `github-readme-activity-graph.vercel.app` were
returning **HTTP 402 (Payment Required)** when this README was built — the shared free Vercel
deployments were over quota. Those services are popular and may recover, so if you want the
trophies back, drop this into `README.md`:

```html
<img src="https://github-profile-trophy.vercel.app/?username=Deepankar010497&theme=tokyonight&no-frame=true&no-bg=true&column=7" alt="Trophies" />
```

Check it returns an image in a browser first — don't ship a broken widget.

**How to check a widget before trusting it.**
Every card here is just an image URL, so you can paste the URL straight into a browser tab. If
it renders an image, it works. If you get JSON, an error page, or `402`/`429`, swap it out. The
tidy `github-profile-summary-cards` set is a reliable alternative for almost any stat.

**The whole README looks different in light mode.**
All cards use `theme=tokyonight`, which is dark. It's the most common choice and reads fine on
light, but if you'd rather match: change `theme=tokyonight` → `theme=default` and
`bg_color=0d1117` → `bg_color=ffffff` on each card. For true per-theme switching, wrap an
`<img>` in `<picture>` with two `<source>` tags using
`media="(prefers-color-scheme: dark)"` and `media="(prefers-color-scheme: light)"` — the snake
block in §4 is a good template for that.

**The banner is huge / tiny.**
It's `width="100%"`, so it fills the column. Edit the `<img>` tag in `README.md` to
`width="880"` for a fixed size.

**Nothing shows on my profile at all.**
99% of the time it's one of: repo isn't **public**, repo name doesn't **exactly** match the
username, or the file isn't named `README.md` at the **root**.

**GitHub caches the rendered README.** Hard-refresh with `Ctrl+F5` (or
`Cmd+Shift+R`) after editing.

---

## 8. Optional upgrades

Drop these in if you want them. Each adds a dependency, so only add what you'll maintain.

<details>
<summary><b>Latest blog posts, auto-updated</b></summary>

<br/>

Needs an **RSS/Atom feed**. Your `blog.html` doesn't publish one yet, so this only works once
you add one (or if you start posting on dev.to / Medium / Hashnode).

```yaml
# .github/workflows/blog-posts.yml
name: Latest blog posts
on:
  schedule:
    - cron: "0 0 * * *"
  workflow_dispatch:
jobs:
  update-readme-with-blog:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: gautamkrishnar/blog-post-workflow@v1
        with:
          feed_list: "https://your-feed-url/rss.xml"
          max_post_count: 5
          comment_tag_name: "BLOG-POST-LIST"
```

Then add this pair of markers to `README.md` where you want the list to appear:

```markdown
<!-- BLOG-POST-LIST:START -->
<!-- BLOG-POST-LIST:END -->
```

</details>

<details>
<summary><b>Spotify "now playing"</b></summary>

<br/>

Requires a Spotify account, an app registration, and a refresh token, then deploying
<https://github.com/kittinan/spotify-github-profile>. It's a fair amount of setup for a widget
that reads "not playing" most of the time. Only worth it if you actually listen while coding.

</details>

<details>
<summary><b>Coding time stats (WakaTime)</b></summary>

<br/>

Requires the WakaTime editor plugin and an API key stored as a repo secret. Genuinely
interesting if you keep it installed — it adds a "languages over the last 7 days" panel.
See <https://github.com/anmol098/waka-readme-stats>. Note it needs `GH_TOKEN` plus a
`WAKATIME_API_KEY` secret in *Settings → Secrets and variables → Actions*.

</details>

<details>
<summary><b>A light/dark aware banner</b></summary>

<br/>

Your banner is dark artwork. On GitHub's light theme it still looks intentional (dark banner,
light page), which is the common approach. If you want a light variant, duplicate
`assets/banner.svg`, flip the background stops to light greys, and wrap both in a
`<picture>` exactly like the snake block does.

</details>

---

## 9. Files in this folder

```
Deepankar010497/
├── README.md                        ← the profile README itself
├── SETUP.md                         ← this file
├── assets/
│   ├── banner.svg                   ← header artwork (custom, no external service)
│   └── footer.svg                   ← footer artwork
└── .github/
    └── workflows/
        └── snake.yml                ← generates the contribution snake
```

Delete this `SETUP.md` before or after pushing — it's for you, not for visitors, and it will
appear in the repo listing. Keeping it is harmless, but removing it is tidier. (It will *not*
render on your profile page; only `README.md` does.)
