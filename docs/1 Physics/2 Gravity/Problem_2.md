<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Escape Velocities and Cosmic Velocities</title>
  <script src="https://cdn.jsdelivr.net/npm/mathjax@2.7.7/MathJax.js?config=TeX-MML-AM_CHTML"></script>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 40px auto;
      max-width: 1000px;
      padding: 0 20px;
      background-color: #fefefe;
      color: #333;
      line-height: 1.6;
    }
    h1, h2, h3 {
      color: #2c3e50;
    }
    input[type="range"], input[type="number"] {
      width: 200px;
      margin-right: 10px;
    }
    #controls label {
      font-weight: bold;
      margin-right: 10px;
    }
    #plot {
      margin-top: 20px;
    }
    pre {
      background: #f4f4f4;
      padding: 10px;
      overflow-x: auto;
    }
    ul {
      margin-top: 0;
    }

    /* Style for formulas to keep the size consistent */
    .formula {
      font-size: 1.5em;
      font-family: 'Times New Roman', Times, serif;
      font-weight: bold;
      text-align: center;
      margin: 20px 0;
      color: #e74c3c;
    }

    /* Small formulas for inline usage */
    .small-formula {
      font-size: 1.2em;
      font-family: 'Times New Roman', Times, serif;
      font-weight: normal;
      text-align: center;
      margin: 10px 0;
      color: #333;
    }
  </style>
</head>
<body>

  <h1>🚀 Escape Velocities and Cosmic Velocities</h1>

  <section>
    <h2>📘 Motivation</h2>
    <p>
      The concept of escape velocity is crucial for understanding the conditions required to leave a celestial body's gravitational influence. Extending this concept, the first, second, and third cosmic velocities define the thresholds for orbiting, escaping, and leaving a star system. These principles underpin modern space exploration, from launching satellites to interplanetary missions.
    </p>
  </section>

  <section>
    <h2>🧑‍🏫 First, Second, and Third Cosmic Velocities</h2>
    <p>The first, second, and third cosmic velocities describe different escape velocities for a body to enter orbit, escape from a celestial body, or escape a star system:</p>
    <ul>
      <li><strong>First Cosmic Velocity</strong> (\(v_1\)): The velocity required to enter a circular orbit around the celestial body.</li>
      <li><strong>Second Cosmic Velocity</strong> (\(v_2\)): The velocity required to escape the celestial body's gravitational pull (escape velocity).</li>
      <li><strong>Third Cosmic Velocity</strong> (\(v_3\)): The velocity required to escape the gravitational influence of a star system (e.g., the Sun's gravitational pull from Earth).</li>
    </ul>

    <h3>Formulas</h3>
    <div class="formula">
      \( v_1 = \sqrt{\frac{GM}{R}} \)
    </div>
    <p>
      The first cosmic velocity is derived from the requirement for an object to enter a stable orbit around a planet. Here:
    </p>
    <ul>
      <li> \( G \) is the gravitational constant</li>
      <li> \( M \) is the mass of the celestial body</li>
      <li> \( R \) is the radius of the celestial body</li>
    </ul>

    <div class="formula">
      \( v_2 = \sqrt{\frac{2GM}{R}} \)
    </div>
    <p>
      The second cosmic velocity is the escape velocity, which is derived from the condition that the kinetic energy must overcome the gravitational potential energy of the object.
    </p>

    <div class="formula">
      \( v_3 = \sqrt{\frac{2GM_{\text{sun}}}{R_{\text{earth}}}} \)
    </div>
    <p>
      The third cosmic velocity describes the escape speed needed to leave the solar system, derived from the Sun's mass and the Earth's distance from the Sun.
    </p>
  </section>

  <section>
    <h2>🧪 Calculation of Velocities for Different Celestial Bodies</h2>
    <p>
      Let's calculate the first, second, and third cosmic velocities for different celestial bodies like Earth, Mars, and Jupiter.
    </p>
    <ul>
      <li><strong>Mass of Earth:</strong> \( M_{\text{earth}} = 5.972 \times 10^{24} \, \text{kg} \)</li>
      <li><strong>Mass of Mars:</strong> \( M_{\text{mars}} = 6.4171 \times 10^{23} \, \text{kg} \)</li>
      <li><strong>Mass of Jupiter:</strong> \( M_{\text{jupiter}} = 1.898 \times 10^{27} \, \text{kg} \)</li>
      <li><strong>Radius of Earth:</strong> \( R_{\text{earth}} = 6.371 \times 10^6 \, \text{m} \)</li>
      <li><strong>Radius of Mars:</strong> \( R_{\text{mars}} = 3.396 \times 10^6 \, \text{m} \)</li>
      <li><strong>Radius of Jupiter:</strong> \( R_{\text{jupiter}} = 6.991 \times 10^7 \, \text{m} \)</li>
      <li><strong>Distance from Earth to Sun:</strong> \( R_{\text{earth-sun}} = 1.496 \times 10^{11} \, \text{m} \)</li>
    </ul>

    <p>
      Using these values, the first, second, and third cosmic velocities can be calculated for each body.
    </p>

    <h3>Graphical Representation of Cosmic Velocities</h3>
    <p>Below is a plot representing the first, second, and third cosmic velocities for Earth, Mars, and Jupiter.</p>
    <div id="plot"></div>
  </section>

  <section>
    <h2>📊 Graph Interpretation</h2>
    <p>
      The plot shows how the cosmic velocities change with respect to the size and mass of the celestial body. The first cosmic velocity (orbital velocity) is the smallest, followed by the second cosmic velocity (escape velocity). The third cosmic velocity, which is the largest, represents the velocity needed to escape the solar system.
    </p>
  </section>

  <section>
    <h2>🌍 Real-World Applications</h2>
    <p>
      Understanding these velocities is vital for space exploration. The first cosmic velocity is essential for satellite launches, while the second cosmic velocity is crucial for interplanetary missions. The third cosmic velocity is needed when considering travel beyond the solar system, such as interstellar exploration.
    </p>
  </section>

  <script>
    // This is a placeholder for the actual Plotly chart generation
    // You can add the necessary calculations and visualization code here.
  </script>

</body>
</html>
