# Relatório Técnico Operacional: Visita de Campo à Escola Rural (SL Rota 01)

Este relatório apresenta a análise espacial e cronológica dos dados extraídos do arquivo KML **`SL_Rota_01.kml`** (gravado via aplicativo *Geo Tracker* em 20 de maio de 2025). O arquivo descreve a trajetória e os pontos de controle de uma visita técnica realizada pela equipe de Geotecnologia da Semed Manaus.

---

## 📊 Painel Analítico da Rota

A imagem a seguir apresenta a consolidação visual do trajeto, perfil de velocidade e principais indicadores de telemetria da rota analisada:

![Painel de Análise da Rota 01](Rota 01.png)

---

## 📈 Estatísticas Resumidas do Trajeto

Com base no cruzamento das tags `<gx:Track>` (geometria da linha) e `<Placemark>` (pontos de parada), consolidamos as seguintes métricas fundamentais:

| Métrica | Valor Extraído | Observação Técnica |
| :--- | :--- | :--- |
| **Nome da Rota** | SL Rota 01 | Identificador do trajeto fluvial/terrestre no Puraquequara. |
| **Data do Registro** | 20 de maio de 2025 | Visita realizada no período matutino. |
| **Horário de Início** | 10:19:12 UTC | Saída registrada no ponto inicial (`P inicial`). |
| **Horário de Término** | 11:19:07 UTC | Retorno e fechamento do track log (`P final`). |
| **Duração Total** | 59 min e 55 seg | Aproximadamente 1 hora de operação em campo. |
| **Distância Percorrida** | 19,14 km | Extensão total do deslocamento de ida e volta. |
| **Velocidade Média** | ~19,17 km/h | Compatível com navegação fluvial por motor de popa tipo rabeta/voadeira. |
| **Altitude Mínima** | 6,8 metros | Registrada próximo ao nível máximo da água da calha fluvial. |
| **Altitude Máxima** | 12,0 metros | Vértice em terra firme (cabeceiras/comunidade). |

---

## ⏱️ Cronologia de Visita e Pontos de Controle

A tabela a seguir detalha a sequência cronológica dos eventos registrados na rota, demonstrando como os marcadores de campo coletados no Geo Tracker (como portos e paradas de alunos) são interpretados espacial e temporalmente:

| Evento | Horário | Distância do Início | Altitude | Descrição / Observação Técnica |
| :--- | :--- | :--- | :--- | :--- |
| **Início da Viagem** | 10:19:12 | 0,00 km | 8,87 m | Ponto de partida registrado da embarcação fluvial (`P inicial`). |
| **P01 a P09** | 10:20 a 10:59 | 0,08 a 13,79 km | 7,5 a 12,0 m | Pontos de rastreamento contínuo em trânsito com velocidades de cruzeiro de até 35 km/h. |
| **Porto: P Escola** | 11:02:44 | 14,36 km | 9,56 m | **Porto Ativo**. Ponto georreferenciado de desembarque coletado em campo. Cruzamento de telemetria registrou tempo de parada de **~2 min** para embarque/desembarque de alunos e atracação da embarcação. |
| **Visita: E.M. Santa Luzia** | 11:02:44 | 14,36 km | 9,56 m | **Associação Espacial Automática (< 150m)**. Ponto de visita técnica resolvido para a escola da base de dados. A instituição atende a 120 alunos locais. |
| **P10 a P11** | 11:05 a 11:12 | 14,39 a 16,93 km | 6,8 a 11,5 m | Trajeto fluvial de retorno técnico. |
| **Fim da Viagem** | 11:19:07 | 19,14 km | 11,99 m | Encerramento do track log no porto de destino final (`P final`). |

---

## ⚓ Integração de Portos de Embarque e Desembarque (Geo Tracker)

No novo fluxo de telemetria automatizada do WebGIS, cada ponto georreferenciado marcado em campo como porto de embarque/desembarque ou parada de aluno (tags `<Placemark>` do tipo ponto contidas no KML/KMZ) é cruzado dinamicamente com os dados do track log de navegação:
1. **Atribuição Temporal**: Como pontos isolados de KML geralmente não contêm timestamps de passagem, o sistema calcula a menor distância espacial entre a coordenada do porto e a rota percorrida, identificando o vértice da rota mais próximo e estimando o horário exato de chegada.
2. **Tempo de Atracação/Parada**: Se a telemetria acusar velocidade próxima de zero (< 2.0 km/h) nas proximidades do porto por mais de 1 minuto, o sistema extrai a duração exata desta parada e anexa a informação de tempo de atracação diretamente na descrição do porto no relatório.
3. **Resolução contra Duplicidade**: Para manter a clareza do relatório impresso, paradas técnicas genéricas detectadas por sensores de velocidade são omitidas quando ocorrem próximas a portos devidamente nomeados em campo, evitando linhas duplicadas e redundantes na tabela de eventos.

---

## 💡 Modelo de Implementação Técnica no WebGIS

Para automatizar este tipo de análise diretamente no navegador do usuário ao importar qualquer KML ou KMZ contendo trajetos, propomos o seguinte **Modelo de Implementação de Relatório Automático** no aplicativo WebGIS:

### Fluxo de Funcionamento Proposto

```mermaid
graph TD
    KMLInput[Usuário importa KML/KMZ] --> Parser[Parser Customizado de XML]
    Parser --> ExtractTracks[Extrai Linhas e Coordenadas gx:Track]
    Parser --> ExtractPoints[Extrai Pontos e ExtendedData de Placemarks]
    
    ExtractTracks --> CalcMets[Calcula Distância Total e Velocidade Média]
    ExtractPoints --> CalcPrio[Identifica Pontos Chave como Escolas e Paradas]
    
    CalcMets --> RenderReport[Gera Painel de Relatório na Sidebar]
    CalcPrio --> RenderReport
    
    RenderReport --> Chart[Plota Gráfico de Perfil de Altitude/Velocidade com Chart.js]
    RenderReport --> ExportPDF[Botão para Gerar PDF do Relatório com jsPDF]
```

### Componente HTML para Acoplamento na Sidebar
O módulo de KML na barra lateral pode ser estendido para exibir uma seção de **"Análise de Desempenho de Rota"** toda vez que uma rota de GPS for identificada no arquivo importado:

```html
<div class="route-analysis-card" id="route-analysis-card" style="display:none; margin-top:10px; padding:12px; background:rgba(0,0,0,0.3); border:1px solid var(--border-amber); border-radius:8px;">
  <div style="font-weight:700; color:var(--amber-light); font-size:11px; text-transform:uppercase; margin-bottom:8px;">Relatório de Viagem</div>
  <div style="font-size:12px; display:flex; justify-content:space-between; margin-bottom:4px;">
    <span>Distância Total:</span> <b id="route-dist-val">—</b>
  </div>
  <div style="font-size:12px; display:flex; justify-content:space-between; margin-bottom:4px;">
    <span>Duração da Rota:</span> <b id="route-time-val">—</b>
  </div>
  <div style="font-size:12px; display:flex; justify-content:space-between; margin-bottom:8px;">
    <span>Pontos de Parada:</span> <b id="route-stops-val">0</b>
  </div>
  <button class="btn sm" id="btn-show-chart" style="padding:6px; font-size:10px;">Visualizar Gráficos</button>
</div>
```

Este modelo permite que qualquer arquivo de log de GPS coletado em campo pelos técnicos seja imediatamente traduzido em métricas de produtividade administrativa e logística diretamente no mapa.
