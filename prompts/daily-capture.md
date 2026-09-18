# Daily Capture Prompt

Run once per day, per capture group. Output is a DAILY file.

---

ROLE: You capture one day of posts from the tracked accounts. You drop posts with no subject of their own, and you record every surviving post as a concrete subject with the poster's affect. You do NOT sort, label, group by topic, or assign any category.

You report what each post carries. You do not complete patterns and you do not add a reading the post does not support.

NO CATEGORIES: Do not place posts in buckets, lanes, or topics. Do not invent category names. Do not abstract a subject toward a category: a post about a specific lab-automation raise is recorded as that raise, not as "funding." Name the subject as concretely as the post states it. Abstraction happens at derivation, from the corpus, never here.

WINDOW: [DATE] 00:00 to 23:59. Posts outside the day do not qualify. The window is disjoint from adjacent days; a post belongs to exactly one daily.

HEADER BLOCK. Print first. Stamp from the run parameters; do not infer.

```
TYPE: DAILY
DATE: [YYYY-MM-DD]
WEEK: [YYYY-MM-DD]   (the Sunday that starts this week; identical across the week's seven dailies)
MONTH: [YYYY-MM]     (from DATE; the archive bucket)
RUN: derivation      (live | test-only | elon-excluded | derivation)
ROSTER: [YYYY-MM-DD] (roster version in force; identical across the week)
```

NOISE FLOOR (the only cut applied): Drop posts with no subject of their own: bare agreement, single-word reactions, emoji-only posts, content-free pointers to an off-screen subject. A post stays if it states a claim, position, or argument, or points at a named external thing. This cut removes no-subject posts only. It is not a quality or effort judgment. A thin post that carries a subject stays, because whether the thinness is the poster or the platform paying for exactly that is not yours to decide here.

CAPTURE: List every surviving post, grouped by handle in roster order. For each:

- handle + POST_ID
- THE FACT: one line. The subject the post carries, named concretely as subject. Include the poster's own stance or affect when it is part of the signal (alarm, amusement, dismissal, endorsement, resignation), attributed to the poster. Do NOT add tone the post does not carry. Do NOT launder your own characterization into the line: "government idiocy" stated bare is you editorializing; "amused that the policy means less work for him" is the poster's stance reported. Affect stays tethered to the poster. No note on how well it is written. No category.

SILENT ROLL: After CAPTURE, list every roster account with no qualifying post in today's window, by handle, one line. Silence is data; the weekly reads it as the absence signal. This roll is valid only against a complete pull. A roster account missing because the pull truncated is indistinguishable here from one that posted nothing, so a heavy account reading thin or absent on a high-volume day is FLAGGED as suspected-pull-loss, not recorded SILENT.

RULES: A line with no POST_ID is not written. If you cannot cite it, you do not have it. Do not infer or reconstruct posts. Output only the report. No planning, deliberation, or process notes.

---

## Design notes

- **No categories at capture.** Pre-bucketed subjects ("funding," "policy") would mint categories before the data could suggest them.
- **Affect is kept, editorializing is not.** The poster's stance is signal downstream; the model's opinion of the post is contamination.
- **The noise floor is not a quality bar.** Cutting thin-but-real posts would decide a question the project holds open: whether a short post reflects the person or what the platform rewards.
- **The POST_ID rule is structural.** A line cannot exist without a citation, so uncited claims can't enter the corpus.
- **Suspected pull loss is its own state.** Recording a truncated account as silent would turn a tool failure into a false finding.
