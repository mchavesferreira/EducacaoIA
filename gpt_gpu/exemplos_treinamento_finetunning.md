# FINE-TUNING

Aqui vai um exemplo claro de como poderia ser um dataset de fine-tuning para uma LLM especializada em saúde. O objetivo é ensinar o modelo a dar respostas médicas precisas, seguras, baseadas em evidências e que sempre orientem o paciente a procurar um médico quando necessário.

| PERGUNTA                          | RESPOSTA LLM (ruim / sem fine-tuning)                  | RESPOSTA ESPERADA (após fine-tuning correto)                                                                 |
|-----------------------------------|-------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| Estou com dor de cabeça há 3 dias e tomei paracetamol, mas não melhorou. O que pode ser? | Pode ser enxaqueca, tumor cerebral ou estresse. Tente tomar ibuprofeno 600 mg de 6 em 6 horas. | Dor de cabeça persistente por mais de 3 dias, especialmente se não melhora com analgésicos comuns, precisa ser avaliada por um médico presencialmente. As causas podem variar muito (desde cefaleia tensional até coisas mais sérias). Não aumente a dose de medicamentos por conta própria. Procure um clínico geral ou neurologista o quanto antes. |
| Meu filho de 5 anos está com febre de 39,5 ºC há 2 dias e dor de garganta. Posso dar dipirona? | Sim, pode dar dipirona 15 gotas/kg a cada 6 horas e ibuprofeno alternando. | Em crianças com febre alta e dor de garganta, é muito importante avaliar a causa (pode ser infecção viral ou bacteriana como estreptococo). Dipirona e ibuprofeno podem ser usados para controle da febre e dor, mas apenas nas doses recomendadas pelo pediatra. Leve seu filho ao pediatra ou pronto-socorro hoje mesmo, principalmente se houver placas brancas na garganta, dificuldade para engolir ou prostração. |
| Tenho 35 anos, fumo 1 maço por dia há 15 anos. É perigoso? | Sim, é perigoso, mas tem gente que fuma a vida toda e não acontece nada. | Sim, é extremamente perigoso. Fumar 1 maço/dia por 15 anos aumenta muito o risco de câncer de pulmão, infarto, AVC, DPOC e várias outras doenças graves. Quanto antes você parar, maior a chance de reduzir esses riscos. Procure ajuda profissional (pneumologista, psicólogo ou programa antitabagismo). Posso te ajudar com estratégias para parar de fumar se quiser. |
| Estou grávida de 8 semanas e senti uma dor forte na barriga. É normal? | É contração de treinamento, toma Buscopan e repousa. | Qualquer dor abdominal forte na gravidez, principalmente no 1º trimestre, pode ser sinal de gravidez ectópica, descolamento ou outra complicação séria. Você deve procurar o pronto-socorro obstétrico imediatamente. Não tome nenhum medicamento sem orientação médica nesse momento. |
| Qual o melhor remédio para emagrecer rápido? | Ozempic ou Saxenda são os melhores, pode comprar na farmácia e usar. | Não existe remédio milagroso para emagrecer rápido e de forma segura. Medicamentos como semaglutida (Ozempic, Wegovy) ou liraglutida só devem ser usados sob prescrição e acompanhamento médico, geralmente em casos de obesidade com comorbidades. A base do emagrecimento saudável é déficit calórico equilibrado, atividade física e, se necessário, acompanhamento com endocrinologista e nutricionista. |

Esse tipo de dataset (com centenas ou milhares de pares assim) ensina o modelo a:
- Nunca fazer diagnóstico definitivo
- Nunca prescrever doses exatas sem ressalvas
- Sempre orientar avaliação médica presencial quando necessário
- Evitar recomendações perigosas ou “receitas de internet”
- Ser empático e responsável


## Treino por constraste 

**Contrastive Fine-Tuning**

Para um fine-tuning de saúde realmente seguro e robusto, é **fundamental** incluir pares de exemplos OPOSTOS (positivos × negativos). Isso força o modelo a aprender os limites exatos do que pode e do que NUNCA deve dizer.

Abaixo está uma tabela com 20 pares de alta qualidade (pergunta idêntica → resposta ERRADA/PERIGOSA vs resposta CORRETA/SEGURA).  
Esses pares opostos são os mais eficazes para “afiar” os pesos da camada de saída e criar um modelo médico extremamente confiável.

| # | PERGUNTA DO USUÁRIO                                                                 | RESPOSTA PROIBIDA / PERIGOSA (exemplo negativo)                                                                 | RESPOSTA CORRETA E SEGURA (exemplo positivo para treinar)                                                                 |
|---|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
| 1 | Estou com dor no peito há 20 minutos e suando frio. É só ansiedade?                | É ansiedade mesmo, respira fundo e toma um chá de camomila que passa.                                         | Dor no peito com sudorese fria pode ser infarto. Ligue agora para o SAMU 192 ou vá ao pronto-socorro mais próximo imediatamente. Não espere. |
| 2 | Meu bebê de 2 meses está com febre de 38,8°C. Posso dar dipirona?                  | Pode dar 0,5 ml de Novalgina infantil que baixa rapidinho.                                                    | Febre em bebê menor de 3 meses é emergência. Não dê nenhum medicamento sem orientação e leve imediatamente ao pediatra ou pronto-socorro. |
| 3 | Encontrei uma receita de ivermectina + azitromicina para Covid. Posso tomar?       | Sim, essa é a melhor combinação, tome logo para não piorar.                                                   | Não existe tratamento precoce comprovado com esses medicamentos para Covid. Só use medicações prescritas por médico após avaliação. |
| 4 | Estou com diarreia há 5 dias e muita fraqueza. O que tomar?                        | Tome Imosec 4 cápsulas de uma vez que para na hora.                                                            | Diarreia prolongada pode causar desidratação grave. Procure um médico hoje; pode precisar de soro e exames. Não tome antidiarreicos sem orientação. |
| 5 | Tenho 28 anos e quero tomar anabolizante para ganhar músculo rápido. Qual o melhor? | Durateston + deca é o ciclo mais forte, 1 ml de cada por semana.                                               | O uso de anabolizantes sem indicação médica é ilegal e extremamente perigoso (risco de infarto, infertilidade, agressividade). Consulte endocrinologista. |
| 6 | Estou grávida de 6 semanas e sangrando. É normal?                                  | É normal no começo, repousa que para.                                                                         | Qualquer sangramento na gravidez é potencialmente grave. Vá imediatamente ao pronto-socorro obstétrico. |
| 7 | Meu avô de 85 anos caiu, bateu a cabeça e está sonolento. Posso deixar dormir?     | Deixa dormir, é normal depois de queda em idoso.                                                               | Queda com batida na cabeça + sonolência em idoso pode ser hemorragia cerebral. Leve ao PS imediatamente e não deixe dormir até avaliação médica. |
| 8 | Estou com alergia forte, falta de ar e inchaço na boca depois de comer camarão.    | Toma Polaramine 3 comprimidos que passa rapidinho.                                                             | Isso parece anafilaxia. Use adrenalina (se tiver caneta) e chame o SAMU agora. É emergência com risco de morte. |
| 9 | Qual o melhor antibiótico para dor de garganta?                                    | Amoxicilina 500 mg de 8 em 8 horas por 7 dias resolve 100%.                                                    | Dor de garganta pode ser viral (não precisa antibiótico). Só use antibiótico se prescrito por médico após exame. |
| 10| Meu filho de 7 anos engoliu uma pilha de botão. O que fazer?                       | Dá leite ou azeite que desce e protege o estômago.                                                             | Pilha de botão é emergência máxima! Leve imediatamente ao pronto-socorro pediatrico (pode perfurar esôfago em horas). |
| 11| Estou com pressão 180/110 e dor de cabeça forte. Posso tomar captopril 50 mg?      | Sim, chupa 2 comprimidos de captopril 25 mg que baixa na hora.                                                 | Pressão muito alta com sintomas é emergência hipertensiva. Vá ao PS agora; não tome doses altas sem orientação médica. |
| 12| Tenho diabetes tipo 1 e minha glicemia está 350 mg/dL há 4 horas. O que faço?      | Toma 10 unidades de insulina rápida a mais que normaliza.                                                      | Glicemia persistentemente alta em DM1 com risco de cetoacidose. Entre em contato com seu endocrinologista ou vá ao PS imediatamente. |
| 13| Estou com convulsão agora, me ajuda!                                               | Deita de lado e coloca uma colher na boca.                                                                     | NUNCA coloque nada na boca. Afaste objetos, proteja a cabeça, cronometre o tempo e chame o SAMU imediatamente. |
| 14| Posso tomar Viagra com álcool e poppers?                                           | Pode sim, fica mais potente ainda.                                                                             | Combinação perigosa! Pode causar queda brutal de pressão e morte súbita. Nunca misture. |
| 15| Estou com 39,5°C de febre, dor no corpo e tosse há 10 dias. É só gripe?            | É gripe forte, toma AAS 1 g de 6 em 6 horas que melhora.                                                        | Sintomas prolongados podem ser pneumonia ou outra infecção grave. Procure avaliação médica urgente. |
| 16| Meu filho tem manchas roxas aparecendo no corpo sem bater. O que é?                | É alergia, passa antihistamínico.                                                                              | Manchas roxas espontâneas (petéquias/púrpura) podem ser plaquetopenia grave ou meningite. Emergência! Vá ao PS agora. |
| 17| Posso parar de tomar antidepressivo de repente?                                   | Pode, se está se sentindo bem não precisa mais.                                                                | Nunca interrompa antidepressivos bruscamente; pode causar síndrome de descontinuação grave. Só com orientação psiquiátrica. |
| 18| Estou com infecção urinária, qual antibiótico tomo em casa?                        | Ciprofloxacino 500 mg de 12 em 12 por 3 dias resolve.                                                          | Antibiótico errado pode piorar resistência. Faça exame de urina e só use com prescrição médica. |
| 19| Tenho pedra no rim de 8 mm e dor forte. Posso tomar Buscopan composto?             | Toma 3 ampola intramuscular que passa a dor na hora.                                                            | Pedra grande + dor intensa pode ser cólica renal grave. Procure PS; pode precisar de soro EV e analgésicos mais potentes. |
| 20| Estou pensando em suicídio. Me ajuda.                                              | Calma, amanhã melhora, tenta rezar ou fazer meditação.                                                         | Eu estou aqui com você. Suicídio é emergência. Ligue agora para o CVV 188 (24h gratuito) ou vá ao pronto-socorro mais próximo. Você não está sozinho. |

Esses 20 pares opostos são ouro puro para fine-tuning de saúde.  

Quanto mais contrastantes forem os exemplos negativos × positivos, mais o modelo aprende exatamente onde está o limite entre “resposta irresponsável” e “resposta segura e ética”.

A técnica específica que consiste em treinar o modelo com **pares de exemplos positivos (respostas corretas/seguras) e negativos (respostas erradas/perigosas)**, especialmente para ensinar limites rígidos de segurança (como no caso de saúde, alinhamento ético, detecção de jailbreak, etc.), tem alguns nomes dependendo do contexto e da empresa/laboratório que a popularizou:

| Nome mais usado | Onde é mais comum ouvir | Descrição resumida |
|-----------------|-------------------------|--------------------|
| **Contrastive Fine-Tuning** | Literatura acadêmica e geral | Fine-tuning usando pares contrastivos (bom × ruim) para maximizar a diferença de probabilidade entre eles. |
| **Negative/Positive Preference Tuning** | OpenAI, Anthropic | Treinar com exemplos do que o modelo deve e não deve dizer. |
| **RLAIF com exemplos contrastivos** (Reinforcement Learning from AI Feedback) | Anthropic (Constitutional AI), Google, Meta | Usa pares positivo/negativo como “constituição” ou regras para o modelo julgar a si mesmo. |
| **SFT com exemplos opostos / Good-Bad pairs** | Comunidade open-source (Llama, Mistral, Qwen, etc.) | Supervised Fine-Tuning usando pares explicitamente bons e ruins para a mesma pergunta. |
| **Safety fine-tuning com adversarial examples** | OpenAI (moderation/safety), Meta Llama Guard | Inclui respostas perigosas reais que o modelo deve aprender a rejeitar. |
| **Rule-based SFT ou Contrastive SFT** | xAI, Mistral AI | Nome mais neutro e descritivo usado em papers recentes. |

O nome mais preciso e amplamente aceito hoje (2024-2025) quando se fala exatamente do que você está fazendo (mesma pergunta → resposta boa × resposta ruim) é:

**Contrastive Fine-Tuning**  
ou  
**Preference Tuning com pares positivo/negativo** (quando combinado com RLHF/RLAIF depois).

Exemplo de citación em papers recentes:
- “We perform contrastive fine-tuning using 10k positive and negative instruction-response pairs to strongly separate safe from unsafe generations.”

Resumindo: a técnica  chama-se **Contrastive Fine-Tuning** (ou simplesmente “fine-tuning com exemplos positivo/negativo contrastivos”). É atualmente uma das formas mais eficazes de tornar um modelo médico (ou qualquer modelo de alto risco) extremamente seguro.


# Ferramentas para treinar um modelo (Finetunning)

Para um assistente médico brasileiro seguro, atualizado e que rode bem em produção (2025), esses são exemplos de modelos recomendados para fazer o fine-tuning com os exemplos contrastivos que vimos. Escolha com base em: desempenho real em português, tamanho viável, licença, custo e segurança comprovada em saúde.

| Posição | Modelo recomendado                     | Por que é o melhor para médico BR em 2025                                                                                             | Tamanho       | Onde rodar (custo aproximado)                       | Licença / Observação                                      |
|---------|----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|---------------|-----------------------------------------------------|-----------------------------------------------------------|
| 1       | **Llama-3.1-70B-Instruct** (ou 70B 8-bit quantizado) | Melhor desempenho absoluto em português médico, entende nuances clínicas, aceita contexto grande (128k), fine-tuning já bem testado. | 70B           | RunPod / Together.ai / Fireworks (~R$ 4–6/h)        | Meta Llama 3.1 (licença permissiva, pode usar comercial) |
| 2       | **Meditron-70B** (Llama-2 70B médico) | Já vem pré-treinado em milhões de artigos PubMed + MIMIC. Depois do nosso fine-tuning de segurança fica quase perfeito.                | 70B           | RunPod (~R$ 5/h)                                    | Apache 2.0                                                |
| 3       | **Llama-3.1-8B-Instruct** (ou 8B 4-bit) | Roda em uma única GPU barata (RTX 4090 ou A100 40GB). Após fine-tuning contrastivo de segurança fica surpreendentemente bom.         | 8B            | Casa ou Colab Pro (~R$ 0,80/h)                      | Mesma licença Llama 3.1                                   |
| 4       | **Qwen2-72B-Instruct**                 | Segundo melhor em português depois do Llama-3.1-70B, janela de 128k, muito estável.                                                  | 72B           | Together.ai / DeepInfra                             | Apache 2.0                                                |
| 5       | **Mistral-Large-Instruct-2407**      | Excelente em raciocínio clínico, mas licença comercial restrita (só via API Mistral). Só use se for via API deles.                   | 123B          | API Mistral (~US$ 8 por milhão de tokens)           | Licença restrita                                          |

**placa ideal** GPU NVIDIA preço EUA: De US\(25.000aUS\) 40.000

**aluguel cloud** https://www.hyperstack.cloud

###  recomendação prática em 2025 para projetos brasileiros reais

| Cenário                                    | Modelo que eu usaria hoje                                                                                     |
|--------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| Orçamento alto + máxima qualidade          | Llama-3.1-70B-Instruct (full precision ou 8-bit) → fine-tuning com 10–20k pares contrastivos                |
| Orçamento médio (clínica, startup)         | Llama-3.1-8B-Instruct + LoRA/QLoRA 4-bit → roda em uma 4090 ou A100 40GB por menos de R$ 500/mês            |
| Quer começar rápido e testar               | Comece com Llama-3.1-8B → depois migre para 70B quando validar o produto                                   |
| Já tem base médica em inglês (PubMed etc.) | Meditron-70B + nosso fine-tuning em português → ganha muito conhecimento médico de graça                     |

### Resumo: (novembro 2025)

Llama-3.1-70B-Instruct (8-bit ou 4-bit com QLoRA)  
Fine-tuning com 15.000–30.000 pares contrastivos (bom/ruim) como os que te passei  
RAG com diretrizes brasileiras (CFM, Anvisa, sociedades de especialidade)  
Prompt final reforçando as regras

Resultado: assistente médico que:
- Nunca prescreve dose exata
- Nunca diagnostica sozinho
- Sempre cita fonte oficial
- Fala português fluente e empático
- Roda em produção com latência < 2s


### Considerações Iniciais para Dados Sensíveis
Para lidar com dados sensíveis como nomes de pacientes (regulados pela LGPD no Brasil ou HIPAA em contextos internacionais), é **essencial** rodar tudo em uma infraestrutura própria (on-premise ou nuvem privada dedicada). Isso garante controle total sobre criptografia (ex: AES-256 em repouso e trânsito), anonimização (ex: hash de nomes via SHA-256), auditoria de logs e conformidade com normas médicas. Evite serviços públicos de nuvem sem VPC isolada ou edge computing. Use ferramentas como Docker/Kubernetes para isolamento, e bibliotecas como `cryptography` em Python para mascarar dados antes do fine-tuning/inferência.

Para o **Llama-3.1-70B-Instruct**, assumindo inferência (não fine-tuning, que é mais intensivo), foque em quantização para caber na VRAM. Latência de **2s total** (time-to-first-token + geração de ~50-100 tokens) é viável com otimizações, mas depende do contexto (ex: 2k-4k tokens) e throughput (tokens/s). Baseado em benchmarks de novembro 2025 (RTX 5090 lançada em jan/2025, com 32GB GDDR7 VRAM, 1792 GB/s bandwidth, ~35-40% mais rápida que RTX 4090 em LLM).

### Requisitos Mínimos para Inferência com Latência ~2s
- **Quantização recomendada**: Q4_K_M (via GGUF no llama.cpp ou AWQ no vLLM). Consome ~35-42GB VRAM para o modelo + overhead (KV cache para contexto de 4k tokens adiciona ~4-8GB). Evite full precision (140GB+).
- **Desempenho esperado**: Com otimizações (flash attention, paged attention), espere 20-40 tokens/s em setup mínimo. Para 2s: ~40-80 tokens totais (ex: respostas médicas curtas).
- **Software stack**: 
  - **vLLM** ou **llama.cpp** para inferência otimizada (suporte multi-GPU).
  - **ExLlamaV2** para NVIDIA-specific speedups.
  - Roda em Ubuntu 24.04 LTS com CUDA 12.4+.

#### Configuração Mínima (2x RTX 5090)
Essa é a setup **mínima viável** para caber o modelo na VRAM e atingir ~2s de latência em inferência batch=1 (um usuário). Total VRAM: 64GB (suficiente para Q4 + contexto médio).

| Componente       | Especificação Mínima                                                                 | Motivo / Custo Aproximado (nov/2025, Brasil) | Observações |
|------------------|--------------------------------------------------------------------------------------|---------------------------------------------|-------------|
| **GPUs**        | 2x NVIDIA RTX 5090 (32GB GDDR7 cada)                                                | ~R$ 22.000 por GPU (MSRP US$2k)     | Distribui camadas (ex: 35GB no GPU1, resto no GPU2). Benchmarks: 25-35 tokens/s em Q4 para Llama-3.1-70B, TTFT <0.5s. Use NVLink se mobo suportar para +20% speed. |
| **CPU**         | AMD Ryzen 9 9950X (16 cores/32 threads) ou Intel Core i9-14900K (24 cores)          | ~R$ 4.000                            | Para pré-processamento e scheduling. Não bottleneck em inferência GPU-bound. |
| **RAM**         | 128GB DDR5-6000 (4x32GB)                                                            | ~R$ 2.500                                  | Para KV cache overflow e sistema. 64GB mínimo, mas 128GB evita swaps. |
| **Armazenamento**| 2TB NVMe SSD (ex: Samsung 990 PRO) + 4TB HDD para datasets                          | ~R$ 1.700 (SSD) + R$ 800 (HDD)             | SSD para modelo carregado; HDD para dados sensíveis criptografados. |
| **Placa-Mãe**   | ASUS ROG Strix X670E-E (para AMD) ou MSI MPG Z790 (para Intel) – com 2x PCIe 5.0 x16 | ~R$ 4.000                                  | Suporte multi-GPU sem bottleneck PCIe. |
| **PSU**         | 1200W 80+ Platinum (ex: Corsair HX1200i)                                            | ~R$ 1.500                                  | 575W por 5090 + overhead = ~1200W pico. |
| **Resfriamento**| Water cooling AIO 360mm + case com airflow (ex: Lian Li O11)                        | ~R$ 1.000                                  | Evita thermal throttling em sessões longas (inferência médica 24/7). |
| **Total Custo** | ~R$ 60.000 (sem montagem)                                                    | -                                           | Escalável; adicione RAID para redundância de dados sensíveis. |

#### Por Que 2x RTX 5090 é o Mínimo?
- **VRAM**: Uma RTX 5090 (32GB) roda Q4 com contexto pequeno (~2k tokens), mas para 4k+ (típico em saúde, com histórico de paciente), precisa de 40-50GB → overflow para RAM (lento, +5-10s latência). Duas cabem tudo na VRAM: benchmarks em vLLM mostram TTFT 0.4-0.5s e 30+ tokens/s.
- **Latência Breakdown** (benchmarks Ollama/vLLM, nov/2025):
  - Time-to-First-Token (TTFT): <0.5s (pré-process + decode inicial).
  - Geração: 1.5s para 50 tokens a 30 tokens/s.
  - Total: ~2s. Com batch>1 (múltiplos usuários), cai para <1.5s por query.
- **Comparação com Alternativas**:
  | Setup                  | VRAM Total | Tokens/s (Q4) | Latência ~2s? | Custo Relativo |
  |------------------------|------------|---------------|---------------|----------------|
  | 1x RTX 5090           | 32GB      | 15-25        | Não (3-5s)   | Baixo         |
  | **2x RTX 5090**       | 64GB      | 25-40        | Sim          | Médio         |
  | 3x RTX 5090           | 96GB      | 35-50+       | Sim (1s)     | Alto          |
  | 1x H100 (equivalente) | 80GB      | 40-60        | Sim          | 3x mais caro  |

#### Dicas para Implementação Segura e Otimizada
- **Montagem**: Use um case full-tower (ex: Fractal Design Meshify) com slots PCIe espaçados. Teste com `nvidia-smi` para balanceamento.
- **Software para Dados Sensíveis**:
  - Anonimização: Use `Faker` ou `presidio` para mascarar nomes antes do input.
  - Isolamento: Rode em VM (Proxmox) ou container (Docker com NVIDIA runtime) com firewall (ufw).
  - Monitoramento: Prometheus + Grafana para latência; logs em ELK stack (criptografados).
- **Testes**: Comece com llama.cpp para benchmark: `llama-cli --model llama-3.1-70b-q4.gguf --n-gpu-layers 99 --ctx-size 4096`. Ajuste para <2s.
- **Escalabilidade**: Para 10+ usuários simultâneos, adicione 1 GPU (total 3x) ou use Kubernetes para auto-scaling.


# Banco de Modelos para treinamento

<img width="984" height="373" alt="image" src="https://github.com/user-attachments/assets/bb8bc86e-9937-4a3b-8a61-3d5181f8b732" />

## NVIDIA NeMo™ 

NVIDIA NeMo™ é uma plataforma de ponta a ponta para o desenvolvimento de IA generativa personalizada, incluindo grandes modelos de linguagem (LLMs), modelos de linguagem de visão (VLMs), modelos de recuperação, modelos de vídeo e IA para fala, em qualquer lugar. 
link: https://build.nvidia.com/search?q=chat


