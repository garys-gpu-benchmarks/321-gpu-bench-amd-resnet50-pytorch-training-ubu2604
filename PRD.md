# PRD.md:  "The Why"; Product requirements, benchmark metadata table, high-level requirements, etc.

Product Requirements Document

"The Why"; Product requirements, benchmark metadata table, high-level requirements, etc. Defines the benchmark goal, validation objective, test name, benchmark number, category, and high-level success criteria.

## Benchmark Matrix Document Metadata (via benchmark_specification.json)

This PRD.md section is populated from benchmark_specification.json, which is the structured source of benchmark-specific product requirements.

## Workload Number
321

## Workload Name
ResNet-50 FP16 Training Step (PyTorch)

## Execution Summary (Run and Measure)
Run an in-process PyTorch ResNet-50 FP16 trainer (torchvision.models.resnet50) on synthetic images with yaml image_size, batch_size, and num_iterations, then write step_time_ms, images_per_sec, and peak memory, to measure mixed-precision training efficiency. This is one inline pass, not two subprocess trainers

## Main Goal
Measure mixed-precision ResNet-50 training-step efficiency

## Validation Objective
Validates step_time_ms, images/s, and peak memory from an inline PyTorch trainer. This is not pytest

## Workload Category
Training, Inference, Model Workloads

## Validation Requirement

The benchmark must include an automated SQLite-integrated validation layer that verifies persisted results from `results/benchmark.db`. Validation must confirm:

1. The benchmark run completed successfully with no tool errors.
2. Required samples and aggregate metrics were persisted for every swept shape.
3. Metrics are finite and physically sensible (positive, within plausible bounds).
4. Measured values satisfy configured thresholds when the workload defines pass/fail gates.
5. The benchmark fails validation when required data is missing, invalid, or outside bounds.

## Non-Functional Requirements

| Requirement | Target |
|---|---|
| Automation | Runs to completion without manual intervention after `bash run_benchmark.sh` |
| Idempotency | Re-running `run_benchmark.sh` appends a new run; never corrupts existing rows |
| Persistence | All metrics survive script exit; `results/benchmark.db` is the durable record |
