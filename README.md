# trilha-da-moeda
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Jogo: Trilha da Demanda por Moeda</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(to right, #4facfe, #00f2fe);
      color: #fff;
      text-align: center;
      margin: 0;
      padding: 0;
    }
    h1 {
      margin-top: 20px;
      text-shadow: 2px 2px #000;
    }
    #game-board {
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
      max-width: 500px;
      margin: 20px auto;
      gap: 10px;
    }
    .tile {
      width: 50px;
      height: 50px;
      background-color: #ffd700;
      border: 2px solid #000;
      border-radius: 10px;
      font-weight: bold;
      font-size: 16px;
      color: #000;
      display: flex;
      align-items: center;
      justify-content: center;
      box-shadow: 2px 2px 4px #000;
      transition: transform 0.3s ease;
    }
    .tile.player {
      background-color: #00ff99;
      transform: scale(1.2);
    }
    #question-box {
      background-color: rgba(0, 0, 0, 0.7);
      padding: 20px;
      border-radius: 15px;
      max-width: 700px;
      margin: 20px auto;
    }
    button {
      margin: 10px;
      padding: 12px 25px;
      font-size: 16px;
      border: none;
      border-radius: 10px;
      background-color: #ffcc00;
      color: #000;
      cursor: pointer;
      font-weight: bold;
      transition: background-color 0.3s, transform 0.2s;
    }
    button:hover {
      background-color: #ffdd33;
      transform: scale(1.05);
    }
    #excellent, #wrong {
      display: none;
      font-size: 28px;
      font-weight: bold;
      margin-top: 15px;
      text-shadow: 2px 2px #000;
    }
    #excellent {
      color: #00ff00;
    }
    #wrong {
      color: #ff4444;
    }
  </style>
</head>
<body>
  <h1>Trilha da Demanda por Moeda</h1>
  <div id="game-board"></div>
  <div id="question-box">
    <p id="question"></p>
    <div id="options"></div>
    <div id="excellent">✨ Excelente! ✨</div>
    <div id="wrong">❌ Errou, tente novamente!</div>
  </div>

  <script>
    const questions = [
      { question: "Qual teoria associa a demanda por moeda com transação, precaução e especulação?", options: ["Clássica", "Monetarista", "Keynesiana", "Friedmaniana"], answer: 2 },
      { question: "Quem desenvolveu a teoria da preferência pela liquidez?", options: ["Friedman", "Keynes", "Baumol", "Marshall"], answer: 1 },
      { question: "A teoria de Baumol-Tobin mostra que a demanda por moeda depende de:", options: ["Taxa de inflação", "Juros e custo de sacar", "Taxa de câmbio", "Política fiscal"], answer: 1 },
      { question: "Na teoria quantitativa da moeda, MV = PY, o que representa V?", options: ["Volume de crédito", "Velocidade da moeda", "Valor real", "Variação da renda"], answer: 1 },
      { question: "Para os monetaristas, a demanda por moeda é função estável da:", options: ["Taxa de juros", "Inflação", "Renda", "Taxa de crescimento"], answer: 2 },
      { question: "Qual destes NÃO é um motivo para demanda por moeda segundo Keynes?", options: ["Transação", "Precaução", "Especulação", "Inflação"], answer: 3 },
      { question: "Na visão clássica, a moeda serve apenas como:", options: ["Reserva de valor", "Meio de troca", "Hedge", "Instrumento fiscal"], answer: 1 },
      { question: "A demanda por moeda como ativo está presente em qual teoria?", options: ["Clássica", "Keynesiana", "Quantitativa", "Fisheriana"], answer: 1 },
      { question: "Friedman via a moeda como parte de um:", options: ["Orçamento fiscal", "Portfólio de ativos", "Bem de consumo", "Sistema bancário"], answer: 1 },
      { question: "A teoria de Cambridge enfatiza a:", options: ["Oferta de moeda", "Demanda especulativa", "Demanda por moeda proporcional à renda", "Taxa de câmbio"], answer: 2 },
      { question: "Qual componente da demanda por moeda é mais sensível à taxa de juros?", options: ["Transação", "Precaução", "Especulação", "Moeda ativa"], answer: 2 },
      { question: "Para Baumol-Tobin, a frequência de saques depende da:", options: ["Renda e custo fixo", "Inflação", "Taxa de câmbio", "Taxa de juros real"], answer: 0 },
      { question: "A equação de troca de Fisher é:", options: ["P=MV", "MV=PY", "P=Y/M", "Y=PMV"], answer: 1 },
      { question: "O que a teoria monetarista critica na visão keynesiana?", options: ["Ênfase na política fiscal", "Instabilidade da demanda por moeda", "Reserva de valor", "Neutralidade da moeda"], answer: 1 },
      { question: "A função da moeda mais associada à demanda por especulação é:", options: ["Unidade de conta", "Meio de troca", "Reserva de valor", "Lastro"], answer: 2 }
    ];

    let playerPosition = 0;
    let currentQuestion = 0;

    const board = document.getElementById("game-board");
    const questionText = document.getElementById("question");
    const optionsDiv = document.getElementById("options");
    const excellentMsg = document.getElementById("excellent");
    const wrongMsg = document.getElementById("wrong");

    function renderBoard() {
      board.innerHTML = "";
      for (let i = 0; i < questions.length; i++) {
        const tile = document.createElement("div");
        tile.className = "tile";
        tile.textContent = i + 1;
        if (i === playerPosition) tile.classList.add("player");
        board.appendChild(tile);
      }
    }

    function showQuestion() {
      if (currentQuestion >= questions.length) {
        questionText.textContent = "🏁 Parabéns! Você completou a trilha da moeda!";
        optionsDiv.innerHTML = "";
        return;
      }
      const q = questions[currentQuestion];
      questionText.textContent = q.question;
      optionsDiv.innerHTML = "";
      q.options.forEach((opt, i) => {
        const btn = document.createElement("button");
        btn.textContent = opt;
        btn.onclick = () => {
          if (i === q.answer) {
            playerPosition++;
            excellentMsg.style.display = "block";
            setTimeout(() => { excellentMsg.style.display = "none"; }, 1500);
            currentQuestion++;
            renderBoard();
            showQuestion();
          } else {
            wrongMsg.style.display = "block";
            setTimeout(() => { wrongMsg.style.display = "none"; }, 1500);
          }
        };
        optionsDiv.appendChild(btn);
      });
    }

    renderBoard();
    showQuestion();
  </script>
</body>
</html>
