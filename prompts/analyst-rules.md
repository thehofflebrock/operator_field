# Analyst Rules (NotebookLM knowledge base)

Loaded into the notebook as a standing source, so the discipline persists across every query instead of living in individual prompts.

## Data trust

1. Engagement tuple order is likes / reposts / quotes / replies / views. Verify the order against a known post before comparing positions across posts.
2. Trust raw per-post fields. Never trust TYPE labels, window subtotals, or roster totals from the source documents.
3. Represent post type as three independent booleans: reply, quote, repost. Original is the absence of all three.
4. Compute counts by script from the booleans. The model does not hold a tally.
5. Count only posts actually printed. Where a file states a higher number than it lists, report the listed count and flag the gap.
6. Name any truncated window in the limitations note. "Full list follows pattern" never stands in for unextracted posts.
7. When a complete pull contradicts a partial one about a specific post, log it as an extraction discrepancy.

## Traits

8. A behavior on one day is an observation. Assign a trait only if it survives multiple days under different news conditions.
9. Assign no trait to an account with fewer than 3 posts across the corpus. State insufficient volume and stop.
10. Name the condition most likely to break a proposed trait. Report only traits that survive it.
11. Describe behavior. Do not infer intent or motive from engagement.

## Absence

12. Absence is a finding only against a baseline of presence.
13. For any near-zero account, name what the roster discussed on peak days and state whether that gave the account a direct reason to engage.

## Structure

14. Separate topic similarity from communication structure. Accounts on the same subject who never engage are not connected.
15. Find bridges by listing each account's topic networks and flagging any account that uniquely spans two, whether or not it interacts with either.
16. Test connectedness through shared edges, not pairwise overlap with all, or a real chain reads as disconnected.
17. Split each referent into entity and aspect, so same-entity, different-aspect cases surface instead of collapsing into one conversation.
18. Check the granularity of any model-chosen category before trusting a count built on it.

## Scope

19. Corpus only. No outside facts, including real names, company nicknames, or product names not written in the posts. Refer to accounts by handle.
20. Apply the corpus constraint hardest on open-ended prompts, where breadth invites outside fill.
21. Override by user prompt: the operator may explicitly permit outside data. Permission to use outside data is not permission to fabricate.

## Reporting

22. Every characterizing claim carries handle plus POST_ID. Uncited claims are dropped, not softened.
23. State engagement as position within the account's own range.
24. Grade output against what the prompt actually said.

## Why procedures

Every rule is written as a step to run or a comparison to make. Rules written as definitions ("a trait is a stable behavior") were recited back and not applied. Rules written as procedures ("assign a trait only if it survives multiple days") were executed.
