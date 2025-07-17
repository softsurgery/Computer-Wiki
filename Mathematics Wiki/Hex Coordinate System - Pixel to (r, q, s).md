This note explains how to convert **2D pixel coordinates (x, y)** to **hexagonal grid coordinates** using the **cube coordinate system** `(r, q, s)`.

---

## 📐 Coordinate System

- `r`, `q`, `s` are cube coordinates for hex grids.
- They follow the rule: `r + q + s = 0`.

---

## 🧩 Grid Orientation

The conversion depends on the hex orientation:

- **Pointy-Topped Hexes:** flat sides on left & right.
- **Flat-Topped Hexes:** flat sides on top & bottom.

---

## ✅ Conversion Formulas

### 1️⃣ Pointy-Topped Hexes

**Formulas:**
```
q = (√3 / 3 * x - 1/3 * y) / size
r = (2/3 * y) / size
s = -q - r
```
---

### 2️⃣ Flat-Topped Hexes

**Formulas:**
```
q = (2/3 * x) / size
r = (-1/3 * x + √3/3 * y) / size
s = -q - r
```
---

## 🧮 Example in JavaScript

```javascript
function pixelToHex(x, y, size) {
  const q = (Math.sqrt(3) / 3 * x - 1 / 3 * y) / size;
  const r = (2 / 3 * y) / size;
  const s = -q - r;
  return { r, q, s };
}
```

🗒️ Notes
- Size is the hex radius (distance from center to corner) in pixels.
- Always verify r + q + s = 0 to check your math.
- This system is useful for axial/cube hex grid algorithms.

References: Red Blob Games – Hexagonal Grids