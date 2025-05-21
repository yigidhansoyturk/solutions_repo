<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Understanding Kepler's Third Law</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

    body {
      font-family: 'Roboto', sans-serif;
      line-height: 1.6;
      margin: 0;
      padding: 0;
      background: linear-gradient(to right, #e0f7fa, #f3e5f5);
      color: #333;
      overflow-x: hidden;
    }

    header {
      background: linear-gradient(to right, #1a237e, #3949ab);
      color: white;
      text-align: center;
      padding: 60px 20px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
      animation: fadeIn 1.5s ease;
    }

    header h1 {
      font-size: 3em;
      margin-bottom: 10px;
    }

    header p {
      font-size: 1.3em;
      opacity: 0.95;
    }

    section {
      background: white;
      margin: 40px auto;
      padding: 40px;
      max-width: 960px;
      border-radius: 16px;
      box-shadow: 0 12px 24px rgba(0, 0, 0, 0.1);
      animation: slideIn 1s ease;
    }

    h2 {
      color: #512da8;
      margin-top: 0;
      font-size: 1.8em;
    }

    p {
      font-size: 1.15em;
    }

    canvas {
      display: block;
      margin: 30px auto;
      background: #fefefe;
      border: 2px dashed #b39ddb;
      border-radius: 10px;
      transition: transform 0.3s ease;
    }

    canvas:hover {
      transform: scale(1.03);
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(-20px); }
      to { opacity: 1; transform: translateY(0); }
    }

    @keyframes slideIn {
      from { opacity: 0; transform: translateX(-30px); }
      to { opacity: 1; transform: translateX(0); }
    }
  </style>
</head>
<body>
  <header>
    <h1>Understanding Kepler's Third Law of Planetary Motion</h1>
    <p>Exploring the profound connection between time and space in orbital mechanics</p>
  </header>

  <section>
    <h2>Kepler's Third Law Explained</h2>
    <p>Kepler's Third Law of Planetary Motion reveals a precise and elegant relationship: the square of a planet's orbital period is directly proportional to the cube of its orbit's semi-major axis. Mathematically, this is written as <strong>T<sup>2</sup> ∝ r<sup>3</sup></strong>. This law applies to all bodies in orbit, from artificial satellites around Earth to distant exoplanets revolving around other stars.</p>

    <p>To derive this relationship for circular orbits, we start by balancing two forces. The gravitational force pulling the orbiting object inward is given by <strong>F = GMm/r<sup>2</sup></strong>. This must equal the centripetal force needed to keep it moving in a circle: <strong>F = mv<sup>2</sup>/r</strong>. Setting them equal:</p>
    <p><strong>mv<sup>2</sup>/r = GMm/r<sup>2</sup></strong></p>
    <p>After canceling and solving for velocity, we substitute <strong>v = 2πr / T</strong>, leading us to:</p>
    <p><strong>T<sup>2</sup> = (4π<sup>2</sup>/GM)r<sup>3</sup></strong></p>
    <p>This shows that for a fixed central mass, the ratio T<sup>2</sup> / r<sup>3</sup> is constant. It is a key result of Newtonian mechanics and forms the foundation of orbital analysis.</p>
  </section>

  <section>
    <h2>Visualizing Orbital Relationships</h2>
    <p>Here is a simplified visualization of circular orbits around a central mass. Each orbit shows a planet at a proportional distance. The farther a planet is, the longer it takes to complete an orbit, as predicted by Kepler's Third Law.</p>
    <canvas id="orbitCanvas" width="500" height="500"></canvas>
    <script>
      const canvas = document.getElementById("orbitCanvas");
      const ctx = canvas.getContext("2d");

      function drawSystem() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        const centerX = canvas.width / 2;
        const centerY = canvas.height / 2;

        // Central star
        ctx.beginPath();
        ctx.arc(centerX, centerY, 10, 0, 2 * Math.PI);
        ctx.fillStyle = "orange";
        ctx.fill();

        const orbits = [
          { radius: 60, color: "#ef5350", label: "Planet A" },
          { radius: 120, color: "#66bb6a", label: "Planet B" },
          { radius: 180, color: "#42a5f5", label: "Planet C" }
        ];

        orbits.forEach((orbit, i) => {
          ctx.beginPath();
          ctx.arc(centerX, centerY, orbit.radius, 0, 2 * Math.PI);
          ctx.strokeStyle = orbit.color;
          ctx.lineWidth = 2;
          ctx.setLineDash([6, 4]);
          ctx.stroke();
          ctx.setLineDash([]);

          const angle = Math.PI / 4;
          const x = centerX + orbit.radius * Math.cos(angle);
          const y = centerY + orbit.radius * Math.sin(angle);

          ctx.beginPath();
          ctx.arc(x, y, 6, 0, 2 * Math.PI);
          ctx.fillStyle = orbit.color;
          ctx.fill();

          ctx.font = "14px Roboto";
          ctx.fillStyle = "#333";
          ctx.fillText(orbit.label, x + 10, y);
        });
      }

      drawSystem();
    </script>
  </section>

  <section>
    <h2>Beyond Circles: Elliptical Orbits</h2>
    <p>While circular orbits make the math elegant, most celestial paths are elliptical. Kepler discovered that the same law holds if we use the ellipse's semi-major axis. This insight means that even in stretched orbits—like those of comets or some moons—the same fundamental relationship governs motion. This universality is what makes Kepler's Third Law so powerful.</p>
  </section>

  <section>
    <h2>Practical Use in Astronomy</h2>
    <p>In astronomy, Kepler’s Third Law enables the measurement of unknown masses. Observing a moon orbiting a planet reveals the planet's mass. Astronomers use this technique extensively, not just in our Solar System but also to estimate the masses of exoplanets and stars based on the orbits of companions.</p>
    <p>Engineers and mission planners also rely on this law to design satellite orbits, space probes, and even calculate escape velocities and gravitational slingshots.</p>
  </section>

  <section>
    <h2>Examples from the Real Universe</h2>
    <p>The Moon’s average orbital distance is around 384,000 km, and its orbital period is about 27.3 days. Using Kepler’s formula, we can accurately back-calculate the Earth’s mass. Likewise, Kepler’s Third Law predicts that planets further from the Sun, like Saturn or Neptune, take significantly longer to complete an orbit than closer planets like Mercury or Venus.</p>
  </section>

  <section>
    <h2>Simulation and Numerical Verification</h2>
    <p>To verify Kepler's Law numerically, we simulate circular orbits with increasing radii. We calculate the orbital period and compare the ratio T<sup>2</sup>/r<sup>3</sup> for each. Open the browser console to see how consistently the law holds across distances.</p>
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
