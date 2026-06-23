# XOR First-Principles Network

A fully connected neural network compiled without third-party frameworks to cement absolute mathematical understanding of algorithmic updates. Hand-coded across C++, C#, and Python yielding identical results.

This repository serves as a multi-language parent container linking the independent language implementations via Git submodules.

## Repository Structure

* **`cpp/`** — C++ implementation (`Cpp-Neural-Network`)
* **`csharp/`** — C# implementation (`C-Sharp-Neural-Network`)
* **`python/`** — Python implementation (`Python-Neural-Network`)

---

## How to Clone

Because this repository utilizes Git submodules, standard cloning will leave the language directories empty. Use the `--recurse-submodules` flag to fetch the parent repository and all sub-repositories simultaneously:

```bash
git clone --recurse-submodules [https://github.com/lonlydwolf/xor-first-principles-network.git](https://github.com/lonlydwolf/xor-first-principles-network.git)