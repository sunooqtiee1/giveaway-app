<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Twitter Giveaway</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body { font-family: sans-serif; margin: 0; padding: 0; background: #f2f2f2; }
    .container { max-width: 500px; margin: 30px auto; padding: 20px; background: white; border-radius: 12px; box-shadow: 0 4px 12px rgba(0,0,0,0.1); }
    h2 { margin-top: 0; }
    input[type="text"], input[type="file"], button {
      width: 100%; padding: 10px; margin-top: 10px; border: 1px solid #ccc;
      border-radius: 8px; font-size: 16px;
    }
    button { background: #007bff; color: white; border: none; cursor: pointer; }
    button:hover { background: #0056b3; }
    .entry { margin: 10px 0; border-bottom: 1px solid #eee; padding-bottom: 10px; }
    .entry img { max-width: 100%; margin-top: 5px; border-radius: 8px; }
    .success { color: green; margin-top: 10px; }
    .winner-box { background: #d4edda; padding: 10px; margin-top: 15px; border-radius: 8px; }
  </style>
</head>
<body>

<div class="container" id="joinForm">
  <h2>📝 Join the Giveaway</h2>
  <input type="text" id="username" placeholder="@yourusername">
  <input type="file" id="proof" accept="image/*">
  <button onclick="submitEntry()">Submit Entry</button>
  <div class="success" id="entryMessage"></div>
</div>

<div class="container" id="hostDashboard">
  <h2>🎯 Host Dashboard</h2>
  <button onclick="drawWinner()">🎲 Draw Winner</button>
  <div class="winner-box" id="winnerBox"></div>
  <h3>📋 Entries:</h3>
  <div id="entryList"></div>
</div>

<script>
  let entries = JSON.parse(localStorage.getItem("entries") || "[]");
  let winner = localStorage.getItem("winner");

  function submitEntry() {
    const username = document.getElementById("username").value.trim();
    const file = document.getElementById("proof").files[0];
    if (!username || !file) {
      alert("Please provide both your @username and proof image.");
      return;
    }

    const reader = new FileReader();
    reader.onload = function(event) {
      const newEntry = {
        username,
        proof: event.target.result
      };
      entries.push(newEntry);
      localStorage.setItem("entries", JSON.stringify(entries));
      document.getElementById("entryMessage").innerText = "✅ You're entered!";
      document.getElementById("username").value = "";
      document.getElementById("proof").value = "";
      renderEntries();
    };
    reader.readAsDataURL(file);
  }

  function renderEntries() {
    const entryList = document.getElementById("entryList");
    entryList.innerHTML = "";
    entries.forEach((entry, index) => {
      const div = document.createElement("div");
      div.className = "entry";
      div.innerHTML = `<strong>${index + 1}. ${entry.username}</strong><br><img src="${entry.proof}" alt="proof" />`;
      entryList.appendChild(div);
    });
  }

  function drawWinner() {
    if (winner) {
      alert("Winner already drawn!");
      return;
    }
    if (entries.length === 0) {
      alert("No entries available.");
      return;
    }
    const randomIndex = Math.floor(Math.random() * entries.length);
    winner = entries[randomIndex].username;
    localStorage.setItem("winner", winner);
    showWinner();
  }

  function showWinner() {
    if (winner) {
      document.getElementById("winnerBox").innerHTML = `🎉 Winner: <strong>${winner}</strong>`;
    }
  }

  renderEntries();
  showWinner();
</script>

</body>
</html>
