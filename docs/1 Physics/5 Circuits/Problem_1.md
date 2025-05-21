<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Equivalent Resistance Using Graph Theory</title>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
  <script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
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
    .code-highlight { color: darkred; font-weight: bold; }
  </style>
</head>
<body>
  <h1>Equivalent Resistance Using Graph Theory</h1>

  <section>
    <h2>Understanding the Concept</h2>
    <p>
      In complex electrical circuits, calculating the equivalent resistance using traditional methods can be challenging due to nested series and parallel components. Graph theory provides a systematic approach by treating the circuit as a graph: nodes represent junctions and edges represent resistors with weights equivalent to their resistance values.
    </p>
    <p>
      By applying graph reduction techniques—such as identifying nodes of degree two for series simplification and detecting cycles for parallel combinations—we can automate and streamline the process. This enables accurate modeling of intricate circuits used in power distribution, PCB design, and microelectronic layout analysis.
    </p>
  </section>

  <section>
    <h2>Advanced Algorithm Overview</h2>
    <pre><code>function simplify_circuit(graph, start, end):
    while changes_detected:
        for each node in graph:
            if node.degree == 2 and not endpoint:
                merge_series(node)
        for each set of parallel edges:
            merge_parallel(set)
        detect_supernodes_and_contract()
    return compute_resistance(start, end)</code></pre>
    <p>
      This version improves scalability by introducing supernode detection and graph contraction, which collapse reducible clusters and simplify evaluation across complex topologies.
    </p>
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
    return G[start][end]['resistance']

G = nx.MultiGraph()
G.add_edge('A', 'X', resistance=2)
G.add_edge('X', 'B', resistance=3)
G.add_edge('A', 'B', resistance=6)  # parallel path
R_eq = simplify(G, 'A', 'B')
print("Equivalent Resistance:", R_eq)</code></pre>
  </section>

  <section>
    <h2>Visualizing Complex Combinations</h2>
    <div id="plot" style="height:400px;"></div>
    <script>
      const layout = {
        title: 'Resistor Network Visualization',
        xaxis: { title: 'Position' },
        yaxis: { title: 'Potential Level' }
      };
      const trace = {
        x: [0, 1, 2, 1],
        y: [0, 1, 0, -1],
        text: ['A', 'X', 'B', 'C'],
        mode: 'lines+text',
        type: 'scatter',
        line: { shape: 'spline' },
        textposition: 'top center'
      };
      Plotly.newPlot('plot', [trace], layout);
    </script>
  </section>

  <section>
    <h2>Performance and Extensions</h2>
    <p>
      For dense or irregular circuits, incorporating adjacency matrices or Laplacian matrices enables resistance computation via linear algebra. The effective resistance \( R_{eff} = (e_i - e_j)^T L^+ (e_i - e_j) \), where \( L^+ \) is the pseudoinverse of the Laplacian, is particularly useful in high-performance applications.
    </p>
    <p>
      Future work could include:
      <ul>
        <li>GUI for drag-and-drop resistor networks</li>
        <li>Symbolic algebra support using <code>sympy</code></li>
        <li>Integration into electronic circuit simulators</li>
      </ul>
    </p>
  </section>
</body>
</html>
