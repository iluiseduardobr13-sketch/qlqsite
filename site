<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Agendamento - Barber Shop</title>
  <style>
    :root {
      --azul: #1E3A8A;
      --laranja: #F97316;
      --fundo: #f0f2f5;
      --branco: #ffffff;
      --cinza: #555;
    }

    body {
      font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
      background-color: var(--fundo);
      margin: 0;
      padding: 0;
      color: var(--cinza);
    }

    header {
      background: linear-gradient(90deg, var(--azul), var(--laranja));
      color: white;
      padding: 30px 20px;
      text-align: center;
      border-bottom-left-radius: 25px;
      border-bottom-right-radius: 25px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.2);
    }

    header h1 {
      margin: 0;
      font-size: 2rem;
    }

    header p {
      margin: 5px 0 0;
      font-size: 1.1rem;
    }

    main {
      max-width: 750px;
      margin: 30px auto;
      background: var(--branco);
      padding: 25px;
      border-radius: 20px;
      box-shadow: 0 6px 18px rgba(0,0,0,0.1);
    }

    h2 {
      color: var(--azul);
      margin-top: 0;
    }

    label {
      display: block;
      margin: 15px 0 6px;
      font-weight: bold;
      color: var(--azul);
    }

    select, input {
      width: 100%;
      padding: 12px;
      margin-bottom: 18px;
      border-radius: 10px;
      border: 1px solid #ccc;
      font-size: 1rem;
      outline: none;
      transition: 0.3s;
    }

    select:focus, input:focus {
      border-color: var(--laranja);
      box-shadow: 0 0 5px rgba(249, 115, 22, 0.6);
    }

    button {
      background: var(--laranja);
      color: white;
      border: none;
      padding: 14px;
      width: 100%;
      font-size: 1rem;
      font-weight: bold;
      border-radius: 12px;
      cursor: pointer;
      transition: 0.3s;
    }

    button:hover {
      background: #ea580c;
      transform: scale(1.02);
    }

    .agenda {
      margin-top: 30px;
    }

    .agendamento-item {
      background: var(--azul);
      color: white;
      padding: 16px;
      margin: 12px 0;
      border-radius: 12px;
      box-shadow: 0 3px 8px rgba(0,0,0,0.2);
      transition: 0.3s;
    }

    .agendamento-item:hover {
      transform: translateY(-3px);
    }

    .agendamento-item strong {
      font-size: 1.1rem;
      color: var(--laranja);
    }

    footer {
      text-align: center;
      padding: 20px;
      margin-top: 40px;
      font-size: 0.9rem;
      color: #777;
    }
  </style>
</head>
<body>
  <header>
    <h1>💈 Barber Shop</h1>
    <p>Agende seu horário online com praticidade!</p>
  </header>

  <main>
    <form id="form-agendamento">
      <h2>📝 Novo Agendamento</h2>
      <label for="nome">Seu nome:</label>
      <input type="text" id="nome" placeholder="Digite seu nome" required>

      <label for="servico">Serviço:</label>
      <select id="servico" required>
        <option value="">Selecione um serviço</option>
        <option value="Corte de cabelo - R$30">Corte de cabelo - R$30</option>
        <option value="Barba - R$20">Barba - R$20</option>
        <option value="Corte + Barba - R$45">Corte + Barba - R$45</option>
      </select>

      <label for="data">Data:</label>
      <input type="date" id="data" required>

      <label for="hora">Horário:</label>
      <select id="hora" required></select>

      <button type="submit">✅ Confirmar Agendamento</button>
    </form>

    <div class="agenda">
      <h2>📅 Meus Agendamentos</h2>
      <div id="lista-agendamentos"></div>
    </div>
  </main>

  <footer>
    💈 Barber Shop - Seu estilo em boas mãos.
  </footer>

  <script>
    const form = document.getElementById("form-agendamento");
    const lista = document.getElementById("lista-agendamentos");
    const horaSelect = document.getElementById("hora");
    const dataInput = document.getElementById("data");

    // Gerar horários de 9h até 18h
    function gerarHorarios() {
      horaSelect.innerHTML = '<option value="">Selecione um horário</option>';
      for (let h = 9; h <= 18; h++) {
        const hora = (h < 10 ? "0" + h : h) + ":00";
        const option = document.createElement("option");
        option.value = hora;
        option.textContent = hora;
        horaSelect.appendChild(option);
      }
    }

    function carregarAgendamentos() {
      lista.innerHTML = "";
      const agendamentos = JSON.parse(localStorage.getItem("agendamentos")) || [];
      if (agendamentos.length === 0) {
        lista.innerHTML = "<p>Nenhum agendamento ainda.</p>";
      }
      agendamentos.forEach((item) => {
        const div = document.createElement("div");
        div.classList.add("agendamento-item");
        div.innerHTML = `
          <strong>${item.nome}</strong><br>
          ${item.servico}<br>
          📅 ${item.data} às ⏰ ${item.hora}
        `;
        lista.appendChild(div);
      });
    }

    // Bloquear horários já agendados na data escolhida
    dataInput.addEventListener("change", () => {
      gerarHorarios();
      const agendamentos = JSON.parse(localStorage.getItem("agendamentos")) || [];
      const dataSelecionada = dataInput.value;
      const horariosOcupados = agendamentos
        .filter(a => a.data === dataSelecionada)
        .map(a => a.hora);

      Array.from(horaSelect.options).forEach(opt => {
        if (horariosOcupados.includes(opt.value)) {
          opt.disabled = true;
          opt.textContent = opt.value + " (Indisponível)";
        }
      });
    });

    form.addEventListener("submit", (e) => {
      e.preventDefault();
      const nome = document.getElementById("nome").value;
      const servico = document.getElementById("servico").value;
      const data = document.getElementById("data").value;
      const hora = document.getElementById("hora").value;

      const agendamentos = JSON.parse(localStorage.getItem("agendamentos")) || [];

      // Verificar se horário já está ocupado
      const conflito = agendamentos.some(a => a.data === data && a.hora === hora);
      if (conflito) {
        alert("⚠ Este horário já foi agendado. Escolha outro.");
        return;
      }

      const novoAgendamento = { nome, servico, data, hora };
      agendamentos.push(novoAgendamento);
      localStorage.setItem("agendamentos", JSON.stringify(agendamentos));

      form.reset();
      carregarAgendamentos();
      alert("✅ Agendamento realizado com sucesso!");
    });

    // Inicialização
    gerarHorarios();
    carregarAgendamentos();
  </script>
</body>
</html>
