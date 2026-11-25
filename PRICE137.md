# Run the seven-symbol price-137 search

**Jaehun Jung** · Independent Researcher, Republic of Korea · 25 November 2026

Can 2 to 9 directed trails, together with wheel packets, cover all 720 rotation classes exactly once at price 137? The price is the number of selected rows plus the number of nonempty trails plus the number of packets. Each row covers 1 to 6 classes. Each packet covers 1 to 5 classes on one wheel. Adjacent rows of a trail must agree at their four-symbol ports.

The search remains open. The two archived whole-branch trials (2 and 9 trails) both reached their 25-second solver limit. This supplement makes the search, positive control and partial exclusions available to run independently. Halfar’s existing computer-assisted proof of the length bound 5,901 is available separately. [Halfar’s proof](https://github.com/jhalfar/superpermutations/tree/8e3c02b5234b49a1422751f86f9e472706082374/proofs/n7_5901)

## Quick check

Download the ZIP and extract it, or run the following from a clone of the paper repository. Run the remaining command blocks from the `price137` directory. The numerical code was tested on macOS. The profile programs use a GCC- or Clang-compatible C++17 compiler. The timing harness also uses POSIX `ps`. [Download the supplement](https://github.com/aistaralchemist-cyber/superpermutation-completion-7/raw/refs/heads/main/price137-search.zip)

```sh
unzip -q price137-search.zip
cd price137
scripts=Haruhi7/lower/target5901/exact48
python3 -B "$scripts/price137_catalog.py"
```

This check uses the Python standard library. It reads Andrei Teodorescu’s W-0021 and verifies the extracted assignment. Expect `covered=720`, `M=6` trails, `R=70` rows, `k=66` packets and `price=142` in the JSON output. Price 142 supplies a known feasible control for the model.

## Run one of the eight branches

Use Python 3.11 or later and install the numerical dependencies in a virtual environment. The first command that invokes the solver fixes the known control. The second example gives the 2-trail branch an hour of solver time. Substitute any integer from 2 through 9. Setup time is additional. Separate processes can run different trail counts on independent machines.

```sh
python3 -m venv .venv
. .venv/bin/activate
python -m pip install -r requirements.txt
python -B "$scripts/price137_milp.py" --control
python -B "$scripts/price137_milp.py" --trails 2 --seconds 3600 > M2.jsonl
```

- `checked_board` reports a geometric assignment rechecked with integer counts, including all 720 classes, packet sizes, price and the minimum number of directed trails. The output includes row IDs and packet data. A price-137 board would still need to be lifted to an actual word.
- `unknown` leaves the branch open. Repeating a command starts a fresh search. The program writes JSON lines and has no checkpoint or proof-log facility.
- `solver_infeasible_unverified` records the numerical solver’s conclusion. A certified exclusion would also require an independently checkable infeasibility proof and a checked connection to the emitted model.

With no `--holes` option, the default search removes width-five rows and anchors a forced run of full rows. A fixed-hole search, for example `--trails 9 --holes 48`, keeps width-five rows available because normalization can change the total hole count. `--raw` also retains those rows. The model adds connectivity constraints when an incumbent contains a component with no trail start.

## Replay the profile counts

A profile is an unordered multiset of pairs `(holes, rows)`, one pair per trail. The first program counts profiles meeting its arithmetic conditions. The second uses the archived subtrail checks in `price137_core_coverage.jsonl` to count how many profiles contain an excluded subconfiguration.

```sh
c++ -O2 -std=c++17 "$scripts/price137_census.cpp" -o ./price137_census
./price137_census --no-width-five
python3 -B "$scripts/price137_core_census.py" > core-replay.jsonl
diff "$scripts/price137_core_census.log" core-replay.jsonl
```

The arithmetic consequence of width-five removal, together with the archived nine-trail exclusion for at most 42 holes, leaves **2,073,505,789** profiles. Applying the 26 archived subtrail exclusions removes **12,383**, leaving **2,073,493,406** profiles under those filters. The last output record reports `also_using_M9_H_ge_43=12383`. A surviving profile may have no geometric realization. These counts do not determine the time required to exhaust the search.

To regenerate the sampled geometric subtrail tests or replicate the original timing pilot, use the commands below. Fixed random seeds identify the samples. Runtime, peak memory and the profiles that hit resource limits can vary by machine. `price137_pilot.jsonl`, `price137_core_coverage.jsonl` and `price137_milp_measurements.jsonl` preserve the original observations.

```sh
python3 -B "$scripts/price137_core_coverage.py" > core-geometry-local.jsonl
python3 -B "$scripts/price137_pilot.py" > pilot-local.jsonl
```

To rerun the earlier nine-trail low-hole check used in the count, compile the profile checker and replay its three ranges. The two last logs are included under `hole_frontier`.

```sh
lower=Haruhi7/lower
c++ -O3 -std=c++17 -I"$lower" "$lower/hole_frontier/corecheck.cpp" -o ./frontier-check
python3 -B "$lower/check_next_counts.py" 1 --through 40 --checker "$PWD/frontier-check"
python3 -B "$lower/hole_frontier/replay_frontier.py" "$lower" ./frontier-check 1 41
python3 -B "$lower/hole_frontier/replay_frontier.py" "$lower" ./frontier-check 1 42
```

## Lean replay

The `lean/README.md` inside the download gives the pinned source inputs and two replay commands. Three endpoint modules were recompiled with Lean 4.35.0 against an existing dependency build. The 293-module source manifest and ten data files were checked during staging. A full rebuild from a clean dependency tree remains available for independent reproduction. `lean/checked-endpoints.log` records the axiom inventories, including uses of native evaluation. From `price137`, verify the extracted files with `shasum -a 256 -c SHA256SUMS` on macOS or `sha256sum -c SHA256SUMS` on Linux.

## Mathematical scope and credits

A word of length 1,196 that visits all 720 classes and completes exactly 48 yields an exact-price board with 1 to 11 trails. The reduction to 2 through 9 uses three separate trail-cap hypotheses for 1, 10 and 11 trails. Proving the eight exclusions, together with the endpoint hypotheses, would give an independent route to the 5,901 lower bound. No complete geometric exclusion or machine-checkable infeasibility certificate for the eight branches accompanies this release.

The package includes attribution, original license notices and hashes for Halfar’s tables and Teodorescu’s control word under `third-party/`. Jaehun Jung directed the research. OpenAI Codex assisted with the formalization, programs and documentation.
