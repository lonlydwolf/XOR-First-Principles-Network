# A reproducible neural-network fundamentals exercise

This project contains direct implementations of a small fully connected network in Python, C++, and C#. It preserves an earlier learning exercise and makes each implementation easy to inspect and run. It does not claim production readiness or measured improvements over an ML library.

## What the implementation exposes

The neuron and network classes make the calculations visible: weighted sums, `tanh` activation, output and hidden-layer gradients, and updates with momentum. The XOR examples use two inputs, four hidden neurons, and one output.

For Python's single-output gradient check, the loss is `L = 0.5 * (prediction - target)^2`. The derivative of `tanh` is `1 - output^2`. On the initial update, momentum contributes zero, so a weight's update should equal the negative numerical loss gradient multiplied by the learning rate. Comparing every weight against a finite-difference estimate checks forward propagation and backpropagation without using the same analytic formula as the test oracle.

C# extends this check to a two-output test network over two updates, summing half-squared errors and including momentum. C++ checks the corrected error statistic and convergence without claiming numerical-gradient coverage.

The fixed `+1` neuron bias is a simplification. There are no trainable bias parameters. The four XOR inputs are both the task and the evaluation cases, so successful classification is evidence of learning this truth table, not broader generalization.

## What the closeout repairs

The historical code had useful behavior behind unreliable entry points. Windows-specific paths prevented ordinary Python startup on macOS. The parent clone command contained Markdown link syntax, while SSH submodule URLs imposed an authentication prerequisite for public source. The Python closeout repaired paths and added a seeded example that leaves tracked training logs alone.

The subsequent C++ and C# closeouts make both ports runnable as well. C++ initializes numerical state, accumulates squared error across all outputs, validates dimensions, and provides a standalone seeded example. C# supplies the missing input fixture, moves from .NET Core 3.1 to .NET 10, uses one seeded random generator, repairs error aggregation, and validates data independently of locale. Report-file output is opt-in for the new examples.

All three retain their original activation and weight-update equations. The default training schedules are explicit, and the examples require no external ML packages. The parent pins the merged closeout revisions and runs the implementations' checks in CI.

## Why the language ports are not called equivalent

The ports explore the same general algorithm, but they do not use a shared random generator, initial-weight fixture, momentum, or training budget. Python reads 4,000 checked-in examples; C++ repeats the truth table for 4,000 updates; C# repeats it for 40,000 updates. C++ retains momentum 0.5 and nonnegative initial weights, while Python and C# use momentum 0.001 and weights around zero.

Describing their numerical results as identical would require controlled inputs, matched settings, and comparison within an explicit tolerance. The closeout verifies each example on its own terms. It does not establish cross-language parity or turn the historical C++ Visual Studio project into the supported demo build.

## What the evidence supports

A reviewer can run all three examples, inspect their four predictions, and run focused tests. Python checks numerical gradients, convergence, and portable paths. C++ checks error initialization/aggregation, dimension handling, and convergence. C# checks gradients and momentum, convergence, error aggregation, and fixture/path handling. CI exercises the pinned code using the platforms documented in the README.

These suites do not establish correctness for arbitrary topologies, new datasets, or every seed. Their value is a compact, inspectable demonstration of the algorithm and its limits. Further feature development is not planned.
