# Operator Field

A pipeline for turning AI-retrieved social media data into content you can trust, and the record of how it broke.

Operator Field captured daily posts from a roster of AI and tech figures on X, recorded each post as a concrete subject with the poster's stance attached, and fed a synthesis layer meant to report what the field pays attention to and how that shifts. The newsletter it fed ran for about a week. The newsletter was the test. The pipeline is the product.

## Three failures that reported success

Every one of these produced clean, confident output. None threw an error. All three are verifiable against live X as of September 2026.

**1. A capture gate deleted a 7M-view post and wrote SILENT.**
On June 10, 2026, the first version of the pipeline filtered at capture: list only posts that "carry an authored signal." It marked @DarioAmodei SILENT. That afternoon he had posted a six-part thread announcing a policy essay; the root now shows over 7.4M views. @TheZvi quote-posted it the same hour, and the pipeline captured Zvi's reply while calling Dario silent. It also marked @GaryMarcus SILENT on a day he posted at least three original takes.

**2. A summary card assigned one account's posts to another.**
The same run's Stage Two card for @pmarca cited three status IDs as his evidence. All three belong to @elonmusk. A status ID can't belong to two accounts. An independent pull of pmarca's day, 20 posts, contains none of them.

**3. A pull declared a full morning empty.**
The first June 14 pull returned exactly 10 posts for pmarca and stated "fewer than 10 results; no pagination required." It marked his 00:00-06:00 UTC window empty. That window held 10 posts. A second pull, after a prompt rewrite, returned 22 for pmarca, 32 for Marcus (first pull: 9), and 13 for @PalmerLuckey (first pull: 10).

Full write-up with IDs: [docs/case-study.md](docs/case-study.md)

## What changed because of them

| Failure | Structural fix |
|---|---|
| Gate at capture deleted real posts | Capture everything; the only cut is posts with no subject at all. Judgment moves to read time, where a mistake can be seen and reversed. |
| Generated card misattributed posts | Raw fields are ground truth. No generated layer is trusted until rebuilt from raw. Every claim carries handle plus POST_ID. |
| Page one treated as the whole day | Paginate with `max_id` until a page returns under 10 or repeats. A heavy account reading thin on a busy day is flagged as pull loss, never recorded silent. |
| Model-assigned labels drifted | The capture prompt stopped emitting TYPE labels and totals. Post type is computed downstream from structural fields. |

## How it evolved

Five prompt generations in six days, each responding to a documented failure. Full timeline: [docs/evolution.md](docs/evolution.md). Every version is in [prompts/history](prompts/history).

## Repo map

```
docs/
  case-study.md         the three incidents, with IDs and how each was caught
  evolution.md          dated timeline of prompts, rosters, and why each changed
  architecture.md       stages, tags, file conventions
  failure-modes.md      every failure class, its tell, and the fix
  design-principles.md  rules that survived repeated runs
  roster-design.md      who gets tracked and why
prompts/
  capture-dragnet.md    final raw-pull prompt (Grok)
  daily-capture.md      final capture-report prompt
  analyst-rules.md      final NotebookLM ruleset
  reddit-capture.md     the method ported to Reddit
  history/              every earlier version, verbatim
codebook/
  buckets.md            seven-bucket codebook and minting rule
roster/
  roster-2026-06-18.md  55 accounts in 10 load-balanced groups
samples/
  june-10-three-records.md  the same day from three pulls, side by side
```

## What I'd do next

- Move mechanical checks into a script between capture and synthesis: decode status IDs to timestamps, flag cross-account ID collisions, flag windows that return exactly 10.
- Build the long-form layer (podcasts, essays) as a parallel pipeline for the figures who are quiet on X.
- Run the expanded roster for a month so the weekly has a baseline to measure change against.

## Stack

Grok (capture) · Gemini and NotebookLM (processing, corpus) · Claude (verification, analysis)
