# YOUGIFT-2025

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Secret Santa - Select Your Name</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #f8f9fa;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }
    .container {
      background: white;
      padding: 2em;
      border-radius: 16px;
      box-shadow: 0 8px 20px rgba(0,0,0,0.1);
      text-align: center;
    }
    select, button {
      font-size: 1em;
      padding: 0.5em;
      margin-top: 1em;
      border-radius: 8px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>🎁 Secret Santa Name Selection 🎄</h2>
    <p>Select your name from the dropdown below.</p>
    <select id="nameSelect">
      <option value="">-- Select Your Name --</option>
      <option value="Yasmine Benachenhou">Yasmine Benachenhou</option>
      <option value="Sarah Seladji">Sarah Seladji</option>
      <option value="Sirine Bassaid">Sirine Bassaid</option>
      <option value="Ryma Benmansour">Ryma Benmansour</option>
      <option value="So Fia">So Fia</option>
      <option value="Rayan Zekri">Rayan Zekri</option>
      <option value="Ines Bestaoui">Ines Bestaoui</option>
      <option value="Elissa Belarbi">Elissa Belarbi</option>
    </select><br>
    <button onclick="selectName()">Next: Spin the Wheel</button>
  </div>

  <script>
    function selectName() {
      const selectedName = document.getElementById("nameSelect").value;
      if (!selectedName) {
        alert("Please select your name!");
        return;
      }
      localStorage.setItem("selectedName", selectedName);  // Save selected name in localStorage
      window.location.href = "draw.html";  // Redirect to the draw page
    }
  </script>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Secret Santa - Draw Your Gift</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #f8f9fa;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }
    .container {
      background: white;
      padding: 2em;
      border-radius: 16px;
      box-shadow: 0 8px 20px rgba(0,0,0,0.1);
      text-align: center;
    }
    #box {
      width: 300px;
      height: 50px;
      overflow: hidden;
      margin: 20px auto;
      border: 2px solid #007bff;
      border-radius: 8px;
      position: relative;
    }
    #namesContainer {
      position: absolute;
      width: 100%;
      animation: none;
    }
    .name {
      height: 50px;
      line-height: 50px;
      font-size: 1em;
      color: #333;
      background-color: #e9f5ff;
    }
    @keyframes slideUpAnim {
      from { transform: translateY(0); }
      to { transform: translateY(-VAR_HEIGHTpx); }
    }
    button {
      font-size: 1em;
      padding: 0.5em;
      margin-top: 1em;
      border-radius: 8px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>🎁 Secret Santa Draw 🎄</h2>
    <p>Your selected name will be revealed below!</p>
    <div id="box">
      <div id="namesContainer"></div>
    </div>
    <button onclick="startDraw()">Start Draw</button>
    <div id="result"></div>
  </div>

  <script>
    const assignments = {
      "Yasmine Benachenhou": "Sarah Seladji",
      "Sarah Seladji": "Sirine Bassaid",
      "Sirine Bassaid": "Ryma Benmansour",
      "Ryma Benmansour": "So Fia",
      "So Fia": "Rayan Zekri",
      "Rayan Zekri": "Ines Bestaoui",
      "Ines Bestaoui": "Elissa Belarbi",
      "Elissa Belarbi": "Yasmine Benachenhou"
    };

    const allNames = Object.values(assignments);

    function startDraw() {
      const selectedName = localStorage.getItem("selectedName");  // Get the selected name from localStorage
      if (!selectedName) {
        alert("No name selected! Please go back and choose a name.");
        return;
      }

      const resultName = assignments[selectedName];
      const fakeNames = shuffle([...allNames, ...allNames]).filter(n => n !== resultName);
      fakeNames.push(resultName); // Add actual result at the end

      const namesContainer = document.getElementById("namesContainer");
      namesContainer.innerHTML = '';

      fakeNames.forEach(name => {
        const div = document.createElement("div");
        div.className = "name";
        div.textContent = name;
        namesContainer.appendChild(div);
      });

      const nameHeight = 50;
      const totalHeight = nameHeight * fakeNames.length;

      namesContainer.style.animation = `slideUpAnim 4s ease-in-out forwards`;
      const style = document.createElement('style');
      style.textContent = `
        @keyframes slideUpAnim {
          from { transform: translateY(0); }
          to { transform: translateY(-${totalHeight - nameHeight}px); }
        }
      `;
      document.head.appendChild(style);

      setTimeout(() => {
        document.getElementById("result").textContent = `🎁 You are gifting to: ${resultName}`;
      }, 4000);
    }

    function shuffle(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    }
  </script>
</body>
</html>

