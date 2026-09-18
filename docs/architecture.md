# Architecture

## Flow

```
X (tracked roster)
   |
   v
CAPTURE ........ Grok, paginated, one day per window, groups of 5-6 accounts
   |
   v
DAILY .......... noise floor, subject + affect per post, silent roll
   |
   v
CORPUS ......... NotebookLM, analyst ruleset in the knowledge base
   |
   v
WEEKLY ......... derivation or bucket sort, traits tested across days
   |
   v
OUTPUT ......... newsletter, long-form, field tracking
```

## Capture

- One calendar day per window, 00:00 to 23:59. Windows are disjoint; a post belongs to exactly one daily.
- Accounts are pulled in groups of 5-6, balanced by posting volume so no group stacks heavy accounts.
- Pagination: `limit=10` plus `max_id`, looping until a page returns fewer than 10 or only duplicates. One follow-up is not enough.
- The quietest account in each group goes last as a canary. A thinned account and a silent one look identical, so the one most likely to fade is watched.
- Raw fields kept: text, timestamp, POST_ID, URL, engagement tuple (likes / reposts / quotes / replies / views). The tuple order is stated at the top of every pull.

## Daily capture report

- Only cut: posts with no subject of their own (bare agreement, single-word reactions, emoji, content-free pointers).
- Each surviving post becomes one FACT line: the subject, named as concretely as the post states it, plus the poster's own stance where it is part of the signal.
- No categories at this stage. Abstraction happens at derivation.
- Silent roll lists every roster account with no qualifying post. A heavy account reading thin on a busy day is flagged as suspected pull loss, not recorded as silent.
- No line without a POST_ID.

Prompt: [../prompts/daily-capture.md](../prompts/daily-capture.md)

## File conventions

Every output file opens with a typed header:

```
TYPE: DAILY | WEEKLY | RAW | CODEBOOK | ROSTER
DATE: YYYY-MM-DD
WEEK: YYYY-MM-DD      (Sunday anchor, identical across the week)
MONTH: YYYY-MM        (archive bucket)
RUN: live | test-only | elon-excluded | derivation
ROSTER: YYYY-MM-DD    (roster version in force)
```

- Filenames are lexical: `OF_DAILY_YYYY-MM-DD`, so date ranges select cleanly for weekly and monthly reads.
- The typed header does the isolation work. The weekly reads DAILY files only, which keeps raw posts from bleeding into synthesis.
- RUN labels keep changed conditions visible. Test weeks are flagged, not deleted.
- Referenced files keep fixed names. Prompts call them literally, and a rename breaks the reference without erroring.

## Archiving

Rolling three-month active window. On completion of each month past the third, the oldest month moves out of the active notebook.

## Synthesis

- Traits require repetition across changed conditions: a slow day, a busy day, an event day.
- Absence counts only against a baseline of presence. An account that never posts going quiet is not news.
- Every characterizing claim carries handle plus POST_ID. Uncited claims are dropped.
- Engagement is reported as position within an account's own range, since absolute counts drift between pulls.
