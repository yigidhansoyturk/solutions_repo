<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Equivalent Resistance Using Graph Theory</title>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
  <script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <script src="https://unpkg.com/vis-network/standalone/umd/vis-network.min.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
      background-color: #f9f9f9;
      color: #222;
    }
    section {
      background: #fff;
      padding: 20px;
      margin: 20px 0;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    h1, h2 { color: #003366; }
    pre { background: #eee; padding: 10px; border-radius: 5px; overflow-x: auto; }
    #network { height: 500px; border: 1px solid lightgray; }
  </style>
</head>
<body>
  <h1>Equivalent Resistance Using Graph Theory</h1>

  <section>
    <h2>Understanding the Concept</h2>
    <p>
      Analyzing electrical circuits involves determining the total resistance between two terminals. For simple series or parallel configurations, this can be done manually. However, when circuits contain loops, branches, and mixed configurations, traditional methods fall short.
    </p>
    <p>
      Graph theory transforms this problem: nodes represent circuit junctions and edges represent resistors with weights as resistance values. Algorithms can then traverse and simplify these graphs by identifying patterns such as series (linear chains) and parallel (cycles or multiple edges) connections, allowing even large circuits to be simplified algorithmically.
    </p>
    <p>
      This mathematical approach is not just elegant—it is also practical for circuit design tools, power grid modeling, and embedded system simulations where dynamic or complex topologies are the norm.
    </p>
  </section>

  <section>
    <h2>Live Editable Circuit Network</h2>
    <div id="network"></div>
    <script>
      const nodes = new vis.DataSet([
        { id: 'A', label: 'A' },
        { id: 'B', label: 'B' },
        { id: 'X', label: 'X' },
        { id: 'C', label: 'C' }
      ]);

      const edges = new vis.DataSet([
        { from: 'A', to: 'X', label: '2 Ω' },
        { from: 'X', to: 'B', label: '3 Ω' },
        { from: 'A', to: 'B', label: '6 Ω' },
        { from: 'X', to: 'C', label: '4 Ω' }
      ]);

      const container = document.getElementById('network');
      const data = { nodes, edges };
      const options = {
        physics: false,
        edges: {
          font: { align: 'top' },
          color: 'gray'
        },
        nodes: {
          shape: 'dot',
          size: 20,
          font: { size: 16, color: '#003366' },
          borderWidth: 2
        }
      };
      new vis.Network(container, data, options);
    </script>
    <div style="margin-top: 20px;">
      <button onclick="computeResistance()">Compute Resistance from A to B</button>
      <p id="resistanceResult" style="margin-top: 10px; font-weight: bold;"></p>
    </div>
    <script>
      function computeResistance() {
        const edgeList = edges.get();
        let total = 0;
        let pathCount = 0;
        for (const edge of edgeList) {
          if ((edge.from === 'A' && edge.to === 'B') || (edge.from === 'B' && edge.to === 'A')) {
            const value = parseFloat(edge.label);
            if (!isNaN(value)) {
              total += 1 / value;
              pathCount++;
            }
          }
        }
        if (pathCount > 0) {
          const result = 1 / total;
          document.getElementById('resistanceResult').textContent = `Approx. Resistance (A-B): ${result.toFixed(2)} Ω`;
        } else {
          document.getElementById('resistanceResult').textContent = 'No direct or parallel connection between A and B found.';
        }
      }
    </script>
  </section>

  <section>
  <h2>Graph-Based Circuit Simplification Illustration</h2>
  <p>
    Below is a visual summary of how graph-based circuit simplification works. Series and parallel connections are dynamically reduced to calculate the total resistance between terminals. This process mirrors how software like SPICE or network analyzers internally process circuits.
  </p>
  <div id="simplificationDiagram"></div>
  <script>
    const diagramLayout = {
      title: 'Simplification Pattern: Series and Parallel Edges',
      margin: { t: 40, l: 40, r: 40, b: 40 },
      xaxis: { showgrid: false, zeroline: false },
      yaxis: { showgrid: false, zeroline: false }
    };
    const seriesTrace = {
      x: [0, 1, 2],
      y: [0, 0, 0],
      mode: 'lines+markers+text',
      type: 'scatter',
      name: 'Series',
      line: { width: 3, color: 'blue' },
      text: ['A', '', 'B'],
      textposition: 'top center'
    };
    const parallelTrace = {
      x: [4, 5, 6, 5, 4],
      y: [0, 1, 0, -1, 0],
      mode: 'lines+markers+text',
      type: 'scatter',
      name: 'Parallel',
      line: { width: 3, color: 'orange' },
      text: ['A', '', 'B', '', ''],
      textposition: 'top center'
    };
    Plotly.newPlot('simplificationDiagram', [seriesTrace, parallelTrace], diagramLayout);
  </script>
</section>

  <section>
    <h2>Python Implementation with networkx</h2>
    <pre><code>import networkx as nx

def simplify(G, start, end):
    def is_series(node):
        return G.degree[node] == 2 and node not in [start, end]

    changed = True
    while changed:
        changed = False
        for node in list(G.nodes):
            if is_series(node):
                u, v = list(G.neighbors(node))
                R = G[u][node]['resistance'] + G[v][node]['resistance']
                G.remove_node(node)
                G.add_edge(u, v, resistance=R)
                changed = True
        for u, v in list(G.edges):
            parallels = list(G.get_edge_data(u, v).values())
            if len(parallels) > 1:
                total = 1 / sum(1 / e['resistance'] for e in parallels)
                G.remove_edges_from([(u, v)] * len(parallels))
                G.add_edge(u, v, resistance=total)
    return G[start][end]['resistance']</code></pre>
  </section>

  <section>
    <h2>Performance and Extensions</h2>
    <p>
      For dense or irregular circuits, matrix-based methods are superior. One such method uses the Laplacian matrix \( L \) of the graph. The effective resistance between nodes \( i \) and \( j \) is given by:
    </p>
    <p>
      \[ R_{eff} = (e_i - e_j)^T L^+ (e_i - e_j) \]
    </p>
    <p>
      where \( L^+ \) is the Moore–Penrose pseudoinverse of the Laplacian. This approach is robust in high-performance computing contexts.
    </p>
    <p>
      Future capabilities could include:
      <ul>
        <li>Dynamic drag-and-drop circuit editor with export</li>
        <li>Interactive resistance solver integrated with symbolic libraries</li>
        <li>Real-time simulation overlay for current/voltage animation</li>
      </ul>
    </p>
  </section>
</body>
</html>
