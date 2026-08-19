---
name: refinar-tarefa-sustentacao
description: Refina um chamado de suporte ou relato de algo que já deveria funcionar de um jeito e não está — erro, divergência de dado, lentidão, comportamento inconsistente — em um documento de investigação e resolução completo. Use quando o relato for sobre comportamento incorreto de algo que já existe. Para pedido de algo novo (customização, melhoria), use a skill `refinar-requisito-customizacao` em vez desta. Pergunta primeiro qual sistema/contexto usar (pasta em contextos/), depois conduz o fluxo de esclarecimento até produzir o documento final.
---

# refinar-tarefa-sustentacao

## Quando usar

- O relato descreve algo que **já existe** no sistema e não está se comportando
  como deveria: erro, divergência de dado entre telas/relatórios, lentidão,
  travamento, comportamento inconsistente.
- Pedidos como: "refina esse chamado", "formaliza esse relato de erro", "prepara a
  investigação a partir desse chamado de suporte".
- **Não é esta skill** se o relato pedir algo que o sistema ainda não faz — isso é
  `refinar-requisito-customizacao`. Na dúvida, pergunte: *"isso é algo que ainda não
  existe, ou um problema com algo que já deveria funcionar?"*

## Passo 0 — Qual sistema?

1. Liste as pastas existentes em `contextos/` (ignore `_template-contexto.md`).
2. Pergunte ao analista a qual sistema esse relato pertence — mesmo que só exista
   um sistema configurado hoje.
3. Se o sistema escolhido não tiver pasta em `contextos/`, prossiga normalmente —
   carregar contexto é **opcional**, nunca um bloqueio. Você já atua como analista
   de suporte/sustentação generalista, aplicando o conhecimento que a função
   carrega. Se o analista preferir uma análise mais específica desde já, ofereça
   (sem insistir) rodar a skill `carregar-contexto-sistema` primeiro.
4. Se existir, leia integralmente `contextos/<sistema>/CONTEXTO.md` — em especial a
   seção "Comportamentos conhecidos que parecem bug e não são", que pode explicar o
   relato sem precisar de investigação nova.
5. **Nunca leia ou misture o contexto de outro sistema durante esta sessão.** Se o
   analista colar um relato de outro sistema no meio da conversa, pare e confirme a
   troca antes de continuar.

## Passo 1 — Buscar histórico de tarefas semelhantes

Depois de confirmar o sistema (Passo 0) e antes de seguir para o fluxo de
esclarecimento (Passo 4), verifique se esse relato já foi tratado antes:

1. Liste as subpastas existentes em `tarefas/<sistema>/` — tanto `sustentacao-*`
   quanto `customizacao-*`; o relato atual pode já ter sido tratado antes como o
   outro tipo de tarefa (ex.: um pedido de customização que originou uma regra que
   agora está divergindo).
2. Compare os nomes das subpastas — e, quando não for óbvio só pelo nome, o
   conteúdo do `.md` de dentro — com o assunto, tela, campo ou sintoma do relato
   atual.
3. Se encontrar uma tarefa anterior que já trata do mesmo relato: avise o analista
   já no resumo inicial (Passo 4, item 2), aponte o caminho do documento existente,
   e pergunte se ele quer reaproveitar/atualizar aquele documento em vez de abrir
   um novo.
4. Se encontrar uma tarefa relacionada mas não idêntica (mesma tela/campo, sintoma
   parecido): use as hipóteses e fatos já confirmados lá como ponto de partida das
   perguntas deste refinamento — mas nunca os trate como fato assumido sem
   reconfirmar. Cite sempre a origem (caminho do documento anterior) ao perguntar
   se aquilo ainda vale hoje.
5. Se não encontrar nada relacionado, ou `tarefas/<sistema>/` ainda não existir,
   diga isso brevemente e siga em frente — não é bloqueio.
6. Registre o resultado desta busca no apêndice "Histórico de tarefas consultado"
   do documento final (modelo no Passo 5).

Regra completa e o princípio por trás dela: `AGENTS.md`, seção 5.

## Passo 2 — Quem você é enquanto executa esta skill

Analista de suporte/sustentação sênior. Seu foco é diagnóstico e formalização — não
implementação, não correção de código, não decisão de causa raiz sem evidência.

## Passo 3 — O princípio inegociável

Não infira, pergunte. Nunca declare uma causa provável como se fosse confirmada —
toda hipótese é hipótese até virar fato verificado. Se o `CONTEXTO.md` do sistema
já documentar esse comportamento como "conhecido, não é bug", aponte isso primeiro,
em vez de tratar como incidente novo.

## Passo 4 — O fluxo

1. Leia o relato e o `CONTEXTO.md` do sistema selecionado (se existir).
2. Resuma o sintoma relatado com suas próprias palavras — sem já nomear uma causa,
   incluindo o que encontrou no Passo 1 sobre tarefas anteriores relacionadas, se
   houver.
3. Liste as perguntas de esclarecimento, priorizadas. Agrupe por tema:
   comportamento esperado vs. observado, como reproduzir, ambiente/perfil de quem
   relatou, impacto e urgência, o que já foi verificado ou descartado.
4. Aguarde as respostas. Não monte o documento final sem elas.
5. Se as respostas abrirem perguntas novas (nova hipótese, novo dado necessário),
   faça uma segunda rodada.
6. Produza o documento final (modelo no Passo 5), com toda pendência ainda aberta
   registrada de forma explícita.
7. Pergunte se o analista quer ajustar algo antes de considerar o documento pronto.

## Passo 5 — Modelo do documento final

```markdown
# Tarefa de sustentação: <título curto>

## 1. Contexto
Quem relatou, quando, canal (chamado/e-mail/etc.), sistema e módulo, se souber.

## 2. Sintoma relatado
Descrição literal do que foi reportado — o sintoma, sem interpretação nem causa.

## 3. Comportamento esperado vs. observado
| Esperado | Observado |
|---|---|
| <o que deveria acontecer, confirmado com quem relatou ou com o CONTEXTO.md> | <o que de fato acontece, conforme relatado> |

## 4. Passos para reproduzir
Passo a passo, se conhecido — dado de teste, perfil/permissão do usuário, ambiente,
frequência (sempre acontece? só às vezes? só para um grupo?).

## 5. Hipóteses e evidências
Hipóteses de causa levantadas durante a conversa, e o que já foi verificado ou
descartado. Toda hipótese sem confirmação técnica fica marcada como **hipótese**,
nunca como causa.

## 6. Impacto e urgência
Quantas pessoas/processos afetados, é bloqueante, existe prazo legal ou financeiro
envolvido, existe contorno (workaround) enquanto não é corrigido.

## 7. Fora de escopo
O que este chamado não cobre, para não virar retrabalho de escopo depois.

## 8. Pendências
| Pergunta | Por quê importa | Responsável | Status |
|---|---|---|---|

## 9. Critério de resolução
O que precisa ser verdade para considerar esta tarefa resolvida (comportamento
observável, não "corrigir o código").

## Apêndice: Histórico de tarefas consultado
Resultado da busca feita no Passo 1 em `tarefas/<sistema>/`.
- Tarefas anteriores verificadas: <lista de subpastas revisadas, ou "nenhuma
  tarefa anterior encontrada">.
- Se alguma foi considerada relacionada: <caminho do documento> — o que foi
  reaproveitado e se cada fato/hipótese herdado foi reconfirmado pelo analista ou
  segue como pendência.
```

## Passo 6 — Onde salvar

Cada relato ganha sua própria subpasta dentro de `tarefas/<sistema>/`, nomeada com o
mesmo slug do documento: `tarefas/<sistema>/sustentacao-<slug-do-relato>/`. Salve o
documento final dentro dela, em
`tarefas/<sistema>/sustentacao-<slug-do-relato>/sustentacao-<slug-do-relato>.md`,
criando as pastas que faltarem (`tarefas/<sistema>/` e a subpasta da tarefa) se ainda
não existirem. Sugira o slug a partir do título e confirme com o analista antes de
gravar. Todo artefato gerado para este relato — o `.md`, o `.pdf` quando pedido, e
qualquer outro arquivo futuro — fica dentro dessa mesma subpasta, nunca solto direto
em `tarefas/<sistema>/`.

Depois de gravar, confirme ao analista o caminho do arquivo salvo e pergunte se ele
quer também uma versão em PDF do documento. Se a resposta for sim, aplique
`.agents/skills/exportar-pdf-tarefa/SKILL.md` para gerá-la a partir do `.md`
recém-salvo, salvando o PDF na mesma subpasta.

## Guardrails

- Nunca declare a causa raiz como fato sem evidência técnica confirmada — fica como
  hipótese, na seção 5, até alguém com acesso técnico validar.
- Não assuma que é bug antes de checar se é comportamento conhecido (ver
  `CONTEXTO.md` do sistema, seção "parecem bug e não são").
- Cite a origem de cada fato confirmado (quem respondeu, quando, ou qual documento).
- Não sugira correção técnica nem estrutura de código — isso é da etapa seguinte.
- Sinalize dado sensível envolvido no relato ou nas evidências anexadas.
- Não estime prazo ou esforço de correção. Classifique impacto/urgência (seção 6),
  não prazo de entrega.
- Nunca declare a tarefa "pronta para desenvolvimento" enquanto houver pendência
  crítica aberta na seção 8.
