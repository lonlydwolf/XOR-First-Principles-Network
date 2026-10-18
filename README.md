# XOR First-Principles Network

A completed learning project implementing a small neural network directly in Python, C++, and C#, without an ML framework. The exercise explores forward propagation, backpropagation, and momentum updates on the four-case XOR problem.

The Python implementation is the reproducible reference for this closeout. C++ and C# remain historical ports with different initialization and training settings; this repository does not establish identical results across languages. There is no active feature roadmap.

Read the [technical retrospective](docs/RETROSPECTIVE.md) for the scope, checks, and limitations.

## Clone

The language implementations are separate repositories pinned as Git submodules. Public HTTPS URLs allow cloning without configuring GitHub SSH keys:

```sh
git clone --recurse-submodules https://github.com/lonlydwolf/XOR-First-Principles-Network.git
cd XOR-First-Principles-Network
```

For an existing checkout, initialize or refresh the submodules at the revisions recorded by this repository:

```sh
git submodule sync --recursive
git submodule update --init --recursive
```

## Run the Python reference

Use Python 3.12 or later; the example and tests use only the standard library. From this repository's root:

```sh
python3 python/examples/xor_demo.py --seed 0
python3 -m unittest discover -s python/tests -v
```

On Windows, use `py -3` in place of `python3` if that is how Python is installed.

The example reads the checked-in 4,000-example training sequence, seeds weight initialization, and prints predictions for `00`, `01`, `10`, and `11` after training. Classification uses a threshold of `0.5`. It does not rewrite the historical training-output file. Repeating a seed makes the experiment reproducible within the same runtime; exact floating-point values can vary across runtimes.

The tests check XOR classification at fixed seeds, compare a backpropagation update with finite-difference gradients, and exercise file paths from a different working directory. The GitHub Actions workflow runs the reference checks on Linux and Windows. These checks cover the Python implementation, not numerical equivalence with the other ports.

XOR contains only four possible input pairs. Demonstrating convergence on those pairs verifies this small exercise; it is not a benchmark of generalization to unseen tasks.

## Repository structure

- [`python/`](python): Python reference, deterministic example, and regression tests. See its [README](python/README.md) for the original trainer's file-output behavior.
- [`cpp/`](cpp): historical C++ implementation and Visual Studio project. The source compiled during review, but its full training run was not validated.
- [`csharp/`](csharp): historical C# implementation targeting .NET Core 3.1. Its expected input-data fixture is not included in the pinned tree; its run was not validated.

All implementations use `tanh` activation and a fixed `+1` bias at each non-input neuron. Biases are not learned. The Python reference has a `[2, 4, 1]` topology for the checked-in dataset.

## Limits and historical differences

- C++ uses different momentum and random-initialization settings from Python and C#. No cross-language parity test is included.
- The C++ smoothed-error accumulator starts uninitialized. Its logged error is not a validated metric, even if the source compiles.
- The C++ and C# error calculations need attention before expanding to multiple output neurons.
- The parent repository collects earlier implementations whose pinned history dates to 2020. This closeout makes the Python exercise easier to reproduce; it does not turn the project into a general-purpose neural-network library.

Changes to language code belong in the corresponding repository. This parent records the reviewed submodule revisions and provides the common entry point.
