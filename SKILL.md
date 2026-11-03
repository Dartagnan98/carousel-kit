---
name: carousel-kit
description: "Locked-design Instagram carousels using the user's own name, photos, clay figure and colours: clay (clay figure soft-card deck), twitter / twitter-photo (Hormozi-style tweet cards). Only the content varies, the design never does. Use when the user says 'carousel-kit' or '/carousel-kit', or asks to make a carousel from this kit."
metadata:
  version: 1.0.0
---

# Carousel Kit

Locked-design Instagram carousels. You pick the template, write the content, render. Never redesign, never ask about fonts or colours. Every template is 1080x1350 (4:5), exported at 2x.

## Install (once per machine)

```bash
git clone https://github.com/Dartagnan98/carousel-kit.git ~/.claude/skills/carousel-kit
cd ~/.claude/skills/carousel-kit && npm install && npx playwright install chromium-headless-shell
```

The skill then answers to `/carousel-kit`. Needs Node 18+.

## Make it yours (once)

- `assets/profile.js`: name, handle, blue check, colours, image paths.
- `assets/profile/avatar.png`: real photo, square.
- `assets/photos/photo-1..3.jpg`: 1080x1350 photos for tweet decks.
- `assets/figure/*.png`: the four clay poses. `GUIDE.md` covers photos, the clay figure and how to organise files.

Before rendering, read `assets/profile.js`. If it still says "Your Name", or the chosen style uses images that are still placeholders, tell the user what is missing and point them to the list above. Placeholders are fine for a test, never for a post.

## Invocation

`/carousel-kit [style] [topic]`. Both are optional. The process below runs every time, in this order: **topic, then writing, then carousel style, then render.** Never skip a phase, never reorder them. The design is locked, so the copy is the whole game and phase 2 is where the deck is won or lost.

## Phase 1: Topic

Lock three things before writing a word. Infer them from the request when you can; if the topic itself is missing, ask exactly one short question and stop.

1. **Reader.** One person, in the user's market. If you do not know who they sell to, ask.
2. **The one thing they get wrong**, in their words. Not a category ("pricing"), a mistake ("they trust the app's number").
3. **Awareness.** Does the reader know the problem, know solutions exist, or know the user already? Problem-aware readers need the copy to reveal HOW this is different. Solution-aware, jaded readers need the mechanism and the outcome first, never the promise alone.

Write the three down in one line in your reply ("Reader: … / Mistake: … / They know: …"). If you cannot name the mistake, the deck has no angle yet. Find one before continuing.

Grounding: every fact comes from the user's own material. Never invent a number, a stat, a testimonial or a deadline. If a claim needs one you do not have, write the line so it does not need it.

## Phase 2: Writing

Run this whether or not a copywriting skill is installed. If `copywriting` (Copy OS) or `hormozi-brain` is available, use it for the same steps; the bar below still applies.

**2a. Hook.** Write at least 8 cover headline candidates across different shapes:
- the reader's own deadline ("Your [thing] gets one first [moment].")
- cost of the mistake ("Get this wrong and you find out in week one.")
- contrast ("People spend weeks on X and five minutes on Y.")
- mechanism reveal ("Do it tonight, not Thursday.")
- specific count with a gap ("Six kinds of leads. One follow-up fits one of them.")
- the tangible list ("One event. 30 doors, one draw, zero lost leads.")

Score every candidate 0 to 10 on each of Bly's 4 U's and total out of 40: **Useful** (a clear benefit), **Urgent** (a reason it matters now: the reader's own timing counts, fake deadlines do not), **Unique** (could a competitor post it word for word?), **Ultra-specific** (numbers, names, tangible details). Keep only candidates at **30 or above**. If nothing clears 30, the angle is wrong: go back to Phase 1 and pick a different mistake. Do not polish a loser.

Rules for the winner: one idea, under 9 words, no label headlines ("5 pricing tips"), no clever wordplay the reader has to decode, no hype words (game-changer, unlock, elevate, secret, hack). The cover sub-line finishes the thought and promises the list ("The 5 checks I run first.") so the swipe has a reason.

**2b. Slides.** One idea per slide. Headline = the move in 3 to 5 words. Sub = why it matters, first person, the user's voice. Card = the concrete specifics (days, numbers, names). Every slide needs at least one specific. If a slide needs a second read, split it or cut it.

**2c. CTA.** The last slide is a comment CTA built from all five parts: **command** (Comment), **the word** (one capitalised word), **benefit** (what the user sends or does: "I will send the full breakdown"), **ease** (free, two minutes to read), **why now** (this fall, before your next launch). Only promise things that exist. Never "DM me for more info", never "link in bio", never two CTAs.

**2d. Voice.** First person, short sentences, plain words. Direct about numbers. No em dashes, no exclamation marks, no AI cliches, no rule-of-three filler.

**2e. Gate, before you touch a template.** Answer each in one word in your head; any "no" means rewrite:
- Would a stranger stop scrolling on the cover?
- Does the cover promise the list?
- Does every slide earn the next swipe with one specific?
- Does the CTA have all five parts and a real payoff?
- Is every fact from the user's material?
- Card lines 38 chars or fewer, cover headline 3 lines or fewer, sub 2 lines?

State the winning hook's 4 U's score in your reply, one line.

## Phase 3: Carousel style

Now pick the container. Use the style word if the user gave one; otherwise pick by fit and say which you picked.

| Word | Template | Use it when |
|---|---|---|
| `clay` | `template-clay.html` | Education, step-by-step. Clay figure, brand colour cover + outro, off-white body, accent colour, Open Sauce Sans Black headline, soft white card, pill kicker, counter, photo + handle footer. |
| `twitter` | `template-tweet.html` | A punchy opinion or a short list, every slide plain white. Big header (photo, bold name, blue check, grey handle), one statement in Inter. |
| `twitter-photo` | `template-tweet.html` | The same, every slide on a photo (`photo-1/2/3.jpg`, rotate), white card bottom-left, circle arrow on the cover only. Best scroll-stopper for a hook-led list. |
| `tweet` (alias `x`) | `template-tweet.html` | Mixed: you decide `photo:` per slide. |

Map the copy into the template's `slidesData` shape (formats under "Writing content" below). Clay carries kicker, headline, sub and card per slide; tweet carries one text block per slide, so merge headline and sub into one statement there.

## Phase 4: Render and check

1. Copy the template to a working file (never edit the templates):
   ```bash
   cp ~/.claude/skills/carousel-kit/template-clay.html /tmp/carousel-<slug>.html
   ```
2. Rewrite only the `slidesData` array inside the EDIT ZONE.
3. Render:
   ```bash
   cd ~/.claude/skills/carousel-kit && node render.mjs /tmp/carousel-<slug>.html /tmp/carousel-<slug>-out
   ```
   Output: `slide-01.jpg … slide-NN.jpg`. `node render.mjs clay|twitter-photo /tmp/out` renders the sample deck baked into each template. `carousel-assets/` inside any deck always resolves to this skill's `assets/`, so copied decks render from anywhere.
4. The clay layout self-corrects: if a slide's copy is long, the layout drops the spark, then shrinks the sub, then the headline, then the card spacing, then scales the card, until the card clears the footer line. The renderer re-measures every slide afterwards and exits with `OVERLAP: … slide(s) N` if anything still touches the footer. Treat that as a copy problem: trim the named slides and re-render. Never patch CSS around it. Then look at every slide anyway: no headline past 3 lines, no text behind the figure, nothing shrunk so far it looks like a different deck (if the guard had to scale a card, the copy is too long).
5. Open the slides for the user (`open /tmp/carousel-<slug>-out/*.jpg`) and report in three lines: Reader / Mistake / They know, the winning hook with its score, and the CTA word.

## Writing content

### clay (`slidesData`)

```js
{ cover:true,                 // first slide only
  outro:true,                 // last slide only
  kicker:'STEP 01',           // becomes an accent pill
  hl:'First <span class="o">move</span>',   // headline, auto-uppercased, <span class="o"> = accent punch words
  sub:'Why this step matters. <b>The part to remember.</b>',   // Recoleta serif, <b> = ink
  term:[                      // the soft card: title, bullets, check. Symbols in the data are ignored.
    ['c','','','First move'],                   // 'c' = card TITLE in Recoleta: a human phrase, NEVER a fake command
    ['a','','','One concrete detail'],          // 'a' = dot bullet (sentence-cased automatically)
    ['ok','','','The result of this step'] ] }   // 'ok' = bold accent check line
```

Rules:
- 7 slides ideal (cover + 5 + outro), 5 to 9 allowed. Counter and figure pose are automatic (cover = point up; body rotates present / point-left / thumbs-up).
- Card lines 38 chars max, 3 to 5 lines. No `$`, `--flags`, arrows or `->` anywhere.
- Cover headline 3 lines max (about 8 words), sub 2 lines. The layout script tightens gaps on overflow, but a 4-line headline plus a 3-line sub still crowds the bottom: trim copy, do not shrink type.
- Outro = a comment CTA (`Comment <b>"WORD"</b> …`).
- No em dashes. No AI cliches.

### twitter / twitter-photo / tweet (`slidesData`)

```js
{ photo:'photo-2.jpg', text:'1) First point, in one plain sentence.' }   // photo card
{ text:'Want the full version?\n\nComment WORD.' }                          // plain white
```

- `\n\n` = paragraph break. Text auto-shrinks to fit (plain 88px down to 46px, card 42px down to 30px).
- Cover text: hook + "The N things …:" then one point per slide, last slide = CTA. Keep each slide under about 30 words.
- `twitter` = no `photo` keys at all. `twitter-photo` = a photo on every slide, rotating 1/2/3.

## Assets

`assets/profile.js` (who and what colours), `figure/*.png` (the four clay poses), `profile/avatar.png`, `photos/photo-1..3.jpg`, `open-sauce-sans-black.ttf`, `recoleta.otf`, `inter.woff2`. Setup, photos and the figure: `GUIDE.md`.

## Posting to Instagram

Only when the user asks to post. Posting is public and irreversible.

1. Open the exported slides for the user, then ask: *"This posts to <handle from profile.js>. Reply POST to submit, or tell me what to change."* Wait for an explicit **POST**. Never publish on a vague "looks good".
2. Composio, connected once with `composio link instagram`. Get the account id: `composio execute INSTAGRAM_GET_USER_INFO -d '{}'`.
3. Caption: short hook, the points, the outro CTA wording. Max 2,200 chars, max 30 hashtags.
4. Create the container (2 to 10 JPEGs, in order):
   ```bash
   composio execute INSTAGRAM_CREATE_CAROUSEL_CONTAINER -d '{"ig_user_id":"<id>","caption":"<caption>","child_image_files":["/tmp/out/slide-01.jpg","..."]}'
   ```
5. Publish only after the POST gate: `composio execute INSTAGRAM_POST_IG_USER_MEDIA_PUBLISH -d '{"ig_user_id":"<id>","creation_id":"<id>"}'` and report the permalink.

Pitfalls: JPEG only, 4:5 (the renderer already enforces 1080x1350); one bad file fails the whole container; publishing the same creation id twice returns 409, make a fresh container.
