# experiments/ — GPU variants (PyTorch/CuPy)

GPU versions of the deep-dive calculation. They ran correctly but gave **no speedup** compared to the CPU. The final results were obtained on the CPU (see `src/`).

- `simulation_4conn_CUDA.ipynb` — 4-connectivity on CuPy.
- `simulation_yukawa_GPU.ipynb` — Yukawa on GPU (PyTorch/CuPy).

## Result (an honest negative result)

The GPU variants were **not faster** than the CPU, so the main repository uses the CPU scripts. This is documented deliberately: a negative result is also useful.

## Why the GPU did not help (analysis)

1. **The Metropolis algorithm is inherently sequential.** Every occupied↔empty swap depends on the previous state and on the accept/reject decision. The main loop cannot be parallelised — the GPU has nothing to run in parallel.
2. **The 100×100 lattice is too small.** The overhead of launching GPU kernels and transferring data between CPU and GPU exceeds the time of the computation itself.
3. The only fast GPU part here is the convolution (`F.conv2d`), and it is not the bottleneck.

## Conclusion

For sequential Monte Carlo on a small lattice, the GPU is the wrong tool. The GPU will become useful for the **3D version** (a 100³ lattice) with vectorised updates.

---

## Русская версия

# experiments/ — GPU-варианты (PyTorch/CuPy)

Здесь лежат GPU-версии глубокого расчёта. Они запускались, работали корректно, но **не дали ускорения** по сравнению с CPU. Финальные результаты получены на CPU (см. `src/`).

- `simulation_4conn_CUDA.ipynb` — 4-связность на CuPy.
- `simulation_yukawa_GPU.ipynb` — Юкава на GPU (PyTorch/CuPy).

**Итог (честный отрицательный результат):** GPU-версии считали **не быстрее** CPU, поэтому основной репозиторий использует CPU-скрипты.

**Почему GPU не помог:** (1) алгоритм Метрополиса последовательный — основной цикл не распараллеливается; (2) решётка 100×100 слишком мала — накладные расходы превышают время вычисления; (3) быстро на GPU только свёртка `F.conv2d`, но она не узкое место.

**Вывод:** для последовательного Монте-Карло на маленькой решётке GPU — неподходящий инструмент; GPU пригодится в 3D-версии (100³).
