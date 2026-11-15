---
name: book-author-videos
description: Daily pick from a reading list with three verified videos of the author talking about that book (~10, 20-45, 60+ min), plus feedback and learning-note capture. Use to set up, run or tune a daily book-author video digest.
---

# Book author videos

Every day, pick one book from the user's reading list and find three videos where **the author personally talks about that book**: one around 10 minutes, one of 20 to 45 minutes, and one over an hour. Verify each one, say plainly where there is no match, and capture the user's feedback and learning notes.

Keep code to a minimum. This skill is judgement guided by rules and examples, plus a few small state files.

## 1. Setup (first use only)

Ask the user these, in one message, with defaults in brackets:
1. **Reading list:** a Notion page, a file, or a pasted list. Each book may carry an optional "source", such as who recommended it or which course or session it came from.
2. **Time and time zone** for the daily post. [8:00 AM local]
3. **Optional context** for the "why it matters to you" line: goals, role, course, or personal decision rules. [none]
4. **Where to post:** this conversation or thread. [here]

Then:
- Create a state folder (default `book-videos/` in the user's working or project folder) holding:
  - `picks.md`: the log table `| Date | Round | Book | Author | ~10 min | 20-45 min | 60+ min |`
  - `notes.md`: the feedback and learning-notes table (section 6)
  - `books-cache.md`: a copy of the reading list with sources, used if the list's source is unreachable
  - `rules.md`: the user's own rule changes and calibration examples, which win over this skill's defaults
- Create a daily scheduled task at the chosen time. Use a few minutes before the hour, because on-the-hour runs can be delayed. Its prompt: "Daily run: follow the book-author-videos skill, section 2. State folder: <path>."
- Run once straight away, so the user sees a sample and can calibrate.
- Never tick or edit the user's reading list itself. Checkboxes there usually mean "read".

## 2. Daily run

1. Read `rules.md` (the user's overrides), `picks.md` and `notes.md`.
2. If the last run has no row in `picks.md`, say so briefly in today's post.
3. Read the reading list. If it is unreachable, use `books-cache.md` and say so. If it is reachable and changed, refresh the cache.
4. Pick one book at random that isn't yet in `picks.md` for the current round. When all are covered, start the next round and say so.
5. Search and verify (sections 3 and 4).
6. Post (section 5).
7. Append the row to `picks.md` only after posting.

## 3. What counts

- **The author's own voice on THIS book** is the bar. A co-author counts.
- **Topic relevance is non-negotiable.** The book or its core framework must be the subject in the description or chapters, not just in an episode or channel name. Clips cut from longer episodes need their own topic check.
- **Video with the author on screen is preferred.** An audio interview of the author about the book still counts.
- **Talks from before publication** generally don't count, even if they cover the same ideas.
- **Never:** summaries, audiobooks, reviews, reaction videos, or other people discussing the book.
- **Length buckets:** about 10 min = 5 to 18; middle = 20 to 45; long = 60+.
- **Empty buckets:** a nearest match may fill one only as a clearly labelled fallback. Fallbacks must be neutral, with no commentator opinion, such as a straight summary or a filmed reading. If nothing fits, say "no match".
- **Unconfirmed videos:** anything promising but not fully confirmed goes under "Couldn't verify", never in a slot.

### Calibration examples (from the first user, Nov 2026, *The Beginning of Infinity*)
- **Best:** "chat with David Deutsch about The Beginning of Infinity". The author himself, the whole conversation about the book.
- **Good:** the On Point (WBUR) launch interview. Audio only, but the author on the book.
- **Good:** "The Beginning of Infinity is coming to Germany". Video, author present, about the book.
- **Worst:** a podcast clip whose episode is named after the book, but whose clip is about another topic. Reject anything like it.

## 4. Search and verification

**Search before settling.** Run many queries, using web search limited to youtube.com plus open searches:
- book title + author
- author + talk, interview, podcast, keynote or lecture
- "<author> keynote full"
- "<author> full interview <book> podcast video"
- Talks at Google, RSA, book-launch radio shows from the publication year, university lectures, translated-edition interviews
- the author's own channel or website video page

Long podcast interviews are the main source for the middle and long slots.

**Verify three things for every slot: speaker, book and length.**
- **YouTube pages are often blocked or rate-limited for agents.** Fetch them one at a time, at most 3 tries per run.
- **Speaker and topic:** look up the video ID on summary or transcript sites (videohighlight.com/v/<ID>, rosetta.to, glasp.co/youtube/<ID>). There must be positive evidence the author is the speaker or guest. If the transcript refers to the author in the third person, reject it.
- **Length:** take it from the show's own listing or a transcript site (podwise.ai, podcasts.happyscribe.com via the last timestamp, metacast.app, ted.com, the show's website). Podcast audio length is close enough to the YouTube upload. If only a lower bound is known (from chapters), use it for the long slot only.
- **If a browser or yt-dlp is available,** use it for metadata only: `yt-dlp --skip-download --print "%(title)s|%(duration_string)s|%(channel)s"`.
- **Links:** only post URLs that appeared in this run's search or fetch output. Check every video ID before posting. Never type or guess an ID.
- **Budget:** about 30 page checks, then post what is verified and name the gaps.

## 5. Post format

1. **📚 Title, by Author**
2. **Why it matters to you** (2 to 4 sentences): what the book argues, tied to where it came from (the list's source field) and to the user's optional context.
3. **The three slots:** each with a linked title, channel, length, year, and one line on what the author covers. Fallbacks are labelled.
4. **Couldn't verify:** only if there are any.
5. **▶ Watch first:** which one to start with, and why.
6. **🎧 Listen for:** one key idea.
7. **Feedback prompt:** "Reply with 👍 or 👎 for each video, and a 2-3 line note on what you learned."

Keep the post short and skimmable.

## 6. Feedback and learning notes

When the user replies to a post, add a row to `notes.md`:
`| Date | Book | Video | 👍/👎 | Note (user's words) | Depth 0-4 | Why that score |`

**Depth rubric** (default; the user can change it in `rules.md`):
- **0:** no note
- **1:** recall, restating a fact or that they liked it
- **2:** explains the idea in their own words
- **3:** applies it to their own work, a decision or a situation
- **4:** connects it to another idea or book, or questions it

Score the note itself, not how long it is.

**Weekly** (the first run of each week): add a short line to the post with last week's thumbs-up rate, average depth, and which slot length gets the most 👍.

**Feedback changes behaviour:** a 👎 with a reason becomes a calibration example in `rules.md`. Ask before turning a pattern into a new rule.

## 7. Failure points and fallbacks

| What fails | Do this |
| --- | --- |
| Reading list unreachable | Use `books-cache.md` and say so |
| Book has few or no author videos | Run the full query set, fill only verified slots, name the gaps, offer a neutral fallback |
| YouTube blocked or rate-limited | Use mirror sites and podcast listings; never fetch in bursts |
| Someone other than the author is speaking | Reject without positive evidence of the author |
| Off-topic clip | Topic check on the clip itself |
| Length unknown | Show listing or transcript; lower bound only for the long slot; else "Couldn't verify" |
| Wrong or invented link | IDs only from results, checked before posting |
| Run takes too long | Post at about 30 checks with the gaps named |
| A run fails midway | The log is written after posting, and the next run flags the gap |
| All books covered | Start the next round and say so |
