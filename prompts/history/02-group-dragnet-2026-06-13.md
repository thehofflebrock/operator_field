# DRAGNET — RAW POST PULL (GROUP 1)

> Superseded. Emitted model-assigned TYPE labels and per-account totals, both of which drifted from the raw rows.

ACCOUNTS (one fully complete before the next):
@pmarca, @ylecun, @sama, @DarioAmodei

DATE: [YYYY-MM-DD] (UTC)

For each account, run four searches, one per 6-hour window:
  00:00–06:00, 06:00–12:00, 12:00–18:00, 18:00–24:00

Each search: x_keyword_search, Latest mode, from:handle with the window's since/until bounds, limit=10. If a window returns 10, run a follow-up with max_id = the oldest post's ID and keep paginating until a page returns fewer than 10 or only duplicates. Ten is a page, not a total.

List EVERY post returned. Replies, quotes, reposts included. No filtering, no length cut, no signal judgment. Empty window is a valid result. Do not invent posts to fill a window.

Per post:
- TEXT: verbatim, no summary or truncation. Bare repost: REPOST (NO ADDED TEXT). Media, no text: MEDIA ONLY (image / video / link card).
- TYPE: ORIGINAL | REPLY | REPOST | QUOTE
- PARENT: quotes and reposts, capture parent handle + text + URL. Replies, capture only if it surfaces in the result, else PARENT: UNAVAILABLE. Do not thread-fetch.
- URL, TIME, ENGAGEMENT (likes / reposts / quotes / replies / views, else UNAVAILABLE)

After each account: [@handle: N posts | Xo / Yr / Zrep / Wq]
After the last account: roster table of totals.

(Groups 2, 3, and Elon used the identical body with different account lists.)
