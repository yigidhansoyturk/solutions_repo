<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>📊 Central Limit Theorem Simulation</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/10.0.0/math.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background-color: #f5f7fa;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 40px;
    }
    .container {
      background-color: #ffffff;
      padding: 30px;
      border-radius: 10px;
      box-shadow: 0px 10px 30px rgba(0, 0, 0, 0.1);
      width: 100%;
      max-width: 1000px;
    }
    h1, h2 {
      color: #2c3e50;
      margin-bottom: 10px;
    }
    p {
      margin-bottom: 15px;
      line-height: 1.6;
    }
    select, input {
      margin-right: 10px;
      padding: 6px 12px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
    button {
      background-color: #3498db;
      color: white;
      border: none;
      padding: 10px 15px;
      border-radius: 5px;
      cursor: pointer;
    }
    canvas {
      max-width: 100%;
      margin-top: 30px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>📊 Exploring the Central Limit Theorem</h1>
    <p>
      Welcome! This interactive tool is designed to help you understand one of the most important ideas in statistics — the Central Limit Theorem (CLT).
      The CLT explains why many distributions in the real world tend to be normal (bell-shaped) when we average things out. Whether we are measuring manufacturing defects,
      average scores, or experimental results, the mean of those measurements will often look normal — even if the original data does not.
    </p>
    <p>
      Use the options below to select a population distribution and simulate the process of sampling from it. You’ll observe how the sample means begin to form a normal distribution as you increase the sample size.
    </p>

    <h2>🎯 Choose Distribution and Parameters</h2>
    <p>
      Population Type:
      <select id="distribution">
        <option value="uniform">Uniform</option>
        <option value="exponential">Exponential</option>
        <option value="binomial">Binomial</option>
      </select>
      Sample Size:
      <input type="number" id="sampleSize" value="30" min="1" max="1000">
      Samples Count:
      <input type="number" id="samples" value="1000" min="10" max="10000">
      <button onclick="simulate()">Run Simulation</button>
    </p>

    <canvas id="histogram"></canvas>
    <p id="summary"></p>

    <h2>🧠 Why This Matters</h2>
    <p>This interactive visualization shows how sample means form a normal distribution even when starting from highly skewed or discrete populations. The CLT underpins confidence intervals, hypothesis testing, and is vital in statistical modeling.</p>
  </div>

  <script>
    function generatePopulation(dist, size) {
      const data = [];
      if (dist === 'uniform') {
        for (let i = 0; i < size; i++) data.push(Math.random());
      } else if (dist === 'exponential') {
        for (let i = 0; i < size; i++) data.push(-Math.log(1 - Math.random()));
      } else if (dist === 'binomial') {
        for (let i = 0; i < size; i++) {
          let sum = 0;
          for (let j = 0; j < 10; j++) sum += Math.random() < 0.5 ? 1 : 0;
          data.push(sum);
        }
      }
      return data;
    }

    function simulate() {
      const distribution = document.getElementById('distribution').value;
      const sampleSize = parseInt(document.getElementById('sampleSize').value);
      const samples = parseInt(document.getElementById('samples').value);
      const means = [];

      for (let i = 0; i < samples; i++) {
        const pop = generatePopulation(distribution, sampleSize);
        const mean = pop.reduce((a, b) => a + b, 0) / sampleSize;
        means.push(mean);
      }

      const ctx = document.getElementById('histogram').getContext('2d');
      if (window.histChart) window.histChart.destroy();

      const bins = 40;
      const min = Math.min(...means);
      const max = Math.max(...means);
      const step = (max - min) / bins;
      const labels = Array.from({ length: bins }, (_, i) => (min + i * step).toFixed(2));
      const frequencies = Array(bins).fill(0);
      means.forEach(m => {
        const index = Math.min(Math.floor((m - min) / step), bins - 1);
        frequencies[index]++;
      });

      window.histChart = new Chart(ctx, {
        type: 'bar',
        data: {
          labels: labels,
          datasets: [{
            label: 'Sample Means',
            data: frequencies,
            backgroundColor: 'rgba(52, 152, 219, 0.5)',
            borderColor: 'rgba(41, 128, 185, 1)',
            borderWidth: 1
          }]
        },
        options: {
          scales: {
            x: {
              title: {
                display: true,
                text: 'Sample Mean'
              }
            },
            y: {
              title: {
                display: true,
                text: 'Frequency'
              }
            }
          },
          plugins: {
            title: {
              display: true,
              text: `Sampling Distribution of the Mean (${distribution})`
            }
          }
        }
      });

      const avg = (means.reduce((a, b) => a + b, 0) / samples).toFixed(3);
      const variance = (means.reduce((a, b) => a + Math.pow(b - avg, 2), 0) / samples).toFixed(3);
      document.getElementById('summary').innerText = `Average of sample means: ${avg}, Variance: ${variance}`;
    }
  </script>
</body>
</html>
