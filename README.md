```
const _readline = require("readline");

const _reader = _readline.createInterface({
  input: process.stdin,
});

const _inputLines = [];
let _curLine = 0;

_reader.on("line", (line) => {
  _inputLines.push(line);
});

process.stdin.on("end", solve);

function dfs(start, adjacencyList) {
  if (!Array.isArray(adjacencyList[start])) {
    return [start];
  }

  const result = [];
  const stack = [start];
  const visited = {};
  visited[start] = true;
  let currentVertex;

  while (stack.length) {
    currentVertex = stack.pop();
    result.push(currentVertex);

    for (let i = 0; i < adjacencyList[currentVertex].length; i++) {
      const neighbor = adjacencyList[currentVertex][i];
      if (!visited[neighbor]) {
        visited[neighbor] = true;
        stack.push(neighbor);
      }
    }
  }

  return result;
}

function solve() {
  const firstLine = readArray();
  const verticesQty = firstLine[0];
  const edgesQty = firstLine[1];
  const adjacencyList = readAdjacencyList(edgesQty);
  const firstVertex = readInt();

  const result = dfs(firstVertex, adjacencyList);

  process.stdout.write(result.join(" "));
}

function readArray() {
  var arr = _inputLines[_curLine]
    .trim(" ")
    .split(" ")
    .map((num) => Number(num));
  _curLine++;
  return arr;
}

function readInt() {
  const n = Number(_inputLines[_curLine]);
  _curLine++;
  return n;
}

function createAdjacencyListInstance() {
  const adjacencyList = {};

  return function (start, end) {
    if (Number.isInteger(start) && Number.isInteger(end)) {
      if (!adjacencyList[start]) {
        adjacencyList[start] = [];
      }
      if (!adjacencyList[end]) {
        adjacencyList[end] = [];
      }
      adjacencyList[start].push(end);
      adjacencyList[end].push(start);
      adjacencyList[start].sort((a, b) => b - a);
      adjacencyList[end].sort((a, b) => b - a);
    }

    return adjacencyList;
  };
}

function readAdjacencyList(edgesQty) {
  const getAdjacencyList = createAdjacencyListInstance();

  for (let i = 0; i < edgesQty; i++) {
    const edge = readArray();
    getAdjacencyList(edge[0], edge[1]);
  }

  return getAdjacencyList();
}
```
