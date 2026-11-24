---
title: "Unit 3 Lab 4 Setup - Processing 4 Installation & Git Basics"
layout: page
nav_order: 18
parent: 2026csa
---

# Unit 3 Lab 4 Setup: Processing 4 Installation & Git Basics
{: .no_toc }

## Overview
{: .no_toc }
Before you start building your 3D Virtual Gallery room, follow this guide to install **Processing 4** and learn the essential **Git** commands your team will use throughout the project's Software Development Life Cycle (SDLC).

## Table of Contents
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part 1: Installing Processing 4

### 1. Download Processing 4
Download the official installer for your operating system from the Processing website:

* **Official Download Link:** **[https://processing.org/download](https://processing.org/download)**

Choose the build that matches your laptop:
* **macOS (Apple Silicon — M1 / M2 / M3 / M4):** Download **macOS (Apple Silicon)**
* **macOS (Intel):** Download **macOS (Intel)**
* **Windows:** Download **Windows (Intel 64-bit)**

### 2. Install & Launch
* **On macOS:**
  1. Unzip the downloaded `.zip` archive (or open the `.dmg`).
  2. Drag **`Processing.app`** into your **`Applications`** folder.
  3. Open **Processing** from Applications (if macOS asks for confirmation on first launch, click **Open**).
* **On Windows:**
  1. Right-click the downloaded `.zip` file and select **Extract All...** to a permanent folder (e.g., `C:\Processing\`).
  2. Double-click **`processing.exe`** inside the extracted folder to launch Processing 4.

### 3. Configure Memory (Recommended: 4096 MB)
3D galleries with high-resolution paintings (`.jpg`/`.png`) and 3D statue meshes (`.obj`) need more memory than Processing's small default setting. Configure this once:

1. Open Processing 4 and go to **Processing → Settings...** (`Cmd + ,` on macOS) or **File → Preferences...** (`Ctrl + ,` on Windows).
2. Check **"Increase maximum available memory to:"** and enter **`4096`** MB.
3. Click **OK** and restart Processing.

### 4. Open & Run the Starter Gallery
1. In Processing 4, select **File → Open...**.
2. Navigate to your cloned `APCSA_GalleryStarter` folder and open **`APCSA_GalleryStarter.pde`**.

{: .important }
> **Rule of Processing Sketches:**
> Always open the **`.pde` file whose name matches the folder name** (`APCSA_GalleryStarter.pde`). Processing automatically loads all other `.pde` tabs (`Painting.pde`, `Statue.pde`, `GalleryLight.pde`, `Room1_Mythology.pde`, `Room2_StudentTemplate.pde`, `Doorway.pde`, `GalleryEngine.pde`) in the same window.

3. Click the **Play (`▶`)** button in the top-left corner to launch the 3D Virtual Gallery!
4. **Adding Artworks Easily:** To add a painting image (`.jpg`/`.png`) or 3D statue (`.obj`/`.mtl`/`.png`), either copy the files into the sketch's `data/` folder or **drag and drop** them directly onto the Processing editor window.

*(Optional macOS Terminal Launch:)*
```sh
/Applications/Processing.app/Contents/MacOS/Processing cli --sketch=~/Documents/Processing/APCSA_GalleryStarter --run
```

---

## Part 2: Basic Usage of Git

**Git** is a version control system used by software engineers to track changes, collaborate safely in teams, and recover previous versions if something breaks.

### 1. Check / Install Git & Configure Your Identity
Open **Terminal** (macOS) or **Git Bash** (Windows) and check if Git is installed:

```sh
git --version
```

* **If Git is not installed:**
  * **macOS:** Run `xcode-select --install` in Terminal and click **Install**.
  * **Windows:** Download and install Git from **[https://git-scm.com/downloads](https://git-scm.com/downloads)**.

Next, set your name and email once so every commit records who authored the change:

```sh
git config --global user.name "Your Name"
git config --global user.email "your.email@school.edu"
```

---

### 2. Clone the Starter Repository
Download your team's copy of the starter project into your Processing sketchbook folder (`~/Documents/Processing/`):

```sh
cd ~/Documents/Processing
git clone git@github.com:novemberwu/APCSA_GalleryStarter.git
cd APCSA_GalleryStarter
```

---

### 3. Create Your Team Branch (Never Code Directly on `main`)
Before making any edits, create a dedicated branch for your team:

```sh
git checkout -b teamX-renaissance-room
```

Check which branch you are currently on at any time:

```sh
git branch
```

---

### 4. The Core 4-Step Git Cycle (`status` → `add` → `commit` → `push`)

Follow this cycle after every milestone instead of saving everything for one giant commit at the end:

1. **Check what files changed:**
   ```sh
   git status
   ```
2. **Stage the files you want to save:**
   ```sh
   git add Room2_StudentTemplate.pde data/
   ```
   *(Tip: Check `git status` first so you don't accidentally add temporary files or 200 MB raw downloads.)*
3. **Commit with a clear, descriptive message:**
   ```sh
   git commit -m "feat(room2): add 4 curated paintings and GalleryLight preset"
   ```
4. **Push your branch to GitHub:**
   ```sh
   git push -u origin teamX-renaissance-room
   ```
   *(After the first push with `-u`, future pushes only need `git push`.)*

#### Recommended 5 Milestone Commits (SDLC Rubric)
* **Commit 1:** Room title, curator credit, `GalleryLight`, and `for`-loop ceiling beams & floor medallions.
* **Commit 2:** 4 wall `Painting` objects + optimized images (`<= 2560px`) in `data/`.
* **Commit 3:** 2 floor `Statue` objects + decimated `.obj`/`.mtl`/`.png` files (`<= 50k` faces) in `data/`.
* **Commit 4:** Proximity selectors (`getActivePainting`, `getActiveStatue`) & wall/pedestal collision (`canMoveTo`).
* **Commit 5:** Lighting tuning, bug fixes, and final 3D walkthrough verification.

---

### 5. Inspecting History & Recovering Mistakes

* **View your commit history:**
  ```sh
  git log --oneline
  ```
* **See exact line-by-line changes before committing:**
  ```sh
  git diff
  ```
* **Recover an accidentally deleted or broken file** (restores the file from your last commit!):
  ```sh
  git restore APCSA_GalleryStarter.pde
  ```
* **Unstage a file you added by mistake** (keeps your edits on disk):
  ```sh
  git restore --staged <filename>
  ```
* **Pull latest updates from your teammate on the same branch:**
  ```sh
  git pull
  ```

---

### 6. Git Quick Reference Cheat Sheet

| Command | What It Does | When to Use It |
| :--- | :--- | :--- |
| `git clone <url>` | Downloads a remote repository to your laptop | Once at the start of the project |
| `git checkout -b <branch>` | Creates and switches to a new feature branch | Once when starting your team's room |
| `git status` | Shows modified, staged, and untracked files | Before every `git add` and `git commit` |
| `git diff` | Shows exact code changes since the last commit | Reviewing your work before committing |
| `git add <file>` | Stages a file or folder for the next commit | After finishing a milestone |
| `git commit -m "msg"` | Saves a permanent snapshot with a message | At each milestone (4–5+ times per project) |
| `git push` | Uploads your commits to GitHub | To back up work and share with your teammate |
| `git pull` | Downloads your teammate's latest commits | At the start of every class period |
| `git log --oneline` | Displays a compact list of past commits | Checking milestone history |
| `git restore <file>` | Restores a deleted or broken file to the last commit | When you accidentally break or delete a file |
