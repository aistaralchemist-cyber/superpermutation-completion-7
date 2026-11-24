# A rotation-class completion criterion for seven-symbol superpermutations

**Jaehun Jung** · Independent Researcher, Republic of Korea · 24 November 2026

[Read the preprint (PDF)](seven-symbol-completion.pdf) · [LaTeX source](seven-symbol-completion.tex) · [Verification supplement (ZIP)](seven-symbol-verification.zip)

## Main result

A seven-symbol superpermutation contains all 5,040 permutations as consecutive seven-letter blocks. These permutations form 720 classes under cyclic rotation. A word *visits* a class when it contains a six-letter block of distinct symbols that determines the class. It *completes* the class when all seven rotations occur.

Let $L(7)$ be the shortest superpermutation length and let $M_k$ be the shortest length of a word that visits all 720 classes and completes at least $k$. The paper proves

$$
M_k+7(720-k)=L(7) \qquad (91\leq k\leq 720).
$$

For $91 \leq k < 720$, raising the completion requirement by one therefore raises the minimum length by exactly seven letters, even when the word is chosen anew.

In particular, $L(7)\leq 5904$ if and only if a word of exactly 1,501 letters visits every class and completes 91. The paper exhibits two words visiting all 720 classes. One completes 90 at length 1,501, and the other completes 91 at length 1,502. Whether 91 completions are possible at length 1,501 remains open.

The known interval $5901\leq L(7)\leq5905$ uses Jakub Halfar’s computer-assisted lower bound and Andrei Teodorescu’s length-5,905 word W-0021. The supplement derives its examples from W-0021 and checks a separate restricted-family result against 98 record classes in Halfar’s catalogue. [Halfar’s lower-bound proof](https://github.com/jhalfar/superpermutations/tree/8e3c02b5234b49a1422751f86f9e472706082374/proofs/n7_5901) · [Teodorescu’s W-0021](https://superpermutations.com/w/W-0021) · [Record catalogue](https://github.com/jhalfar/superpermutations/tree/8e3c02b5234b49a1422751f86f9e472706082374/ties)

## Reproduce the examples

From the repository root, extract the supplement and run the Python 3 checker. It uses only the standard library.

```sh
unzip -q seven-symbol-verification.zip -d seven-symbol-verification
cd seven-symbol-verification
python3 check-seven-controls.py
```

| Word | Length | Classes visited | Classes completed |
| --- | ---: | ---: | ---: |
| W-0021 | 5,905 | 720 | 720 |
| After deleting 653 specified laps | 1,334 | 720 | 67 |
| After deleting the first 629 laps | 1,502 | 720 | 91 |
| After also deleting the initial letter | 1,501 | 720 | 90 |

The checker also finds 716 visited classes and 608 distinct permutations in the 1,469-letter prefix of the last word. The README inside the supplement gives the pinned Lean dependencies, instructions for rebuilding the formal proofs, and the commands for checking the record catalogue.

The ZIP’s SHA-256 digest is `0f58e7441ccd42c12119ebb988d9874f30822863b93d31e5463b0e03c90c41fd`. Its 614 entries consist of 613 files and their checksum manifest. All 613 digests were checked. To repeat this check in the extracted directory, run `shasum -a 256 -c SHA256SUMS` on macOS or `sha256sum -c SHA256SUMS` on Linux.

## Authorship and source credits

Jaehun Jung set the research questions and directed the investigation. OpenAI Codex (GPT-6) assisted with the mathematical arguments, Lean development, computational experiments, and manuscript preparation. The supplement credits the external proofs, data, and formal libraries used in the paper. Teodorescu’s W-0021 is reproduced under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
