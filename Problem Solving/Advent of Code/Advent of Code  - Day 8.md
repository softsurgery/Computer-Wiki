```js
const fs = require("node:fs");
const path = require("node:path");

Number.prototype.between = function (a, b) {
  return a <= this && this <= b;
};

function readField(callback) {
  const fullPath = path.join(__dirname, "./field.txt");
  fs.readFile(fullPath, "utf8", (err, data) => {
    if (err) {
      console.error("Error reading file:", err);
      return;
    }
    callback(data.trim());
  });
}

function mapField(field) {
  return field.split("\\n").map((line) => line.trim().split(""));
}

function searchForAllPositions(antenna, field) {
  return field
    .map((row, y) =>
      row.map((cell, x) => (cell === antenna ? { x, y } : null)).filter(Boolean)
    )
    .flat();
}

function findSymmetry(origin, dot) {
  const diffX = dot.x - origin.x;
  const diffY = dot.y - origin.y;

  return {
    x: origin.x - diffX,
    y: origin.y - diffY,
  };
}

function getAntinodes(positions, maxX, maxY) {
  const antinodes = new Set();

  for (let i = 0; i < positions.length; i++) {
    for (let j = i + 1; j < positions.length; j++) {
      const sym1 = findSymmetry(positions[i], positions[j]);
      const sym2 = findSymmetry(positions[j], positions[i]);

      if (sym1.x.between(0, maxX) && sym1.y.between(0, maxY)) {
        antinodes.add(`${sym1.x},${sym1.y}`);
      }
      if (sym2.x.between(0, maxX) && sym2.y.between(0, maxY)) {
        antinodes.add(`${sym2.x},${sym2.y}`);
      }
    }
  }

  return antinodes;
}

function getAllAntinodes(mappedField, maxX, maxY) {
  const allAntinodes = new Set();
  const seenFrequencies = new Set();

  for (let y = 0; y <= maxY; y++) {
    for (let x = 0; x <= maxX; x++) {
      const freq = mappedField[y][x];
      if (freq !== "." && !seenFrequencies.has(freq)) {
        seenFrequencies.add(freq);

        const positions = searchForAllPositions(freq, mappedField);
        const localAntinodes = getAntinodes(positions, maxX, maxY);

        for (const pos of localAntinodes) {
          allAntinodes.add(pos);
        }
      }
    }
  }

  return allAntinodes.size;
}

readField((field) => {
  const mappedField = mapField(field);
  const maxY = mappedField.length - 1;
  const maxX = mappedField[0].length - 1;

  const count = getAllAntinodes(mappedField, maxX, maxY);
  console.log("Unique antinodes:", count);
});

```

```js
const fs = require("node:fs");
const path = require("node:path");

Number.prototype.between = function (a, b) {
  return a <= this && this <= b;
};

function readField(callback) {
  const fullPath = path.join(__dirname, "./field.txt");
  fs.readFile(fullPath, "utf8", (err, data) => {
    if (err) {
      console.error("Error reading file:", err);
      return;
    }
    callback(data.trim());
  });
}

function mapField(field) {
  return field.split("\\n").map((line) => line.trim().split(""));
}

function searchForAllPositions(antenna, field) {
  return field
    .map((row, y) =>
      row.map((cell, x) => (cell === antenna ? { x, y } : null)).filter(Boolean)
    )
    .flat();
}

function getGCD(a, b) {
  a = Math.abs(a);
  b = Math.abs(b);
  while (b) {
    const temp = b;
    b = a % b;
    a = temp;
  }
  return a;
}

function getLineCoordinates(dot1, dot2, maxX, maxY) {
  const dx = dot2.x - dot1.x;
  const dy = dot2.y - dot1.y;

  const gcd = getGCD(dx, dy);
  const stepX = dx / gcd;
  const stepY = dy / gcd;

  const points = new Set();

  // Start from one end and go both ways (forward and back)
  let x = dot1.x - stepX;
  let y = dot1.y - stepY;
  while (x.between(0, maxX) && y.between(0, maxY)) {
    points.add(`${x},${y}`);
    x -= stepX;
    y -= stepY;
  }

  x = dot1.x;
  y = dot1.y;
  while (x.between(0, maxX) && y.between(0, maxY)) {
    points.add(`${x},${y}`);
    x += stepX;
    y += stepY;
  }

  return points;
}

function getAntinodes(positions, maxX, maxY) {
  const antinodes = new Set();

  for (let i = 0; i < positions.length; i++) {
    for (let j = i + 1; j < positions.length; j++) {
      const points = getLineCoordinates(positions[i], positions[j], maxX, maxY);
      for (const point of points) {
        antinodes.add(point);
      }
    }
  }

  return antinodes;
}

function getAllAntinodes(mappedField, maxX, maxY) {
  const allAntinodes = new Set();
  const seenFrequencies = new Set();

  for (let y = 0; y <= maxY; y++) {
    for (let x = 0; x <= maxX; x++) {
      const freq = mappedField[y][x];
      if (freq !== "." && !seenFrequencies.has(freq)) {
        seenFrequencies.add(freq);

        const positions = searchForAllPositions(freq, mappedField);
        const localAntinodes = getAntinodes(positions, maxX, maxY);

        for (const pos of localAntinodes) {
          allAntinodes.add(pos);
        }
      }
    }
  }
  return allAntinodes.size;
}

readField((field) => {
  const mappedField = mapField(field);
  const maxY = mappedField.length - 1;
  const maxX = mappedField[0].length - 1;

  const count = getAllAntinodes(mappedField, maxX, maxY);
  console.log("Unique antinodes:", count);
});

```