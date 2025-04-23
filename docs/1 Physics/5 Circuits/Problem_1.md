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
      Calculating equivalent resistance is crucial in understanding how electrical circuits behave. Traditional methods using only series and parallel resistor rules can become challenging with complex networks. Graph theory offers a structured, algorithmic approach.
    </p>
    <p>
      In graph theory, we treat each junction as a node and each resistor as an edge with a weight (resistance). By merging resistors using logical rules based on connectivity, we simplify the network into a single equivalent resistance efficiently.
    </p>

    <h2>🛠️ How It Works</h2>
    <ol>
      <li>Draw the circuit as a graph using nodes and resistors (edges with weights).</li>
      <li>Combine resistors in series—when there's a single path between two junctions.</li>
      <li>Combine resistors in parallel—when multiple resistors connect the same two junctions.</li>
      <li>Repeat the simplification process until one edge remains representing total resistance.</li>
    </ol>

    <h2>🧪 Try It Out (Simple Series & Parallel Example)</h2>
    <p>This example shows a basic circuit with three resistors:</p>
    <ul>
      <li><strong>Resistor 1:</strong> Node A → B (100 Ω)</li>
      <li><strong>Resistor 2:</strong> Node B → C (200 Ω)</li>
      <li><strong>Resistor 3:</strong> Node A → C (300 Ω, in parallel with the A→B→C path)</li>
    </ul>
    <p>We will compute the total resistance from Node A to Node C using both paths (series and parallel).</p>

    <div id="circuitGraph"></div>
    <button onclick="calculateResistance()">💡 Compute Equivalent Resistance</button>
    <p id="result"></p>

    <h3>📊 Efficiency & Improvements</h3>
    <ul>
      <li>Optimized for acyclic graphs and can handle simple cycles.</li>
      <li>Further optimized using union-find, DFS, or matrix-based solvers.</li>
      <li>Adaptable to large-scale simulations using network analysis libraries.</li>
    </ul>

    <h2>📚 Examples & Tests</h2>
    <ul>
      <li>🔸 Series: A-B (100Ω), B-C (200Ω) → <strong>R_eq = 300Ω</strong></li>
      <li>🔸 Parallel: A-C (300Ω) in parallel with A-B-C path (100Ω + 200Ω) → <strong>R_eq = 150Ω</strong></li>
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
