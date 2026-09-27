# Spanish A1-B1 vocab pack

Static data pack for a language-agnostic vocab trainer (`key: "es"`). 2000
words spanning A1-B1, each with a short English gloss, plus 3288 example
sentences with English translations. The Read tab adds 60 short reading
passages with comprehension questions (see "Reading passages" below).

**Live:** https://bannerless-studio.github.io/spanish/

Open the link, pick a level (or take the placement test), and start a Today
session: short rounds of flashcard-style review mixed with new words, plus a
Read tab with short passages and comprehension questions, and typing practice
for spelling. Progress (what you've seen, what's due for review) is saved in
your browser only, and can be exported/imported as a file to move between
devices. The site works offline once loaded (it registers a service worker).

**Scope note:** this app is a vocabulary base for B1. The DELE B1 also needs
grammar, writing and speaking, which this app does not teach.

**Data quality:** three QA rounds, hand-checked on stratified samples (30
per level). Round 1 (seeds 11/12): 179/180 correct primary sense, 527/529
word links correct. Round 2 (seeds 707/808): 89/90 correct primary sense,
544/547 sentence links (99.5%), 87/90 sentences fully correct. Round 3 fixed
wrong-lemma classes (verb homographs such as crees/creer, pare/parar,
vete/ir; rare fallback lemmas; "no des" = dar). An audit of all 20,604
token links, including one against the far more used reading of each
surface, finds one wrong-lemma link (llamadas read as llamado). For 1984 of
1998 words, at least one example sentence shows the headword as taught. The
top 300 words have no wrong part of speech.

**Content policy:** sentences on sexual content, threats, violence, death,
weapons, suicide or vulgar English are kept out of A1/A2 (a word that is
itself on the list, such as morir or matar, takes B1-level examples).
Sentences about rape or sexual/child abuse are removed at every level. No
A1/A2 gloss carries a vulgar or sexual sense. matar, morir and muerto stay
at A1 as neutral core vocabulary; their violent uses reach learners only
through B1 sentences (same call as Russian убить). See `TODO.md` for the
exact rule history and counts.

## What's in this repo

This repo holds the Spanish data pack (`pack/`) and the data files its build
reads (`tools/`), plus [`vocab-engine`](https://github.com/Bannerless-Studio/vocab-engine)
as a git submodule at `engine/`, which holds the shared UI, drill logic and
pack builder used by every language in this trainer. See `tools/README.md`
for a file-by-file breakdown of `tools/`, and `CLAUDE.md` for the full
architecture and build commands.

## Rebuild and publish (maintainers)

```
git clone --recurse-submodules <this repo>
cd spanish && python3 -m venv .venv && source .venv/bin/activate
pip install -r tools/requirements.txt
python3 tools/build_pack.py && python3 engine/tools/jsonify_pack.py pack
./build.sh && ./check.sh
```

See `tools/README.md` for what each rebuild step reads/writes and `CLAUDE.md`
for the pinned commands, submodule-update flow and forbidden patterns.

## Sources and licences

| Data | Source | Licence | Used for |
|---|---|---|---|
| Spoken frequency | [hermitdave/FrequencyWords](https://github.com/hermitdave/FrequencyWords) `es_full.txt` (2018 OpenSubtitles) | CC-BY-SA 4.0 | word ranking |
| Written frequency | [`wordfreq`](https://github.com/rspeer/wordfreq) | CC-BY-SA 4.0 | word ranking |
| Glosses, POS, gender, inflections | [kaikki.org](https://kaikki.org) Spanish Wiktionary extract | CC-BY-SA 3.0 / GFDL | glosses, POS, gender, lemma validation |
| POS tagging (build time only) | [spaCy](https://spacy.io) (MIT) + `es_core_news_sm` 3.8.0 | model: **GNU GPL 3.0** | corpus POS, lemmas, sense choice, sentence links. The pack ships no model files or model output beyond derived word/sentence data. |
| Example sentences | [Tatoeba](https://tatoeba.org) `spa_sentences_detailed.tsv` | CC-BY 2.0 FR | sentence text (contributors in `pack/attribution.json`) |
| Translations | Tatoeba `eng_sentences.tsv` + `spa-eng_links.tsv` | CC-BY 2.0 FR | English translations |
| Audio | Tatoeba `sentences_with_audio` | CC BY / CC BY-SA / CC0 clips only | 3 sentences. Most Spanish Tatoeba audio is CC BY-NC-ND or unlicensed, so the trainer relies on browser TTS. |

Licence: code MIT, pack data CC BY-SA 4.0, see LICENSE.

There is no open CEFR word list for Spanish, so levels are a frequency
proxy, not an official CEFR classification. doozan/spanish_data was
considered as a cross-check only and is not shipped.

## Reading passages (Read tab)

60 short reading texts, 20 each at A1, A2 and B1, with comprehension
questions each. A level's 20 passages unlock once you've learned 70% of that
level's words. Tapping any word in a passage shows its gloss, including
inflected forms. Comprehension questions feed missed words back into the
review queue as weak words. The passages and questions are machine-written,
checked by an automated QA pass rather than a native speaker.

A passage's spaced re-read on Today (after 7 days) becomes a listening pass
when audio is available: the text stays hidden behind numbered play rows,
and about half the questions are audio-only. Note: almost no Spanish
sentence audio is permissively licensed (see table above), so most listening
and pronunciation relies on browser TTS (es-ES) rather than recorded audio.

Typing practice is accent-lenient and case-insensitive at A1/A2, strict at
B1. A fold-only match is rejected when it would spell another pack word
(`si` won't match `sí`, and vice versa, for 13 such pairs).
