## Algoritmo de Dijkstra: Encontrando o Caminho Mais Curto em Grafos Ponderados

Este README explica o algoritmo de Dijkstra, um algoritmo guloso clássico usado para encontrar o caminho mais curto entre um nó de origem e todos os outros nós em um grafo ponderado. O algoritmo funciona em grafos direcionados ou não direcionados, desde que os pesos das arestas sejam não negativos.

### Funcionamento do Algoritmo:

O algoritmo de Dijkstra opera com base nos seguintes princípios:

1. **Inicialização:**
    - Define-se a distância do nó de origem para ele mesmo como 0 (zero).
    - Define-se a distância do nó de origem para todos os outros nós como infinito (∞).
    - Cria-se um conjunto (ou fila de prioridade) para armazenar os nós a serem processados, inicialmente contendo apenas o nó de origem.

2. **Iteração:**
    - Enquanto o conjunto de nós a serem processados não estiver vazio, seleciona-se o nó com a menor distância atual (que inicialmente será o nó de origem).
    - Para cada vizinho do nó selecionado:
        - Calcula-se a distância do nó de origem até o vizinho, passando pelo nó atual.
        - Se a distância calculada for menor que a distância atual do vizinho, atualiza-se a distância do vizinho.
    - Marca-se o nó atual como processado, garantindo que ele não seja revisitado.

3. **Resultado:**
    - Após o processamento de todos os nós, a distância do nó de origem para qualquer outro nó será a menor distância possível.

### Pseudo-código do Algoritmo de Dijkstra:

```
função dijkstra(grafo, origem):
    # Inicialização
    distancia[origem] = 0
    para cada nó v no grafo:
        se v != origem:
            distancia[v] = infinito
    fila_prioridade.adicionar(origem)

    # Iteração
    enquanto fila_prioridade não estiver vazia:
        u = fila_prioridade.remover_menor() # Nó com a menor distância
        para cada vizinho v de u:
            nova_distancia = distancia[u] + peso(u, v)
            se nova_distancia < distancia[v]:
                distancia[v] = nova_distancia
                fila_prioridade.adicionar(v)

    retornar distancia
```

### Exemplo de Uso:

**Grafo:**

```
Número de nós: 5
Número de arestas: 6

Arestas (origem, destino, peso):
0 1 4
0 2 1
1 3 1
2 1 2
2 3 5
3 4 3

Nó inicial: 0
Nó de destino: 4
```

**Saída Esperada:**

```
Distância do nó 0 ao nó 4: 7
```

**Explicação:**

O algoritmo de Dijkstra calculará que o caminho mais curto do nó `0` ao nó `4` tem um custo total de `7`. Um possível caminho mais curto é:

`0 -> 2 -> 1 -> 3 -> 4`

### Implementação em C++:

```c++
#include <bits/stdc++.h>
using namespace std;

typedef pair<int, int> pii;

const int maxn = 1e5 + 10;
const int inf = 1e9 + 10;

int n, m;
int distancia[maxn];
bool visitado[maxn];
vector<pii> grafo[maxn];

void dijkstra(int no_inicial) {
    for (int i = 0; i < n; i++) {
        distancia[i] = inf;
        visitado[i] = false;
    }
    distancia[no_inicial] = 0;

    priority_queue<pii, vector<pii>, greater<pii>> fila;
    fila.push({0, no_inicial});

    while (!fila.empty()) {
        int no_atual = fila.top().second;
        fila.pop();

        if (visitado[no_atual]) {
            continue;
        }
        visitado[no_atual] = true;

        for (auto visinho : grafo[no_atual]) {
            int peso = visinho.second;
            int no_vizinho = visinho.first;

            if (distancia[no_vizinho] > distancia[no_atual] + peso) {
                distancia[no_vizinho] = distancia[no_atual] + peso;
                fila.push({distancia[no_vizinho], no_vizinho});
            }
        }
    }
}

int main() {
    cin >> n >> m;

    for (int i = 0; i < m; i++) {
        int u, v, p;
        cin >> u >> v >> p;
        grafo[u].push_back({v, p});
        grafo[v].push_back({u, p});
    }

    int no_inicial, no_destino;
    cin >> no_inicial >> no_destino;

    dijkstra(no_inicial);

    cout << distancia[no_destino] << endl;

    return 0;
}
```

Este código implementa o algoritmo de Dijkstra e calcula a distância mais curta do nó de origem para um nó de destino especificado pelo usuário.
