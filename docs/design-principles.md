# Design Principles

Rules that held across repeated runs.

## Capture

- Put structure into capture, not selection. Every descriptive field, no gates.
- Filter at read time. A cut at the pull discards data you can't get back.
- Favor capture over discard when the loss would be silent.
- Scale by heavy-account count, not headcount. Per-turn load is the sum.

## Trust

- Verify raw fields where possible; generated labels, subtotals, and summaries are suspect until rebuilt from verified raw.
- Treat any tool's "complete" as unverified.
- Mechanical checks belong in code, not in a prompt.
- When a present-day claim looks too strong, verify it against outside sources. Don't rule it out by instinct either.

## Prompting

- Fix failures structurally: window, chunk size, output format. Wording can't move a page size or a context limit.
- Write rules as procedures to run, not definitions to recite. Models execute steps and ignore characterizations.
- Make the output format enforce the rule. A line that can't exist without a POST_ID stays cited.
- Put durable analytical discipline in the knowledge base, not the prompt.

## Analysis

- Describe behavior; leave cause open. Engagement counts don't show motive.
- Separate output from reach. High-follower accounts get distribution regardless of what they post.
- Don't average bursty output. A weekly mean hides the shape of dumps and silence.
- Topic similarity is not connection. Accounts on the same subject who never engage are separate conversations.
- Check the granularity of any category before trusting a count built on it.

## Roster

- A slot is earned by posting signal at usable volume, not by fame.
- Cut accounts that don't post. Prefer the account posting analysis over the principal's promotional feed.
- Size the roster to the goal. A test rig needs the smallest set spanning the axes.
