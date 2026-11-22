# Results: 321 ResNet-50 FP16 Training Step (PyTorch)

**Main goal:** Measure mixed-precision ResNet-50 training-step efficiency  
**Tools:** Run inline torchvision ResNet-50 training on synthetic tensors

Reference results from the validation run on the system shown in each section (recorded 2026-11-22). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× AMD Instinct MI300X VF (196288 MB) · INTEL(R) XEON(R) PLATINUM 8568Y+ · Ubuntu 26.04.1 LTS · kernel 7.0.0-43-generic · driver 7.1.9.31600000
- **Run:** 2026-11-22T11:12:24Z · status **PASS** (exit 0)
- **Time (mm:ss):** setup 01:11 · benchmark 00:01 · whole run 01:22
- **Source:** run 321_20261122_111224
- **Command:** `bash run_benchmark.sh --smoke --precision fp16 --image-size 64 --batch-size 2 --learning-rate 0.1 --warmup-iters 1 --num-iterations 3`

| # | Metric | Result |
|---|---|---|
| 1 | Training batch step time (step_time_ms) | 12.57512899996982 |
| 2 | Training throughput (images_per_sec) | 159.0440941007285 |
| 3 | Training throughput from counted FLOPs, TFLOP/s (counted_tflops) | 0.31727437102311834 |
| 4 | Peak PyTorch-allocated GPU memory (peak_gpu_memory_GB) | 0.280826368 |
| 5 | Modeled parameter and input traffic, GB/s (modeled_param_input_traffic_gb_s) | 16.26659734468656 |
<!-- /profile:smoke -->
