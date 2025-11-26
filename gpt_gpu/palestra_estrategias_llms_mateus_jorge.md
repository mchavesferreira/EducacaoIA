# 🧠 Adaptações e Estratégias de Uso de LLMs

## 📖 Introdução: O que é uma LLM?

<img width="610" height="384" alt="Captura de tela 2025-11-25 224210" src="https://github.com/user-attachments/assets/9d0470cc-bedc-4f12-843b-fa2bcbf2cca6" />

<img width="587" height="364" alt="Captura de tela 2025-11-25 224219" src="https://github.com/user-attachments/assets/7ae06e22-7f24-4616-87f2-fa45d535d640" />

As **Large Language Models (LLMs)** são modelos de linguagem baseados em aprendizado profundo capazes de **prever a próxima palavra** de uma sequência de texto.  
Formalmente, elas calculam a **probabilidade condicional** de uma sequência de tokens:

\[
P(w_1, w_2, ..., w_n) = \prod_i P(w_i | w_1, ..., w_{i-1})
\]

Isso significa que, dado o contexto anterior, o modelo tenta prever o token seguinte.

### 🧩 Exemplo:
Sentença:  
> “O gato está entrando no …”

Possíveis previsões:  
- quarto ✅  
- cozinha ❌  
- papagaio ❌  

A LLM atribui maior probabilidade à palavra **“quarto”**, pois o contexto torna essa previsão mais plausível.

---

## 🕰️ Histórico da Evolução dos Modelos de Linguagem

### 🔹 Recurrent Neural Networks (RNNs)
As RNNs processam sequências passo a passo, armazenando informações anteriores em um **estado oculto (h)**.  
Porém, sofrem com o **problema da memória de longo prazo**, pois esquecem informações antigas em sentenças longas.

- **Vantagem:** Boa para séries temporais curtas.  
- **Limitação:** Cálculo sequencial — não aproveita paralelismo de GPU.

### 🔹 Long Short-Term Memory (LSTM) e GRU
Surgiram para mitigar a perda de memória, mas ainda mantinham dependência sequencial.

---

## ⚡ A Revolução dos Transformers (2017)

O paper **“Attention is All You Need” (Vaswani et al., Google, 2017)** substituiu a recorrência pela **atenção**.

### 🎯 Conceito de Atenção:
A rede aprende **aonde olhar** na sentença para prever a próxima palavra.  
Cada token se relaciona com todos os outros simultaneamente — o que permite **processamento paralelo** e **memória de longo prazo**.

### 🧠 Self-Attention:
Cada palavra se relaciona consigo mesma e com as demais, aprendendo **relações internas** (gramaticais e semânticas).

### 🧮 Multi-Head Attention:
O modelo aprende múltiplas projeções (“cabeças”), cada uma capturando diferentes tipos de relação — semântica, sintática, léxica etc.

---

## 🚀 Do Transformer ao GPT e BERT

| Modelo | Ano | Aplicação | Característica |
|--------|-----|------------|----------------|
| **GPT (OpenAI)** | 2018 | Geração de texto | Treinamento unidirecional |
| **BERT (Google)** | 2019 | Análise de texto | Treinamento bidirecional |

Ambos introduziram o **pré-treinamento**: aprender a linguagem antes de resolver tarefas específicas.

---

## 💡 O Salto das LLMs: ChatGPT e Modelos Gigantes

A evolução de **GPT-3 e GPT-4** introduziu bilhões de parâmetros e **Feedback Humano (RLHF)**, tornando as respostas mais “humanas” e contextuais.

> O grande diferencial do ChatGPT não foi o tamanho, mas o **feedback humano** e a interface de conversção.

---

## 🧮 Aplicações Práticas de LLMs

### 🗂️ Exemplo: OCR Tradicional vs. LLM

**Antes:**  
1. Coleta de dados  
2. Rotulação manual  
3. Treinamento e avaliação

**Agora com LLM:**  
Uma imagem de uma placa de motor pode ser enviada diretamente para a LLM multimodal, que extrai a potência em segundos — sem treinamento adicional.

---

## 🎨 Prompt Engineering

### 🧠 Definição:
Arte de estruturar **instruções (prompts)** para obter respostas precisas, sem alterar os parâmetros do modelo.

### 📋 Exemplo ruim:
> “Resuma este texto.”

### 📋 Exemplo bom:
> “Resuma o texto abaixo em 3 frases, destacando os principais argumentos jurídicos em linguagem simples.”

### 🔧 Boas Práticas:
- Seja **específico** sobre o formato da resposta (JSON, tabela, texto).  
- **Defina papéis:** “Você é um professor de eletrônica...”  
- Use **exemplos** (few-shot).  
- Limite o tamanho com **max tokens**.  

---

## 🔥 Parâmetros Importantes na API

| Parâmetro | Descrição | Recomendações |
|------------|-------------|---------------|
| **Temperature** | Controla criatividade (0 = previsível, 1 = criativo) | 0 para cálculos, 1 para ideias |
| **Top-K / Top-P** | Limita as palavras candidatas | Reduz alucinações e latência |
| **Max Tokens** | Limite de saída | Evita custos e respostas longas |

---



## 🧩 Técnicas de Prompt Engineering

### 1. **General Prompting /Zero Shot**
Pedir a tarefa diretamente, sem exemplos.  
> “Classifique este texto como positivo, negativo ou neutro.”


### 2. **Single /Few Shot**
Fornece exemplos no prompt entrada/saida (ou apenas um).  
> Entrada: “Adorei o atendimento.” → Saída: “Positivo”

### 3. **Step-Back Prompting**
Criar um contexto intermediário.  
> “Liste os tipos de jogos FPS antes de propor um novo enredo.”


### 4. **Chain-of-Thought (CoT)**
Pedir que o modelo **pense passo a passo**.  
> “Explique o raciocínio antes de dar a resposta final.”



### 5. **Pipeline Prompts**
Dividir uma tarefa grande em **etapas menores** — ideal para produção.



## 🔍 Recuperação de Contexto: RAG (Retrieval Augmented Generation)

### 🧩 Conceito:
Combina **busca semântica** + **geração textual**.

### ⚙️ Etapas:
1. **Carregar dados (Data Loading)**  
   - Extrair texto de PDFs, imagens (OCR), vídeos (transcrição).  
2. **Chunking**  
   - Dividir texto em partes menores (ex: 1000 tokens).  
3. **Criação de Embeddings**  
   - Mapear cada trecho para um vetor numérico.  
4. **Armazenamento em Banco Vetorial**  
   - Ex: ChromaDB, PostgreSQL + PGVector, Weaviate.  
5. **Busca por Similaridade (Cosine Distance)**  
   - Localiza os trechos mais próximos da pergunta.  
6. **Geração com Contexto**  
   - Envia os trechos relevantes para a LLM responder com precisão.

### 🧮 Exemplo:
Pergunta: “Qual a potência do motor WEG modelo X?”  
→ Busca semântica encontra o documento técnico correto  
→ LLM gera resposta citando a fonte.

---

## 🔺 Fine-Tuning (Ajuste Fino)

### 🎯 Definição:
Re-treinar parte de um modelo já existente para uma tarefa específica.

- Requer **dataset rotulado**  
- É **caro e lento**  
- Pode fazer o modelo “esquecer” o conhecimento anterior (**catastrophic forgetting**)

### 💰 Exemplo de Custo:
Treinamento de um modelo DeepSeek → 2 meses / 5 milhões USD.

### ⚙️ Técnicas Eficientes:
- **PEFT (Parameter-Efficient Fine-Tuning)**  
  Treina apenas parte do modelo (últimas camadas).  
- **LoRA (Low-Rank Adaptation)**  
  Adiciona pequenas matrizes ΔW para adaptar o comportamento, sem alterar os pesos originais.

> Vantagem: múltiplos LoRAs podem coexistir sobre o mesmo modelo base.


---

## ⚗️ Outras Estratégias Avançadas

| Técnica | Função | Observação |
|---------|--------|-------------|
| **Quantização** | Reduz precisão dos pesos (ex: FP32 → INT8) | Acelera inferência |
| **Distilação** | Treinar modelo menor a partir de um modelo grande | Economiza GPU |
| **Mixture of Experts (MoE)** | Divide o modelo em especialistas ativados sob demanda | Reduz custo de inferência |
| **GraphRAG** | Conecta documentos por relações semânticas (ontologia) | Ideal para bases corporativas |
| **Re-Ranker** | Reordena resultados da busca pelo melhor contexto | Melhora precisão no RAG |


- Quantização

<img width="1080" height="1115" alt="quantizacao" src="https://github.com/user-attachments/assets/c800c8f6-eaa1-4ba0-a83d-3155c94bc4c1" />

- Destilação
<img width="799" height="402" alt="destilação" src="https://github.com/user-attachments/assets/2fd1aefe-ae8f-4334-91eb-c5c96aaf3381" />


- Mixture of Experts (MoE)
<img width="535" height="444" alt="moe" src="https://github.com/user-attachments/assets/508fada0-8473-4444-93aa-cbc3587b908d" />



- GraphRAG/
 
![graph_rag](https://github.com/user-attachments/assets/c41c8b5b-ce4c-4f39-aa89-e49d0c958721)

- Re-Ranker
<img width="1185" height="458" alt="Reranking_after_initial_vector_search" src="https://github.com/user-attachments/assets/00b44870-8eb3-4476-913f-94decf1e325c" />

- 
---

## 🦯 Agentes e MCP (Model Context Protocol)

### 🤖 Agentes
São LLMs capazes de **tomar decisões e executar ações** (como chamar APIs ou outros agentes).

### 🔗 MCP
Protocolo que permite a um agente **descobrir automaticamente** quais rotas e funções externas estão disponíveis.

---

## 🏢 LLMs no Contexto Industrial

Segundo a experiência da **Tractian**, o uso ideal ocorre em:
- Dados **não estruturados** (documentos, relatórios, áudios, OCRs).
- Processos de **análise preliminar e prototipagem rápida**.
- Suporte a **engenheiros de manutenção** e **diagnóstico textual**.

> Para análise espectral e sinais de sensores, métodos tradicionais (estatística + ML clássico) ainda são mais adequados.

---

## 💬 Boas Práticas Gerais

1. **Comece simples** — otimize depois.  
2. **Documente** o raciocínio de cada prompt.  
3. **Prefira modularidade** — divida tarefas complexas.  
4. **Use RAG antes de Fine-Tuning.**  
5. **Controle custos** — tokens = tempo + dinheiro.  
6. **Verifique fontes** — LLMs não garantem veracidade.  
7. **Evite acoplamento a um banco ou API.**

---

## 📚 Referências Recomendadas

- Vaswani et al. (2017) – *Attention Is All You Need*  
- Devlin et al. (2018) – *BERT: Pre-training of Deep Bidirectional Transformers*  
- Brown et al. (2020) – *Language Models are Few-Shot Learners (GPT-3)*  
- DeepSeek Team (2023) – *Open Source Efficient Training Techniques*  
- Google (2024) – *Prompt Design Guide for Gemini*  
- Hugging Face Docs – *PEFT & LoRA Implementations*

---

## 🧒‍⚖️ Conclusão

Os **modelos de linguagem** revolucionaram o aprendizado de máquina ao eliminar boa parte da barreira de dados e permitir que tarefas complexas fossem resolvidas **com simples prompts**.

> O futuro está na combinação: **LLMs + RAG + Fine-Tuning seletivo + Engenharia de Prompts.**

