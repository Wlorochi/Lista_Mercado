<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>Lista de Mercado</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        :root {
            --primary: #6366f1;
            --bg: #0f172a;
            --card: #1e293b;
            --text: #f8fafc;
            --accent: #22c55e;
            --danger: #ef4444;
            --warning: #f59e0b;
            --cancel: #475569;
        }

        body { font-family: 'Inter', sans-serif; background: var(--bg); color: var(--text); margin: 0; padding-bottom: 120px; }
        .container { width: 100%; max-width: 480px; margin: 0 auto; padding: 20px; box-sizing: border-box; position: relative; min-height: 100vh; }
        
        #tela-login, #tela-escolha, #tela-historico { display: flex; flex-direction: column; align-items: center; min-height: 80vh; text-align: center; }
        #app-principal, #tela-escolha, #tela-historico { display: none; }

        .btn-principal { background: var(--primary); color: white; border: none; padding: 18px; border-radius: 15px; font-weight: bold; width: 100%; margin-bottom: 12px; cursor: pointer; font-size: 1rem; }
        
        input[type=number]::-webkit-inner-spin-button, 
        input[type=number]::-webkit-outer-spin-button { -webkit-appearance: none; margin: 0; }
        input[type=number] { -moz-appearance: textfield; }

        input { 
            background: var(--card); border: 1px solid #475569; padding: 15px; border-radius: 12px; 
            color: white; width: 100%; box-sizing: border-box; font-size: 16px; outline: none; 
            margin-bottom: 10px; text-align: center;
        }
        
        .painel-mensal { background: linear-gradient(135deg, #1e293b, #334155); width: 100%; padding: 20px; border-radius: 20px; margin-bottom: 20px; border: 1px solid var(--primary); box-sizing: border-box; }
        .mes-item { display: flex; justify-content: space-between; padding: 8px 0; border-bottom: 1px solid #475569; }

        .grid-cat { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-bottom: 20px; }
        .btn-cat { padding: 12px; border: none; border-radius: 12px; color: white; font-weight: 800; font-size: 0.7rem; cursor: pointer; }
        .cat-1 { background: #ef4444; } .cat-2 { background: #3b82f6; } 
        .cat-3 { background: #f59e0b; } .cat-4 { background: #10b981; }
        .cat-5 { background: #8b5cf6; } .cat-6 { background: #ec4899; }
        .cat-7 { background: #06b6d4; } .cat-8 { background: #f97316; }

        .item { display: flex; justify-content: space-between; align-items: center; background: var(--card); padding: 12px 15px; border-radius: 15px; margin-bottom: 8px; border-left: 5px solid var(--primary); }
        .btn-check { background: var(--primary); border: none; color: white; padding: 8px 12px; border-radius: 10px; font-weight: bold; }
        .item-carrinho { border-left-color: var(--accent); background: #064e3b; }
        
        #overlay { display: none; position: fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.9); z-index: 1000; align-items:center; justify-content:center; }
        .modal { background: var(--card); width: 85%; padding: 25px; border-radius: 25px; border: 1px solid #475569; }

        .btn-cancelar { background: var(--cancel); color: #cbd5e1; border: none; padding: 15px; border-radius: 15px; font-weight: bold; width: 100%; cursor: pointer; margin-top: 10px; }
        .btn-perigo { background: var(--danger); color: white; border: none; padding: 12px; border-radius: 12px; font-weight: bold; width: 100%; cursor: pointer; margin-top: 10px; }
        
        .footer { position: fixed; bottom: 0; left: 0; width: 100%; background: #1e293b; padding: 20px; border-top: 2px solid var(--primary); display: flex; justify-content: space-between; align-items: center; box-sizing: border-box; }
        
        .hist-card { background: var(--card); padding: 15px; border-radius: 15px; margin-bottom: 10px; width: 100%; border-left: 3px solid #64748b; text-align: left; box-sizing: border-box; }
        
        .search-box { background: #0f172a; border: 1px solid var(--primary); border-radius: 10px; padding: 8px; color: white; width: 100%; margin-bottom: 15px; font-size: 0.9rem; text-align: left; }
        
        .header-categoria { font-size: 0.75rem; font-weight: bold; color: #94a3b8; text-align: left; margin: 15px 0 5px 5px; text-transform: uppercase; letter-spacing: 1px; }

        /* ESTILO DO CRÉDITO */
        .creditos { 
            font-size: 0.7rem; 
            color: #64748b; 
            margin-top: 30px; 
            font-weight: bold; 
            letter-spacing: 1px;
            opacity: 0.8;
        }
    </style>
</head>
<body>

<div class="container">
    <div id="tela-login">
        <div style="font-size: 50px; margin-top: 50px;">🛒</div>
        <h1 style="margin-bottom: 30px; color: var(--primary);">Lista de Mercado</h1>
        <button class="btn-principal" onclick="mostrarEscolha()">ABRIR LISTAS</button>
        <button class="btn-principal" style="background:var(--accent)" onclick="mostrarHistorico()">GASTOS MENSAIS</button>
        <div class="creditos">Desenvolvido por WLaurenco</div>
    </div>

    <div id="tela-escolha">
        <h2 style="margin-top: 50px;">Quem vai usar?</h2>
        <button class="btn-principal" style="background:#3b82f6" onclick="abrirLista('Marido')">LISTA DO MARIDO</button>
        <button class="btn-principal" style="background:#ec4899" onclick="abrirLista('Esposa')">LISTA DA ESPOSA</button>
        <button class="btn-principal" style="background:#10b981" onclick="abrirLista('Filho')">LISTA DO FILHO</button>
        <button class="btn-cancelar" style="width:100%" onclick="voltarInicio()">VOLTAR</button>
        <div class="creditos">Desenvolvido por WLaurenco</div>
    </div>

    <div id="tela-historico" style="padding-top: 20px;">
        <h2>Histórico de Gastos</h2>
        <div class="painel-mensal" id="painel-mensal"></div>
        <button class="btn-principal" style="background:#25d366; font-size:0.8rem" onclick="compartilharZap()">ENVIAR RESUMO POR WHATSAPP</button>
        <div id="cont-hist" style="width:100%"></div>
        <button class="btn-perigo" style="margin-top:20px" onclick="zerarGastosMensais()">ZERAR GASTOS (INÍCIO DE ANO)</button>
        <button class="btn-cancelar" style="margin-top:10px" onclick="voltarInicio()">VOLTAR AO INÍCIO</button>
        <div class="creditos">Desenvolvido por WLaurenco</div>
    </div>

    <div id="app-principal" style="padding-top: 20px;">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:20px">
            <span id="titulo-lista" style="font-weight:900; color:var(--primary)">LISTA</span>
            <button onclick="voltarInicio()" style="background:none; border:none; color:#64748b; font-weight:bold">SAIR</button>
        </div>

        <input type="text" id="inputNome" placeholder="O que comprar?">
        
        <div class="grid-cat">
            <button class="btn-cat cat-1" onclick="addItem('Açougue')">🥩 Açougue</button>
            <button class="btn-cat cat-2" onclick="addItem('Alimentos')">🍚 Alimentos</button>
            <button class="btn-cat cat-3" onclick="addItem('Limpeza')">🧹 Limpeza</button>
            <button class="btn-cat cat-4" onclick="addItem('Frutas')">🍎 Frutas</button>
            <button class="btn-cat cat-5" onclick="addItem('Padaria')">🥖 Padaria</button>
            <button class="btn-cat cat-6" onclick="addItem('Higiene')">🧴 Higiene</button>
            <button class="btn-cat cat-7" onclick="addItem('Bebidas')">🥤 Bebidas</button>
            <button class="btn-cat cat-8" onclick="addItem('Hortifrúti')">🥦 Hortifrúti</button>
        </div>

        <input type="text" class="search-box" id="searchItem" placeholder="🔍 Buscar na lista..." onkeyup="render()">

        <div id="lista-pendentes"></div>
        <div style="color:var(--accent); font-weight:900; margin: 30px 0 10px; text-align: left;">🛒 NO CARRINHO</div>
        <div id="lista-carrinho"></div>
        
        <button class="btn-perigo" style="margin-top: 40px;" onclick="limparListaAtiva()">LIMPAR ESTA LISTA</button>
        <div class="creditos" style="text-align:center">Desenvolvido por WLaurenco</div>
    </div>
</div>

<div class="footer" id="footer-app" style="display:none">
    <div>
        <small style="font-weight:bold; opacity:0.7">TOTAL</small>
        <div id="v-total" style="font-size:1.6rem; font-weight:900">R$ 0,00</div>
    </div>
    <button class="btn-principal" style="width:auto; background:var(--accent); margin:0; padding:10px 20px" onclick="finalizarCompra()">FINALIZAR</button>
</div>

<div id="overlay">
    <div class="modal">
        <h2 id="m-nome">Item</h2>
        <div style="display:flex; gap:10px">
            <div style="flex:1">
                <label style="font-size:0.7rem; display:block">QTD (Padrão: 1)</label>
                <input type="number" id="m-qtd" inputmode="numeric" placeholder="1">
            </div>
            <div style="flex:2">
                <label style="font-size:0.7rem; display:block">PREÇO UNIT.</label>
                <input type="number" id="m-preco" inputmode="decimal">
            </div>
        </div>
        <button class="btn-principal" style="margin-top:20px; margin-bottom: 0;" onclick="confirmarPeguei()">CONFIRMAR</button>
        <button class="btn-cancelar" style="width:100%" onclick="fecharModal()">CANCELAR</button>
    </div>
</div>

<script>
    let banco = JSON.parse(localStorage.getItem('listaMercado_vFinal_Full')) || {
        listaMarido: { pendentes: [], carrinho: [], total: 0 },
        listaEsposa: { pendentes: [], carrinho: [], total: 0 },
        listaFilho: { pendentes: [], carrinho: [], total: 0 },
        historico: []
    };
    let listaAtiva = 'Marido';
    let itemSelecionado = null;

    function salvar() { localStorage.setItem('listaMercado_vFinal_Full', JSON.stringify(banco)); }
    function mostrarEscolha() {
        document.getElementById('tela-login').style.display = 'none';
        document.getElementById('tela-escolha').style.display = 'flex';
    }
    function abrirLista(user) {
        listaAtiva = user;
        document.getElementById('titulo-lista').innerText = "LISTA DO(A) " + user.toUpperCase();
        document.getElementById('tela-escolha').style.display = 'none';
        document.getElementById('app-principal').style.display = 'block';
        document.getElementById('footer-app').style.display = 'flex';
        render();
    }
    function voltarInicio() { location.reload(); }

    function addItem(cat) {
        const nome = document.getElementById('inputNome').value;
        if(!nome) return;
        banco["lista"+listaAtiva].pendentes.push({ id: Date.now(), nome, cat });
        document.getElementById('inputNome').value = "";
        render(); salvar();
    }

    function abrirModal(id) {
        itemSelecionado = banco["lista"+listaAtiva].pendentes.find(i => i.id === id);
        document.getElementById('m-nome').innerText = itemSelecionado.nome;
        document.getElementById('m-qtd').value = ""; 
        document.getElementById('m-preco').value = ""; 
        document.getElementById('overlay').style.display = 'flex';
        setTimeout(() => document.getElementById('m-preco').focus(), 150);
    }

    function fecharModal() { document.getElementById('overlay').style.display = 'none'; }

    function confirmarPeguei() {
        let qVal = document.getElementById('m-qtd').value;
        if (qVal === "" || qVal <= 0) qVal = "1";
        const pVal = document.getElementById('m-preco').value;
        if(!pVal) { alert("Informe o Preço!"); return; }
        const sub = parseFloat(qVal) * parseFloat(pVal);
        const L = banco["lista"+listaAtiva];
        L.carrinho.unshift({ ...itemSelecionado, qtd: qVal, subtotal: sub });
        L.total += sub;
        L.pendentes = L.pendentes.filter(i => i.id !== itemSelecionado.id);
        fecharModal(); render(); salvar();
    }

    function render() {
        const L = banco["lista"+listaAtiva];
        const termoBusca = document.getElementById('searchItem').value.toLowerCase();
        const dPend = document.getElementById('lista-pendentes');
        const dCart = document.getElementById('lista-carrinho');
        dPend.innerHTML = ""; dCart.innerHTML = "";

        const categorias = [...new Set(L.pendentes.map(i => i.cat))].sort();
        
        categorias.forEach(cat => {
            const itensDaCat = L.pendentes.filter(i => i.cat === cat && i.nome.toLowerCase().includes(termoBusca));
            if(itensDaCat.length > 0) {
                dPend.innerHTML += `<div class="header-categoria">${cat}</div>`;
                itensDaCat.forEach(i => {
                    dPend.innerHTML += `<div class="item">
                        <span style="text-align:left">${i.nome}</span>
                        <div><button class="btn-check" onclick="abrirModal(${i.id})">PEGUEI</button>
                        <button onclick="removerPendente(${i.id})" style="background:none; border:none; margin-left:10px">🗑️</button></div>
                    </div>`;
                });
            }
        });

        L.carrinho.forEach((i, idx) => {
            if(i.nome.toLowerCase().includes(termoBusca)) {
                dCart.innerHTML += `<div class="item item-carrinho">
                    <div style="text-align:left"><strong>${i.qtd}x</strong> ${i.nome} <small style="display:block; opacity:0.5">${i.cat}</small></div>
                    <div style="text-align:right">
                        <div style="font-weight:900">R$ ${i.subtotal.toFixed(2)}</div>
                        <button onclick="devolver(${idx})" style="background:none; border:none; color:var(--warning); font-size:0.75rem; font-weight:bold;">Volta</button>
                    </div>
                </div>`;
            }
        });
        document.getElementById('v-total').innerText = `R$ ${L.total.toFixed(2)}`;
    }

    function removerPendente(id) {
        banco["lista"+listaAtiva].pendentes = banco["lista"+listaAtiva].pendentes.filter(i => i.id !== id);
        render(); salvar();
    }

    function devolver(idx) {
        const L = banco["lista"+listaAtiva];
        L.total -= L.carrinho[idx].subtotal;
        L.pendentes.push({ id: L.carrinho[idx].id, nome: L.carrinho[idx].nome, cat: L.carrinho[idx].cat });
        L.carrinho.splice(idx, 1);
        render(); salvar();
    }

    function finalizarCompra() {
        const L = banco["lista"+listaAtiva];
        if(!L.carrinho.length) return;
        const agora = new Date();
        banco.historico.unshift({
            data: agora.toLocaleString('pt-BR'),
            mesAno: agora.toLocaleDateString('pt-BR', { month: 'long', year: 'numeric' }),
            total: L.total,
            usuario: listaAtiva
        });
        banco["lista"+listaAtiva] = { pendentes: [], carrinho: [], total: 0 };
        salvar();
        alert("Salvo no Histórico!");
        voltarInicio();
    }

    function mostrarHistorico() {
        document.getElementById('tela-login').style.display = 'none';
        document.getElementById('tela-historico').style.display = 'flex';
        const gastosMes = {};
        banco.historico.forEach(h => {
            gastosMes[h.mesAno] = (gastosMes[h.mesAno] || 0) + h.total;
        });
        const painel = document.getElementById('painel-mensal');
        painel.innerHTML = '<h3 style="margin-top:0; font-size:0.9rem">RESUMO MENSAL</h3>';
        for (const [mes, valor] of Object.entries(gastosMes)) {
            painel.innerHTML += `<div class="mes-item"><span style="text-transform: capitalize;">${mes}</span><span style="font-weight:900; color:var(--accent)">R$ ${valor.toFixed(2)}</span></div>`;
        }
        const cont = document.getElementById('cont-hist');
        cont.innerHTML = banco.historico.length ? '<h3 style="font-size:0.8rem; margin: 20px 0 10px; opacity:0.6">COMPRAS FINALIZADAS</h3>' : "Histórico vazio.";
        banco.historico.forEach(h => {
            cont.innerHTML += `<div class="hist-card">
                <div style="font-size:0.7rem; opacity:0.6">${h.data} - ${h.usuario}</div>
                <div style="font-size:1.1rem; font-weight:900; color:var(--primary)">R$ ${h.total.toFixed(2)}</div>
            </div>`;
        });
    }

    function compartilharZap() {
        if (banco.historico.length === 0) { alert("Histórico vazio!"); return; }
        let msg = "*RESUMO DE GASTOS - LISTA DE MERCADO*\n\n";
        const gastosMes = {};
        banco.historico.forEach(h => {
            gastosMes[h.mesAno] = (gastosMes[h.mesAno] || 0) + h.total;
        });
        for (const [mes, valor] of Object.entries(gastosMes)) {
            msg += `*${mes}:* R$ ${valor.toFixed(2)}\n`;
        }
        window.open(`https://wa.me/?text=${encodeURIComponent(msg)}`);
    }

    function limparListaAtiva() {
        if(confirm("Deseja limpar todos os itens desta lista?")) {
            banco["lista"+listaAtiva] = { pendentes: [], carrinho: [], total: 0 };
            salvar(); render();
        }
    }

    function zerarGastosMensais() {
        if(confirm("Deseja apagar TODO o histórico de gastos do ano?")) {
            banco.historico = [];
            salvar(); mostrarHistorico();
        }
    }
</script>
</body>
</html>
