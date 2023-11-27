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
      adjacencyList[start].sort((a, b) => a - b);
      adjacencyList[end].sort((a, b) => a - b);
    }

    return adjacencyList;
  };
}

function dfs(start, adjacencyList) {
  if (!Array.isArray(adjacencyList[start])) {
    return [start];
  }

  const result = [];
  const visited = {};

  let vertices = Object.keys(adjacencyList).map((vertex) => {
    return Number(vertex);
  });

  const startIndex = vertices.indexOf(start);

  vertices = vertices
    .slice(startIndex, vertices.length)
    .concat(vertices.slice(0, startIndex));

  function recution(vertices) {
    for (const vertex of vertices) {
      if (!visited[vertex]) {
        visited[vertex] = true;
        result.push(vertex);

        recution(adjacencyList[vertex]);
      }
    }
  }
  recution(vertices);

  return result;
}

function solve() {
  const firstLine = readArray();
  const verticesQty = firstLine[0];
  const edgesQty = firstLine[1];
  const matrix = readMatrix(edgesQty);
  const firstVertex = readInt();

  const getAdjacencyList = createAdjacencyListInstance();
  for (const edge of matrix) {
    getAdjacencyList(edge[0], edge[1]);
  }

  const result = dfs(firstVertex, getAdjacencyList());

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

function readMatrix(rowsCount) {
  var arr = [];
  for (let i = 0; i !== rowsCount; i++) {
    arr.push(readArray());
  }
  return arr;
}

function readInt() {
  const n = Number(_inputLines[_curLine]);
  _curLine++;
  return n;
}
```
