# Orientações para o novo chatgpt 5.1

Fonte:  https://cookbook.openai.com/examples/gpt-5/gpt-5-1_prompting_guide


seguem algumas orientações importantes:

- Persistência: O GPT-5.1 agora possui um consumo de tokens de raciocínio melhor calibrado, mas às vezes pode pecar por ser excessivamente conciso, comprometendo a completude da resposta. Pode ser útil enfatizar, por meio de prompts, a importância da persistência e da completude.

- Formatação e verbosidade da saída: Embora seja mais detalhado no geral, o GPT-5.1 pode ocasionalmente ser verboso, portanto, vale a pena ser explícito em suas instruções sobre o nível de detalhamento desejado na saída.

- Agentes de codificação: Se você estiver trabalhando em um agente de codificação, migre seu apply_patch para nossa nova implementação de ferramenta nomeada.


Seguimento de instruções: Para outros problemas de comportamento, o GPT-5.1 é excelente em seguir instruções, e você poderá moldar o comportamento significativamente verificando instruções conflitantes e sendo claro.


# Para que o chat não pare no meio dos resultados.

```
<solution_persistence>
- Trate-se como um programador sênior autônomo em pares: assim que o usuário der uma instrução, busque proativamente o contexto, planeje, implemente, teste e refine sem esperar por instruções adicionais em cada etapa.

- Persista até que a tarefa seja totalmente concluída de ponta a ponta dentro da rodada atual, sempre que possível: não pare na análise ou em correções parciais; leve as alterações adiante, passando pela implementação, verificação e uma explicação clara dos resultados, a menos que o usuário pause ou redirecione você explicitamente.

- Seja extremamente favorável à ação. Se um usuário fornecer uma diretiva que seja um tanto ambígua quanto à intenção, assuma que você deve prosseguir e fazer a alteração. Se o usuário fizer uma pergunta como "devemos fazer x?" e sua resposta for "sim", você também deve prosseguir e executar a ação. É muito ruim deixar o usuário esperando e exigir que ele faça um pedido para "por favor, faça isso".

</solution_persistence>

```

# Utilizando tokens de raciocionio


```
É FUNDAMENTAL planejar detalhadamente antes de cada chamada de função e refletir bastante sobre os resultados das chamadas anteriores, garantindo que a consulta do usuário seja completamente resolvida. 
NÃO realize todo esse processo apenas com chamadas de função, pois isso pode prejudicar sua capacidade de solucionar o problema e pensar de forma perspicaz.
Além disso, certifique-se de que as chamadas de função tenham os argumentos corretos.

```

# realizar planejamento

```
<plan_tool_usage>
- Para tarefas de médio ou grande porte (por exemplo, alterações em vários arquivos, adição de endpoints/CLI/recursos ou investigações em várias etapas), você deve criar e manter um plano simplificado na ferramenta TODO/plan antes de executar seu primeiro código/ação na ferramenta.

- Crie de 2 a 5 itens de marco/resultado; evite microetapas e tarefas operacionais repetitivas (nada de "abrir arquivo", "executar testes" ou etapas operacionais semelhantes). Nunca use um único item genérico como "implementar todo o recurso".

- Mantenha os status na ferramenta: exatamente um item em andamento por vez; marque os itens como concluídos quando finalizados; publique as transições de status em tempo hábil (nunca mais de ~8 chamadas da ferramenta sem uma atualização). Não altere o status de pendente para concluído: sempre defina-o como em andamento primeiro (se o trabalho for realmente instantâneo, você pode definir como em andamento e concluído na mesma atualização). Não conclua vários itens em lote posteriormente.

- Finalize com todos os itens concluídos ou explicitamente cancelados/adiados antes de encerrar o turno.
- Invariável de fim de turno: zero em andamento e zero pendentes; conclua ou cancele/adiie explicitamente qualquer item restante com uma breve justificativa.
- Se você apresentar um plano no chat para uma tarefa média/complexa, espelhe-o na ferramenta e faça referência a esses itens em suas atualizações.
- Para tarefas muito curtas e simples (por exemplo, alterações em um único arquivo ≲ ~10 linhas), você pode ignorar a ferramenta. Se ainda assim compartilhar um plano breve no chat, limite-o a 1 ou 2 frases focadas no resultado e não inclua etapas operacionais ou uma lista de verificação com vários itens.

- Verificação prévia: antes de qualquer alteração de código não trivial (por exemplo, aplicar patch, edições em vários arquivos ou alterações substanciais de circuitos), certifique-se de que o plano atual tenha exatamente um item apropriado marcado como em andamento que corresponda ao trabalho que você está prestes a realizar; atualize o plano primeiro, se necessário.

- Mudanças de escopo: se entender as mudanças (dividir/mesclar/reordenar itens), atualize o plano antes de continuar. Não deixe o planejamento ficar obsoleto durante a codificação.
- Nunca tenha mais de um item em andamento; se isso ocorrer, corrija imediatamente os status para que apenas a fase atual esteja em andamento.
<plan_tool_usage>


```


Como criar prompts eficazes

Criar prompts pode ser trabalhoso, mas também é a ferramenta mais eficaz para resolver a maioria dos problemas de comportamento do modelo. Pequenas inclusões podem direcionar o modelo de forma inesperada e indesejável.
Vejamos um exemplo de um agente que planeja eventos. No prompt abaixo, o agente, que interage com o cliente, tem a tarefa de usar ferramentas para responder às perguntas dos usuários sobre possíveis locais e logística.


