<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>The Mathrix: a Staircase-Themed Puzzle for Learning Rational Equation, Inequality and Functions</title>
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
  <header>🎮 The Mathrix: a Staircase-Themed Puzzle for Learning Rational Equation, Inequality and Functions 🎮</header>
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
  12: { question: "(x - 20)/(x - 10) = 2", answers: ["100"] },
  13: { question: "3x/(x - 80) = 5/2", answers: ["200"] },
  14: { question: "(x - 50)/(x - 30) = 5/4", answers: ["170"] },
  15: { question: "(x + 80)/(x - 20) = 7/5", answers: ["150"] },
  16: { question: "(4x - 200)/(x - 90) = 6", answers: ["140"] },
  17: { question: "(x + 50)/(x - 40) = 9/8", answers: ["130"] },
  18: { question: "(2x - 100)/(x - 20) = 4", answers: ["120"] },
  19: { question: "(5x - 250)/(x - 25) = 10", answers: ["150"] },
  20: { question: "(x - 100)/(x - 50) = 3/2", answers: ["200"] },

  21: { question: "(x - 120)/(x - 50) > 0", answers: ["x > 120"] },
  22: { question: "(x - 150)/(x - 100) ≥ 0", answers: ["x ≥ 150"] },
  23: { question: "(x - 200)/(x - 80) < 0", answers: ["100 ≤ x < 200"] },
  24: { question: "((x - 140)(x - 60))/(x - 90) ≤ 0", answers: ["100 ≤ x ≤ 140"] },
  25: { question: "(x - 100)/(x - 200) > 0", answers: ["x > 200"] },
  26: { question: "(x - 300)/(x - 150) ≥ 0", answers: ["x ≥ 300"] },
  27: { question: "((x - 250)(x - 50))/(x - 180) < 0", answers: ["100 ≤ x < 180", "x > 250"] },
  28: { question: "(x - 400)/(x - 200) > 0", answers: ["x > 400"] },
  29: { question: "((x - 125)^2)/(x - 100) ≥ 0", answers: ["x > 100"] },
  30: { question: "(x - 500)/(x - 300) ≤ 0", answers: ["100 ≤ x ≤ 500"] },

  31: { question: "(x² - 14400)/(x - 100) = 140", answers: ["220"] },
  32: { question: "(x² - 19600)/(x - 150) = 200", answers: ["250"] },
  33: { question: "(x² - 12100)/(x - 100) = 110", answers: ["210"] },
  34: { question: "(x² - 22500)/(x - 125) = 175", answers: ["250"] },
  35: { question: "(x² - 40000)/(x - 200) = 300", answers: ["400"] },
  36: { question: "(x² - 25600)/(x - 160) = 180", answers: ["340"] },
  37: { question: "(x² - 10000)/(x - 80) = 120", answers: ["200"] },
  38: { question: "(x² - 4900)/(x - 70) = 130", answers: ["200"] },
  39: { question: "(x² - 32400)/(x - 180) = 240", answers: ["300"] },
  40: { question: "(x² - 15625)/(x - 125) = 175", answers: ["250"] },

  41: { question: "(x³ - 64000)/(x - 40) = 5000", answers: ["140"] },
  42: { question: "((x - 100)(x - 300))/(x - 150) = 200", answers: ["400"] },
  43: { question: "(x² - 40000)/(x - 250) = 150", answers: ["350"] },
  44: { question: "(2x² - 50000)/(x - 200) = 400", answers: ["300"] },
  45: { question: "((x - 120)(x - 200))/(x - 150) = 150", answers: ["270"] },
  46: { question: "(x² - 14400)/(x - 80) = 220", answers: ["200"] },
  47: { question: "(3x² - 90000)/(x - 300) = 600", answers: ["400"] },
  48: { question: "(x² - 16900)/(x - 130) = 150", answers: ["260"] },
  49: { question: "((x - 200)(x - 500))/(x - 400) = 300", answers: ["800"] },
  50: { question: "(x² - 62500)/(x - 250) = 200", answers: ["450"] }
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
  appendLine("Step 3: If your answer is wrong, retry or type a new card number.");
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
  // allow switching card mid-answer
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

window.onload = () => {
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
