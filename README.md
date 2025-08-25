<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
<title>The Mathrix: a Staircase-Themed Puzzle</title>
<link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap" rel="stylesheet">
<style>
  body {
    margin: 0;
    font-family: "Press Start 2P", monospace, Arial, sans-serif;
    background: radial-gradient(circle at top, #2a003f, #0d001a);
    color: #e0b3ff;
    text-align: center;
    padding: 8px;
    max-width: 480px;
    margin: auto;
  }
  h1 {
    color: #ff66ff;
    font-size: 4vw;
    text-shadow: 0 0 5px #ff00ff, 0 0 10px #cc00cc;
    margin-bottom: 10px;
    line-height: 1.3;
  }
  p {
    font-size: 3.2vw;
    margin-bottom: 12px;
    color: #80ffff;
    text-shadow: 0 0 5px #00ffff;
  }
  .sentence {
    margin: 10px auto;
    font-size: 3vw;
    background: rgba(20, 0, 40, 0.9);
    padding: 10px;
    border: 2px solid #ff00cc;
    border-radius: 12px;
    display: block;
    max-width: 95%;
    box-shadow: 0 0 12px #ff00cc, 0 0 18px #800080;
    color: #ffffff;
  }
  input {
    padding: 6px;
    font-size: 3vw;
    border-radius: 6px;
    border: 2px solid #00ffff;
    outline: none;
    margin: 5px auto;
    background: #1a0033;
    color: #fff;
    width: 85%;
    display: block;
    box-shadow: 0 0 8px #00ffff, 0 0 12px #0088ff inset;
    text-align: center;
  }
  input::placeholder { color: #ccccff; }
  .result {
    display: block;
    font-weight: bold;
    font-size: 3vw;
    margin-top: 3px;
  }
  button {
    background: linear-gradient(135deg, #ff00cc, #6600ff);
    color: #fff;
    font-size: 3.5vw;
    padding: 10px 18px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    margin-top: 14px;
    text-transform: uppercase;
    font-weight: bold;
    box-shadow: 0 0 12px #ff00ff, 0 0 16px #6600ff;
  }
  button:hover {
    background: linear-gradient(135deg, #ff33ff, #8000ff);
    box-shadow: 0 0 16px #ff33ff, 0 0 20px #8000ff;
  }
  #score {
    margin-top: 14px;
    font-size: 3.5vw;
    color: #80ffff;
    text-shadow: 0 0 6px #00ffff;
  }
  /* --- Terminal Section --- */
  .terminal-container {
    margin-top: 20px;
    text-align: left;
  }
  .terminal {
    background: #000;
    border: 2px solid #00ffcc;
    padding: 0.6rem;
    overflow-y: auto;
    height: 220px;
    box-shadow: 0 0 8px #00ffcc;
    font-size: 0.9rem;
    line-height: 1.4;
    white-space: pre-wrap;
    color: #00ffcc;
  }
  .term-input {
    display: flex;
    gap: 0.4rem;
    margin-top: 5px;
  }
  .term-input input {
    flex: 1;
    background: black;
    color: #fff;
    border: 2px solid #ff00ff;
    padding: 0.4rem;
    font-size: 0.9rem;
  }
  .term-input button {
    background: black;
    color: #fff;
    border: 2px solid #ff00ff;
    padding: 0.4rem;
    cursor: pointer;
    font-size: 0.9rem;
  }
  .term-input button:hover {
    background: #ff00ff;
    color: black;
  }
</style>
</head>
<body>
  <h1>The Mathrix: a Staircase-Themed Puzzle</h1>

  <!-- Terminal Game FIRST -->
  <p>🎮 Step 1: Enter the <b>Mathrix Terminal</b> to solve equations & functions 🎮</p>
  <div class="terminal-container">
    <div class="terminal" id="terminal"></div>
    <div class="term-input">
      <input id="termInput" placeholder="Type card number first" autocomplete="off" />
      <button id="okBtn" type="button">OK</button>
    </div>
  </div>

  <!-- Fill in the Blank SECOND -->
  <p>🎮 Step 2: Climb the staircase by filling in the blanks! 🎮</p>
  <div class="sentence">
    Math makes 
    <input type="text" id="blank0" placeholder="Noun (math)"> <span id="result0" class="result"></span>
    <input type="text" id="blank1" placeholder="Adjective"> <span id="result1" class="result"></span>, 
    so keep 
    <input type="text" id="blank2" placeholder="Verb-ing (action)"> <span id="result2" class="result"></span> 
    and you will 
    <input type="text" id="blank3" placeholder="Verb"> <span id="result3" class="result"></span> 
    the 
    <input type="text" id="blank4" placeholder="Noun (arcade/math)"> <span id="result4" class="result"></span>!
  </div>
  <button onclick="checkAnswers()">Check Answers</button>
  <div id="score"></div>

<script>
/* --- Terminal Game --- */
function normalize(s) {
  return s.toLowerCase()
          .replace(/\s+/g,'')
          .replace(/−/g,'-')
          .replace(/\u200b/g,'')
          .replace(/\bor\b/g,'')
          .replace(/[()]/g,'')
          .replace(/±/g,'');
}

const correctAnswers = {
  1:{question:"x/5 = 25",answers:["125"]},
  2:{question:"x/4 = 30",answers:["120"]},
  3:{question:"3x/2 = 180",answers:["120"]},
  4:{question:"(x - 50)/5 = 20",answers:["150"]},
  5:{question:"(x - 25)/2 = 60",answers:["145"]},
  6:{question:"x/10 = 15",answers:["150"]},
  7:{question:"5x/3 = 200",answers:["120"]},
  8:{question:"(x - 100)/4 = 10",answers:["140"]},
  9:{question:"(x - 20)/8 = 15",answers:["140"]},
  10:{question:"2x/5 = 60",answers:["150"]},

  11:{question:"x/(x - 40) = 4/3",answers:["160"]},
  12:{question:"(x - 20)/(x - 10) = 2",answers:["0"]},
  13:{question:"3x/(x - 80) = 5/2",answers:["-400"]},
  14:{question:"(x - 50)/(x - 30) = 5/4",answers:["-50"]},
  15:{question:"(x + 80)/(x - 20) = 7/5",answers:["270"]},
  16:{question:"(4x - 200)/(x - 90) = 6",answers:["170"]},
  17:{question:"(x + 50)/(x - 40) = 9/8",answers:["760"]},
  18:{question:"(2x - 100)/(x - 20) = 4",answers:["-40"]},
  19:{question:"(5x - 250)/(x - 25) = 10",answers:["0"]},
  20:{question:"(x - 100)/(x - 50) = 3/2",answers:["-50"]},

  21:{question:"f(x) = 5x − 7; f(a) = 18",answers:["5"]},
  22:{question:"f(x) = 2x + 9; f(a) = −5",answers:["-7"]},
  23:{question:"f(x) = x/4 + 6; f(a) = 10",answers:["16"]},
  24:{question:"f(x) = 7 − 3x; f(a) = −11",answers:["6"]},
  25:{question:"f(x) = 4x²; f(a) = 64",answers:["4","-4","±4"]},
  26:{question:"f(x) = x² − 5; f(a) = 20",answers:["5","-5","±5"]},
  27:{question:"f(x) = (2x + 3)/5; f(a) = 7",answers:["16"]},
  28:{question:"f(x) = (x − 4)/2; f(a) = −6",answers:["-8"]},
  29:{question:"f(x) = (x − 2)²; f(a) = 49",answers:["9","-5"]},
  30:{question:"f(x) = 3x² + 2; f(a) = 29",answers:["3","-3","±3"]},
  31:{question:"f(x) = (x + 5)/3 − 2; f(a) = 4",answers:["13"]},
  32:{question:"f(x) = 10 − x/2; f(a) = 1",answers:["18"]},
  33:{question:"f(x) = x² − x; f(a) = 12",answers:["4","-3"]},
  34:{question:"f(x) = 5(x − 1)² − 3; f(a) = 17",answers:["3","-1"]},
  35:{question:"f(x) = (4x − 1)/(x + 2); f(a) = 3",answers:["7"]},

  36:{question:"f(x) = (x + 5)/(x - 2)\nVertical asymptote?",answers:["x=2","2"]},
  37:{question:"f(x) = (3x)/(x^2 - 1)\nHorizontal asymptote?",answers:["y=0"]},
  38:{question:"f(x) = (2x - 1)/(x + 3)\nX-intercept?",answers:["1/2,0","0.5,0"]},
  39:{question:"f(x) = 4/(x^2 - 9)\nY-intercept?",answers:["0,-4/9"]},
  40:{question:"f(x) = (x^2 - 4)/(x - 2)\nHole (x-coordinate)?",answers:["2","x=2"]},

  41:{question:"f(x) = (x + 3)/(x - 5)\nVertical asymptote?",answers:["5","x=5"]},
  42:{question:"f(x) = (5x)/(x^2 + 1)\nHorizontal asymptote?",answers:["y=0"]},
  43:{question:"f(x) = (x^2 + 2x)/(x^2 - 4)\nVertical asymptotes?",answers:["x=2,x=-2","x=±2","2,-2"]},
  44:{question:"f(x) = (2x^2)/(x^2 - 16)\nHorizontal asymptote?",answers:["y=2"]},
  45:{question:"f(x) = (x - 6)/(x + 6)\nX-intercept?",answers:["6,0","x=6"]},
  46:{question:"f(x) = (x^2 - x - 6)/(x^2 + 3x + 2)\nHole (x-coordinate)?",answers:["-2","x=-2"]},
  47:{question:"f(x) = 3/(x-5) + 2\nHorizontal asymptote?",answers:["y=2"]},
  48:{question:"f(x) = (x^2 - 1)/(x^3 + 1)\nY-intercept?",answers:["0,-1"]},
  49:{question:"f(x) = (x + 7)/(x^2 - 49)\nVertical asymptote (not hole)?",answers:["7","x=7"]},
  50:{question:"f(x) = (x^2)/(x^2 + 4)\nRange?",answers:["[0,1)","0<=y<1","0≤y<1"]}
};

let currentCard = null;
let waitingForAnswer = false;
let terminal, termInput, okBtn;

function appendLine(text) {
  const div = document.createElement("div");
  div.textContent = text;
  terminal.appendChild(div);
  terminal.scrollTop = terminal.scrollHeight;
}

function printWelcome() {
  appendLine("Welcome to The Mathrix");
  appendLine("Step 1: Type a card number (1–50).");
  appendLine("Step 2: Type your answer.");
}

function handleCardNumber(input) {
  const num = parseInt(input, 10);
  if (!correctAnswers[num]) {
    appendLine(`❌ Card ${input} not found.`);
    resetState();
    return;
  }
  currentCard = num;
  waitingForAnswer = true;
  appendLine(`📜 Card ${num}: ${correctAnswers[num].question}`);
  appendLine("Now type your answer (or another card number):");
  termInput.placeholder = "Type your answer or another card";
}

function handleAnswer(input) {
  // If input is a number and valid card, treat as new card number
  if (/^\d+$/.test(input) && correctAnswers[parseInt(input, 10)]) {
    handleCardNumber(input);
    return;
  }
  if (currentCard === null) {
    appendLine("❌ Please select a card first.");
    resetState();
    return;
  }
  
  const ans = normalize(input);
  const valid = correctAnswers[currentCard].answers.map(a => normalize(a));
  if (valid.includes(ans)) {
    appendLine("✅ Correct!");
    resetState();
    appendLine("Type another card number to continue.");
  } else {
    appendLine("❌ Wrong. Try again, or type a different card number.");
    termInput.placeholder = "Retry or new card";
  }
}

function sendCommand() {
  const val = termInput.value.trim();
  if (!val) return;
  appendLine("> " + val);
  if (!waitingForAnswer)
    handleCardNumber(val);
  else
    handleAnswer(val);
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

/* --- Sentence Game --- */
const answers = [
  ["numbers","patterns","equations","functions","logic","puzzles","solutions","formulas","sequences","graphs"],
  ["powerful","exciting","unbeatable","awesome","infinite","legendary","amazing","unstoppable","fun","supreme"],
  ["playing","solving","climbing","leveling","thinking","continuing","trying","calculating","exploring","studying"],
  ["conquer","unlock","master","win","dominate","reach","clear","achieve","complete","overcome"],
  ["challenge","game","function","quest","equation","puzzle","stage","level","problem","maze"]
];

function checkAnswers() {
  let score = 0;
  for (let i = 0; i < answers.length; i++) {
    let input = document.getElementById("blank" + i).value.trim().toLowerCase();
    const resultSpan = document.getElementById("result" + i);
    input = input.replace(/\s+/g, " ");
    if (answers[i].includes(input)) {
      resultSpan.textContent = "✅ Correct!";
      resultSpan.style.color = "#00ff99";
      score++;
    } else {
      resultSpan.textContent = "❌ Wrong!";
      resultSpan.style.color = "#ff3366";
    }
  }
  document.getElementById("score").textContent = `You got ${score}/5 correct.`;
}
</script>
</body>
</html>
