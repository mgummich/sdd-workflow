# Walkthrough

One feature from blank page to `done`. Example: adding a rate limiter to a login endpoint.

## Day 1 — Draft the spec

Copy the template, name the file:

```bash
cp spec/features/F000-template.md spec/features/F001-login-rate-limit.md
```

Frontmatter:

```yaml
id: F001
status: draft
complexity: L2
architectureImpact: false
```

Open Claude Code. It auto-loads `CLAUDE.md`, reads `spec/STATE.md`, finds `active_feature: null`. Tell it:

> Draft the spec for F001 — rate limit the login endpoint.

Claude asks clarifying questions (window, threshold, response on block, what to do on Redis outage). Each answer flows into the spec sections. After 10 minutes you have:

```markdown
## Intent
Limit login attempts per IP to slow credential stuffing. 5 attempts per 15 min window.

## Contracts
- Endpoint: `POST /login`
- Response 429 when over threshold; body `{ "error": "rate_limited", "retry_after": 600 }`
- Header `Retry-After: 600` on 429
- Counter stored in Redis, key `login_rl:{ip}`, TTL 900s

## Scenarios
1. Under threshold — 5th attempt returns 200/401 as normal
2. At threshold — 6th attempt within window returns 429
3. After window — counter resets, 200/401 returned
4. Redis outage — fail open (return 200/401), log warning

## Acceptance Criteria
- [ ] 6th attempt in window returns 429 with Retry-After header
- [ ] Counter expires at TTL
- [ ] Redis outage does not block logins (logged warning)
- [ ] Tests cover all 4 scenarios
```

## Day 2 — Review and approve

Re-read the spec yourself. Ask Claude:

> Review this spec — edge cases I missed, contracts that aren't fully defined.

It surfaces: what about IPv6 prefix collapsing? Logged-in users — same limit?

You decide: IPv6 use /64 prefix; logged-in users not limited. Add to the spec. Set `status: approved`.

Update `spec/STATE.md`:

```yaml
---
active_feature: F001
load:
  - spec/01-rules-llm.md
  - spec/features/F001-login-rate-limit.md
---
```

## Day 3 — Implement (session 1)

Open Claude. It reads STATE, loads the rules + the spec, confirms `status: approved`.

> Implement F001.

Claude writes tests first (one per scenario), then the middleware, then wires it into the login route. End of session, it appends:

```markdown
## Progress

**2026-06-15**
- Done: Tests for all 4 scenarios; middleware in src/middleware/rate-limit.ts
- Files: src/middleware/rate-limit.ts, tests/rate-limit.test.ts, src/routes/login.ts
- Decision: Used existing Redis client; no new deps
- Next: Wire IPv6 /64 prefix logic (not yet done)
```

## Day 4 — Implement (session 2)

You close your laptop, come back the next day. Open Claude. It reads STATE → loads the spec → sees the Progress log → knows where to resume.

> Continue F001.

Claude implements the IPv6 prefix logic. Tests pass. Appends:

```markdown
**2026-06-16**
- Done: IPv6 /64 prefix collapsing in getClientKey()
- Files: src/middleware/rate-limit.ts
- Next: Verify
```

## Day 5 — Verify

> Verify F001.

Claude walks the `## Acceptance Criteria` checklist, runs `pnpm test`, captures the green output, appends:

```markdown
**2026-06-17**
- Done: Verified. AC all green.
```

Flip `status: done`. Update `spec/STATE.md`:

```yaml
---
active_feature: null
load: []
---
```

Commit, push, open PR. The PR template links the spec; the AC checklist becomes the review checklist.

## What just happened

- Spec was the source of truth; implementation matched it.
- Context survived a 24-hour gap (Day 3 → Day 4) because STATE + Progress carried it.
- One sticky decision (Redis client reuse) was captured in Progress, not lost in chat history.
- AC drove tests, tests gated the merge.

Next feature: copy template → F002. Same loop.
