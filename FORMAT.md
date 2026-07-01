# BlockTube Filter List Format — v1

Filter lists are plain-text files, one rule per line. Lines starting with `#` are comments and
are ignored. Blank lines are ignored.

## Rule Prefixes

| Prefix | Matches against | Example |
|--------|----------------|---------|
| `channel:` | YouTube channel ID (exact) | `channel:UCxxxxxxxxxxxxxxx` |
| `keyword:` | Channel name (word-boundary, case-insensitive) | `keyword:AI News Daily` |
| `title:` | Video title — plain word or `/regex/flags` | `title:/\bai.generated\b/i` |
| `videoid:` | YouTube video ID (exact) | `videoid:xxxxxxxxxxx` |

## Channel ID Rules — Annotation Format

Channel ID rules **must** carry an inline comment with the channel display name and the date
the rule was added. This makes the list human-reviewable and git-diffable.

```
channel:<id> # <Channel Display Name> | added:<YYYY-MM-DD>
```

Example:
```
channel:UCxxxxxxxxxxxxxxx # AI Factory Channel | added:2025-01-15
```

The extension strips everything after `#` before matching, so the annotation does not affect
blocking behaviour.

## Title Rules

Plain words are matched case-insensitively at word boundaries:
```
title:AI Generated
```

Regular expressions use `/pattern/flags` syntax:
```
title:/\bai[- ]generated\b/i
title:/^Top \d+ AI/i
```

## Keyword Rules

`keyword:` rules match against the channel **name** using word-boundary matching:
```
keyword:AI News Daily
keyword:Artificial Intelligence Hub
```

## Full Example

```
# BlockTube Filter List — AI Slop (Global)
# Format version: 1
# Last updated: auto (do not edit this line)

# ── Channel IDs ──────────────────────────────────────────────────────────────
channel:UCxxxxxxxxxxxxxxx # AI Factory Channel | added:2025-01-15
channel:UCyyyyyyyyyyyyyyy # Robot Reacts | added:2025-01-20

# ── Title patterns ───────────────────────────────────────────────────────────
title:/\bai[- ]generated\b/i
title:/\bvirtual youtuber\b/i
title:/^Top \d+ AI tools/i

# ── Channel name keywords ─────────────────────────────────────────────────────
keyword:AI News Daily
keyword:Artificial Intelligence Hub
```

## Comments

- Use `//` comments inside the BlockTube options UI (legacy format).
- Use `#` comments in filter list files (this format).
- The extension ignores both styles in the appropriate context.
