# From SMT to ASP: Solver-Based Approaches to Solving Datalog Synthesis-as-Rule-Selection Problems

Welcome to the artifact for POPL'23 submission #63!

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
docker run -it aaronbembenek/datalog-synth-smt-asp-artifact:0.1.0 # may require sudo
```

We ran the experiments reported in the paper directly (i.e., without Docker) on a powerful AWS EC2 (32 vCPUs and 128 GB RAM).
However, many of the results should be reproducible using Docker on a personal laptop.
You want to make sure that Docker is provisioned with enough CPUs and RAM (in our personal setup, Docker has access to 4 vCPUs and 16 GB RAM).
If you are using Docker Desktop on OS X, you can set this via Preferences > Resources > Advanced.

## Artifact Structure

Once you are running a Docker container, you should see the following directories:

- XXX

## "Kick the Tires" Phase Instructions

These instructions will run a small set of experiments; they took about XXX minutes to complete on our laptop.
If they complete successfully, that hopefully means that you will not encounter bugs when running the full evaluation phase.
The number of trials, benchmarks, timeout, etc. can be modified by changing the variables in the appropriate script.

### Evaluating Sections 7.1-7.4

This command will run all the tools (five trials per tool) on two quick-running benchmarks (`path` and `traffic`) from the evaluation in Sections 7.1-7.4 of the paper:

```
XXX
```

It takes about XXX minutes on our laptop.

Once it is complete, results will be put in the directory `section_7_1_to_7_4_results/`.

**How will they know that results are correct?**

### Evaluating Section 7.5 (Regular Benchmarks)

This command will run all the tools (5 trials per tool) on two quick-running benchmarks (`path` and `traffic`) from the regular benchmark evaluation in Section 7.5 of the paper:

```
XXX
```

It takes about XXX minutes on our laptop.

Once it is complete, results will be put in the directory `section_7_5_regular_results/`.

### Evaluating Section 7.5 (Scaling Evaluation)

This command will run all the tools (5 trials per tool) on two benchmarks (`path` and `traffic`) from the scaling evaluation in Section 7.5 of the paper:

```
XXX
```

It takes about XXX minutes on our laptop.

Once it is complete, results will be put in the directory `section_7_5_scaling_results/`.

## "Evaluation" Phase Instructions

XXX

### Evaluating Sections 7.1-7.4

XXX

### Evaluating Section 7.5 (Regular Benchmarks)

XXX

The scripts invoked here do not report the ProSynth compilation time.
To get a sense for this (for, say, the `scc` benchmark), you can run this command:

```
rm -rf ~/benchmarks/build/regular/scc && time cmake --build ~/benchmarks/build --target scc 
```

This command compiles the benchmark twice (once for ProSynth and once for MonoSynth), and so the ProSynth compilation time will be approximately half of the reported time.

### Evaluating Section 7.5 (Scaling Evaluation)

XXX

## Updates

None so far.

## TODO

- Write scripts for each of the experiments
- Clean up scripts directory
- Test scripts