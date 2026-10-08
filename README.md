# Mathforfun
Pembelajaran berbasis game
math-ceria/
│
├── index.html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>🧮 Matematika Ceria Kelas 1 SD</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <div class="game-container">
    <h1>🧮 Matematika Ceria 🧮</h1>
    <p id="question">Berapa hasilnya?</p>

    <div id="choices" class="choices"></div>

    <p id="feedback" class="feedback"></p>

    <p class="score">Skor: <span id="score">0</span> / 10</p>
    <button id="nextBtn" class="hidden">Soal Selanjutnya ➜</button>
    <button id="restartBtn" class="hidden">Main Lagi 🔁</button>
  </div>

  <audio id="sound-correct" src="sounds/correct.mp3" preload="auto"></audio>
  <audio id="sound-wrong" src="sounds/wrong.mp3" preload="auto"></audio>

  <script src="script.js"></script>
</body>
</html>

├── styles.css
body {
  font-family: "Comic Sans MS", sans-serif;
  background: linear-gradient(135deg, #f9d6ff, #b3e5fc);
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
}

.game-container {
  background: white;
  padding: 20px 30px;
  border-radius: 20px;
  text-align: center;
  box-shadow: 0 5px 20px rgba(0,0,0,0.1);
  width: 320px;
}

h1 {
  color: #ff6f00;
  margin-bottom: 10px;
}

#question {
  font-size: 1.6rem;
  color: #333;
  margin-bottom: 15px;
}

.choices {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-bottom: 15px;
}

.choice-btn {
  background: #ffca28;
  border: none;
  padding: 10px;
  font-size: 1.2rem;
  border-radius: 10px;
  cursor: pointer;
  transition: 0.2s;
}

.choice-btn:hover {
  background: #ffd54f;
}

.feedback {
  font-size: 1.3rem;
  height: 25px;
  margin-bottom: 10px;
}

.score {
  font-weight: bold;
  color: #444;
}

button.hidden {
  display: none;
}

#nextBtn, #restartBtn {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 10px;
  border-radius: 10px;
  cursor: pointer;
}

#restartBtn {
  background-color: #2196f3;
}

├── script.js
const questionEl = document.getElementById("question");
const choicesEl = document.getElementById("choices");
const feedbackEl = document.getElementById("feedback");
const scoreEl = document.getElementById("score");
const nextBtn = document.getElementById("nextBtn");
const restartBtn = document.getElementById("restartBtn");
const soundCorrect = document.getElementById("sound-correct");
const soundWrong = document.getElementById("sound-wrong");

let score = 0;
let currentQuestion = 0;
const totalQuestions = 10;

function randomInt(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

function generateQuestion() {
  const a = randomInt(1, 10);
  const b = randomInt(1, 10);
  const isAddition = Math.random() > 0.5;
  const questionText = isAddition ? `${a} + ${b}` : `${a} - ${b}`;
  const answer = isAddition ? a + b : a - b;

  const choices = [answer];
  while (choices.length < 3) {
    let wrong = randomInt(answer - 3, answer + 3);
    if (wrong !== answer && wrong >= 0 && !choices.includes(wrong)) {
      choices.push(wrong);
    }
  }

  return { questionText, answer, choices: shuffle(choices) };
}

function shuffle(arr) {
  return arr.sort(() => Math.random() - 0.5);
}

let currentData = generateQuestion();

function showQuestion() {
  feedbackEl.textContent = "";
  nextBtn.classList.add("hidden");
  questionEl.textContent = `Soal ${currentQuestion + 1}: ${currentData.questionText} = ?`;
  choicesEl.innerHTML = "";
  currentData.choices.forEach((choice) => {
    const btn = document.createElement("button");
    btn.textContent = choice;
    btn.classList.add("choice-btn");
    btn.onclick = () => checkAnswer(choice);
    choicesEl.appendChild(btn);
  });
}

function checkAnswer(choice) {
  if (choice === currentData.answer) {
    feedbackEl.textContent = "Hebat! 😍";
    feedbackEl.style.color = "green";
    soundCorrect.currentTime = 0;
    soundCorrect.play();
    score++;
  } else {
    feedbackEl.textContent = "Coba lagi ya! 😅";
    feedbackEl.style.color = "red";
    soundWrong.currentTime = 0;
    soundWrong.play();
  }
  scoreEl.textContent = score;
  nextBtn.classList.remove("hidden");
  document.querySelectorAll(".choice-btn").forEach((btn) => (btn.disabled = true));
}

nextBtn.addEventListener("click", () => {
  currentQuestion++;
  if (currentQuestion < totalQuestions) {
    currentData = generateQuestion();
    showQuestion();
  } else {
    showResult();
  }
});

function showResult() {
  questionEl.textContent = `🎉 Selesai! Skor kamu: ${score} / ${totalQuestions}`;
  choicesEl.innerHTML = "";
  feedbackEl.textContent = "";
  nextBtn.classList.add("hidden");
  restartBtn.classList.remove("hidden");
}

restartBtn.addEventListener("click", () => {
  score = 0;
  currentQuestion = 0;
  scoreEl.textContent = 0;
  restartBtn.classList.add("hidden");
  currentData = generateQuestion();
  showQuestion();
});

showQuestion();

├── sounds/
│   ├── correct.mp3
suara pendek “ting!” (benar)
│   └── wrong.mp3
suara “buzz” (salah)
└── README.md
# 🧮 Matematika Ceria Kelas 1 SD
Game sederhana untuk belajar penjumlahan dan pengurangan angka 1–10 dengan warna ceria dan suara motivasi.

## 🎯 Fitur
- Soal acak (penjumlahan/pengurangan)
- Suara saat jawaban benar/salah
- Skor otomatis
- Tampilan ramah anak SD

## 🚀 Cara Menjalankan
1. Buka `index.html` di browser.
2. Jawab soal yang muncul.
3. Dengar suara motivasi setiap kali menjawab.

## 💡 Deploy ke GitHub Pages
1. Buat repo baru di GitHub (mis. `math-ceria`).
2. Upload semua file (termasuk folder `sounds/`).
3. Masuk ke Settings → Pages → pilih Branch `main` dan Folder `/ (root)`.
4. Tunggu sebentar, lalu buka URL:
   `https://username.github.io/math-ceria/`

