# MultiLevel_Paging_Lab

A small C++ memory-translation simulator demonstrating a segmented + multi-level page table model with a TLB and configurable physical memory replacement (LRU or FIFO).

This repository contains a single example program, `memory_simulator_advanced.cpp`, which implements:

- A segment table mapping segments to directory tables
- Directory tables containing page tables
- Page tables with per-page metadata (frame number, present bit, protection, last access)
- A simple TLB with LRU eviction and hit-rate reporting
- Physical memory simulation with a configurable number of frames and LRU/FIFO replacement
- Batch and random address generation utilities and interactive mode

Features
--------
- Command-line flags to control frames, TLB size, page size, replacement policy, and batch input
- Interactive shell for adding/removing segments and translating addresses
- Logging for batch/random runs and a statistics display

Build
-----
Requires a C++11-capable compiler (g++ or clang++). From the repository root run:

```bash
g++ -std=c++11 memory_simulator_advanced.cpp -o memsim
```

Run (interactive)
-----------------
Start the simulator and use the interactive prompt:

```bash
./memsim
```

Interactive commands
--------------------
- `add <id> <base> <limit> <prot>` — add a segment (`prot` is `RO` or `RW`)
- `remove <id>` — remove a segment
- `translate <seg> <dir> <page> <offset> <access>` — perform a translation (`access` is `RO` or `RW`)
- `random <num>` — generate `<num>` random address translations and log results
- `stats` — display page-fault stats, average latency and TLB contents
- `quit` — exit

Command-line options
--------------------
- `--frames <n>` — number of physical frames (default 10)
- `--tlb <n>` — TLB capacity (default 4)
- `--pagesize <n>` — logical page size used in calculations (default 1000)
- `--replace <lru|fifo>` — replacement policy for physical memory (default `lru`)
- `--batch <file>` — run translations from a batch file (space-separated lines: `seg dir page offset access`) and log to `batch_results.txt`

Batch and random runs
---------------------
- `--batch <file>` reads translations from the named file and writes `batch_results.txt`.
- Use the `random` interactive command to produce `random_results.txt` for quick experiments.

Notes & Implementation details
------------------------------
- The program simulates faults and TLB behavior; many values are randomized initially to illustrate behavior.
- `DirectoryTable` currently uses raw pointers for `PageTable` objects; converting these to smart pointers would improve safety.
- `Segment` and `Page` structures hold protection and bookkeeping fields to model access control and LRU updates.
---

File: `memory_simulator_advanced.cpp`
Refer to the source for more details and inline comments.