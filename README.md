# Sliding window suffix trees

Luís M. S. Russo and Simon J. Puglisi.  Code accompanying the paper
"A pragmatic implementation of sliding window suffix trees".

* `swst.c`, `swst.h`, `swstmain.c` — the implementation presented in the
  paper (64-bit pointers, lazy position maintenance).  Code snippets in
  the paper are extracted from the `/* name : 1 */ ... /* name : 0 */`
  markers by `paper/extract.py`.
  Build: `gcc -O2 -o swst swst.c swstmain.c`;
  run: `(printf "%d " $(stat -c %s FILE); cat FILE) | ./swst W` prints the
  LZ77 phrases `(source, length) literal`.
* `brute/` — an independent, earlier reference implementation (64-bit,
  hash table children, eager chain repair of positions).
* `bench/` — the experimental framework: `swstx.c` compiles into one
  binary per position maintenance method and reference width
  (`make` builds `bin/{lz,ms}-{lazy,lazyw,lazy0,credit,plp,batch,eager}-{32,64}`).
  - `lz.c` LZ77 parsing driver, `ms.c` sliding window matching statistics.
  - `make stress debug` builds the consistency-checking test binaries.
  - `run_lz.py`, `run_ms_time.py`, `run_ms_quality.py`, `run_disp.py`
    run the experiments (results in `bench/results/*.jsonl`);
    `analyze_lz.py` and `analyze_ms.py` produce the tables, figures and
    macros used by the paper (`paper/results`, `paper/figs`).
  - `fasta2seq.py` converts FASTA files to raw ACGT sequences.

* `verify/` — exhaustive and random verification of the credit method's
  invariants and of the Fiala–Greene variants (see `verify/README.md`).

Data: Pizza&Chili corpus (https://pizzachili.dcc.uchile.cl) and NCBI
RefSeq accessions listed in the paper (Table "Genomes").  Statistical
analysis requires Python 3 with numpy, scipy and matplotlib.

## Use of AI tools

The implementation in `swst.c`, `swst.h` and `swstmain.c` was written by
the authors; two changes (the LZ77 source bound in `windowPos` and the
`-1` result of `getnchar` for out of range letters) were made at their
direction.  The benchmark framework (`bench/`), the analysis scripts and
the verification programs (`verify/`) were written with Claude Opus 5.5
(Anthropic) through the Claude Code agent in November 2026, under the
direction of Luís M. S. Russo, who reviewed the code and its results.
