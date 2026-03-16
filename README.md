# J903 – Inteligência Artificial

## 🧠 Contexto
Projeto de **planejamento de rotas para drone agrícola** em uma fazenda modelada como um grid 2D. O campo conta com duas regiões geométricas distintas:

- 🌐 **Região retangular (azul)**
- 🌕 **Região circular (amarela)**

O objetivo é planejar um caminho entre essas regiões respeitando a **restrição de energia (bateria limitada)** do drone.

---

## ✅ O que já está implementado
O notebook `Howork/activity.ipynb` (e o módulo auxiliar `test.py`) já contém:

- Geração de um **grid 2D** com:
  - Região retangular (com obstáculos aleatórios)
  - Região circular (com obstáculos aleatórios)
  - Start dentro do retângulo e Goal dentro do círculo
- Visualização da fazenda com Matplotlib (cores distintas + coloca estrelas para start/goal)
- Implementação de 3 algoritmos de busca com limite de energia (`E_max`):
  - **A\\*** (A-Star)
  - **IDA\\*** (Iterative Deepening A-Star)
  - **RBFS** (Recursive Best-First Search)
- Cada algoritmo retorna métricas básicas de desempenho:
  - Nós expandidos
  - Tempo de execução (usando `time.perf_counter()`)
  - Energia consumida (quando aplicável)

---

## ▶️ Como executar
### 1) Ativar o ambiente virtual
```bash
source .venv/bin/activate
```

### 2) Abrir o notebook (recomendado)
Execute no terminal:
```bash
code Howork/activity.ipynb
```

### 3) Rodar a célula final do notebook
A célula final já executa:
- geração do grid
- execução dos 3 algoritmos (A*, IDA*, RBFS)
- plot do caminho encontrado

> ✅ Caso prefira, também é possível importar e executar `generate_field` + os algoritmos diretamente a partir de `test.py`.

---

## 📌 O que falta/pendências (para completar os entregáveis do PDF)
### ✔️ Já feito
- Algoritmos A*, IDA* e RBFS
- Geração de grid com retângulo/círculo e obstáculos
- Lógica de limitação de energia (E_max)

### ❗ Ainda não feito (recomendações)
- **Medição de consumo de memória** (requisito do enunciado):
  - Usar `tracemalloc` ou `memory_profiler` para medir memória alocada por cada algoritmo
- **Matriz de performance comparativa** (tabela com resultados):
  - Deve listar: algoritmo, geometria inicial/final, tempo, memória e energia
- **Discussão técnica e conclusão** (mínimo 1 página):
  - Explicar trade-offs entre tempo e memória
  - Justificar score global (α, β) e indicar o melhor algoritmo por cenário

---

## 📁 Estrutura de arquivos
- `Howork/activity.ipynb` – Notebook principal com código, visualizações e execução.
- `test.py` – Módulo com funções utilitárias (geração de grid, plot, algoritmos).
- `J903_Homework_Agro_Robos_Atividade.pdf` – Enunciado do trabalho.

---

## 🔧 Sugestões de melhoria rápida
- Adicionar uma função `evaluate_algorithms()` que rode todos os algoritmos sobre a mesma instância de grid e coleta as métricas em um `pandas.DataFrame`.
- Salvar a tabela de performance em CSV/Excel (`pandas.DataFrame.to_csv()`) para facilitar análise.
- Implementar variações de heurísticas (ex: euclidiana, diagonal) e pautar a comparação.

---

Se quiser, posso também gerar automaticamente a tabela de performance (tempo + memória) usando `tracemalloc`, e construir uma seção de comparação pronta para incluir em um relatório.