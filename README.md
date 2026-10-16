# T2 — Teoria dos Grafos

Implementação e análise de algoritmos de grafos, com foco na busca em profundidade (DFS), detecção de ciclos e análise de conectividade.

## Sobre o projeto

Este repositório reúne os materiais desenvolvidos para o Trabalho 2 (T2) da disciplina de Teoria dos Grafos.

O projeto aborda problemas de análise de grafos por meio de algoritmos de busca, incluindo a identificação de ciclos em grafos dirigidos e o estudo de componentes conexas em grafos não dirigidos.

## Problemas abordados

### Marco 1 — Detecção de ciclos em grafos dirigidos

Baseado no problema [Round Trip II — CSES](https://cses.fi/problemset/task/1678/).

O objetivo é identificar um ciclo dirigido em um grafo que representa cidades e voos. Caso exista um ciclo, o algoritmo deve reconstruir e apresentar o percurso encontrado. Caso contrário, deve indicar que não existe solução.

**Estratégia utilizada:**
- Representação do grafo por lista de adjacência.
- Busca em profundidade (DFS).
- Controle de estados dos vértices.
- Registro dos predecessores para reconstrução do ciclo.

**Complexidade esperada:**
- Tempo: \(O(n+m)\)
- Espaço: \(O(n+m)\)

Em que \(n\) representa o número de vértices e \(m\) o número de arestas.

### Marco 2 — Análise de grafos não dirigidos

Estudo da estrutura de grafos não dirigidos por meio de busca em profundidade.

O objetivo é percorrer os vértices e identificar os grupos de vértices conectados entre si, utilizando a análise de componentes conexas.

**Estratégia utilizada:**
- Representação por lista de adjacência.
- Busca em profundidade (DFS).
- Marcação de vértices visitados.
- Identificação de componentes conexas.

## Estrutura do repositório

```text
T2/
├── acompanhamento/  # Documentação dos marcos do trabalho
├── apresentacao/    # Materiais de apresentação
├── evidencias/      # Registros e evidências do desenvolvimento
├── src/             # Código-fonte
└── README.md        # Documentação principal
```

## Tecnologias e conceitos

- Python
- Teoria dos Grafos
- Busca em Profundidade (DFS)
- Lista de adjacência
- Detecção e reconstrução de ciclos
- Componentes conexas
- Análise de complexidade de algoritmos

## Referências

- [CSES Problem Set — Round Trip II](https://cses.fi/problemset/task/1678/)
- Materiais e conteúdos da disciplina de Teoria dos Grafos.

## Autor

**Antônio Eduardo Silva do Nascimento**

Repositório acadêmico destinado ao desenvolvimento e à documentação do Trabalho 2 de Teoria dos Grafos.
