```js
const fs = require("node:fs");
const path = require("node:path");

function readNumbers(callback) {
  const fullPath = path.join(__dirname, "./numbers.txt");
  fs.readFile(fullPath, "utf8", (err, data) => {
    if (err) {
      console.error("Error reading file:", err);
      return;
    }
    callback(data.trim());
  });
}

const numeric = [
  ["7", "8", "9"],
  ["4", "5", "6"],
  ["1", "2", "3"],
  [null, "0", "A"],
];

const moves = [
  [null, "^", "A"],
  ["<", "v", ">"],
];

function getCoords(char, keypad) {
  for (let x = 0; x < keypad.length; x++) {
    for (let y = 0; y < keypad[x].length; y++) {
      if (keypad[x][y] === char) return { x, y };
    }
  }
  return null;
}

function move(x, y, xd, yd, keypad) {
  function tryPath(first, second) {
    let moves = [];
    let xi = x;
    let yi = y;

    while (xi !== xd || yi !== yd) {
      if (first === "y") {
        if (yi < yd) {
          yi++;
          moves.push(">");
        } else if (yi > yd) {
          yi--;
          moves.push("<");
        } else if (xi < xd) {
          xi++;
          moves.push("v");
        } else if (xi > xd) {
          xi--;
          moves.push("^");
        }
      } else {
        if (xi < xd) {
          xi++;
          moves.push("v");
        } else if (xi > xd) {
          xi--;
          moves.push("^");
        } else if (yi < yd) {
          yi++;
          moves.push(">");
        } else if (yi > yd) {
          yi--;
          moves.push("<");
        }
      }

      if (
        keypad[xi] === undefined ||
        keypad[xi][yi] === undefined ||
        keypad[xi][yi] == null
      ) {
        return null; // path hits forbidden
      }
    }

    return moves;
  }

  // First try: Y-first
  let path = tryPath("y", "x");
  if (path !== null) return path;

  // Second try: X-first
  path = tryPath("x", "y");
  if (path !== null) return path;

  throw new Error("No valid path found!"); // Should never happen for valid keypad
}

function moveRobot(code, keypad) {
  let initial = getCoords("A", keypad);
  let movement = "";
  for (const c of code) {
    const coord = getCoords(c, keypad);
    movement +=
      move(initial.x, initial.y, coord.x, coord.y, keypad).join("") + "A";
    initial = { x: coord.x, y: coord.y };
  }
  return movement;
}

function moveThreeRobots(code) {
  return moveRobot(moveRobot(moveRobot(code, numeric), moves), moves);
}

function splitNumbers(numbers) {
  return numbers.split("\\n").map((e) => e.trim());
}

function moveThreeRobotsMultiple(codes) {
  let count = 0;
  for (let code of codes) {
    const res = moveThreeRobots(code);
    console.log(
      code,
      ": ",
      // moveRobot(code, numeric),
      " |",
      // moveRobot(moveRobot(code, numeric), moves),
      " |",
      res,
      res.length,
      " ",
      parseInt(code)
    );
    count += parseInt(code) * res.length;
  }
  return count;
}

readNumbers((text) => {
  const codes = splitNumbers(text);
  console.log(moveThreeRobotsMultiple(codes));
});

```

## Mixed

```js
const fs = require("node:fs");
const path = require("node:path");

const numeric = [
  ["7", "8", "9"],
  ["4", "5", "6"],
  ["1", "2", "3"],
  [null, "0", "A"],
];

const moves = [
  [null, "^", "A"],
  ["<", "v", ">"],
];

function getCoords(char, keypad) {
  for (let x = 0; x < keypad.length; x++) {
    for (let y = 0; y < keypad[x].length; y++) {
      if (keypad[x][y] === char) return { x, y };
    }
  }
  return null;
}

// ✅ Your GREEDY move with fallback
function moveGreedy(x, y, xd, yd, keypad) {
  function tryPath(first) {
    let moves = [];
    let xi = x;
    let yi = y;

    while (xi !== xd || yi !== yd) {
      if (first === "y") {
        if (yi < yd) {
          yi++;
          moves.push(">");
        } else if (yi > yd) {
          yi--;
          moves.push("<");
        } else if (xi < xd) {
          xi++;
          moves.push("v");
        } else if (xi > xd) {
          xi--;
          moves.push("^");
        }
      } else {
        // first === "x"
        if (xi < xd) {
          xi++;
          moves.push("v");
        } else if (xi > xd) {
          xi--;
          moves.push("^");
        } else if (yi < yd) {
          yi++;
          moves.push(">");
        } else if (yi > yd) {
          yi--;
          moves.push("<");
        }
      }
      if (
        xi < 0 ||
        xi >= keypad.length ||
        yi < 0 ||
        yi >= keypad[xi].length ||
        keypad[xi][yi] == null
      ) {
        return null; // fail
      }
    }
    return moves;
  }

  let path = tryPath("y");
  if (path) return path;

  path = tryPath("x");
  if (path) return path;

  throw new Error("No valid path!");
}

// ✅ True BFS shortest move
function moveBFS(x, y, xd, yd, keypad) {
  const queue = [{ x, y, path: [] }];
  const seen = new Set([`${x},${y}`]);

  const directions = [
    { dx: -1, dy: 0, move: "^" },
    { dx: 1, dy: 0, move: "v" },
    { dx: 0, dy: -1, move: "<" },
    { dx: 0, dy: 1, move: ">" },
  ];

  while (queue.length > 0) {
    const { x, y, path } = queue.shift();

    if (x === xd && y === yd) return path;

    for (const { dx, dy, move } of directions) {
      const nx = x + dx;
      const ny = y + dy;

      if (
        nx >= 0 &&
        nx < keypad.length &&
        ny >= 0 &&
        ny < keypad[nx].length &&
        keypad[nx][ny] != null
      ) {
        const key = `${nx},${ny}`;
        if (!seen.has(key)) {
          seen.add(key);
          queue.push({ x: nx, y: ny, path: [...path, move] });
        }
      }
    }
  }
  throw new Error("No valid BFS path!");
}

function moveRobot(code, keypad, moveFn) {
  let pos = getCoords("A", keypad);
  let sequence = "";
  for (const c of code) {
    const target = getCoords(c, keypad);
    sequence += moveFn(pos.x, pos.y, target.x, target.y, keypad).join("") + "A";
    pos = target;
  }
  return sequence;
}

function moveThreeRobots(code, moveFn) {
  return moveRobot(
    moveRobot(moveRobot(code, numeric, moveFn), moves, moveFn),
    moves,
    moveFn
  );
}

function splitNumbers(numbers) {
  return numbers.split("\\n").map((e) => e.trim());
}

function runBoth(codes) {
  let sumGreedy = 0;
  let sumBFS = 0;
  let bestSum = 0;

  for (const code of codes) {
    const numericPart = parseInt(code);

    const resGreedy = moveThreeRobots(code, moveGreedy);
    const lenGreedy = resGreedy.length;

    const resBFS = moveThreeRobots(code, moveBFS);
    const lenBFS = resBFS.length;

    sumGreedy += numericPart * lenGreedy;
    sumBFS += numericPart * lenBFS;
    bestSum += numericPart * (lenBFS < lenGreedy ? lenBFS : lenGreedy);

    console.log(`\\n${code}`);
    console.log(` GREEDY: ${resGreedy} [${lenGreedy}]`);
    console.log(`    BFS: ${resBFS} [${lenBFS}]`);
    console.log(
      `  Mixed: ${resGreedy.length < resBFS.length ? resGreedy : resBFS} [${
        resGreedy.length < resBFS.length ? resGreedy : resBFS
      }]`
    );
  }

  console.log(`\\nTotal GREEDY: ${sumGreedy}`);
  console.log(`Total BFS:    ${sumBFS}`);
  console.log(`Total Mixed:  ${bestSum}`);
}

const fullPath = path.join(__dirname, "./numbers.txt");
fs.readFile(fullPath, "utf8", (err, data) => {
  if (err) throw err;
  const codes = splitNumbers(data);
  runBoth(codes);
});

```