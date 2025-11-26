

### 📚 O que é **RAG**?

**RAG** significa **Retrieval-Augmented Generation**, ou em português, **Geração Aumentada por Recuperação**.

É uma técnica usada para melhorar a geração de texto de modelos como o GPT, **combinando duas coisas**:

1. **Recuperação de informações (Retrieval)** — buscar dados em uma base de conhecimento.
2. **Geração de texto (Generation)** — usar um modelo como o GPT para gerar a resposta.

---

### 🧠 Como funciona o RAG?

Pense assim:

> O modelo **não sabe de tudo**, então quando ele recebe uma pergunta, ele pode **procurar a resposta** em documentos, sites ou bases de dados antes de responder.

#### Passos do RAG:

1. 🕵️ **Busca**: O sistema usa palavras da sua pergunta para **buscar informações relevantes** em uma base de dados (por exemplo, documentos, PDFs, Wikipédia, etc).
2. 🧩 **Combina**: Ele **seleciona os trechos mais úteis**.
3. ✍️ **Gera a resposta**: Com esses trechos, o modelo **usa o GPT** (ou outro modelo de linguagem) para **gerar uma resposta mais precisa e atualizada**.

---

### 🧪 Exemplo prático

Se você perguntar:

> "Qual é a previsão do tempo para amanhã em Lisboa?"

* Um modelo normal como o GPT pode não saber, porque seu conhecimento tem limite de tempo.
* Um sistema com **RAG** pode **buscar online ou em uma base atualizada**, encontrar a previsão e depois **gerar uma resposta natural** com base nisso.

---

### ✅ Vantagens do RAG

* Usa **conhecimento atualizado**.
* Permite que o modelo **responda com base em dados externos**, como documentos da sua empresa.
* Reduz o risco de **alucinação** (quando o modelo "inventa" respostas).

---


<img width="974" height="652" alt="image" src="https://github.com/user-attachments/assets/6e10f24c-6ac9-4e1b-b366-85110614a5c8" />
**arquitetura de ingestão de dados no sistema RAG (Retrieval-Augmented Generation)**. A seguir, um resumo das etapas ilustradas:

### 🔍 Etapas da Ingestão no RAG:

1. **Entrada de Arquivos**:
   Arquivos em diversos formatos (PDF, TXT, HTML, sites etc.) são usados como fonte de informação.

2. **Divisão dos Dados**:
   Os arquivos são **divididos em pedaços menores** (chunks) para facilitar o processamento.

3. **Geração de Embeddings**:
   Um **modelo de embeddings** transforma os pedaços de texto em **vetores numéricos** que representam seu significado semântico.

4. **Armazenamento Vetorial**:
   Os vetores são salvos em um **banco de dados vetorial**, permitindo buscas por similaridade.

5. **Criação do Índice de Busca**:
   O banco de dados vetorial gera um **índice de busca**, que será usado para **recuperar informações relevantes** durante a geração de respostas pelo modelo RAG.

---

💡 Em resumo: **RAG Ingestão** envolve extrair, dividir, vetorização com embeddings e armazenamento em banco vetorial para permitir consultas semânticas em tempo de execução.



<img width="974" height="652" alt="image" src="https://github.com/user-attachments/assets/0dde5730-0554-4432-a7ec-979afbac413a" />

Este slide mostra a segunda parte da **Arquitetura RAG (Retrieval-Augmented Generation)**, focada nas etapas de **recuperação e geração** de respostas. A seguir, o resumo das etapas ilustradas:

---

### 🧠 Etapas da Recuperação e Geração no RAG:

1. **Entrada da Consulta (Query)**:
   O usuário faz uma pergunta ou consulta (query).

2. **Conversão da Query em Embedding**:
   A query é processada por um **modelo de embedding**, que a transforma em um **vetor de sentido semântico**.

3. **Busca Semântica no Banco Vetorial**:
   Esse vetor é usado para buscar no **índice de busca vetorial** dentro do **banco de dados vetorial**, retornando as **Top K sentenças mais relevantes** (mais semelhantes).

4. **Combinação da Query + Sentenças Recuperadas**:
   A consulta original é combinada com os **trechos recuperados** do banco vetorial.

5. **Geração da Resposta pelo LLM**:
   Um **modelo de linguagem (LLM)** recebe essa combinação e gera uma **resposta final** para o usuário.

---

💡 Em resumo: **RAG Recuperação e Geração** transforma a pergunta do usuário em um vetor, recupera os trechos mais relevantes e gera a resposta com base na combinação da pergunta e contexto, aumentando a precisão e confiabilidade.

