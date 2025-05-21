<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Equivalent Resistance Using Graph Theory</title>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
  <script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
  <script src="https://unpkg.com/vis-network/standalone/umd/vis-network.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
  <style>
    body { font-family: Arial, sans-serif; background: #f9f9f9; color: #222; margin: 20px; }
    h1, h2 { color: #003366; }
    section { background: #fff; padding: 20px; margin: 20px 0; border-radius: 10px; box-shadow: 0 0 10px rgba(0,0,0,0.1); }
    #network { height: 500px; border: 1px solid #ccc; margin-bottom: 20px; }
    pre { background: #eee; padding: 10px; border-radius: 5px; overflow-x: auto; }
    select, button { margin-top: 10px; margin-right: 10px; }
  </style>
</head>
<body>
  <h1>Equivalent Resistance Using Graph Theory</h1>

  <section>
    <h2>Interactive Circuit Editor</h2>
    <div id="network"></div>
    <label for="nodeStart">Start Node:</label>
    <select id="nodeStart"></select>
    <label for="nodeEnd">End Node:</label>
    <select id="nodeEnd"></select>
    <button onclick="computeResistance()">Compute Resistance</button>
    <button onclick="exportAsPNG()">Export as PNG</button>
    <button onclick="exportAsJSON()">Export as JSON</button>
    <p id="resistanceResult" style="font-weight: bold; margin-top: 10px;"></p>
  </section>

  <section>
  <h2>Concept and Explanation</h2>
  <p>
    In electrical circuits, calculating the total or "equivalent" resistance between two points is essential for understanding current flow and power distribution. Traditional methods involve step-by-step application of series and parallel rules. However, for more complex circuits—especially ones with nested loops or multiple branches—this approach becomes inefficient.
  </p>
  <p>
    Graph theory offers a powerful alternative. A circuit can be modeled as a graph, where <strong>nodes</strong> represent junctions and <strong>edges</strong> represent resistors. This abstraction allows us to apply algorithms for automatically simplifying and analyzing the network. Series connections are identified as nodes with two neighbors and merged into a single edge. Parallel connections are detected when multiple edges connect the same node pair, and can be reduced using the reciprocal sum formula for parallel resistances.
  </p>
  <p>
    Using these principles, this tool lets you:
  </p>
  <ul>
    <li>Create and edit circuit graphs interactively</li>
    <li>Visually assign resistance values between any two nodes</li>
    <li>Select start and end nodes to compute their combined resistance using direct or parallel connection logic</li>
    <li>Export your circuit as an image or downloadable JSON file for reuse</li>
  </ul>
  <p>
    This approach is not only more scalable but also opens the door to symbolic computation, live simulation, and integration with digital circuit design tools.
  </p>
</section>

  <script>
    const nodes = new vis.DataSet([
      { id: 'A', label: 'A' },
      { id: 'B', label: 'B' },
      { id: 'C', label: 'C' },
      { id: 'X', label: 'X' }
    ]);

    const edges = new vis.DataSet([
      { from: 'A', to: 'X', label: '2', title: '2 Ω' },
      { from: 'X', to: 'B', label: '3', title: '3 Ω' },
      { from: 'A', to: 'B', label: '6', title: '6 Ω' },
      { from: 'X', to: 'C', label: '4', title: '4 Ω' }
    ]);

    const container = document.getElementById('network');
    const network = new vis.Network(container, { nodes, edges }, {
      manipulation: { enabled: true },
      physics: false,
      edges: { font: { align: 'middle' } },
      nodes: { shape: 'dot', size: 20, font: { size: 16, color: '#003366' }, borderWidth: 2 }
    });

    function populateSelectors() {
      const nodeList = nodes.get();
      const startSelect = document.getElementById('nodeStart');
      const endSelect = document.getElementById('nodeEnd');
      startSelect.innerHTML = '';
      endSelect.innerHTML = '';
      nodeList.forEach(node => {
        startSelect.add(new Option(node.label, node.id));
        endSelect.add(new Option(node.label, node.id));
      });
    }

    function computeResistance() {
      const start = document.getElementById('nodeStart').value;
      const end = document.getElementById('nodeEnd').value;
      let total = 0;
      let count = 0;
      edges.get().forEach(edge => {
        if ((edge.from === start && edge.to === end) || (edge.from === end && edge.to === start)) {
          const r = parseFloat(edge.label);
          if (!isNaN(r)) {
            total += 1 / r;
            count++;
          }
        }
      });
      const resultElement = document.getElementById('resistanceResult');
      if (count > 0) {
        const result = 1 / total;
        resultElement.textContent = `Resistance from ${start} to ${end}: ${result.toFixed(2)} Ω`;
      } else {
        resultElement.textContent = `No direct connection between ${start} and ${end}`;
      }
    }

    function exportAsPNG() {
      html2canvas(document.getElementById('network')).then(canvas => {
        const link = document.createElement('a');
        link.download = 'circuit.png';
        link.href = canvas.toDataURL();
        link.click();
      });
    }

    function exportAsJSON() {
      const content = {
        nodes: nodes.get(),
        edges: edges.get()
      };
      const dataStr = 'data:text/json;charset=utf-8,' + encodeURIComponent(JSON.stringify(content, null, 2));
      const a = document.createElement('a');
      a.setAttribute('href', dataStr);
      a.setAttribute('download', 'circuit.json');
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
    }

    window.addEventListener('load', populateSelectors);
  </script>
</body>
</html>
