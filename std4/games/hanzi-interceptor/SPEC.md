# Hanzi Interceptor — game specification

A working reference for rebuilding this gameplay with different subject content and a
different visual identity. Written to be pasted into a fresh chat as the opening brief.

Built for a Malaysian SJKC Year 4 pupil (pre-high-school). Current build: one self-contained
HTML file, ~269 KB, 544 question-bank items across 35 sections, Chapters 5–11 of the History
syllabus.

---

## 1. What the game is

A single-file browser arcade game for exam revision. A prompt in Chinese appears in a panel at
the top of the screen. Its possible English meanings drift down the playfield as targets. The
player flies a small ship along the bottom and shoots the one that matches.

The whole design rests on one principle: **the arcade layer must never punish the player for
anything other than a knowledge mistake.** Every time the two got tangled, the game got worse
and had to be fixed. That principle is worth carrying over intact.

---

## 2. Core loop

1. Prompt appears in the band at the top, with pinyin above it.
2. Three or four answers fall, **each in its own vertical lane**.
3. Player moves left/right and fires upward.
4. Correct hit → points, possible upgrade pod drop, next prompt.
5. Wrong hit → lose a shield, and the item is marked *flawed*.
6. Correct answer falls off the bottom → lose a shield, item returns to the deck.

**Identify ten to clear a level.** There is no fixed run length; levels continue until shields
run out. Clearing a level repairs one shield and pays a bonus of 300 × level.

**Only a clean first-time hit counts toward the ten.** If the player shot a wrong answer first,
they can still take the correct one for points, but it does not count, and the item slides back
into the deck to reappear 3–6 turns later. This is the single most valuable mechanic in the
game: a level cannot be cleared while the player is still fumbling any of its items. It turns
an arcade score into a mastery loop.

---

## 3. Rules in full

**Shields: 4.** Only two things cost one:
- shooting a wrong answer
- letting the correct answer fall off the bottom

Nothing else. Wrong answers drifting past cost nothing, so there is never a penalty for
reading carefully. Asteroid collisions cost firepower, not shields (see §7).

**Scoring**
- 100 base per correct answer, plus up to 90 for speed
- 3 correct in a row starts a multiplier, rising to ×5
- level clear bonus: 300 × level
- asteroid rocks: 15 large / 25 medium / 40 small

**Pace**
- ~15 seconds per item at level 1, tightening ~1.15 s per level, floored at ~5.6 s
- Three difficulty tiers: Cadet (3 options, 0.78× speed), Pilot (4 options, 1.0×),
  Ace (4 options, 1.32×, pinyin hidden)

**Deck**
- Items are drawn from a shuffled deck of the chosen section; the deck reshuffles when empty,
  so small sections legitimately repeat (good for a 3-item section)
- Missed items are re-inserted 3–6 positions from the end

---

## 4. Screens

- **Landing** — title, one-line explanation, section picker, Launch and Study as two large
  buttons, four small ones (Hangar, How to play, Settings, Library), and the top 5 scores.
  Keep this sparse. It grew into a dashboard once and had to be cut back.
- **Game** — HUD (shields, cannon level, score, streak, level, cleared n/10, pause, abort),
  question band, playfield.
- **Debrief** — score, level reached, correct/total, accuracy, best streak, personal best,
  credits earned; name entry if the score makes the board; the board; then **Fly again**,
  **Drill the misses**, **Study the misses**, **Back to base**; then the **review log** listing
  every missed item with its correct meaning and what the player shot instead.
- **Study mode** — the same bank, two views. *Cards*: one item at a time, meaning hidden behind
  a blur until revealed; arrows move, space reveals, L reads aloud; progress bar. *All at once*:
  the whole section as a list, prompt beside meaning, grouped under section headings, speaker on
  every row. A "Fly this section" button hands straight to the game with the same section.
- **Hangar** — credits balance, four ships, six accessories with two fitting slots.
- **Settings** — difficulty tier, display/sound toggles, reading speed, music volume.
- **How to play** — sample target, controls, run rules, pod legend, asteroid and credit rules.
- **Library** — add/edit/delete bank items, bulk paste import, export, reset.

---

## 5. Question bank — data model

One flat array of objects. Everything else derives from it.

```js
{
  zh:    "气温急剧下降",                  // the prompt shown on screen
  py:    "qì wēn jí jù xià jiàng",        // pinyin, shown above it
  en:    "a sharp drop in temperature",   // the correct answer
  wrong: ["a slow rise in temperature",   // explicit distractors (optional)
          "a sharp rise in temperature",
          "a steady temperature"],
  tag:   "5 · Terms & Phrases (Ice Age)", // the section; drives the dropdown
  kind:  "phrase"                         // "term" | "phrase" | undefined (= sentence)
}
```

**Three item kinds, three display sizes.** A single term renders largest with a gold TERM
badge; a phrase renders mid-size with a blue PHRASE badge; a full sentence renders smallest
with no badge. The player can see what they are facing before reading it.

**Sections are the unit of study.** Tag naming controls sort order in the dropdown, so prefix
with the chapter number and use a space before the separator to sort a chapter's
terms-and-phrases section above its numbered sections:

```
5 · Terms & Phrases (Ice Age)     ← sorts first within chapter 5
5.1 Understanding the Ice Age
5.2 Post-glacial Timeline
```

**Distractor filling.** If `wrong` has fewer than needed, top up from other items **in the same
section first**, then from anywhere. A distractor from a different chapter is too easy to be
useful.

---

## 6. Content rules — the pedagogy

These matter more than any mechanic.

**The answer must be a faithful rendering of the prompt on screen.** This was violated across
58 items and had to be fixed. The prompt was 公元一世纪 and the answer was "the Kingdom of
Funan" — which is an answer to an unseen question, not a translation, and study mode was
teaching it as a meaning. Where the source book carries an attributor in a table's left column
(a century, a person, an office, a heading), that column belongs in the **prompt**, not only in
the answer:

- 公元一世纪：扶南 → "1st century AD — the Kingdom of Funan"
- 大臣：由苏丹委任 → "Minister — appointed by the Sultan"
- 宰相：百官之首 → "Bendahara — head of all the officials"

This does not make it easier, because the distractors share the same shape.

**Distractors must be near-misses along one dimension.** 气温急剧下降 competes with *a sharp
rise*, *a slow rise*, *a steady temperature* — so the player must read 急剧 and 下降, not merely
recognise "temperature". Never let a distractor be dismissible by topic alone.

**Segment long sentences into their phrases and add them as separate items.** A 26-character
sentence is hard to read under time pressure and teaches nothing about its parts. Pulling out
气温急剧下降 and 覆盖着厚冰层 gives the child the building blocks. Grammar connectives are the
most valuable of these — 进而导致, 凭借, 得以发展, 误以为, 被迫离开, 是指 — because they are what
makes a textbook sentence hard to parse. Target sentences of ~18+ characters.

**Correct the source when it is wrong, and say so.** The History notes rendered 冰层 as "rock
water" throughout (a literal take on Malay *air batu*), 白鼠鹿 as "white moth" instead of white
mouse-deer, and 有首领作为领导 as "Having a leader as a leader". Clean English was used and the
parent was told each time. A garbled English answer teaches nonsense.

**Validate the whole bank before every publish.** Three checks, all of which caught real
problems:
1. **No duplicate prompts.** Same prompt with two different answers is an ambiguous question.
2. **No duplicate answers.** Two items sharing an answer can put two correct options on screen.
   This caught 敦霹雳, 汉都亚 and 海军统帅 appearing in two chapters each.
3. **Every item can field enough distinct wrong options** without borrowing across chapters.
4. Plus: no missing pinyin, no missing fields.

---

## 7. The arcade layer

**Lanes.** Every answer falls in its own vertical lane. This is not cosmetic — an earlier build
used two columns on narrow screens, which meant the correct answer could sit *behind* a wrong
one, and the only ways through were shooting the wrong answer or waiting for it to clear. Lanes
guarantee a clear shot to every option. On narrow screens, reduce the option count to 3 rather
than stacking.

**Cannon upgrades and the lane guarantee.** Levels 2 and 3 fire 2 and 3 bolts. Every bolt in a
volley is clamped inside the lane the ship is under, so a wider cannon can never clip a
neighbouring wrong answer. Upgrades must never create risk.

**Upgrade pods** drop from destroyed answers — guaranteed on every third correct in a row,
otherwise ~50%. Fly into them to collect.

| Pod | Effect |
|---|---|
| Cannon | +1 level, max 3; faster and wider fire. A wrong shot knocks it back one |
| Shield | repairs one shield |
| Slow-mo | answers drift at half speed for 8 s |
| Scan | burns away one wrong answer — the 50/50 lifeline |
| Bonus | +300 points |

**Asteroid interludes** run between levels for 18 s. Rocks fall and split: large → two medium →
two small; smaller ones are worth more. **A collision never costs a shield** — it knocks the
cannon down a level and eats into the clear bonus. This is the rule that keeps the arcade layer
from ending a revision session. It also gives the cannon upgrades a genuine purpose, since
against a single falling answer firepower is irrelevant.

**Hangar, ships and accessories.** Credits are banked at 1 per 20 points, in a **separate purse
from score**, so purchases never lower the number on the leaderboard — otherwise nobody spends.

Four ships, each a real trade-off rather than a skin:
- Skiff (free) — balanced
- Bastion (450) — 5 shields, slow, −5% score. For a chapter still being learned
- Lancer (600) — 3 shields, fast, starts at cannon L2, +15% score. For a chapter already known
- Phantom (850) — answers drift 15% slower, −10% score. Buys thinking time with points

Six accessories, **two fitting slots** so owning everything doesn't mean flying with everything:
hull plating (+1 shield), auto-loader (start at cannon L2), magnet coil (pods steer toward the
ship), chrono core (slow-mo lasts 14 s), scanner array (one wrong answer burns away at the top
of each level), and one purely cosmetic livery — cheap, so there's an early win available.

---

## 8. Supporting features

**Read aloud.** Uses the device's own speech synthesis with a zh-CN voice, at adjustable rate
(0.55 / 0.72 / 0.9). Speaker button in the question band, or press L; works mid-game without
pausing. Also on every study card and every row of the study list, and on the review log.
Degrades gracefully: if no suitable voice is installed the button and toggle grey out and the
page explains why rather than reading in the wrong language.

**Music.** Generative ambient built live in Web Audio — no audio files, nothing to download.
Four chords, bass + detuned sawtooth pad + arpeggio, scheduled against the audio clock with a
lookahead window (JS timers are too imprecise for musical timing). Tempo rises with the level.
Ducks to 35% while speech is playing. Volume slider that previews while dragging.

**Roll of honour.** Top 12 runs in localStorage, with name, score, level reached,
correct/total, difficulty, which section, and date. Recording *the section* turned out to
matter — 5,800 on "All sections" and 5,800 on one small section mean different things.

**Review loop.** Everything missed is listed on the debrief with its correct meaning and what
the player shot instead. **Drill the misses** replays only those; **Study the misses** opens
them as study cards. A short run that surfaces four weak items is more useful than a long clean
one, and the UI should say so.

---

## 9. Technical constraints

- One self-contained HTML file. No separate CSS, JS or asset files.
- No external requests except Google Fonts. All graphics drawn in canvas or CSS; all sound
  synthesised in Web Audio; no images.
- Canvas for starfield, ship, bullets, pods, particles and asteroids. **DOM elements for the
  answer targets** — crisper text, free wrapping, easy measurement. Collision runs against the
  model coordinates, not the DOM.
- All state in `localStorage`, every read and write wrapped in try/catch, page renders correctly
  when storage is empty or throws.
- Responsive: nothing overflows sideways at 360 px. Lanes drop to 3, HUD reflows, fonts scale.
- Single committed dark visual theme (an arcade screen), painted explicitly so it holds on any
  host background.

**Known gaps on mobile** (tested, not yet fixed):
1. On touch, tapping to reposition also fires — so moving can cost a shield. Needs a separate
   thumb fire button with drag-to-move.
2. On a 360×640 phone the HUD (101 px) and band (155 px) leave only 391 px of playfield and the
   page scrolls vertically during play. Needs a compact one-row HUD and a viewport-locked game
   screen.

---

## 10. Lessons that cost testing to find

- A wider cannon that can clip a neighbouring answer punishes the player for upgrading.
- Three shields shared between knowledge mistakes and arcade mistakes means an unlucky asteroid
  field ends a revision session where every answer was right.
- Buying things with score stops anyone from buying anything.
- Two items sharing an English answer eventually puts two correct options on screen.
- An answer that is not a translation of the prompt teaches a falsehood in study mode.
- Finishing a second run in one session crashed the debrief when the first run had no misses —
  a stale DOM reference. Always test the *second* time through a flow.
- The magnet accessory did nothing for a whole release because it was gated to a vertical range
  pods rarely entered. Test each perk's actual effect, not just that it is purchasable.

---

## 11. For the Science version

Keep: the core loop, lanes, the ten-to-clear-a-level mastery rule, the shield economy, the
separate credits purse, study mode, the review loop, the three item kinds, and all of §6.

Change freely: the visual identity, the ship and hazard theme, the names of everything. The
arcade dressing is interchangeable; the learning rules are not.

Worth considering for Science specifically: the prompt does not have to be text. A diagram, a
circuit, a labelled apparatus or a short animation could be the prompt with text answers
falling — the lane mechanic does not care what sits in the band.
