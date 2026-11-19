---
title: "Unit 3 Lab 4 Addendum - 3D Statue (.obj) Benchmarks & Exhibition Guidelines"
layout: page
nav_order: 17
parent: 2026csa
math: mathjax
---

# Unit 3 Lab 4 Addendum: 3D Statue (`.obj`) Benchmarks & Multi-Team Exhibition Guidelines
{: .no_toc }

## Overview
{: .no_toc }
This guide summarizes our stress-testing benchmarks (`20 rooms × 10 statues = 200 statues`), explains the internal limits of Processing's 3D `.obj` loader (`PShapeOBJ` & `PShapeOpenGL`), and provides practical `.obj` polygon budget and lazy-loading guidelines for hosting a **10-team integrated Virtual Gallery Exhibition** with open, see-through doorways.

## Table of Contents
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part 1: 10-Team Exhibition Architecture & See-Through Doorways

In our multi-room virtual exhibition (`Room 1 <-> Room 2 <-> ... <-> Room 10`), each student team designs a self-contained room centered at local coordinates $$(0, 0)$$. The teacher connects adjacent rooms via open stone archways (`Doorway`).

Because the archways are **open and see-through**, standing inside **Room $$k$$** renders:
1. **Room $$k$$** (the active room the visitor is currently exploring)
2. **Room $$k + 1$$** (physically rendered at `translate(0, 0, -1208.0f)` beyond the North Archway)
3. **Room $$k - 1$$** (physically rendered at `translate(0, 0, +1208.0f)` beyond the South Archway)

```
+-----------------------------------------------------------------------------------+
|                   3-ROOM ACTIVE SLIDING WINDOW (LAZY LOADING)                     |
+-----------------------------------------------------------------------------------+
|                                                                                   |
|   [Room k-2]       +------------+       +------------+       +------------+       |
|   (Unloaded)       |  Room k-1  | <===> |   Room k   | <===> |  Room k+1  |       |
|                    |  (South    | Open  |  (Visitor  | Open  |  (North    |       |
|                    |  Neighbor) | Arch  |  Inside)   | Arch  |  Neighbor) |       |
|                    +------------+       +------------+       +------------+       |
|                                                                                   |
|                    |<----------- Active in RAM / GPU VRAM -------------->|       |
+-----------------------------------------------------------------------------------+
```

{: .important }
> **Key Architectural Insight:**
> With **Lazy Loading** (maintaining only a 3-room sliding window `[k - 1, k, k + 1]` in memory), the total number of teams in the exhibition (10 rooms, 20 rooms, or 50 rooms) **no longer limits JVM Heap memory**, because at most **3 rooms** ever reside in RAM/VRAM simultaneously.

---

## Part 2: Empirical Stress Test & `.obj` Resolution Benchmark

We conducted an automated stress test (`GalleryStressTest.java`) modeling **20 connected rooms with 10 statues per room (`200` statues total)** plus **4 paintings per room (`80` paintings)** and benchmarked synthetic `.obj` statue meshes from **`1,000` faces** up to **`250,000` faces**.

### 1. 20-Room × 10-Statue (`200` Statues) Walkthrough Benchmark
* **200 Statues with Default `statue.obj` (`~96` faces each):**
  * **Total Build + Parse + Bounding-Box Normalization Time:** `~90–125 ms`
  * **Total JVM Heap Footprint:** `8.52 MB`
  * **Full 20-Room Walkthrough Simulation** (traversing all 19 North doorways, checking pedestal collision against all 200 statues, and evaluating HUD proximity across all 200 statues): **`1.99–4.20 ms` total** (`< 0.02 ms` per room).
* **200 Statues Sharing a High-Res `50,000`-Face `.obj` Mesh (`10,000,000` virtual triangles across 20 rooms):**
  * **Total Setup Time:** `95 ms`
  * **Total JVM Heap Footprint:** `22.15 MB`

### 2. Per-Model `.obj` Face Resolution Benchmark (`PShapeOBJ`)

| `.obj` Resolution (Faces / Triangles) | Vertices (`v`) | `.obj` File Size on Disk | Parse + Normalize Time (1 Statue) | JVM Heap per Loaded `.obj` (`PShapeOBJ`) | Estimated Heap for **200 Unique (Unshared)** Statues |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **1,000 faces** | ~502 | `0.04 MB` | `13 ms` | **`0.43 MB`** | `85.7 MB` |
| **5,000 faces** | ~2,502 | `0.19 MB` | `24–30 ms` | **`2.18 MB`** | `435.3 MB` |
| **10,000 faces** | ~5,002 | `0.39 MB` | `24–27 ms` | **`4.31 MB`** | `862.2 MB` |
| **25,000 faces** | ~12,502 | `1.01 MB` | `54–59 ms` | **`10.86 MB`** | `2,171.5 MB` (~2.17 GB) |
| **50,000 faces** | ~25,002 | `2.08 MB` | `101–106 ms` | **`21.66 MB`** | `4,331.7 MB` (~4.33 GB) |
| **100,000 faces** | ~50,002 | `4.23 MB` | `172–246 ms` | **`43.29 MB`** | `8,658.0 MB` (~8.66 GB — OOM on default heap) |
| **250,000 faces** | ~125,002 | `10.80 MB` | `493 ms` | **`107.98 MB`** | `21,595.2 MB` (~21.6 GB — OOM) |

---

## Part 3: Under the Hood — Why Processing `.obj` Files Have Specific Limits

Inspecting Processing's `processing.core.PShapeOBJ` and `processing.opengl.PShapeOpenGL` reveals three internal design constraints:

1. **Java Heap Overhead (`~452 bytes` per Face):**
   * `PShapeOBJ.addChildren(...)` allocates **one separate child `PShapeOBJ` object plus a `float[vertexCount][12]` 2D array for every single `f` (face) line** in the `.obj` file.
   * Each face therefore consumes `~450 bytes` of Java heap on load, plus another `~150–250 bytes` when `PShapeOpenGL` tessellates the children into OpenGL buffers on first draw.
2. **16-Bit Unsigned Short GPU Index Batches (`65,536` vertices per draw call):**
   * Processing's P3D OpenGL renderer (`TessGeometry`) uses `java.nio.ShortBuffer polyIndicesBuffer` (`GL_UNSIGNED_SHORT`).
   * Whenever a single `.obj` shape exceeds **65,536 unshared vertices** (~21,845 disconnected triangles), Processing's `IndexCache` automatically splits the mesh across multiple OpenGL draw calls.
3. **`.obj` Syntax Support & Restrictions in `PShapeOBJ`:**
   * **Positive 1-Based Indices Required:** Standard Wavefront `.obj` permits negative (relative) indices (`f -3 -2 -1`), but `PShapeOBJ` computes `vertIdx.get(i) - 1` directly into its vertex `ArrayList`, throwing an `IndexOutOfBoundsException` on negative indices.
   * **Triangles, Quads, and Convex N-gons Supported:** `3`-vertex (`TRIANGLES`), `4`-vertex (`QUADS`), and `>4`-vertex (`POLYGON`) faces all parse cleanly, though **triangulated meshes (`3` vertices per face)** load and tessellate fastest.
   * **Unsupported Directives Ignored:** NURBS/curves (`cstype`, `curv`, `surf`), vertex RGB extensions (`v x y z r g b`), smoothing groups (`s`), and lines (`l`) are ignored.

---

## Part 4: Real Limitations in a Live 10-Team Exhibition (With Lazy Loading)

When lazy loading is active (only keeping `[Room k - 1, Room k, Room k + 1]` in memory), the real bottlenecks are:

1. **Doorway Crossing Hitch (Parse + First-Frame OpenGL Tessellation Latency):**
   * Stepping from `Room k` into `Room k + 1` requires loading `Room k + 2` so it can appear through `Room k + 1`'s far North archway.
   * Loading a statue has two phases:
     1. **CPU Disk Parse & Normalization (`new PShapeOBJ(...)`):** Can run asynchronously on a background thread (`~55 ms` for `25k` faces; `~105 ms` for `50k` faces per statue).
     2. **First-Draw OpenGL VBO Tessellation (`shape(objModel)`):** Runs on the **main OpenGL draw thread** the first frame a `PShape` is rendered (`~40–80 ms` for `25k–50k` faces per statue).
   * If a room's statues exceed the recommended polygon budget, visitors will feel a brief frame pause when crossing a doorway threshold.
2. **Per-Frame GPU Polygon Budget (`60 FPS` = `16.6 ms/frame`):**
   * Looking through an open doorway renders up to **2 rooms simultaneously** in the camera frustum.
   * Processing P3D on macOS smoothly sustains **60 FPS** at up to **`~400,000 – 600,000` visible triangles per frame** (with dynamic lighting and rotating pedestals).
3. **Active 3-Room Memory Window:**
   * Only 3 rooms are held in RAM/VRAM at once (`3 × statues_per_room`), keeping total heap usage well under `200 MB`.

---

## Part 5: Recommended `.obj` Resolution Table (With Lazy Loading)

| Exhibition Room Layout | Active Statues in 3-Room Window (`k-1, k, k+1`) | Max Statues Visible in One View (`2` Rooms) | **Best `.obj` Resolution (Sweet Spot)** | **Max Safe `.obj` Resolution (Hard Cap)** | Why This Is the Sweet Spot |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Standard Starter** (**2 statues / room**, 10 rooms = 20 total) | **6 statues** in RAM | **4 statues** on screen | **`35,000 – 60,000` faces** per statue (`~1.5 – 2.5 MB` `.obj`) | **`100,000` faces** per statue (`~4.2 MB` `.obj`) | 4 visible statues × `50k` faces = `200k` triangles on screen (locked 60 FPS); only `~130 MB` RAM for the 3-room window; doorway pre-load takes `< 200 ms`. |
| **Medium Gallery** (**4 statues / room**, 10 rooms = 40 total) | **12 statues** in RAM | **8 statues** on screen | **`20,000 – 30,000` faces** per statue (`~0.8 – 1.2 MB` `.obj`) | **`50,000` faces** per statue (`~2.1 MB` `.obj`) | 8 visible statues × `25k` faces = `200k` triangles on screen; `~130 MB` RAM for the 3-room window. |
| **Heavy Sculpture Hall** (**10 statues / room**, 10 rooms = 100 total) | **30 statues** in RAM | **20 statues** on screen | **`10,000 – 15,000` faces** per statue (`~0.4 – 0.6 MB` `.obj`) | **`25,000` faces** per statue (`~1.0 MB` `.obj`) | 20 visible statues × `15k` faces = `300k` triangles on screen; `~160 MB` RAM for the 3-room window. |

---

## Part 6: Practical Guidelines for Student Teams & Teacher Integration

### 1. Student Guide: Preparing 3D Scans in Blender
Museum 3D scans downloaded from **Sketchfab**, **Smithsonian 3D**, or **MyMiniFactory** often contain `500,000+` faces. Before placing a `.obj` file into the project's `data/` folder, students should simplify it in **Blender**:

1. Import the model into Blender (**File > Import > Wavefront (.obj)**).
2. Select the mesh, open the **Modifiers** tab (wrench icon), and add a **Decimate** modifier (*Collapse* mode):
   * **For 2 statues per room:** Adjust the *Ratio* until **Face Count** is between **`30,000` and `50,000` triangles** (`~1.5 – 2.2 MB` `.obj` file). At gallery viewing distance with smooth vertex normals, `40,000` faces looks visually identical to `500,000` faces.
   * **For 10 statues per room:** Adjust the *Ratio* until **Face Count** is between **`10,000` and `15,000` triangles** (`~0.5 MB` `.obj` file).
3. Right-click the statue in the 3D Viewport and choose **Shade Smooth** so vertex normals interpolate cleanly across marble surfaces.

### 2. Student Checklist: Blender `.obj` Export Settings
When exporting (**File > Export > Wavefront (.obj)**), use these exact settings for Processing `PShapeOBJ` compatibility:
* [x] **Triangulated Mesh:** **ON** (Triangles parse and tessellate faster than N-gons)
* [x] **Write Normals (`vn`):** **ON** (Essential for realistic 3D marble lighting)
* [x] **UV Coordinates (`vt`):** **ON** (If using an `.mtl` texture)
* [ ] **Relative Index:** **OFF** (Must be unchecked — Processing requires positive 1-based indices)

### 3. Teacher Guide: Lazy-Loading & Background Pre-Fetching for 10 Rooms
When combining all 10 student rooms into the final exhibition showcase:
1. **3-Room Active Window (`[currentRoomId - 1, currentRoomId, currentRoomId + 1]`):**
   * Only keep `PShape` models loaded for the current room and its immediate North and South neighbors.
   * When the visitor advances from `Room k` to `Room k + 1`, unload `Room k - 1`'s heavy `PShape` references so the garbage collector reclaims them.
2. **Asynchronous Background Pre-Parse:**
   * As soon as the visitor enters `Room k`, start parsing the `.obj` files for `Room k + 1` (and `Room k + 2`) on a background worker thread (`CompletableFuture.supplyAsync(...)` using `new PShapeOBJ(engine, reader)`).
   * Because `new PShapeOBJ(...)` only builds CPU-side Java arrays and does not touch the OpenGL context until `shape(...)` is called in `draw()`, background parsing eliminates file I/O pauses while walking between rooms.
