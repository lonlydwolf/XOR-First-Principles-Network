# XOR First-Principles Network

A completed learning project implementing a small neural network directly in Python, C++, and C#, without an ML framework. The exercise explores forward propagation, backpropagation, and momentum updates on the four-case XOR problem.

Each implementation now includes a runnable seeded example and focused regression checks. The languages retain different initialization and training settings; this repository does not establish identical numerical results across them. There is no active feature roadmap.

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

All commands below run from this parent repository's root. Install only the toolchain for the implementation you want to explore. None of the examples requires a model download or an external ML package.

## Python

Use Python 3.12 or later:

```sh
python3 python/examples/xor_demo.py --seed 0
python3 -m unittest discover -s python/tests -v
```

On Windows, use `py -3` in place of `python3` if that is how Python is installed. The example trains on the checked-in 4,000-example sequence and leaves historical output files unchanged. Tests cover numerical gradients, convergence for seeds 0–4, and portable paths. See the [Python README](python/README.md) for the original trainer's file-output behavior.

## C++

Use a C++17 compiler such as Clang or GCC. These commands use a POSIX shell:

```sh
mkdir -p cpp/build
c++ -std=c++17 -Wall -Wextra -pedantic -I cpp/NN cpp/NN/Network.cpp cpp/NN/Neuron.cpp cpp/examples/xor_demo.cpp -o cpp/build/xor_demo
c++ -std=c++17 -Wall -Wextra -pedantic -I cpp/NN cpp/NN/Network.cpp cpp/NN/Neuron.cpp cpp/tests/network_tests.cpp -o cpp/build/network_tests
./cpp/build/xor_demo --seed 0
./cpp/build/network_tests
```

The example trains on 4,000 samples by repeating the four XOR cases and writes no files. Tests cover initial and multi-output error statistics, dimension validation, and convergence for seeds 0–4. The standalone example and tests are separate from the historical Visual Studio project. See the [C++ README](cpp/README.md).

## C#

Use a .NET 10 SDK:

```sh
dotnet build csharp/NN.csproj --configuration Release
dotnet run --project csharp/NN.csproj --configuration Release --no-build
dotnet run --project csharp/NN.csproj --configuration Release --no-build -- --test
```

The bundled fixture is copied beside the executable. By default, seed 42 initializes weights and 10,000 epochs traverse the four cases, for 40,000 updates. The example writes no report file unless requested. Tests cover convergence for seeds 42, 7 and 123, numerical gradients and momentum, multi-output error, and data/path validation. See the [C# README](csharp/README.md) for arguments and the text-data format.

## Shared concepts and different settings

All three examples use a `2 → 4 → 1` topology, `tanh` activation, and a fixed `+1` bias at each non-input neuron. Biases are not learned. All use learning rate `0.2` and evaluate the four XOR inputs with a classification threshold of `0.5`.

| Implementation | Initial weights | Momentum | Default training |
| --- | --- | --- | --- |
| Python | Uniform between -0.5 and 0.5 | 0.001 | 4,000 checked-in examples |
| C++ | C `rand` scaled to [0, 1] | 0.5 | 4,000 cyclic truth-table samples |
| C# | `Random` scaled to [-0.5, 0.5) | 0.001 | 40,000 cyclic truth-table samples |

Seeds repeat initialization within the same runtime; random generators and floating-point results can differ across runtimes. The examples do not use a common initial-weight fixture or matched training budget, so their outputs are not a controlled language comparison.

The parent CI checks the pinned implementations: Python on Linux/Windows, C++ on Linux/macOS, and C# on Linux. XOR contains only four possible input pairs; fitting those cases is a convergence check, not a benchmark of generalization to unseen tasks.

## Scope and history

The closeout fixes the C++ uninitialized error accumulator and both ports' multi-output error aggregation. C# now includes its data fixture and targets .NET 10. The earlier implementations date to 2020; the 2026 closeout adds reproducible entry points and bounded checks while retaining their original activation and weight-update equations.

This remains a small learning exercise, not a general-purpose neural-network library. C++ does not have a comprehensive numerical-gradient test, and none of the suites establishes correctness for arbitrary topologies, datasets, or seeds. The historical C++ Visual Studio entry point and training traces remain reference artifacts.

Changes to language code belong in the corresponding repository. This parent records the reviewed submodule revisions and provides the common entry point. Earlier pinned revisions predate these repairs; the child READMEs' historical-parent notes describe that earlier state.
