*This project has been created as part of the 42 curriculum by alkonsta, fsalinas.*

# Push Swap

## Description

**Push Swap** is a C project focused on sorting algorithms, data structures, and algorithmic complexity. The goal is to sort a stack of unique integers in ascending order by generating an efficient sequence of operations using only two stacks: **A** (initially containing the input) and **B** (initially empty).

The project includes input parsing and validation, three sorting strategies with different operation-count complexities, and the bonus component. Valid instructions are `sa`, `sb`, `ss`, `pa`, `pb`, `ra`, `rb`, `rr`, `rra`, `rrb`, and `rrr`.

## Instructions

### Compilation

From the repository root, run:

```sh
make
```

The standard Makefile targets can be used for cleaning and rebuilding:

```sh
make clean
make fclean
make re
```

The bonus implementation is included. If your Makefile provides a separate bonus target, compile it with `make bonus`.

### Execution

Pass a sequence of distinct integers to `push_swap`:

```sh
./push_swap 4 2 5 1 3
```

The program outputs the stack operations needed to sort the input. To count them:

```sh
ARG="4 2 5 1 3"
./push_swap $ARG | wc -l
```

If the bonus checker is built as `checker`, you can validate the generated operations with:

```sh
ARG="4 2 5 1 3"
./push_swap $ARG | ./checker $ARG
```

Invalid input, including duplicate or non-integer values, is rejected during argument validation.

## Algorithm Selection and Design

The project implements three approaches to compare sorting strategies under the restricted Push Swap instruction set.

| Strategy | Algorithm | Stack-operation complexity |
| --- | --- | --- |
| **Simple** | Selection Sort for two stacks | **O(n²)** |
| **Medium** | Chunk-based sorting | **O(n√n)** |
| **Complex** | Binary LSD radix sort | **O(n log n)** |

### Ranking (shared preprocessing)

Each distinct input value receives a rank from `0` to `n - 1`, preserving its relative numerical order. Rankings make it possible to work with arbitrary positive and negative integers without relying on their magnitudes. In this implementation, assigning ranks by pairwise comparison takes **O(n²) CPU time** but emits no Push Swap operations.

### Simple — Selection Sort Adapted to Two Stacks

An adaptation of Selection Sort for two stacks. The algorithm locates the next smallest ranked element and brings it to the top of stack A using rotations (`ra` or `rra`). It then pushes the element to stack B and repeats the process. Finally, all elements are pushed back to A in sorted order.

**Why this approach:** Simple, predictable, and easy to implement and debug. Uses bidirectional rotations to reduce unnecessary operations while maintaining **O(n²)** complexity.

### Medium — Chunk-Based Sorting

The ranked input is divided into groups of approximately `√n` elements. The algorithm sends the chunks from A to B in increasing rank ranges, keeping the ranges grouped even though values within each chunk are not yet sorted. It then processes the chunks in reverse order: the maximum remaining element of the top chunk is brought to the top of B and pushed to A, restoring rotations as needed to preserve the remaining chunk structure.

**Why this approach:** grouping limits repeated searches to smaller regions and provides a middle ground between implementation simplicity and operation efficiency. With chunk sizes proportional to `√n`, the designed stack-operation cost is **O(n√n)**.

### Complex — Binary LSD Radix Sort

The algorithm examines the bits of each rank, starting at the least significant bit. On each pass, a `0` bit sends the element to B (`pb`), while a `1` bit rotates it within A (`ra`). Pushing all elements from B back to A (`pa`) completes the stable partition for that bit. This process repeats only for the number of bits required by the maximum rank.

**Why this approach:** it scales predictably to larger inputs and uses a small, repeatable set of stack operations. Processing **O(log n)** bits with **O(n)** operations per pass results in **O(n log n)** stack operations.

> **Complexity note:** the stated complexities measure generated Push Swap operations. The current ranking preprocessing takes O(n²) CPU time and is not included in those operation counts.

## Bonus

The bonus component is included in the project and was developed collaboratively. It supports checking the validity of a sequence of Push Swap operations and the resulting stack state. We used the functions we created in previous project as Libft, ft_printf and Get Next Line

## Team Contributions

| Contributor | Responsibilities |
| --- | --- |
| **alkonsta** | Parser, input handling, output handling, bonus |
| **fsalinas** | Sorting algorithms, README, code formatting, bonus |

## Resources

- [Push Swap Visualizer — o-reo](https://github.com/o-reo/push_swap_visualizer) — visualizing and inspecting stack operations.
- [GeeksforGeeks — Sorting Algorithms](https://www.geeksforgeeks.org/dsa/sorting-algorithms/) — comparison of sorting methods and complexity.
- [GeeksforGeeks — Radix Sort](https://www.geeksforgeeks.org/dsa/radix-sort/) — LSD radix sort principles.
- [cppreference — C language](https://en.cppreference.com/w/c) — C language documentation and operators.
- [VisuAlgo — Sorting](https://visualgo.net/en/sorting) — interactive algorithm demonstrations.
- [Harvard CS50 — Lecture 3: Algorithms](https://cs50.harvard.edu/x/2025/weeks/3/) — video explanations of sorting algorithms and asymptotic complexity.
- [GNU Make Manual](https://www.gnu.org/software/make/manual/make.html) — Makefile and build-system reference.
- Additional educational videos explaining sorting algorithms and their trade-offs.
- [Push Swap Tester](https://github.com/SimonCROS/push_swap_tester/tree/v2) — One of the seven tests we used to test our program.

### AI Usage

AI was used as a **learning and debugging aid**: to clarify C and algorithmic concepts, discuss the suitability of different sorting strategies, review logic and edge cases, and help investigate implementation errors. AI also assisted with the organization and wording of this README. 
The algorithm selection, integration, and final validation remained the responsibility of the project contributors.
