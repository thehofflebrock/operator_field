# Capture Prompt: DRAGNET Raw Post Pull

Current version (June 16, 2026). History in [history/](history/).

ACCOUNTS: [roster]

DATE: [YYYY-MM-DD] (UTC)

ENGAGEMENT order: likes / reposts / quotes / replies / views (bookmarks not pulled)

Tool: x_keyword_search, Latest mode. Per account, query from:HANDLE since:DATE until:DATE+1. Paginate with max_id whenever a page returns 10 results; stop when a page returns fewer than 10 or repeats IDs. Complete one account fully before starting the next.

Segment each account's day into four windows by post timestamp (UTC): 00:00–06:00, 06:00–12:00, 12:00–18:00, 18:00–24:00.

For every post emit exactly these fields, verbatim, no summarizing, no invention, no thread-fetching:
- TEXT: [exact text, or "MEDIA ONLY" if none]
- POST_ID: [the status ID of this post]
- CONVERSATION_ID: [the conversation_id the tool returns, else UNAVAILABLE]
- QUOTED: [verbatim text of the embedded quoted post, else NONE]
- REPOST_OF: [original author handle if pure rebroadcast with no added text, else NONE]
- URL: [full status URL]
- TIME: [YYYY-MM-DD HH:MM:SS UTC]
- ENGAGEMENT: likes / reposts / quotes / replies / views

Read these from post structure, never from what the text says. Capture what the tool returns and stop; do not infer a relationship the fields don't show. Emit no reply/quote/repost boolean and no "original" label. Type is computed downstream from the raw fields:
- QUOTED present is a quote.
- REPOST_OF present is a repost.
- CONVERSATION_ID == POST_ID is a root post.
- CONVERSATION_ID != POST_ID and the conversation root is itself a POST_ID in this account's pull is a self-thread continuation.
- CONVERSATION_ID != POST_ID and the root is not in this account's pull is a reply to another account.
These compose: a post can be a reply that also quotes. Never drop a post for ambiguous structure.

Do NOT emit any per-window subtotal, per-account total, or roster table. Raw post rows only. Counts are computed downstream from the fields. End with a Limitations note covering: tool and mode, pagination behavior, which windows were empty, and any window where extraction was truncated by output volume (name the window explicitly and say extraction was incomplete; never substitute "full list follows pattern"). Include these two standing lines every run:
- x_keyword_search does not expose in_reply_to; reply detection runs off CONVERSATION_ID against the account's own POST_ID set, not off a reply pointer.
- A reply or self-thread continuation whose root fell outside the pull window reads as a reply to another account: correct that it is not a root, but its root text is absent and a same-account root from a prior window will not be matched.
