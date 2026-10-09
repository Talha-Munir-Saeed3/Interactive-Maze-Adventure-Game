<div align="center">

# 🧩 MAZE RUNNER CLI

### *Algorithmic Grid Navigation & Pathfinding Challenge in Modern C++*

[![C++](https://img.shields.io/badge/Language-C%2B%2B11%20%7C%20C%2B%2B14%20%7C%20C%2B%2B17-00599C?logo=c%2B%2B&logoColor=white)](#)
[![Algorithm](https://img.shields.io/badge/Algorithm-BFS%20Pathfinding-darkgreen)](#)
[![Data Structures](https://img.shields.io/badge/Data%20Structures-Graphs%20%7C%20Stacks%20%7C%20Queues-blueviolet)](#)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](#)

<p align="center">
  <b>A terminal-based maze puzzle engine featuring dynamic procedural obstacle generation, BFS-guaranteed path solvability, stack-based state rollback (undo), and algorithmic scoring.</b>
</p>

[Key Features](#-key-features) •
[Data Structures & Mechanics](#-core-data-structures--algorithms) •
[Game Controls](#-controls--legend) •
[Build & Run](#-getting-started) •
[Level Progression](#-level-progression--scoring)

---

</div>

## 📌 Overview

**Maze Runner CLI** is an interactive console game where the player navigates an $N \times N$ grid from the origin `(0, 0)` to the extraction target `(N-1, N-1)`. 

Unlike static maze games, every round generates random procedural barriers and uses an underlying **Adjacency List Graph + Breadth-First Search (BFS)** to guarantee a valid path exists before rendering. It tests user spatial awareness against the mathematically optimal path length computed by the solver.

---

## ✨ Key Features

- 🔄 **Guaranteed Solvability (BFS Verification):** Obstacles are placed stochastically, and a graph traversal ensures the maze is navigable ($minEdgeBFS > 0$) before handing control to the player.
- ⏪ **Move Reversal System (`std::stack`):** Track movement history with an undo mechanism, penalizing blind trial-and-error via life mechanics.
- 🎯 **Efficiency-Based Scoring Engine:** Compares player moves against the theoretical minimum BFS path distance and an obstacle-adjusted worst-case ceiling.
- 📈 **Dynamic Grid Scaling:** Maze dimensions ($8\times8$ to $12\times12$) and barrier densities ($15\%$ to $35\%$) scale across 5 difficulty levels.

---

## 🏗 Core Data Structures & Algorithms

<details open>
<summary><b>1. Adjacency List Graph & BFS Pathfinding</b></summary>
<br>

The grid cells $(i, j)$ are mapped directly to 1D graph vertices via:
$$\text{Node Index} = (i \times N) + j$$

- **Graph Structure:** `vector<int>* adj` stores valid orthogonal step transitions (North, South, East, West) excluding barriers (`'X'`).
- **BFS Traversal:** `minEdgeBFS(int u, int v)` uses a `std::queue<int>` and a visited vector to determine the shortest step count from start $(0)$ to target $(N^2 - 1)$. If unreachable ($sp = 0$), the maze regenerates automatically.

</details>

<details open>
<summary><b>2. Move Tracking & Backtracking via LIFO Stack</b></summary>
<br>

- Every valid movement pushes coordinate pairs `std::pair<int, int>` onto a `std::stack<pair<int,int>>`.
- Selecting **Undo (`'U'`)** pops the latest coordinate and restores player position.
- Retreading visited cells or stepping back directly impacts player lives ($lives = 3$).

</details>

---

## 🎮 Controls & Legend

### ⌨️ Keybindings

| Key | Action |
|:---:|:---|
| <kbd>W</kbd> | Move **UP** (Decrements $y$) |
| <kbd>A</kbd> | Move **LEFT** (Decrements $x$) |
| <kbd>S</kbd> | Move **DOWN** (Increments $y$) |
| <kbd>D</kbd> | Move **RIGHT** (Increments $x$) |
| <kbd>U</kbd> | **UNDO** last move (Pops coordinate stack, consumes 1 life) |
| <kbd>Q</kbd> | **QUIT** current run and show statistics |

### 🗺 Map Symbols

| Symbol | Description |
|:---:|:---|
| `*` | **Player Position** |
| `$` | **Extraction Gate / Target Goal** |
| `X` | **Impassable Wall / Obstacle** |
| `_` | **Traversable Corridor / Traversed Cell** |

---

## 📊 Level Progression & Scoring
