<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Kepler's Third Law</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      line-height: 1.6;
      margin: 0;
      padding: 0;
      background-color: #f4f4f4;
      color: #333;
    }
    header, section, footer {
      padding: 20px;
      max-width: 900px;
      margin: auto;
    }
    header {
      background-color: #283593;
      color: white;
      text-align: center;
    }
    h1, h2 {
      color: #283593;
    }
    canvas {
      display: block;
      margin: 20px auto;
      background: #fff;
      border: 1px solid #ccc;
    }
    footer {
      text-align: center;
      font-size: 0.9em;
      color: #666;
    }
  </style>
</head>
<body>
  <header>
    <h1>Kepler's Third Law</h1>
    <p>Exploring the relationship between orbital period and radius</p>
  </header>

  <section>
    <h2>1. Detailed Explanation</h2>
    <p>Kepler's Third Law states that the square of the orbital period (T) of a planet is directly proportional to the cube of the semi-major axis (r) of its orbit: <strong>T<sup>2</sup> ∝ r<sup>3</sup></strong>. For circular orbits, this becomes an exact relationship. It was derived from empirical observations by Johannes Kepler and later explained using Newton's Law of Gravitation.</p>
  </section>

  <section>
    <h2>2. Graphical Representations</h2>
    <canvas id="orbitCanvas" width="400" height="400"></canvas>
    <script>
      const canvas = document.getElementById("orbitCanvas");
      const ctx = canvas.getContext("2d");

      function drawOrbit(radius, color) {
        ctx.beginPath();
        ctx.arc(200, 200, radius, 0, 2 * Math.PI);
        ctx.strokeStyle = color;
        ctx.stroke();
      }

      drawOrbit(50, "red");
      drawOrbit(100, "green");
      drawOrbit(150, "blue");
    </script>
  </section>

  <section>
    <h2>3. Extension to Elliptical Orbits</h2>
    <p>For elliptical orbits, Kepler's Third Law still applies when using the semi-major axis as the orbital radius. This law is crucial for calculating orbits of planets, moons, and artificial satellites. It shows that even in elongated orbits, the relationship between period and radius remains consistent.</p>
  </section>

  <section>
    <h2>Tasks</h2>
    <h3>Task 1: Derivation</h3>
    <p>By equating centripetal force and gravitational force, <em>mv<sup>2</sup>/r = GMm/r<sup>2</sup></em>, we derive <em>T<sup>2</sup> = (4π<sup>2</sup>/GM)r<sup>3</sup></em>.</p>

    <h3>Task 2: Implications</h3>
    <p>This law allows astronomers to determine masses of celestial bodies and distances between them, revolutionizing our understanding of the cosmos.</p>

    <h3>Task 3: Real-world Examples</h3>
    <p>The Moon orbits Earth in about 27.3 days at a distance of ~384,000 km. Using Kepler's law, we can derive Earth's mass. Similarly, we model planetary orbits in the Solar System.</p>

    <h3>Task 4: Computational Model</h3>
    <p>Check your browser's console for verification of Kepler's Law using simulated orbits.</p>
    <script>
      function verifyKeplerLaw() {
        const G = 6.67430e-11;
        const M = 5.972e24;
        const results = [];

        for (let r = 1e7; r <= 5e7; r += 1e7) {
          let T = 2 * Math.PI * Math.sqrt(r ** 3 / (G * M));
          results.push({ r, T, T2: T ** 2, r3: r ** 3, T2_r3: (T ** 2) / (r ** 3) });
        }

        console.log("Kepler's Law Verification (T^2/r^3):", results);
      }

      verifyKeplerLaw();
    </script>
  </section>

  <footer>
    <p>&copy; 2025 Kepler's Third Law Educational Site</p>
  </footer>
</body>
</html>
