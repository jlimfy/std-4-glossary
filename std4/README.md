# 四年级 Standard 4 — Glossaries & Games

Bilingual study material for the Malaysian **Standard 4 SJKC** curriculum: interactive
glossaries for Mathematics and Science, plus four browser revision games.

Everything is plain HTML, CSS and JavaScript. There is no build step, no framework and no
server — each page is a single self-contained file that runs straight from the filesystem or
from GitHub Pages.

## What's inside

### Glossaries — `glossary/`

Every entry has the Chinese characters, pinyin, a read-aloud button (browser speech synthesis),
a simple English definition, the Malay term, the textbook page and a hand-drawn SVG diagram.
Each page also has a search box that matches characters, pinyin (with or without tone marks)
and English, and a "hide English" toggle for self-testing.

**Mathematics — `glossary/Matematik/`**

| Page | | Words |
|---|---|---|
| `Common_Words_Glossary.html` | 通用词 Common maths words | 57 |
| `Unit2_Fractions_Glossary.html` | 分数、小数与百分比 Fractions, decimals, percentages | 58 |
| `Unit3_Money_Glossary.html` | 钱币 Money | 55 |
| `Unit4_Time_Glossary.html` | 时间与时刻 Time | 64 |
| `Unit5_Measurement_Glossary.html` | 度量衡 Measurement | 69 |
| `Unit6_Space_Glossary.html` | 空间 Space | 75 |
| `Unit7_Ratio_Glossary.html` | 坐标、比与比例 Coordinates, ratio, proportion | 46 |
| `Unit8_Data_Glossary.html` | 数据处理 Data handling | 41 |

**Science — `glossary/Science/`**

| Page | | Words |
|---|---|---|
| `Ch1_Science_Skills_Glossary.html` | 科学技能 Scientific skills | 56 |
| `Ch5_Light_Glossary.html` | 光的特性 Properties of light | 40 |
| `Ch6_Sound_Glossary.html` | 声音 Sound | 37 |
| `Ch7_Energy_Glossary.html` | 能 Energy | 53 |
| `Ch8_Materials_Glossary.html` | 材料 Materials | 47 |
| `Ch9_Earth_Glossary.html` | 地球 Earth | 36 |
| `Ch10_Machines_Glossary.html` | 机械 Machines | 42 |

Chapters 2–4 (Humans, Animals, Plants) are not written yet and show as "Coming soon".

### Games — `games/`

| Game | Subject | What it is |
|---|---|---|
| `hanzi-interceptor/` | History | A Chinese prompt appears and its possible English meanings drift down the playfield; fly the ship and shoot the match. 544 items across 35 sections, Chapters 5–11. See `SPEC.md`. |
| `voxel-island/` | Science | Build a voxel world by answering KSSR Year 4 science questions. |
| `abyss-decoder/` | Science | A term appears in the beacon and its meanings sink past your submersible; pulse the right one. Ten clean reads clears the depth. |
| `lab-blaster/` | Science | A science-lab arcade run. |

`games/hanzi-interceptor/SPEC.md` is the design brief for the Hanzi Interceptor gameplay,
written so the same loop can be rebuilt with different subject content.

## Running it

Open `index.html` in a browser — that is all. Nothing here needs a local server.

If you prefer one anyway:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Publishing with GitHub Pages

The repository is already laid out for Pages: `index.html` at the root, no build step, and an
empty `.nojekyll` file so that Jekyll does not reprocess the HTML.

1. Push the repository to GitHub.
2. **Settings → Pages → Build and deployment**.
3. Source: **Deploy from a branch**. Branch: **main**, folder: **/ (root)**. Save.
4. After a minute the site is live at `https://<username>.github.io/<repository>/`.

## Browser notes

- **Read-aloud** uses the browser's built-in `speechSynthesis`. Chinese playback needs a
  Mandarin voice installed; Chrome, Edge and Safari normally have one. If nothing is spoken,
  the voice is missing rather than the page being broken.
- **Google Fonts** and, for Lab Blaster, **three.js** are loaded from a CDN, so first load
  needs a connection. Everything else works offline.
- The pages follow the system light/dark setting.

## About the content

The word lists follow the Standard 4 SJKC textbooks (Kementerian Pendidikan Malaysia), the EPH
mathematics workbook and the KSSR 科学笔记 science notes. Definitions, diagrams and example
sentences were written fresh for this project; no textbook pages, images or exercise answers
are reproduced here.

Pinyin and the Malay equivalents were added by hand and have not been checked by a teacher, so
treat them as a study aid and not as an authority. Corrections are very welcome.

Made for one pupil, shared in case it is useful to someone else.
