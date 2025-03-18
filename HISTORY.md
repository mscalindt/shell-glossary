# History of the shell-glossary Repository

This document outlines the major changes and milestones in the evolution of the
`shell-glossary` repository. In this document, AI has been (consciously) used
to summarize the changes.

## Table of Contents

1. [Reconstruction of Repository Structure - 2023-08-15](#1-reconstruction-of-repository-structure---2023-08-15)
2. [Merge of main-functions Repository - 2024-07-14](#2-merge-of-main-functions-repository---2024-07-14)

## 1. Reconstruction of Repository Structure - 2023-08-15

### Background:

Originally, the `shell-glossary` repository was organized as a singular
`README.md` file. This file contained a collection of pure POSIX `sh`
functions, each detailed with its description, parameters, and code. While this
format was concise, it presented challenges:

1. **Git History Clarity**: As more functions were added and modified, tracking
changes to individual functions within a large `README.md` became less
intuitive. Git's inherent nature to track file-based changes meant that the
history of a specific function was muddled amidst changes to others.

2. **Repository Structure**: The monolithic structure was less modular and made
the repository less self-contained. Storing functions within the README, while
convenient for quick reference, did not align with common practices of storing
functional code.

### Changes Made:

To address these concerns and to improve the repository's organization, the
following restructuring steps were taken:

1. **Individual Function Files**: Each function was moved to its dedicated file
within the `src` directory. This change promotes modularity and allows users
and contributors to focus on individual functions more efficiently.
Furthermore, it ensures that the Git history for each function remains clear
and logical going forward.

2. **Updated README**: The `README.md` was revamped to link to each individual
function file in the `src` directory. This ensures that the repository remains
user-friendly, with the README serving as an index to all available functions.

3. **Git Tagging**: To help users and contributors reference the previous
structure, a Git tag named [`pre-split`](https://github.com/mscalindt/shell-glossary/releases/tag/pre-split)
was created. This tag points to the last commit where all functions were
contained within the `README.md`.

### Rationale:

The primary motivation behind this restructuring was twofold:

1. **Enhance Git History**: By having dedicated files for each function, future
changes to a function can be tracked in a granular manner, making the Git
history more logical and meaningful.

2. **Improve Repository Organization**: Shifting to a more conventional
directory structure makes the repository more self-contained and aligns it
closer to standard coding practices.

## 2. Merge of main-functions Repository - 2024-07-14

### Background:

The repository [`main-functions`](https://github.com/mscalindt/main-functions)
has been merged into `shell-glossary`.

### Changes Made:

1. **Extended Function Collection**: `main-functions` added specialized
functions for shell option parsing to `shell-glossary`, extending its value as
a glossary of functions.

2. **Updated README**: To ease referencing, `README.md` was revamped to
separate the functions of `shell-glossary` and `main-functions` into respective
collections.

3. **Git Tagging**: To help users and contributors reference the previous
structure, a Git tag named [`pre-merge`](https://github.com/mscalindt/shell-glossary/releases/tag/pre-merge)
was created. This tag points to the last commit before the merge of
`main-functions`.
