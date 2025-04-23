<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>📐 Equivalent Resistance Using Graph Theory</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/10.0.0/math.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      background-color: #f7f9fa;
      min-height: 100vh;
    }
    .container {
      max-width: 960px;
      background: white;
      padding: 40px;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
      border-radius: 10px;
    }
    h1, h2, h3 {
      color: #2c3e50;
    }
    code {
      background: #eee;
      padding: 2px 6px;
      border-radius: 4px;
    }
    pre {
      background: #f4f4f4;
      padding: 10px;
      border-radius: 6px;
      overflow-x: auto;
    }
    #result {
      font-weight: bold;
      color: #1e8449;
    }
    button {
      margin-top: 10px;
      padding: 10px 20px;
      background: #3498db;
      border: none;
      color: white;
      border-radius: 5px;
      cursor: pointer;
    }
    #circuitGraph {
      margin-top: 30px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>📐 Equivalent Resistance Using Graph Theory</h1>

    <h2>🧠 Motivation</h2>
    <p>
      Complex resistor networks can be tough to simplify using only series and parallel rules. Graph theory gives us a smarter way. Each node is a junction, and each resistor is an edge with a weight (the resistance).
    </p>
    <p>
      With graph algorithms, we can reduce even complicated networks to one resistance value—fast and with automation in mind.
    </p>

    <h2>🛠️ How It Works</h2>
    <ol>
      <li>Represent the circuit as a graph (nodes and weighted edges).</li>
      <li>Find simple series (single path between 2 nodes) and merge.</li>
      <li>Find parallel connections (multiple edges between same two nodes) and merge.</li>
      <li>Repeat until only one edge remains between source and sink.</li>
    </ol>

    <h3>📄 Pseudocode</h3>
    <pre>
Repeat until graph cannot be simplified:
  For all pairs of nodes:
    If exactly one resistor connects them:
      Merge in series if part of a line
    If multiple resistors connect them:
      Merge using parallel rule: 1/R_eq = 1/R1 + 1/R2 + ...
Return final edge between source and sink as equivalent resistance
    </pre>

    <h2>🧪 Try It Out (Simple Series & Parallel Example)</h2>
    <p>Here’s a simple graph:</p>
    <ul>
      <li>Node A → B (resistor: 100 Ω)</li>
      <li>Node B → C (resistor: 200 Ω)</li>
      <li>Node A → C (parallel resistor: 300 Ω)</li>
    </ul>

    <div id="circuitGraph"></div>

    <button onclick="calculateResistance()">💡 Compute Equivalent Resistance</button>
    <p id="result"></p>

    <h3>📊 Efficiency & Improvements</h3>
    <ul>
      <li>Efficient for tree-like and small cyclic graphs.</li>
      <li>Can be optimized using union-find, DFS, and sparse matrix solvers.</li>
      <li>Scaleable using networkx, scipy, or symbolic solvers.</li>
    </ul>

    <h2>📚 Examples & Tests</h2>
    <ul>
      <li>🔸 Series: A-B (100Ω), B-C (200Ω) → R_eq = 300Ω</li>
      <li>🔸 Parallel: A-C (300Ω), via B path (100+200) → R_eq = 1 / (1/300 + 1/300) = 150Ω</li>
    </ul>
  </div>

  <script>
    function calculateResistance() {
      const R1 = 100;
      const R2 = 200;
      const R3 = 300;
      const seriesPath = R1 + R2;
      const parallel = 1 / (1 / R3 + 1 / seriesPath);
      document.getElementById("result").innerText = `Total Equivalent Resistance: ${parallel.toFixed(2)} Ω`;
    }

    const nodes = [
      { id: 'A', x: 0, y: 1 },
      { id: 'B', x: 1, y: 1 },
      { id: 'C', x: 2, y: 0.5 },
    ];

    const edges = [
      { from: 'A', to: 'B', value: 100 },
      { from: 'B', to: 'C', value: 200 },
      { from: 'A', to: 'C', value: 300 },
    ];

    const edgeTraces = edges.map(edge => {
      const fromNode = nodes.find(n => n.id === edge.from);
      const toNode = nodes.find(n => n.id === edge.to);
      return {
        type: 'scatter',
        x: [fromNode.x, toNode.x],
        y: [fromNode.y, toNode.y],
        mode: 'lines+text',
        line: { width: 2 },
        text: [`${edge.value}Ω`],
        textposition: 'top center',
        hoverinfo: 'none',
        showlegend: false
      };
    });

    const nodeTrace = {
      type: 'scatter',
      mode: 'markers+text',
      x: nodes.map(n => n.x),
      y: nodes.map(n => n.y),
      marker: { size: 16, color: '#2980b9' },
      text: nodes.map(n => n.id),
      textposition: 'bottom center',
      hoverinfo: 'text'
    };

    const layout = {
      title: 'Resistor Network Visualization',
      margin: { t: 50 },
      xaxis: { visible: false },
      yaxis: { visible: false },
      height: 400
    };

    Plotly.newPlot('circuitGraph', [...edgeTraces, nodeTrace], layout);
  </script>
</body>
</html>
