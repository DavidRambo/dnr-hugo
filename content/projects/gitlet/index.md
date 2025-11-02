---
title: "Gitlet (Rust)"
date: "2025-11-01"
---

`Gitlet` is a simple git-like CLI, which I like to implement when learning a new language.
I initially learned about it through UC Berkeley's CS 61B course, which has assigned it in the past.
I implemented that original spec in Java, and then followed it up with a Python version (see [Gitlepy](../gitlepy)).

This time, I wanted to make it more like git by including ZLib compression and nested working trees.
I also added support for calling the gitlet binary from anywhere within a gitlet repository.
Two functions contain the bulk of the logic that makes this possible, both in the `repo` library module: `abs_path_to_repo_root` and `find_working_tree_dir`.
The former ensures that the gitlet command was called from a directory within a repository and then returns the absolute path to the root of the working tree.
The latter converts a file name or path to a file path that is relative to the repository's root.
Thus, the user can name files relative to their PWD, and the index will track files by their relative paths.
