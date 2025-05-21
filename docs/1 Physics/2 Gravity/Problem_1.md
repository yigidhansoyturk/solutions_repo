<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kepler's Third Law: Orbital Mechanics</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/three@0.150.1/build/three.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/three@0.150.1/examples/js/controls/OrbitControls.js"></script>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
  <script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
      line-height: 1.6;
      background: #f9f9f9;
      color: #222;
      transition: background 0.4s, color 0.4s;
    }
    .dark-mode {
      background: #121212;
      color: #f0f0f0;
    }
    h1, h2 {
      color: #003366;
    }
    .dark-mode h1, .dark-mode h2 {
      color: #66ccff;
    }
    section {
      background: #fff;
      padding: 20px;
      margin: 20px 0;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      transition: background 0.4s, color 0.4s;
    }
    .dark-mode section {
      background: #1e1e1e;
      box-shadow: 0 0 10px rgba(255, 255, 255, 0.05);
    }
    canvas { margin-top: 20px; }
    button {
      margin: 10px 5px;
      padding: 10px 20px;
      font-size: 14px;
      cursor: pointer;
      border: none;
      border-radius: 5px;
      background-color: #003366;
      color: white;
      transition: background 0.3s;
    }
    .dark-mode button {
      background-color: #66ccff;
      color: #000;
    }
    #orbit3D {
      width: 100%;
      height: 400px;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <button onclick="toggleDarkMode()">Toggle Dark Mode</button>
  <button onclick="addOrbit()">Add Orbit</button>
  <h1>Kepler's Third Law and Orbital Mechanics</h1>

  <section>
    <h2>3D Orbital Animation</h2>
    <div id="orbit3D"></div>
    <script>
      let scene, camera, renderer, controls;
      let planets = [], angles = [], orbits = [];

      function initScene() {
        scene = new THREE.Scene();
        const container = document.getElementById('orbit3D');
        const width = container.clientWidth;
        const height = container.clientHeight;

        camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 1000);
        camera.position.set(0, 2, 6);

        renderer = new THREE.WebGLRenderer({ antialias: true });
        renderer.setSize(width, height);
        container.appendChild(renderer.domElement);

        controls = new THREE.OrbitControls(camera, renderer.domElement);
        controls.enableDamping = true;
        controls.dampingFactor = 0.05;
        controls.enableZoom = true;

        const starGeometry = new THREE.SphereGeometry(0.2, 32, 32);
        const starMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 });
        const star = new THREE.Mesh(starGeometry, starMaterial);
        scene.add(star);
      }

      function addOrbit() {
        const base = 2;
        const radius = base + orbits.length * 0.5;
        const geometry = new THREE.SphereGeometry(0.05, 32, 32);
        const material = new THREE.MeshBasicMaterial({ color: new THREE.Color(`hsl(${orbits.length * 72}, 100%, 50%)`) });
        const planet = new THREE.Mesh(geometry, material);
        scene.add(planet);
        planets.push({ mesh: planet, radius: radius });
        angles.push(0);
        orbits.push(radius);

        const segments = 64;
        const orbitGeometry = new THREE.BufferGeometry();
        const points = [];
        for (let j = 0; j <= segments; j++) {
          const theta = (j / segments) * 2 * Math.PI;
          points.push(new THREE.Vector3(radius * Math.cos(theta), 0, radius * Math.sin(theta)));
        }
        orbitGeometry.setFromPoints(points);
        const orbitMaterial = new THREE.LineBasicMaterial({ color: 0x888888 });
        const orbitLine = new THREE.LineLoop(orbitGeometry, orbitMaterial);
        scene.add(orbitLine);
      }

      function animate() {
        requestAnimationFrame(animate);
        planets.forEach((p, i) => {
          angles[i] += 0.01 / Math.sqrt(p.radius);
          p.mesh.position.x = p.radius * Math.cos(angles[i]);
          p.mesh.position.z = p.radius * Math.sin(angles[i]);
        });
        controls.update();
        renderer.render(scene, camera);
      }

      window.addEventListener('resize', () => {
        const container = document.getElementById('orbit3D');
        const width = container.clientWidth;
        const height = container.clientHeight;
        camera.aspect = width / height;
        camera.updateProjectionMatrix();
        renderer.setSize(width, height);
      });

      initScene();
      addOrbit();
      animate();
    </script>
  </section>

  <script>
    function toggleDarkMode() {
      document.body.classList.toggle('dark-mode');
    }
  </script>
</body>
</html>
