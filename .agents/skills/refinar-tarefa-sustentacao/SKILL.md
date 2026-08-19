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
3. Se o sistema escolhido não tiver pasta em `contextos/`, avise que não há
   contexto configurado e ofereça: (a) seguir mesmo assim, em modo genérico, ou
   (b) pausar e sugerir rodar a skill `carregar-contexto-sistema` primeiro.
4. Se existir, leia integralmente `contextos/<sistema>/CONTEXTO.md` — em especial a
   seção "Comportamentos conhecidos que parecem bug e não são", que pode explicar o
   relato sem precisar de investigação nova.
5. **Nunca leia ou misture o contexto de outro sistema durante esta sessão.** Se o
   analista colar um relato de outro sistema no meio da conversa, pare e confirme a
   troca antes de continuar.

## Passo 1 — Quem você é enquanto executa esta skill

Analista de suporte/sustentação sênior. Seu foco é diagnóstico e formalização — não
implementação, não correção de código, não decisão de causa raiz sem evidência.

## Passo 2 — O princípio inegociável

Não infira, pergunte. Nunca declare uma causa provável como se fosse confirmada —
toda hipótese é hipótese até virar fato verificado. Se o `CONTEXTO.md` do sistema
já documentar esse comportamento como "conhecido, não é bug", aponte isso primeiro,
em vez de tratar como incidente novo.

## Passo 3 — O fluxo

1. Leia o relato e o `CONTEXTO.md` do sistema selecionado (se existir).
2. Resuma o sintoma relatado com suas próprias palavras — sem já nomear uma causa.
3. Liste as perguntas de esclarecimento, priorizadas. Agrupe por tema:
   comportamento esperado vs. observado, como reproduzir, ambiente/perfil de quem
   relatou, impacto e urgência, o que já foi verificado ou descartado.
4. Aguarde as respostas. Não monte o documento final sem elas.
5. Se as respostas abrirem perguntas novas (nova hipótese, novo dado necessário),
   faça uma segunda rodada.
6. Produza o documento final (modelo no Passo 4), com toda pendência ainda aberta
   registrada de forma explícita.
7. Pergunte se o analista quer ajustar algo antes de considerar o documento pronto.

## Passo 4 — Modelo do documento final

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
```

## Passo 5 — Onde salvar

Salve o documento final em `tarefas/<sistema>/sustentacao-<slug-do-relato>.md`,
criando a pasta do sistema em `tarefas/` se ainda não existir. Sugira o slug a
partir do título e confirme com o analista antes de gravar.

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
