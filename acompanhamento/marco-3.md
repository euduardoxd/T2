# Marco 3 — Estratégia Algorítmica

## 1. Propriedade estrutural e critério algorítmico

A propriedade estrutural central do problema é a existência de um **ciclo simples dirigido** no grafo.

Um ciclo simples dirigido é um caminho fechado:

```text
v₁ → v₂ → ... → vₖ → v₁
```

onde todos os vértices intermediários `vᵢ` são distintos e:

```text
k ≥ 2
```

### Critério de reconhecimento

Para identificar a existência de um ciclo dirigido, utiliza-se uma **Busca em Profundidade (DFS)** com três estados para os vértices:

```text
BRANCO (0) → Vértice não visitado

CINZA (1)  → Vértice em processamento,
             presente na pilha de recursão atual

PRETO (2)  → Vértice totalmente processado
```

Um ciclo dirigido é reconhecido quando a DFS encontra uma **aresta de retorno (back-edge)**.

Isso acontece quando o algoritmo tenta navegar para um vértice vizinho que ainda está no estado:

```text
CINZA
```

Por exemplo:

```text
u → v
```

Se:

```text
cor[v] = CINZA
```

então existe uma aresta de retorno e, consequentemente, um ciclo dirigido.

---

## 2. Obtenção da resposta exigida

Para construir a rota e obter a sequência das cidades pertencentes ao ciclo, utiliza-se um vetor de pais:

```text
pai[]
```

Esse vetor armazena o antecessor de cada vértice na árvore formada pela DFS.

Quando uma aresta de retorno:

```text
u → v
```

é encontrada, onde `v` está no estado CINZA, são armazenados:

```text
inicio_ciclo = v

fim_ciclo = u
```

A partir de `u`, o algoritmo retrocede utilizando o vetor `pai[]` até alcançar `v`.

O caminho encontrado é armazenado em uma lista e posteriormente invertido para obter a ordem correta da viagem:

```text
v → ... → u → v
```

Dessa forma, é possível reconstruir o ciclo completo e apresentá-lo no formato exigido pelo problema.

---

## 3. Implementação de referência — algs4

Para a resolução do problema, utiliza-se como referência a classe **DirectedCycle**, presente no livro *Algorithms, 4th Edition*, de Sedgewick e Wayne, através da biblioteca algs4.

### Papel da implementação DirectedCycle

A classe `DirectedCycle` fornece a estrutura base da DFS utilizada para detectar ciclos em grafos dirigidos.

Na implementação original são utilizados:

```text
onStack[]
```

e:

```text
edgeTo[]
```

Essas estruturas possuem funções equivalentes às utilizadas na solução:

```text
onStack[] → estado CINZA

edgeTo[] → vetor pai[]
```

O vetor `onStack[]` permite identificar se determinado vértice ainda está presente no caminho atual da DFS.

Já o vetor `edgeTo[]` permite reconstruir o caminho percorrido até a descoberta do ciclo.

---

## 4. Adaptações previstas

Apesar de a implementação `DirectedCycle` servir como referência, algumas adaptações são necessárias para atender às características do problema.

### 4.1. Adaptação de tipos e leitura

A estrutura de entrada da algs4 será substituída por uma leitura de alta performance.

Podem ser utilizadas estruturas como:

```text
sys.stdin.read
```

ou:

```text
BufferedReader
```

A escolha é necessária devido ao limite de até:

```text
M = 2 · 10⁵
```

arestas e ao limite de execução de:

```text
1,00 s
```

---

### 4.2. Representação da resposta

A implementação original armazena o ciclo utilizando uma estrutura de pilha (`Stack`).

Na adaptação, a extração do ciclo será modificada para imprimir:

1. o tamanho `k` da rota;
2. a sequência de cidades separadas por espaço.

O vértice inicial também aparece novamente no final da sequência para fechar o ciclo.

Por exemplo:

```text
4
2 1 3 2
```

Nesse caso:

```text
k = 4
```

porque a cidade `2` aparece tanto no início quanto no final da rota.

---

### 4.3. Tratamento de grafos acíclicos

Ao final da execução, deve ser feita uma verificação para determinar se algum ciclo foi encontrado.

Caso nenhuma aresta de retorno seja detectada, o programa deverá imprimir:

```text
IMPOSSÍVEL
```

---

## 5. Instância pequena e rastreamento manual

Para demonstrar o funcionamento da estratégia, será utilizada uma instância pequena do problema.

### Instância do problema

O grafo possui:

```text
N = 4 cidades

M = 5 voos
```

As arestas dirigidas são:

```text
E = {
    (1 → 3),
    (2 → 1),
    (2 → 4),
    (3 → 2),
    (3 → 4)
}
```

---

## 6. Lista de adjacência

A representação do grafo utilizando lista de adjacência é:

```text
1: [3]

2: [1, 4]

3: [2, 4]

4: []
```

Como o grafo é dirigido, cada aresta aparece somente na lista de adjacência do vértice de origem.

Por exemplo:

```text
2 → 1
```

aparece apenas como:

```text
2: [1, 4]
```

---

## 7. Estado inicial das estruturas de dados

Antes do início da DFS, os vértices estão no estado BRANCO:

```text
cor = [BRANCO, BRANCO, BRANCO, BRANCO]
```

O vetor de pais é inicializado como:

```text
pai = [0, 0, 0, 0]
```

As variáveis responsáveis por armazenar o ciclo são inicializadas como:

```text
inicio_ciclo = -1

fim_ciclo = -1
```

---

## 8. Rastreamento manual da DFS

### Passo 1 — Laço externo

O algoritmo percorre os vértices:

```text
for i = 1..4
```

O primeiro vértice analisado é:

```text
i = 1
```

Como:

```text
cor[1] = BRANCO
```

uma DFS é iniciada:

```text
DFS(1)
```

---

### Passo 2 — DFS(1)

O vértice `1` é marcado como CINZA:

```text
cor[1] = CINZA
```

O estado passa a ser:

```text
cor = [CINZA, BRANCO, BRANCO, BRANCO]
```

A lista de adjacência de `1` é:

```text
1: [3]
```

O próximo vértice analisado é:

```text
3
```

Como:

```text
cor[3] = BRANCO
```

o algoritmo define:

```text
pai[3] = 1
```

e chama:

```text
DFS(3)
```

---

### Passo 3 — DFS(3)

O vértice `3` é marcado como CINZA:

```text
cor[3] = CINZA
```

Agora:

```text
cor = [CINZA, BRANCO, CINZA, BRANCO]
```

A lista de adjacência de `3` é:

```text
3: [2, 4]
```

O primeiro vizinho analisado é:

```text
2
```

Como:

```text
cor[2] = BRANCO
```

o algoritmo define:

```text
pai[2] = 3
```

e chama:

```text
DFS(2)
```

---

### Passo 4 — DFS(2)

O vértice `2` é marcado como CINZA:

```text
cor[2] = CINZA
```

Agora:

```text
cor = [CINZA, CINZA, CINZA, BRANCO]
```

A lista de adjacência de `2` é:

```text
2: [1, 4]
```

O primeiro vizinho analisado é:

```text
1
```

O algoritmo consulta:

```text
cor[1]
```

Como:

```text
cor[1] = CINZA
```

é encontrada uma **aresta de retorno**:

```text
2 → 1
```

Isso significa que existe um ciclo dirigido.

---

## 9. Detecção do ciclo

Quando a aresta:

```text
2 → 1
```

é encontrada, são registrados:

```text
inicio_ciclo = 1

fim_ciclo = 2
```

A busca pode então ser interrompida, retornando `True` em cascata pelas chamadas da DFS.

O caminho percorrido até o momento foi:

```text
1 → 3 → 2
```

e a aresta encontrada fecha o ciclo:

```text
2 → 1
```

Portanto, o ciclo é:

```text
1 → 3 → 2 → 1
```

---

## 10. Reconstrução da rota

A reconstrução começa pelo:

```text
fim_ciclo = 2
```

O algoritmo utiliza o vetor `pai[]` para voltar até o início do ciclo.

### Primeiro passo

```text
atual = 2
```

Adiciona-se:

```text
2
```

Depois:

```text
atual = pai[2]
```

Como:

```text
pai[2] = 3
```

adiciona-se:

```text
3
```

---

### Segundo passo

Agora:

```text
atual = 3
```

Então:

```text
atual = pai[3]
```

Como:

```text
pai[3] = 1
```

adiciona-se:

```text
1
```

O vértice `1` corresponde ao:

```text
inicio_ciclo
```

A reconstrução é encerrada.

---

## 11. Rota encontrada

Durante a reconstrução, a sequência obtida é:

```text
[2, 3, 1]
```

Essa sequência está invertida em relação à ordem desejada.

Após a inversão:

```text
[1, 3, 2]
```

Para fechar o ciclo, o vértice inicial é adicionado novamente ao final:

```text
[1, 3, 2, 1]
```

A mesma rota também pode ser apresentada começando pelo vértice `2`:

```text
[2, 1, 3, 2]
```

Portanto:

```text
k = 4
```

e a resposta pode ser apresentada como:

```text
4
2 1 3 2
```

Essa sequência representa o ciclo:

```text
2 → 1 → 3 → 2
```

---

## 12. Complexidade de tempo

A complexidade da estratégia utilizando DFS é:

```text
O(V + E)
```

ou, utilizando a nomenclatura do problema:

```text
O(N + M)
```

### Processamento dos vértices

O laço externo e a DFS analisam os vértices do grafo.

Cada vértice é processado de maneira controlada, sendo marcado de acordo com seu estado.

A DFS é chamada no máximo uma vez para cada vértice ainda não visitado.

Portanto:

```text
O(V)
```

---

### Processamento das arestas

A busca percorre as listas de adjacência.

Ao longo da execução, cada uma das `M` arestas dirigidas é examinada no máximo uma vez.

Assim:

```text
O(E)
```

---

### Reconstrução do ciclo

Após a detecção do ciclo, o algoritmo utiliza o vetor `pai[]` para reconstruir a rota.

O tempo necessário é proporcional ao tamanho do ciclo.

No pior caso, o ciclo pode envolver todos os vértices:

```text
O(V)
```

---

### Complexidade total

Somando as etapas:

```text
O(V) + O(E) + O(V)
```

temos:

```text
O(V + E)
```

ou:

```text
O(N + M)
```

Para:

```text
N = 10⁵

M = 2 · 10⁵
```

a quantidade de elementos processados permanece linear:

```text
N + M = 3 · 10⁵
```

---

## 13. Complexidade de memória

A complexidade total de memória é:

```text
O(V + E)
```

A memória utilizada pode ser dividida entre a representação do grafo e as estruturas auxiliares.

### Representação do grafo

A lista de adjacência necessita de espaço para armazenar os vértices e as arestas:

```text
O(V + E)
```

São utilizadas:

```text
V posições de listas

E arestas dirigidas
```

---

### Estruturas auxiliares

O vetor de estados possui:

```text
cor[1...N]
```

e ocupa:

```text
O(V)
```

O vetor de antecessores possui:

```text
pai[1...N]
```

e também ocupa:

```text
O(V)
```

---

### Pilha de recursão

Em uma implementação recursiva, a DFS também utiliza a pilha de chamadas.

No pior caso, o grafo pode possuir um caminho simples como:

```text
1 → 2 → 3 → ... → N
```

Nesse cenário, a profundidade da recursão pode chegar a:

```text
O(N)
```

---

## 14. Complexidade total de memória

Considerando todas as estruturas utilizadas:

```text
Lista de adjacência → O(V + E)

Vetor cor[]         → O(V)

Vetor pai[]         → O(V)

Pilha da DFS        → O(V)
```

Portanto, a complexidade total permanece:

```text
O(V + E)
```

ou:

```text
O(N + M)
```

Essa quantidade de memória permanece abaixo do limite estipulado de:

```text
512 MB
```

para os limites de entrada considerados.

---

## 15. Conclusão

A estratégia utilizada para resolver o problema consiste em aplicar uma **Busca em Profundidade (DFS)** sobre o grafo dirigido.

O algoritmo utiliza três estados:

```text
BRANCO
CINZA
PRETO
```

A existência de uma aresta para um vértice CINZA indica a presença de um ciclo dirigido.

O processo pode ser resumido como:

```text
Grafo dirigido
       ↓
Lista de adjacência
       ↓
DFS
       ↓
Estados dos vértices
       ↓
Detecção de aresta de retorno
       ↓
Identificação do ciclo
       ↓
Vetor pai[]
       ↓
Reconstrução da rota
```

Na instância analisada, a DFS encontra a aresta:

```text
2 → 1
```

enquanto o vértice `1` ainda está CINZA.

Isso permite identificar o ciclo:

```text
2 → 1 → 3 → 2
```

A resposta correspondente é:

```text
4
2 1 3 2
```

A complexidade da solução é:

```text
Tempo:   O(N + M)

Memória: O(N + M)
```
