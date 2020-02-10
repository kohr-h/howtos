# How to Build Halide on Linux

This document describes how to build the [Halide language](http://halide-lang.org/) and its Python bindings.

**Note:** A prebuilt binary package is available as `halide-python` on `conda-forge`.

## Steps

0. Make sure that LLVM and Clang >= 6.0 are installed on your system.
1. Start a shell where no Conda environment is activated (within an env, building works fine but tests fail, so the library may not work).
2. Run `make` in the project root, here assumed to be `~/git/halide`.
3. Run `make distrib HALIDE_DISTRIB_PATH=~/git/halide/distrib` to generate a distribution package.
4. Get [`pybind11`](https://github.com/pybind/pybind11), here assumed to be in `~/git/pybind11`.
5. Enter the Python bindings directory, `cd python_bindings`, and activate a conda environment, e.g., `conda activate halide_src`.
6. Run `make distrib HALIDE_DISTRIB_PATH=~/git/halide/distrib PYBIND11_PATH=~/git/pybind11`. This builds the Python C extension module in the `bin/` subdirectory.
7. Put `/home/user/git/halide/python_bindings/bin` on the `PYTHONPATH` when running scripts or applications. Alternatively, run `import sys; sys.path.append("/home/user/git/halide/python_bindings/bin")` before the `halide` import statement.

   **Note:** Paths must be absolute and may not contain "~", otherwise they are not accepted.

## Resources

- [Building Halide](https://github.com/halide/Halide#building-halide)
- [Building Halide Python Bindings](https://github.com/halide/Halide/tree/master/python_bindings#prerequisites)
