# 🗺️ Sistema de Rotas para Metrô com Programação Dinâmica

> **Checkpoint 3 — Estrutura de Dados e Algoritmos**  
> Professor: **Andre Marques**

</div>

---

## 👥 Integrantes

| Nome | RM |
|---|---|
| 👩‍💻 Giovana Gaspar Larocca | `564965` |
| 👩‍💻 Giovanna Lins Sayama | `565901` |
| 👩‍💻 Rayssa Luzia Portela Aquino | `562024` |

---

## 📋 Sobre o Projeto

Este projeto implementa um **sistema de otimização de rotas para metrô** em três grandes metrópoles globais, combinando teoria de grafos com programação dinâmica para resolver dois problemas clássicos:

- 🔵 **Caminho mais curto** (menor tempo de viagem)
- 🟠 **Caminho mais longo simples** (sem revisitar estações)

Cada rede de metrô é modelada como um **grafo não-dirigido ponderado**, onde os pesos representam o tempo médio de percurso em minutos entre estações adjacentes — ajustados dinamicamente por fatores de horário de pico.

---

## 🌆 Cidades Modeladas

```
🇨🇳 BEIJING          🇺🇸 SAN FRANCISCO       🇧🇷 SÃO PAULO
─────────────────   ──────────────────────   ──────────────────────
Linhas 1, 2, 4 e 10  Rede BART               Metrô + CPTM
Sihui East           Dublin/Pleasanton        Tucuruvi (L1-Azul)
    ↓                    ↓                        ↓
Xizhimen             Daly City               Capão Redondo (L5-Lilás)
~30 estações         ~40 estações            ~35 estações
```

---

## ⚙️ Como Funciona

### 1️⃣ Modelagem do Grafo

As redes são representadas em uma estrutura de dicionário de adjacência:

```python
grafo = {
    "Estação A": [("Estação B", 3), ("Estação C", 5)],
    "Estação B": [("Estação A", 3), ("Estação D", 2)],
    ...
}
```

- **Vértices (V):** estações de metrô
- **Arestas (E):** conexões entre estações
- **Pesos:** tempo médio de percurso em minutos
- **Complexidade espacial:** `O(V + E)`

---

### 2️⃣ Fatores de Horário

O custo real de cada trecho é ajustado dinamicamente com base no horário de partida:

| Faixa Horária | Fator | Status |
|:---:|:---:|:---|
| 05h – 07h | `× 0.6` | 🟢 **Bônus** — metrô vazio, embarque rápido |
| 07h – 09h | `× 1.5` | 🟡 **Pico manhã** — espera maior |
| 09h – 17h | `× 1.0` | ⚪ **Normal** — sem penalidade |
| 17h – 20h | `× 2.0` | 🔴 **Penalidade** — alto volume, lotação |
| Demais horas | `× 0.8` | 🔵 **Noturno** — metrô tranquilo |

---

### 3️⃣ Algoritmos Implementados

#### 🔵 Caminho Mais Curto — Recursão + Memoização

```python
@functools.lru_cache(maxsize=None)
def menor_custo(atual, destino, hora, visitados: frozenset):
    ...
```

A memoização via `lru_cache` utiliza a tupla `(estação_atual, destino, hora, frozenset(visitados))` como chave de cache. Isso garante que cada **subproblema único seja resolvido apenas uma vez**.

| Abordagem | Complexidade |
|---|---|
| Sem memoização | `O(V!)` — explosão fatorial |
| **Com memoização** | **`O(2^V × V)`** — cada par (estação, visitados) calculado uma vez |

#### 🟠 Caminho Mais Longo — Backtracking

O problema do caminho mais longo simples em grafos gerais é **NP-difícil**. A solução utiliza backtracking com poda de ramos inviáveis — a memoização pura não se aplica aqui, pois o resultado depende do conjunto completo de visitados.

---

### 4️⃣ Análise de Desempenho

O projeto inclui benchmarks automáticos comparando execução **com** e **sem** memoização:

```
📊 Métricas coletadas:
   ├── ⏱️  Tempo de execução (segundos)
   └── 💾  Uso de memória pico (KB)
```

Os gráficos gerados (`desempenho.png`) evidenciam o ganho exponencial da memoização à medida que o grafo cresce.

---

### 5️⃣ Visualização Interativa com Folium

Mapas interativos são gerados para cada cidade:

```
📍 Legenda dos mapas:
   ══ (azul)    Caminho mais curto
   ══ (laranja) Caminho mais longo
   ── (cinza)   Rede completa
   🟢           Estação de origem
   🔴           Estação de destino
```

| Arquivo gerado | Cidade |
|---|---|
| `mapa_sao_paulo.html` | 🇧🇷 São Paulo |
| `mapa_beijing.html` | 🇨🇳 Beijing |
| `mapa_sf.html` | 🇺🇸 San Francisco |

---

## 🚀 Como Executar

### Pré-requisitos

```bash
pip install folium matplotlib numpy
```

### Rodando o Notebook

```bash
jupyter notebook CP3-Dynamic_Programming.ipynb
```

Execute as células **em ordem** (use `Run All` ou `Shift + Enter` em cada célula):

```
[1] Imports e Configurações
[2] Modelagem dos Grafos
[3] Fatores de Horário
[4] Algoritmos (Recursão + Memoização)
[5] Execução e Resultados
[6] Análise de Desempenho
[7] Visualização com Folium
[8] Análise de Complexidade e Conclusões
```

---

## 📂 Estrutura de Arquivos

```
CP3-Dynamic_Programming/
│
├── 📓 CP3-Dynamic_Programming.ipynb   ← Notebook principal
│
├── 🗺️  mapa_sao_paulo.html            ← Mapa interativo SP
├── 🗺️  mapa_beijing.html              ← Mapa interativo Beijing
├── 🗺️  mapa_sf.html                   ← Mapa interativo SF
│
└── 📊 desempenho.png                  ← Gráfico comparativo (gerado)
```

---

## 📚 Tecnologias

| Biblioteca | Uso |
|---|---|
| `functools.lru_cache` | Memoização automática das funções recursivas |
| `tracemalloc` | Medição de uso de memória em tempo real |
| `folium` | Geração de mapas interativos com Leaflet.js |
| `matplotlib` | Gráficos de comparação de desempenho |
| `numpy` | Cálculos e arrays para os gráficos |

---
**FIAP · 2025 · Checkpoint 3 — Programação Dinâmica**

*Giovana Larocca · Giovanna Sayama · Rayssa Aquino*

</div>
