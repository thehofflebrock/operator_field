# Reddit Daily Capture

The same method ported to Reddit film communities. Note the self-checks: header counts must equal listed entries, and the FLOOR CHECK set must equal the set of summarized posts, by construction. A mismatch is flagged, never reconciled silently.

---

PARAMETERS (set per run): SUBREDDIT, DATE, COMMENT_FLOOR

ROLE: Capture one day of posts from r/[SUBREDDIT], sorted New. Record each surviving post as a concrete subject with the poster's stance. For posts whose num_comments meets or exceeds the floor, add one paragraph summarizing what the comment thread argues. Do not sort into topics, assign topical categories, or add a reading the post does not support.

METHOD (use the JSON API, not HTML pages):
- Pull https://www.reddit.com/r/[SUBREDDIT]/new.json?limit=100
- Page backward with the &after= token. Continue until a page's posts are all older than the window start. One page is not the window.
- Do NOT use HTML, search, or old.reddit; they return loading placeholders and empty states. On error or empty children, retry the same JSON URL.
- Read id, title, selftext, created_utc, num_comments, score, upvote_ratio, permalink.
- For num_comments >= COMMENT_FLOOR, pull /comments/[ID].json and summarize from actual comment bodies.

WINDOW: [DATE] 00:00 to 23:59 UTC by created_utc.

PULL STAMP: Record PULL_UTC. Posts created in the final 3 hours before pull are flagged LATE-WINDOW so a low count reads as age, not lack of interest.

THE FLOOR IS num_comments. Not score, not upvotes. No "borderline."

HEADER: TYPE: REDDIT-DAILY · SUB · DATE · WINDOW_TZ: UTC · PULL_UTC · COMMENT_FLOOR · POSTS_CAPTURED (equals entries listed) · POSTS_AT_OR_ABOVE_FLOOR (equals summaries written, equals FLOOR CHECK length)

NOISE FLOOR: Drop only posts with no subject of their own. Thinness is not a cut. Capture wide; this day cannot be re-pulled.

CAPTURE: every surviving post, ordered by num_comments descending (verify before printing). Per post: POST_ID + permalink, CREATED_UTC (+ LATE-WINDOW), SCORE / COMMENTS / UPVOTE_RATIO, TYPE (one structural tag: debate, technical-question, feedback-request, showcase, crew-or-classified, tutorial, news-link, other), THE FACT (poster's stance attributed, no laundered judgment), SUMMARY OF TOP COMMENTS if and only if at or above floor.

FLOOR CHECK: list IDs at or above floor. Must equal the set with summaries. If not, flag the mismatch explicitly.

RULES: Do not infer or reconstruct posts. Do not fill the day to a size it "should" be. Output only the report.
