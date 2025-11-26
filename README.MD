# Formação Continuada: Utilizando ferramentas de IA na educação

## Autor: Marcos Aparecido Chaves Ferreira

Este repositório reúne links e tutoriais sobre **IA generativa** e como aplicá-la de forma prática na atividade docente.

---

## Chat para Cursos do IFSP Catanduva (GPTs Custom)

A seguir estão exemplos de espaços criados com IA para consulta sobre os cursos do IFSP Catanduva.
O docente pode adaptar a ideia para sua disciplina: basta inserir seus materiais e permitir que os alunos façam perguntas diretamente às LLMs.

Esses chats utilizam **RAG (Retrieval-Augmented Generation)**, sem necessidade de programação.

### Acessos

* **Grok:** [https://grok.com/project/dab8f908-ed7a-49fb-ab0d-ea7aa260fc94?tab=attachments](https://grok.com/project/dab8f908-ed7a-49fb-ab0d-ea7aa260fc94?tab=attachments)
* **ChatGPT:** [https://chatgpt.com/g/g-691e570c7fcc8191888a7aa1aad70999-ia-aplicando-em-cursos-do-ifsp-catanduva](https://chatgpt.com/g/g-691e570c7fcc8191888a7aa1aad70999-ia-aplicando-em-cursos-do-ifsp-catanduva)
* **Gemini:** [https://gemini.google.com/gem/1cKeOnNLbSMtJf1QfSisiJJ3wU6if9amr?usp=sharing](https://gemini.google.com/gem/1cKeOnNLbSMtJf1QfSisiJJ3wU6if9amr?usp=sharing)
* **Perplexity:** [https://www.perplexity.ai/spaces/cursos-ifsp-catanduva-jPlVTyBkTuqF2zv_YTO7Ag#0](https://www.perplexity.ai/spaces/cursos-ifsp-catanduva-jPlVTyBkTuqF2zv_YTO7Ag#0)

> **Observação:** Entre as quatro plataformas, **Grok** é a única que permite criar “Projetos” gratuitamente e compartilhá-los publicamente.

---

<img src=https://github.com/user-attachments/assets/9d0470cc-bedc-4f12-843b-fa2bcbf2cca6>

## Tópicos da palestra

- <a href=https://github.com/mchavesferreira/EducacaoIA/blob/main/Artigo_DeepResearch_IA_na_Educacao.md>Artigo sobre o uso da IA na Educação</a>

- <a href=https://github.com/mchavesferreira/EducacaoIA/blob/main/gpt_gpu/transformers.md> O que são Transformers</a>

- <a href=https://github.com/mchavesferreira/EducacaoIA/blob/main/gpt_gpu/explicandoRag.md>O que é RAG?</a>

- <a href=https://github.com/mchavesferreira/EducacaoIA/blob/main/gpt_gpu/exemplos_treinamento_finetunning.md>O que é FINE-TUNING</a>

- <a href=https://github.com/mchavesferreira/EducacaoIA/blob/main/gpt_gpu/gpu_npu.md>Diferenças entre CPU, GPU, NPU</a>

- <a href=https://github.com/mchavesferreira/EducacaoIA/blob/main/promptseducacao/persona.md>Utilizando Personas para Aprendizagem com ChatGPT</a>

- <a href=https://github.com/mchavesferreira/EducacaoIA/blob/main/promptseducacao/tags.md> Tags de Prompt para IA</a>

- <a href=https://github.com/mchavesferreira/EducacaoIA/blob/main/promptseducacao/busca_professores.md>Guia de prompts utilizados para construção dos GPTs Custom/Projetos</a>

- <a href=https://github.com/mchavesferreira/EducacaoIA/blob/main/promptseducacao/guia_prompt_chatgpt5.1.md> Orientações para o novo chatgpt 5.1


## Crie infográficos e Slides com figuras customizadas

A ferramenta **NotebookLM** ganhou destaque nas notícias no final de novembro de 2025 com o lançamento de um novo recurso: a criação automática de **infográficos** e **apresentações** utilizando imagens geradas pelo modelo **NanoBanana**, o novo gerador multimodal da plataforma.<a href=https://notebooklm.google/> Experimente a novidade</a> e explore suas possibilidades.

## Boas Práticas de Prompt (resumo)

**Objetivo:** obter respostas úteis, verificáveis e no formato desejado, com o mínimo de retrabalho.

### Estrutura rápida de prompt

Use frases curtas e diretas:

1. **Eu sou:** defina seu perfil/nível.
2. **Preciso de:** descreva o objetivo.
3. **Contexto:** dados essenciais e limites.
4. **Formato da resposta:** tabela, passos, checklist, código, etc.
5. **Fontes/Verificação:** peça conferência de fatos.
6. **Tom/Idioma:** formal, simples, técnico, PT-BR, etc.
7. **Critérios de qualidade:** sem invenções, com passos claros.

Melhore pedindo ao próprio a própria LLM direcionar o prompt acima para o o asssunto desejado.

Exemplo:

<details><summary>Clique para ver um exemplo de geografia</summary>

```

Eu sou: professor de Geografia do IFSP–Catanduva, atuando no 2º ano do Curso Técnico em Química Integrado ao Ensino Médio.

Preciso de: um plano de aula de 30 a 40 minutos para um tema específico da ementa de Geografia do 2º ano (por exemplo: uso e ocupação do solo, indústria e impactos ambientais, dinâmica climática regional, bacias hidrográficas, urbanização e poluição, etc.).

Contexto:
• Turma iniciada na integração entre Geografia e Química.
• Aula sem computadores, com materiais simples.
• Necessário articular processos territoriais (Geografia) com conteúdos químicos presentes nas disciplinas técnicas do 2º ano (Microbiologia e Bioquímica, Química Geral Experimental, Química Analítica Qualitativa e Quantitativa).
• Considerar o território de Catanduva e região (bacias, uso do solo, relevância industrial).

Responda desta forma:

Ementa resumida (2–3 linhas) baseada no PPC integrado (Geografia).

5 objetivos de aprendizagem articulando fenômenos espaciais e processos químicos/ambientais.

3 atividades práticas de baixo custo, sem laboratório.

Checklist de avaliação: indicadores observáveis.

Competências/habilidades alinhadas ao PPC (Geografia + formação técnica), incluindo a carga horária prevista para o encontro.

Fontes/Verificação:
Use exclusivamente o PPC do Técnico em Química Integrado ao EM do IFSP–Catanduva. Quando possível, indique trechos da ementa ou da abordagem ambiental do PPC, como:
– Integração Geografia + Química na discussão das dinâmicas da natureza, impactos ambientais e uso do território.

Tom/Idioma: formal, PT-BR.

Regras:
• Não inventar conteúdos fora do PPC.
• Confirmar conceitos básicos (ciclos naturais, impactos ambientais, territorialidades).
• Relacionar Geografia aos conteúdos químicos pertinentes do 2º ano sempre que possível.
• Refletir 1 minuto antes de estruturar a resposta.

```

</details>



## **Como evitar perdas de contexto e minimizar alucinações durante uma conversa com IA**

### **1. Guarde manualmente o texto importante**

Para não depender apenas da janela de contexto da IA, copie e salve os trechos essenciais (instruções, tabelas, listas, resumos) em um arquivo Markdown `.md`.
Assim, você pode **retomar o trabalho** mais tarde sem risco de perder nada.

> *Recomendação:* Sempre peça ao LLM:
> **“Resuma e organize o conteúdo essencial em Markdown para eu salvar.”**


### **2. Exporte respostas em Markdown (.md)**

Use o formato Markdown para armazenar:

* resumos de projetos
* especificações
* scripts
* orientações importantes
* prompts mestres

O LLM gera o texto já formatado; você apenas salva como `.md`.

---

### **3. Exporte dados e tabelas em JSON**

Sempre que trabalhar com listas, tabelas, banco de dados ou estruturas que serão reutilizadas:

* peça para gerar **um único JSON completo**,
* ou peça para **unificar vários blocos em um único arquivo**.



