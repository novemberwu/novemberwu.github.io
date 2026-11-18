---
title: "Unit 3 Lab 4 - Capstone: 3D Virtual Gallery in Processing"
layout: page
nav_order: 16
parent: 2026csa
math: mathjax
---

# Unit 3 Lab 4: Capstone — 3D Virtual Gallery in Processing
{: .no_toc }

## Goals
{: .no_toc }
* Apply the complete object-oriented workflow from Labs 1–3 (**Writing Classes, Encapsulation, Constructors, and Methods**) in an interactive 3D graphical environment.
* Understand **Composition** (has-a relationships): a `Room` object *has four* `Painting` objects.
* Connect core skills across **AP CSA Units 1–3** without requiring **Unit 4 (Data Collection / Arrays / ArrayLists)**:
  * **Unit 1 (Elementary Programming):** Primitive variables (`float`, `int`, `boolean`), `Math.sqrt`/`Math.pow`, and `String` methods.
  * **Unit 2 (If/Else & Loops):** Compound boolean expressions (`&&`, `||`) for 3D wall collision, `if / else-if` proximity checks, and `for` loops for repeating architectural ceiling beams and floor medallion patterns.
  * **Unit 3 (Creating Classes):** Class anatomy, private instance variables (encapsulation), constructors, instance methods, and object composition (a `Room` *has 4* `Painting` objects).
* Curate four artworks with high-resolution image textures and AP Art History analysis (Form, Content, Context).
* Implement your own custom 3D museum gallery room using the starter kit in `~/Documents/Processing/APCSA_GalleryStarter`.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part 1: Project Overview & Architecture

Welcome to the **AP CSA Virtual Museum Capstone Project**! In this lab, you will step beyond text-based console output into an immersive 3D world rendered in real-time with **Java Processing (`P3D`)**.

```
+---------------------------------------------------------------------------------+
|                                 VIRTUAL MUSEUM ARCHITECTURE                     |
+---------------------------------------------------------------------------------+
|                                                                                 |
|   +-----------------------+              +-----------------------+              |
|   |   Room1_Mythology     |              | Room2_StudentTemplate |              |
|   | (Botticelli Masterpieces)            |  (Your Curated Room)  |              |
|   |                       |              |                       |              |
|   | - westPainting        |   Doorway    | - westPainting        |              |
|   | - eastPainting        | <==========> | - eastPainting        |              |
|   | - northLeftPainting   |  (Pietra     | - northLeftPainting   |              |
|   | - northRightPainting  |   Serena)    | - northRightPainting  |              |
|   +-----------------------+              +-----------------------+              |
|               ^                                      ^                          |
|               |                                      |                          |
|               +------------------+-------------------+                          |
|                                  |                                              |
|                       +--------------------+                                    |
|                       |   Painting Class   |                                    |
|                       | (Encapsulated Art) |                                    |
|                       +--------------------+                                    |
+---------------------------------------------------------------------------------+
```

### How the Multi-Room Museum Works
1. Every student room is an encapsulated Java class centered at local coordinates $$(0, 0)$$.
2. A room contains **exactly 4 `Painting` objects** placed on the walls.
3. The North center wall ($$z = -600$$, $$x \in [-90, +90]$$) and South center wall ($$z = +600$$, $$x \in [-90, +90]$$) are reserved for stone archway **`Doorway`** objects.
4. When you walk your visitor into a doorway arch, the main controller (`APCSA_GalleryStarter.pde`) automatically transitions your view into the connected room!

---

## Part 2: Project File Structure

The project starter files are located at:
```text
~/Documents/Processing/APCSA_GalleryStarter/
```

| File | Owner | Concepts & Role |
| :--- | :--- | :--- |
| `Painting.pde` | **Core Model Class** | Encapsulates one artwork (`title`, `artist`, dimensions, 3D position, textures, and distance detection). |
| `Room1_Mythology.pde` | **Reference Model** | Complete, working room modeling *Sala I: Botticelli Myth & Allegory* (Primavera, Birth of Venus). |
| `Room2_StudentTemplate.pde` | **Student Workspace** | Your canvas! Contains step-by-step `// === STUDENT TODO ===` markers for your team's room. |
| `Doorway.pde` | Teacher Framework | Renders Renaissance stone archways and teleports visitors between rooms. |
| `GalleryEngine.pde` | Teacher Framework | Handles 3D camera controls (`WASD` + mouse), lighting, gold leaf frames, and the Curator HUD. |
| `APCSA_GalleryStarter.pde` | Main Sketch | Instantiates student rooms and links them with doorways. |
| `data/` | Asset Folder | Contains all artwork `.png` and `.jpg` image files. |

---

## Part 3: Deep Dive into `Painting.pde`

Inspect `Painting.pde`. Notice how every single technique learned in Labs 1, 2, and 3 is applied here:

```java
class Painting {
  // 1. Private Instance Variables (Encapsulation from Lab 1)
  private String title;
  private String artist;
  private String yearAndMedium;
  private String formAndContent;     // AP Art History Form & Content
  private String historicalContext;  // AP Art History Context

  private String wallSide;           // "NORTH", "SOUTH", "EAST", "WEST"
  private float x;
  private float y;
  private float z;
  private float frameWidth;
  private float frameHeight;
  private boolean isCircularTondo;   // true for circular Renaissance tondi

  private PImage canvasImage;
  private PImage plaqueImage;

  // 2. Constructor (Object Initialization from Lab 2)
  public Painting(
      GalleryEngine engine,
      String title,
      String artist,
      String yearAndMedium,
      String imageFileName,
      String wallSide,
      float x, float y, float z,
      float frameWidth, float frameHeight,
      boolean isCircularTondo,
      String formAndContent,
      String historicalContext) {

    this.title = title;
    this.artist = artist;
    this.yearAndMedium = yearAndMedium;
    this.wallSide = wallSide;
    this.x = x;
    this.y = y;
    this.z = z;
    this.frameWidth = frameWidth;
    this.frameHeight = frameHeight;
    this.isCircularTondo = isCircularTondo;
    this.formAndContent = formAndContent;
    this.historicalContext = historicalContext;

    // Load artwork texture from the sketch's data/ folder
    this.canvasImage = engine.loadOrCreateArtwork(imageFileName, title);
    this.plaqueImage = engine.createWallPlaqueTexture(title, artist, yearAndMedium);
  }

  // 3. Instance Methods (Calculations & Behaviors from Lab 3)

  // Euclidean distance between visitor (visitorX, visitorZ) and this painting in 2D floor space
  public float distanceToVisitor(float visitorX, float visitorZ) {
    double dx = this.x - visitorX;
    double dz = this.z - visitorZ;
    return (float) Math.sqrt(Math.pow(dx, 2) + Math.pow(dz, 2));
  }

  // Returns true if visitor is standing within 250 units of this artwork
  public boolean isVisitorNear(float visitorX, float visitorZ) {
    return distanceToVisitor(visitorX, visitorZ) < 250.0f;
  }

  // Formats museum title line
  public String getFormattedHeader() {
    return this.title.toUpperCase() + "  |  " + this.artist + " (" + this.yearAndMedium + ")";
  }

  // Displays framed painting in 3D space
  public void display(GalleryEngine engine) {
    engine.drawFramedPaintingOnWall(
      this.canvasImage, this.plaqueImage, this.wallSide,
      this.x, this.y, this.z, this.frameWidth, this.frameHeight,
      this.isCircularTondo
    );
  }

  // Getters from Lab 1
  public String getTitle() { return this.title; }
  public String getArtist() { return this.artist; }
  public String getFormAndContent() { return this.formAndContent; }
  public String getHistoricalContext() { return this.historicalContext; }
}
```

---

## Part 4: Step-by-Step Student Walkthrough

### Step 1: Launch & Explore the Starter Gallery (15 min)
1. Open **Processing 4** on your computer.
2. Select **File > Open...** and navigate to:
   ```text
   ~/Documents/Processing/APCSA_GalleryStarter/APCSA_GalleryStarter.pde
   ```
3. Click the **Run** button (play icon `▶`).
4. **Controls:**
   * Move around using `W` (forward), `S` (backward), `A` (strafe left), `D` (strafe right).
   * Look around with mouse movement.
   * `Shift` allows running; `Space` toggles mouse lock.
5. Walk up to Botticelli's *Primavera* on the West wall:
   * Notice the gold leaf frame illuminates.
   * Look down: a 3D museum label plaque is affixed below the frame.
   * Notice the 2D **Curator HUD** displays the artwork title, artist, date, form & content, and historical context!
6. Turn toward the North wall and walk straight through the stone archway into **Room 2**.

---

### Step 2: Curate Your Room's Artworks (20 min)
With your partner, decide on the curatorial theme for your gallery room. Examples:
* **The High Renaissance:** Leonardo da Vinci, Michelangelo, Raphael, Titian
* **The Dutch Golden Age:** Rembrandt, Vermeer, Frans Hals, Judith Leyster
* **Impressionism & Post-Impressionism:** Monet, Degas, Van Gogh, Cézanne
* **Surrealism & Modernism:** Salvador Dalí, René Magritte, Frida Kahlo, Georgia O'Keeffe
* **Original Student Art:** Digital paintings or photography created by your team!

#### Asset Preparation:
1. Obtain 4 high-resolution images (`.png` or `.jpg`).
2. Rename them with simple snake_case filenames (e.g. `starry_night.jpg`, `mona_lisa.png`).
3. Copy/drag all 4 images into the `APCSA_GalleryStarter/data/` folder.

---

### Step 3: Implement Your Room Class (`Room2_StudentTemplate.pde`) (60 min)
Open `Room2_StudentTemplate.pde`. Complete the five `STUDENT TODO` sections:

#### TODO 1: Declare Instance Variables
Declare the private state of your room inside `Room2_StudentTemplate.pde`:
```java
// === STUDENT TODO 1 (AP CSA Unit 3: Instance Variables & Composition) ===
// 1. Declare private String variables for roomTitle and curatorNames.
// 2. Declare 4 individual Painting variables (NO arrays required!):
//    westPainting, eastPainting, northLeftPainting, northRightPainting.
```

#### TODO 2: Write the Constructor
Initialize your room's title and instantiate all 4 `Painting` objects.

{: .important }
> **Crucial 3D Coordinate Placement Guide:**
> * **West Wall:** `x = -492.0f, z = 0.0f`, `wallSide = "WEST"`
> * **East Wall:** `x = +492.0f, z = 0.0f`, `wallSide = "EAST"`
> * **North-West Wing:** `x = -295.0f, z = -592.0f`, `wallSide = "NORTH"` (Leaves center doorway $$[-90, +90]$$ clear!)
> * **North-East Wing:** `x = +295.0f, z = -592.0f`, `wallSide = "NORTH"` (Leaves center doorway $$[-90, +90]$$ clear!)

```java
// === STUDENT TODO 2 (AP CSA Unit 3: Constructor) ===
public Room2_StudentTemplate(GalleryEngine engine) {
  // TODO 2A: Initialize this.roomTitle and this.curatorNames

  // TODO 2B: Instantiate this.westPainting
  // Use: new Painting(engine, title, artist, yearAndMedium, imageFileName, "WEST",
  //                   -492.0f, -10.0f, 0.0f, frameWidth, frameHeight, isCircularTondo,
  //                   formAndContent, historicalContext);

  // TODO 2C: Instantiate this.eastPainting on the "EAST" wall (+492.0f, -10.0f, 0.0f)

  // TODO 2D: Instantiate this.northLeftPainting on "NORTH" wall (-295.0f, -10.0f, -592.0f)

  // TODO 2E: Instantiate this.northRightPainting on "NORTH" wall (+295.0f, -10.0f, -592.0f)
}
```

#### TODO 3: Implement `display(GalleryEngine engine)`
Render the room's walls, architectural features using `for` loops, and display all 4 paintings:

```java
// === STUDENT TODO 3 (AP CSA Unit 2: Loops & Unit 3: Method Calls) ===
public void display(GalleryEngine engine) {
  // TODO 3A: Draw outer room walls (South doorway open back to Room 1):
  // engine.drawStandardRoomWalls(false, true);

  // TODO 3B: Use a for-loop to draw repeating wooden ceiling beams
  // zPos starts at -560, continues while zPos <= 560, increments by 140
  // Inside loop: engine.drawCeilingBeam(zPos);

  // TODO 3C: Use a for-loop to draw repeating floor medallions along the center corridor
  // zTile starts at -360, continues while zTile <= 360, increments by 240
  // Inside loop: engine.drawFloorMedallion(0, zTile);

  // TODO 3D: Call display(engine) on each of your 4 Painting objects
}
```

#### TODO 4: Implement Proximity Detection (`getActivePainting`)
When the visitor walks up to an artwork, the HUD needs to know *which* painting is active. Use an `if / else-if / else` structure:

```java
// === STUDENT TODO 4 (AP CSA Unit 2: If/Else Selection) ===
public Painting getActivePainting(float visitorX, float visitorZ) {
  // TODO 4: Check which painting the visitor is near using .isVisitorNear(visitorX, visitorZ)
  // If near westPainting, return this.westPainting
  // Else if near eastPainting, return this.eastPainting
  // Else if near northLeftPainting, return this.northLeftPainting
  // Else if near northRightPainting, return this.northRightPainting
  // Else return null (visitor is in open floor space)
}
```

#### TODO 5: Implement Wall Collision (`canMoveTo`)
Ensure the visitor cannot walk through solid stone walls, but **can** step into the doorway recess leading back to Room 1:

```java
// === STUDENT TODO 5 (AP CSA Unit 2: Compound Booleans & Collision) ===
public boolean canMoveTo(float nextX, float nextZ) {
  // TODO 5A: Compound boolean for inner room boundaries:
  // (nextX > -445.0f && nextX < 445.0f) AND (nextZ > -545.0f && nextZ < 545.0f)

  // TODO 5B: Compound boolean for South doorway recess:
  // (nextX > -80.0f && nextX < 80.0f) AND (nextZ >= 545.0f && nextZ < 585.0f)

  // TODO 5C: Return true if inside room bounds OR in South doorway recess
}
```

---

## Part 5: Testing & Exhibition Verification

1. Run the sketch in Processing.
2. Step through the doorway from Room 1 into your Room 2.
3. Verify your room's title and curator credit banner appear at the top:
   ```text
   Sala II: High Renaissance Masters  |  Curated by: Alice Zhang & Bob Smith
   ```
4. Walk toward each of your 4 paintings:
   * Confirm the gold frame highlights as you approach within 250 units.
   * Confirm the artwork label appears on the wall.
   * Confirm the Curator HUD displays your research text with full formatting.
5. Test the walls: walk into the corners and verify your collision prevents walking out into the void.
6. Walk backward through the South doorway: verify you smoothly return to Room 1!

---

## Capstone Rubric & AP CSA Learning Standards

| Criteria | Points | Description |
| :--- | :---: | :--- |
| **Encapsulation & Fields** | 20 pts | All room fields (`roomTitle`, `curatorNames`, 4 `Painting` objects) are strictly `private`. |
| **Constructor Integrity** | 25 pts | Constructor cleanly instantiates all 4 `Painting` objects with correct wall sides and non-colliding coordinates. |
| **Iterative Architectural Decor** | 15 pts | Uses `for` loops to generate ceiling beams and floor inlays with proper loop bounds. |
| **Proximity & Selection** | 20 pts | `getActivePainting` cleanly executes `isVisitorNear` calls and returns correct references or `null`. |
| **Compound Wall Collision** | 10 pts | Uses compound boolean operators (`&&`, `||`) to enforce walkable bounds while preserving doorway access. |
| **Curatorial Quality** | 10 pts | Complete, insightful Art History text entries for all 4 paintings with high-quality textures in `data/`. |
| **Total** | **100 pts** | **Mastery of AP CSA Units 1–3!** |
