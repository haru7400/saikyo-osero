# saikyo-osero<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<title>オセロ</title>
<style>
  body {
    font-family: sans-serif;
    text-align: center;
    background: #2e7d32;
    color: white;
  }
  #board {
    display: grid;
    grid-template-columns: repeat(8, 60px);
    grid-template-rows: repeat(8, 60px);
    gap: 2px;
    margin: 20px auto;
  }
  .cell {
    background: #388e3c;
    cursor: pointer;
    position: relative;
  }
  .stone {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    margin: 5px;
  }
  .black { background: black; }
  .white { background: white; }
</style>
</head>
<body>

<h1>🟢 オセロ 🟢</h1>
<p id="turn">黒の番</p>

<div id="board"></div>

<script>
const SIZE = 8;
const board = [];
let current = 1; // 1=黒, -1=白

const boardDiv = document.getElementById("board");
const turnText = document.getElementById("turn");

function init() {
  for (let y = 0; y < SIZE; y++) {
    board[y] = Array(SIZE).fill(0);
    for (let x = 0; x < SIZE; x++) {
      const cell = document.createElement("div");
      cell.className = "cell";
      cell.onclick = () => move(x, y);
      boardDiv.appendChild(cell);
    }
  }
  board[3][3] = board[4][4] = -1;
  board[3][4] = board[4][3] = 1;
  render();
}

function render() {
  [...boardDiv.children].forEach((cell, i) => {
    cell.innerHTML = "";
    const x = i % SIZE;
    const y = Math.floor(i / SIZE);
    if (board[y][x] !== 0) {
      const stone = document.createElement("div");
      stone.className = "stone " + (board[y][x] === 1 ? "black" : "white");
      cell.appendChild(stone);
    }
  });
  turnText.textContent = current === 1 ? "黒の番" : "白の番";
}

function move(x, y) {
  if (board[y][x] !== 0) return;
  const flips = getFlips(x, y);
  if (flips.length === 0) return;

  board[y][x] = current;
  flips.forEach(([fx, fy]) => board[fy][fx] = current);
  current *= -1;
  render();
}

function getFlips(x, y) {
  const dirs = [
    [1,0],[-1,0],[0,1],[0,-1],
    [1,1],[1,-1],[-1,1],[-1,-1]
  ];
  let result = [];

  for (const [dx, dy] of dirs) {
    let nx = x + dx, ny = y + dy;
    let temp = [];
    while (nx >= 0 && ny >= 0 && nx < SIZE && ny < SIZE && board[ny][nx] === -current) {
      temp.push([nx, ny]);
      nx += dx;
      ny += dy;
    }
    if (temp.length && nx >= 0 && ny >= 0 && nx < SIZE && ny < SIZE && board[ny][nx] === current) {
      result = result.concat(temp);
    }
  }
  return result;
}

init();
</script>

</body>
</html>
