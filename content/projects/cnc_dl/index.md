---
title: "Downloading old podcast episodes with Rust"
date: 2025-11-16
---

[Repository](https://github.com/DavidRambo/crateandcrowbar-dl)

This little Rust binary crate downloads episodes of _The Crate and Crowbar_ podcast from the download links provided on [their website](https://www.crateandcrowbar.com).
It uses `reqwest::blocking` to make blocking requests and `std::threads` to spawn threads so that a multicore system can possibly run some of the downloads in parallel.
I go into more detail in the repository about how the URLs are formatted in various ways to account for the different naming conventions and two different web servers hosting the episodes.
