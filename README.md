<!DOCTYPE html>
<html>
<head>
  <title>Gioco di Matematica</title>
  <style>
    body { font-family: Arial, sans-serif; text-align: center; background: #f5f5f5; }
    #container { background: white; padding: 25px; width: 350px; margin: 40px auto; border-radius: 12px; box-shadow: 0 0 10px rgba(0,0,0,0.2); }
    button { padding: 12px 20px; margin: 5px; font-size: 18px; cursor: pointer; border-radius: 8px; }
    input { padding: 10px; width: 100px; font-size: 20px; text-align: center; }
    h1 { color: #333; }
    .easy { background: #90ee90; }
    .medium { background: #ffd966; }
    .hard { background: #ff6961; }
  </style>
</head>
<body>

<div id="container">
  <h1>Gioco di Matematica</h1>

  <h3>Scegli la difficoltà:</h3>
  <button class="easy" onclick="startGame('easy')">Facile</button>
  <button class="medium" onclick="startGame('medium')">Medio</button>
  <button class="hard" onclick="startGame('hard')">Difficile</button>

  <div id="game" style="display:none;">
    <h2 id="question"></h2>
    <input id="answer" type="number">
    <br><br>
    <button onclick="checkAnswer()">Invia</button>

    <p id="score">Punteggio: 0</p>
    <p id="timer">Tempo rimasto: 60s</p>
  </div>
</div>

<script>
  let score = 0;
  let correct;
  let time = 60;
  let level;
  let timer;

  function startGame(selectedLevel) {
    level = selectedLevel;
    score = 0;
    time = 60;

    document.getElementById("score").innerText = "Punteggio: 0";
    document.getElementById("timer").innerText = "Tempo rimasto: 60s";

    document.getElementById("game").style.display = "block";

    newQuestion();

    clearInterval(timer);
    timer = setInterval(() => {
      time--;
      document.getElementById("timer").innerText = "Tempo rimasto: " + time + "s";

      if (time <= 0) {
        clearInterval(timer);
        alert("Tempo scaduto! Punteggio finale: " + score);
      }
    }, 1000);
  }

  function newQuestion() {
    let num1 = Math.floor(Math.random() * 10) + 1;
    let num2 = Math.floor(Math.random() * 10) + 1;
    let operation;

    if (level === "easy") {
      operation = "+";
      correct = num1 + num2;
    } 
    else if (level === "medium") {
      let rand = Math.random();
      if (rand < 0.5) {
        operation = "+";
        correct = num1 + num2;
      } else {
        operation = "-";
        correct = num1 - num2;
      }
    }
    else if (level === "hard") {
      let rand = Math.random();
      if (rand < 0.25) { operation = "+"; correct = num1 + num2; }
      else if (rand < 0.50) { operation = "-"; correct = num1 - num2; }
      else if (rand < 0.75) { operation = "×"; correct = num1 * num2; }
      else { 
        operation = "÷";
        num2 = Math.floor(Math.random() * 9) + 1; 
        num1 = num2 * Math.floor(Math.random() * 10 + 1);
        correct = num1 / num2;
      }
    }

    document.getElementById("question").innerText = num1 + " " + operation + " " + num2 + " = ?";
  }

  function checkAnswer() {
    let user = parseInt(document.getElementById("answer").value);

    if (user === correct) {
      score++;
      alert("Corretto!");
    } else {
      alert("Sbagliato! La risposta era: " + correct);
    }

    document.getElementById("score").innerText = "Punteggio: " + score;
    document.getElementById("answer").value = "";
    newQuestion();
  }
</script>

</body>
</html>
