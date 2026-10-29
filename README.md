Pagina-Questoes-C-PROI/
├── index.html
└── README.md
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Simulador C-Pro I</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f7fa;
      margin: 0;
      padding: 0;
    }
    header {
      background: #004aad;
      color: white;
      padding: 15px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    h1 { margin: 0; }
    #progress-bar {
      width: 50%;
      background: #ccc;
      height: 10px;
      border-radius: 5px;
      overflow: hidden;
    }
    #progress {
      height: 10px;
      background: #00c853;
      width: 0%;
    }
    #quiz-container { padding: 20px; }
    .card {
      background: white;
      border-radius: 8px;
      padding: 15px;
      margin-bottom: 20px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    button.option {
      display: block;
      width: 100%;
      margin: 5px 0;
      padding: 10px;
      border: none;
      border-radius: 5px;
      background: #e0e0e0;
      cursor: pointer;
    }
    button.option.correct { background: #00c853; color: white; }
    button.option.incorrect { background: #d32f2f; color: white; }
    #pause-btn {
      background: #ff9800;
      border: none;
      padding: 10px;
      border-radius: 5px;
      cursor: pointer;
    }
    #finish-btn {
      background: #004aad;
      color: white;
      border: none;
      padding: 15px;
      border-radius: 5px;
      cursor: pointer;
      margin: 20px;
    }
    #result {
      font-size: 1.2em;
      margin: 20px;
    }
  </style>
</head>
<body>
  <header>
    <h1>Simulador C-Pro I</h1>
    <div id="progress-bar"><div id="progress"></div></div>
    <button id="pause-btn">⏸️ Pausar</button>
  </header>

  <main id="quiz-container"></main>

  <footer>
    <button id="finish-btn">Finalizar Teste</button>
    <div id="result"></div>
  </footer>

  <script>
    let questions = [
      {
        "question": "Qual documento formaliza uma dívida bancária?",
        "options": ["CPR", "CCB", "Nota Promissória Rural", "Termo de Crédito Agropecuário"],
        "correct": 1,
        "explanation": "A Cédula de Crédito Bancário (CCB) formaliza a dívida e pode ser registrada."
      },
      {
        "question": "Qual é a função principal da CPR?",
        "options": ["Formalizar dívida", "Garantir entrega futura de produtos", "Servir como garantia complementar", "Documento não obrigatório"],
        "correct": 1,
        "explanation": "A CPR representa compromisso de entrega futura de produtos agropecuários."
      },
      {
        "question": "Qual a taxa mínima para aprovação na C-Pro I?",
        "options": ["50%", "60%", "70%", "80%"],
        "correct": 2,
        "explanation": "É necessário atingir pelo menos 70% de acertos para aprovação."
      }
      // 👉 Aqui você pode adicionar até 200 questões seguindo o mesmo formato
    ];

    let currentQuestions = [];
    let score = 0;
    let paused = false;

    function startQuiz() {
      score = 0;
      currentQuestions = shuffle(questions).slice(0, 40);
      renderQuestions();
    }

    function shuffle(array) {
      return array.sort(() => Math.random() - 0.5);
    }

    function renderQuestions() {
      const container = document.getElementById("quiz-container");
      container.innerHTML = "";
      currentQuestions.forEach((q, index) => {
        const card = document.createElement("div");
        card.className = "card";
        card.innerHTML = `<h3>${index+1}. ${q.question}</h3>`;
        q.options.forEach((opt, i) => {
          const btn = document.createElement("button");
          btn.className = "option";
          btn.textContent = opt;
          btn.onclick = () => checkAnswer(btn, q, i);
          card.appendChild(btn);
        });
        container.appendChild(card);
      });
    }

    function checkAnswer(btn, q, i) {
      if (i === q.correct) {
        btn.classList.add("correct");
        score++;
      } else {
        btn.classList.add("incorrect");
      }
      const explanation = document.createElement("p");
      explanation.textContent = "Explicação: " + q.explanation;
      btn.parentElement.appendChild(explanation);
      updateProgress();
    }

    function updateProgress() {
      const progress = document.getElementById("progress");
      progress.style.width = `${(score/currentQuestions.length)*100}%`;
    }

    document.getElementById("finish-btn").onclick = () => {
      const result = document.getElementById("result");
      const percent = (score/currentQuestions.length)*100;
      result.textContent = `Você acertou ${score} de ${currentQuestions.length} (${percent.toFixed(1)}%).`;
      if (percent >= 70) {
        result.textContent += " ✅ Aprovada!";
      } else {
        result.textContent += " ❌ Reprovada.";
      }
    };

    document.getElementById("pause-btn").onclick = () => {
      paused = !paused;
      alert(paused ? "Teste pausado. Você pode retomar depois." : "Teste retomado.");
    };

    startQuiz();
  </script>
</body>
</html>
