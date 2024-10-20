# COMS30005-High-Performance-Computing

*All work done in C programming language*

### Coursework 1 - Serial Optimisation :

Details of the optimisations used are in [COMS30005-HPC-Serial/report.pdf)[https://github.com/kheeran/COMS30005-HPC-Serial/blob/759db167a6650709750fa9b4d79f851a1cb14661/report.pdf]


### Coursework 2 - Parallel Optimisation :

*Follows directly from the serially optimised code from the first coursework*

Optimisation details are in the file (COMS30005-HPC-Parallel/report.pdf)[https://github.com/kheeran/COMS30005-HPC-Parallel/blob/9c10ab8efaf2083b448f35a09025bf8ebf5abf39/report.pdf]

----------------------------------------------------------------------------------------------------------------
QUESTION:

# 5-point stencil

This code implements a weighted 5-point stencil on a rectangular grid.
The value in each cell of the grid is updated based on an average of the values in the neighbouring North, South, East and West cells.

The grid is initialised into a 8x8 checkerboard pattern.
The stencil operation reads from one grid and writes to a temporary grid.
The stencil is run twice for every iteration, with the final result being held in the original array.
The results are quantised to integers in the range [0,255] and output as a binary image file that may be viewed graphically.

The only output of each run is the runtime of the iteration loop of the program.
Initialisation and output are not timed.

## Building and running the code

A simple `Makefile` is provided to build the code using the GCC compiler.
Just type `make` to build the code.
A binary named `stencil` is produced on a successful build.

A job script for BlueCrystal Phase 3 is also provided in `stencil.job`.
This will request time on 1 node of the `teaching` queue and execute the `stencil` binary on one of the input problems.
Submit this to the queue with the following command:

    qsub stencil.job


There are *three* input problems tested, representing different grid sizes.
The inputs are all set as command line arguments to the executable in the following format:

    ./stencil nx ny niters

The inputs required are:

| nx   | ny   | niters | Command                   |
| ---- | ---- | ------ | ------------------------- |
| 1024 | 1024 | 100    | `./stencil 1024 1024 100` |
| 4096 | 4096 | 100    | `./stencil 4096 4096 100` |
| 8000 | 8000 | 100    | `./stencil 8000 8000 100` |

## Checking results

The program will have executed correctly if the output image matches the provided reference output images with a small tolerance of +/- 1.
A Python check script is provided to check the values.
Python must first be loaded on BlueCrystal Phase 3, and the script can be run as follows:

    module load languages/python-2.7.6
    python check.py --ref-stencil-file stencil_1024_1024_100.pgm --stencil-file stencil.pgm

If any errors are found, the script can be rerun with the addition of the `--verbose` flag to provide information about where the errors occur in the grid.

The reference input files for the different problems are named:

| nx   | ny   | niters | Reference file              |
| ---- | ---- | ------ | --------------------------- |
| 1024 | 1024 | 100    | `stencil_1024_1024_100.pgm` |
| 4096 | 4096 | 100    | `stencil_4096_4096_100.pgm` |
| 8000 | 8000 | 100    | `stencil_8000_8000_100.pgm` |
