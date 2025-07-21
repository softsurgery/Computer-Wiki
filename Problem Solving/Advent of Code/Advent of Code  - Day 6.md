```js
const fs = require("node:fs");
const path = require("node:path");

const anchors = ["^", "v", "<", ">"];

function incldueAnchor(row) {
  let index = -1;
  for (const anchor of anchors) {
    index = row.indexOf(anchor);
    if (index !== -1) {
      return {
        hasAnchor: anchor !== -1,
        anchorIndex: index,
        pivot: ">",
      };
    }
  }
  return undefined;
}

function detect(map) {
  const rows = map.split("\\n").map((line) => line.trim().split(""));
  let detection;
  for (let xi = 0; xi < rows.length; xi++) {
    detection = incldueAnchor(rows[xi]);
    if (detection)
      return {
        rows,
        xi,
        yi: detection.anchorIndex,
      };
  }
}

function getMap(rows) {
  return rows.map((row) => row.join("")).join("\\n");
}

function nextDirection(direction) {
  if (direction == "^") return ">";
  else if (direction == ">") return "v";
  else if (direction == "v") return "<";
  else return "^";
}

Number.prototype.between = function (a, b) {
  return a <= this && this <= b;
};

function inBound(x, y, rows) {
  return x.between(0, rows.length - 1) && y.between(0, rows[0].length - 1);
}

function nextPosition(rows, x, y, inbound) {
  const next = {
    x,
    y,
    rows,
    inbound,
  };

  if (!inbound) return next;

  if (rows[x][y] == "^") {
    if (rows[x - 1][y] != "#") {
      next.x = x - 1;
      next.inbound = inBound(next.x, next.y, rows);
      if (next.inbound) {
        next.rows[x][y] = "X";
        next.rows[x - 1][y] = "^";
      }
    } else {
      const newPivot = nextDirection(rows[x][y]);
      next.rows[x][y] = newPivot;
    }
  } else if (rows[x][y] == ">") {
    if (rows[x][y + 1] != "#") {
      next.y = y + 1;
      next.inbound = inBound(next.x, next.y, rows);
      if (next.inbound) {
        next.rows[x][y] = "X";
        next.rows[x][y + 1] = ">";
      }
    } else {
      const newPivot = nextDirection(rows[x][y]);
      next.rows[x][y] = newPivot;
    }
  } else if (rows[x][y] == "v") {
    if (rows[x + 1][y] != "#") {
      next.x = x + 1;
      next.inbound = inBound(next.x, next.y, rows);
      if (next.inbound) {
        next.rows[x][y] = "X";
        next.rows[x + 1][y] = "v";
      }
    } else {
      const newPivot = nextDirection(rows[x][y]);
      next.rows[x][y] = newPivot;
    }
  } else if (rows[x][y] == "<") {
    if (rows[x][y - 1] != "#") {
      next.y = y - 1;
      next.inbound = inBound(next.x, next.y, rows);
      if (next.inbound) {
        next.rows[x][y] = "X";
        next.rows[x][y - 1] = "<";
      }
    } else {
      const newPivot = nextDirection(rows[x][y]);
      next.rows[x][y] = newPivot;
    }
  }

  return next;
}

function readMap(callback) {
  const fullPath = path.join(__dirname, "./map.txt");
  fs.readFile(fullPath, "utf8", (err, data) => {
    if (err) {
      console.error("Error reading file:", err);
      return;
    }
    const map = data.trim();
    callback(map);
  });
}

readMap((map) => {
  let { rows, xi, yi } = detect(map);
  let next = nextPosition(rows, xi, yi, true);
  let i = 0;
  try {
    while (next.inbound) {
      console.log(`\\n map ${i} : \\n`);
      console.log(getMap(next.rows));
      i++;
      next = nextPosition(next.rows, next.x, next.y, next.inbound);
    }
  } catch (e) {
    console.log((getMap(next.rows).match(/X/g) || []).length + 1);
  }
});

```

```js
const fs = require("node:fs");
const path = require("node:path");

const anchors = ["^", "v", "<", ">"];

function incldueAnchor(row) {
  let index = -1;
  for (const anchor of anchors) {
    index = row.indexOf(anchor);
    if (index !== -1) {
      return {
        hasAnchor: anchor !== -1,
        anchorIndex: index,
        pivot: ">",
      };
    }
  }
  return undefined;
}

function detect(map) {
  const rows = map.split("\\n").map((line) => line.trim().split(""));
  let detection;
  for (let xi = 0; xi < rows.length; xi++) {
    detection = incldueAnchor(rows[xi]);
    if (detection)
      return {
        rows,
        xi,
        yi: detection.anchorIndex,
      };
  }
}

function getMap(rows) {
  return rows.map((row) => row.join("")).join("\\n");
}

function nextDirection(direction) {
  if (direction == "^") return ">";
  else if (direction == ">") return "v";
  else if (direction == "v") return "<";
  else return "^";
}

Number.prototype.between = function (a, b) {
  return a <= this && this <= b;
};

function inBound(x, y, rows) {
  return x.between(0, rows.length - 1) && y.between(0, rows[0].length - 1);
}

function nextPosition(rows, x, y, inbound, inLoop, patterns = []) {
  const next = {
    x,
    y,
    rows,
    inbound,
    inLoop,
    patterns,
  };

  if (!inbound) return next;
  if (inLoop) return next;

  if (rows[x][y] == "^") {
    if (rows[x - 1][y] != "#") {
      next.x = x - 1;
      next.inbound = inBound(next.x, next.y, rows);
      if (next.inbound) {
        next.rows[x][y] = "X";
        next.rows[x - 1][y] = "^";
      }
    } else {
      const newPivot = nextDirection(rows[x][y]);
      next.rows[x][y] = newPivot;
      patterns.push(`${x},${y},${newPivot}`);
    }
  } else if (rows[x][y] == ">") {
    if (rows[x][y + 1] != "#") {
      next.y = y + 1;
      next.inbound = inBound(next.x, next.y, rows);
      if (next.inbound) {
        next.rows[x][y] = "X";
        next.rows[x][y + 1] = ">";
      }
    } else {
      const newPivot = nextDirection(rows[x][y]);
      next.rows[x][y] = newPivot;
      patterns.push(`${x},${y},${newPivot}`);
    }
  } else if (rows[x][y] == "v") {
    if (rows[x + 1][y] != "#") {
      next.x = x + 1;
      next.inbound = inBound(next.x, next.y, rows);
      if (next.inbound) {
        next.rows[x][y] = "X";
        next.rows[x + 1][y] = "v";
      }
    } else {
      const newPivot = nextDirection(rows[x][y]);
      next.rows[x][y] = newPivot;
      patterns.push(`${x},${y},${newPivot}`);
    }
  } else if (rows[x][y] == "<") {
    if (rows[x][y - 1] != "#") {
      next.y = y - 1;
      next.inbound = inBound(next.x, next.y, rows);
      if (next.inbound) {
        next.rows[x][y] = "X";
        next.rows[x][y - 1] = "<";
      }
    } else {
      const newPivot = nextDirection(rows[x][y]);
      next.rows[x][y] = newPivot;
      patterns.push(`${x},${y},${newPivot}`);
    }
  }

  next.patterns = patterns;
  next.inLoop = patterns.length !== new Set(patterns).size;

  return next;
}

function readMap(callback) {
  const fullPath = path.join(__dirname, "./map.txt");
  fs.readFile(fullPath, "utf8", (err, data) => {
    if (err) {
      console.error("Error reading file:", err);
      return;
    }
    const map = data.trim();
    callback(map);
  });
}

function play(map) {
  let { rows, xi, yi } = detect(map);
  let next = nextPosition(rows, xi, yi, true, false, []);
  let i = 0;
  try {
    while (next.inbound && !next.inLoop) {
      // console.log(`\\n map ${i} : \\n`);
      // console.log(getMap(next.rows));
      i++;
      next = nextPosition(
        next.rows,
        next.x,
        next.y,
        next.inbound,
        next.inLoop,
        next.patterns
      );
      console.log(next.inLoop);
    }
  } catch (e) {}
  console.log((getMap(next.rows).match(/X/g) || []).length + 1);
  return next;
}

function loopPlay(map, i, j) {
  let { rows, xi, yi } = detect(map);
  if (rows[i][j] == ".") rows[i][j] = "#";

  console.log(`TEST ${i}, ${j}`);
  console.log(getMap(rows));

  let next = nextPosition(rows, xi, yi, true, false, []);
  try {
    while (next.inbound && !next.inLoop) {
      // console.log(`\\n map ${i} : \\n`);
      // console.log(getMap(next.rows));
      next = nextPosition(
        next.rows,
        next.x,
        next.y,
        next.inbound,
        next.inLoop,
        next.patterns
      );
    }
  } catch (e) {}
  console.log("\\n", getMap(next.rows), "\\n");
  return next;
}

function howManyLoops(map) {
  let count = 0;
  for (let i = 0; i < 10; i++) {
    for (let j = 0; j < 10; j++) {
      next = loopPlay(map, i, j);
      if (!next) continue;
      else if (!next.inbound) continue;
      else if (next.inLoop) {
        count++;
      }
      delete next.rows;
      console.log(next);
    }
  }
  console.log(count);
  return count;
}

readMap((map) => howManyLoops(map));

```

```js

//light brut force
const fs = require("node:fs");
const path = require("node:path");

const anchors = ["^", "v", "<", ">"];
const movement = {
  "^": [-1, 0],
  v: [1, 0],
  "<": [0, -1],
  ">": [0, 1],
};

function incldueAnchor(row) {
  let index = -1;
  for (const anchor of anchors) {
    index = row.indexOf(anchor);
    if (index !== -1) {
      return {
        hasAnchor: true,
        anchorIndex: index,
        pivot: anchor,
      };
    }
  }
  return undefined;
}

function detect(map) {
  const rows = map.split("\\n").map((line) => line.trim().split(""));
  let detection;
  for (let xi = 0; xi < rows.length; xi++) {
    detection = incldueAnchor(rows[xi]);
    if (detection)
      return {
        rows,
        xi,
        yi: detection.anchorIndex,
        pivot: detection.pivot,
      };
  }
}

Number.prototype.between = function (a, b) {
  return a <= this && this <= b;
};

function inBound(x, y, rows) {
  return x.between(0, rows.length - 1) && y.between(0, rows[0].length - 1);
}

function nextDirection(direction) {
  if (direction == "^") return ">";
  if (direction == ">") return "v";
  if (direction == "v") return "<";
  return "^";
}

function simulate(rows, x, y, pivot) {
  const visited = new Set();
  let inbound = true;

  while (inbound) {
    const key = `${x},${y},${pivot}`;
    if (visited.has(key)) {
      return true; // loop detected!
    }
    visited.add(key);

    const [dx, dy] = movement[pivot];
    const nx = x + dx;
    const ny = y + dy;

    if (!inBound(nx, ny, rows)) break;

    if (rows[nx][ny] !== "#") {
      x = nx;
      y = ny;
    } else {
      pivot = nextDirection(pivot);
    }
  }

  return false; // guard exited the map
}

function findLoopObstructions(map) {
  const { rows, xi, yi, pivot } = detect(map);

  // Find all reachable positions
  const reachable = new Set();
  const visited = new Set();
  let queue = [{ x: xi, y: yi, pivot }];

  while (queue.length > 0) {
    const { x, y, pivot } = queue.pop();
    const key = `${x},${y},${pivot}`;
    if (visited.has(key)) continue;
    visited.add(key);
    reachable.add(`${x},${y}`);

    const [dx, dy] = movement[pivot];
    const nx = x + dx;
    const ny = y + dy;

    if (inBound(nx, ny, rows) && rows[nx][ny] !== "#") {
      queue.push({ x: nx, y: ny, pivot });
    } else {
      const newPivot = nextDirection(pivot);
      queue.push({ x, y, pivot: newPivot });
    }
  }

  // Try adding a wall at each reachable empty cell (except starting)
  let count = 0;
  reachable.forEach((pos) => {
    const [x, y] = pos.split(",").map(Number);
    if (x === xi && y === yi) return; // cannot block starting cell
    if (rows[x][y] !== ".") return;

    const copy = rows.map((row) => [...row]);
    copy[x][y] = "#";

    if (simulate(copy, xi, yi, pivot)) {
      count++;
    }
  });

  console.log(`Possible new obstructions causing loops: ${count}`);
  return count;
}

function readMap(callback) {
  const fullPath = path.join(__dirname, "./map.txt");
  fs.readFile(fullPath, "utf8", (err, data) => {
    if (err) {
      console.error("Error reading file:", err);
      return;
    }
    callback(data.trim());
  });
}

readMap(findLoopObstructions);

```