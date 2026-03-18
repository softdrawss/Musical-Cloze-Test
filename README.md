# Musical Cloze Test

**Quantifying Melodic Predictability in Bach Chorales via Information-Theoretic Methods**

Júlia Serra Trujillo & Lydia Krifka-Dobes — Symbolic Music Analysis and Computational Musicology

---

## What this is

We took 412 Bach chorales from the music21 corpus and asked a simple question: if you hide a note in a melody, how well can a statistical model guess what it was? This is the musical version of the [cloze test](https://en.wikipedia.org/wiki/Cloze_test) from psycholinguistics.

We trained unigram, bigram, and trigram Markov models, evaluated them with leave-one-out cross-validation (no cheating — the model never sees the test chorale during training), and measured accuracy, cross-entropy, and perplexity.

The short version: a trigram model gets the right note about 40% of the time, and if you give it three guesses, it's right 75% of the time. Not bad for counting note pairs.

## Results at a glance

| Model | Accuracy | Cross-Entropy (bits) | Perplexity |
|---|---|---|---|
| Random baseline | 0.022 | — | 45 |
| Unigram | 0.153 ± 0.088 | 3.937 | 20.5 |
| Bigram | 0.325 ± 0.097 | 2.727 | 6.9 |
| Trigram | 0.397 ± 0.100 | 2.726 | 7.1 |
| Interpolated trigram | 0.396 ± 0.103 | 2.716 | 6.9 |

Top-k accuracy for the trigram: Top-1 = 39.3%, Top-3 = 75.0%, Top-5 = 89.0%.

The trigram cross-entropy of 2.73 bits/note lands right in the 2.5–3.5 range that Conklin & Witten estimated back in 1995 on a smaller corpus. Good to know the numbers hold up thirty years later.

## What we found

**More context helps, but not as much as you'd think.** Going from no context (unigram) to one note of context (bigram) more than doubles accuracy. Adding a second note of context (trigram) helps, but the jump is smaller. Most of the predictable structure in Bach lives in adjacent note pairs.

**Pitch variety doesn't predict difficulty.** A chorale that uses lots of different pitches isn't necessarily harder to predict than one that sticks to a few. The correlation between per-chorale Shannon entropy and trigram accuracy is basically zero (r = −0.03). Predictability comes from sequential patterns, not from how big the vocabulary is.

**Outer voices are more predictable than inner voices.** Bass (40.2%) and soprano (39.7%) beat alto (33.7%) and tenor (35.8%). This makes sense — the soprano carries a hymn tune that follows strong conventions, and the bass has to outline the chord progression. The inner voices have more freedom, which means more ways to surprise the model. The bass has an interesting quirk: highest accuracy but also highest cross-entropy (3.243 bits), because its wider pitch range means that when the model is wrong, it's *really* wrong.

**Phrase beginnings are the most predictable.** Chorale openings tend to be formulaic, so the model does best there. We also ran a fermata-based segmentation (splitting at actual musical phrase boundaries rather than arbitrary thirds), which gave clearer and more musically meaningful results.

## Supplementary experiments

Beyond the five main research questions, we ran a few extras:

- **Interpolated smoothing** — does it fix the weird accuracy/cross-entropy dissociation between bigram and trigram? A bit, yes.
- **Melody length vs. accuracy** — longer chorales aren't systematically harder or easier.
- **Top-k accuracy** — the correct note is in the model's top 5 guesses 89% of the time.
- **Fermata-based phrase segmentation** — using actual musical boundaries instead of chopping the chorale into thirds.
- **Pitch-class transition matrix** — a heatmap of which notes follow which. Stepwise motion dominates, and the leading tone really wants to go to the tonic.
- **Interval distribution** — 84.3% of melodic intervals are stepwise (≤ 2 semitones).

## Voice-part breakdown

| Voice | N | Accuracy | Cross-Entropy | Perplexity |
|---|---|---|---|---|
| Soprano | 411 | 0.397 ± 0.100 | 2.726 | 7.1 |
| Alto | 411 | 0.337 ± 0.081 | 2.849 | 7.6 |
| Tenor | 411 | 0.358 ± 0.086 | 3.027 | 8.6 |
| Bass | 412 | 0.402 ± 0.096 | 3.243 | 10.1 |

## Figures

All saved at 300 dpi (PNG) and as vector PDFs.

| Figure | What it shows |
|---|---|
| fig1 | Accuracy by model order — bar chart + histogram |
| fig2 | Cross-entropy and perplexity by model order |
| fig3 | Shannon entropy vs. trigram accuracy — scatter |
| fig4 | Voice-part comparison — boxplot + bar |
| fig5 | Per-note surprisal trace for a single chorale |
| fig6 | Surprisal by phrase position — thirds + fermata |
| fig7 | Pitch-class transition heatmap |
| fig8 | Interval distribution |
| fig9 | Melody length vs. accuracy |
| fig10 | Top-k accuracy |

## How to run

```bash
pip install music21 matplotlib pandas numpy scipy seaborn
jupyter notebook musical_cloze_test.ipynb
```

Takes about 20–40 minutes end-to-end. The bottleneck is the leave-one-out loop (training 412 models for each experiment). Go make coffee.

## References

1. Conklin & Witten (1995). Multiple viewpoint systems for music prediction.
2. Cuthbert & Ariza (2010). music21: A toolkit for computer-aided musicology.
3. Devlin et al. (2019). BERT.
4. Hadjeres et al. (2017). DeepBach.
5. Huron (2006). Sweet Anticipation.
6. Meyer (1957). Meaning in music and information theory.
7. Pearce (2005). Statistical models of melodic structure. PhD thesis.
8. Pearce (2018). Statistical learning and probabilistic prediction in music cognition.
9. Shannon (1948). A mathematical theory of communication.
10. Taylor (1953). Cloze procedure.
11. Zeng et al. (2021). MusicBERT.
12. Temperley (2007). Music and Probability.
13. Rohrmeier & Graepel (2012). Comparing feature-based models of harmony.

## License

This project is free software: you can redistribute it and/or modify it under the terms of the **GNU General Public License v3.0** as published by the Free Software Foundation. See [LICENSE](LICENSE) for the full text, or visit https://www.gnu.org/licenses/gpl-3.0.html.
