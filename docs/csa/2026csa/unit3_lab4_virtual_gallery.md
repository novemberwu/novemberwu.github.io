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
* Apply the complete object-oriented workflow from Labs 1–3 (**Writing Classes, Encapsulation, Constructor Overloading, and Instance Methods**) in an interactive 3D graphical environment.
* Understand **Object Composition** (*has-a* relationships): a `Room` object *has four* wall `Painting` objects and *two* 3D floor `Statue` objects.
* Connect core skills across **AP CSA Units 1–3** without requiring **Unit 4 (Data Collection / Arrays / ArrayLists)**:
  * **Unit 1 (Elementary Programming):** Primitive variables (`float`, `int`, `boolean`), `Math.sqrt`/`Math.pow` for 2D Euclidean distance, and `String` methods (`toUpperCase()`).
  * **Unit 2 (If/Else & Loops):** Compound boolean expressions (`&&`, `||`, `!`) for 3D wall and circular statue pedestal collision, `if / else-if` proximity checks, and `for` loops for repeating architectural ceiling beams and floor medallion patterns.
  * **Unit 3 (Creating Classes):** Class anatomy, private instance variables (encapsulation), **overloaded constructors with `this(...)`**, instance methods, and object composition (`Room` composed of `Painting` and `Statue` instances).
* Curate **4 wall paintings** (`.png`/`.jpg`) and **2 floor sculptures** (`.obj` 3D meshes or Renaissance marble styles) with AP Art History analysis (Form, Content, Context).
* Implement your own custom 3D museum gallery room using the starter kit in `~/Documents/Processing/APCSA_GalleryStarter` (or pure Java in `~/src/APCSA_GalleryStarter`).

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
|   +-------------------------+   Open Arch    +-------------------------+        |
|   |     Room1_Mythology     |   Doorway      |  Room2_StudentTemplate  |        |
|   | (Botticelli Gallery)    | <============> |   (Your Curated Room)   |        |
|   |                         | (See-Through)  |                         |        |
|   | Wall Paintings (4):     |                | Wall Paintings (4):     |        |
|   | - westPainting          |                | - westPainting          |        |
|   | - eastPainting          |                | - eastPainting          |        |
|   | - northLeftPainting     |                | - northLeftPainting     |        |
|   | - northRightPainting    |                | - northRightPainting    |        |
|   |                         |                |                         |        |
|   | Floor Statues (2):      |                | Floor Statues (2):      |        |
|   | - westFloorStatue       |                | - westFloorStatue       |        |
|   | - eastFloorStatue       |                | - eastFloorStatue       |        |
|   +-------------------------+                +-------------------------+        |
|             |             \                    /             |                  |
|             |              \                  /              |                  |
|             v               v                v               v                  |
|   +--------------------+     +------------------------------+                   |
|   |   Painting Class   |     |         Statue Class         |                   |
|   | (Wall Canvas+Frame)|     | (3D .obj Mesh/Marble+Pedestal|                   |
|   +--------------------+     +------------------------------+                   |
+---------------------------------------------------------------------------------+
```

### How the Multi-Room Museum Works
1. Every student room is an encapsulated Java class centered at local coordinates $$(0, 0)$$.
2. A room contains **4 `Painting` objects** mounted on the walls and **2 `Statue` objects** placed on 3D pedestals on the gallery floor.
3. The North center wall ($$z = -600$$, $$x \in [-90, +90]$$) and South center wall ($$z = +600$$, $$x \in [-90, +90]$$) are reserved for open stone archway **`Doorway`** objects.
4. Doorways are **100% open and see-through**: you can see the adjacent room's paintings and rotating statues directly through the archway and walk continuously from room to room using `W` / `Up Arrow`!

---

## Part 2: Project File Structure

The project starter files are available in Processing IDE format at `~/Documents/Processing/APCSA_GalleryStarter/` (and pure Java format at `~/src/APCSA_GalleryStarter/`):

| File | Owner | Concepts & Role |
| :--- | :--- | :--- |
| `Painting.pde` | **Core Model Class 1** | Encapsulates one wall painting (`title`, `artist`, dimensions, 3D wall position, textures, and proximity detection). |
| `Statue.pde` | **Core Model Class 2** | Encapsulates one 3D floor sculpture (`title`, `sculptor`, `.obj` mesh or marble style, pedestal type, rotation speed, circular collision radius, and overloaded constructors). |
| `Room1_Mythology.pde` | **Reference Model** | Complete, working room modeling *Sala I: Botticelli Myth, Allegory & Classical Sculpture* (4 paintings + 2 rotating `.obj` floor statues). |
| `Room2_StudentTemplate.pde` | **Student Workspace** | Your canvas! Contains step-by-step `// === STUDENT TODO ===` markers for your team's 4 paintings and 2 floor statues. |
| `Doorway.pde` | Teacher Framework | Renders open Renaissance *pietra serena* stone archways and manages room-to-room walk-through transitions. |
| `GalleryEngine.pde` | Teacher Framework | Handles 3D camera controls (`WASD`/Arrows + mouse), lighting, gold leaf frames, `.obj` normalization, 3D pedestals, and the Curator HUD. |
| `APCSA_GalleryStarter.pde` | Main Sketch | Instantiates student rooms, renders see-through neighbor rooms, and links rooms with doorways. |
| `data/` | Asset Folder | Contains artwork `.png`/`.jpg` textures and 3D `.obj` statue models (`statue.obj`, `vase.obj`). |

---

## Part 3: Deep Dive into `Painting.pde` and `Statue.pde`

### 1. Wall Artwork Abstraction: `Painting.pde`
Inspect `Painting.pde`. Notice how every technique learned in Labs 1, 2, and 3 is applied to a wall-mounted painting:

```java
class Painting {
  // 1. Private Instance Variables (Encapsulation from Lab 1)
  private String title;
  private String artist;
  private String yearAndMedium;
  private String formAndContent;     // AP Art History Form & Content
  private String historicalContext;  // AP Art History Context

  private String wallSide;           // "NORTH", "SOUTH", "EAST", "WEST"
  private float x, y, z;
  private float frameWidth, frameHeight;
  private boolean isCircularTondo;   // true for circular Renaissance tondi

  private PImage canvasImage;
  private PImage plaqueImage;

  // 2. Constructor (Object Initialization from Lab 2)
  public Painting(
      GalleryEngine engine,
      String title, String artist, String yearAndMedium,
      String imageFileName, String wallSide,
      float x, float y, float z,
      float frameWidth, float frameHeight,
      boolean isCircularTondo,
      String formAndContent, String historicalContext) {

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

    if (engine != null) {
      this.canvasImage = engine.loadOrCreateArtwork(imageFileName, title);
      this.plaqueImage = engine.createWallPlaqueTexture(title, artist, yearAndMedium);
    }
  }

  // 3. Instance Methods (Calculations & Behaviors from Lab 3)
  public float distanceToVisitor(float visitorX, float visitorZ) {
    double dx = this.x - visitorX;
    double dz = this.z - visitorZ;
    return (float) Math.sqrt(Math.pow(dx, 2) + Math.pow(dz, 2));
  }

  public boolean isVisitorNear(float visitorX, float visitorZ) {
    return distanceToVisitor(visitorX, visitorZ) < 250.0f;
  }

  public String getFormattedHeader() {
    return this.title.toUpperCase() + "  |  " + this.artist + " (" + this.yearAndMedium + ")";
  }

  public void display(GalleryEngine engine) {
    engine.drawFramedPaintingOnWall(
      this.canvasImage, this.plaqueImage, this.wallSide,
      this.x, this.y, this.z, this.frameWidth, this.frameHeight,
      this.isCircularTondo
    );
  }
}
```

---

### 2. 3D Floor Sculpture Abstraction: `Statue.pde`
Unlike a flat painting hung on a wall, a 3D **`Statue`** sits on the walkable gallery floor at $$(x, z)$$. This introduces three new object-oriented design requirements:
1. **3D Mesh & Pedestal Encapsulation:** A statue encapsulates a 3D model (`modelOrStyle`: either a Wavefront `.obj` file like `"statue.obj"` / `"vase.obj"` or a procedural marble style like `"VENUS_MARBLE"`, `"MEDICI_BUST"`, `"PORPHYRY_URN"`), a `pedestalType` (`"BLOCK"`, `"ROTATING"`, or `"COLUMN"`), and an animated `rotatingSpeed` (degrees/second).
2. **Constructor Overloading (`this(...)`):**
   * **Constructor 1 (Full Constructor):** Lets the curator specify custom `pedestalType` and `rotatingSpeed`.
   * **Constructor 2 (Overloaded Default Constructor):** Uses `this(..., "BLOCK", 30.0f, ...)` to default to a classic stone block pedestal rotating at `30.0` degrees/second.
3. **Circular Pedestal Collision (`collidesWithVisitor`):** Because pedestals stand inside the room, visitors must not walk through them! Calling `collidesWithVisitor(nextX, nextZ)` checks whether `distanceToVisitor(nextX, nextZ) < this.collisionRadius` (`72.0f`).

```java
class Statue {
  // 1. Private Instance Variables
  private String title;
  private String sculptor;
  private String yearAndMedium;
  private String modelOrStyle;      // "statue.obj", "vase.obj", "VENUS_MARBLE", "MEDICI_BUST", "PORPHYRY_URN"
  private String pedestalType;      // "BLOCK", "ROTATING", or "COLUMN"
  private float x;                  // Floor X coordinate (room center is 0)
  private float z;                  // Floor Z coordinate (room center is 0)
  private float rotatingSpeed;      // Degrees per second (0 = stationary)
  private float collisionRadius;    // Circular collision radius (72.0f) around pedestal
  private String formAndContent;
  private String historicalContext;

  private PShape objModel;

  // 2A. Constructor 1: Full Statue Constructor
  public Statue(
      GalleryEngine engine,
      String title, String sculptor, String yearAndMedium,
      String modelOrStyle, float x, float z,
      String pedestalType, float rotatingSpeed,
      String formAndContent, String historicalContext) {

    this.title = title;
    this.sculptor = sculptor;
    this.yearAndMedium = yearAndMedium;
    this.modelOrStyle = modelOrStyle;
    this.x = x;
    this.z = z;
    this.pedestalType = pedestalType;
    this.rotatingSpeed = rotatingSpeed;
    this.collisionRadius = 72.0f;
    this.formAndContent = formAndContent;
    this.historicalContext = historicalContext;

    if (engine != null && modelOrStyle != null && modelOrStyle.toLowerCase().endsWith(".obj")) {
      engine.loadNormalizedObjStatue(this, modelOrStyle);
    }
  }

  // 2B. Constructor 2: Overloaded Constructor (Chains via this(...) with BLOCK & 30 deg/s defaults)
  public Statue(
      GalleryEngine engine,
      String title, String sculptor, String yearAndMedium,
      String modelOrStyle, float x, float z,
      String formAndContent, String historicalContext) {
    this(engine, title, sculptor, yearAndMedium, modelOrStyle, x, z, "BLOCK", 30.0f, formAndContent, historicalContext);
  }

  // 3. Euclidean Distance, HUD Proximity & Circular Pedestal Collision
  public float distanceToVisitor(float visitorX, float visitorZ) {
    double dx = this.x - visitorX;
    double dz = this.z - visitorZ;
    return (float) Math.sqrt(Math.pow(dx, 2) + Math.pow(dz, 2));
  }

  // Triggers the 3D Statue Curator HUD when visitor steps within 195 units
  public boolean isVisitorNear(float visitorX, float visitorZ) {
    return distanceToVisitor(visitorX, visitorZ) < 195.0f;
  }

  // Prevents visitor from walking through the marble pedestal (radius 72 units)
  public boolean collidesWithVisitor(float nextX, float nextZ) {
    return distanceToVisitor(nextX, nextZ) < this.collisionRadius;
  }

  public String getFormattedHeader() {
    return "[3D STATUE] " + this.title.toUpperCase() + "  |  " + this.sculptor + " (" + this.yearAndMedium + ")";
  }

  public void display(GalleryEngine engine) {
    if (engine == null) return;
    engine.drawStatueOnPedestal(this);
  }
}
```

---

## Part 4: Step-by-Step Student Walkthrough

### Step 1: Launch & Explore the Starter Gallery (15 min)
1. Open **Processing 4** and open:
   ```text
   ~/Documents/Processing/APCSA_GalleryStarter/APCSA_GalleryStarter.pde
   ```
   *(Or run `/Users/wuyuan/src/APCSA_GalleryStarter/scripts/run.sh` in IntelliJ / terminal).*
2. Click **Run** (`▶`).
3. **Controls:**
   * Move with `W` / `S` / `A` / `D` or **Arrow Keys**.
   * Look around with the mouse (press `Space` to lock/unlock mouse look) or `Q` / `E`.
4. Walk up to Botticelli's *Primavera* on the West wall and then walk toward the two rotating 3D floor statues in Room 1:
   * Notice you cannot walk *through* the statue pedestals (`collidesWithVisitor`).
   * Notice the **Curator HUD** switches to `[3D STATUE]` mode when you approach a pedestal, displaying the sculptor, pedestal type, rotation speed, and AP Art History analysis!
5. Look North: notice you can **see Room 2 directly through the open stone archway**. Walk forward (`W` or `Up Arrow`) straight through the archway into **Room 2**!

---

### Step 2: Curate Your Room's 4 Paintings & 2 Statues (20 min)
With your partner, choose a curatorial theme for your room and prepare:
1. **4 Wall Painting Images (`.png` or `.jpg`):** Copy into `APCSA_GalleryStarter/data/`.
2. **2 Floor Sculptures (`.obj` files or Built-In Styles):**
   * You can place custom `.obj` files into `data/` (e.g., `"statue.obj"`, `"vase.obj"`, or a simplified museum scan — see the [3D Statue `.obj` Benchmarks & Blender Export Guide](unit3_lab4_exhibition_guidelines.html)).
   * Or choose from the built-in Renaissance marble styles: `"VENUS_MARBLE"`, `"MEDICI_BUST"`, or `"PORPHYRY_URN"`.

---

### Step 3: Implement Your Room Class (`Room2_StudentTemplate.pde`) (60 min)

#### TODO 1: Declare Instance Variables (Paintings + Statues)
Declare the private state of your room inside `Room2_StudentTemplate.pde`:
```java
// === STUDENT TODO 1 (AP CSA Unit 3: Instance Variables & Composition) ===
private String roomTitle;
private String curatorNames;

// 4 Wall Paintings:
private Painting westPainting;
private Painting eastPainting;
private Painting northLeftPainting;
private Painting northRightPainting;

// 2 Floor Statues placed at (x, z) coordinates:
private Statue westFloorStatue;
private Statue eastFloorStatue;
```

#### TODO 2: Write the Constructor (4 Paintings + 2 Floor Statues)
Initialize your room's title, instantiate all 4 `Painting` objects on the walls, and instantiate your 2 `Statue` objects on the floor.

{: .important }
> **Crucial 3D Coordinate Placement Guide:**
> * **West Wall Painting:** `x = -492.0f, z = 0.0f`, `wallSide = "WEST"`
> * **East Wall Painting:** `x = +492.0f, z = 0.0f`, `wallSide = "EAST"`
> * **North-West Wing Painting:** `x = -295.0f, z = -592.0f`, `wallSide = "NORTH"`
> * **North-East Wing Painting:** `x = +295.0f, z = -592.0f`, `wallSide = "NORTH"`
> * **West Floor Statue:** `x = -210.0f, z = -140.0f` (Leaves center walkway $$x \in [-100, +100]$$ clear!)
> * **East Floor Statue:** `x = +210.0f, z = -140.0f` (Leaves center walkway $$x \in [-100, +100]$$ clear!)

```java
// === STUDENT TODO 2B (Statue Placement on the Gallery Floor) ===
// Pedestal types: "BLOCK", "ROTATING", or "COLUMN" (or use the 9-parameter overloaded constructor!)
this.westFloorStatue = new Statue(
  engine,
  "Bust of Lorenzo il Magnifico",
  "Florentine Renaissance Workshop",
  "c. 1485, Carrara Marble & Gilded Bronze",
  "MEDICI_BUST",          // or "statue.obj"
  -210.0f, -140.0f,       // Floor (x, z)
  "COLUMN",               // Pedestal type
  20.0f,                  // Rotating speed (deg/s)
  "FORM & CONTENT: Portrait bust mounted on a fluted Corinthian column pedestal...",
  "CONTEXT: Patron of Sandro Botticelli and founder of the San Marco sculpture garden..."
);

this.eastFloorStatue = new Statue(
  engine,
  "Imperial Porphyry Medici Urn",
  "Florentine Stonecutters",
  "c. 1480, Red Egyptian Porphyry & Gold Leaf",
  "PORPHYRY_URN",         // or "vase.obj"
  210.0f, -140.0f,        // Floor (x, z)
  "ROTATING",             // Twisted rotating pedestal
  -30.0f,                 // Counter-clockwise at 30 deg/s
  "FORM & CONTENT: Carved imperial red porphyry ceremonial vessel...",
  "CONTEXT: Red porphyry was prized by the Medici as the stone of Roman emperors..."
);
```

#### TODO 3: Implement `display(GalleryEngine engine)`
Render the room's walls, architectural features using `for` loops, all 4 paintings, and both 3D statues:

```java
// === STUDENT TODO 3 (AP CSA Unit 2: Loops & Unit 3: Method Calls) ===
public void display(GalleryEngine engine) {
  engine.drawStandardRoomWalls(false, true);

  for (int zPos = -560; zPos <= 560; zPos += 140) {
    engine.drawCeilingBeam(zPos);
  }

  for (int zTile = -360; zTile <= 360; zTile += 240) {
    engine.drawFloorMedallion(0, zTile);
  }

  // Display all 4 Wall Paintings
  this.westPainting.display(engine);
  this.eastPainting.display(engine);
  this.northLeftPainting.display(engine);
  this.northRightPainting.display(engine);

  // Display both 3D Floor Statues
  this.westFloorStatue.display(engine);
  this.eastFloorStatue.display(engine);
}
```

#### TODO 4: Implement Proximity Detection (`getActivePainting` & `getActiveStatue`)
Use `if / else-if / else` selection to return whichever painting or statue the visitor is currently standing near:

```java
// === STUDENT TODO 4A: Wall Painting Proximity ===
public Painting getActivePainting(float visitorX, float visitorZ) {
  if (this.westPainting.isVisitorNear(visitorX, visitorZ)) {
    return this.westPainting;
  } else if (this.eastPainting.isVisitorNear(visitorX, visitorZ)) {
    return this.eastPainting;
  } else if (this.northLeftPainting.isVisitorNear(visitorX, visitorZ)) {
    return this.northLeftPainting;
  } else if (this.northRightPainting.isVisitorNear(visitorX, visitorZ)) {
    return this.northRightPainting;
  } else {
    return null;
  }
}

// === STUDENT TODO 4B: 3D Floor Statue Proximity ===
public Statue getActiveStatue(float visitorX, float visitorZ) {
  if (this.westFloorStatue.isVisitorNear(visitorX, visitorZ)) {
    return this.westFloorStatue;
  } else if (this.eastFloorStatue.isVisitorNear(visitorX, visitorZ)) {
    return this.eastFloorStatue;
  } else {
    return null;
  }
}
```

#### TODO 5: Implement Wall & Statue Pedestal Collision (`canMoveTo`)
Combine outer wall bounds, the open doorway recess, and statue pedestal collision (`!hitsStatue`):

```java
// === STUDENT TODO 5 (AP CSA Unit 2: Compound Booleans + Statue Collision) ===
public boolean canMoveTo(float nextX, float nextZ) {
  boolean insideEastWestWalls = (nextX > -445.0f && nextX < 445.0f);
  boolean insideNorthSouthWalls = (nextZ > -545.0f && nextZ < 545.0f);
  boolean inSouthDoorwayRecess = (nextX > -80.0f && nextX < 80.0f && nextZ >= 545.0f && nextZ < 585.0f);

  boolean insideRoomOrDoor = (insideEastWestWalls && insideNorthSouthWalls) || inSouthDoorwayRecess;

  boolean hitsStatue = this.westFloorStatue.collidesWithVisitor(nextX, nextZ)
                    || this.eastFloorStatue.collidesWithVisitor(nextX, nextZ);

  return insideRoomOrDoor && !hitsStatue;
}
```

---

## Part 5: Testing & Exhibition Verification

1. Run the sketch in Processing (or IntelliJ).
2. Stand in Room 1 and look North through the open archway—confirm you can see your Room 2 paintings and rotating statues through the doorway.
3. Walk straight through the North archway (`W` or `Up Arrow`) into Room 2.
4. Walk up to each of your **4 wall paintings** and **2 floor statues**:
   * Confirm the Curator HUD displays the artwork details for each painting and `[3D STATUE]` card for each sculpture.
   * Confirm you cannot walk through either statue pedestal (`collidesWithVisitor`).
5. Walk back through the South doorway into Room 1 to confirm seamless two-way navigation.

{: .tip }
> **Preparing High-Resolution `.obj` Statues for the Class Exhibition:**
> Read the companion guide **[Unit 3 Lab 4 Addendum: 3D Statue (`.obj`) Benchmarks & Exhibition Guidelines](unit3_lab4_exhibition_guidelines.html)** for `.obj` polygon count targets (`30,000–50,000` triangles), Blender *Decimate* instructions, and 10-team exhibition lazy-loading architecture.

---

## Capstone Rubric & AP CSA Learning Standards

| Criteria | Points | Description |
| :--- | :---: | :--- |
| **Encapsulation & Composition** | 20 pts | All room fields (`roomTitle`, `curatorNames`, 4 `Painting` objects, 2 `Statue` objects) are strictly `private`. |
| **Constructor & Overloading** | 25 pts | Constructor cleanly instantiates all 4 `Painting` objects and 2 `Statue` objects (using full or overloaded `Statue` constructors) with valid 3D coordinates. |
| **Iterative Architectural Decor** | 15 pts | Uses `for` loops to generate ceiling beams and floor inlays, and calls `.display(engine)` on all 4 paintings and 2 statues. |
| **Proximity & Selection** | 20 pts | `getActivePainting` and `getActiveStatue` cleanly execute `isVisitorNear` checks and return the active object or `null`. |
| **Wall & Pedestal Collision** | 10 pts | Uses compound boolean operators (`&&`, `||`, `!`) to enforce wall boundaries, open doorway walk-through access, and circular statue pedestal collision (`collidesWithVisitor`). |
| **Curatorial Quality** | 10 pts | Complete, insightful AP Art History entries for all 4 paintings and 2 sculptures with textures/models in `data/`. |
| **Total** | **100 pts** | **Mastery of AP CSA Units 1–3!** |
