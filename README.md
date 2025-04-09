# SNAKE-AND-LADDER
JOM TEKA SAMBIL MAIN
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Snake and Ladder Quiz Game</title>
  <style>
    body { font-family: sans-serif; text-align: center; }
    #board { display: grid; grid-template-columns: repeat(10, 40px); gap: 2px; margin: 20px auto; }
    .cell {
      width: 40px; height: 40px;
      border: 1px solid #ccc;
      display: flex; justify-content: center; align-items: center;
      font-size: 12px;
      position: relative;
    }
    .player1 { background-color: rgba(255,0,0,0.6); border-radius: 50%; width: 20px; height: 20px; }
    .player2 { background-color: rgba(0,0,255,0.6); border-radius: 50%; width: 20px; height: 20px; position: absolute; bottom: 2px; right: 2px; }
    .quiz { background-color: #ffffcc; }
    #dice { margin: 10px; font-size: 20px; }
  </style>
</head>
<body>
  <h1>Snake and Ladder - Kuiz Haiwan 🐾</h1>
  <div id="board"></div>
  <div>
    <button onclick="rollDice()">🎲 Roll Dice</button>
    <div id="dice"></div>
    <p>Turn: <span id="turn">Player 1</span></p>
  </div>

  <script>
    const board = document.getElementById("board");
    const diceDiv = document.getElementById("dice");
    const turnSpan = document.getElementById("turn");

    const quizTiles = {
      3: ["The king of the jungle", "Lion"],
      8: ["Black and white striped animal, similar to a horse", "Zebra"],
      14: ["Big heavy animal that loves water", "Hippopotamus"],
      21: ["Tallest land animal", "Giraffe"],
      27: ["Similar to a Crocodile", "Alligator"],
      33: ["Intelligent, social primate", "Chimpanzee"],
      39: ["The biggest animal", "Blue Whale"],
      45: ["A big cat with orange fur and black stripes", "Tiger"],
      52: ["Legless reptile", "Snake"],
      66: ["The fastest land animal", "Cheetah"]
    };

    const positions = [0, 0];
    let currentPlayer = 0;

    // Create board
    for (let i = 100; i >= 1; i--) {
      const cell = document.createElement("div");
      cell.className = "cell";
      cell.id = `cell-${i}`;
      cell.innerText = i;
      if (quizTiles[i]) cell.classList.add("quiz");
      board.appendChild(cell);
    }

    function drawPlayers() {
      document.querySelectorAll(".player1, .player2").forEach(p => p.remove());
      positions.forEach((pos, idx) => {
        if (pos === 0) return;
        const piece = document.createElement("div");
        piece.className = idx === 0 ? "player1" : "player2";
        const cell = document.getElementById(`cell-${pos}`);
        cell.appendChild(piece);
      });
    }

    function rollDice() {
      const roll = Math.floor(Math.random() * 6) + 1;
      diceDiv.innerText = `You rolled: ${roll}`;

      positions[currentPlayer] += roll;
      if (positions[currentPlayer] > 100) positions[currentPlayer] = 100;

      const pos = positions[currentPlayer];
      drawPlayers();

      if (quizTiles[pos]) {
        const [question, answer] = quizTiles[pos];
        const userAnswer = prompt(`📍 Petak ${pos} - ${question}`);
        if (userAnswer && userAnswer.trim().toLowerCase() === answer.toLowerCase()) {
          alert("✅ Betul! Teruskan perjalanan.");
        } else {
          alert(`❌ Salah. Jawapan betul: ${answer}. Anda undur 1 petak.`);
          positions[currentPlayer] = Math.max(1, positions[currentPlayer] - 1);
        }
        drawPlayers();
      }

      if (positions[currentPlayer] === 100) {
        alert(`🎉 Player ${currentPlayer + 1} menang!`);
        return;
      }

      currentPlayer = 1 - currentPlayer;
      turnSpan.innerText = `Player ${currentPlayer + 1}`;
    }

    drawPlayers();
  </script>
</body>
</html>
