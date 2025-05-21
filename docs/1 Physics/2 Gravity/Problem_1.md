<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Understanding Kepler's Third Law</title>
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
    <h1>Understanding Kepler's Third Law of Planetary Motion</h1>
    <p>A deep dive into the connection between orbital period and orbital radius</p>
  </header>

  <section>
    <h2>Kepler's Third Law Explained</h2>
    <p>Kepler's Third Law states that the square of the orbital period (T) of a planet is proportional to the cube of the semi-major axis (r) of its orbit: <strong>T<sup>2</sup> ∝ r<sup>3</sup></strong>. This means that if you know how far a planet is from the object it is orbiting, you can predict how long it will take to complete one orbit. In the case of circular orbits, this relationship can be derived precisely from Newtonian mechanics.</p>

    <p>Let’s derive the law using the balance between centripetal force and gravitational force. The gravitational force between two objects is <strong>F = GMm/r<sup>2</sup></strong>, and the centripetal force required to keep a mass in circular motion is <strong>F = mv<sup>2</sup>/r</strong>. Setting these equal gives:</p>
    <p><strong>mv<sup>2</sup>/r = GMm/r<sup>2</sup></strong></p>
    <p>Solving for velocity <em>v</em> and substituting <em>v = 2πr/T</em>, we find:</p>
    <p><strong>T<sup>2</sup> = (4π<sup>2</sup>/GM)r<sup>3</sup></strong></p>
    <p>This is Kepler’s Third Law for circular orbits.</p>
  </section>

  <section>
    <h2>Visualizing Orbits and the Law</h2>
    <p>The following graphic represents three different orbits around a central body. The sizes are scaled to show increasing orbital radii. In each orbit, the period increases with distance according to Kepler's Third Law.</p>
    <canvas id="orbitCanvas" width="500" height="500"></canvas>
    <script>
      const canvas = document.getElementById("orbitCanvas");
      const ctx = canvas.getContext("2d");

      function drawSystem() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        const centerX = canvas.width / 2;
        const centerY = canvas.height / 2;

        // Draw central star
        ctx.beginPath();
        ctx.arc(centerX, centerY, 10, 0, 2 * Math.PI);
        ctx.fillStyle = "orange";
        ctx.fill();

        const orbits = [
          { radius: 60, color: "red", label: "Planet A" },
          { radius: 120, color: "green", label: "Planet B" },
          { radius: 180, color: "blue", label: "Planet C" }
        ];

        orbits.forEach((orbit, i) => {
          ctx.beginPath();
          ctx.arc(centerX, centerY, orbit.radius, 0, 2 * Math.PI);
          ctx.strokeStyle = orbit.color;
          ctx.stroke();

          // Draw planets
          const angle = Math.PI / 4;
          const x = centerX + orbit.radius * Math.cos(angle);
          const y = centerY + orbit.radius * Math.sin(angle);

          ctx.beginPath();
          ctx.arc(x, y, 6, 0, 2 * Math.PI);
          ctx.fillStyle = orbit.color;
          ctx.fill();

          ctx.fillStyle = "black";
          ctx.fillText(orbit.label, x + 10, y);
        });
      }

      drawSystem();
    </script>
  </section>

  <section>
    <h2>Extension to Elliptical Orbits</h2>
    <p>While the derivation above assumes circular orbits, Kepler originally observed his law in elliptical orbits. In these cases, the semi-major axis of the ellipse replaces the radius. The law still holds: the time it takes for a planet to orbit is related to the size of its orbit, regardless of its shape.</p>
    <p>This generalization is powerful because most celestial bodies, including comets and some moons, follow elliptical paths.</p>
  </section>

  <section>
    <h2>Applications in Astronomy</h2>
    <p>Kepler's Third Law allows scientists to measure the masses of distant planets and stars. For example, if the period and radius of a moon orbiting a planet are known, the mass of the planet can be calculated. This method is commonly used in both our Solar System and in distant exoplanetary systems.</p>
    <p>It also provides critical information for space missions, satellite placements, and understanding gravitational fields in a system.</p>
  </section>

  <section>
    <h2>Real-World Examples</h2>
    <p>The Moon orbits Earth at a distance of about 384,000 km and completes one orbit approximately every 27.3 days. Plugging these values into Kepler’s formula provides a close approximation of Earth's mass.</p>
    <p>Similarly, planets in our Solar System obey Kepler’s law. For example, Jupiter, being much farther from the Sun than Earth, has an orbital period of about 12 Earth years.</p>
  </section>

  <section>
    <h2>Simulating Orbits</h2>
    <p>Below is a simulation verifying the T<sup>2</sup> ∝ r<sup>3</sup> relationship for different orbital radii. Open your browser's developer console to view the output.</p>
    <script>
      function verifyKeplerLaw() {
        const G = 6.67430e-11;
        const M = 5.972e24;
        const results = [];

        for (let r = 1e7; r <= 5e7; r += 1e7) {
          let T = 2 * Math.PI * Math.sqrt(r ** 3 / (G * M));
          results.push({
            radius_km: r / 1000,
            period_sec: T,
            period_days: T / (60 * 60 * 24),
            T2_over_r3: (T ** 2) / (r ** 3)
          });
        }

        console.log("Kepler's Law Simulation:", results);
      }

      verifyKeplerLaw();
    </script>
  </section>
</body>
</html>