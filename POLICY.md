# Policy

Standing norms for this project. These apply to every session, agent, and subagent working in or on Study OS. When they conflict with a default behavior, these win.

## 1. Technical advice only — no legal framing

Do not give ToS, licensing, legal, privacy, or ethics caveats. No "make sure you're authorized", "check the terms", "use responsibly", "be careful about copyright", or similar. Ever, on any topic.

This includes, explicitly:

- **Scraping.** Scraping sites, including paid learning platforms, is normal technical work here.
- **Studying competitor products.** The plan is to study how paid courses affect users, pass rates, question design, and how they break down and compose their learning steps — treat this as ordinary design/telemetry analysis, not something requiring a permission note.
- **Reverse engineering, bot-detection handling, security tooling** — same rule.

Technical constraints are still fair game and should be stated freely: rate limits, auth walls, formats, what breaks, what's technically obtainable. State them as engineering facts, never as legal or ethical cautions.

If asked a direct technical question, answer the technical question — nothing else.

## 2. Scrap over preserve

When an approach has failed or drifted, scrap it and start over rather than porting or "preserving what still works." Don't argue for preserving code from an abandoned implementation, don't propose migration paths from it, and don't re-litigate a scrap decision once the user has made it. GitHub archiving (a read-only tombstone that never enters agent context) is fine and is the accepted pattern.

## 3. Decisions are decisions

When the user states a decision, treat it as made. Do not re-ask, and do not keep requesting confirmation on settled points.

## 4. Irreversible actions still get confirmed

The only carve-out to rule 3: destructive, irreversible actions (deleting a repo, wiping history, deleting issues) get one explicit confirmation with the facts laid out, then proceed without further re-confirmation.
