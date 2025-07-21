<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Desafio</title>
  <style>
    body {
      font-family: 'Georgia', serif;
      background: #f0f0f0;
      margin: 0;
      padding: 20px;
      color: #5a3e2b;
    }

    .container {
      max-width: 700px;
      margin: 0 auto;
      background: white;
      padding: 30px;
      border-radius: 12px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      background-image: URL(./desafio.png);
      background-image: cover;
    }

    h1 {
      text-align: center;
      font-size: 26px;
      letter-spacing: 1px;
    }

    .titulo {
      color: #8c3b3b;
      font-size: 24px;
      text-align: center;
      margin-bottom: 5px;
    }

    .datas {
      background-color: #7fdc9f;
      display: inline-block;
      padding: 10px 10px;
      border-radius: 8px;
      font-weight: bold;
      margin-bottom: 12px;
      margin-left: 230px;
      margin-top: 10px;
      font-size: 20px;
    }

    .metas {
      margin-top: 20px;
      line-height: 1.7;
      font-size: 16px;
    }

    .frase {
      text-align: center;
      margin: 30px 0 10px;
      font-style: italic;
      color: #333;
    }

    .dias {
      display: grid;
      grid-template-columns: repeat(7, 1fr);
      gap: 10px;
      justify-items: center;
      margin-bottom: 20px;
    }

    .dia {
      width: 60px;
      height: 60px;
      border: 2px solid #7f7f7f;
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      font-weight: bold;
      font-size: 22px;
      color: #5a3e2b;
      cursor: pointer;
      transition: background 0.2s;
      user-select: none;
    }

    .dia:hover {
      background-color: #e4e4e4;
    }

    .legenda {
      font-size: 14px;
      text-align: center;
      margin-bottom: 20px;
    }

    .nota {
      text-align: center;
      font-size: 14px;
      color: #444;
    }

    .coracao {
      font-size: 20px;
      color: red;
    }

    #verResumo {
      margin: 10px auto;
      display: block;
      padding: 8px 16px;
      background: #8c3b3b;
      color: white;
      border: none;
      border-radius: 6px;
      font-size: 16px;
      cursor: pointer;
    }

    #resumo {
      text-align: center;
      font-size: 16px;
      margin-top: 10px;
    }

    /* Responsividade */
    @media (max-width: 768px) {
      .container {
        padding: 20px;
      }

      h1 {
        font-size: 22px;
      }

      .titulo {
        font-size: 20px;
      }

      .metas {
        font-size: 15px;
      }

      .dias {
        grid-template-columns: repeat(6, 1fr);
        gap: 8px;
      }

      .dia {
        width: 36px;
        height: 36px;
        font-size: 15px;
      }
    }

    @media (max-width: 480px) {
      .dias {
        grid-template-columns: repeat(5, 1fr);
        gap: 6px;
      }

      .dia {
        width: 32px;
        height: 32px;
        font-size: 14px;
      }

      .legenda,
      .nota {
        font-size: 13px;
      }

      #verResumo {
        font-size: 14px;
        padding: 6px 12px;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>DESAFIO 13 DIAS</h1>
    <div class="titulo">Desafio</div>
    <div class="datas">Dia 21-07 ao dia 02-08</div>
    <div class="titulo">Metas de Comprometimento</div>

    <div class="metas">
      <ul>
        <li>Beber no mínimo 3L de água/dia</li>
        <li>Inserir uma fruta no café da manhã e uma fruta no lanche da tarde</li>
        <li>Fazer alguma atividade física da sua escolha no mínimo 3x/semana</li>
        <li>Zero refeições livres: sem doces, sem besteiras, sem salgados, sem álcool, sem beliscos — tudo aquilo que você sabe que te atrasa.</li>
      </ul>
    </div>

    <div class="frase">Não é sobre perfeição, é sobre constância.</div>

    <div class="dias" id="dias">
      <!-- Gerado dinamicamente pelo JS -->
    </div>

    <div class="legenda">
      MARQUE UM “X” NOS DIAS CUMPRIDOS E UM “F” NOS DIAS EM QUE NÃO DEU
    </div>

    <div class="nota">
      Não tem problema falhar, seja sincera!<br> Esse controle é importante pra você analisar onde precisa melhorar para os próximos. <br>
      Lembre-se: CONSTÂNCIA! <br><span class="coracao">❤️❤️❤️</span>
    </div>

    <button id="verResumo">Ver Resumo</button>
    <div id="resumo"></div>
  </div>

  <script>
  const diasContainer = document.getElementById("dias");

  function salvarEstado(dia, estado) {
    localStorage.setItem(`desafio_dia_${dia}`, estado);
  }

  function obterEstado(dia) {
    return localStorage.getItem(`desafio_dia_${dia}`) || "vazio";
  }

  function atualizarVisual(diaEl, estado, numero) {
    if (estado === "feito") {
      diaEl.textContent = "X";
    } else if (estado === "falhou") {
      diaEl.textContent = "F";
    } else {
      diaEl.textContent = numero;
    }
  }

  const diasDesafio = [...Array.from({length: 11}, (_, i) => 21 + i), 1, 2]; // [21..31, 1, 2]

  // Criar os círculos dos dias
  diasDesafio.forEach(i => {
    const dia = document.createElement("div");
    dia.classList.add("dia");

    let estadoInicial = obterEstado(i);
    dia.dataset.estado = estadoInicial;
    atualizarVisual(dia, estadoInicial, i);

    dia.addEventListener("click", () => {
      let estado = dia.dataset.estado;

      if (estado === "vazio") {
        estado = "feito";
      } else if (estado === "feito") {
        estado = "falhou";
      } else {
        estado = "vazio";
      }

      dia.dataset.estado = estado;
      atualizarVisual(dia, estado, i);
      salvarEstado(i, estado);
    });

    diasContainer.appendChild(dia);
  });

  // Botão de Resumo
  document.getElementById("verResumo").addEventListener("click", () => {
    let feitos = 0, falhados = 0, vazios = 0;

    diasDesafio.forEach(i => {
      const estado = obterEstado(i);
      if (estado === "feito") feitos++;
      else if (estado === "falhou") falhados++;
      else vazios++;
    });

    const resumoTexto = `
      ✅ Dias cumpridos: <strong>${feitos}</strong><br>
      ❌ Dias não cumpridos: <strong>${falhados}</strong><br>
      ⭕ Dias em branco: <strong>${vazios}</strong>
    `;
    document.getElementById("resumo").innerHTML = resumoTexto;
  });
</script>
</body>
</html>
