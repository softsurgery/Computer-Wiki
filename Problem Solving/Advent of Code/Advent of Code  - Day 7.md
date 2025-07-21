```js
const fs = require("node:fs");
const path = require("node:path");

function readEquations(callback) {
  const fullPath = path.join(__dirname, "./equations.txt");
  fs.readFile(fullPath, "utf8", (err, data) => {
    if (err) {
      console.error("Error reading file:", err);
      return;
    }
    callback(data.trim());
  });
}

function detect(equations) {
  return equations.split("\\n").map((line) => line.trim());
}

function checkEquation(target, nums) {
  const slots = nums.length - 1;
  const totalCombos = 1 << slots; // 2^(n-1)
  for (let mask = 0; mask < totalCombos; mask++) {
    let value = nums[0];
    for (let i = 0; i < slots; i++) {
      if ((mask >> i) & 1) {
        value = value * nums[i + 1];
      } else {
        value = value + nums[i + 1];
      }
    }
    if (value === target) return true;
  }
  return false;
}

function solveAll(equations) {
  let total = 0;
  for (const line of equations) {
    const [targetPart, numsPart] = line.split(":");
    const target = Number(targetPart.trim());
    const nums = numsPart.trim().split(/\\s+/).map(Number);
    if (checkEquation(target, nums)) {
      total += target;
      console.log(`${target}: OK`);
    } else {
      console.log(`${target}: X`);
    }
  }
  return total;
}

readEquations((equations) => {
  const detectedEquations = detect(equations);
  console.log(solveAll(detectedEquations));
});

```

```js
const fs = require("node:fs");
const path = require("node:path");

Number.prototype.concat = function (n) {
  return Number(this.toString() + n.toString());
};

function readEquations(callback) {
  const fullPath = path.join(__dirname, "./equations.txt");
  fs.readFile(fullPath, "utf8", (err, data) => {
    if (err) {
      console.error("Error reading file:", err);
      return;
    }
    callback(data.trim());
  });
}

function detect(equations) {
  return equations.split("\\n").map((line) => line.trim());
}

function checkEquation(target, nums) {
  const slots = nums.length - 1;
  const totalCombos = 1 << slots; // 2^(n-1)
  for (let mask = 0; mask < totalCombos; mask++) {
    let value = nums[0];
    for (let i = 0; i < slots; i++) {
      if ((mask >> i) & 1) {
        value = value * nums[i + 1];
      } else {
        value = value + nums[i + 1];
      }
    }
    if (value === target) return true;
  }
  return false;
}

function checkEquation3(target, nums) {
  const slots = nums.length - 1;
  const totalCombos = Math.pow(3, slots);

  for (let mask = 0; mask < totalCombos; mask++) {
    let value = nums[0];
    let m = mask;

    for (let i = 0; i < slots; i++) {
      const op = m % 3; // base-3 digit
      m = Math.floor(m / 3);

      if (op === 0) {
        value = value + nums[i + 1];
      } else if (op === 1) {
        value = value.concat(nums[i + 1]);
      } else {
        value = value * nums[i + 1];
      }
    }

    if (value === target) return true;
  }

  return false;
}

function solveAll(equations) {
  let total = 0;
  let total3 = 0;
  for (const line of equations) {
    const [targetPart, numsPart] = line.split(":");
    const target = Number(targetPart.trim());
    const nums = numsPart.trim().split(/\\s+/).map(Number);
    if (checkEquation(target, nums)) {
      total += target;
      console.log(`${target}: OK`);
    } else {
      console.log(`${target}: X`);
    }
    if (checkEquation3(target, nums)) {
      total3 += target;
      console.log(`${target}: 3OK`);
    } else {
      console.log(`${target}: 3X`);
    }
  }
  return total3;
}

readEquations((equations) => {
  const detectedEquations = detect(equations);
  console.log(solveAll(detectedEquations));
});

```