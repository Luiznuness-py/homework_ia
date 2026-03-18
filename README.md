# Fazenda Inteligente - Algoritmos de Busca de Caminho

## 📋 Descrição do Projeto

Este projeto implementa e compara três algoritmos clássicos de inteligência artificial para busca de caminho em um grid com obstáculos: **A\***, **IDA\*** e **RBFS**. O objetivo é encontrar o caminho mais eficiente para um drone navegando em uma "Fazenda Inteligente" contendo regiões retangulares e circulares com obstáculos aleatórios.

## 🎯 Objetivo

Comparar o desempenho de três algoritmos de busca heurística em termos de:
- **Tempo de execução** (ms)
- **Consumo de memória** (KB)
- **Energia consumida** (número de movimentos)

Os algoritmos são testados em grids de tamanhos crescentes (35x35 até 55x55) para análise de complexidade.

## 🏗️ Arquitetura

### Componentes Principais

#### 1. **Geração de Campo** (`generate_field`)
Cria um grid com obstáculos distribuídos em duas regiões:
- **Região Retangular**: Obstáculos com densidade aleatória (10%-40%)
- **Região Circular**: Obstáculos com densidade aleatória (5%-30%)

**Parâmetros aleatórios:**
- Posição e dimensões do retângulo
- Centro, raio e densidade da circunferência
- Posições de início e objetivo (garantindo células livres)

```python
grid, start, goal, region_map, params = generate_field(n_rows=35, n_cols=35)
```

#### 2. **Visualização** (`plot_field`)
Renderiza o mapa usando Matplotlib com códigos de cores:
- 🟣 Roxo: Fundo
- 🔵 Azul: Região Retangular
- 🟡 Amarelo: Região Circular
- 🔷 Ciano: Obstáculos
- ⭐ Verde (Start) e Vermelho (Goal)
- ⚪ Branco: Caminho encontrado

#### 3. **Funções Auxiliares**
- `in_bounds()`: Verifica se posição está dentro dos limites
- `passable()`: Verifica se célula não é obstáculo
- `neighbors()`: Retorna vizinhos passáveis (cima, baixo, esquerda, direita)
- `cost()`: Retorna custo de movimento (fixo = 1.0)
- `h_manhattan()`: Heurística de distância de Manhattan
- `reconstruct_path()`: Reconstrói caminho a partir do dicionário de precedentes

## 🔍 Algoritmos Implementados

### 1. **A\* (A-Star)**
Algoritmo de busca informada que combina o custo real com uma heurística.

**Características:**
- Usa fila de prioridade (heap)
- Avalia f(n) = g(n) + h(n)
  - `g(n)`: Custo real até o nó atual
  - `h(n)`: Heurística (Manhattan) até o objetivo
- Completo e ótimo (encontra o melhor caminho)
- Melhor para grids pequenos/médios

**Complexidade:**
- Tempo: O(b^d) onde b = fator de ramificação
- Espaço: O(b^d) - mantém todos os nós na memória

### 2. **IDA\* (Iterative Deepening A\*)**
Algoritmo de busca em profundidade com limite crescente.

**Características:**
- Usa DFS recursiva com limite de custo f(n)
- Reitera aumentando o limite até encontrar solução
- Completo e ótimo
- **Vantagem**: Usa menos memória que A\*

**Complexidade:**
- Tempo: O(b^d) - similar ao A\*, com overhead de re-visitação
- Espaço: O(d) - apenas ramo da árvore

### 3. **RBFS (Recursive Best-First Search)**
Busca best-first recursiva com memória limitada.

**Características:**
- Explora o melhor nó disponível
- Guarda o segundo melhor como alternativa (backtracking)
- Completo e ótimo, com uso de memória controlado
- Bom balanço entre tempo e memória

**Complexidade:**
- Tempo: O(b^d)
- Espaço: O(d) com possibilidade de re-exploração

## 📊 Métricas de Desempenho

O projeto mede e compara:

### Por Teste:
| Métrica | Descrição |
|---------|-----------|
| **Tamanho** | Dimensões do grid (n×n) |
| **Geometria** | Regiões onde start/goal estão |
| **Tempo (ms)** | Tempo de execução do algoritmo |
| **Memória (KB)** | Pico de memória consumida |
| **Energia** | Número de movimentos do drone |

### Agregado:
- **Médias** por algoritmo
- **Score de Eficiência**: α×tempo_normalizado + β×memória_normalizada
  - α = 0.6 (peso para tempo - drone precisa ser rápido)
  - β = 0.4 (peso para memória)

## 🚀 Uso

### Executar Geração e Visualização
```python
grid, start, goal, region_map, params = generate_field(n_rows=35, n_cols=35)
plot_field(grid, start, goal, region_map)
```

### Executar um Algoritmo Específico
```python
# A*
path, metrics = astar(grid, start, goal, E_max=100)
plot_field(grid, start, goal, region_map, path)

# IDA*
path, metrics = ida_star(grid, start, goal, E_max=100)

# RBFS
path, metrics = rbfs(grid, start, goal, E_max=100)
```

### Medir Desempenho
```python
tempo_ms, memoria_kb, energia = desempenho(
    algoritmo=astar,
    grid=grid,
    start=start,
    goal=goal
)
```

### Executar Coleta Completa de Resultados
O notebook executa automaticamente os 3 algoritmos em 5 grids diferentes, gerando:
- Tabela de resultados por execução
- Médias gerais
- Score de eficiência global com ranking

## 📦 Dependências

```
numpy
pandas
matplotlib
```

## 🔧 Parâmetros Configuráveis

- **E_max**: Limite de energia máxima (células de movimento permitidas)
- **n_rows, n_cols**: Dimensões do grid
- **seed**: Semente para reprodução de resultados
- **obstacle_ratio_rect**: Densidade de obstáculos no retângulo
- **obstacle_ratio_circ**: Densidade de obstáculos no círculo

## 📈 Esperado

Baseado na teoria de algoritmos de busca:

1. **A\*** tende a ser **mais rápido** mas consome **mais memória**
2. **IDA\*** usa **menos memória** mas pode ser **mais lento** por re-visitação
3. **RBFS** oferece um **balanço** entre tempo e memória

A escolha depende do contexto:
- **Dispositivos com pouca memória**: Preferir IDA\* ou RBFS
- **Aplicações em tempo real**: Preferir A\*
- **Drones com bateria limitada**: Analisar energia vs. tempo

## 📝 Notas Implementação

- Movimentos permitidos: Cima, Baixo, Esquerda, Direita (4-conectividade)
- Heurística: Distância de Manhattan
- Ordem de vizinhos: A ordem afeta a busca mas não altera otimalidade
- Limite de energia: Descarta caminhos que excedem E_max

## 👨‍💻 Autor

Atividade de Inteligência Artificial - Sistemas de Busca

---

**Última atualização:** Março de 2026
