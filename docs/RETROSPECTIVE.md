# A reproducible neural-network fundamentals exercise

This project contains direct implementations of a small fully connected network in Python, C++, and C#. It preserves an earlier learning exercise and makes one reference implementation easy to inspect and run. It does not claim production readiness or measured improvements over an ML library.

## What the implementation exposes

The neuron and network classes make the calculations visible: weighted sums, `tanh` activation, output and hidden-layer gradients, and updates with momentum. The checked-in Python dataset specifies two inputs, four hidden neurons, and one output.

For the single-output reference, the gradient check uses the loss `L = 0.5 * (prediction - target)^2`. The derivative of `tanh` is `1 - output^2`. On the initial update, momentum contributes zero, so a weight's update should equal the negative numerical loss gradient multiplied by the learning rate. Comparing every weight against a finite-difference estimate checks the interaction of forward propagation and backpropagation without reimplementing the same analytic formula as the test oracle.

The fixed `+1` neuron bias is a simplification of this implementation. There are no trainable bias parameters. The four XOR inputs are both the task and the evaluation cases, so successful classification is evidence of learning this truth table, not broader generalization.

## Why the closeout focuses on Python

The historical code had useful behavior behind an unreliable entry point. Windows-specific path concatenation prevented ordinary Python startup on macOS. The parent clone command also contained Markdown link syntax, while SSH submodule URLs imposed an unnecessary authentication prerequisite for public source.

The closeout repairs those entry points, adds a seeded example that leaves tracked training logs alone, and adds regression checks for convergence, numerical gradients, and paths. The neural-network algorithm remains the historical implementation. The example and tests require no model download or third-party Python packages.

## Why the language ports are not called equivalent

The ports explore the same general algorithm, but they do not use a shared initial-weight fixture or the same momentum. They also have different execution prerequisites. Describing their results as identical would require controlled inputs, matched settings, and comparison within an explicit numerical tolerance.

C++ source compilation was checked in the October 2026 review; the full executable was not run. Its uninitialized smoothed-error accumulator remains a known limitation. C# was not executed, and its expected data fixture is absent. Those implementations remain historical reference material rather than newly validated examples.

## What the evidence supports

A reviewer can now run the Python exercise, inspect all four predictions, and run tests that would detect meaningful regressions in weight updates and classification. The test suite is intentionally small and tied to the exercise's claims. It does not establish that arbitrary network topologies, new datasets, or the other language ports behave correctly.

The broader lesson is that a compact fundamentals project benefits from explicit assumptions and reproducible results. Its value comes from making the algorithm and its limits understandable. Further feature development is not planned.
