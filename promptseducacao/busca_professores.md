
# #Guia de prompts utilizados para construção dos GPTs Custom

## Criação do arquivo lattes de professores

Utilizei o navegador "Comet" instalado em minha máquina a abri sequencialmente 5 abas por vez de cada perfil disponível em https://integra.ifsp.edu.br/institucional com a escolha do Campus Catanduva

Link:  https://www.perplexity.ai/comet

prompt
```
Acesse as abas abertas dos perfis de docentes e gere um  json para o curriculo de docentes , com a formação academica/titulação completa, captando informações sobre a graduação, mestrado, doutorado, especializações, pós doutorado se houverem. Registre título, a instituição, ano início e término de cada um, colete as palavras chaves de temas trabalhados. Estes dados serão mesclados um único arquivo.
```

## Extraindo dados do calendario escolar

Extraindo as informações do arquivo pdf para o calendário 2026.
Utilizando **gemini** em sua versão pro gratuita com maior chances de extração com grandes contextos, fiz upload do arquivo e várias tentativas frustadas em função da organização das informações no pdf.

prompt versão 1
```
Crie um arquivo json para o calendário de cursos técnicos integrados com esta base de conhecimento fornecida que servira de base de conhecimento RAG para uma projeto. 
considerando desde 01 de Janeiro até 31 de dezembro
O arquivo deve separar semana a semana o dia letivo para os dois semestres. Incluir no calendário os sabados letivos. 
inclua as férias docentes e recessos, e eventos administrativos.
```

prompt aprimorado com chat
```
"Crie um calendário completo, no formato JSON, para os cursos cobrindo o período de 01 de Janeiro a 31 de Dezembro de 2026.

Use as informações fornecidas no arquivo como ÚNICA fonte de dados.

O arquivo JSON deve ser estruturado por 'semanas' (começando na semana 01). Para cada dia, inclua os seguintes campos:

1.  **data** (formato YYYY-MM-DD) e **dia_semana** (ex: Seg, Sab).
2.  **tipo_dia**: Classifique o dia com uma destas etiquetas: 'Letivo', 'Feriado/Recesso', 'Férias Docentes' ou 'Administrativo'.
3.  **descricao**: Breve descrição do dia ou evento principal.
4.  **eventos_administrativos**: Uma lista (array) de strings para quaisquer eventos que ocorram no dia (Ex: Reunião Geral, Prazo final).

Regras de Classificação (Prioridade):
* Dias de **Férias Docentes** (Janeiro e Julho) mantêm essa classificação, mesmo se houver Rematrícula ou Matrícula.
* Dias de **Espaço Pedagógico**, **Planejamento** e **Conselho de Classe Deliberativo** devem ser classificados como **Administrativo**, pois não há aulas regulares.
* Dias com **Reunião Geral**, **Formação Continuada** ou **Conselho de Classe Pedagógico** devem ser classificados como **Letivo**, se a atividade letiva (aulas) não tiver sido explicitamente suspensa.
* **Sabados Letivos** devem ser explicitamente classificados como **Letivo**.
* Todos os outros **Sábados** que não são marcados como Letivos, Recessos ou Férias Docentes devem ser classificados como **Não Letivo** (e não Letivo).
* Os **Domingos** são sempre **Não Letivo**, a menos que haja um evento específico (como IFsport - Domingo letivo).

Assegure-se de que a classificação de Sábados Letivos seja precisa, baseada na fonte original."
```


## Prompt multiagente no chatgpt

Simula multiagentes mencionando vários @GPTs no mesmo prompt. A ideia é você agir como **Orquestrador**, dividindo o trabalho em passos curtos e pedindo que cada @agente entregue **saídas padronizadas** (ex.: JSON/Markdown) para que você consolide no final.
### 1) Protocolo de orquestração 

(copie e use como prompt mestre). 

```
[ORQUESTRADOR]
Objetivo geral: <descreva em 1–2 linhas o que precisa obter>
Contexto breve: <links, requisitos, público-alvo, restrições de prazo e de custo>

REGRAS GERAIS (aplicam-se a todos os agentes chamados por @):
- Delimite sua resposta ao escopo da sua função.
- Use o formato de SAÍDA pedido em cada tarefa (evite texto livre fora do template).
- Se detectar ambiguidade, proponha hipóteses e siga com a melhor suposição.
- NÃO repita conteúdo já entregue por outro agente; referencie pelo identificador da tarefa (ex.: T2).
- Sinalize dependências: escreva “DEPENDE: T#” quando precisar da saída de outra tarefa.
- Tamanho máximo por resposta: 250–400 palavras (ou 60–100 linhas de código), salvo quando eu pedir o contrário.

PAPÉIS (cada um mapeia para um @GPT custom):
- @Pesquisador: levanta dados, fontes, normas e limitações.
- @Arquiteto: define solução/arquitetura, trade-offs e diagrama textual.
- @Engenheiro: implementa (código, comandos, config), com testes mínimos.
- @Revisor: checa qualidade, riscos, lacunas, segurança e dá checklist final.
(Altere nomes/papéis conforme seus @ disponíveis.)

TAREFAS
T1 (@Pesquisador) — Levantamento
- Entregáveis: “achados” + referências (em lista curta). 
- SAÍDA (JSON):
{
  "requisitos": ["..."],
  "restricoes": ["..."],
  "riscos": ["..."],
  "referencias": [{"titulo":"...", "url":"..."}]
}

T2 (@Arquiteto) — Arquitetura de solução
- Insumos: T1
- Entregáveis: visão de componentes e fluxo de dados.
- SAÍDA (Markdown):
### Arquitetura
- Componentes: ...
- Fluxo: ...
- Decisões: ...
- Alternativas: ...
- Métricas de sucesso: ...

T3 (@Engenheiro) — Implementação mínima viável
- Insumos: T1, T2
- Entregáveis: código/arquivos essenciais + instruções de execução.
- SAÍDA (blocos de código + passos enumerados)
# comandos
# arquivos
# testes rápidos

T4 (@Revisor) — Garantia de qualidade
- Insumos: T1, T2, T3
- Entregáveis: matriz de verificação, riscos remanescentes, melhorias.
- SAÍDA (Checklist):
- [ ] Item 1 ...
- [ ] Item 2 ...
Riscos remanescentes: ...
Melhorias sugeridas: ...

ORDEM DE EXECUÇÃO
1) Responda T1.
2) Depois T2.
3) Depois T3.
4) Por fim T4.
Quando concluir cada tarefa, comece a próxima automaticamente.
No fim, entregue um “RESUMO FINAL” (por você, Orquestrador): 
- Objetivo, principais decisões, como executar, próximos passos.

INICIAR com T1 agora.
```

> Como usar: cole o bloco acima no chat onde seus @GPTs custom estão disponíveis. Eles irão responder em cadeia (T1→T2→T3→T4) dentro do mesmo fio, cada um limitado ao seu papel.

---
