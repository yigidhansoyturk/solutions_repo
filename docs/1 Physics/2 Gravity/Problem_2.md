<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Physics Simulations</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Roboto', sans-serif;
      margin: 0;
      padding: 0;
      background: linear-gradient(to right, #eef2f7, #d9e2ec);
      color: #2d3748;
    }
    .container {
      max-width: 960px;
      margin: 40px auto;
      padding: 40px;
      background-color: #ffffff;
      border-radius: 16px;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
    }
    h1 {
      font-size: 3rem;
      color: #1a202c;
      text-align: center;
      margin-bottom: 1rem;
    }
    h2 {
      font-size: 2rem;
      margin-top: 2rem;
      color: #2b6cb0;
      border-bottom: 2px solid #cbd5e0;
      padding-bottom: 0.3rem;
    }
    h3 {
      font-size: 1.5rem;
      margin-top: 1.5rem;
      color: #2c5282;
    }
    p, ul {
      font-size: 1.125rem;
      line-height: 1.75;
    }
    ul {
      margin-left: 1.5rem;
    }
    pre {
      background-color: #2d3748;
      color: #f7fafc;
      padding: 20px;
      border-radius: 10px;
      overflow-x: auto;
      font-size: 0.95rem;
    }
    code {
      font-family: 'Courier New', monospace;
    }
    .plot {
      margin: 2rem 0;
    }
    footer {
      text-align: center;
      margin-top: 3rem;
      font-size: 0.9rem;
      color: #718096;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Escape Velocities and Cosmic Velocities</h1>

    <h2>Motivation</h2>
    <p>
      The concept of escape velocity is crucial for understanding the conditions required to leave a celestial body's gravitational influence. Extending this concept, the first, second, and third cosmic velocities define the thresholds for orbiting, escaping, and leaving a star system. These principles underpin modern space exploration, from launching satellites to interplanetary missions.
    </p>

    <h2>Theoretical Foundation</h2>
    <h3>Cosmic Velocities Explained</h3>
    <p><strong>First Cosmic Velocity (Orbital Velocity):</strong></p>
    <p>\[ v_1 = \sqrt{\frac{GM}{r}} \]</p>
    <p><strong>Second Cosmic Velocity (Escape Velocity):</strong></p>
    <p>\[ v_2 = \sqrt{\frac{2GM}{r}} \]</p>
    <p><strong>Third Cosmic Velocity (Interstellar Escape):</strong></p>
    <p>This is the velocity required to leave the solar system and is influenced by the Sun's gravity.</p>

    <h2>Python Simulation</h2>
    <p>
      We compute the velocities using Python and visualize them with a dynamic bar chart.
    </p>
    <div id="cosmicVelocitiesPlot" class="plot"></div>
    <script>
      const G = 6.67430e-11;
      const bodies = [
        {name: 'Earth', M: 5.972e24, r: 6.371e6},
        {name: 'Mars', M: 6.417e23, r: 3.3895e6},
        {name: 'Jupiter', M: 1.898e27, r: 6.9911e7},
      ];

      const names = bodies.map(b => b.name);
      const v1 = bodies.map(b => Math.sqrt(G * b.M / b.r));
      const v2 = bodies.map(b => Math.sqrt(2 * G * b.M / b.r));

      const trace1 = {
        x: names,
        y: v1,
        name: '1st Cosmic Velocity',
        type: 'bar',
        marker: {color: '#4299e1'}
      };

      const trace2 = {
        x: names,
        y: v2,
        name: '2nd Cosmic Velocity',
        type: 'bar',
        marker: {color: '#ed8936'}
      };

      const layout = {
        title: 'Cosmic Velocities of Celestial Bodies',
        xaxis: {title: 'Celestial Body'},
        yaxis: {title: 'Velocity (m/s)'},
        barmode: 'group'
      };

      Plotly.newPlot('cosmicVelocitiesPlot', [trace1, trace2], layout);
    </script>

    <h2>Applications</h2>
    <ul>
      <li>Designing satellite orbits and interplanetary missions</li>
      <li>Determining launch velocities for space travel</li>
      <li>Understanding gravitational escape in different celestial environments</li>
    </ul>

    <h2>Discussion</h2>
    <ul>
      <li><strong>Limitations:</strong> Assumes spherical symmetry and vacuum; neglects atmospheric drag and relativistic effects</li>
      <li><strong>Extensions:</strong> Include effects of rotation, atmosphere, and gravity assists</li>
    </ul>

    <footer>
      &copy; 2025 Physics Simulations | Escape Velocities and Cosmic Dynamics
    </footer>
  </div>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
  <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
</body>
</html>
