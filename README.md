# WebGIS Operacional Rural

Aplicativo web cartográfico voltado ao **apoio operacional de visitas técnicas em escolas rurais**, desenvolvido para a **Secretaria Municipal de Educação de Manaus — Setor de Geotecnologia**.

Permite que a equipe visualize o território, registre evidências em campo e apoie a tomada de decisão — mapa interativo, base de satélite/vetor, camadas vetoriais, coleta de pontos/linhas/polígonos, priorização de escolas, importação e exportação KML, e rastreamento GPS em tempo real.

## 🎯 Funcionalidades

- **Mapa interativo** (Leaflet) com base alternável: satélite (Esri) / vetor escuro (CARTO)
- **85 escolas rurais** carregadas com popup completo (bairro, endereço, alunos, modalidade, acesso)
- **Escolas prioritárias selecionáveis** — marque/desmarque qualquer escola; persiste no navegador
- **Coleta de vetores**: ponto (clique/GPS), linha (distância) e polígono (área) — separados e independentes
- **Descrições editáveis** em todos os popups, com link direto para o Google Maps
- **Tabela de atributos** (modal tela cheia) — visualização e edição de todas as feições, com busca e zoom
- **Importação de KML** e **exportação KML** (pontos, linhas e polígonos)
- **Rastreamento GPS** em tempo real com círculo de precisão
- **Persistência local** (localStorage) — feições, descrições e prioridades sobrevivem ao recarregar
- **Interface escura operacional** — preto grafite, destaques âmbar, cartões em vidro fosco

## 🚀 Como usar

Abra o arquivo `index.html` diretamente no navegador. Não requer instalação nem servidor.

> Requer conexão de internet para carregar os tiles de mapa (CDN). A logo carrega do arquivo local `logo_manaus.png`.

## 📁 Arquivos

| Arquivo | Descrição |
|---|---|
| `index.html` | Aplicativo completo (HTML + CSS + JS autossuficiente) |
| `logo_manaus.png` | Brasão da Prefeitura de Manaus (usado no cabeçalho) |

## 🛠️ Tecnologias

- HTML5, CSS3, JavaScript (vanilla)
- [Leaflet](https://leafletjs.com/) — mapas interativos
- Tiles: Esri World Imagery, CARTO Dark Matter
- Persistência: localStorage

## 🏛️ Origem dos dados

As 85 escolas foram extraídas do GeoPackage `Escolas Rurais.gpkg` (SIRGAS 2000 / EPSG:4674), já com os atributos convertidos e embutidos no `index.html`.

---

**Secretaria Municipal de Educação — Setor de Geotecnologia · Prefeitura de Manaus**
