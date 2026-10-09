import sys

# Aumenta o limite de recursão para suportar o pior caso
# de um caminho linear (N = 10^5)
sys.setrecursionlimit(200000)


class DirectedCycle:

    def _init_(self, n, adj):
        self.n = n
        self.adj = adj

        # Estados:
        # 0 = BRANCO (não visitado)
        # 1 = CINZA (na pilha de recursão)
        # 2 = PRETO (concluído)
        self.cor = [0] * (n + 1)

        self.pai = [0] * (n + 1)

        self.inicio_ciclo = -1
        self.fim_ciclo = -1

    def encontrar_ciclo(self):

        # Percorre todos os vértices para cobrir
        # grafos desconexos / múltiplas componentes
        for i in range(1, self.n + 1):

            if self.cor[i] == 0:

                if self._dfs(i):
                    break

        return self.inicio_ciclo != -1

    def _dfs(self, u):

        # Marca o vértice como CINZA
        self.cor[u] = 1

        for v in self.adj[u]:

            # Vértice ainda não visitado
            if self.cor[v] == 0:

                self.pai[v] = u

                if self._dfs(v):
                    return True

            # Encontrou uma aresta para um vértice CINZA
            # Isso significa que encontramos um ciclo
            elif self.cor[v] == 1:

                self.inicio_ciclo = v
                self.fim_ciclo = u

                return True

        # Terminou de processar o vértice
        self.cor[u] = 2

        return False

    def obter_rota(self):

        if self.inicio_ciclo == -1:
            return None

        rota = [self.inicio_ciclo]

        atual = self.fim_ciclo

        while atual != self.inicio_ciclo:

            rota.append(atual)

            atual = self.pai[atual]

        rota.append(self.inicio_ciclo)

        # Inverte para colocar a rota na ordem correta
        rota.reverse()

        return rota


def main():

    # Lê o arquivo entrada.txt
    with open("entrada.txt", "r") as arquivo:
        input_data = arquivo.read().split()

    if not input_data:
        return

    # Primeiros valores:
    # n = número de vértices
    # m = número de arestas
    n = int(input_data[0])
    m = int(input_data[1])

    # Lista de adjacência
    adj = [[] for _ in range(n + 1)]

    idx = 2

    # Lê as M arestas
    for _ in range(m):

        u = int(input_data[idx])
        v = int(input_data[idx + 1])

        adj[u].append(v)

        idx += 2

    # Cria o detector
    detector = DirectedCycle(n, adj)

    # Procura ciclo
    if detector.encontrar_ciclo():

        rota = detector.obter_rota()

        print(len(rota))
        print(*(rota))

    else:

        print("IMPOSSÍVEL")


if _name_ == '_main_':
    main()
