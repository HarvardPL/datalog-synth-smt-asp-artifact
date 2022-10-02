# From SMT to ASP: Solver-Based Approaches to Solving Datalog Synthesis-as-Rule-Selection Problems

Welcome to the artifact for POPL'23 artifact #7!

This document is divided into the following sections:
- Setup
- Artifact structure
- "Kick the tires" phase instructions
- "Evaluation" phase instructions
- Updates

We'll push updates to the final section as necessary during the evaluation process to report any bug fixes or new instructions.
Accordingly, if you are reading a local copy of this document, please also check the [GitHub repository](https://github.com/HarvardPL/datalog-synth-smt-asp-artifact) for any updates.

## Setup 

Our artifact can be found on a Docker image, publicly available on [Docker Hub](https://hub.docker.com/r/aaronbembenek/datalog-synth-smt-asp-artifact).
Assuming that you have Docker installed, to download the image and run an interactive session you can simply use this command:

```
docker run --name datalog-synth-smt-asp-artifact -it aaronbembenek/datalog-synth-smt-asp-artifact:0.1.0 # may require sudo
```

We ran the experiments reported in the paper directly (i.e., without Docker) on a powerful AWS EC2 (32 vCPUs and 128 GB RAM).
However, many of the results should be reproducible using Docker on a personal laptop.
You want to make sure that Docker is provisioned with enough CPUs and RAM (in our personal setup, Docker has access to 4 vCPUs and 16 GB RAM).
If you are using Docker Desktop on OS X, you can set this via Preferences > Resources > Advanced.

## Artifact Structure

Once you are running a Docker container, you should see the following directories:

- `benchmarks/`: the benchmarks, split between `regular` (the 40 ProSynth benchmarks) and `scale` (the scaling benchmarks).
Each benchmark directory contains a `rules.small.dl` file with the candidate rules, plus `.fact` files for the example input, `.expected` files  for the expected output tuples,  and `.complement` files for the undesired output tuples.
- `bin/`: links to the ProSynth-X, MonoSynth-X, LoopSynth-X, and AspSynth-X tools.
- `build_data/`: miscellaneous data used to set up the image.
- `datalog-smmt-cvc4-impl/`: our implementation of Datalog-as-a-monotonic-theory, hacked into CVC4 as a new theory.
This directory contains all the source for CVC4, but most of our changes are in `~/datalog-smmt-cvc4-impl/CVC4-1.8/src/theory/datalog`. 
- `datalog-smmt-z3-impl/`: our implementation of Datalog-as-a-monotonic-theory, built on top of Z3 using the custom propagator API.
- `gensynth/`: a clone of the [GenSynth repository](https://github.com/jonomendelson/gensynth).
- `scripts/`: source code for ProSynth-X, LoopSynth-X, and AspSynth-X tools, plus benchmarking scripts.
- `section_7_5_regular_results/`: results for the regular (i.e., non-scaling) experiments in the Section 7.5 are put here.
- `section_7_5_scale_results/`: results for the scaling experiments in the Section 7.5 are put here.
- `sections_7_1_to_7_4_results/`: results for the experiments in Sections 7.1-7.4 are put here.

### Results Format

Say you run the "kick the tires" experiment for the experiments in Sections 7.1-7.4 (the other experiments are similar).
Results will be in the following format:

- Raw data for tool `X` will be put in `~/sections_7_1_to_7_4_results/data/[X]/kick_the_tires/`.
- The processed data will appear in `~/sections_7_1_to_7_4_results/kick_the_tires/`, including:
    - `kick_the_tires.csv`, which contains all the results from all the tools.
    - Some `.tsv` files that hold tables and can be opened in a spreadsheet viewer (e.g., Excel, Numbers, Google Sheets).
    - Some `.pdf` files containing plots.
- Scripts for generating the tables and plots are in `~/sections_7_1_to_7_4_results/chart-scripts/`.
- Scripts for running various tiny data analyses that are reported in the text (like the average time per Datalog call) can be found in `sections_7_1_to_7_4_results/stats-scripts/`.

To view the `.pdf` files (and to open the `.csv` or `.tsv` files in a spreadsheet viewer), you will have to copy them from the Docker container to the host machine (e.g., your laptop) using the `docker cp` command:

```
docker cp datalog-synth-smt-asp-artifact:/root/sections_7_1_to_7_4_results/kick_the_tires/figure_9.pdf . # may require sudo
```

## "Kick the Tires" Phase Instructions

These instructions will run all the experiments from Section 7 of the paper, but with fewer trials (2 vs 10) and a shorter timeout (10 seconds vs 10 minutes).
If they complete successfully, that hopefully means that you will not encounter bugs when running the full evaluation phase.
The number of trials, benchmarks, timeout, etc. can be modified by changing the variables in the appropriate script.

**Say how they can check if the results make sense.**

### Evaluating Sections 7.1-7.4

This command will run the experiments in Sections 7.1-7.4:

```
~/scripts/kick_the_tires_sections_7_1_to_7_4.sh
```

It takes about XXX seconds on our laptop.

Once it is complete, results will be put in the directory `~/sections_7_1_to_7_4_results/kick_the_tires/`.

### Evaluating Section 7.5 (Regular Benchmarks)

This command will run the regular (non-scaling) experiments in Section 7.5:

```
~/scripts/kick_the_tires_section_7_5_regular.sh
```

It takes about 16 minutes on our laptop.

Once it is complete, results will be put in the directory `~/section_7_5_regular_results/kick_the_tires/`.
It should contain the following files:

- XXX 
It should contain the following files:

- XXX 

### Evaluating Section 7.5 (Scaling Evaluation)

This command will run all the scaling experiments in Section 7.5:

```
~/scripts/kick_the_tires_section_7_5_scale.sh
```

It takes about 10 minutes on our laptop.

Once it is complete, results will be put in the directory `~/section_7_5_scale_results/kick_the_tires`.
It should contain the following files:

- `kick_the_tires.csv`: the aggregated data
- `times.tsv`: a performance table (times in seconds)
- `figure_10b`: Figure 10b from the paper (using the new data, of course)

## "Evaluation" Phase Instructions

XXX

### Evaluating Sections 7.1-7.4

XXX

### Evaluating Section 7.5 (Regular Benchmarks)

XXX

The scripts invoked here do not recompile the benchmarks to record ProSynth compilation time; instead, they reuse compilation time results from the experiments reported in the paper (the data is in `~/section_7_5_regular_results/data/prosynth-z3/compilation_times/`).
To get a sense for the compilation times yourself (for, say, the `path` benchmark), you can run this command:

```
export BENCH=path && rm -rf ~/benchmarks/build/regular/$BENCH && time cmake --build ~/benchmarks/build --target $BENCH 
```

This command compiles the benchmark's Souffle code twice (once for ProSynth and once for MonoSynth), and so the ProSynth compilation time will be approximately half of the reported time.

### Evaluating Section 7.5 (Scaling Evaluation)

XXX

## Updates

None so far.

## TODO

- [ ] Write scripts for section 7.5 experiments
    - [X] Add script for evaluating 7.5 scale
    - [X] Update data processing scripts for 7.5 scale experiments
    - [ ] Update data processing scripts for 7.5 regular experiments
    - [ ] Update data processing scripts for 7.1-7.4 experiments
- Clean up scripts directory
- Remove unused directories
- Instructions on how to view PDFs
- Come up with better format for tables (maybe just output a TSV)
- Test scripts
- Clean up CVC4 source