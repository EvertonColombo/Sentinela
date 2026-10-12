# Sentinela do Tempo

Painel web de tempo severo para qualquer ponto do Brasil: vento, chuva, raios, tempestades, ciclones e nevoeiro.
Gratuito, código aberto, sem cadastro e sem chave de API. Os dados vêm do [Open-Meteo](https://open-meteo.com).

**✨ Vibe coded com Claude Opus 5.5.** Todo o código foi escrito em conversa com a IA da Anthropic, a partir de ideias, testes e ajustes meus.

> [!WARNING]
> **Projeto pessoal e experimental. Pode conter erros**, tanto nos dados (previsões de modelos erram, principalmente para tempestades isoladas e efeitos do relevo) quanto no próprio código.
> Como o código foi gerado com IA e revisado por uma pessoa só, vale desconfiar e conferir.
> **Não use como única fonte para decisões de segurança.** Siga sempre os avisos oficiais da **Defesa Civil** (cadastre seu CEP por SMS no **40199**) e do **INMET**.

## Como usar

Abra o link do GitHub Pages no navegador, no computador ou no celular. Escolha uma capital, busque sua cidade, use "📍 Onde estou" ou cole coordenadas (ou um link do Google Maps). O painel se atualiza sozinho a cada 30 minutos.

### Atalho na tela inicial do celular

- **iPhone (Safari):** botão Compartilhar → "Adicionar à Tela de Início".
- **Android (Chrome):** menu ⋮ → "Instalar app" ou "Adicionar à tela inicial".

Pelo atalho, o painel abre em tela cheia, como um app.

## Como o painel lê os modelos

O nível do topo da faixa é o **aviso oficial do INMET**. Abaixo dele, o painel compara **5 modelos globais**: ECMWF, GFS, ICON, GEM e UKMO, com a mesma regra em toda a página.

- **Riscos** (vento, chuva, tempestade, queda de pressão): o valor é o maior que **pelo menos 2 modelos** atingem **na mesma hora**. Um modelo sozinho, exagerando para cima ou para baixo, não muda o número nem o nível.
- **Condições gerais** (temperatura, pressão, Comparar dias): o valor é a **mediana** dos modelos, tirando o modelo que destoar muito dos outros.
- **Tempestade:** energia (CAPE) e vento em altura (cisalhamento) vêm sempre do mesmo modelo e da mesma hora. Só há risco quando os dois aparecem juntos.
- **Modelo que destoa:** nos medidores, cada modelo é um ponto. O que se afasta muito dos outros aparece em rosa e fica fora da conta.

**Uma regra de cores em toda a página** (riscos, matriz, Hoje e dia aberto):
- nas colunas de chuva do hora a hora, cada coluna mostra o que pelo menos 2 modelos preveem naquela hora, e a altura é a quantidade de chuva; uma coluna cinza quer dizer que só um modelo prevê chuva ali; amarelo a partir de 20 mm na hora, laranja a partir de 30 e vermelho a partir de 60;
- **azul** é previsto por 2 ou mais modelos;
- **cinza** é previsto por um modelo só (sem acordo);
- **amarelo, laranja e vermelho** são atenção, alerta e severo, os mesmos degraus do INMET;
- **violeta** é trovoada;
- **cinza-azulado** é nevoeiro.

Na matriz, cada linha de modelo mostra um **ponto** quando aquele modelo prevê o fenômeno, mas ainda abaixo do limite (chuva a partir de 0,2 mm na hora, rajada a partir de 30 km/h, instabilidade a partir de 500 J/kg, queda de pressão a partir de 4 hPa). A célula fica **cheia na cor do nível** quando passa do limite. Na linha **"quantos"**, a altura é o número de modelos que preveem o fenômeno ou já estão em risco naquela hora. Nas abas de raios e nevoeiro, a célula é preenchida na cor do fenômeno, violeta ou cinza-azulado.

Níveis: **sem risco**, **atenção**, **alerta** e **severo**.

| Categoria | Atenção | Alerta | Severo |
|---|---|---|---|
| Rajada (km/h) | 40 | 60 | 100 |
| Chuva em 1 hora (mm) | 20 | 30 | 60 |
| Chuva em 24 horas (mm) | 30 | 50 | 100 |
| Queda de pressão em 24 horas (hPa) | 8 | 12 | 18 |
| Tempestade (instabilidade CAPE J/kg + vento em altura m/s) | 1.000 + 15 | 1.500 + 20 | 2.500 + 25 |
| Trovoada prevista (2 ou mais modelos na mesma hora) | sim | — | — |

Vento e chuva seguem os mesmos degraus dos avisos do INMET (amarelo = perigo potencial, laranja = perigo, vermelho = grande perigo). Para 24 horas, o INMET fala em "até 50 mm" no amarelo; aqui a atenção começa em 30 mm. Os limites podem ser editados no bloco `CFG`, no início do `<script>` do `index.html`.

## O que tem na página

A página tem quatro partes, de cima para baixo.

**1. Para todo mundo**
- **Céu do título com o tempo de agora:** sol, nuvens, chuva, noite estrelada e relâmpagos só quando há trovoada prevista, com a temperatura e a condição no canto.
- **Faixa do topo:** o **aviso oficial do INMET** para o ponto (perigo potencial, perigo ou grande perigo), com tipo e horário, e uma linha com o que os modelos mostram. Se o INMET não puder ser consultado, vale o nível dos modelos, com aviso.
- **Hoje:** temperatura; chuva, com o total do dia, a chance e as horas com chuva prevista por 2 ou mais modelos; vento em bússola; arco do sol com nascer e pôr.

**2. Riscos nos próximos 10 dias**

Um cartão por risco (tempestade, vento, chuva, raios, queda de pressão e nevoeiro), do mais grave para o mais leve. Cada cartão mostra:
- o nível, o primeiro período com acordo entre modelos e quantos períodos vêm depois;
- o pico (rajada, chuva em 24 horas, instabilidade, queda de pressão, visibilidade) e quantos modelos concordam;
- uma faixa de agora até 10 dias, com uma marca por hora e uma linha nas 72 horas. Depois de 3 dias, o cartão leva a etiqueta **tendência**, porque os modelos erram mais.

Quando só um modelo prevê o risco, o cartão aparece tracejado, como "sem acordo". Tocar no cartão, ou num ponto da faixa, abre o dia certo na aba certa.

**3. Próximos 10 dias**

Uma linha por dia: ícone, chance de chuva, mínima, curva da temperatura ao longo do dia e máxima. Embaixo do nome do dia aparecem um ponto para cada risco com acordo e a chuva prevista no dia.

**Tocar num dia abre o dia ali mesmo**, com uma aba para cada assunto: **temperatura, chuva, vento, tempestade, raios, queda de pressão e nevoeiro**. Cada aba mostra, sempre na mesma ordem:
- quando, por quanto tempo, quantos modelos concordam e um número do dia (chuva total, rajada máxima, menor visibilidade…);
- a **matriz** daquele dia: uma linha por modelo e uma coluna por hora, com a linha do INMET no topo e a linha "quantos" embaixo. Os períodos com acordo ficam contornados, e ao passar o dedo ou o mouse um cursor mostra a hora. Embaixo vem uma explicação em linguagem simples. Na aba temperatura, no lugar da matriz, aparece a faixa de mínima a máxima de cada modelo, com o que destoa em rosa;
- o **hora a hora** do dia (curva, ou colunas na chuva), com a opção de ver a linha de cada modelo, e o **detalhe da hora** escolhida;
- na aba tempestade, o gráfico de **ingredientes** (instabilidade e vento em altura).

A aba **queda de pressão** mostra quanto a pressão do ar cai em 24 horas. Queda pequena costuma ser só mudança de tempo, como uma frente fria; só uma queda forte pode indicar ciclone. Por isso o nome não fala em ciclone.

A aba **nevoeiro** considera nevoeiro ou cerração quando o modelo prevê o código de nevoeiro, visibilidade abaixo de 1 km, ou ar saturado (umidade de 97% ou mais) com vento fraco. É um aviso para quem vai pegar a estrada e não muda o nível de risco do painel.

Ao abrir a página, já vem aberto o dia do risco mais grave das próximas 72 horas; se não houver risco, abre hoje.

**4. Contexto e referência**
- **Neste dia, desde 1940:** como costuma ser a data no lugar escolhido, com dados da **reanálise ERA5**, ou seja, o que aconteceu, e não previsão. Mostra a máxima de cada ano, a normal de 1991 a 2020, recordes e tendência. Carrega depois do resto e fica guardado no navegador até o dia seguinte.
- **Comparar dias:** ontem, hoje e amanhã em 3D, hora a hora, e em barras com a diferença entre os dias.
- **Os 5 modelos:** origem, tamanho do quadrado e última rodada de cada um, o que cada um prevê em réguas visuais (com a tabela recolhida) e o cartão **"E o INMET?"**, sobre o modelo COSMO que o INMET roda.
- **Termos explicados** e **links úteis** (radar, raios ao vivo, satélite, avisos oficiais).

## Privacidade

- Não há servidor, conta nem rastreamento. A página só busca previsões no Open-Meteo e nomes de cidades na busca.
- **Meus lugares** ficam guardados apenas no navegador em que você salvou (localStorage). Em outro aparelho ou navegador, salve de novo.
- **Sem inteligência artificial rodando:** a IA ajudou a escrever o código, mas a página em funcionamento não consulta nenhuma IA. Todos os números e textos saem de regras fixas em JavaScript, sempre com o mesmo resultado para os mesmos dados.

## Arquivos

| Arquivo | Para que serve |
|---|---|
| `index.html` | O painel inteiro (HTML, CSS e JavaScript num arquivo só). |
| `manifest.webmanifest` | Nome, ícone e cores usados quando o painel é instalado na tela inicial. |
| `icons/` | Ícones para a aba do navegador, iPhone e Android. |

Bibliotecas carregadas de CDN: [three.js](https://threejs.org) (cenas 3D) e [Chart.js](https://www.chartjs.org) (gráficos). Sem internet para elas, o painel funciona sem essas partes.

## Limites honestos

- Os modelos globais têm resolução de 9 a 25 km. Eles não "veem" uma célula de tempestade isolada nem o efeito exato do relevo. Para o que está acontecendo **agora**, use radar (RainViewer, Windy).
- Ambiente favorável a tornado não quer dizer que haverá tornado. Serve como sinal para ficar de olho.
- Dias passados mostram o que os modelos simularam, não o que foi medido.
- **Não substitui a Defesa Civil nem o INMET.**

## Créditos

Dados de previsão: [Open-Meteo](https://open-meteo.com) (licença CC BY 4.0), que reúne os modelos do ECMWF, NOAA (GFS), DWD (ICON), Environment Canada (GEM) e Met Office (UKMO).
