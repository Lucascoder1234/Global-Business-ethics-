<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Global Business Etiquette Quest</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      background-color: #f4f4f9;
    }
    #game-container {
      text-align: center;
      width: 80%;
      max-width: 600px;
      background: white;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
      display: none;
    }
    #title-screen {
      text-align: center;
      width: 80%;
      max-width: 600px;
      background: white;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }
    .button {
      padding: 10px 20px;
      font-size: 1em;
      border: none;
      background-color: #007bff;
      color: white;
      border-radius: 5px;
      cursor: pointer;
      margin: 10px 0;
    }
    .button:hover {
      background-color: #0056b3;
    }
    .question {
      font-size: 1.2em;
      margin-bottom: 20px;
    }
    .choices {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }
    #score, #level, #timer {
      margin: 10px 0;
    }
    .hidden {
      display: none;
    }
  </style>
</head>
<body>
  <div id="title-screen">
    <h1>Global Business Etiquette Quest</h1>
    <button class="button" id="start-game">Start Game</button>
    <button class="button" id="quit-game">Quit Game</button>
  </div>

  <div id="game-container">
    <h1>Global Business Etiquette Quest</h1>
    <div id="level"></div>
    <div id="score"></div>
    <div id="timer"></div>
    <div id="question-container">
      <p class="question"></p>
      <div class="choices"></div>
    </div>
    <div id="result" style="margin-top: 20px; font-weight: bold;"></div>
  </div>

  <script>
    const gameData = [
      {
        level: 1,
        title: "Business Etiquette in Japan",
        questions: [
          {
            question: "In Japan, how should you present your business card?",
            choices: [
              { text: "With one hand", correct: false },
              { text: "With both hands", correct: true },
              { text: "Place it on the table", correct: false },
            ],
          },
          {
            question: "What should you avoid doing during a business meeting in Japan?",
            choices: [
              { text: "Make eye contact", correct: false },
              { text: "Cross your legs", correct: true },
              { text: "Exchange greetings", correct: false },
            ],
          },
        ],
      },
      {
        level: 2,
        title: "Business Etiquette in Germany",
        questions: [
          {
            question: "What is important when scheduling a business meeting in Germany?",
            choices: [
              { text: "Being punctual", correct: true },
              { text: "Bringing gifts", correct: false },
              { text: "Speaking casually", correct: false },
            ],
          },
          {
            question: "How should you address colleagues in Germany?",
            choices: [
              { text: "Use first names immediately", correct: false },
              { text: "Use formal titles", correct: true },
              { text: "Avoid greetings", correct: false },
            ],
          },
        ],
      },
    ];

    let currentLevel = 0;
    let currentQuestion = 0;
    let score = 0;
    let lives = 3;
    let timerInterval;

    const titleScreen = document.getElementById("title-screen");
    const gameContainer = document.getElementById("game-container");
    const startGameButton = document.getElementById("start-game");
    const quitGameButton = document.getElementById("quit-game");
    const levelEl = document.getElementById("level");
    const scoreEl = document.getElementById("score");
    const timerEl = document.getElementById("timer");
    const questionEl = document.querySelector(".question");
    const choicesEl = document.querySelector(".choices");
    const resultEl = document.getElementById("result");

    function showTitleScreen() {
      titleScreen.style.display = "block";
      gameContainer.style.display = "none";
    }

    function startGame() {
      titleScreen.style.display = "none";
      gameContainer.style.display = "block";
      loadLevel();
    }

    function quitGame() {
      alert("Thank you for playing!");
      showTitleScreen();
    }

    function loadLevel() {
      const currentData = gameData[currentLevel];
      levelEl.textContent = `Level: ${currentData.title}`;
      currentQuestion = 0;
      loadQuestion();
    }

    function loadQuestion() {
      const currentData = gameData[currentLevel];
      const questionData = currentData.questions[currentQuestion];

      scoreEl.textContent = `Score: ${score} | Lives: ${lives}`;
      questionEl.textContent = questionData.question;

      choicesEl.innerHTML = "";
      questionData.choices.forEach((choice) => {
        const button = document.createElement("button");
        button.textContent = choice.text;
        button.classList.add("button");
        button.onclick = () => handleAnswer(choice.correct);
        choicesEl.appendChild(button);
      });
    }

    function handleAnswer(correct) {
      if (correct) {
        score += 10;
        resultEl.textContent = "Correct!";
      } else {
        lives--;
        resultEl.textContent = "Incorrect!";
        if (lives <= 0) {
          resultEl.textContent = "Game Over!";
          return;
        }
      }

      currentQuestion++;
      if (currentQuestion < gameData[currentLevel].questions.length) {
        setTimeout(loadQuestion, 1000);
      } else {
        currentLevel++;
        if (currentLevel < gameData.length) {
          resultEl.textContent = "Level Complete!";
          setTimeout(loadLevel, 1000);
        } else {
          resultEl.textContent = "Congratulations! You've completed all levels!";
        }
      }
    }

    startGameButton.onclick = startGame;
    quitGameButton.onclick = quitGame;

    showTitleScreen();
  </script>
</body>
</html>
