# Musical Cloze Test

## Quantifying Melodic Predictability in Bach Chorales via Information-Theoretic Methods

**Júlia Serra Trujillo & Lydia Krifka-Dobes**
Computational & Cognitive Musicology

---

### Overview

This repository contains the complete code and figures for our study of melodic
predictability in J.S. Bach's chorales. We frame note prediction as a _musical
cloze test_: given the preceding context, how well can a statistical model guess
the next note?

We evaluate unigram, bigram, and trigram Markov models using leave-one-out
cross-validation on the full music21 Bach chorale corpus (412 pieces).

### Key Results

| Model                | Accuracy      | Cross-Entropy (bits) | Perplexity |
| -------------------- | ------------- | -------------------- | ---------- |
| Random baseline      | 0.022         | —                    | 45         |
| Unigram              | 0.153 ± 0.088 | 3.937                | 20.5       |
| Bigram               | 0.325 ± 0.097 | 2.727                | 6.9        |
| Trigram              | 0.397 ± 0.100 | 2.726                | 7.1        |
| Interpolated trigram | 0.396 ± 0.103 | 2.716                | 6.9        |

**Top-k accuracy (trigram):** Top-1 = 39.3%, Top-3 = 75.0%, Top-5 = 89.0%

### Research Questions

1. **RQ1 — Model order:** Trigram significantly outperforms bigram and unigram
   (Wilcoxon p < 10⁻⁴⁰). Effect size (rank-biserial): bigram→trigram r = 0.877.
2. **RQ2 — Entropy vs. accuracy:** No significant correlation (r = -0.030,
   p = 5.45e-01). Predictability is a property of sequences, not vocabularies.
3. **RQ3 — Voice parts:** Outer voices (soprano 39.7%,
   bass 40.2%) are more predictable than inner voices
   (alto 33.7%, tenor 35.8%).
   Kruskal-Wallis H = 157.6, ε² = 0.0942.
4. **RQ4 — Phrase position:** Beginnings are slightly more predictable than middles.
   Fermata-based segmentation reveals clearer patterns than coarse thirds.
5. **RQ5 — Benchmark:** Our trigram cross-entropy (2.726 bits/note)
   falls within Conklin & Witten's (1995) 2.5–3.5 range.

### Extensions (Supplementary)

- **Interpolated smoothing** reduces the accuracy–cross-entropy dissociation
  compared to Laplace smoothing.
- **Melody length** vs. accuracy correlation.
- **Top-k accuracy** shows the correct note is in the model's top-5 guesses
  89% of the time.
- **Fermata-based phrase segmentation** gives musically more meaningful results
  than the coarse thirds approach.

### Voice-Part Details

| Voice   | N   | Accuracy      | Cross-Entropy | Perplexity |
| ------- | --- | ------------- | ------------- | ---------- |
| Soprano | 411 | 0.397 ± 0.100 | 2.726         | 7.1        |
| Alto    | 411 | 0.337 ± 0.081 | 2.849         | 7.6        |
| Tenor   | 411 | 0.358 ± 0.086 | 3.027         | 8.6        |
| Bass    | 412 | 0.402 ± 0.096 | 3.243         | 10.1       |

### Figures

All figures are saved at 300 dpi (PNG) and as vector PDFs.

| Figure | Description                                     |
| ------ | ----------------------------------------------- |
| fig1   | Accuracy by model order (bar + distribution)    |
| fig2   | Cross-entropy and perplexity by model order     |
| fig3   | Shannon entropy vs. trigram accuracy (scatter)  |
| fig4   | Voice-part comparison (boxplot + bar)           |
| fig5   | Per-note surprisal trace (single chorale)       |
| fig6   | Surprisal by phrase position (thirds + fermata) |
| fig7   | Pitch-class transition heatmap                  |
| fig8   | Interval distribution                           |
| fig9   | Melody length vs. accuracy                      |
| fig10  | Top-k accuracy                                  |

### How to Run

```bash
pip install music21 matplotlib pandas numpy scipy seaborn
jupyter notebook musical_cloze_test.ipynb
```

The notebook runs end-to-end in ~20–40 minutes depending on hardware (the
leave-one-out loop over 412 chorales is the bottleneck).

### References

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

### License
