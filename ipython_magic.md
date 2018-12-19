# Magic Commands in IPython

## Built-in commands

### Generic

|                         |                                                              |
| ----------------------- | ------------------------------------------------------------ |
| Command                 | Effect                                                       |
| `%<statement>`          | Run `<statement>` in “line mode”, i.e., a single statement   |
| `%%<command>`           | Run `<command>` in “cell mode”, i.e., multiple lines of code |
| `%load_ext <extension>` | Load an extension (usually a Python module)                  |
| `%<command>?`           | Get help for `<command>`                                     |

### Timing

|                              |                                                                    |
| ---------------------------- | ------------------------------------------------------------------ |
| Command                      | Effect                                                             |
| `%timeit <statement>`        | Measure runtime of `<statement>` using an average of multiple runs |
| `%prun <statement>`          | Profile all function calls in `<statement>`                        |
| `%prun ... -D outfile.cprof` | Dump profile results to a pickle file (load with `pstats.Stats`)   |
|                              |                                                                    |

## Extensions

|                 |                 |                                                |                                                                                                     |
| --------------- | --------------- | ---------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| Extension       | Module          | Functionality                                  | More info                                                                                           |
| `line_profiler` | `line_profiler` | Profile scripts, functions, etc. per code line |                                                                                                     |
| `cython`        | `cython`        | Define and compile Cython snippets (cell mode) | <http://docs.cython.org/en/latest/src/reference/compilation.html#compiling-with-a-jupyter-notebook> |
|                 |                 |                                                |                                                                                                     |

### `line_profiler`

|                                                   |                                                             |
| ------------------------------------------------- | ----------------------------------------------------------- |
| Command                                           | Effect                                                      |
| `%lprun -f <function> -f <function2> <statement>` | Run `<statement>` and profile functions after `-f`          |
| `%lprun -m <module> <statement>`                  | Run `<statement>` and profile all functions in `<module>`   |
| `%lprun ... -D outfile.lprof`                     | Dump profile results to a pickle file                       |
| `%lprun ... -T summary.txt`                       | Write profile results with code side by side to a text file |
|                                                   |                                                             |
