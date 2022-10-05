# From SMT to ASP: Solver-Based Approaches to Solving Datalog Synthesis-as-Rule-Selection Problems

Welcome to POPL'23 artifact #7!

This document is divided into the following sections:

1. Setup
2. Artifact Structure
3. "Kick the Tires" Phase Instructions
4. "Evaluation" Phase Instructions
5. Changelog

We'll push updates to the final section as necessary during the evaluation process to report any bug fixes or new instructions.

## 1. Setup 

Our artifact can be found on Zenodo; **please check for the newest version of it.**

First, download the archive of the Docker image, named `datalog-synth-smt-asp-artifact.tar.gz`.
Assuming that you have Docker installed, you can then load the image from the archive using this command:

```bash
docker load < datalog-synth-smt-asp-artifact.tar.gz # may require sudo
```

To run an interactive session in a container named `datalog-synth-smt-asp-artifact`, run this command:

```bash
docker run --name datalog-synth-smt-asp-artifact -it datalog-synth-smt-asp-artifact:latest # may require sudo
```

We ran the experiments reported in the paper directly (i.e., without Docker) on a powerful AWS EC2 (32 vCPUs and 128 GB RAM).
However, many of the results should be reproducible using Docker on a personal laptop.
You want to make sure that Docker is provisioned with enough CPUs and RAM (in our personal setup, Docker has access to 4 vCPUs and 16 GB RAM).
If you are using Docker Desktop on OS X, you can set this via Preferences > Resources > Advanced.

## 2. Artifact Structure

Once you are running a Docker container, you should see the following directories in `/root/` (the home directory):

- `benchmarks/`: the benchmarks, split between `regular` (the 40 ProSynth benchmarks) and `scale` (the scaling benchmarks).
Each benchmark directory contains a `rules.small.dl` file with the candidate rules, plus `.fact` files for the example input, `.expected` files  for the expected output tuples,  and `.complement` files for the undesired output tuples.
- `bin/`: links to the ProSynth-X, MonoSynth-X, LoopSynth-X, and AspSynth-X tools.
- `build_data/`: miscellaneous data used to set up the image.
- `datalog-smmt-cvc4-impl/`: our implementation of Datalog-as-a-monotonic-theory, hacked into CVC4 as a new theory.
    - This directory contains all the source for CVC4, but most of our changes are in `./CVC4-1.8/src/theory/datalog`.
    - The source code for MonoSynth-CVC4 is in `./CVC4-1.8/src/theory/datalog/monosynth/`.
- `datalog-smmt-z3-impl/`: our implementation of Datalog-as-a-monotonic-theory, built on top of Z3 using the custom propagator API.
The source code for MonoSynth-Z3 is in `./src/monosynth/`.
- `gensynth/`: a clone of the [GenSynth repository](https://github.com/jonomendelson/gensynth).
- `scripts/`: source code for ProSynth-X, LoopSynth-X, and AspSynth-X tools, plus benchmarking scripts.
- `section_7_5_regular_results/`: results for the regular (i.e., non-scaling) experiments in Section 7.5 are put here.
- `section_7_5_scale_results/`: results for the scaling experiments in Section 7.5 are put here.
- `sections_7_1_to_7_4_results/`: results for the experiments in Sections 7.1-7.4 are put here.

### Results Format

Say you run the "kick the tires" experiment for the experiments in Sections 7.1-7.4 (the other experiments are similar).
Results will be in the following format:

- Raw data for tool `X` will be put in `/root/sections_7_1_to_7_4_results/data/[X]/kick_the_tires/`.
- The processed data will appear in `/root/sections_7_1_to_7_4_results/kick_the_tires/`, including:
    - `kick_the_tires.csv`, which contains the assembled results from all the tools.
    - Some `.tsv` files that represent tables and can be opened in a spreadsheet viewer (e.g., Excel, Numbers, Google Sheets).
    - Some `.pdf` files containing plots.
- Scripts for generating the tables and plots are in `/root/sections_7_1_to_7_4_results/chart-scripts/`.
- Scripts for running various tiny data analyses that are reported in the text (like the average time per Datalog call) can be found in `/root/sections_7_1_to_7_4_results/stats-scripts/`.

To view the `.pdf` files (and to open the `.csv` or `.tsv` files in a spreadsheet viewer), you will have to copy them from the Docker container to the host machine (e.g., your laptop) using the `docker cp` command.
For example:

```bash
docker cp datalog-synth-smt-asp-artifact:/root/sections_7_1_to_7_4_results/kick_the_tires/figure_9.pdf . # may require sudo
```

## 3. "Kick the Tires" Phase Instructions

These instructions will run all the experiments from Section 7 of the paper, but with fewer trials (2 vs 10) and a shorter timeout (10 seconds vs 10 minutes).
If the trials run successfully and the scripts post-process results correctly, that hopefully means that you will not encounter bugs when running the full evaluation phase.
The number of trials, benchmarks, timeout, etc. can be modified by changing the variables in the appropriate script.

**Please ensure that the experiments are generating the plots and tables correctly.**
The results should generally look like the results reported in the paper, modulo the much shorter timeouts.

### Evaluating Sections 7.1-7.4

This command will run the experiments in Sections 7.1-7.4:

```bash
/root/scripts/kick_the_tires_sections_7_1_to_7_4.sh
```

It takes about 27 minutes on our laptop.

Once it is complete, results will be put in the directory `/root/sections_7_1_to_7_4_results/kick_the_tires/`.
It should contain the following files:

- `figure_9.pdf` (boxplot)
- `kick_the_tires.csv` (assembled data)
- `table_2.tsv` (average time, in seconds)
- `table_3.tsv` (number of conflicts)

### Evaluating Section 7.5 (Regular Benchmarks)

This command will run the regular (non-scaling) experiments in Section 7.5:

```bash
/root/scripts/kick_the_tires_section_7_5_regular.sh
```

It takes about 17 minutes on our laptop.

Once it is complete, results will be put in the directory `/root/section_7_5_regular_results/kick_the_tires/`.
It should contain the following files:

- `figure_10a.pdf` (boxplot)
- `kick_the_tires.csv` (assembled data)
- `program_sizes.pdf` (boxplot of synthesized solution size)
- `times.tsv` (average time, in seconds)

### Evaluating Section 7.5 (Scaling Evaluation)

This command will run all the scaling experiments in Section 7.5:

```bash
/root/scripts/kick_the_tires_section_7_5_scale.sh
```

It takes about 10 minutes on our laptop.

Once it is complete, results will be put in the directory `/root/section_7_5_scale_results/kick_the_tires`.
It should contain the following files:

- `figure_10b` (bar graph)
- `kick_the_tires.csv` (assembled data)
- `times.tsv` (average time, in seconds)

## 4. "Evaluation" Phase Instructions

The instructions here are similar to the instructions for the "kick the tires" phase; the main difference is that the scripts are set to do more trials (10) with longer timeouts (10 minutes).
These are the settings used in the paper, but they will take a **LONG** time to complete, so you might want to adjust them (by modifying the variables in the relevant script).

### Evaluating Sections 7.1-7.4

Run this command:

```bash
/root/scripts/evaluation_sections_7_1_to_7_4.sh
```

**Modifying this script to run 3 trials with a timeout of 180 seconds**, it takes 196 minutes to complete on our laptop.

You should see the following files in the `/root/sections_7_1_to_7_4_results/evaluation/` directory:

- `figure_9.pdf` (boxplot)
- `evaluation.csv` (assembled data)
- `table_2.tsv` (average time, in seconds)
- `table_3.tsv` (number of conflicts)

These are the main claims that this experiment should validate:

- MonoSynth-Z3 on average outperforms ProSynth-Z3, although the gains are not universal.
- LoopSynth-Z3 occasionally outperforms ProSynth-Z3 and MonoSynth-Z3.
- MonoSynth-CVC4 consistently outperforms ProSynth-CVC4.
- LoopSynth-CVC4 outperforms ProSynth-CVC4, and occasionally MonoSynth-CVC4.
- ASPSynth-Clingo has by far the best performance: it is fast and consistent.
- ASPSynth-WASP also performs consistently well, except for a single benchmark it times out on (`sql-15`).

### Evaluating Section 7.5 (Regular Benchmarks)

Run this command:

```bash
/root/scripts/evaluation_section_7_5_regular.sh
```

**Modifying this script to run 3 trials with a timeout of 180 seconds**, it takes 106 minutes to complete on our laptop.

You should see the following files in the `/root/section_7_5_regular_results/evaluation/` directory:

- `figure_10a.pdf` (boxplot)
- `evaluation.csv` (assembled data)
- `program_sizes.pdf` (boxplot of synthesized solution size)
- `times.tsv` (average time, in seconds)

These are the main claims that this experiment should validate:

- The ASP-based approaches (especially ASPSynth-Clingo and ASPSynth-Clingo-MinPremise, and to a lesser extent ILASP) achieve significant speedups on average over ProSynth (even excluding ProSynth compilation time) and GenSynth.
- GenSynth has the worst overall performance on this benchmark suite.
- ASPSynth-Clingo-MinPremise performs about the same as ASPSynth-Clingo on this benchmark suite (i.e., minimization is not expensive here).
- ASPSynth-Clingo-MinPremise and ILASP synthesize smaller solutions (i.e., programs with fewer premises) than ProSynth.

**NB #1:** Unless you are running with many cores, GenSynth might be slower than what is reported in the paper, as the paper reports it running 32 populations in parallel (this setting can be modified in the benchmarking script).

**NB #2:** The scripts invoked here do not recompile the benchmarks to record ProSynth compilation time; instead, they reuse compilation time results from the experiments reported in the paper (the data is in `/root/section_7_5_regular_results/data/prosynth-z3/compilation_times/`).
To get a sense for the compilation times yourself (for, say, the `path` benchmark), you can run this command:

```bash
export BENCH=path && rm -rf /root/benchmarks/build/regular/$BENCH && time cmake --build /root/benchmarks/build --target $BENCH 
```

CMake compiles the benchmark's Souffle code twice (once for ProSynth and once for MonoSynth), and so the ProSynth compilation time will be approximately half of the reported time.

### Evaluating Section 7.5 (Scaling Evaluation)

Run this command:

```bash
/root/scripts/evaluation_section_7_5_scale.sh
```

**Modifying this script to run 3 trials with a timeout of 180 seconds**, it takes 161 minutes to complete on our laptop.

You should see the following files in the `/root/section_7_5_scale_results/evaluation/` directory:

- `figure_10b` (bar graph)
- `evaluation.csv` (assembled data)
- `times.tsv` (average time, in seconds)

These are the main claims that this experiment should validate:

- The ASP-based approaches (ASPSynth-Clingo, ASPSynth-Clingo-MinPremise, ILASP) scale better than ProSynth.
- ASPSynth-Clingo scales better than ASPSynth-Clingo-MinPremise, which scales better than ILASP.
- Relative to GenSynth, the ASP-based approaches scale well if one dimension of the experiment is scaled up (i.e., the number of candidate rules or the size of the input-output example), but do not scale as well if both dimensions are scaled up simultaneously.

**NB #1:** Unless you are running with many cores, ASPSynth-Clingo-MinPremise, ILASP, and GenSynth might be slower than what is reported in the paper.
The paper reports ASPSynth-Clingo-MinPremise using 32 threads to find a minimal solution, ILASP using 16 threads, and GenSynth running 32 populations in parallel.
These settings can be modified in the benchmarking script.

## 5. Changelog 

Nothing so far.