# AI Engineering Roadmap

A single-page, self-tracking learning roadmap: PHP/JS developer → AI enablement engineer.

Nine phases, four hands-on labs, self-check questions, and per-phase notes.

No build step, no dependencies, no backend. One HTML file.

---

## Run it locally

```bash
open index.html          # macOS
xdg-open index.html      # Linux
```

That's it. If you never want it on the internet, stop here — everything works from the local file.

---

## Publish it on GitHub Pages

```bash
git init
git add index.html README.md
git commit -m "AI engineering roadmap"
gh repo create ai-roadmap --private --source=. --push
```

Then: **Settings → Pages → Build and deployment → Deploy from a branch → `main` / `(root)` → Save.**

Live in a minute or two at `https://<your-username>.github.io/ai-roadmap/`.

### Two things to know before you do that

**1. Pages from a private repo needs a paid plan.** GitHub Pages works from public repos on Free, and from public *or private* repos on Pro, Team and Enterprise. On Free with a private repo, the Pages option is disabled — you'd have to make the repo public.

**2. A private repo does not make the site private.** Repository visibility and published-site visibility are separate settings. On Pro or Team, the repo stays private but the published page is reachable by anyone with the URL — and since everything here lives in `index.html`, that means anyone with the URL reads the whole thing. Truly private Pages (access restricted to org members) requires GitHub Enterprise Cloud.

That's fine for this content — it's a study plan, not a secret. But **don't add anything internal to it** (real system names, internal URLs, company process detail) while it's on public Pages. If you later want it locked down, the practical options are: keep it local, put Cloudflare Access or Tailscale in front of your own host, or use Enterprise Cloud Pages access control.

The page ships with `<meta name="robots" content="noindex, nofollow">`, which keeps it out of search results but is not access control.

---

## Where your progress lives

Checkboxes, quiz answers and notes are saved to `localStorage` in whichever browser you're using. They do not sync between devices and they disappear if you clear site data. Nothing is sent anywhere.

If you want them to follow you, export by hand from the browser console:

```js
copy(localStorage.getItem('ai-roadmap-v2'))     // copies your state
localStorage.setItem('ai-roadmap-v2', '<paste>') // restores it elsewhere
```

---

## Editing the content

Everything lives in the `PHASES` array near the top of the `<script>` block in `index.html`. Each phase object takes:

| Key | What it does |
|---|---|
| `band` | `1` = sequential (solid spine), `2` = parallel (dashed spine) |
| `n`, `time`, `tag`, `core` | Header metadata; `core: true` turns the tag green |
| `intro`, `concepts[]`, `analogies[]`, `caution` | Theory content |
| `sub[]`, `extra`, `table`, `res[]` | Longer sections, tooling notes, resource list |
| `quiz[]` | `{q, o: [...], a: correctIndex, why}` |
| `lab` | `"sampling"` \| `"retrieval"` \| `"decide"` \| `"trifecta"` |
| `steps[]` | Numbered practice steps |
| `gate` | The "you know it when" test |

Sections are tagged `data-kind="theory"` or `data-kind="practice"`, which is what the Both / Theory / Practice switch filters on. If you add a new section type, tag it or it will always be visible.

To add a lab, add a function to the `LABS` object that takes the container element and renders into it, then reference its key from a phase.

---

## The labs

| Lab | Phase | What it's for |
|---|---|---|
| Temperature and sampling | 0 | Feel non-determinism instead of reading about it. Flatten the distribution, sample 30 times, count distinct completions. |
| Why hybrid search wins | 3 | Same query, three retrieval strategies over eight support chunks. Semantic misses `E-4471`; keyword misses "how do I get my money back". |
| Workflow or agent? | 4 | Six questions about a task you're actually considering, and a verdict you should argue with. |
| Trifecta auditor | 8 | Tick which of private data / untrusted content / external communication an agent has. Three is exploitable. |

They are deliberately simplified simulations — the retrieval lab uses a hand-written synonym map, not real embeddings. They're built for intuition, not as reference implementations.
