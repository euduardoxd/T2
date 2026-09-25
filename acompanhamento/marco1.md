# Marco 1 — Problema e conhecimento prévio

## 1. Problema

### 1.1 Problema escolhido

O problema escolhido foi o **Round Trip II**, do CSES Problem Set.

O problema apresenta `n` cidades e `m` conexões de voo. Cada conexão é direcionada, ou seja, uma conexão `a b` representa um voo da cidade `a` para a cidade `b`.

O objetivo é encontrar um percurso que:

* comece em uma cidade;
* passe por uma ou mais cidades;
* retorne à cidade inicial;
* não repita as cidades intermediárias.

Caso exista um ciclo, deve ser apresentada a quantidade de cidades e a sequência que forma o percurso.

Caso nenhum ciclo seja encontrado, deve ser apresentada:

```text
IMPOSSIBLE
```

### 1.2 Restrições

* `1 ≤ n ≤ 10^5`
* `1 ≤ m ≤ 2 · 10^5`
* As cidades são numeradas de `1` até `n`.
* Todas as conexões são direcionadas.

---

## 2. Entrada e saída

### 2.1 Entrada

A primeira linha contém:

```text
n m
```

Onde:

* `n` representa a quantidade de cidades;
* `m` representa a quantidade de conexões.

Nas `m` linhas seguintes são informadas duas cidades `a` e `b`, representando uma conexão:

```text
a → b
```

### Exemplo de entrada

```text
4 5
1 3
2 1
2 4
3 2
3 4
```

As arestas correspondentes são:

```text
1 → 3
2 → 1
2 → 4
3 → 2
3 → 4
```

### 2.2 Saída

Quando um ciclo é encontrado, a saída deve conter:

1. a quantidade de cidades do ciclo;
2. a sequência de cidades que forma o ciclo.

Exemplo:

```text
4
2 1 3 2
```

Representação:

```text
2 → 1 → 3 → 2
```

Caso não exista um ciclo:

```text
IMPOSSIBLE
```

---

## 3. Modelagem do problema

O problema pode ser representado por um **grafo dirigido e não ponderado**.

### 3.1 Vértices

Cada cidade representa um vértice.

Para a instância utilizada:

```text
V = {1, 2, 3, 4}
```

### 3.2 Arestas

Cada conexão de voo representa uma aresta direcionada.

```text
E = {
    (1,3),
    (2,1),
    (2,4),
    (3,2),
    (3,4)
}
```

Portanto:

```text
1 → 3
2 → 1
2 → 4
3 → 2
3 → 4
```

### 3.3 Representação visual

```text
1 ─────→ 3
↑        │
│        ↓
└────────2 ─────→ 4

3 ─────────────────→ 4
```

O ciclo existente é:

```text
1 → 3 → 2 → 1
```

---

## 4. Características do grafo

O grafo utilizado possui as seguintes características:

| Característica | Descrição                              |
| -------------- | -------------------------------------- |
| Direção        | As arestas possuem origem e destino    |
| Peso           | Não possui pesos ou custos             |
| Conectividade  | Pode possuir componentes desconectados |
| Ciclos         | Pode possuir ciclos                    |
| Representação  | Lista de adjacência                    |

A principal característica para a solução é o fato de o grafo ser **dirigido**.

---

## 5. Conhecimento prévio

Para solucionar o problema, são necessários os seguintes conhecimentos:

* representação de grafos;
* vértices e arestas;
* grafos dirigidos;
* lista de adjacência;
* Busca em Profundidade (DFS);
* estados dos vértices;
* detecção de ciclos;
* reconstrução de caminhos.

O objetivo principal é utilizar esses conceitos para determinar se existe um ciclo no grafo e, caso exista, recuperar os vértices que fazem parte dele.

---

## 6. Busca em Profundidade (DFS)

A **Busca em Profundidade (DFS)** é utilizada para percorrer o grafo.

Para detectar ciclos em um grafo dirigido, cada vértice pode possuir um dos três estados:

```text
0 → não visitado
1 → sendo explorado
2 → exploração finalizada
```

### Estado 0 — Não visitado

O vértice ainda não foi alcançado pela DFS.

Ao iniciar sua exploração, ele passa para o estado `1`.

### Estado 1 — Sendo explorado

O vértice está atualmente no caminho da DFS.

Se a DFS encontrar uma aresta apontando para um vértice nesse estado, significa que retornamos para um vértice que ainda está no caminho atual.

Isso caracteriza um ciclo.

### Estado 2 — Exploração finalizada

O vértice já foi completamente explorado e não pertence mais ao caminho atual da DFS.

Encontrar uma aresta para um vértice nesse estado não caracteriza, por si só, um ciclo.

---

## 7. Instância pequena

Para demonstrar a solução, será utilizada a seguinte entrada:

```text
4 5
1 3
2 1
2 4
3 2
3 4
```

O grafo pode ser simplificado para:

```text
1 → 3
↑   ↓
└── 2 → 4
```

Além disso:

```text
3 → 4
```

---

## 8. Execução da DFS

A DFS pode começar pelo vértice `1`.

### Passo 1 — Vértice 1

```text
DFS(1)
```

O vértice `1` passa para o estado `1`:

```text
estado = [1, 0, 0, 0]
caminho = [1]
```

---

### Passo 2 — Vértice 3

Existe uma aresta:

```text
1 → 3
```

Então:

```text
DFS(3)
```

Agora:

```text
estado = [1, 0, 1, 0]
caminho = [1, 3]
```

---

### Passo 3 — Vértice 2

Existe uma aresta:

```text
3 → 2
```

Então:

```text
DFS(2)
```

Agora:

```text
estado = [1, 1, 1, 0]
caminho = [1, 3, 2]
```

---

### Passo 4 — Retorno para o vértice 1

A partir de `2`, existe a aresta:

```text
2 → 1
```

Porém, o vértice `1` ainda possui estado `1`.

Isso significa que `1` ainda está no caminho atual da DFS:

```text
1 → 3 → 2 → 1
```

Portanto, foi encontrado um ciclo.

---

## 9. Ciclo encontrado

O ciclo identificado é:

```text
1 → 3 → 2 → 1
```

Também é possível representá-lo começando por outro vértice:

```text
2 → 1 → 3 → 2
```

As duas representações correspondem ao mesmo ciclo.

### Visualização

```text
      ┌──────────┐
      │          ↓
      1 → 3 → 2
          │
          └──→ 4
```

---

## 10. Critério utilizado para detectar o ciclo

A condição utilizada pela solução é:

```text
Se uma aresta aponta para um vértice com estado 1,
então existe um ciclo.
```

Isso acontece porque o estado `1` representa um vértice que ainda está sendo explorado pela DFS.

No exemplo:

```text
1 → 3 → 2
↑       │
└───────┘
```

A aresta:

```text
2 → 1
```

retorna para um vértice que ainda está no caminho atual.

Logo:

```text
2 → 1
```

fecha o ciclo:

```text
1 → 3 → 2 → 1
```

---

## 11. Complexidade

O problema possui até:

```text
n = 10^5 vértices
m = 2 · 10^5 arestas
```

Utilizando **lista de adjacência + DFS**, cada vértice e cada aresta é processado de forma limitada.

### Complexidade de tempo

```text
O(n + m)
```

Onde:

* `n` = número de vértices;
* `m` = número de arestas.

### Complexidade de espaço

```text
O(n + m)
```

O espaço é utilizado principalmente para:

* lista de adjacência;
* estados dos vértices;
* informações necessárias para reconstrução do caminho.

---

## 12. Conclusão

O problema **Round Trip II** pode ser modelado utilizando um grafo dirigido, no qual:

```text
Cidade → Vértice
Conexão de voo → Aresta direcionada
```

A DFS percorre o grafo utilizando três estados para cada vértice:

```text
0 → não visitado
1 → em exploração
2 → finalizado
```

A existência de uma aresta para um vértice no estado `1` indica que a busca retornou para um vértice que ainda está no caminho atual.

Assim, o ciclo pode ser identificado e reconstruído.

### Fluxo da solução

```text
Grafo dirigido
      ↓
Lista de adjacência
      ↓
DFS
      ↓
Estados dos vértices
      ↓
Identificação de estado 1
      ↓
Detecção do ciclo
      ↓
Reconstrução do percurso
```

Para a instância utilizada:

```text
1 → 3 → 2 → 1
```

é o ciclo identificado pela DFS.
