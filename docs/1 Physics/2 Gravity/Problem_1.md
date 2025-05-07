<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Orbital Period and Orbital Radius - Kepler's Third Law</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0 auto;
      max-width: 1000px;
      padding: 20px;
      background-color: #fefefe;
      color: #333;
      line-height: 1.6;
    }
    h1, h2, h3 {
      color: #2c3e50;
    }
    .section {
      margin-bottom: 30px;
    }
    code {
      background: #f4f4f4;
      padding: 3px 6px;
      border-radius: 4px;
    }
    .button {
      padding: 10px 15px;
      cursor: pointer;
      background-color: #4CAF50;
      color: white;
      border: none;
      border-radius: 4px;
      font-size: 16px;
      margin-top: 20px;
    }
    .button:hover {
      background-color: #45a049;
    }
    .container {
      max-width: 1200px;
      margin: 0 auto;
    }
    #plot {
      margin-top: 40px;
      height: 400px;
    }
  </style>
</head>
<body>

  <div class="container">
    <h1>Orbital Period and Orbital Radius</h1>

    <section class="section">
      <h2>Motivation</h2>
      <p>Kepler's Third Law is a cornerstone of celestial mechanics. It describes the relationship between the square of the orbital period (\(T\)) and the cube of the orbital radius (\(r\)), stating that:</p>
      <p><strong>\(T^2 \propto r^3\)</strong></p>
      <p>This relationship, while simple, is a foundational principle in celestial mechanics, providing insights into planetary motion and gravitational interactions. It allows us to understand satellite orbits and planetary systems and can be used to determine the mass of celestial bodies or distances in space.</p>
    </section>

    <section class="section">
      <h2>Theoretical Foundation</h2>
      <p>For circular orbits, we can derive the relationship between the orbital period and orbital radius using Newton’s Law of Universal Gravitation and centripetal force.</p>
      <pre>
        F = G * M * m / r^2   (Gravitational force)
        F = m * v^2 / r       (Centripetal force)
        v = √(G * M / r)
        T = 2πr / v
        T^2 = 4π²r³ / G * M
      </pre>
      <p>This gives us the fundamental relationship:</p>
      <p><strong>\(T^2 \propto r^3\)</strong></p>
    </section>

    <section class="section">
      <h2>Implications</h2>
      <p>This relationship is crucial for determining planetary masses, distances, and orbital periods. For instance:</p>
      <ul>
        <li><strong>Moon’s Orbit around Earth:</strong> The orbital period is about 27.3 days, and the radius is 384,400 km.</li>
        <li><strong>Earth’s Orbit around the Sun:</strong> The orbital period is 1 year, and the radius is 1 AU (astronomical unit).</li>
      </ul>
    </section>

    <section class="section">
      <h2>Real-world Example: Moon’s Orbit Around Earth</h2>
      <p>The Moon's orbital radius is 384,400 km, and its orbital period is about 27.3 days. Using Kepler’s Third Law, we can verify that the Moon’s orbital period and radius follow the expected relationship, and use it to calculate the mass of Earth if needed.</p>
    </section>

    <section class="section">
      <h2>Computational Model</h2>
      <p>Below is a Python implementation that simulates circular orbits and visually demonstrates the relationship between orbital period and radius.</p>
      <pre>
# Python code for simulating orbital periods and radii
import numpy as np
import matplotlib.pyplot as plt

# Constants
G = 6.67430e-11  # Gravitational constant in m^3 kg^-1 s^-2
M = 5.972e24     # Mass of Earth in kg (can be adjusted for other celestial bodies)

# Function to calculate orbital period
def orbital_period(r):
    return 2 * np.pi * np.sqrt(r**3 / (G * M))

# Radii (in meters)
radii = np.linspace(1e6, 1e8, 100)

# Orbital periods corresponding to the radii
periods = orbital_period(radii)

# Calculate T^2 and r^3
T_squared = periods**2
r_cubed = radii**3

# Plotting T^2 vs r^3
plt.figure(figsize=(8, 6))
plt.plot(r_cubed, T_squared, label=f'Mass of Earth = {M} kg')
plt.xlabel('r^3 (m^3)')
plt.ylabel('T^2 (s^2)')
plt.title("Kepler's Third Law: T^2 vs r^3")
plt.grid(True)
plt.legend()
plt.show()
      </pre>
      <button class="button" onclick="alert('Python code example for orbital period and radius is shown above.')">See Python Code</button>
    </section>

    <section class="section">
      <h2>Graphical Representation</h2>
      <p>The plot below shows the relationship between \(T^2\) and \(r^3\) based on a simulated set of orbital radii. The linear relationship is clear, confirming Kepler’s Third Law.</p>
      <div id="plot"></div>
    </section>

    <section class="section">
      <h2>Further Discussion</h2>
      <p>This relationship can also be extended to elliptical orbits. Kepler's Third Law still holds, but the semi-major axis (\(a\)) is used instead of the orbital radius \(r\). In this case, the relationship becomes:</p>
      <p><strong>\(T^2 \propto a^3\)</strong></p>
      <p>This modification accounts for the varying distance in elliptical orbits, but the principle remains the same. The law applies to other celestial bodies and allows scientists to calculate orbital properties for a wide range of systems.</p>
    </section>

    <section class="section">
      <h2>Deliverables</h2>
      <ul>
        <li><strong>Markdown Document with Python Script:</strong> The Python code is included above for simulating circular orbits and plotting the relationship between orbital period and radius.</li>
        <li><strong>Description of the Relationship:</strong> The derived equation \( T^2 \propto r^3 \) and its implications are discussed in the document.</li>
        <li><strong>Graphical Representation:</strong> The graph of \( T^2 \) vs \( r^3 \) is provided, showing the linear relationship between orbital period and orbital radius.</li>
        <li><strong>Discussion of Elliptical Orbits:</strong> The extension of Kepler’s Third Law to elliptical orbits is discussed, with the semi-major axis replacing the radius.</li>
      </ul>
    </section>
  </div>

  <script>
    // Simulating the graph for T^2 vs r^3 using JavaScript for web visualization
    let G = 6.67430e-11;  // Gravitational constant in m^3 kg^-1 s^-2
    let M = 5.972e24;     // Mass of Earth in kg

    // Function to calculate orbital period
    function orbitalPeriod(r) {
      return 2 * Math.PI * Math.sqrt(Math.pow(r, 3) / (G * M));
    }

    // Radii (in meters)
    let radii = [];
    let periods = [];
    for (let i = 1e6; i <= 1e8; i += 1e5) {
      radii.push(i);
      periods.push(orbitalPeriod(i));
    }

    // Calculate T^2 and r^3
    let T_squared = periods.map(p => p ** 2);
    let r_cubed = radii.map(r => r ** 3);

    // Plotting T^2 vs r^3 using Plotly
    let trace = {
      x: r_cubed,
      y: T_squared,
      mode: 'lines',
      type: 'scatter',
      name: "T^2 vs r^3"
    };

    let layout = {
      title: "Kepler's Third Law: T^2 vs r^3",
      xaxis: { title: "r^3 (m^3)" },
      yaxis: { title: "T^2 (s^2)" },
      showlegend: true
    };

    Plotly.newPlot('plot', [trace], layout);
  </script>

</body>
</html>
