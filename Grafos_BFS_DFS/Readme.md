# 📘 Caderno do Estudante

## 🌐 Conectividade em Rede Simulada com Grafos, BFS e DFS

**Disciplina:** Estrutura de Dados II — Ciência da Computação

**Instituição:** UDF

**Estudante:** Emmanuel Ferreira Gomes

> ⚠️ **Aviso:** Esta atividade é uma simulação computacional didática. Não representa um modelo de navegação aeronáutica e não deve ser utilizada para orientar voos reais. A notícia sobre a pane de GPS no Aeroporto de Brasília (G1, 22/09/2026) serve apenas como motivação. As cidades, conexões e bloqueios utilizados são fictícios.

## 🧭 1. O desafio

Uma falha nos recursos de navegação pode exigir o planejamento de rotas alternativas. Nesta atividade, foi criada uma rede fictícia de conexões entre cidades, representada por um grafo, para estudar a conectividade e comparar os algoritmos BFS e DFS.

**Pergunta principal:**

Quantas conexões podem ser bloqueadas antes que deixe de existir um caminho entre Brasília e Curitiba?

**Objetivo:** implementar os algoritmos BFS e DFS em Python, simular bloqueios de conexões e comparar os resultados sem modificar o grafo original.

## 📚 2. Conceitos essenciais

- 🔵 **Vértice (ou nó):** representa uma cidade.
- 🔗 **Aresta:** representa uma conexão entre duas cidades.
- 🛤️ **Caminho:** sequência de cidades em que cada par consecutivo está conectado.
- 🔍 **BFS:** busca em largura, que encontra o menor caminho em quantidade de arestas em grafos não ponderados.
- 🔎 **DFS:** busca em profundidade, que explora um caminho até onde for possível antes de retornar.
- ⚠️ **Conectividade:** indica se existe um caminho entre dois vértices.

Neste projeto, as conexões são recíprocas. Portanto, utilizamos um grafo não direcionado e não ponderado.

## 🗺️ 3. Conheça as cidades

O grafo utilizado possui 10 cidades e 14 conexões únicas.

| Cidade | Conexões |
| --- | --- |
| Brasília | Goiânia, Belo Horizonte, Uberlândia |
| Goiânia | Brasília, Uberlândia, Campo Grande |
| Uberlândia | Brasília, Goiânia, Belo Horizonte, Campinas |
| Belo Horizonte | Brasília, Uberlândia, Rio de Janeiro |
| Rio de Janeiro | Belo Horizonte, São Paulo |
| São Paulo | Rio de Janeiro, Campinas, Campo Grande, Curitiba |
| Campinas | Uberlândia, São Paulo |
| Campo Grande | Goiânia, São Paulo, Curitiba |
| Curitiba | São Paulo, Campo Grande, Florianópolis |
| Florianópolis | Curitiba |

### 📊 3.1. Resumo do grafo

- **Vértices:** 10 cidades.
- **Arestas:** 14 conexões únicas.
- **Registros de vizinhança:** 28, pois cada conexão aparece nos dois sentidos.
- **Representação:** lista de adjacência em Python.
- **Tipo:** grafo não direcionado e não ponderado.

### ✏️ 3.2. Desenho do grafo original

![Diagrama do grafo](diagrama_grafo.png)

## 💻 4. Passo a passo: como executar

**Requisitos:** Python 3.8 ou superior. A biblioteca `matplotlib` é necessária apenas para gerar o diagrama.

Digite os comandos abaixo no terminal, dentro da pasta do projeto:

```bash
# Executar o programa
python solucao_grafos.py

# Executar os testes automatizados
python solucao_grafos.py --testes

# Gerar o diagrama
python gerar_diagrama.py
```

### 🧩 4.1. Menu interativo

| Opção | Função |
| --- | --- |
| `1` | Buscar caminho entre cidades |
| `2` | Bloquear uma conexão |
| `3` | Limpar os bloqueios |
| `4` | Listar conexões |
| `0` | Sair |

O programa aceita nomes com ou sem acentos e não diferencia letras maiúsculas de minúsculas.

Os bloqueios são aplicados somente durante a simulação. O grafo original permanece preservado, pois a função `bloquear_conexao()` trabalha com uma cópia.

## ⚙️ 5. Passo a passo: entenda os algoritmos

### 🔵 5.1. BFS — Busca em largura

O BFS utiliza uma fila FIFO para visitar os vértices por níveis. Em grafos não ponderados, encontra o caminho com o menor número de arestas.

### 🔎 5.2. DFS — Busca em profundidade

O DFS utiliza uma pilha LIFO para explorar um caminho até onde for possível antes de retornar. Ele encontra um caminho possível, mas não garante que seja o menor.

### 🐍 5.3. Principais funções do código

| Função | Descrição |
| --- | --- |
| `bfs(rede, origem, destino)` | Realiza a busca em largura. |
| `dfs(rede, origem, destino)` | Realiza a busca em profundidade. |
| `bloquear_conexao(rede, a, b)` | Remove uma conexão nos dois sentidos, trabalhando em uma cópia. |
| `comparar(...)` | Executa BFS e DFS e compara os caminhos encontrados. |
| `resolver_cidade(...)` | Valida os nomes das cidades. |

Quando não existe caminho entre a origem e o destino, os algoritmos retornam `None`, apresentado como **SEM CAMINHO**.

## 📊 6. Passo a passo: resultados dos testes

**Origem:** Brasília

**Destino:** Curitiba

Os resultados completos estão registrados nos arquivos `evidencia_testes.txt` e `evidencia_interativa.txt`.

### 🛤️ 6.1. Comparação dos cenários

| Cenário | BFS | DFS |
| --- | --- | --- |
| **A — Rede completa** | Brasília → Goiânia → Campo Grande → Curitiba (**3 arestas**) | Brasília → Goiânia → Uberlândia → Belo Horizonte → Rio de Janeiro → São Paulo → Campo Grande → Curitiba (**7 arestas**) |
| **B — Bloqueio Uberlândia–Campinas** | Mesmo caminho do cenário A (**3 arestas**) | Mesmo caminho do cenário A (**7 arestas**) |
| **C — Bloqueio Goiânia–Campo Grande** | Brasília → Belo Horizonte → Rio de Janeiro → São Paulo → Curitiba (**4 arestas**) | Mesmo caminho do cenário A (**7 arestas**) |
| **C2 — Bloqueio Rio de Janeiro–São Paulo** | Sem caminho | Sem caminho |
| **D — Inversão dos vizinhos de Brasília** | **3 arestas** | Brasília → Uberlândia → Goiânia → Campo Grande → São Paulo → Curitiba (**5 arestas**) |

### 🔍 6.2. Análise dos resultados

- **Cenário B:** o bloqueio não altera os caminhos encontrados, mas reduz as alternativas disponíveis na rede.
- **Cenário C:** com o bloqueio entre Goiânia e Campo Grande, o BFS encontra uma rota alternativa pelo Sudeste, aumentando o caminho em uma aresta.
- **Cenário C2:** após três bloqueios, Brasília e Curitiba ficam em componentes desconexas. Por isso, os dois algoritmos indicam que não existe caminho.
- **Cenário D:** a alteração da ordem dos vizinhos modifica o resultado do DFS, que passa de 7 para 5 arestas. O BFS continua encontrando o menor caminho.

**Resposta à pergunta principal:** três bloqueios específicos são suficientes para desconectar Brasília de Curitiba nesta rede simulada. Isso demonstra que não importa apenas a quantidade de conexões bloqueadas, mas também quais conexões são escolhidas.

## 🔄 7. Compare BFS e DFS

| Característica | BFS | DFS |
| --- | --- | --- |
| Estrutura utilizada | Fila (FIFO) | Pilha (LIFO) |
| Resultado | Menor caminho em arestas | Um caminho possível |
| Detecta ausência de caminho | Sim | Sim |
| Complexidade | O(V + E) | O(V + E) |
| Principal utilização | Encontrar o menor caminho | Verificar conectividade |

Os dois algoritmos conseguem verificar se existe conexão entre dois vértices. A principal diferença é que o BFS garante o menor número de arestas em um grafo não ponderado, enquanto o DFS depende da ordem de exploração.

## ⚠️ 8. Limitações do projeto

- O grafo é fictício e representa uma rede pequena, diferente de uma rede real de navegação.
- As conexões não possuem pesos. Portanto, o menor caminho representa a menor quantidade de arestas, e não a menor distância ou tempo.
- Todas as conexões são consideradas bidirecionais.
- Os bloqueios são apenas hipóteses utilizadas na simulação.
- O código armazena os caminhos durante a busca, o que facilita a implementação, mas pode consumir mais memória.
- O modelo não considera fatores reais de segurança, clima, regulamentação ou operação aeronáutica.

Para trabalhar com distâncias e custos, seria necessário utilizar arestas ponderadas e algoritmos como Dijkstra.

## 🧠 9. Questões de reflexão

**1. Qual a diferença entre um grafo e uma árvore binária?**

Um grafo pode possuir ciclos, diferentes caminhos e vários vizinhos para um mesmo vértice. Já uma árvore binária não possui ciclos e cada nó pode ter, no máximo, dois filhos.

**2. Quando o BFS encontra o menor caminho?**

Quando trabalha com um grafo não ponderado, pois considera o número de arestas percorridas. Isso não significa necessariamente menor distância ou tempo.

**3. Por que o DFS pode encontrar caminhos diferentes?**

Porque ele explora um caminho até onde consegue antes de voltar. O resultado depende da ordem dos vizinhos, como demonstrado no cenário D.

**4. O que acontece se não houver um conjunto de visitados?**

Em grafos com ciclos, o algoritmo pode visitar os mesmos vértices repetidamente, causando um laço infinito ou aumentando o processamento.

**5. Quais informações seriam necessárias para calcular tempo ou custo?**

Seria necessário adicionar pesos às conexões, considerando distância, duração, custo, capacidade e outras condições que possam influenciar o percurso.

**6. Por que o modelo não pode ser utilizado em voos reais?**

Porque é uma simulação simplificada e não considera as exigências de segurança, regulamentação e condições reais da aviação.

## 📦 10. Entrega

Entregue os seguintes arquivos:

- 🐍 `solucao_grafos.py` — código-fonte.
- 📘 `README.md` — documentação do projeto.
- 🖼️ `diagrama_grafo.png` — imagem do grafo.
- ⚙️ `gerar_diagrama.py` — código para gerar o diagrama.
- 📄 `evidencia_testes.txt` — resultados dos testes automatizados.
- 📄 `evidencia_interativa.txt` — resultados da execução interativa.
- 📝 `roteiro_apresentacao.md` — roteiro da apresentação de 5 minutos.

## 📚 11. Referências

GOMES, Paulo César Rodacki. **Grafos: conceitos fundamentais, algoritmos e aplicações**. Blumenau: Editora IFC, 2022. ISBN 978-65-88089-12-5.

G1 DISTRITO FEDERAL. **Pane em GPS afeta voos da Azul no Aeroporto de Brasília; passageiros reclamam de atrasos**. 22 set. 2026. Disponível em: [G1 — Distrito Federal](https://g1.globo.com/df/distrito-federal/noticia/2026/09/22/pane-em-gps-afeta-voos-da-azul-no-aeroporto-de-brasilia-passageiros-reclamam-de-atrasos.ghtml).
