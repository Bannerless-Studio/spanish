# TODO

Residuals after QA round 1 and its fix round. Rules and counts are in
`tools/REPORT.md`.

## Known deviations from the ideal spec
- The es spaCy model is GPL-3.0. It runs only at build time and nothing
  from it ships except the derived pack data.
- Almost no permissive audio exists; sentences rely on TTS (es-ES). See
  "Audio" below.
- Same-spelling pairs (solo adv/adj, sí intj/pron, bajo prep/adj) make the
  engine validator print "share surface form" warnings; that is expected.
- Fixed phrases rank after the frequency words (rank is informational).

## Sentence links
- Determiner uses of mucho, poco and tanto are never linked ("muchos
  pueblos", "mucha fruta"). The pack has only their adverb entries, and a
  determiner token finds no entry to link. Fix: a determiner second entry, or
  a fallback from DET to the adverb entry.
- Idioms are read word by word: darse cuenta, por su cuenta, pena de muerte,
  comerse la cabeza. A wider phrase table would fix them. Phrases with an
  inflected verb (darse cuenta) need lemma-level matching.
- A -se display verb can be linked from a non-reflexive use (a
  non-reflexive convertir links convertirse).
- fue/fueron resolve to ir only before a/al/hacia. Other ir uses of the
  shared preterite still link ser.
- A sentence-initial word that starts a multi-word name links as a common
  noun (General Motors, Santo Tomé).
- Verb homographs are settled by the tagger's person/mood, then by corpus
  use (crees: creer, pare: parar, vete: ir). A pair within 8x of each other
  (sé saber/ser, fue ser/ir) keeps the tagger's lemma, with context rules for
  "sé" + adjective, "ve a", "fue a", "te sientas mal".
- sentir now shows as sentirse (to feel): the -se gate counts "te sientas
  mal" as reflexive. Revisit if QA prefers the base verb at A1.
- Adjectives used as nouns or adverbs link the adjective sense: llamadas
  (calls) tagged ADJ links llamado "called", "saldría seguido" (often)
  links seguido "consecutive", "el oficial" (officer) links oficial
  "official", "harto tiempo" (a lot of) links harto "fed up".
- A noun homograph of a preposition after an article links the
  preposition ("el sobre", envelope). The after-article rule covers only
  ADV/VERB tags.
- Tatoeba typos link as written ("A mi me gusta" links mi "my", "El ató"
  links the article).
- A predicate adjective whose only Wiktionary senses are marked (lindo:
  dated/uncommon) is not linked. The rarity guard stops it linking the rare
  verb lindar instead.
- Feminine person nouns are not linked when only the masculine is in the
  pack (muchacha, prima).

## Words and glosses
- Vulgar/sexual senses never lead a gloss (the check fails on any A1/A2 hit).
  Words whose only senses are vulgar would keep them; none are in the pack.
- fiesta and señora moved A1 -> A2 by rank (no forced entry). esposo is
  forced to A1 beside esposa.
- Diminutives (señorita, abuelito, ahorita) are words of their own. They
  link only when the pack has them.
- Feminine-only adjective senses show in masculine citation form
  (embarazado).
- Some sense choices are off below rank 300 (pista "clue" vs "track";
  el medio "middle" in "el fin justifica los medios").
- favor and acuerdo are not taught as nouns; they are dropped as bound to
  por favor / de acuerdo.
- ése/éste accented demonstratives fold into ese/este.
- -se verbs reverted to the base show the -se gloss (enterar "to find
  out", acordar "to remember" where acordar alone is "to agree").
- Two words have no sentence (el período, ultimar).
- reino and similar rescued common nouns rank below the 2000 cutoff.

## Audio
- Only 3 sentences have permissively licensed audio. Most Spanish Tatoeba
  audio is CC BY-NC-ND. Revisit if licences change or another CC-BY source
  appears.

## Builder
- `revert_dedupe_gloss` is on for Spanish only. Enabling it for Italian
  would change 4 glosses (muovere, ritirare, concludere, concentrare).
- The link and filter flags added for Spanish (phrase_token_spans,
  homograph_by_translation, initial_noun_verb_homograph, sensitive_re,
  translation_mismatch) are off for Italian. Each could be tried there.

## Reading passages
- A native-speaker pass over the 60 texts has not been done yet; only an
  automated QA pass plus one round of manual/external QA fixes.
- Known builder quirk: the adjective solo (w0110) is added to the
  sentence's `words` without a span in p0017 s4, p0045 s1 and p0051 s0.
  This comes from the classify lemma fallback and was not changed.
- The passage linker rules for Spanish (phrase-part links, surface-reading
  fallback, truecase after «¡¿ and !?, passage-only fue/fuera resolution)
  live in vocab-engine's `packbuilder/langs/es.py` hooks, not in this repo.
- Full manual QA notes and per-passage coverage/link numbers are in
  `tools/REPORT_passages.md`.

## Republish 09e90bc (2026-09-29)
- Republish 09e90bc: sentence spans (20346/20346 linked words placed); inflected forms now cloze targets

## Republish ef44c6e (2026-09-30)
- Republish ef44c6e: disparar, la bomba A2->B1 (band edge: juzgar, presente B1->A2); deleted override key tuyo|det (tuyo ships as pron, gloss unchanged); set-counter and no-voice planner fixes

## Republish 439df3d (2026-10-08), port wave 1
- Republish 439df3d: typed modes, day-aware scheduling, reading rotation, goals, pairs, frequency tiers, Progress v2, redesigned tabs, session estimates. Pack diff vs 47167a2: every word gains `ft` (A1 ambient 100 / core 440 / peripheral 60, A2 0/525/175, B1 0/420/280); pack.json gains the generic flag set + `eta` (gain 0.00503 / 0.00324 / 0.00269, known 7.33; 600 sessions, 85%, seeds 5/6/7, all gates PASS 3/3); nothing else changes. `tools/eta.json` holds the constants; `check.sh` runs `enrich --check`.
- Migration proof: rollback hash 47167a2a2a9f1cfc6f6b363db67f214016ff3189 (engine ef44c6e). Live md5 before: index.html 35ad2b3127ea77fe74b3a8577862c3ae, sw.js 26e0fb3b71dd93f0a7975822e34fb141. New build: index.html 796b78c2971ae8a904d2159d6ce605e5, sw.js bc442cc3ed3586e40fc7ba2297b02035. Storage: new fields day/sn/t/u/f/p/pm/pv/pause/read.done s,ls/today.tw on first use; boot writes nothing; previous build ef44c6e/aa00571 carries them (migration [port] 9/9).
- Live proof 439df3d (2026-10-08, KEEP; .cache/live/port-439df3d/proof): live md5s == local (marker 1456112280-2074003); 12-session seed played on 47167a2 byte-equal after boot, reload and Progress, one key vocab_es, no backup keys; one Today session on live writes sn/day/pm and t/u/f/p on words; that record boots on 47167a2 byte-equal (boot + reload), no backup keys, a session runs there; back on live byte-equal; 0 console errors, 0 failed requests.
- ETA out-of-sample gate 439df3d (2026-10-08, `eta_checks.js --pack pack --gate --sessions 600 --seeds 8,9,10`): goal 1 / 2 / 3 and gate estimate all PASS 3/3 within +-30% (worst: goal 3 -3%, gate 13%). Calibration seeds 5/6/7 were in-sample and also PASS.
