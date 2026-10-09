# ResNet-50 FP16 Training Step (PyTorch) Benchmark

[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![CI](https://github.com/garys-gpu-benchmarks/321-gpu-bench-amd-resnet50-pytorch-training-ubu2604/actions/workflows/ci.yml/badge.svg)](https://github.com/garys-gpu-benchmarks/321-gpu-bench-amd-resnet50-pytorch-training-ubu2604/actions/workflows/ci.yml)

Target: Ubuntu 26.04 · AMD · see Hardware Requirements. This is a host benchmark, not a laptop `pip install` project.

## Quick Start

```bash
git clone https://github.com/garys-gpu-benchmarks/321-gpu-bench-amd-resnet50-pytorch-training-ubu2604.git
cd 321-gpu-bench-amd-resnet50-pytorch-training-ubu2604
sudo bash setup.sh --assume-yes
bash run_benchmark.sh --profile smoke --validate
```
Results are written to `results/benchmark.db` and `results/summary.json`.

This workload is executed on the validation host after the repository is copied there. `setup.sh` and `run_benchmark.sh` do not open an outbound SSH session.

Prerequisites: Ubuntu 26.04; AMD; Python 3.14.4; root or sudo for `setup.sh`. Framework: Bash, SQLite, Python, PyYAML, ROCm Runtime, PyTorch-ROCm, torchvision, HIP/ROCm. This is a host benchmark, not a laptop `pip install` project.

```mermaid
flowchart LR
  setup.sh --> run_benchmark.sh --> parse_results.py --> results/benchmark.db
```

## 1. Overview

Trains torchvision ResNet-50 in-process on synthetic images (one timed pass, not two subprocess trainers). image_size, batch_size, warmup_iters, num_iterations, precision (fp16 default), and learning_rate come from yaml. dataset_name is ignored. achieved_TFLOPS and memory_bandwidth_GBps are hardcoded 1.0. Sweep dimensions: dataset_name, precision, image_size, batch_size, learning_rate, warmup_iters, num_iterations.

## 2. What It Validates

- Validates step_time_ms, images/s, and peak memory from an inline PyTorch trainer. This is not pytest
- #1: Training batch step time (step_time_ms); is present and physically sensible.
- #2: Training throughput (images_per_sec); is present and physically sensible.
- #3: Achieved TFLOPS, model-FLOPs estimate (achieved_TFLOPS); is present and physically sensible.
- #4: Peak GPU memory usage (peak_gpu_memory_GB); is present and physically sensible.
- #5: Modeled parameter and input traffic, GB/s (modeled_param_input_traffic_gb_s) is present and physically sensible.

## 3. Metrics Captured

- **#1: Training batch step time** — stored as `step_time_ms`.
- **#2: Training throughput** — stored as `images_per_sec`.
- **#3: Achieved TFLOPS, model-FLOPs estimate** — stored as `achieved_TFLOPS`.
- **#4: Peak GPU memory usage** — stored as `peak_gpu_memory_GB`.
- **#5: Modeled parameter and input traffic, GB/s** — stored as `modeled_param_input_traffic_gb_s`.

## 4. Hardware Requirements

### Supported environment

- OS: Ubuntu 26.04
- GPU vendor: AMD
- Framework family: Bash, SQLite, Python, PyYAML, ROCm Runtime, PyTorch-ROCm, torchvision, HIP/ROCm
- Python: Python 3.14.4

### Reference validation environment

The tables below describe the machine used to generate the reference results. They are not a requirement that every user buy that exact cloud instance.

### System

Ubuntu 26.04 / AMD / Bash, SQLite, Python, PyYAML, ROCm Runtime, PyTorch-ROCm, torchvision, HIP/ROCm

### GPU

Ubuntu 26.04 / AMD / Bash, SQLite, Python, PyYAML, ROCm Runtime, PyTorch-ROCm, torchvision, HIP/ROCm

## 5. Software Requirements

| Component | Version |
|---|---|
| OS | Ubuntu 26.04 |
| Kernel | kernel 7.0.0 |
| Python | Python 3.14.4 |
| ROCm | ROCm 7.14 |
| rocBLAS | rocBLAS 5.2.0 |

Trains torchvision ResNet-50 in-process on synthetic images (one timed pass, not two subprocess trainers). image_size, batch_size, warmup_iters, num_iterations, precision (fp16 default), and learning_rate come from yaml. dataset_name is ignored.

## 6. Installation

```bash
Run inline torchvision.models.resnet50 training on synthetic tensors. Does not launch a separate train_resnet50.py twice
```

## 7. Running the Benchmark

```bash
Run inline torchvision.models.resnet50 training on synthetic tensors. Does not launch a separate train_resnet50.py twice
```

**Validating results separately:**

```bash
export BENCHMARK_PYTHON=/usr/bin/python3.13  # optional
python3 -m venv .venv
source ".venv/bin/activate"
".venv/bin/python" scripts/validate_results.py
```

## 8. Output

### `results/benchmark.db` (SQLite)

CSV with two identical training-sample rows

sample_index,step,loss,images_per_sec,step_time_ms,achieved_TFLOPS,memory_bandwidth_GBps,modeled_param_input_traffic_gb_s,peak_gpu_memory_GB
0,0,1.0,120,16.7,1.2,40,40,2.5

```bash
Run inline torchvision.models.resnet50 training on synthetic tensors. Does not launch a separate train_resnet50.py twice
```

### `results/summary.json`

Consolidated metrics from the most recent run — suitable for CI artifact upload or dashboard ingestion.

### `results/raw/<timestamp>.txt`

CSV with two identical training-sample rows

sample_index,step,loss,images_per_sec,step_time_ms,achieved_TFLOPS,memory_bandwidth_GBps,modeled_param_input_traffic_gb_s,peak_gpu_memory_GB
0,0,1.0,120,16.7,1.2,40,40,2.5

## 9. Baselines / Thresholds

Expected ranges and gates live in `config/benchmark_config.yaml` under `baselines:` or `thresholds:`. To update them, edit that file — never edit validation code directly.

## 10. Troubleshooting

**`setup.sh` missing collector**
Create cannot finish without `scripts/collect_workload.py`.

**`self_check` overlay rewritten**
Do not overwrite files listed in `results/overlay_lock.json`.

**Remote SSH drop during setup**
Reconnect and resume `bash setup.sh --assume-yes`. Do not wipe `.venv` or `.cache`.

## 11. NVIDIA H100 Coding Differences

Primary target is AMD ROCm. NVIDIA notes in this section are reference only and are not the execution path.

## Repository layout

```text
.
├── setup.sh
├── run_benchmark.sh
├── benchmark_specification.json
├── .github/workflows/      # thin CI callers (see Continuous Integration)
├── config/
├── scripts/
├── src/
├── tests/
├── docs/
├── results/
└── LICENSE
```

## Continuous Integration

| Workflow | Runs on | When | What it does |
|---|---|---|---|
| [CI](.github/workflows/ci.yml) | GitHub-hosted runner | every pull request, and every push to `main` | shellcheck, ruff, `bash -n`, `compileall`, `run_benchmark.sh --help`, specification schema, the results validator on a seeded fixture, required files, and actionlint. No GPU and no benchmark run. |
| [GPU Smoke Benchmark](.github/workflows/gpu-smoke.yml) | self-hosted runner labeled `gpu`, `amd`, `ubu2604` | only when started by hand: **Actions → GPU Smoke Benchmark → Run workflow** (choose `smoke`, `baseline` or `extended`) | Verifies the pre-provisioned GPU stack, records `results/environment.json` (driver, runtime, kernel, GPU), runs the profile with `--validate`, shows headline metrics on the run page, and uploads the results. |

Both files are short callers. The steps themselves live once, for every workload in the suite, in [`garys-gpu-benchmarks/shared-workflows`](https://github.com/garys-gpu-benchmarks/shared-workflows), pinned at `@v1`. The GPU workflow is never triggered by pull requests, so code from a fork cannot run on the GPU host.

### Running it as part of the AMD Ubuntu 26.04 bundle

This repository is one of the 32 workloads in [`bundle-amd-ubuntu-2604`](https://github.com/garys-gpu-benchmarks/bundle-amd-ubuntu-2604), which holds them as git submodules. To put the whole bundle on a GPU host and run this workload from it:

```bash
git clone --recurse-submodules https://github.com/garys-gpu-benchmarks/bundle-amd-ubuntu-2604 /opt/benchmarks
cd /opt/benchmarks/321-gpu-bench-amd-resnet50-pytorch-training-ubu2604
bash run_benchmark.sh --profile smoke --validate
```
