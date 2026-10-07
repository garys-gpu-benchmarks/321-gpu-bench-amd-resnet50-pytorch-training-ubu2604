# SPEC.md: "The Human How"; Exact technical requirements, environment setup, implementation details, etc.

## Execution Description

Trains torchvision ResNet-50 in-process on synthetic images (one timed pass, not two subprocess trainers). image_size, batch_size, warmup_iters, num_iterations, precision (fp16 default), and learning_rate come from yaml. dataset_name is ignored. achieved_TFLOPS and memory_bandwidth_GBps are hardcoded 1.0. Sweep dimensions: dataset_name, precision, image_size, batch_size, learning_rate, warmup_iters, num_iterations.

## Parameters

| Parameter | CLI Flag | Tested Values | Default | Description |
| --- | --- | --- | --- | --- |
| dataset_name | `--dataset-name` | smoke=synthetic, baseline=synthetic, extended=synthetic | synthetic | From Parameter list; see Execution Description With Parameters. |
| precision | `--precision` | smoke=fp16, baseline=fp16, extended=fp16 | fp16 | From Parameter list; see Execution Description With Parameters. |
| image_size | `--image-size` | smoke=64, baseline=224, extended=224 | 224 | From Parameter list; see Execution Description With Parameters. |
| batch_size | `--batch-size` | smoke=2, baseline=8, extended=16 | 8 | From Parameter list; see Execution Description With Parameters. |
| learning_rate | `--learning-rate` | smoke=0.1, baseline=0.1, extended=0.1 | 0.1 | From Parameter list; see Execution Description With Parameters. |
| warmup_iters | `--warmup-iters` | smoke=1, baseline=2, extended=5 | 2 | From Parameter list; see Execution Description With Parameters. |
| num_iterations | `--num-iterations` | smoke=3, baseline=18000, extended=49000 | 18000 | From Parameter list; see Execution Description With Parameters. |

## Invocation

```bash
Run inline torchvision.models.resnet50 training on synthetic tensors. Does not launch a separate train_resnet50.py twice
```

## Raw Output Format

CSV with two identical training-sample rows

sample_index,step,loss,images_per_sec,step_time_ms,achieved_TFLOPS,memory_bandwidth_GBps,modeled_param_input_traffic_gb_s,peak_gpu_memory_GB
0,0,1.0,120,16.7,1.2,40,40,2.5

## Metrics

- **#1: Training batch step time** — stored as `step_time_ms`.
- **#2: Training throughput** — stored as `images_per_sec`.
- **#3: Achieved TFLOPS, model-FLOPs estimate** — stored as `achieved_TFLOPS`.
- **#4: Peak GPU memory usage** — stored as `peak_gpu_memory_GB`.
- **#5: Modeled parameter and input traffic, GB/s** — stored as `modeled_param_input_traffic_gb_s`.

## Framework

Trains torchvision ResNet-50 in-process on synthetic images (one timed pass, not two subprocess trainers). image_size, batch_size, warmup_iters, num_iterations, precision (fp16 default), and learning_rate come from yaml. dataset_name is ignored.

## Installation and Execution Summary

Run an in-process PyTorch ResNet-50 FP16 trainer (torchvision.models.resnet50) on synthetic images with yaml image_size, batch_size, and num_iterations, then write step_time_ms, images_per_sec, and peak memory, to measure mixed-precision training efficiency. This is one inline pass, not two subprocess trainers

## Platform Portability

- **AMD (primary):** ```bash
Run inline torchvision.models.resnet50 training on synthetic tensors. Does not launch a separate train_resnet50.py twice
```
- **NVIDIA:** Primary target is AMD ROCm. NVIDIA notes in this section are reference only and are not the execution path.

## Model Context Protocols

- **Active:** None

## Execution-Loop Validation Contract

EXECUTION CHAIN: `run_benchmark.sh` ➔ raw output ➔ `scripts/parse_results.py` ➔ `results/benchmark.db` ➔ `scripts/validate_results.py`

This benchmark uses a lightweight, SQLite-integrated execution loop for result validation. All validation is performed by `scripts/validate_results.py`.

### Validation script usage

```bash
export BENCHMARK_PYTHON=/usr/bin/python3.13  # optional; select the installed interpreter

# After a live run:
".venv/bin/python" scripts/validate_results.py --db results/benchmark.db

# CI / no-GPU path (seeds fixture and validates it):
".venv/bin/python" scripts/validate_results.py --seed-fixture --quiet

# Override DB path via environment variable:
BENCHMARK_DB=tests/fixtures/benchmark.db \
  ".venv/bin/python" scripts/validate_results.py
```

### Run artifact contract

CSV with two identical training-sample rows

sample_index,step,loss,images_per_sec,step_time_ms,achieved_TFLOPS,memory_bandwidth_GBps,modeled_param_input_traffic_gb_s,peak_gpu_memory_GB
0,0,1.0,120,16.7,1.2,40,40,2.5

```bash
bash run_benchmark.sh --help
bash run_benchmark.sh --profile smoke --validate
bash run_benchmark.sh --profile baseline --validate
bash run_benchmark.sh --profile extended --validate
```
`run_benchmark.sh --help` prints usage and exits. The harness calls `scripts/ensure_setup.sh` when `.setup_state` is absent.

### Required integrity checks (built into `validate_results.py`)

1. Latest run exists and `runs.status = 'ok'`.
2. `run.error_message` is NULL.
3. `started_at` and `finished_at` are valid ISO-8601 UTC strings.
4. All required aggregate metrics in `runs` are non-NULL and finite.
5. All required aggregate metrics are physically sensible (positive values). Trains torchvision ResNet-50 in-process on synthetic images (one timed pass, not two subprocess trainers). image_size, batch_size, warmup_iters, num_iterations, precision (fp16 default), and learning_rate come from yaml. dataset_name is ignored.
6. At least 2 sample rows exist for the latest `run_id` (sweep coverage).
7. No sample has `status = 'error'`.
8. Trains torchvision ResNet-50 in-process on synthetic images (one timed pass, not two subprocess trainers). image_size, batch_size, warmup_iters, num_iterations, precision (fp16 default), and learning_rate come from yaml. dataset_name is ignored.

### Baseline / Threshold configuration (`config/benchmark_config.yaml`)

Expected ranges and gates live in `config/benchmark_config.yaml` under `baselines:` or `thresholds:`. To update them, edit that file — never edit validation code directly.

Threshold key suffixes encode comparison direction when `thresholds:` is present: `_min` → observed value must be ≥ threshold. `_max` → observed value must be ≤ threshold. Informational `baselines:` ranges are not pass/fail gates.
