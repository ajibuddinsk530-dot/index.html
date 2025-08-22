<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>টি ট্যাক টো | প্রিমিয়াম</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(to right, #f0fff0, #ccffcc);
      margin: 0;
      padding: 20px;
      text-align: center;
    }

    h1 {
      color: #0c7500;
      margin-bottom: 5px;
    }

    .scoreboard {
      display: flex;
      justify-content: space-around;
      margin: 10px auto;
      max-width: 320px;
      font-size: 16px;
      background: #e6ffe6;
      border-radius: 10px;
      padding: 8px;
      color: #006600;
    }

    #board {
      display: grid;
      grid-template-columns: repeat(3, 100px);
      grid-template-rows: repeat(3, 100px);
      gap: 10px;
      justify-content: center;
      margin: 20px auto;
    }

    .cell {
      background-color: white;
      border-radius: 15px;
      font-size: 2.5em;
      color: #009933;
      display: flex;
      align-items: center;
      justify-content: center;
      box-shadow: 2px 2px 8px rgba(0,0,0,0.2);
      cursor: pointer;
      transition: transform 0.2s;
    }

    .cell:hover {
      transform: scale(1.05);
    }

    #status {
      font-size: 20px;
      color: #004d00;
      margin-top: 10px;
    }

    button {
      margin-top: 15px;
      padding: 10px 20px;
      font-size: 16px;
      background: #00cc66;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
    }

    button:hover {
      background: #00994d;
    }

    .ad-container {
      margin-top: 25px;
    }
  </style>
</head>
<body>

  <h1>টি ট্যাক টো 🎮</h1>
  <div class="scoreboard">
    <div>❌ X: <span id="xScore">0</span></div>
    <div>⭕ O: <span id="oScore">0</span></div>
    <div>🎯 রাউন্ড: <span id="round">1</span></div>
  </div>

  <div id="board"></div>
  <div id="status">আপনার পালা: ❌</div>
  <button onclick="resetGame()">পরবর্তী রাউন্ড</button>

  <!-- ✅ Adsterra ad -->
  <div class="ad-container">
    <script type="text/javascript">
      atOptions = {
        'key' : 'efb975245dfcbf39d8bf2511556127d8',
        'format' : 'iframe',
        'height' : 50,
        'width' : 320,
        'params' : {}
      };
    </script>
    <script type="text/javascript" src="//www.highperformanceformat.com/efb975245dfcbf39d8bf2511556127d8/invoke.js"></script>
  </div>

  <!-- ✅ Sound Effects -->
  <audio id="clickSound" src="https://www.soundjay.com/buttons/sounds/button-16.mp3"></audio>
  <audio id="winSound" src="https://www.soundjay.com/misc/sounds/bell-ringing-05.mp3"></audio>
  <audio id="drawSound" src="https://www.soundjay.com/misc/sounds/fail-buzzer-01.mp3"></audio>

  <script>
    const board = document.getElementById("board");
    const statusText = document.getElementById("status");
    const xScore = document.getElementById("xScore");
    const oScore = document.getElementById("oScore");
    const roundDisplay = document.getElementById("round");
    const clickSound = document.getElementById("clickSound");
    const winSound = document.getElementById("winSound");
    const drawSound = document.getElementById("drawSound");

    let cells = [];
    let currentPlayer = "X";
    let gameActive = true;
    let scores = { X: 0, O: 0 };
    let round = 1;

    function checkWinner() {
      const combos = [
        [0,1,2], [3,4,5], [6,7,8],
        [0,3,6], [1,4,7], [2,5,8],
        [0,4,8], [2,4,6]
      ];

      for (let [a,b,c] of combos) {
        if (
          cells[a].textContent &&
          cells[a].textContent === cells[b].textContent &&
          cells[a].textContent === cells[c].textContent
        ) {
          statusText.textContent = `🎉 বিজয়ী: ${cells[a].textContent}`;
          scores[cells[a].textContent]++;
          updateScores();
          winSound.play();
          gameActive = false;
          return;
        }
      }

      if ([...cells].every(cell => cell.textContent)) {
        statusText.textContent = "🤝 ড্র হয়েছে!";
        drawSound.play();
        gameActive = false;
      }
    }

    function handleClick(e) {
      const cell = e.target;
      if (cell.textContent || !gameActive) return;

      cell.textContent = currentPlayer;
      clickSound.play();
      checkWinner();

      if (gameActive) {
        currentPlayer = currentPlayer === "X" ? "O" : "X";
        statusText.textContent = `আপনার পালা: ${currentPlayer === "X" ? "❌" : "⭕"}`;
      }
    }

    function updateScores() {
      xScore.textContent = scores["X"];
      oScore.textContent = scores["O"];
    }

    function resetGame() {
      cells.forEach(cell => cell.textContent = "");
      currentPlayer = round % 2 === 1 ? "X" : "O";
      statusText.textContent = `আপনার পালা: ${currentPlayer === "X" ? "❌" : "⭕"}`;
      gameActive = true;
      round++;
      roundDisplay.textContent = round;
    }

    // Setup board
    for (let i = 0; i < 9; i++) {
      const div = document.createElement("div");
      div.classList.add("cell");
      div.addEventListener("click", handleClick);
      board.appendChild(div);
      cells.push(div);
    }
  </script>
</body>
</html>
