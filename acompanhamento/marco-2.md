# Marco 2 — Componentes Conexas

## 1. Adaptação para grafo não dirigido

Para a análise das componentes conexas, o grafo utilizado no problema foi adaptado para sua versão **não dirigida**, desconsiderando a direção das arestas.

### Vértices

```text
V = {1, 2, 3, 4}
```

### Arestas

```text
E = {
    {1,3},
    {2,1},
    {2,4},
    {3,2},
    {3,4}
}
```

---

## 2. Lista de adjacência

A representação do grafo não dirigido por meio de lista de adjacência é:

```text
1: [3, 2]
2: [1, 4, 3]
3: [1, 2, 4]
4: [2, 3]
```

Como o grafo é não dirigido, cada aresta aparece na lista de adjacência dos dois vértices envolvidos.

Por exemplo:

```text
1 — 3
```

é representada como:

```text
1: [3]
3: [1]
```

---

## 3. Componentes conexas

Uma **componente conexa** é um conjunto de vértices no qual existe um caminho entre qualquer par de vértices pertencentes à componente.

Para identificar as componentes conexas, é utilizada uma DFS.

### Estrutura de dados

Inicialmente:

```text
visitado = [FALSO, FALSO, FALSO, FALSO]

componentes = []

comp_id = 0
```

O vetor `visitado` indica quais vértices já foram explorados.

---

## 4. Execução da DFS

A busca começa pelo primeiro vértice ainda não visitado.

### Passo 1 — Vértice 1

O vértice `1` ainda não foi visitado.

Uma nova componente é criada:

```text
C1 = {}
```

A DFS é iniciada:

```text
DFS(1)
```

O vértice `1` é marcado como visitado.

```text
visitado = [VERDADEIRO, FALSO, FALSO, FALSO]
```

A partir de `1`, existem conexões com:

```text
1 → 3
1 → 2
```

---

### Passo 2 — Vértice 3

A DFS pode seguir para o vértice `3`.

```text
DFS(3)
```

Agora:

```text
visitado = [VERDADEIRO, FALSO, VERDADEIRO, FALSO]
```

A partir de `3`, existem conexões com:

```text
3 → 1
3 → 2
3 → 4
```

O vértice `1` já foi visitado, então a busca continua para `2`.

---

### Passo 3 — Vértice 2

```text
DFS(2)
```

Agora:

```text
visitado = [VERDADEIRO, VERDADEIRO, VERDADEIRO, FALSO]
```

O vértice `2` possui conexões com:

```text
2 → 1
2 → 4
2 → 3
```

Os vértices `1` e `3` já foram visitados.

A busca continua para `4`.

---

### Passo 4 — Vértice 4

```text
DFS(4)
```

Agora:

```text
visitado = [VERDADEIRO, VERDADEIRO, VERDADEIRO, VERDADEIRO]
```

O vértice `4` possui conexões com:

```text
4 → 2
4 → 3
```

Ambos já foram visitados.

A DFS termina.

---

## 5. Componente encontrada

Durante a primeira execução da DFS, todos os quatro vértices foram alcançados:

```text
C1 = {1, 2, 3, 4}
```

Portanto, o grafo possui apenas uma componente conexa:

```text
Componentes = {
    {1, 2, 3, 4}
}
```

Visualmente:

```text
      1
     / \
    /   \
   3 ─── 2
    \   /
     \ /
      4
```

Todos os vértices pertencem à mesma componente porque existe um caminho entre qualquer par de vértices.

---

## 6. Verificação dos demais vértices

Após a conclusão da DFS iniciada em `1`, o algoritmo continua verificando os demais vértices.

### Vértice 2

```text
visitado[2] = VERDADEIRO
```

Nenhuma nova DFS é iniciada.

### Vértice 3

```text
visitado[3] = VERDADEIRO
```

Nenhuma nova DFS é iniciada.

### Vértice 4

```text
visitado[4] = VERDADEIRO
```

Nenhuma nova DFS é iniciada.

Assim, nenhuma nova componente é criada.

---

## 7. Resultado

O resultado da análise é:

```text
Quantidade de componentes: 1

C1 = {1, 2, 3, 4}
```

O grafo é, portanto, **conexo**.

---

## 8. Complexidade

A identificação das componentes conexas utilizando DFS percorre os vértices e as arestas do grafo.

### Complexidade de tempo

```text
O(V + E)
```

ou, utilizando `N` para vértices e `M` para arestas:

```text
O(N + M)
```

Cada vértice e cada aresta é processado um número limitado de vezes durante a busca.

### Complexidade de espaço

```text
O(V + E)
```

Esse espaço é utilizado principalmente para:

* armazenar a lista de adjacência;
* armazenar os estados dos vértices;
* armazenar informações auxiliares da DFS;
* armazenar a pilha de recursão, no caso de uma implementação recursiva.

---

## 9. Consultas de conectividade

Após o pré-processamento das componentes, é possível verificar se dois vértices pertencem à mesma componente.

Para dois vértices `u` e `v`, basta comparar seus identificadores de componente:

```text
componente[u] == componente[v]
```

Se forem iguais, os dois vértices pertencem à mesma componente conexa.

Essa consulta pode ser realizada em:

```text
O(1)
```

após a identificação das componentes.

---

## 10. Conclusão

A adaptação do problema para um grafo não dirigido permite utilizar a DFS para identificar suas componentes conexas.

Para o grafo analisado, a busca iniciada no vértice `1` consegue alcançar todos os demais vértices:

```text
1 → 3 → 2 → 4
```

Consequentemente, todos pertencem à mesma componente:

```text
C1 = {1, 2, 3, 4}
```

Portanto:

```text
Número de componentes conexas = 1
```

A solução utiliza:

```text
Grafo não dirigido
       ↓
Lista de adjacência
       ↓
DFS
       ↓
Vértices visitados
       ↓
Identificação das componentes
       ↓
Consultas de conectividade
```
