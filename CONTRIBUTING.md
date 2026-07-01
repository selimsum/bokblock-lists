# Contributing to bokblock-lists

Thank you for helping keep YouTube free of AI slop and low-quality content!

## The Easy Way — Report from the Extension

The simplest way to contribute is directly inside YouTube:

1. Right-click any channel thumbnail, video, or channel page header.
2. Select **Report as AI Slop** from the Bokblock context menu.
3. Pick the target list (e.g. "AI Slop — Global") and click **Report**.

Your submission goes into a review queue. A maintainer will approve it and it will appear in
the next daily release.

## Manual Contributions via Pull Request

You can also open a PR directly against this repository.

### Rule format

See [FORMAT.md](FORMAT.md) for the full specification. Quick reference:

```
# Channel IDs — must include the annotation comment
channel:UCxxxxxxxxxxxxxxx # Channel Display Name | added:YYYY-MM-DD

# Title regex patterns
title:/\bai[- ]generated\b/i

# Channel name keyword (word-boundary match)
keyword:AI News Daily
```

### Steps

1. Fork this repository.
2. Edit the appropriate `.txt` file (e.g. `ai-slop.txt`).
3. Add one rule per line following the format above.
4. Open a pull request with a short description of what you're blocking and why.

### Finding a channel ID

- Visit the channel page on YouTube.
- The channel ID starts with `UC` and appears in the URL after `/channel/`.
- For `/@handle` URLs, open the page source and search for `"externalId"`.

## Maintainer: Running the Edge Function

After reviewing and approving submissions in Supabase Studio:

1. Set rows to `status = 'approved'` in the `submissions` table.
2. Call the `approve-and-pr` Edge Function with your `FUNCTION_SECRET`:

```sh
curl -X POST https://enocijittqepzlgrllrg.supabase.co/functions/v1/approve-and-pr \
  -H "Authorization: Bearer <FUNCTION_SECRET>"
```

The function will:
- Group approved rows by `list_target`
- Create a branch `auto/submissions-YYYY-MM-DD` on the target repo
- Commit the new rules in the annotated format
- Open a pull request for review

### Deploying the Edge Function

```sh
# Install the Supabase CLI: https://supabase.com/docs/guides/cli
supabase login
supabase link --project-ref enocijittqepzlgrllrg
supabase functions deploy approve-and-pr
```

### Required Supabase Secrets

Set these via `supabase secrets set` or the Supabase Dashboard → Project Settings → Edge Functions:

| Secret | Description |
|--------|-------------|
| `FUNCTION_SECRET` | Bearer token to authorize calls to the function |
| `DB_SERVICE_ROLE_KEY` | Service-role key for reading/writing the `submissions` table |

### Per-list GitHub Secrets

For each `list_target` slug (e.g. `ai-slop`), set three secrets
(slug uppercased, hyphens → underscores):

| Secret | Example value |
|--------|---------------|
| `GITHUB_TOKEN_AI_SLOP` | Fine-grained PAT with `contents:write` + `pull-requests:write` |
| `GITHUB_REPO_AI_SLOP` | `selimsum/bokblock-lists` |
| `GITHUB_FILE_AI_SLOP` | `ai-slop.txt` |

## Code of Conduct

- Block content, not people. Rules must target channels, titles, or keywords — not individuals.
- No personal data in rules.
- Keep annotations accurate: channel name and date must match reality.
