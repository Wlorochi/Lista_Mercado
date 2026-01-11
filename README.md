<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>Painel Unimed Kids - Notícias ES & Unimed</title>
    <style>
        :root { 
            --verde-unimed: #006544; 
            --verde-fundo-claro: #28a745; 
            --verde-brilhante: #99cc33; 
            --azul-kids: #00aeef; 
            --fundo: #f0f2f5;
        }

        body, html { 
            margin: 0; padding: 0; width: 100%; height: 100%; 
            background: var(--fundo); font-family: 'Ubuntu', sans-serif;
            overflow: hidden;
        }

        .painel-principal {
            height: 100vh; width: 100vw;
            display: grid;
            grid-template-rows: 150px 1fr 130px;
            grid-template-columns: 420px 1fr;
            grid-template-areas: "topo topo" "lateral video" "rodape rodape";
            gap: 20px; padding: 20px; box-sizing: border-box;
        }

        .bloco {
            background: white; border-radius: 45px; 
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            overflow: hidden; border: 5px solid white;
        }

        header { 
            grid-area: topo; display: flex; flex-direction: column; justify-content: center; align-items: center; 
            background: white; position: relative;
        }

        .area-pasta { position: absolute; left: 25px; top: 15px; }
        .icone-pasta { font-size: 1.5rem; cursor: pointer; opacity: 0.4; transition: 0.3s; }
        .icone-pasta:hover { opacity: 1; }
        #input-video { display: none; }
        
        /* TEMPERATURA COMPACTA */
        .temp-box-topo { 
            position: absolute; right: 40px; top: 15px; 
            text-align: right; display: flex; flex-direction: column; align-items: flex-end;
        }
        .temp-localidade { font-size: 0.85rem; font-weight: 800; color: var(--azul-kids); text-transform: uppercase; margin-bottom: -4px; }
        .temp-info-container { display: flex; align-items: center; gap: 6px; }
        .temp-icone { font-size: 1.8rem; }
        .temp-valor { font-size: 2.2rem; font-weight: 900; color: var(--verde-unimed); line-height: 1; }

        .logo { font-size: 45px; font-weight: 800; color: var(--verde-unimed); margin-top: 5px; }
        .logo span { color: var(--azul-kids); }
        
        .barra-financeira {
            background: #1a1a1a; width: 100%; padding: 10px 0;
            display: flex; justify-content: center; gap: 60px;
            font-weight: bold; font-size: 1.1rem; color: #fff; margin-top: 10px;
        }

        .lateral { grid-area: lateral; display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center; padding: 40px; }
        #titulo-ad { color: var(--azul-kids); font-size: 2.5rem; margin-bottom: 20px; font-weight: 900; transition: 0.5s; }
        #desc-ad { font-size: 1.5rem; color: #444; font-weight: 500; transition: 0.5s; }

        .video-box { grid-area: video; background: #000; }
        video { width: 100%; height: 100%; object-fit: contain; }

        /* RODAPÉ MISTO ES + UNIMED */
        footer { 
            grid-area: rodape; background: var(--verde-fundo-claro); 
            display: flex; align-items: center; border: 5px solid white; border-radius: 45px; 
        }
        .relogio-box { 
            height: 100%; min-width: 300px; 
            display: flex; flex-direction: column; justify-content: center; align-items: center; 
            border-right: 4px solid rgba(255,255,255,0.3); color: white; 
        }
        #txt-hora { font-size: 3.8rem; font-weight: 900; line-height: 1; }
        #txt-data { font-size: 1.2rem; font-weight: bold; }

        .letreiro-container { flex: 1; overflow: hidden; white-space: nowrap; display: flex; align-items: center; }
        .letreiro-texto { 
            display: inline-block; font-size: 2.1rem; font-weight: 800; 
            color: #ffffff; padding-left: 100%; 
            animation: scroll-news 100s linear infinite;
        }
        @keyframes scroll-news { 0% { transform: translateX(0); } 100% { transform: translateX(-100%); } }
    </style>
</head>
<body onclick="liberarSom()">

<div class="painel-principal">
    <header class="bloco">
        <div class="area-pasta">
            <label for="input-video" class="icone-pasta">📂</label>
            <input type="file" id="input-video" accept="video/*" multiple>
        </div>
        <div class="logo">Unimed <span>Kids</span></div>
        <div class="temp-box-topo">
            <div class="temp-localidade">Vitória, ES</div>
            <div class="temp-info-container">
                <span id="clima-icone" class="temp-icone">☀️</span>
                <span class="temp-valor" id="temp-display">--°C</span>
            </div>
        </div>
        <div class="barra-financeira">
            <div class="moeda-item">DÓLAR: <span id="usd">...</span></div>
            <div class="moeda-item">EURO: <span id="eur">...</span></div>
            <div class="moeda-item">BITCOIN: <span id="btc" style="color: #f7931a">...</span></div>
            <div class="moeda-item">IBOVESPA: <span style="color: #00ff00;">+1.2%</span></div>
        </div>
    </header>

    <aside class="lateral bloco">
        <h2 id="titulo-ad">Giro ES</h2>
        <p id="desc-ad">Carregando notícias e avisos Unimed Vitória...</p>
    </aside>

    <main class="video-box bloco">
        <video id="video-player" autoplay></video>
    </main>

    <footer>
        <div class="relogio-box">
            <div id="txt-hora">00:00</div>
            <div id="txt-data">--/--/--</div>
        </div>
        <div class="letreiro-container">
            <div class="letreiro-texto" id="news-bar">INFORMAÇÕES: [ES] JORNAIS DO ESTADO & [UNIMED] VITÓRIA ••• CARREGANDO...</div>
        </div>
    </footer>
</div>

<script>
    const player = document.getElementById('video-player');
    const inputVideo = document.getElementById('input-video');
    let listaDeVideos = [];
    let videoAtual = -1;
    let somAtivado = false;

    function liberarSom() {
        if (!somAtivado) {
            player.muted = false;
            player.volume = 1.0;
            somAtivado = true;
        }
    }

    inputVideo.addEventListener('change', (e) => {
        listaDeVideos = Array.from(e.target.files);
        if (listaDeVideos.length > 0) tocarProximo();
    });

    function tocarProximo() {
        if (listaDeVideos.length === 0) return;
        videoAtual = (videoAtual + 1) % listaDeVideos.length;
        player.src = URL.createObjectURL(listaDeVideos[videoAtual]);
        player.play();
    }
    player.onended = tocarProximo;

    // BUSCA MISTA: NOTÍCIAS ES + UNIMED VITÓRIA
    async function buscarNoticiasMistas() {
        const avisosUnimed = [
            "[UNIMED] PRECISE DE MÉDICO AGORA? USE A TELECONSULTA PELO APP UNIMED VITÓRIA.",
            "[UNIMED] VACINAÇÃO EM DIA É PROTEÇÃO PARA OS PEQUENOS. VISITE NOSSA UNIDADE.",
            "[UNIMED] UNIMED VITÓRIA: ELEITA PELA 30ª VEZ A MARCA MAIS LEMBRADA PELOS CAPIXABAS.",
            "[UNIMED] CUIDAR DE VOCÊ, ESSE É O PLANO."
        ];

        try {
            const response = await fetch('https://newsdata.io/api/1/news?apikey=pub_3675276e5d8a866166e578c7c90861618a8b&q=espirito%20santo%20OR%20vitoria&country=br&language=pt');
            const data = await response.json();
            
            let newsFinal = "";
            if (data.results && data.results.length > 0) {
                // Intercala uma notícia do ES com um aviso da Unimed
                data.results.slice(0, 8).forEach((n, index) => {
                    newsFinal += `[ES] ${n.title.toUpperCase()} ••• `;
                    if (index < avisosUnimed.length) {
                        newsFinal += `${avisosUnimed[index]} ••• `;
                    }
                });
            } else {
                newsFinal = avisosUnimed.join(" ••• ");
            }
            document.getElementById('news-bar').innerText = newsFinal;
        } catch (e) {
            document.getElementById('news-bar').innerText = avisosUnimed.join(" ••• ");
        }
    }

    async function atualizarClima() {
        try {
            const res = await fetch('https://api.open-meteo.com/v1/forecast?latitude=-20.3155&longitude=-40.3128&current_weather=true');
            const data = await res.json();
            document.getElementById('temp-display').innerText = Math.round(data.current_weather.temperature) + "°C";
            const code = data.current_weather.weathercode;
            let icon = "☀️";
            if (code >= 1 && code <= 3) icon = "☁️";
            if (code >= 51) icon = "🌧️";
            document.getElementById('clima-icone').innerText = icon;
        } catch (e) {}
    }

    const ads = [
        {t: "Giro de Notícias", d: "As manchetes da Folha Vitória e jornais do ES no seu painel."},
        {t: "Unimed Vitória", d: "Sempre perto de você com a melhor rede de atendimento do estado."},
        {t: "Dica de Saúde", d: "Beber água e se alimentar bem ajuda as crianças a crescerem fortes!"}
    ];
    let adIdx = 0;
    function girarAds() {
        const t = document.getElementById('titulo-ad'), d = document.getElementById('desc-ad');
        t.style.opacity = 0; d.style.opacity = 0;
        setTimeout(() => {
            t.innerText = ads[adIdx].t;
            d.innerText = ads[adIdx].d;
            t.style.opacity = 1; d.style.opacity = 1;
            adIdx = (adIdx + 1) % ads.length;
        }, 500);
    }

    async function atualizarFinanceiro() {
        try {
            const response = await fetch('https://economia.awesomeapi.com.br/last/USD-BRL,EUR-BRL,BTC-BRL');
            const data = await response.json();
            document.getElementById('usd').innerText = "R$ " + parseFloat(data.USDBRL.bid).toFixed(2);
            document.getElementById('eur').innerText = "R$ " + parseFloat(data.EURBRL.bid).toFixed(2);
            document.getElementById('btc').innerText = "R$ " + (parseFloat(data.BTCBRL.bid) / 1000).toFixed(1) + "k";
        } catch (e) {}
    }

    setInterval(() => {
        const agora = new Date();
        document.getElementById('txt-hora').innerText = agora.getHours().toString().padStart(2, '0') + ":" + agora.getMinutes().toString().padStart(2, '0');
        document.getElementById('txt-data').innerText = agora.toLocaleDateString('pt-BR');
    }, 1000);

    window.onload = () => {
        atualizarClima();
        atualizarFinanceiro();
        buscarNoticiasMistas();
        setInterval(girarAds, 6000);
        setInterval(buscarNoticiasMistas, 1200000); // Atualiza tudo a cada 20 min
        setInterval(atualizarClima, 900000);
    };
</script>
</body>
</html>
