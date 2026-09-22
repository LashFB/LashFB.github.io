<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Gestão de Gado Bovino</title>
  <style>
    :root {
      --primary: #2e7d32;
      --primary-dark: #1b5e20;
      --bg: #f4f6f8;
      --card-bg: #ffffff;
      --text: #333333;
      --danger: #d32f2f;
      --warning: #ed6c02;
      --accent: #1976d2;
      --purple: #7b1fa2;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
    body { background-color: var(--bg); color: var(--text); padding: 10px; }
    
    /* ALTERAÇÃO PARA PREENCHER 100% DA TELA */
    .container { width: 100%; max-width: 100%; margin: 0; padding: 5px; }

    header { text-align: center; margin-bottom: 20px; }
    header h1 { color: var(--primary-dark); display: flex; align-items: center; justify-content: center; gap: 10px; }

    /* Dashboard Principal */
    .dashboard {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 15px;
      margin-bottom: 15px;
    }
    .metric-card {
      background: var(--card-bg);
      padding: 18px;
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.08);
      border-left: 5px solid var(--primary);
    }
    .metric-card.accent { border-left-color: var(--accent); }
    .metric-card.warning { border-left-color: var(--warning); }
    .metric-card h3 { font-size: 0.85rem; text-transform: uppercase; color: #666; margin-bottom: 5px; }
    .metric-card .value { font-size: 1.6rem; font-weight: bold; color: var(--text); }

    /* Painel de Categorias */
    .categories-summary {
      background: var(--card-bg);
      padding: 15px 20px;
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.08);
      margin-bottom: 25px;
    }
    .categories-summary h3 {
      font-size: 1rem;
      color: var(--primary-dark);
      margin-bottom: 12px;
      text-transform: uppercase;
      letter-spacing: 0.5px;
    }
    .category-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(130px, 1fr));
      gap: 10px;
    }
    .category-box {
      background: #f8f9fa;
      padding: 10px;
      border-radius: 6px;
      border: 1px solid #e0e0e0;
      text-align: center;
    }
    .category-box .cat-title { font-weight: 600; font-size: 0.85rem; color: #555; }
    .category-box .cat-count { font-size: 1.3rem; font-weight: bold; color: var(--text); margin: 3px 0; }
    .category-box .cat-weight { font-size: 0.75rem; color: #777; }

    .card {
      background: var(--card-bg);
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
      margin-bottom: 25px;
    }

    .form-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 15px;
    }

    .form-group { display: flex; flex-direction: column; gap: 5px; }
    .form-group label { font-weight: 600; font-size: 0.9rem; color: #555; }
    .form-group input, .form-group select {
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 5px;
      font-size: 1rem;
    }
    .form-group input:focus, .form-group select:focus {
      outline: none; border-color: var(--primary);
    }

    .btn-container { margin-top: 15px; display: flex; gap: 10px; flex-wrap: wrap; }
    button {
      padding: 8px 14px;
      border: none;
      border-radius: 5px;
      font-size: 0.95rem;
      cursor: pointer;
      font-weight: 600;
      transition: background 0.2s;
    }
    .btn-primary { background: var(--primary); color: white; }
    .btn-primary:hover { background: var(--primary-dark); }
    .btn-excel { background: #2e7d32; color: white; display: flex; align-items: center; gap: 5px; }
    .btn-excel:hover { background: #1b5e20; }
    .btn-cancel { background: #9e9e9e; color: white; }
    .btn-cancel:hover { background: #757575; }
    .btn-danger { background: var(--danger); color: white; }
    .btn-warning { background: var(--warning); color: white; }
    .btn-info { background: var(--accent); color: white; }

    .controls-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 15px;
      flex-wrap: wrap;
      margin-bottom: 15px;
    }
    .search-box { flex: 1; min-width: 250px; }
    .search-box input {
      width: 100%;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 5px;
      font-size: 1rem;
    }

    .table-responsive { overflow-x: auto; }
    table { width: 100%; border-collapse: collapse; text-align: left; }
    th, td { padding: 12px; border-bottom: 1px solid #ddd; }
    th { background-color: #f1f8e9; color: var(--primary-dark); font-weight: bold; }
    tr:hover { background-color: #f9f9f9; }
    tr.fora-propriedade { background-color: #fff8f8; opacity: 0.8; }

    .badge {
      padding: 3px 8px;
      border-radius: 12px;
      font-size: 0.8rem;
      font-weight: bold;
      display: inline-block;
    }
    .badge-vaca { background: #e8f5e9; color: var(--primary-dark); }
    .badge-novilha { background: #e3f2fd; color: #1565c0; }
    .badge-bezerra { background: #fce4ec; color: #c2185b; }
    .badge-bezerro { background: #fff3e0; color: #e65100; }
    .badge-touro { background: #f3e5f5; color: var(--purple); }

    .badge-prop-sim { background: #e8f5e9; color: var(--primary-dark); }
    .badge-prop-nao { background: #ffebee; color: var(--danger); }

    .empty-msg { text-align: center; padding: 20px; color: #777; }

    /* Modal de Histórico */
    .modal-backdrop {
      position: fixed;
      top: 0; left: 0; width: 100%; height: 100%;
      background: rgba(0,0,0,0.5);
      display: none;
      justify-content: center;
      align-items: center;
      z-index: 1000;
    }
    .modal {
      background: white;
      border-radius: 8px;
      width: 90%;
      max-width: 600px;
      padding: 20px;
      max-height: 85vh;
      overflow-y: auto;
      box-shadow: 0 4px 15px rgba(0,0,0,0.2);
    }
    .modal-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-bottom: 1px solid #ddd;
      padding-bottom: 10px;
      margin-bottom: 15px;
    }
    .modal-close {
      background: transparent;
      border: none;
      font-size: 1.5rem;
      cursor: pointer;
      color: #666;
    }
    .actions-cell { display: flex; gap: 5px; flex-wrap: wrap; }
  </style>
</head>
<body>

<div class="container">
  <header>
    <h1>🐂 Gestão de Gado Bovino</h1>
  </header>

  <!-- PAINEL DE MÉTRICAS PRINCIPAIS -->
  <div class="dashboard">
    <div class="metric-card">
      <h3>Total na Propriedade</h3>
      <div class="value" id="stat-total">0</div>
    </div>
    <div class="metric-card accent">
      <h3>Peso Médio Rebanho</h3>
      <div class="value" id="stat-media-peso">0 kg</div>
    </div>
    <div class="metric-card warning">
      <h3>Total de Vacas</h3>
      <div class="value" id="stat-vacas">0</div>
    </div>
    <div class="metric-card">
      <h3>Peso Total Rebanho</h3>
      <div class="value" id="stat-peso-total">0 kg</div>
    </div>
  </div>

  <!-- RESUMO DE TOTAIS POR CATEGORIA -->
  <div class="categories-summary">
    <h3>Totais por Categoria (Apenas Animais na Propriedade)</h3>
    <div class="category-grid" id="category-grid-container">
      <!-- Gerado via JavaScript -->
    </div>
  </div>

  <!-- FORMULÁRIO DE CADASTRO/EDIÇÃO DE ANIMAL -->
  <div class="card">
    <h2 id="form-title" style="margin-bottom: 15px; font-size: 1.2rem;">Cadastrar Novo Animal</h2>
    <form id="animal-form">
      <input type="hidden" id="animal-id">
      
      <div class="form-grid">
        <div class="form-group">
          <label for="brinco">Nº do Brinco *</label>
          <input type="text" id="brinco" placeholder="Ex: 1042" required>
        </div>

        <div class="form-group">
          <label for="proprietario">Proprietário(a) *</label>
          <input type="text" id="proprietario" placeholder="Ex: João da Silva" required>
        </div>

        <div class="form-group">
          <label for="propriedade">Na Propriedade? *</label>
          <select id="propriedade" required>
            <option value="Sim">Sim</option>
            <option value="Não">Não</option>
          </select>
        </div>

        <div class="form-group">
          <label for="sexo">Sexo *</label>
          <select id="sexo" required>
            <option value="Fêmea">Fêmea</option>
            <option value="Macho">Macho</option>
          </select>
        </div>

        <div class="form-group">
          <label for="nascimento">Data de Nascimento *</label>
          <input type="date" id="nascimento" required>
        </div>

        <div class="form-group">
          <label for="brinco-matriz">Nº Brinco Matriz (Mãe)</label>
          <input type="text" id="brinco-matriz" placeholder="Ex: 502 (Opcional)">
        </div>

        <div class="form-group">
          <label for="data-pesagem">Data Inicial da Pesagem (Opcional)</label>
          <input type="date" id="data-pesagem">
        </div>

        <div class="form-group">
          <label for="peso">Peso Inicial em kg (Opcional)</label>
          <input type="number" id="peso" step="0.1" placeholder="Ex: 450.5">
        </div>
      </div>

      <div class="btn-container">
        <button type="submit" class="btn-primary" id="btn-submit">Salvar Animal</button>
        <button type="button" class="btn-cancel" id="btn-cancel" onclick="resetForm()" style="display: none;">Cancelar</button>
      </div>
    </form>
  </div>

  <!-- LISTA DE ANIMAIS -->
  <div class="card">
    <div class="controls-header">
      <h2>Rebanho Cadastrado</h2>
      <button type="button" class="btn-excel" onclick="exportToCSV()">📊 Exportar para Excel</button>
    </div>

    <div class="search-box">
      <input type="text" id="search" placeholder="Buscar por Proprietário(a), Nº Brinco ou Brinco Matriz..." onkeyup="renderTable()">
    </div>

    <div class="table-responsive">
      <table>
        <thead>
          <tr>
            <th>Brinco</th>
            <th>Proprietário(a)</th>
            <th>Na Propriedade?</th>
            <th>Sexo</th>
            <th>Nascimento</th>
            <th>Categoria</th>
            <th>Brinco Matriz</th>
            <th>Última Pesagem</th>
            <th>Último Peso (kg)</th>
            <th>Ações</th>
          </tr>
        </thead>
        <tbody id="animal-list">
          <!-- Conteúdo gerado via JS -->
        </tbody>
      </table>
    </div>
  </div>
</div>

<!-- MODAL DE HISTÓRICO DE PESAGEM -->
<div class="modal-backdrop" id="modal-historico">
  <div class="modal">
    <div class="modal-header">
      <h3 id="modal-animal-titulo">Histórico de Pesagem</h3>
      <button class="modal-close" onclick="fecharModal()">&times;</button>
    </div>

    <div style="background: #f8f9fa; padding: 15px; border-radius: 5px; margin-bottom: 20px;">
      <h4 style="font-size: 0.95rem; margin-bottom: 10px; color: var(--primary-dark);">Adicionar Nova Pesagem</h4>
      <form id="form-nova-pesagem" style="display: flex; gap: 10px; flex-wrap: wrap;">
        <input type="hidden" id="modal-animal-id">
        <div style="flex: 1; min-width: 140px;">
          <input type="date" id="nova-data-pesagem" required style="width:100%; padding: 8px; border: 1px solid #ccc; border-radius: 4px;">
        </div>
        <div style="flex: 1; min-width: 120px;">
          <input type="number" id="novo-peso" step="0.1" placeholder="Peso (kg)" required style="width:100%; padding: 8px; border: 1px solid #ccc; border-radius: 4px;">
        </div>
        <button type="submit" class="btn-primary" style="padding: 8px 12px;">+ Adicionar</button>
      </form>
    </div>

    <div class="table-responsive">
      <table>
        <thead>
          <tr>
            <th>Data</th>
            <th>Peso (kg)</th>
            <th>Ações</th>
          </tr>
        </thead>
        <tbody id="historico-list">
          <!-- Lista de pesagens tratada via JS -->
        </tbody>
      </table>
    </div>
  </div>
</div>

<script>
  let animais = JSON.parse(localStorage.getItem('gado_db')) || [];

  const form = document.getElementById('animal-form');
  const formTitle = document.getElementById('form-title');
  const animalIdInput = document.getElementById('animal-id');
  const proprietarioInput = document.getElementById('proprietario');
  const propriedadeInput = document.getElementById('propriedade');
  const brincoInput = document.getElementById('brinco');
  const sexoInput = document.getElementById('sexo');
  const nascimentoInput = document.getElementById('nascimento');
  const brincoMatrizInput = document.getElementById('brinco-matriz');
  const dataPesagemInput = document.getElementById('data-pesagem');
  const pesoInput = document.getElementById('peso');
  const btnSubmit = document.getElementById('btn-submit');
  const btnCancel = document.getElementById('btn-cancel');

  form.addEventListener('submit', (e) => {
    e.preventDefault();

    const id = animalIdInput.value;
    const dataPesagemVal = dataPesagemInput.value;
    const pesoVal = pesoInput.value ? parseFloat(pesoInput.value) : null;

    if (id) {
      const index = animais.findIndex(a => a.id === parseInt(id));
      if (index !== -1) {
        animais[index].proprietario = proprietarioInput.value;
        animais[index].propriedade = propriedadeInput.value;
        animais[index].brinco = brincoInput.value.trim();
        animais[index].sexo = sexoInput.value;
        animais[index].nascimento = nascimentoInput.value;
        animais[index].brincoMatriz = brincoMatrizInput.value.trim();

        if (!animais[index].historico) animais[index].historico = [];
        
        if (dataPesagemVal && pesoVal !== null) {
          const registroExistente = animais[index].historico.find(h => h.data === dataPesagemVal);
          if (registroExistente) {
            registroExistente.peso = pesoVal;
          } else {
            animais[index].historico.push({ data: dataPesagemVal, peso: pesoVal });
          }
        }
      }
    } else {
      const novoAnimal = {
        id: Date.now(),
        proprietario: proprietarioInput.value,
        propriedade: propriedadeInput.value,
        brinco: brincoInput.value.trim(),
        sexo: sexoInput.value,
        nascimento: nascimentoInput.value,
        brincoMatriz: brincoMatrizInput.value.trim(),
        historico: []
      };

      if (dataPesagemVal && pesoVal !== null) {
        novoAnimal.historico.push({ data: dataPesagemVal, peso: pesoVal });
      }

      animais.push(novoAnimal);
    }

    saveAndRender();
    resetForm();
  });

  function saveAndRender() {
    localStorage.setItem('gado_db', JSON.stringify(animais));
    renderTable();
  }

  function getUltimaPesagem(animal) {
    if (!animal.historico || animal.historico.length === 0) {
      return { data: '-', peso: null };
    }
    const historicoOrdenado = [...animal.historico].sort((a, b) => new Date(b.data) - new Date(a.data));
    return historicoOrdenado[0];
  }

  function calcularCategoria(animal) {
    const dataNascimento = new Date(animal.nascimento);
    const hoje = new Date();
    const diffDias = Math.ceil(Math.abs(hoje - dataNascimento) / (1000 * 60 * 60 * 24));

    if (animal.sexo === 'Macho') {
      return diffDias > 365 ? 'Touro' : 'Bezerro';
    }

    const possuiProle = animais.some(a => a.brincoMatriz && a.brincoMatriz === animal.brinco);
    if (possuiProle) {
      return 'Vaca';
    }

    return diffDias <= 365 ? 'Bezerra' : 'Novilha';
  }

  function updateDashboard(lista) {
    const listaNaPropriedade = lista.filter(a => (a.propriedade || 'Sim') === 'Sim');

    const total = listaNaPropriedade.length;
    let pesoTotalRebanho = 0;
    let qtdAnimaisComPeso = 0;

    const categorias = {
      'Bezerra': { count: 0, pesoTotal: 0, countComPeso: 0 },
      'Novilha': { count: 0, pesoTotal: 0, countComPeso: 0 },
      'Vaca': { count: 0, pesoTotal: 0, countComPeso: 0 },
      'Bezerro': { count: 0, pesoTotal: 0, countComPeso: 0 },
      'Touro': { count: 0, pesoTotal: 0, countComPeso: 0 }
    };

    listaNaPropriedade.forEach(animal => {
      const ultima = getUltimaPesagem(animal);
      const cat = calcularCategoria(animal);

      if (categorias[cat]) {
        categorias[cat].count++;
        if (ultima.peso !== null) {
          categorias[cat].pesoTotal += ultima.peso;
          categorias[cat].countComPeso++;
          pesoTotalRebanho += ultima.peso;
          qtdAnimaisComPeso++;
        }
      }
    });

    const pesoMedio = qtdAnimaisComPeso > 0 ? (pesoTotalRebanho / qtdAnimaisComPeso) : 0;
    const vacasCount = categorias['Vaca'].count;

    document.getElementById('stat-total').innerText = total;
    document.getElementById('stat-media-peso').innerText = `${pesoMedio.toFixed(1)} kg`;
    document.getElementById('stat-vacas').innerText = vacasCount;
    document.getElementById('stat-peso-total').innerText = `${pesoTotalRebanho.toFixed(1)} kg`;

    const containerCategorias = document.getElementById('category-grid-container');
    containerCategorias.innerHTML = '';

    for (const [nomeCat, dados] of Object.entries(categorias)) {
      const mediaPesoCat = dados.countComPeso > 0 ? (dados.pesoTotal / dados.countComPeso).toFixed(1) : '0';
      const box = document.createElement('div');
      box.className = 'category-box';
      box.innerHTML = `
        <div class="cat-title">${nomeCat}s</div>
        <div class="cat-count">${dados.count}</div>
        <div class="cat-weight">Média: ${mediaPesoCat} kg</div>
      `;
      containerCategorias.appendChild(box);
    }
  }

  function getFilteredAnimais() {
    const search = document.getElementById('search').value.toLowerCase();
    return animais.filter(a => 
      (a.proprietario && a.proprietario.toLowerCase().includes(search)) || 
      (a.nome && a.nome.toLowerCase().includes(search)) ||
      a.brinco.toLowerCase().includes(search) ||
      (a.brincoMatriz && a.brincoMatriz.toLowerCase().includes(search))
    );
  }

  function getBadgeClass(categoria) {
    switch (categoria) {
      case 'Vaca': return 'badge-vaca';
      case 'Novilha': return 'badge-novilha';
      case 'Bezerra': return 'badge-bezerra';
      case 'Touro': return 'badge-touro';
      default: return 'badge-bezerro';
    }
  }

  function renderTable() {
    const tbody = document.getElementById('animal-list');
    const animaisFiltrados = getFilteredAnimais();

    updateDashboard(animaisFiltrados);
    tbody.innerHTML = '';

    if (animaisFiltrados.length === 0) {
      tbody.innerHTML = `<tr><td colspan="10" class="empty-msg">Nenhum animal encontrado.</td></tr>`;
      return;
    }

    animaisFiltrados.forEach(animal => {
      const categoria = calcularCategoria(animal);
      const badgeClass = getBadgeClass(categoria);
      const ultimaPesagem = getUltimaPesagem(animal);
      const nomeProprietario = animal.proprietario || animal.nome || '-';
      const naPropriedade = animal.propriedade || 'Sim';

      const tr = document.createElement('tr');
      if (naPropriedade === 'Não') {
        tr.classList.add('fora-propriedade');
      }

      const pesoExibicao = ultimaPesagem.peso !== null ? `${ultimaPesagem.peso.toFixed(1)} kg` : '-';

      tr.innerHTML = `
        <td><strong>${animal.brinco}</strong></td>
        <td>${nomeProprietario}</td>
        <td>
          <span class="badge ${naPropriedade === 'Sim' ? 'badge-prop-sim' : 'badge-prop-nao'}">
            ${naPropriedade}
          </span>
        </td>
        <td>${animal.sexo}</td>
        <td>${formatDate(animal.nascimento)}</td>
        <td><span class="badge ${badgeClass}">${categoria}</span></td>
        <td>${animal.brincoMatriz || '-'}</td>
        <td>${formatDate(ultimaPesagem.data)}</td>
        <td>${pesoExibicao}</td>
        <td class="actions-cell">
          <button class="btn-info" onclick="abrirHistorico(${animal.id})">⚖️ Histórico</button>
          <button class="btn-warning" onclick="editAnimal(${animal.id})">Editar</button>
          <button class="btn-danger" onclick="deleteAnimal(${animal.id})">Excluir</button>
        </td>
      `;
      tbody.appendChild(tr);
    });
  }

  function editAnimal(id) {
    const animal = animais.find(a => a.id === id);
    if (!animal) return;

    const ultima = getUltimaPesagem(animal);

    animalIdInput.value = animal.id;
    proprietarioInput.value = animal.proprietario || animal.nome || '';
    propriedadeInput.value = animal.propriedade || 'Sim';
    brincoInput.value = animal.brinco;
    sexoInput.value = animal.sexo || 'Fêmea';
    nascimentoInput.value = animal.nascimento;
    brincoMatrizInput.value = animal.brincoMatriz || '';
    dataPesagemInput.value = ultima.data !== '-' ? ultima.data : '';
    pesoInput.value = ultima.peso !== null ? ultima.peso : '';

    formTitle.innerText = "Editar Cadastro do Animal";
    btnSubmit.innerText = "Atualizar Animal";
    btnCancel.style.display = "inline-block";
    window.scrollTo({ top: 0, behavior: 'smooth' });
  }

  function deleteAnimal(id) {
    if (confirm("Tem certeza que deseja excluir este animal e todo o seu histórico?")) {
      animais = animais.filter(a => a.id !== id);
      saveAndRender();
      resetForm();
    }
  }

  function resetForm() {
    animalIdInput.value = '';
    form.reset();
    formTitle.innerText = "Cadastrar Novo Animal";
    btnSubmit.innerText = "Salvar Animal";
    btnCancel.style.display = "none";
  }

  const modalHistorico = document.getElementById('modal-historico');
  const modalAnimalId = document.getElementById('modal-animal-id');
  const modalAnimalTitulo = document.getElementById('modal-animal-titulo');
  const formNovaPesagem = document.getElementById('form-nova-pesagem');
  const novaDataInput = document.getElementById('nova-data-pesagem');
  const novoPesoInput = document.getElementById('novo-peso');

  function abrirHistorico(id) {
    const animal = animais.find(a => a.id === id);
    if (!animal) return;

    modalAnimalId.value = animal.id;
    const propName = animal.proprietario || animal.nome || '';
    modalAnimalTitulo.innerText = `Histórico de Pesagem - Brinco: ${animal.brinco} (${propName})`;
    novaDataInput.value = new Date().toISOString().slice(0, 10);
    novoPesoInput.value = '';

    renderHistoricoModal(animal);
    modalHistorico.style.display = 'flex';
  }

  function fecharModal() {
    modalHistorico.style.display = 'none';
  }

  function renderHistoricoModal(animal) {
    const tbody = document.getElementById('historico-list');
    tbody.innerHTML = '';

    if (!animal.historico || animal.historico.length === 0) {
      tbody.innerHTML = `<tr><td colspan="3" class="empty-msg">Nenhuma pesagem registrada.</td></tr>`;
      return;
    }

    const historicoOrdenado = [...animal.historico].sort((a, b) => new Date(b.data) - new Date(a.data));

    historicoOrdenado.forEach(p => {
      const tr = document.createElement('tr');
      tr.innerHTML = `
        <td>${formatDate(p.data)}</td>
        <td>${p.peso.toFixed(1)} kg</td>
        <td>
          <button class="btn-danger" style="padding: 3px 8px; font-size: 0.8rem;" onclick="excluirPesagem(${animal.id}, '${p.data}', ${p.peso})">Remover</button>
        </td>
      `;
      tbody.appendChild(tr);
    });
  }

  formNovaPesagem.addEventListener('submit', (e) => {
    e.preventDefault();
    const id = parseInt(modalAnimalId.value);
    const animal = animais.find(a => a.id === id);

    if (animal) {
      if (!animal.historico) animal.historico = [];
      animal.historico.push({
        data: novaDataInput.value,
        peso: parseFloat(novoPesoInput.value)
      });

      saveAndRender();
      renderHistoricoModal(animal);
      novoPesoInput.value = '';
    }
  });

  function excluirPesagem(animalId, data, peso) {
    const animal = animais.find(a => a.id === animalId);
    if (!animal) return;

    if (confirm("Deseja excluir este registro de pesagem?")) {
      animal.historico = animal.historico.filter(h => !(h.data === data && h.peso === peso));
      saveAndRender();
      renderHistoricoModal(animal);
    }
  }

  function formatDate(dateString) {
    if (!dateString || dateString === '-') return '-';
    const [year, month, day] = dateString.split('-');
    return `${day}/${month}/${year}`;
  }

  function exportToCSV() {
    const lista = getFilteredAnimais();
    if (lista.length === 0) {
      alert("Não há dados para exportar.");
      return;
    }

    let csvContent = "\uFEFF";
    csvContent += "Nº Brinco;Proprietário(a);Na Propriedade;Sexo;Data Nascimento;Categoria;Nº Brinco Matriz;Data Última Pesagem;Último Peso (kg);Total Pesagens Registradas\n";

    lista.forEach(a => {
      const categoria = calcularCategoria(a);
      const ultima = getUltimaPesagem(a);
      const prop = a.proprietario || a.nome || '-';
      const pesoStr = ultima.peso !== null ? ultima.peso.toString().replace('.', ',') : '-';

      const row = [
        `"${a.brinco}"`,
        `"${prop}"`,
        `"${a.propriedade || 'Sim'}"`,
        `"${a.sexo}"`,
        `"${formatDate(a.nascimento)}"`,
        `"${categoria}"`,
        `"${a.brincoMatriz || '-'}"`,
        `"${formatDate(ultima.data)}"`,
        `"${pesoStr}"`,
        `"${a.historico ? a.historico.length : 0}"`
      ];
      csvContent += row.join(";") + "\n";
    });

    const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
    const url = URL.createObjectURL(blob);
    const link = document.createElement("a");
    link.setAttribute("href", url);
    link.setAttribute("download", `relatorio_gado_bovino_${new Date().toISOString().slice(0,10)}.csv`);
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
  }

  renderTable();
</script>

</body>
</html># LashFB.github.io
