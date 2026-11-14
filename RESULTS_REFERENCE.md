# Results: 321 ResNet-50 FP16 Training Step (PyTorch)

**Main goal:** Measure mixed-precision ResNet-50 training-step efficiency  
**Tools:** Run inline torchvision ResNet-50 training on synthetic tensors

Reference results measured by the maintainer on the system shown in each section (generator TEMPLATE_00_141 · recorded 2026-11-14). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× AMD Instinct MI300X VF (196288 MB) · INTEL(R) XEON(R) PLATINUM 8568Y+ · Ubuntu 26.04.1 LTS · kernel 7.0.0-38-generic · driver 7.1.9.31600000
- **Run:** 2026-11-14T11:18:46Z · status **PASS** (exit 0)
- **Time (mm:ss):** setup 01:17 · benchmark 00:12 · whole run 01:39
- **Source:** run 321_20261114_111846
- **Command:** `bash run_benchmark.sh --smoke --precision fp16 --image-size 64 --batch-size 2 --learning-rate 0.1 --warmup-iters 1 --num-iterations 3`

| # | Metric | Result |
|---|---|---|
| 1 | Training batch step time (step_time_ms) | 13.339141333290172 |
| 2 | Training throughput (images_per_sec) | 149.93468845019646 |
| 3 | Modeled training throughput, 3x forward FLOPs, TFLOP/s (modeled_tflops) | 0.2790621140134269 |
| 4 | Peak GPU memory usage (peak_gpu_memory_GB) | 0.280826368 |
| 5 | Modeled parameter and input traffic, GB/s (modeled_param_input_traffic_gb_s) | 15.334912112333507 |
<!-- /profile:smoke -->
