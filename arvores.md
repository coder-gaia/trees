# Trabalho – Estruturas Avançadas de Árvores

---

## Parte 1 – Tipos de Árvores

---

### AVL (Adelson-Velsky e Landis)

#### Conceito

A árvore AVL é uma Árvore Binária de Busca (BST) com **balanceamento automático**. Foi proposta por Georgy Adelson-Velsky e Evgenii Landis em 1962 e é considerada a primeira estrutura de dados de árvore auto-balanceada da história da computação.

O balanceamento é mantido por meio do **fator de balanceamento (FB)** de cada nó, definido como:

```
FB = altura(subárvore esquerda) - altura(subárvore direita)
```

Em uma AVL, o fator de balanceamento de todo nó deve estar sempre entre **-1, 0 ou +1**. Sempre que uma inserção ou remoção viola essa condição, rotações são realizadas para restaurar o equilíbrio.

#### Características

- É uma BST com restrição adicional de balanceamento.
- Cada nó armazena seu fator de balanceamento (ou a altura da subárvore).
- Após toda inserção ou remoção, o fator de balanceamento é recalculado e, se necessário, rotações simples ou duplas são aplicadas.
- A altura de uma AVL com `n` nós é sempre `O(log n)`.

#### Vantagens

- **Busca garantidamente eficiente:** como a árvore está sempre balanceada, a operação de busca é sempre `O(log n)`, sem degradação.
- **Ideal para leituras frequentes:** o rígido controle de altura torna a estrutura excelente para aplicações que realizam muitas buscas.
- **Previsibilidade:** o desempenho é consistente independentemente da ordem de inserção dos dados.

#### Desvantagens

- **Inserção e remoção mais custosas:** cada operação pode exigir múltiplas rotações para restaurar o balanceamento.
- **Maior uso de memória:** é necessário armazenar o fator de balanceamento ou a altura em cada nó.
- **Implementação mais complexa** do que uma BST simples.

#### Exemplo ilustrado

Inserção sequencial dos valores **10, 20, 30** em uma AVL:

**Passo 1 – Inserindo 10:**

```
10
```

FB(10) = 0 ✓

**Passo 2 – Inserindo 20:**

```
10
  \
   20
```

FB(10) = -1 ✓

**Passo 3 – Inserindo 30 → FB(10) = -2, violação detectada → Rotação Simples à Esquerda:**

Antes da rotação:

```
10
  \
   20
     \
      30
```

FB(10) = -2 ✗

Após a rotação simples à esquerda em torno de 10:

```
   20
  /  \
10    30
```

FB(20) = 0, FB(10) = 0, FB(30) = 0 ✓

---

### Rubro-Negra (Red-Black Tree)

#### Conceito

A árvore Rubro-Negra é uma BST com **balanceamento aproximado** mantido por meio de coloração dos nós. Cada nó recebe a cor **vermelho** ou **preto**, e um conjunto de regras garante que a árvore não fique excessivamente desbalanceada. Foi introduzida por Rudolf Bayer em 1972 e refinada por Leonidas Guibas e Robert Sedgewick em 1978.

Diferente da AVL, que mantém um equilíbrio rigoroso, a Rubro-Negra tolera uma variação maior na altura, garantindo que o caminho mais longo nunca seja mais do que o dobro do caminho mais curto.

#### Regras de coloração

1. **Todo nó é vermelho ou preto.**
2. **A raiz é sempre preta.**
3. **Todo nó folha nulo (NIL) é preto.**
4. **Se um nó é vermelho, ambos os seus filhos são pretos** (não existem dois nós vermelhos consecutivos em qualquer caminho).
5. **Para qualquer nó, todos os caminhos da raiz até as folhas NIL contêm o mesmo número de nós pretos** (altura negra uniforme).

Essas regras garantem que a altura da árvore seja no máximo `2 * log(n+1)`.

#### Vantagens

- **Menos rotações que a AVL:** inserções e remoções tendem a exigir menos rebalanceamento.
- **Excelente para operações mistas:** quando há muitas inserções, remoções e buscas intercaladas.
- **Amplamente utilizada em implementações reais** de linguagens e sistemas operacionais (ex.: `std::map` em C++, Java `TreeMap`, Linux scheduler).

#### Desvantagens

- **Busca ligeiramente mais lenta que a AVL:** como o balanceamento é menos rígido, a árvore pode ser um pouco mais alta.
- **Implementação complexa:** as regras de coloração combinadas com as rotações tornam o código difícil de escrever e depurar.
- **Não intuitiva:** a lógica de coloração não tem correspondência imediata com a hierarquia dos dados.

#### Exemplo ilustrado

Inserção de **10, 20, 30** em uma Rubro-Negra:

**Passo 1 – Inserindo 10 (raiz → preto):**

```
(P)10
```

**Passo 2 – Inserindo 20 (filho → vermelho):**

```
(P)10
     \
    (V)20
```

**Passo 3 – Inserindo 30 → violação: dois vermelhos consecutivos (20 e 30):**

Antes do rebalanceamento:

```
(P)10
     \
    (V)20
          \
          (V)30
```

Rotação simples à esquerda em torno de 10 + recoloração:

```
   (P)20
   /    \
(P)10  (P)30
```

Todos os caminhos da raiz às folhas NIL possuem 1 nó preto. ✓

---

### N-ária

#### Conceito

Uma árvore N-ária (ou árvore de ordem N) é uma estrutura em que cada nó pode ter **até N filhos**, onde N é um valor definido conforme a necessidade da aplicação. Quando N = 2, temos uma árvore binária. Quando N = 3, uma árvore ternária, e assim por diante.

Uma variação importante e amplamente usada é a **Árvore B (B-Tree)**, uma árvore N-ária balanceada otimizada para leitura e escrita em disco.

#### Diferenças em relação às árvores binárias

| Aspecto                       | Árvore Binária                   | Árvore N-ária                            |
| ----------------------------- | -------------------------------- | ---------------------------------------- |
| Número de filhos              | Máximo de 2                      | Máximo de N (N > 2)                      |
| Largura da árvore             | Menor                            | Maior                                    |
| Altura da árvore              | Maior para o mesmo número de nós | Menor, pois cada nível comporta mais nós |
| Acesso a disco                | Menos eficiente (mais níveis)    | Mais eficiente (menos níveis, menos I/O) |
| Complexidade de implementação | Menor                            | Maior                                    |

#### Vantagens

- **Altura reduzida:** com mais filhos por nó, a árvore cresce em largura em vez de altura, reduzindo o número de acessos necessários para chegar a um elemento.
- **Eficiência em I/O:** especialmente relevante para sistemas que leem/escrevem em disco, pois cada nó pode armazenar muitas chaves em um único bloco de memória.
- **Representa hierarquias naturais:** sistemas com múltiplas ramificações (menus, diretórios, taxonomias) são modelados de forma mais fiel.

#### Desvantagens

- **Maior complexidade de implementação:** gerenciar múltiplos filhos, divisões e fusões de nós é mais trabalhoso.
- **Pode desperdiçar memória:** nós com poucos filhos ocupam espaço alocado para N filhos.
- **Algoritmos de travessia mais complexos** do que em árvores binárias.

#### Exemplo ilustrado

Árvore ternária (N = 3) com os valores 1 a 13:

```
            [4, 8]
           /   |   \
       [1,2,3] [5,6,7] [9,10,11,12,13]
```

Cada nó interno pode ter até 3 filhos e armazenar até 2 chaves.

#### Aplicações práticas

- **Sistemas de arquivos:** o NTFS (Windows) e o HFS+ (macOS) usam árvores B para indexar arquivos e diretórios.
- **Bancos de dados relacionais:** PostgreSQL, MySQL e Oracle utilizam B-Trees e B+ Trees para indexação de tabelas.
- **Menus e navegação:** a estrutura de menus de um sistema operacional ou aplicação pode ser representada como uma árvore N-ária.
- **Taxonomias e ontologias:** classificações biológicas, hierarquias de categorias em e-commerce.
- **Compiladores:** árvores sintáticas (parse trees) costumam ser N-árias, pois produções gramaticais podem ter múltiplos filhos.

---

## Parte 2 – Operações em Árvores

---

### Rotação Simples à Direita

#### Objetivo

Reequilibrar uma subárvore que está **sobrecarregada à esquerda**, ou seja, quando o filho esquerdo de um nó possui altura muito maior que o filho direito, causando um fator de balanceamento positivo excessivo (`FB > 1`).

#### Situação em que é utilizada

Ocorre quando há **desbalanceamento Esquerda-Esquerda (LL)**: o nó problemático e seu filho esquerdo estão ambos desbalanceados para a esquerda.

Condição: `FB(nó) = +2` e `FB(filho esquerdo) >= 0`.

#### Exemplo antes e depois da rotação

**Antes:**

```
      C  (FB = +2)
     /
    B   (FB = +1)
   /
  A
```

**Operação:** B sobe para a posição de C. C desce e vira filho direito de B. O filho direito de B (se existir) vira filho esquerdo de C.

**Depois:**

```
    B
   / \
  A   C
```

FB(B) = 0, FB(A) = 0, FB(C) = 0 ✓

---

### Rotação Simples à Esquerda

#### Objetivo

Reequilibrar uma subárvore que está **sobrecarregada à direita**, ou seja, quando o filho direito de um nó possui altura muito maior que o filho esquerdo, causando um fator de balanceamento negativo excessivo (`FB < -1`).

#### Situação em que é utilizada

Ocorre quando há **desbalanceamento Direita-Direita (RR)**: o nó problemático e seu filho direito estão ambos desbalanceados para a direita.

Condição: `FB(nó) = -2` e `FB(filho direito) <= 0`.

#### Exemplo antes e depois da rotação

**Antes:**

```
A   (FB = -2)
 \
  B  (FB = -1)
   \
    C
```

**Operação:** B sobe para a posição de A. A desce e vira filho esquerdo de B. O filho esquerdo de B (se existir) vira filho direito de A.

**Depois:**

```
    B
   / \
  A   C
```

FB(B) = 0, FB(A) = 0, FB(C) = 0 ✓

---

### Rotação Dupla

As rotações duplas são aplicadas quando uma rotação simples não é suficiente para corrigir o desbalanceamento — especificamente quando o filho problemático está desbalanceado para o lado oposto ao do pai.

---

#### Esquerda-Direita (LR)

**Situação:** `FB(nó) = +2` e `FB(filho esquerdo) = -1` — desbalanceamento Esquerda-Direita.

**Procedimento:** primeiro aplica-se uma **rotação simples à esquerda** no filho esquerdo, depois uma **rotação simples à direita** no nó raiz da subárvore.

**Antes:**

```
      C  (FB = +2)
     /
    A   (FB = -1)
     \
      B
```

**Passo 1 – Rotação à esquerda em A:**

```
      C
     /
    B
   /
  A
```

**Passo 2 – Rotação à direita em C:**

```
    B
   / \
  A   C
```

FB(B) = 0 ✓

---

#### Direita-Esquerda (RL)

**Situação:** `FB(nó) = -2` e `FB(filho direito) = +1` — desbalanceamento Direita-Esquerda.

**Procedimento:** primeiro aplica-se uma **rotação simples à direita** no filho direito, depois uma **rotação simples à esquerda** no nó raiz da subárvore.

**Antes:**

```
A   (FB = -2)
 \
  C  (FB = +1)
 /
B
```

**Passo 1 – Rotação à direita em C:**

```
A
 \
  B
   \
    C
```

**Passo 2 – Rotação à esquerda em A:**

```
    B
   / \
  A   C
```

FB(B) = 0 ✓

---

### Inversão (Espelhamento)

#### Conceito

A operação de inversão (ou espelhamento) de uma árvore binária consiste em **trocar recursivamente os filhos esquerdo e direito de todos os nós** da árvore. O resultado é uma imagem espelhada da árvore original.

O algoritmo pode ser implementado de forma recursiva:

```
inverter(nó):
    se nó == null:
        retornar
    trocar(nó.esquerdo, nó.direito)
    inverter(nó.esquerdo)
    inverter(nó.direito)
```

#### Aplicação

- **Processamento de imagens:** inversão de estruturas hierárquicas que representam componentes visuais.
- **Compiladores:** transformações de árvores sintáticas abstratas (AST) durante otimizações.
- **Resolução de puzzles:** o problema de inverter uma árvore binária ficou famoso após uma publicação de Max Howell, criador do Homebrew, que relatou ter sido reprovado em entrevista no Google por não conseguir resolvê-lo — tornando-se um símbolo do debate sobre o que entrevistas técnicas realmente medem.
- **Testes e verificação:** comparação estrutural entre uma árvore e sua versão espelhada.

#### Exemplo antes e depois da operação

**Antes:**

```
        4
       / \
      2   7
     / \ / \
    1  3 6  9
```

**Depois (espelhada):**

```
        4
       / \
      7   2
     / \ / \
    9  6 3  1
```

Todos os filhos esquerdo e direito de cada nó foram trocados recursivamente.

---

## Parte 3 – Aplicação Prática

### Sistema de Banco de Dados Relacional — Indexação com Árvore B+ (N-ária)

#### Descrição do sistema

Bancos de dados relacionais como PostgreSQL, MySQL e Oracle precisam localizar registros em tabelas com milhões ou bilhões de linhas de forma extremamente eficiente. Para isso, utilizam **índices**, que são estruturas de dados separadas que permitem encontrar registros sem percorrer toda a tabela.

#### Estrutura escolhida: Árvore B+ (variação da N-ária)

A **Árvore B+** é uma árvore N-ária balanceada com as seguintes características específicas:

- Todos os dados reais ficam **apenas nas folhas**.
- Os nós internos armazenam somente **chaves de roteamento** para guiar a busca.
- As folhas são **encadeadas em uma lista ligada**, permitindo varreduras sequenciais eficientes (range queries).
- O grau N (número máximo de filhos) é configurado para que cada nó ocupe exatamente **um bloco de disco** (geralmente 4KB ou 8KB).

#### Justificativa

**Desempenho:**
Uma tabela com 1 bilhão de registros indexada em uma B+ Tree com grau 500 terá altura de apenas `log₅₀₀(1.000.000.000) ≈ 3 a 4 níveis`. Isso significa que qualquer registro pode ser encontrado em **3 ou 4 acessos a disco**, independentemente do tamanho da tabela.

**Organização dos dados:**
Como cada nó corresponde a um bloco de disco, o banco de dados realiza a leitura de múltiplas chaves em um único acesso a disco (I/O). Árvores binárias como AVL ou Rubro-Negra teriam altura muito maior (`log₂`), exigindo muito mais acessos ao disco.

**Operações realizadas pelo sistema:**

- **Busca pontual** (`WHERE id = 1000`): O(log N) — desce a árvore até a folha.
- **Busca por intervalo** (`WHERE data BETWEEN '2024-01-01' AND '2024-12-31'`): localiza o início e percorre as folhas encadeadas — extremamente eficiente.
- **Inserção e remoção:** O(log N) com divisão ou fusão de nós quando necessário.

Uma AVL ou Rubro-Negra seria inadequada aqui não por complexidade assintótica, mas por **granularidade de acesso**: cada nó armazenaria apenas uma chave, desperdiçando a capacidade de I/O do disco.

---

## Parte 4 – Comparação entre Estruturas

| Estrutura   | Nº Máximo de Filhos                       | Balanceamento                                                                                                                              | Complexidade de Busca                                           | Complexidade de Inserção                    | Vantagem Principal                                                                                  | Desvantagem Principal                                                                      | Exemplo de Aplicação                                                                        |
| ----------- | ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------- | ------------------------------------------- | --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------- |
| BST         | 2                                         | Não possui balanceamento automático. Pode ficar desbalanceada conforme a ordem de inserção.                                                | O(log n) no melhor caso e O(n) no pior caso                     | O(log n) no melhor caso e O(n) no pior caso | Estrutura simples de entender e implementar                                                         | Pode perder desempenho completamente se ficar desbalanceada (degrada para lista encadeada) | Árvores de busca básicas e fins didáticos                                                   |
| AVL         | 2                                         | Sim. Mantém fator de balanceamento entre -1 e +1 em todos os nós, usando rotações simples e duplas após toda inserção ou remoção.          | O(log n) garantido                                              | O(log n) garantido                          | Busca mais rápida e previsível entre as binárias, ideal para leitura intensiva                      | Inserção e remoção mais custosas por exigirem mais rotações que a Rubro-Negra              | Índices em memória e estruturas que exigem buscas frequentes                                |
| Rubro-Negra | 2                                         | Sim. Usa coloração (vermelho/preto) e rotações para manter o balanceamento aproximado, garantindo que a altura seja no máximo 2\*log(n+1). | O(log n) garantido                                              | O(log n) garantido                          | Balanceamento eficiente com menos rotações médias que a AVL; melhor para operações mistas           | Busca ligeiramente mais lenta que a AVL; implementação mais complexa de escrever e depurar | std::map em C++, Java TreeMap, escalonador do kernel Linux                                  |
| N-ária      | N filhos (N > 2, definido pela aplicação) | Depende da variação. Árvores B e B+ possuem balanceamento automático garantido. Outras variações podem não possuir.                        | O(log N n) em estruturas balanceadas, onde N é o grau da árvore | O(log N n) com possível divisão de nós      | Altura muito menor para grandes volumes de dados; eficiente para acesso a disco (cada nó = 1 bloco) | Implementação mais complexa; gerenciamento de divisão/fusão de nós é trabalhoso            | Sistemas de arquivos (NTFS, HFS+), índices de bancos de dados (B+ Tree no PostgreSQL/MySQL) |

### Explicação das informações

**BST:** a ausência de balanceamento automático é o ponto crítico. Em um caso desfavorável — como inserir elementos já ordenados — a BST degenera em uma lista encadeada, com complexidade O(n) para todas as operações. Seu uso é justificado apenas em contextos didáticos ou quando se tem garantia de que os dados chegarão em ordem aleatória.

**AVL:** o fator de balanceamento rígido (entre -1 e +1) garante a menor altura possível entre as árvores binárias balanceadas, tornando-a ideal para aplicações onde buscas são muito mais frequentes do que inserções/remoções. O custo extra nas modificações é compensado pela velocidade de acesso.

**Rubro-Negra:** representa um compromisso prático. O balanceamento menos rígido que a AVL reduz o número médio de rotações em inserções e remoções, tornando-a mais eficiente para cargas de trabalho mistas. Por isso é a escolha predominante em implementações de bibliotecas padrão de linguagens de programação, onde não se sabe com antecedência o padrão de uso.

**N-ária:** a vantagem central não é a complexidade assintótica em si, mas a **constante oculta na notação Big-O**. Ao aumentar N, a altura da árvore cai drasticamente e cada nó pode ser carregado em um único acesso a disco, tornando a estrutura ordens de magnitude mais eficiente do que as binárias para volumes de dados que não cabem inteiramente na memória RAM.
