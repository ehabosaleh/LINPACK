# LINPACK (Cache-Aware Variant)
This project is a variant from  the classic LINPACK benchmark obtained from Netlib: https://www.netlib.org/benchmark/  Original authors: Jack Dongarra et al.

The program measures floating-point performance of LU factorization and reports GFLOPS.

This version introduces **cache-aware matrix sizing** to better control the working set relative to the CPU's last-level cache (LLC).

---

## Features

- Classic LINPACK LU decomposition benchmark
- Double precision computation
- Automatic iteration scaling (runs until ~10 seconds)
- Cache-aware matrix sizing using detected LLC size
- Command-line interface for reproducible experiments

## LINPACK CLI Usage

Available options:

| Option | Description |
|------|-------------|
| `--array-size=N` | Set matrix dimension to `N × N` |
| `--llc-scale=F` | Compute matrix size as `F × LLC` |
| `--iters=N` | Run a fixed number of repetitions |
| `--help` | Display usage information |


The benchmark supports two methods for selecting the matrix size.

1. Manual Matrix Size:
You can explicitly set the matrix dimension.
```
./linpack --array-size=2000

```
 2. Cache-Aware Matrix Size:
The benchmark can automatically compute the matrix size relative to the CPU's last-level cache (LLC).

```
./linpack --llc-scale=4 // the matrix size is 4 times the cache size
```
The application **ignores `--array-size`** and computes the matrix size from `--llc-scale`.
```
./linpack --array-size=100 --llc-scale=4 
```

## Matrix Dimension

The original LINPACK benchmark performs factorization on a matrix of size `arsize / 2`, even though memory is allocated for an `arsize × arsize` matrix.

This implementation uses the full matrix size: `n = arsize`. This change simplifies the memory model and increases the computational workload, which is more appropriate for modern systems with large memory capacity.

