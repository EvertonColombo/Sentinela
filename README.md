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

O painel compara **5 modelos globais**: ECMWF, GFS, ICON, GEM e UKMO. A mesma regra vale em toda a página.

- **Riscos** (vento, chuva, tempestade, queda de pressão): o valor é o maior que **pelo menos 2 modelos** atingem **na mesma hora**. Um modelo sozinho, exagerando para cima ou para baixo, não muda o número nem o nível.
- **Condições gerais** (temperatura, pressão, Comparar dias): o valor é a **mediana** dos modelos, tirando o modelo que destoar muito dos outros.
- **Tempestade:** energia (CAPE) e vento em altura (cisalhamento) vêm sempre do mesmo modelo e da mesma hora. Só há risco quando os dois aparecem juntos.
- **Modelo que destoa:** nos medidores, cada modelo é um ponto. O que se afasta muito dos outros aparece em rosa e fica fora da conta.

Níveis: **sem risco**, **atenção**, **alerta** e **severo**.

| Categoria | Atenção | Alerta | Severo |
|---|---|---|---|
| Rajada (km/h) | 60 | 80 | 100 |
| Chuva em 1 hora (mm) | 20 | 30 | 50 |
| Chuva em 24 horas (mm) | 50 | 80 | 120 |
| Queda de pressão em 24 horas (hPa) | 8 | 12 | 18 |
| Tempestade (CAPE J/kg + cisalhamento m/s) | 1.000 + 15 | 1.500 + 20 | 2.500 + 25 |

Os limites podem ser editados no bloco `CFG`, no início do `<script>` do `index.html`.

## O que tem na página

- **Faixa de status:** o nível geral, o que (vento, chuva, tempestade, ciclone), quando, por quanto tempo e quantos modelos concordam. Inclui a matriz de concordância: uma linha por modelo e uma coluna por hora das próximas 72 horas.
- **Raios e trovoadas:** quantos modelos preveem trovoada em cada uma das próximas 24 horas, com aviso opcional no navegador enquanto a página estiver aberta. É previsão, não detecção de raios.
- **Medidores de risco:** número, significado ("folhas se mexem", "telhas voam"), zonas de nível e o pico de cada modelo.
- **Hora a hora:** curvas de rajada, chuva em 24 horas, energia para tempestades e queda de pressão, que mudam de cor ao cruzar cada nível.
- **Comparar dias:** ontem, hoje e amanhã em 3D, para temperatura, rajadas ou chuva.
- **O que cada modelo prevê:** tabela por modelo, gráficos e a origem de cada modelo (instituição, resolução, última rodada).
- **Probabilidades:** os 31 cenários do ensemble GFS para rajada forte e chuva volumosa.
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
