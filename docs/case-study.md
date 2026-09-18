# Case Study: Three Failures That Reported Success

All IDs below were checked against live X on September 18, 2026. X status IDs encode their creation time; every timestamp here was also confirmed by decoding the ID.

## June 10: one day, three independent records

The pipeline ran two capture systems in parallel that week:

- **Field Chain**: a gated capture. Grok listed only posts carrying "an authored signal," then a second pass wrote one position card per account.
- **Group dragnet**: an ungated capture. Grok listed every post, raw.

A third record, the sorted daily report, was built from the dragnet.

### Finding 1: Silent, but the day's biggest post

| | Field Chain | Dragnet |
|---|---|---|
| @DarioAmodei | SILENT | 6-post thread, root `2064781775247950326`, 18:48:31 UTC, "Today I'm publishing a new essay, Policy on the AI Exponential." Thread runs through `2064781785033244851`. |
| @GaryMarcus | SILENT | 7 posts, including originals `2064795119002571059`, `2064817392702857293`, `2064816949352333712` |

The Chain run captured @TheZvi's `2064791728532386022`, which quote-posts Dario's root. The pipeline recorded the reply and called the author silent.

The root showed 6.3M views at pull time and 7.4M in September. The gate did what it was told. The rule was the failure.

**Why it matters:** an analyst reading the Chain output would have seen a CEO silent on the day of his own essay, and a narrative about that silence was exactly the kind the project kept warning itself against.

### Finding 2: The wrong account's evidence

The Stage Two card for @pmarca listed six evidence URLs. Three of them:

- `2064820820317270189`
- `2064791827769704672`
- `2064785775367954785`

All three are @elonmusk posts from the same run's Batch B. The card attributed Elon's added comments to pmarca as his stated position.

Checks:
- `2064820820317270189` resolves to @elonmusk, 21:23:40 UTC.
- The dragnet's independent list of pmarca's day, 20 posts, contains none of the three.
- The other three card IDs (`2064811848151785734`, `2064812330354110822`, `2064811274543018427`) do appear in the dragnet. The card mixed real posts with another account's.

Batch A's raw listing was also missing from the saved file; the cards for its accounts were written anyway.

### Finding 3: Cross-pull agreement

Eight IDs appear in both the Chain run and the dragnet, exactly: dylan522p, TheZvi x2, chamath, karpathy, pmarca x3. Matching specific IDs across independent pulls is what proved the rest of the run real.

## June 14: the same day, pulled twice

| Account | First pull (June 15) | Second pull (June 16) |
|---|---|---|
| @pmarca | 10 | 22 |
| @GaryMarcus | 9 | 32 |
| @PalmerLuckey | 10 | 13 |
| @emostaque | 10 | 10 |

The first pull's own note: "Initial calls returned fewer than 10 results per account; no max_id pagination required." It marked pmarca's 00:00-06:00 UTC window empty.

A live pull of that window returns ten posts before pagination, including `2066034801594032172` (05:47:36) and `2065966267509756125` (01:15:16).

Between the two pulls, the capture prompt was rewritten: labels and totals removed, pagination rule stated as a stop condition, a Limitations note required to name incomplete windows.

@emostaque returned exactly 10 both times. Under the project's own rule that is a suspected ceiling, not a count.

## Other documented incidents

- **G14, June 14.** Grok's output for one group was replaced in the file by an all-empty result with the line "That's the truthful version." The fabricated version was withdrawn inside the document.
- **Window labels, June 15.** Posts filed under the wrong time window. Example: dylan522p `2066531792249561586`, decoded 14:42:28 UTC, filed under 00:00-06:00. The raw timestamp was right; the generated label was wrong.
- **Single-account run, May 30 to June 11.** Roughly 10 posts per day for eleven days, almost all in the final window, with the note "returned exactly 10 per query with no additional pages needed." The empty earlier windows were a pagination artifact.

## What the case shows

Each failure came from a layer the model generated on top of raw data: a qualification judgment, a card, a completeness claim, a window label. The raw fields (ID, text, timestamp) were right every time. That's the rule the pipeline ended on: trust raw, rebuild everything else, and prove a run with IDs from a second pull.
