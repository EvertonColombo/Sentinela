# Sentinela do Tempo

Painel web de tempo severo para qualquer ponto do Brasil: vento, chuva, raios, tempestades e ciclones.
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

- **Céu do título com o tempo de agora:** sol, nuvens, chuva, noite estrelada e relâmpagos só quando há trovoada prevista, com a temperatura e a condição no canto.
- **Neste dia, desde 1940:** como costuma ser a data de hoje no lugar escolhido. Mostra a porcentagem de dias com sol, sol entre nuvens, nublado e chuva, a máxima de cada ano desde 1940 com a normal de 1991 a 2020 (mediana das máximas de 3 dias antes a 3 dias depois da data) e a previsão de hoje marcadas, os recordes de calor e de frio, quanto as máximas recentes subiram em relação às antigas, e a maior chuva e a maior rajada nessa data. Os dados são da **reanálise ERA5**: o que aconteceu, reconstruído com observações e um modelo, e **não previsão**. Carrega depois do resto e fica guardado no navegador até o dia seguinte.
- **Faixa de risco:**
  - **No topo, o aviso oficial do INMET** para o ponto: perigo potencial, perigo ou grande perigo, com o tipo e o horário. Os avisos repetidos são juntados, e a lista completa fica recolhida. Se o INMET não puder ser consultado, o topo mostra o nível dos modelos e avisa isso.
  - **Logo abaixo, o que os modelos mostram**, para análise própria: o que (tempestade, raios, vento, chuva, ciclone), quando, por quanto tempo e quantos modelos concordam. Trovoada prevista por 2 ou mais modelos na mesma hora conta como atenção.
  - **A matriz de concordância** tem uma linha por modelo e uma coluna por hora das próximas 72 horas, com uma aba para cada risco, inclusive raios (em violeta). No topo dela fica a linha "INMET", colorida pelo nível do aviso em cada hora. A altura da faixa não muda ao trocar de aba. O aviso de trovoada no navegador fica ali mesmo.
- **Hoje:** quatro cartões visuais. Temperatura; chuva hora a hora, com a chance do dia e a cor de cada coluna mostrando quantos modelos preveem chuva naquela hora; vento em bússola; arco do sol com nascer e pôr.
- **Próximos 10 dias:** ícone e chance de chuva de cada dia, e três visões:
  - **Temperatura:** a curva do dia, com área colorida pela temperatura (com escala), mínima em azul, máxima em laranja e o ponto de agora.
  - **Chuva:** 24 colunas por dia, mostrando em que horas chove. A cor diz quantos modelos concordam: cinza quando é só um, azul cada vez mais forte de 2 a 5.
  - **Vento:** rajada máxima com as marcas de atenção, alerta e severo.
- **Medidores de risco:** número, significado ("folhas se mexem", "telhas voam"), zonas de nível, um ponto por modelo (o que destoa aparece em rosa) e o horário do pico.
- **Hora a hora:** curvas de rajada, chuva em 24 horas, energia para tempestades e queda de pressão, que mudam de cor ao cruzar cada nível. Em "Ver cada modelo", cada modelo aparece como uma linha na sua cor.
- **Comparar dias:** ontem, hoje e amanhã em 3D, hora a hora, para temperatura, rajadas ou chuva. Embaixo, três blocos de barras (temperatura, rajada máxima e chuva), uma barra por dia na cor do dia, com a diferença em relação ao dia escolhido.
- **Ingredientes de tempestade:** energia (CAPE) e cisalhamento ao longo das horas, e quando aparecem juntos.
- **E o INMET?** Cartão explicando que o INMET roda o COSMO, desenvolvido por um consórcio de serviços meteorológicos de vários países. É um modelo de área limitada: calcula só uma região, com quadrados de 7 a 2,8 km. Ele não está disponível no Open-Meteo, e que os avisos oficiais são feitos por meteorologistas a partir de vários modelos e observações.
- **Os 5 modelos:** a origem de cada um (instituição, tamanho do quadrado em 3D sobre o relevo, última rodada) e o que cada um prevê: uma régua por grandeza, com as zonas de atenção, alerta e severo, um ponto colorido por modelo e, ao lado, o valor que pelo menos 2 modelos atingem. O modelo que destoa fica com contorno rosa. A tabela com os números exatos continua disponível, recolhida. Inclui os 31 cenários do ensemble GFS para rajada forte e chuva volumosa.
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
