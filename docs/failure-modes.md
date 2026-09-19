# Failure Modes

Every failure below reported success. None threw an error.

## 1. Fabrication

**What happened:** In-app Grok, asked for a full day, returned posts that did not exist. The gap between what it retrieved and what it expected a day to hold got filled with plausible content.

**Documented instance:** June 14, group G14. The retrospective reports fabricated output. The inspected source preserves an all-empty replacement and the line "That's the truthful version," but not the withdrawn output; its original contents are not independently established by that source.

**Tells:**
- Status IDs that disagree with timestamps when both are sorted. Real X IDs are time-ordered.
- Sequential digit runs inside IDs, which real IDs don't produce.
- Generic content that could belong to anyone.
- No match for the same ID in an independent pull.

**Fix:** Narrow the window so there is no gap to fill. Verify by ID monotonicity, then cross-match specific IDs against a second pull. Click the URL on anything the analysis will build on. Monotonic IDs can expose some inconsistencies; cross-pull matches check agreement on specific records, not the authenticity or completeness of a whole run.

**Contamination rule:** Posts whose IDs sort after the genuine last post of a window are excluded and held for the correct day, preventing double counting.

## 2. Truncation

**What happened:** The API returned partial pulls on high-volume days and presented them as complete.

**Documented instance:** June 14. First pull: pmarca 10, Marcus 9, claimed that fewer than 10 results had returned and no pagination was required; pmarca's 00:00-06:00 window marked empty. Second pull: 22 and 32. The "empty" window held 10 posts. See [case-study.md](case-study.md).

**Tell:** A heavy account reading thin or absent on a busy day. Halving a window and finding the halves sum higher than the whole.

**Fix:** Paginate to exhaustion. Treat "complete" from any tool as unverified. Push a second pull on high-volume days as standing habit. Name truncated windows explicitly in any report and suspend verdicts that depend on them.

## 3. The false ceiling

**What happened:** Every account returned exactly 10 posts.

**Fix:** 10 was the default page size. Looping with `max_id` removed it.

## 4. Generated labels contradicting raw text

**What happened:** The model assigned each post a single type (reply, quote, repost, original). "Reply to a quoted post" landed in different buckets on different days, which manufactured trends that weren't there.

**Fix:** Verify raw fields where possible, then derive labels from verified raw material. Replace the single label with three independent booleans. Original is the absence of all three. Counts are computed by script from the booleans, never tallied by the model. Where a file claims more posts than it lists, report the listed count and flag the gap.

## 5. Cross-account contamination

**What happened:** A generated position card for @pmarca (June 10) cited three status IDs belonging to @elonmusk and attributed Elon's comments to pmarca.

**Tell:** An ID appearing under two accounts. IDs are globally unique.

**Fix:** Cards cite only IDs present in that account's own raw pull. Collision checks belong in a script.

## 6. A capture gate that deletes signal

**What happened:** The June 10 gated capture marked @DarioAmodei and @GaryMarcus SILENT. Dario had posted a six-part essay thread (7M+ views); Marcus posted seven times.

**Fix:** No gate at capture beyond the no-subject floor. Filter at read.

## 7. One account wearing a topic label

**What happened:** Buckets that looked field-wide (politics, space) were mostly a single high-volume account.

**Fix:** Remove the loudest author and re-check. A bucket lives by multi-author recurrence, or by a single author with a multi-day subject nothing else can house.

## 8. One account warping the whole dataset

**What happened:** One account out-posted the rest of the roster combined. Every aggregate was shaped by him.

**Fix:** Treat that account as the environment, not a participant. Exclude it and substitute normal-volume accounts from inside the same organizations so their subject matter keeps its share. Run one clean week before rebuilding the roster against a picture that isn't distorted.

## 9. One day posing as a trait

**What happened:** A single thread made an account look like it had a standing pattern.

**Fix:** A behavior on one day is an observation. It becomes a trait only after surviving multiple days under different news conditions, and only after naming the condition most likely to break it.

## 10. The refinement loop

**What happened:** Passing prompts between models for review kept adding complexity, and the next pass spent its time removing it.

**Fix:** Track outputs produced against hours spent. Prompts certify the output, not the process. Strip scaffolding built for cases that don't exist.
