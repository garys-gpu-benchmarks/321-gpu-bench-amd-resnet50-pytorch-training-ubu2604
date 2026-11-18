# Results: 321 ResNet-50 FP16 Training Step (PyTorch)

**Main goal:** Measure mixed-precision ResNet-50 training-step efficiency  
**Tools:** Run inline torchvision ResNet-50 training on synthetic tensors

Reference results measured by the maintainer on the system shown in each section (generator TEMPLATE_00_146 · recorded 2026-11-18). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× AMD Instinct MI300X VF (196288 MB) · INTEL(R) XEON(R) PLATINUM 8568Y+ · Ubuntu 26.04.1 LTS · kernel 7.0.0-38-generic · driver 7.1.9.31600000
- **Run:** 2026-11-18T11:18:17Z · status **PASS** (exit 0)
- **Time (mm:ss):** setup 01:15 · benchmark 00:01 · whole run 01:27
- **Source:** run 321_20261118_111817
- **Command:** `bash run_benchmark.sh --smoke --precision fp16 --image-size 64 --batch-size 2 --learning-rate 0.1 --warmup-iters 1 --num-iterations 3`

| # | Metric | Result |
|---|---|---|
| 1 | Training batch step time (step_time_ms) | 13.573918333198284 |
| 2 | Training throughput (images_per_sec) | 147.34139037130632 |
| 3 | Training throughput from counted FLOPs, TFLOP/s (counted_tflops) | 0.29392884545666276 |
| 4 | Peak PyTorch-allocated GPU memory (peak_gpu_memory_GB) | 0.280826368 |
| 5 | Modeled parameter and input traffic, GB/s (modeled_param_input_traffic_gb_s) | 15.0696766385954 |
<!-- /profile:smoke -->
