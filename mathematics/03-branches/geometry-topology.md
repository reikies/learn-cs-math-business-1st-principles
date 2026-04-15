# Geometry & Topology — The Mathematics of Shape and Space

> Geometry measures: distances, angles, areas, curvature.
> Topology classifies: what's connected, what has holes, what can be deformed into what.
> Together they describe the shape of everything — from physical space to data manifolds.

**Prerequisites:** [Linear Algebra](../02-core-structures/linear-algebra.md), [Calculus](../02-core-structures/calculus.md), [Algebra](../02-core-structures/algebra.md)
**What it enables:** Physics (general relativity, gauge theory), computer graphics, robotics, data analysis (topological data analysis), manifold learning in ML.

---

## Part I: Geometry — Measuring Space

### 1. Euclidean Geometry — The Foundation

The geometry of flat space. Euclid's five postulates (300 BC):

1. A line can be drawn between any two points
2. A line segment can be extended indefinitely
3. A circle can be drawn with any center and radius
4. All right angles are equal
5. **The parallel postulate:** Through a point not on a line, exactly ONE parallel line exists

The fifth postulate is the interesting one. For 2,000 years, mathematicians tried to derive it from the other four. They failed — because it's independent. Changing it gives you entirely new geometries.

**Key results:**
- Pythagorean theorem: a² + b² = c²
- Triangle angle sum = 180°
- Area of circle = πr²
- These ALL depend on the parallel postulate (they fail in curved spaces)

### 2. Analytic Geometry — Coordinates

Descartes (1637): give every point coordinates. Now geometry IS algebra.

**The distance formula** (Pythagorean theorem with coordinates):
```
d = √((x₂-x₁)² + (y₂-y₁)²)
```

**Lines:** y = mx + b. **Circles:** x² + y² = r². **Conics:** ax² + bxy + cy² + dx + ey + f = 0.

This fusion of algebra and geometry was revolutionary — it meant geometric problems could be solved by computation, and algebraic equations could be visualized as shapes.

### 3. Non-Euclidean Geometry — Curved Space

Deny the parallel postulate:

| Geometry | Parallel postulate | Curvature | Surface |
|----------|-------------------|-----------|---------|
| **Euclidean** | Exactly one parallel | 0 (flat) | Plane |
| **Hyperbolic** | Infinitely many parallels | Negative | Saddle |
| **Spherical/Elliptic** | No parallels (all lines meet) | Positive | Sphere |

On a sphere:
- "Lines" are great circles (equator, meridians)
- Triangle angle sum > 180° (a triangle with three 90° angles exists!)
- No parallel lines — all great circles intersect

On a hyperbolic surface:
- Triangle angle sum < 180°
- Space "expands" faster than in flat space
- Hyperbolic geometry appears in special relativity and in hyperbolic embeddings used in ML for hierarchical data

### 4. Differential Geometry — Calculus on Curves and Surfaces

Uses calculus to study curved spaces. The central objects:

**Curvature of a curve:** How fast the tangent direction changes.
```
κ = |dT/ds|    (T = unit tangent, s = arc length)
```
A straight line has κ = 0. A circle of radius r has κ = 1/r.

**Gaussian curvature of a surface:** The product of the two principal curvatures.
- Sphere: K > 0 (curves same way in all directions)
- Saddle: K < 0 (curves opposite ways)
- Cylinder: K = 0 (flat in one direction)

**Gauss's Theorema Egregium ("Remarkable Theorem"):** Gaussian curvature is INTRINSIC — it can be measured by an ant living on the surface, without seeing the surrounding space. This is why you can't flatten an orange peel without tearing (K > 0 → K = 0 requires distortion), but you CAN flatten a cylinder (K = 0 already).

### 5. Riemannian Geometry — General Curved Spaces

A **Riemannian manifold** is a smooth space with a metric (rule for measuring distances) that can vary from point to point.

The **metric tensor** g defines distance:
```
ds² = Σ gᵢⱼ dxⁱ dxʲ
```

Flat space: ds² = dx² + dy² + dz² (Pythagorean theorem).
Curved space: the gᵢⱼ depend on position — distances stretch and compress.

**This is the language of general relativity.** Einstein's field equations:
```
Gμν = (8πG/c⁴) Tμν
```
The left side is curvature (geometry). The right side is matter/energy. "Matter tells space how to curve. Space tells matter how to move."

GPS satellites must account for this curvature — without general relativistic corrections, GPS would drift by ~10 km per day.

---

## Part II: Topology — The Qualitative Shape of Space

### 6. What Topology Studies

Topology studies properties preserved under continuous deformation (stretching, bending, but NOT tearing or gluing).

**Topologically equivalent (homeomorphic):**
- A coffee mug and a donut (both have one hole)
- A sphere and a cube (no holes)
- The letters C and U (both are open curves)

**NOT equivalent:**
- A sphere and a donut (different number of holes)
- A circle and a line (circle is closed, line is not)

The key property: **number of holes** (more precisely: topological invariants).

### 7. Key Topological Concepts

**Open and closed sets:** The foundation. A set is open if every point has a neighborhood contained in the set. Topology generalizes the notions of "near" and "continuous" without needing distance.

**Homeomorphism:** A continuous bijection with a continuous inverse. If two spaces are homeomorphic, they are topologically identical.

**Compactness:** Generalizes "closed and bounded." Compact spaces have nice properties — continuous functions achieve their maximum, sequences have convergent subsequences. The unit interval [0,1] is compact; (0,1) is not.

**Connectedness:** A space is connected if it can't be split into two separate open pieces. Path-connected: any two points can be joined by a continuous path.

### 8. Surfaces and Classification

Every closed surface (compact, no boundary, 2D) is topologically equivalent to one of:
- **Sphere** (genus 0 — no holes)
- **Torus** (genus 1 — one hole, like a donut)
- **Double torus** (genus 2 — two holes)
- **...**
- Plus non-orientable surfaces: Möbius strip, Klein bottle, projective plane

The **genus** (number of holes) completely classifies orientable surfaces. This is one of the cleanest classification results in mathematics.

**Euler characteristic:** For a surface divided into V vertices, E edges, F faces:
```
χ = V - E + F
```
- Sphere: χ = 2
- Torus: χ = 0
- Genus-g surface: χ = 2 - 2g

This is a topological invariant — it doesn't change no matter how you subdivide the surface.

### 9. The Fundamental Group

For each space, the **fundamental group** π₁ captures the topology of loops:
- Start at a base point, walk a loop, come back
- Two loops are "the same" if one can be continuously deformed into the other
- The group operation: do one loop then another

```
π₁(circle) = Z          (loops can wind around any integer number of times)
π₁(sphere) = {e}        (every loop can be shrunk to a point)
π₁(torus) = Z × Z       (two independent loops — around the hole, through the hole)
π₁(figure-8) = F₂       (free group on 2 generators — non-abelian!)
```

If two spaces have different fundamental groups, they're topologically different.

### 10. Higher-Dimensional Topology

**Manifolds:** Spaces that locally look like Rⁿ (even if globally they're curved or twisted).

- A circle is a 1-manifold (locally looks like a line)
- A sphere is a 2-manifold (locally looks like a plane)
- Spacetime is a 4-manifold (locally looks like R⁴)

**The Poincaré conjecture (proved 2003, Perelman):** Every simply-connected, closed 3-manifold is homeomorphic to the 3-sphere. The only millennium prize problem solved so far.

**Knot theory:** Which loops in 3D space can be untangled? Knot invariants classify knots. Applications: DNA topology, molecular chemistry, quantum computing.

---

## Part III: Connections to Technology

### 11. Computer Graphics

| Concept | How it's used |
|---------|--------------|
| **Bézier curves/surfaces** | Smooth curves defined by control points (differential geometry) |
| **Mesh topology** | Triangle meshes approximate surfaces; Euler characteristic checks validity |
| **Texture mapping** | Mapping 2D images onto 3D surfaces (conformal maps from complex analysis/geometry) |
| **Ray tracing** | Computing intersections with geometric primitives |
| **3D transformations** | Rotation, translation, projection — all via matrices in homogeneous coordinates |

### 12. Robotics

**Configuration space:** The space of all possible positions of a robot. A robot arm with n joints has an n-dimensional configuration manifold. Planning a collision-free path = finding a continuous curve in this manifold that avoids obstacles.

### 13. Data Analysis

**Topological Data Analysis (TDA):** Extract topological features (holes, voids, connected components) from data. **Persistent homology** identifies which features are "real" (persist across scales) vs. noise.

**Manifold hypothesis in ML:** High-dimensional data (images, text) often lies on a low-dimensional manifold. Techniques like t-SNE, UMAP, and autoencoders try to discover this manifold.

---

## What Geometry & Topology Powers

| Technology | Geometric/topological concept |
|------------|------------------------------|
| **GPS** | Riemannian geometry (general relativity corrections) |
| **Computer graphics** | Differential geometry, projective geometry, mesh topology |
| **Robotics** | Configuration spaces, path planning on manifolds |
| **Physics** | General relativity, gauge theory, string theory |
| **ML (manifold learning)** | Data lives on low-dimensional manifolds |
| **TDA** | Persistent homology for shape-of-data analysis |
| **Network analysis** | Topological properties of graphs |
| **Molecular biology** | Knot theory for DNA, protein folding geometry |

---

## Key Insight

Geometry tells you the QUANTITATIVE shape of things — distances, angles, curvature. Topology tells you the QUALITATIVE shape — connectedness, holes, what can be deformed into what. The remarkable insight of modern mathematics and physics is that these aren't just descriptions of shapes we can see, but of abstract spaces we can't — configuration spaces of robots, parameter spaces of models, spacetime itself. The shape of the space you're working in determines what's possible within it.

---

**Next:** [Optimization](../04-applied/optimization.md) — finding the best answer in a landscape of possibilities.
