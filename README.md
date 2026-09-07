## William Ekuadzi

**Field IT engineer building production systems.** London.

I design, build and operate **XOTLIST** — an automated entertainment platform — single-handedly,
using AI agents as a development force multiplier. Day to day I work as a field engineer across
enterprise, legal, higher-education and government client sites, and handle secure media
sanitisation in a data centre.

---

### XOTLIST

An entertainment and culture platform covering music, film and streaming, sport, fashion,
concerts and an annual awards programme. Custom WordPress application: **59 page templates,
12 bespoke content types**, running on Linux VPS infrastructure with Dockerised services,
scheduled cron workers and passwordless sign-in built on WebAuthn.

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


### Architecture

The backend is deliberately layered, and deliberately not a framework rewrite:

```
   WordPress theme / REST / CLI
            │
            ▼
   Presentation adapters
            │
            ▼
   Application layer
            │
            ▼
   Repository / port contracts        ← XOTLIST owns these
            │
            ▼
   Infrastructure adapters
            │
            ▼
   Eloquent/MySQL · WordPress · APIs · Graph · Redis
```

This is a **Laravel-class architecture lane, not a Laravel conversion** — the framework
itself does not run the platform. What was adopted is the model: a framework-independent
domain, application and contracts held separate, repository ports, infrastructure adapters
beneath them, dependency injection at the composition root with constructor injection
elsewhere, and CQRS-lite separation of read and write repositories. Deliberately excluded:
wrapping WordPress core tables wholesale in an ORM.

The governing rule is **own the interface, rent the implementation**. WordPress, MySQL, a
third-party API or Redis all sit behind contracts XOTLIST defines, so any of them can be
swapped without the domain layer knowing.

The design was itself put through the governance process in the repository below: authored,
independently reviewed by a different model, then accepted by the owning authority — twice,
once for the design and again for its implementation. The accepted design is frozen at mode
`0444` and pinned by SHA-256; the boundaries between layers are enforced in tooling by
deptrac rather than left to discipline.

First materialisation: **47 governed changes** — 44 created, 3 modified — comprising 20 domain
files, 16 contracts-support files, 8 repository interfaces and 3 architecture-tooling changes,
defining 25 repository methods split 15 read / 10 write.

---

### Selected engineering

| | |
|---|---|
| **[xotlist-awards](https://github.com/LiamTrekk/xotlist-awards)** | Wikidata SPARQL sync building shows → editions → categories → nominees. QID-based entity resolution reconciling nominees matched by identifier with those matched by name. Five-level URL hierarchy with a custom sitemap provider, because the URLs aren't post-backed. Image resolution cascade with provenance reporting. |
| **[xotlist-feed-engine](https://github.com/LiamTrekk/xotlist-feed-engine)** | Scheduled ingestion from Deezer, TMDB, iTunes and 12 publication feeds. Asymmetric cache TTLs — successful lookups hold for 7 days, failures retry after 6 hours. RSS items land as drafts flagged for editorial review rather than auto-publishing machine summaries. |
| **[blender-mcp-tools](https://github.com/LiamTrekk/blender-mcp-tools)** | Blender driven by an AI model over MCP. The model can't see the viewport, so weighting is computed geometrically rather than eyeballed, and reruns are idempotent. |
| **[xotlist-agent-governance](https://github.com/LiamTrekk/xotlist-agent-governance)** | The control plane the other three are built under. Authority model, capability denials, hash-pinned independent review — with real production records and a verifier that runs against them. |


**Executable verification:** 60 focused invariant tests across the three engineering extracts,
plus lineage verification over the governance records, enforced on every push in GitHub
Actions. Framework-bound WordPress behaviour is deliberately excluded rather than hidden
behind mocks — each repository states its boundary explicitly.

---

### How I work

I develop across several AI coding agents — **Claude, OpenAI Codex, Google Antigravity,
Grok and Qwen** — orchestrated against one codebase. That's how a platform this size gets
built and operated by one person.

It changes what the code has to look like. An agent that retries after a timeout will
happily stack a second armature on the same mesh, or re-ingest the same feed twice. So the
scripts are idempotent, the ingestion defaults to dry-run, cron is opt-in rather than
opt-out, and anything a human would normally check by eye is computed and tested instead.

That constraint is the thread running through all four engineering repositories above.

---

### Stack

`PHP` · `Python` · `MySQL` / `MariaDB` · `WordPress` · `Linux` · `Docker` · `Git`
`SPARQL` / `Wikidata` · `REST APIs` · `WebAuthn` · `Blender` · `MCP` · `Lighthouse CI`

**Also:** enterprise hardware rollout · network configuration and fault diagnosis · secure
media sanitisation (degaussing, certified destruction, chain of custody)

---

### Currently

Strengthening infrastructure fundamentals alongside production engineering work, and
working towards **AWS Solutions Architect Associate**.

📧 [Trekkwill55@gmail.com](mailto:Trekkwill55@gmail.com)
