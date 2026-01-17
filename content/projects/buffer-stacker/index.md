---
title: "Buffer Stacker (neovim plugin)"
date: "2026-01-16"
---

[Buffer Stacker](https://github.com/davidrambo/buffer-stacker.nvim) is a [neovim](https://neovim.io/) plugin written in Lua that orders buffers in a cycle and supports navigating that cycle.
It provides commands to switch to the next and previous buffers in the cycle, which maintains the ordering.
When opening a file or picking a buffer out of order, however, that newly entered buffer is placed in the next position relative to the previously open buffer.
The cycle of buffers can be printed as well as viewed using a [telescope](https://github.com/nvim-telescope/telescope.nvim/) picker.
For me, the most desired behavior was to be able to use the previous buffer command `:bprevious` to return to the buffer I had just left, as if the most recently visited buffer were placed on top of a stack.
Vim and neovim, by contrast, sort open buffers according to their buffer numbers.

Buffer Stacker maintains a table of buffer numbers mapped to their nodes in this cycle.
It also keeps a reference to the currently open buffer number.
Together, the hash map and reference form a stack represented by a circular linked list.
(See `lua/buffer-stacker/init.lua`)
For example, they might look like this:

```lua
links = {
    1 = { next = 18, prev = 4 },
    4 = { next = 1, prev = 18 },
    18 = { next = 4, prev = 1 }
}
current_bufnr = 1
```

When navigating the cycle of buffers, the next `bufnr` is accessed via `links[current_bufnr].next` (likewise for previous), `current_bufnr` is updated, and a flag variable local to the plugin is set in order to prevent the reordering of the cycle.
The neovim api provides functions for getting information about a buffer from its `bufnr` as well as entering a buffer.

Two autocommands handle updating the stack cycle (see `plugin/buffer-stacker.lua`).
One, triggered by the `BufEnter` event, places the now current buffer at the top of the stack.
The second, triggered by the `BufDelete` event, removes a buffer's number from the cycle.
