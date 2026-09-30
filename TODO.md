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
