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

Our artifact can be found on a Docker image, publicly available on [Docker Hub](https://hub.docker.com/r/aaronbembenek/XXX).
Assuming that you have Docker installed, to download the image and run an interactive session you can simply use the command

```
docker run -it aaronbembenek/XXX:0.1.0 # may require sudo
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

### Evaluating Sections 7.1-7.4

XXX

### Evaluating Section 7.5 (Regular Benchmarks)

XXX

### Evaluating Section 7.5 (Scaling Evaluation)

XXX

## "Evaluation" Phase Instructions

XXX

### Evaluating Sections 7.1-7.4

XXX

### Evaluating Section 7.5 (Regular Benchmarks)

XXX

### Evaluating Section 7.5 (Scaling Evaluation)

XXX

## Updates

None so far.