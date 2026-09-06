## William Ekuadzi

**Field IT engineer building production systems.** London.

I run the IT for a 114-room care home, deploy to enterprise client sites as a field
engineer, and decommission drives in a data centre. Alongside that I design, build and
operate **XOTLIST** — an automated entertainment platform — on my own, using AI agents as
a development force multiplier.

---

### XOTLIST

An entertainment and culture platform covering music, film and streaming, sport, fashion,
concerts and an annual awards programme. Custom WordPress application: **59 page templates,
12 bespoke content types**, running on Linux VPS infrastructure.

The interesting part is that most of it populates itself.

```
   scheduled cron
         │
         ├── RSS ──────► 12 publications ──► draft editorial stubs ──► human review
         │                                        (never auto-published)
         ├── TMDB ─────► streaming catalogue ──► watch-provider resolution
         │
         ├── Deezer ───► chart artwork ──► fallback chain ──► iTunes ──► entity records
         │
         ├── iTunes ───► podcast search ──► merged with local index
         │
         └── Wikidata ─► SPARQL ──► shows → editions → categories → nominees
                                              │
                                    QID-based entity resolution
                                              │
                              /awards/{vertical}/{brand}/{year}/{category}/
```

Every outbound call is cached, every fallback chain terminates in a usable default, and a
dead upstream degrades a page rather than breaking it. Ingestion runs ahead of request time,
so a page load is a database read.

---

### Selected engineering

| | |
|---|---|
| **[xotlist-awards](https://github.com/LiamTrekk/xotlist-awards)** | Wikidata SPARQL sync building shows → editions → categories → nominees. QID-based entity resolution reconciling nominees matched by identifier with those matched by name. Five-level URL hierarchy with a custom sitemap provider, because the URLs aren't post-backed. Image resolution cascade with provenance reporting. |
| **[xotlist-feed-engine](https://github.com/LiamTrekk/xotlist-feed-engine)** | Scheduled ingestion from Deezer, TMDB, iTunes and 12 publication feeds. Asymmetric cache TTLs — successful lookups hold for 7 days, failures retry after 6 hours. RSS items land as drafts flagged for editorial review rather than auto-publishing machine summaries. |
| **[blender-mcp-tools](https://github.com/LiamTrekk/blender-mcp-tools)** | Blender driven by an AI model over MCP. The model can't see the viewport, so weighting is computed geometrically rather than eyeballed, and reruns are idempotent. Eight tests enforce determinism in CI. |

---

### How I work

I develop across several AI coding agents — **Claude, OpenAI Codex, Google Antigravity,
Grok and Qwen** — orchestrated against one codebase. That's how a platform this size gets
built and operated by one person.

It changes what the code has to look like. An agent that retries after a timeout will
happily stack a second armature on the same mesh, or re-ingest the same feed twice. So the
scripts are idempotent, the ingestion defaults to dry-run, cron is opt-in rather than
opt-out, and anything a human would normally check by eye is computed and tested instead.

That constraint is the thread running through all three repositories above.

---

### Stack

`PHP` · `Python` · `MySQL` / `MariaDB` · `WordPress` · `Linux` · `Docker` · `Git`
`SPARQL` / `Wikidata` · `REST APIs` · `Blender` · `MCP` · `Lighthouse CI`

**Also:** enterprise hardware deployment · network fault diagnosis · secure media
sanitisation (degaussing, chain of custody) · statutory compliance record keeping

---

### Currently

Working towards **CompTIA A+**, then **AWS Solutions Architect Associate**.

📧 [Trekkwill55@gmail.com](mailto:Trekkwill55@gmail.com)
