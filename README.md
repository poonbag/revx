# Project 02 · Did people feel differently 300 years ago?

Aubrey Wang · PGDV 5200 Major Studio 1 · Milestone 02: Ideation & Sketches
Data: Smithsonian *Revolution Crossroads* datasets on Hugging Face

---

## What is the story?

We cannot recover what anyone felt in 1771. We can recover what the world of 1770–1810 let people *say* about loss, fear, zeal and sympathy, and we can read it the way a detective reads a town: not "what happened here?" but "what kind of world is this, and what are its rules about feeling?"

The story is told through two public forms that follow different rules. The newspapers speak to the town: an obituary for a shoemaker praises his "Industry, Integrity, and whole Deportment" and closes with a couplet from Pope; three pages later a thirteen-year-old girl who has run away is worth two dollars and no adjective. The Revolutionary War pension files speak to the state, fifty years after the war: an old soldier or his widow swears to loss by inventory, because that is the only grammar the form allows.

Every passage is read through twelve lenses (who is named, how the words are sourced, what mood and tense the sentence takes, who it is addressed to, which emotional regime it belongs to). The lenses are not a sentiment score. They are the dials on a mixing desk, and each passage is a chord. The interactive piece lets a viewer play a 1771 obituary and a 2026 obituary on the same desk and hear, with their eyes, which dials moved. The difference between then and now is not that people felt more or less. It is that the instruments available to them for saying it were tuned differently.

![Concept A: the feeling desk](sketches/a1.jpg)

---

## What data am I interested in exploring?

All from the `RevolutionCrossroads` organisation on Hugging Face:

| Dataset | What I use it for |
|---|---|
| `RevX_OCR_results` (`page_level`, ~2.3M pages; collections `chronicling_america` and `rev_war_pension`) | The text. I split page-level OCR into sentences (8+ words) and group them back to the page for context. `block_level` stays available for layout (which column, which block type). |
| `RevX_NER_Entity_Occurrences` (24M rows; persons, places, organisations, dates, with confidence and character offsets) | The word-level lenses. Who is named, beside whom, how often, and how surely. I build a co-occurrence and typed-relation graph from it (widow of, served under, late of, reprinted from). |
| `loc_chronicling_america_1770-1810` (77,793 newspaper pages, 1770–1810) | Newspaper title, place of publication, issue date, and the link to the page image. Also the denominator: pages per year, which rises from ~450 in 1770 to 4,000+ after 1805. |
| `nara_revolutionary_war_pension_files` | Metadata for dating and locating the pension declarations. |

Two registers run through everything: **town** (newspapers, addressed to the public) and **state** (pension files, addressed to a clerk). A small hand-collected **2026 control set** (obituaries, a eulogy, an affidavit, a missing-person alert) makes "differently" mean something: like is compared with like, so the difference is era, not genre.

---

## What are the quantifiable aspects of this data?

Always rates, never raw counts, because the archive has ten times more pages in 1806 than in 1770.

- **Regime rates.** Occurrences per 1,000 pages per year of three period vocabularies of feeling: *sensibility* (tender, sympathy, tears, bosom), *paranoia* (conspiracy, design, slavery, tyranny), *enthusiasm* (zeal, fanatic, inspired, providence). Split by register.
- **The zeal test.** "My/our zeal" vs "his/their zeal": is zeal something one claims or something one accuses others of? By decade and register.
- **Feeling by circumstance.** Share of passages containing any feeling word, by circumstance (a death, an epidemic, a battle, an election, a national mourning) and by notice type (obituary, runaway, estate, muster).
- **Sentence texture.** Words per sentence, clause count, subordinator density, position of the main verb, punctuation per 100 words: by genre × era × register.
- **Cast and reach.** Names per sentence; entity confidence; for each name, how many documents, how many registers, and how many years it reaches. A "passageway" is a name that appears in both the newspapers and the pension files.
- **Twelve lens scores per passage**, each a small number or category, which is what the desk reads:

| Level | Lens | Comes from |
|---|---|---|
| word | 1 Cast · 2 Bonds · 3 Reach | the entity/relations graph |
| word | 4 Source of diction · 5 Feeling-word class · 6 Deixis | the page text |
| sentence | 7 Mood & voice · 8 Temporality · 9 Architecture | sentences split from page OCR |
| block | 10 Address · 11 Form & surplus · 12 Regime | the passage as a whole |

---

## What are the qualitative aspects? Are there personal accounts?

The project only works if every rule it names is anchored to one lit passage and its page scan. The seed passages so far:

- **Virginia Gazette, 17 October 1771, p. 2.** The obituary of John Wilson, shoemaker, and James Esther, block-maker, "whose Industry, Integrity, and whole Deportment, were truly exemplary," closing with a couplet from Pope. A tradesman's death given the full periodic sentence.
- **Same page.** Samuel Henley's letter mocking an opponent's argument for an American bishop as the frontispiece of Hobbes's *Leviathan*: political feeling conducted through theological insult, four years before the war.
- **Delaware Gazette, 8 July 1809, p. 3.** "Two Dollars Reward" for Kitty Hymen, "about thirteen years of age, who absconded." A person gone, with no feeling word at all. Absence is evidence.
- **A widow's pension declaration (Act of 1832).** Loss told as a list: dates, regiments, the name of the officer, what was lost in the fire. The state's grammar of grief.
- **A Washington eulogy, December 1799.** National mourning as it travelled, reprinted from town to town over six weeks.
- **A 2026 obituary and a 2026 missing-person alert.** The modern counterparts, read through the same twelve lenses.

Each passage gets a line-by-line reading in the micro view (the "struck match"): the sentence, its NER rows, its lens values, and the rule of the world it reveals, written in my own words: *"In this world a widow may grieve to the state only by inventory."*

---

## Are there traditional charts that help tell the story?

Yes, and they do the honest work that the expressive piece then plays with:

- **Pages per year** (bar): the denominator, shown rather than hidden.
- **Regime rates by year** (small-multiple lines, one per register): sensibility, paranoia, enthusiasm.
- **Zeal: self vs other** (paired bars by decade and register).
- **Feeling-word share by notice type** (bars): obituary vs runaway vs estate.
- **Sentence length and verb position** (violins and bars, 1770–1810 vs 2026, obituaries only).
- **Circumstance × register matrix** (heat/dot matrix): where in the archive feeling is permitted, and where it is silent.

---

## Can the story benefit from a custom / expressive visualization?

That is the piece. **The Feeling Desk** is a mixing console built in d3 and p5:

- **Twelve strips, three bands.** Block-level lenses are the bass (shape, colour, pulse of the whole passage); sentence-level lenses are the mids (stroke, motion, wave); word-level lenses are the treble (particles, bonds, sparkle). It is the opposite of a Fourier transform: separate signals summed into one picture.
- **Each strip is a small simulation with the same controls as a physics sim.** Cast is a longitudinal wave whose compressions are named entities (amplitude = confidence, frequency = number of names). Bonds is a coupled-spring chain. Reach is a ripple across towns. Mood & voice is a reflecting wave. Temporality is a damped oscillation. The matches to the oPhysics wave simulations were made formally, by counting controls, not by metaphor.
- **A master channel** sums the strips into one animated figure per passage, so two passages can be set side by side: the 1771 obituary and the 2026 obituary, the pension declaration and the modern affidavit.
- **Solo / mute** on every strip, so a viewer can isolate one lens and see whether it is the one that changed between eras.
- **Predict, then check.** Before a passage is revealed the viewer sets the dials where they expect them; the desk then plays the real values.
- **The struck match.** Clicking the master figure opens the micro view: the passage beside its page scan, its twelve lens values, and the rule it reveals.

![The mixing desk](sketches/c1.jpg)

A working p5 prototype of the strips exists (nine lenses, five preset passages); the sketches above are the layout it will grow into.

---

## Two further concepts

The desk is the main concept. Two narrower pieces came out of the same material and could stand on their own.

### B · Two dollars and no adjective

![Concept B](sketches/b1.jpg)

- **Story.** Who was allowed to be mourned in print. The obituary and the runaway notice are the same event, a person gone, in opposite forms. Absence is drawn as absence.
- **Data.** Death, runaway and estate notices from the newspaper pages; the NER graph for names and reach; widows' depositions as a third form of loss; a 2026 set of obituaries and missing-person alerts.
- **Quantifiable.** Notices per year by type; share with any feeling word; words per notice; price offered vs virtues listed; reach per name (documents, registers, span of years).
- **Qualitative.** Kitty Hymen, 1809; Wilson and Esther, 1771; one widow's deposition; one 2026 alert and one 2026 obituary.
- **Traditional charts.** Bars of feeling-word share by notice type; lines of death vs runaway notices per year.
- **Custom.** A page re-set in two columns with feeling words lit and an empty box drawn where feeling is absent; names as nodes with rays for reach (Washington's death reaching forty towns in six weeks; a veteran with two rays fifty years apart; Kitty Hymen with none).

### C · The rhythm of feeling

- **Story.** We cannot compare our feelings to theirs, but we can compare the sentences feeling was written in. An 18th-century obituary suspends its verdict through stacked clauses to a final adjective; a 2026 obituary lands in five words. Form partly makes the feeling.
- **Data.** Sentences from page-level OCR, both registers; the 2026 control set, matched by genre.
- **Quantifiable.** Words per sentence; clause count and subordination depth; main-verb position; punctuation density; by genre × era × register.
- **Qualitative.** Line-by-line readings of one 1771 obituary sentence and one 2026 obituary sentence; Henley's longest sentence; a widow's declaration.
- **Traditional charts.** Violins of sentence length by era; bars of verb position.
- **Custom.** Animated rhythm strips in p5: sentence length as wavelength, subordination as layering, verb position as where the pulse lands, punctuation as grain; a 1771 strip and a 2026 strip side by side. Listen with your eyes.

---

## Method notes

- The sentence is the unit, derived from page-level OCR. Pages are too mixed (an elegy beside a shoe advertisement) to carry one feeling.
- No off-the-shelf sentiment model: those score 1790 with a 2020s lexicon. Passages are clustered by resemblance and named by me.
- OCR errors ("Congrefs", the long s) are kept visible as part of the record, not cleaned away.
- Every finding gets a modern counterpart, or "differently" is an empty word.
- Theory I am leaning on: emotion as constructed and historically situated (Barrett; Dixon; Reddy's *emotives*; Rosenwein's *emotional communities*); the period's three regimes of feeling (sensibility: Knott, Eustace; paranoia: Wood, Bailyn; enthusiasm: Hume, Locke); and modern psychology as testable claims rather than labels (Bonanno and Stroebe & Schut on grief; Doka on disenfranchised grief; Marcus on affective intelligence; Rimé on social sharing).

## Stack

- **Offline (Colab, Python):** `datasets` streaming, `pandas`, regex and `CountVectorizer` for lens scoring, `networkx` for the graph, `spaCy` on the modern control set. Output: small static JSON files (`passages.json`, `graph.json`, `rates.json`, `sentences.json`).
- **Browser (GitHub Pages):** vanilla HTML/CSS/JS, **d3 v7** for data, layout, traditional charts and the force graph, **p5.js** (instance mode) for the strips. matter.js and the Web Audio API only if a strip needs to behave or sound.
- No server, no database. The relations graph is a JSON file filtered in the browser.

## Repo

```
project-02/
  README.md
  sketches/      a1.jpg  b1.jpg  c1.jpg  (hand-drawn)
  data/          passages.json  graph.json  rates.json  sentences.json
  notebooks/     01_build_data.ipynb
  js/            main.js  desk.js  strips.js  lenses.js
  css/           style.css
  index.html
```

Wave simulations referenced for the strip designs: Tom Walsh, [oPhysics](https://ophysics.com) (GeoGebra applets, used for non-profit educational purposes).
