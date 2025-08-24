<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>The Mathrix: a Staircase-Themed Puzzle for Learning Rational Equations, Function and One-to-one Functions</title>
<style>
  body {
    margin: 0;
    font-family: Arial, sans-serif;
    background-color: #0d0d0d;
    color: #00ffcc;
    display: flex;
    flex-direction: column;
    height: 100vh;
    font-size: 14px;
  }
  header {
    text-align: center;
    padding: 0.8rem;
    font-size: 0.9rem;
    background-color: #111;
    color: #ff00ff;
    text-shadow: 0 0 6px #ff00ff;
  }
  main {
    flex: 1;
    display: flex;
    flex-direction: column;
    padding: 0.5rem;
    gap: 0.5rem;
  }
  .terminal {
    flex: 1;
    background: #000;
    border: 2px solid #00ffcc;
    padding: 0.6rem;
    overflow-y: auto;
    box-shadow: 0 0 8px #00ffcc;
    font-size: 0.85rem;
    line-height: 1.4;
    white-space: pre-wrap;
  }
  .term-input {
    display: flex;
    gap: 0.4rem;
  }
  .term-input input {
    flex: 1;
    background: black;
    color: #fff;
    border: 2px solid #ff00ff;
    padding: 0.6rem;
    font-size: 0.85rem;
  }
  .term-input button {
    background: black;
    color: #fff;
    border: 2px solid #ff00ff;
    padding: 0.6rem;
    cursor: pointer;
    font-size: 0.85rem;
    transition: background 0.2s, color 0.2s;
  }
  .term-input button:hover {
    background: #ff00ff;
    color: black;
  }
</style>
</head>
<body>
  <header>🎮 The Mathrix: a Staircase-Themed Puzzle for Learning Rational Equations, Function and One-to-one Functions 🎮</header>
  <main>
    <div class="terminal" id="terminal"></div>
    <div class="term-input">
      <input id="termInput" placeholder="Type card number first" autocomplete="off">
      <button id="okBtn" type="button">OK</button>
    </div>
  </main>

<script>
const correctAnswers = {
  1: { question: "x/5 = 25", answers: ["125"] },
  2: { question: "x/4 = 30", answers: ["120"] },
  3: { question: "3x/2 = 180", answers: ["120"] },
  4: { question: "(x - 50)/5 = 20", answers: ["150"] },
  5: { question: "(x - 25)/2 = 60", answers: ["145"] },
  6: { question: "x/10 = 15", answers: ["150"] },
  7: { question: "5x/3 = 200", answers: ["120"] },
  8: { question: "(x - 100)/4 = 10", answers: ["140"] },
  9: { question: "(x - 20)/8 = 15", answers: ["140"] },
  10: { question: "2x/5 = 60", answers: ["150"] },

  11: { question: "x/(x - 40) = 4/3", answers: ["160"] },
  12: { question: "(x - 20)/(x - 10) = 2", answers: ["0"] },
  13: { question: "3x/(x - 80) = 5/2", answers: ["-400"] },
  14: { question: "(x - 50)/(x - 30) = 5/4", answers: ["-50"] },
  15: { question: "(x + 80)/(x - 20) = 7/5", answers: ["270"] },
  16: { question: "(4x - 200)/(x - 90) = 6", answers: ["170"] },
  17: { question: "(x + 50)/(x - 40) = 9/8", answers: ["760"] },
  18: { question: "(2x - 100)/(x - 20) = 4", answers: ["-40"] },
  19: { question: "(5x - 250)/(x - 25) = 10", answers: ["0"] },
  20: { question: "(x - 100)/(x - 50) = 3/2", answers: ["-50"] },

  21: { question: "Find x if f(x) = 5x − 7 and f(a) = 18.", answers: ["x = 5", "5"] },
  22: { question: "Find x if f(x) = 2x + 9 and f(a) = −5.", answers: ["x = -7", "-7"] },
  23: { question: "Find x if f(x) = x/4 + 6 and f(a) = 10.", answers: ["x = 16", "16"] },
  24: { question: "Find x if f(x) = 7 − 3x and f(a) = −11.", answers: ["x = 6", "6"] },
  25: { question: "Find x if f(x) = 4x² and f(a) = 64.", answers: ["x = 4 or -4","4 or -4","4","-4"] },

  36: { question: "f(x) = (x + 5)/(x - 2)\nFind the vertical asymptote.", answers: ["x=2", "x = 2"] },
  37: { question: "f(x) = (3x)/(x² - 1)\nFind the horizontal asymptote.", answers: ["y=0","y = 0"] },
  38: { question: "f(x) = (2x - 1)/(x + 3)\nFind the x-intercept.", answers: ["(0.5,0)","x=0.5","x = 0.5"] },
  39: { question: "f(x) = 4/(x² - 9)\nFind the y-intercept.", answers: ["(0, -4/9)"] },
  40: { question: "f(x) = (x² - 4)/(x - 2)\nFind the hole (x-coordinate only).", answers: ["x=2","x = 2"] }
};

let currentCard = null;
let waitingForAnswer = false;
let terminal, termInput, okBtn;

function appendLine(text) {
  const div = document.createElement("div");
  div.textContent = text;
  terminal.appendChild(div);
  setTimeout(() => {
    terminal.scrollTop = terminal.scrollHeight;
  }, 0);
}

function printWelcome() {
  appendLine("Welcome to The Mathrix!");
  appendLine("Step 1: Type the card number to see the question.");
  appendLine("Step 2: Type the answer exactly (symbols matter: ², ³, ≥, ≤).");
  appendLine("Step 3: If wrong, retry or type a new card number.");
}

function handleCardNumber(input) {
  const numStr = input.trim();
  if (!/^\d+$/.test(numStr)) {
    appendLine(`❌ Card ${input} not found.`);
    resetState();
    return;
  }
  const num = parseInt(numStr, 10);
  if (!correctAnswers[num]) {
    appendLine(`❌ Card ${input} not found.`);
    resetState();
    return;
  }
  currentCard = num;
  waitingForAnswer = true;
  appendLine(`📜 Card ${num}: ${correctAnswers[num].question}`);
  appendLine("Now type your answer (or type another card number):");
  termInput.placeholder = "Type your answer or another card";
}

function handleAnswer(input) {
  const trimmed = input.trim();
  if (/^\d+$/.test(trimmed) && correctAnswers[parseInt(trimmed, 10)]) {
    handleCardNumber(trimmed);
    return;
  }
  if (currentCard === null) {
    appendLine("❌ Please select a card first.");
    resetState();
    return;
  }
  const answer = trimmed.toLowerCase();
  const validAnswers = correctAnswers[currentCard].answers.map(a => a.toLowerCase());
  if (validAnswers.includes(answer)) {
    appendLine("✅ Correct!");
    resetState();
    appendLine("Type another card number to continue.");
  } else {
    appendLine("❌ Wrong. Try again, or type a different card number.");
    termInput.placeholder = "Retry answer or type new card";
  }
}

function sendCommand() {
  const value = termInput.value.trim();
  if (!value) return;
  appendLine("> " + value);

  if (!waitingForAnswer) {
    handleCardNumber(value);
  } else {
    handleAnswer(value);
  }
  termInput.value = "";
  termInput.focus();
}

function resetState() {
  currentCard = null;
  waitingForAnswer = false;
  termInput.placeholder = "Type card number first";
}

window.onload = function() {
  terminal = document.getElementById("terminal");
  termInput = document.getElementById("termInput");
  okBtn = document.getElementById("okBtn");

  printWelcome();

  okBtn.addEventListener("click", sendCommand);
  termInput.addEventListener("keydown", e => {
    if (e.key === "Enter") sendCommand();
  });
  termInput.focus();
};
</script>
</body>
</html>
