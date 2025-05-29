<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <title>Quiz de Design Gráfico</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #d9c7f9;
      color: #000;
      padding: 20px;
    }
    h1 {
      color: #000;
      text-align: center;
      margin-bottom: 30px;
      font-weight: bold;
    }
    .question {
      background: #fff;
      padding: 15px;
      margin-bottom: 15px;
      border-radius: 10px;
      border: 2px solid transparent;
      transition: border-color 0.3s ease;
    }
    .question.correct {
      border-color: #6a0dad;
      background-color: #f8e9ff;
    }
    .question.incorrect {
      border-color: #ff66cc;
      background-color: #ffe6f0;
    }
    label {
      display: block;
      margin: 8px 0;
      cursor: pointer;
      user-select: none;
      font-size: 16px;
      color: #000;
    }
    input[type="radio"] {
      margin-right: 8px;
      accent-color: #6a0dad;
      cursor: pointer;
    }
    button {
      margin-top: 20px;
      padding: 12px 25px;
      background: #6a0dad;
      border: none;
      color: #fff;
      font-weight: bold;
      cursor: pointer;
      border-radius: 8px;
      font-size: 16px;
      transition: background-color 0.3s ease;
      margin-right: 10px;
      box-shadow: 0 0 8px #ff66cc;
    }
    button:hover {
      background: #ff66cc;
      color: #000;
      box-shadow: 0 0 12px #6a0dad;
    }
    .explanation {
      margin-top: 10px;
      font-style: italic;
      color: #444;
      border-top: 1px solid #ddd;
      padding-top: 8px;
    }
    #resultadoFinal {
      margin-top: 30px;
      padding: 20px;
      background: #fff;
      border-radius: 15px;
      text-align: center;
      font-size: 20px;
      font-weight: bold;
      color: #6a0dad;
      border: 2px solid #ff66cc;
    }
    #resultadoFinal strong {
      color: #000;
    }
    #emailForm {
      text-align: center;
      margin-top: 20px;
      display: none;
    }
    #emailForm input[type="email"] {
      padding: 10px;
      border-radius: 5px;
      width: 250px;
      border: 1px solid #ccc;
      font-size: 16px;
    }
  </style>

  <!-- EmailJS SDK -->
  <script src="https://cdn.emailjs.com/dist/email.min.js"></script>
  <script>
    emailjs.init("5RbthZ_AxdVytpCxe"); // Public Key
  </script>
</head>
<body>

  <h1>Quiz de Design Gráfico</h1>
  <form id="quizForm"></form>

  <div style="text-align:center;">
    <button type="button" onclick="verificarRespostas()">Enviar</button>
    <button type="button" onclick="refazerQuiz()">Refazer</button>
  </div>

  <div id="resultadoFinal"></div>

  <form id="emailForm" onsubmit="event.preventDefault(); enviarEmail();">
    <input type="email" id="emailInput" placeholder="Seu e-mail" required />
    <button type="submit">Enviar resultado por e-mail</button>
  </form>

  <script>
    const perguntas = [
      {
        pergunta: "1. O que é design gráfico?",
        opcoes: ["Técnica de pintura", "Arte de organizar elementos visuais para comunicar uma mensagem", "Programação de software", "Engenharia de produção"],
        correta: 1,
        explicacao: "Design gráfico é a arte de combinar elementos visuais para transmitir uma mensagem."
      },
      {
        pergunta: "2. Qual desses é um software popular para design gráfico?",
        opcoes: ["Excel", "Photoshop", "Word", "PowerPoint"],
        correta: 1,
        explicacao: "Photoshop é um dos softwares mais usados para criação e edição de imagens no design gráfico."
      },
      {
        pergunta: "3. O que significa tipografia?",
        opcoes: ["Impressão em 3D", "Estudo dos tipos de papel", "Arte de organizar textos com fontes, tamanhos e espaçamento", "Corte de imagens em programas gráficos"],
        correta: 2,
        explicacao: "Tipografia é o design e a organização de textos para comunicação eficaz."
      },
      {
        pergunta: "4. O que é o princípio do contraste no design gráfico?",
        opcoes: ["Usar cores claras e escuras juntas para destacar elementos", "Evitar qualquer diferença de cor", "Colocar tudo no centro", "Fazer tudo da mesma cor"],
        correta: 0,
        explicacao: "Contraste ajuda a destacar elementos importantes no design."
      },
      {
        pergunta: "5. O que significa branding visual?",
        opcoes: ["Design de embalagens apenas", "Anúncio pago em redes sociais", "Representação visual da identidade de uma marca (cores, tipografia, logotipo etc.)", "Impressão de catálogos"],
        correta: 2,
        explicacao: "Branding visual é como uma marca se apresenta visualmente ao público."
      },
      {
        pergunta: "6. O que é um logotipo?",
        opcoes: ["Qualquer tipo de arte", "Nome da empresa escrito com fonte personalizada", "Uma peça de roupa", "Um slogan"],
        correta: 1,
        explicacao: "Logotipo geralmente é o nome da marca com tipografia única."
      },
      {
        pergunta: "7. Qual é a diferença entre logotipo e logomarca?",
        opcoes: ["Nenhuma, são sinônimos exatos", "Logotipo é textual; logomarca pode incluir símbolos", "Logomarca é usada apenas no Brasil", "Logotipo é informal"],
        correta: 2,
        explicacao: "‘Logomarca’ é um termo popular no Brasil, mas tecnicamente redundante. ‘Logotipo’ é o termo mais correto."
      },
      {
        pergunta: "8. O que é um símbolo em um logotipo?",
        opcoes: ["Frase de efeito", "Imagem ou forma que representa visualmente a marca", "Tipo de papel", "Tipo de borda"],
        correta: 1,
        explicacao: "O símbolo é a parte gráfica que representa uma marca visualmente, sem texto."
      },
      {
        pergunta: "9. Qual desses é um exemplo de logotipo tipográfico?",
        opcoes: ["Nike", "Apple", "Google", "Shell"],
        correta: 2,
        explicacao: "Google usa apenas texto estilizado como logotipo, caracterizando um logotipo tipográfico."
      },
      {
        pergunta: "10. O que é necessário para um bom logotipo?",
        opcoes: ["Ser chamativo, complicado e colorido", "Ter elementos minimalistas, legibilidade e ser memorável", "Usar o máximo de fontes", "Imitar marcas famosas"],
        correta: 1,
        explicacao: "Bons logotipos são simples, fáceis de lembrar e visualmente claros."
      }
    ];

    const form = document.getElementById("quizForm");
    const resultadoFinal = document.getElementById("resultadoFinal");
    const emailForm = document.getElementById("emailForm");
    const emailInput = document.getElementById("emailInput");

    perguntas.forEach((q, i) => {
      const div = document.createElement("div");
      div.classList.add("question");
      div.id = "pergunta" + i;

      const p = document.createElement("p");
      p.textContent = q.pergunta;
      div.appendChild(p);

      q.opcoes.forEach((opcao, j) => {
        const label = document.createElement("label");
        const input = document.createElement("input");
        input.type = "radio";
        input.name = "q" + i;
        input.value = j;
        label.appendChild(input);
        label.appendChild(document.createTextNode(" " + opcao));
        div.appendChild(label);
      });

      form.appendChild(div);
    });

    let resumoGlobal = "";

    function verificarRespostas() {
      for (let i = 0; i < perguntas.length; i++) {
        const radios = document.getElementsByName("q" + i);
        let respondido = false;
        for (let r of radios) {
          if (r.checked) {
            respondido = true;
            break;
          }
        }
        if (!respondido) {
          alert("Por favor, responda todas as perguntas antes de enviar.");
          return;
        }
      }

      let acertos = 0;
      let resumo = "";

      perguntas.forEach((q, i) => {
        const radios = document.getElementsByName("q" + i);
        let selecionado = -1;
        radios.forEach((r, index) => {
          if (r.checked) selecionado = index;
        });

        const div = document.getElementById("pergunta" + i);
        div.classList.remove("correct", "incorrect");

        if (selecionado === q.correta) {
          div.classList.add("correct");
          acertos++;
        } else {
          div.classList.add("incorrect");
        }

        if (!div.querySelector(".explanation")) {
          const exp = document.createElement("div");
          exp.className = "explanation";
          exp.textContent = "Explicação: " + q.explicacao;
          div.appendChild(exp);
        }

        resumo += `${q.pergunta}\nSua resposta: ${q.opcoes[selecionado]}\nCorreta: ${q.opcoes[q.correta]}\n\n`;
      });

      resumoGlobal = resumo;

      let frase = "";
      if (acertos === 10) {
        frase = "🎉 Incrível! Você acertou todas. Você domina o conteúdo!";
      } else if (acertos >= 8) {
        frase = "👏 Muito bem! Você tem um ótimo conhecimento em design gráfico.";
      } else if (acertos >= 6) {
        frase = "🙂 Você está no caminho certo. Continue estudando e praticando!";
      } else {
        frase = "💡 Que tal revisar o conteúdo e tentar novamente?";
      }

      resultadoFinal.innerHTML = `<strong>Você acertou ${acertos} de 10.</strong><br>${frase}`;
      emailForm.style.display = "block";
      emailInput.focus();
    }

    function refazerQuiz() {
      location.reload();
    }

    function enviarEmail() {
      const email = emailInput.value.trim();

      if (!email) {
        alert("Digite um e-mail válido.");
        return;
      }

      emailjs.send("service_lsjwwsd", "template_ar7j7yj", {
        email: email,
        conteudo: `Resultado do Quiz de Design Gráfico:\n\n${resumoGlobal}`
      })
      .then(() => {
        alert("Resultado enviado com sucesso!");
        emailInput.value = "";
        emailForm.style.display = "none";
      }, (error) => {
        console.error("Erro ao enviar:", error);
        alert("Ocorreu um erro ao enviar o e-mail.");
      });
    }
  </script>
</body>
</html>
