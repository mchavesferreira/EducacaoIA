# Transformers



---

### 🧠 O que é *Transformer*?

**Transformer** é uma **arquitetura de rede neural** criada pelo time do Google em 2017. Ela revolucionou a área de **inteligência artificial** e **processamento de linguagem natural (NLP)**. É a base de modelos como **GPT**, **BERT**, e outros modelos modernos de linguagem.

---

### 🧩 Como funciona, de forma simples?

Antes do Transformer, os modelos usavam sequências (um por um, como uma fila). O Transformer, por outro lado, olha **todas as palavras ao mesmo tempo** e decide **quais palavras são mais importantes** em cada contexto.

Imagine que você está lendo a frase:

> “O cachorro mordeu o carteiro porque ele estava assustado.”

Quem estava assustado? O cachorro ou o carteiro? 🤔
O Transformer tenta entender isso olhando o **contexto completo** da frase, e não só palavra por palavra.

---

### 🛠️ Peças principais do Transformer

1. **Attention (atenção)**:
   Esta é a mágica! O mecanismo de *attention* faz com que o modelo “preste atenção” mais nas palavras que realmente importam.
   Ex: Se a frase fala sobre um "gato", o modelo vai focar mais em palavras relacionadas a ele.

2. **Encoder e Decoder**:

   * O **Encoder** lê e entende a entrada (por exemplo, uma frase).
   * O **Decoder** usa essa informação para gerar uma resposta (por exemplo, traduzir ou responder).

   No caso do **GPT**, só se usa o **Decoder**, porque ele é um modelo de geração de texto.

---

### Visualização gráfica sobre transformes

*  https://bbycroft.net/llm

*  https://poloclub.github.io/transformer-explainer/


### 🎯 Por que o Transformer é tão poderoso?

* Ele entende o **contexto** de forma mais profunda.
* Ele é mais rápido para treinar (usa paralelismo).
* Ele funciona muito bem com **textos longos**.

---

Se quiser, posso mostrar um exemplo visual de como o "attention" funciona ou comparar Transformer com modelos antigos. Quer ver isso?

O modelo GPT (Generative Pre-trained Transformer) é baseado na arquitetura Transformer, que foi originalmente proposta em um artigo chamado *Attention is All You Need* (Vaswani et al., 2017).

Os Transformers, como o usado no GPT, utilizam um mecanismo de atenção que permite ao modelo focar em diferentes partes de uma sequência de entrada ao fazer previsões, em vez de processar os dados sequencialmente, como fazem redes neurais recorrentes (RNNs). Isso torna o Transformer muito eficiente para lidar com grandes volumes de dados, como textos.

A arquitetura **Transformer** é composta por duas partes principais: o **Encoder** e o **Decoder**. No caso de modelos como o **GPT**, apenas a parte do **Decoder** é utilizada. Vamos passar por cada uma dessas etapas e detalhar os componentes principais de um Transformer.

### 1. **Estrutura Básica de um Transformer**

O Transformer, conforme proposto por Vaswani et al. (2017), é composto por **camadas empilhadas de Encoder** e **camadas empilhadas de Decoder**. A arquitetura básica é:

* **Encoder** (apenas utilizado em modelos como BERT, T5, etc.)

  * Recebe a entrada (texto) e gera representações internas.
* **Decoder** (utilizado no GPT, entre outros)

  * Gera a saída (como texto gerado), utilizando as representações do Encoder (no caso do GPT, não há um Encoder externo, mas a arquitetura de Decoder é usada para prever a sequência de saída).

Para modelos como o **GPT**, utilizamos apenas o **Decoder**, que é empilhado várias vezes. A sequência de operações dentro de cada bloco de Decoder é:

### 2. **Componentes principais do Transformer**

#### 2.1 **Embedding**

* **Token Embedding**: Cada palavra (ou subpalavra) da sequência de entrada é mapeada para um vetor de números. Esses vetores são chamados de embeddings.
* **Positional Encoding**: Como o Transformer não tem estrutura sequencial como as RNNs, precisamos de uma maneira de adicionar informações sobre a posição das palavras na sequência. O Positional Encoding fornece isso, com um vetor para cada posição na sequência, que é somado aos embeddings dos tokens.

#### 2.2 **Camada de Atenção (Self-Attention)**

A **Atenção** é o núcleo da arquitetura Transformer. A cada camada, o modelo calcula a relação de cada palavra em relação a todas as outras na sequência. Para isso, usamos três componentes:

* **Queries (Q)**, **Keys (K)** e **Values (V)**: Para cada token de entrada, geramos três vetores diferentes (Q, K e V). Esses vetores são obtidos através de multiplicação da matriz de entrada pelos pesos aprendidos durante o treinamento.

* **Atenção Escalonada (Scaled Dot-Product Attention)**: A atenção calcula uma pontuação entre todas as palavras de entrada, usando os vetores **Query** e **Key**. O resultado é uma ponderação que indica a importância de cada palavra para a palavra de interesse.

  A fórmula da atenção é dada por:
  [
  \text{Atenção}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right) V
  ]
  Onde:

  * ( Q ) é o vetor de consulta (Query).
  * ( K ) é o vetor de chave (Key).
  * ( V ) é o vetor de valor (Value).
  * ( d_k ) é a dimensionalidade do vetor de chave.

A atenção permite ao modelo capturar dependências de longo alcance entre as palavras, sem precisar de uma ordem sequencial explícita.

#### 2.3 **Atenção Multi-Cabeça (Multi-Head Attention)**

Em vez de usar apenas uma única atenção, o Transformer utiliza múltiplas cabeças de atenção em paralelo. Isso permite que o modelo capture diferentes aspectos da relação entre palavras de diferentes "perspectivas". As múltiplas cabeças de atenção geram diferentes representações, que depois são concatenadas e transformadas.

#### 2.4 **Camada Feed-Forward**

Após a camada de atenção, cada token passa por uma camada feed-forward totalmente conectada. Esse componente é composto por duas camadas lineares (densas) com uma função de ativação no meio (tipicamente ReLU). A camada feed-forward é aplicada de forma independente a cada token.

A fórmula da camada feed-forward é:
[
\text{FFN}(x) = \max(0, xW_1 + b_1)W_2 + b_2
]
Onde:

* ( x ) é a entrada da camada.
* ( W_1, W_2 ) são as matrizes de pesos.
* ( b_1, b_2 ) são os vieses.

#### 2.5 **Normalização e Resíduos**

* **Layer Normalization**: Após as operações de atenção e feed-forward, as saídas passam por uma normalização de camada para estabilizar o treinamento.
* **Residual Connection**: Para garantir que o modelo não perca informações importantes durante o processamento, é adicionada uma conexão residual entre a entrada e a saída de cada subcamada (atenção ou feed-forward). Ou seja, a entrada original de cada subcamada é somada à sua saída antes de ser passada para a próxima subcamada.

#### 2.6 **Camada de Saída**

No caso do GPT (que é um modelo de geração de texto), a camada final do modelo é uma **camada de projeção linear** que mapeia as representações internas para uma distribuição de probabilidade sobre o vocabulário. Essa camada é usada para prever o próximo token na sequência.

### 3. **Fluxo de Dados no Transformer**

1. **Entrada**: O modelo recebe uma sequência de palavras (tokens).
2. **Embeddings + Positional Encoding**: Cada palavra é transformada em um vetor denso através do embedding, e a posição é codificada através do positional encoding.
3. **Camadas de Decoder** (empilhadas):

   * **Atenção Multi-Cabeça**: Cada token da sequência é processado em relação aos outros tokens.
   * **Camada Feed-Forward**: Após a atenção, cada token passa por uma rede neural feed-forward.
   * **Normalização e Residual**: A entrada da camada é somada à sua saída e normalizada.
4. **Saída**: O modelo gera uma distribuição de probabilidade para o próximo token, que é usado para fazer previsões ou gerar texto.

### 4. **Resumo do Fluxo em um Transformer**

* A entrada (sequência de tokens) é convertida em embeddings.
* A cada camada de Decoder, o modelo realiza:

  * **Atenção** (multi-cabeça) para calcular as dependências entre os tokens.
  * **Feed-forward** para processar as representações dos tokens.
  * **Normalização e residuals** para estabilidade no treinamento.
* O modelo gera as saídas (como probabilidades para o próximo token, no caso de geração de texto).

### Resumo para o GPT

O **GPT** (Generative Pretrained Transformer) utiliza apenas o **Decoder** do Transformer. Durante o pré-treinamento, ele aprende a prever o próximo token em uma sequência, com base no contexto anterior. A estrutura de cada bloco do Decoder é composta por:

* **Atenção Multi-Cabeça**,
* **Camada Feed-Forward**,
* **Normalização e Conexões Residuais**.

Esse fluxo é repetido por várias camadas de Decoder para gerar representações cada vez mais complexas e, finalmente, prever a sequência de saída.

## prompt

Ótimas perguntas! Vamos esclarecer esses pontos em detalhes.

### 1. **Uso do Encoder e Decoder no GPT-3.5**

Quando as notícias falam que o **ChatGPT 3.5** ou outro modelo GPT "consultou bilhões de informações da internet" para seu treinamento, isso se refere ao processo de **pré-treinamento** do modelo, onde ele foi alimentado com uma grande quantidade de dados textuais extraídos da web, livros, artigos, entre outros. Durante esse treinamento, o GPT **não usou o Encoder**, mas sim o **Decoder** da arquitetura Transformer.

Aqui está o processo de treinamento de forma simplificada:

* **Pré-Treinamento**: Durante o treinamento do GPT-3.5, o modelo foi alimentado com enormes volumes de dados textuais. O objetivo era fazer com que ele aprendesse a prever o próximo token (palavra, por exemplo) em uma sequência de texto. Esse é um processo **autoregressivo** (onde o modelo usa os tokens anteriores para prever o próximo). Para isso, ele usa **somente o Decoder** da arquitetura Transformer.

  * **Decoder**: Durante o treinamento, o modelo GPT-3.5 é alimentado com um texto e, com base nesse texto, ele tenta prever o próximo token. O modelo é treinado para minimizar a diferença entre a previsão que ele fez e o token real da sequência.
* **Pesos e Parâmetros**: Durante esse treinamento, o modelo aprende **pesos** para as operações dentro do Decoder (como as camadas de atenção e feed-forward), que são ajustados para capturar padrões de linguagem e associações entre palavras. Esses pesos são fundamentais para o modelo e são o que ele utiliza para gerar respostas durante a inferência.

  Ou seja, **o GPT-3.5 não usou o Encoder** para esse treinamento. Ele usou **apenas o Decoder** para aprender a prever sequências de texto com base em um contexto dado.

### 2. **O que representa o "prompt" em um modelo LLM (Large Language Model)?**

No contexto de modelos de linguagem grandes como o **GPT-3.5**, o **prompt** é o texto de entrada que você fornece ao modelo para gerar uma resposta ou realizar uma tarefa. Ele pode ser uma pergunta, uma instrução ou qualquer sequência de texto que forneça contexto para o modelo.

Quando você faz uma pergunta para o GPT, por exemplo, o texto que você digita é chamado de **prompt**. Esse prompt serve como **entrada** para o modelo durante o processo de **inferência** (geração de texto).

#### Como funciona o prompt no processo de inferência:

1. **Entrada do Prompt**: O prompt que você fornece é a entrada para o modelo, ou seja, a sequência de texto que o modelo usa como contexto.

2. **Processamento pelo Decoder**: O modelo então passa esse prompt através do **Decoder**, que, usando os **pesos** que aprendeu durante o treinamento, tenta prever a sequência de tokens (palavras, frases) que provavelmente se seguiria ao prompt.

3. **Geração de Resposta**: Com base no prompt e no que foi aprendido durante o treinamento (os pesos e padrões de linguagem), o modelo gera uma resposta ou sequência de tokens que faz sentido dentro do contexto do prompt fornecido.

Então, o **prompt** representa o **contexto de entrada** para o modelo, que utiliza o Decoder para gerar uma saída com base nesse contexto.

### Resumo:

* O GPT-3.5 usa **somente o Decoder** para tanto o pré-treinamento quanto a inferência. Durante o treinamento, o modelo aprende os pesos para prever o próximo token com base no contexto dos tokens anteriores.
* O **prompt** é a **entrada** que você fornece ao modelo durante a inferência, e é utilizado para guiar o modelo a gerar uma resposta apropriada com base no que foi aprendido durante o treinamento.

