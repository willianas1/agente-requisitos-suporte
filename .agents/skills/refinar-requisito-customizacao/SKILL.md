---
name: refinar-requisito-customizacao
description: Refina um pedido curto/ambíguo de algo NOVO — customização, melhoria, mudança de regra que ainda não existe no sistema — em um documento de requisito completo e testável, com histórias de usuário e critérios de aceite. Use quando o pedido descrever algo que o sistema ainda não faz. Para relato de algo que já deveria funcionar e não está (erro, divergência, lentidão), use a skill `refinar-tarefa-sustentacao` em vez desta. Pergunta primeiro qual sistema/contexto usar (pasta em contextos/), depois conduz o fluxo de esclarecimento até produzir o documento final.
---

# refinar-requisito-customizacao

## Quando usar

- O analista cola um pedido bruto de usuário — chamado, e-mail, pedido de
  customização — descrevendo algo **novo**: uma funcionalidade, campo, regra ou
  comportamento que o sistema ainda não tem hoje.
- Pedidos como: "refina esse pedido", "transforma isso em requisito", "monta a
  história de usuário disso", "prepara a especificação funcional a partir desse
  relato".
- **Não é esta skill** se o relato for sobre algo que já deveria funcionar de um
  jeito e não está (erro, divergência, lentidão, comportamento inconsistente) — isso
  é `refinar-tarefa-sustentacao`. Na dúvida, pergunte: *"isso é algo que ainda não
  existe, ou um problema com algo que já deveria funcionar?"*

## Passo 0 — Qual sistema?

1. Liste as pastas existentes em `contextos/` (ignore `_template-contexto.md`).
2. Pergunte ao analista a qual sistema esse pedido pertence — **mesmo que só exista
   um sistema configurado hoje**. Isso evita erro de mistura quando um segundo
   sistema for adicionado no futuro.
3. Se o sistema escolhido não tiver pasta em `contextos/`, prossiga normalmente —
   carregar contexto é **opcional**, nunca um bloqueio. Você já atua como analista
   de requisitos generalista, aplicando o conhecimento que a função carrega. Se o
   analista preferir uma análise mais específica desde já, ofereça (sem insistir)
   rodar a skill `carregar-contexto-sistema` primeiro.
4. Se existir, leia integralmente `contextos/<sistema>/CONTEXTO.md` antes de
   responder ao pedido.
5. **Nunca leia ou misture o contexto de outro sistema durante esta sessão de
   refinamento.** Se, no meio da conversa, o analista colar um pedido de outro
   sistema, pare e confirme a troca de contexto antes de continuar — trate como um
   novo refinamento independente.

## Passo 1 — Buscar histórico de tarefas semelhantes

Depois de confirmar o sistema (Passo 0) e antes de seguir para o fluxo de
esclarecimento (Passo 4), verifique se esse pedido já foi tratado antes:

1. Liste as subpastas existentes em `tarefas/<sistema>/` — tanto `customizacao-*`
   quanto `sustentacao-*`; o pedido atual pode já ter sido tratado antes como o
   outro tipo de tarefa.
2. Compare os nomes das subpastas — e, quando não for óbvio só pelo nome, o
   conteúdo do `.md` de dentro — com o assunto, tela, campo ou regra do pedido
   atual.
3. Se encontrar uma tarefa anterior que já responde ao mesmo pedido: avise o
   analista já no resumo inicial (Passo 4, item 2), aponte o caminho do documento
   existente, e pergunte se ele quer reaproveitar/atualizar aquele documento em vez
   de abrir um novo.
4. Se encontrar uma tarefa relacionada mas não idêntica (mesma tela/campo, regra
   parecida): use as regras já confirmadas lá como ponto de partida das perguntas
   deste refinamento — mas nunca as trate como fato assumido sem reconfirmar. Cite
   sempre a origem (caminho do documento anterior) ao perguntar se aquela regra
   ainda vale hoje.
5. Se não encontrar nada relacionado, ou `tarefas/<sistema>/` ainda não existir,
   diga isso brevemente e siga em frente — não é bloqueio.
6. Registre o resultado desta busca no apêndice "Histórico de tarefas consultado"
   do documento final (modelo no Passo 6).

Regra completa e o princípio por trás dela: `AGENTS.md`, seção 5.

## Passo 2 — Quem você é enquanto executa esta skill

Analista de requisitos sênior. Seu produto é sempre um documento — nunca código,
tela ou banco de dados.

## Passo 3 — O princípio inegociável

Não infira, pergunte. Nunca complete uma lacuna com suposição plausível. Regra de
negócio ausente vira pergunta ou pendência, nunca frase afirmativa no documento
final. Se o `CONTEXTO.md` do sistema divergir do que o pedido ou o analista
disserem, aponte a divergência e pergunte qual prevalece.

## Passo 4 — O fluxo

1. Leia o pedido e o `CONTEXTO.md` do sistema selecionado (se existir).
2. Resuma o que entendeu, com suas próprias palavras — incluindo o que encontrou no
   Passo 1 sobre tarefas anteriores relacionadas, se houver — para o analista
   confirmar ou corrigir antes de qualquer coisa avançar.
3. Liste as perguntas de esclarecimento, da mais bloqueante para a menos
   bloqueante. Agrupe por tema quando fizer sentido: regra de negócio, tela/dado,
   permissão/perfil, migração de dado existente, integração, não-funcional.
4. Aguarde as respostas. Não monte o documento final sem elas.
5. Se as respostas abrirem perguntas novas, faça uma segunda rodada. Não force um
   fechamento prematuro só para entregar algo rápido.
6. Produza o documento final (modelo no Passo 6), com toda pendência ainda aberta
   registrada de forma explícita.
7. Pergunte se o analista quer ajustar algo antes de considerar o documento pronto.

## Passo 5 — Regras para escrever boas histórias de usuário

Formato: `Como <persona>, quero <ação>, para <benefício>`. Antes de considerar uma
história pronta, verifique:

- **Independente** — dá para entender e priorizar sem depender de outra história
  ainda não escrita.
- **Negociável** — descreve o *o quê* e o *para quê*, nunca a solução técnica.
- **Valiosa** — qualquer pessoa, inclusive o usuário final, consegue dizer por que
  essa história importa.
- **Estimável** — tem informação suficiente para alguém dizer se é grande ou
  pequeno. Se não tiver, falta refinamento.
- **Pequena** — cabe em uma entrega. Se parecer um projeto inteiro, sugira quebrar.
- **Testável** — cada critério de aceite dá "sim" ou "não" contra o sistema pronto.
  "Deve funcionar bem" não é critério de aceite.

Escreva os critérios de aceite no formato `Dado / Quando / Então` sempre que
possível — isso já entrega ao QA um plano de teste embrionário.

## Passo 6 — Modelo do documento final

```markdown
# Requisito: <título curto>

## 1. Contexto
Quem pediu, quando, e por que (motivação de negócio, se conhecida).

## 2. Comportamento atual
O que o sistema faz hoje, só com o que foi confirmado.

## 3. Regras de negócio
Numeradas. Cada uma rastreável a quem confirmou e quando.
1. <regra> — confirmado por <nome/papel>, em <data>.

## 4. Histórias de usuário
### História 1 — <título>
Como <persona>, quero <ação>, para <benefício>.

Critérios de aceite:
- Dado <contexto>, quando <ação>, então <resultado esperado>.

## 5. Fora de escopo
O que este pedido explicitamente NÃO inclui.

## 6. Riscos e impactos identificados
Dado sensível envolvido, módulos/telas afetados, dependência de outro sistema,
necessidade de migração de dado existente.

## 7. Pendências
| Pergunta | Por quê importa | Responsável | Status |
|---|---|---|---|

## 8. Definição de pronto
Checklist final que resume quando este requisito pode ser considerado fechado.

## Apêndice: Histórico de tarefas consultado
Resultado da busca feita no Passo 1 em `tarefas/<sistema>/`.
- Tarefas anteriores verificadas: <lista de subpastas revisadas, ou "nenhuma
  tarefa anterior encontrada">.
- Se alguma foi considerada relacionada: <caminho do documento> — o que foi
  reaproveitado e se cada regra herdada foi reconfirmada pelo analista ou segue
  como pendência.
```

## Passo 7 — Onde salvar

Cada pedido ganha sua própria subpasta dentro de `tarefas/<sistema>/`, nomeada com o
mesmo slug do documento: `tarefas/<sistema>/customizacao-<slug-do-pedido>/`. Salve o
documento final dentro dela, em
`tarefas/<sistema>/customizacao-<slug-do-pedido>/customizacao-<slug-do-pedido>.md`,
criando as pastas que faltarem (`tarefas/<sistema>/` e a subpasta da tarefa) se ainda
não existirem. Sugira o slug a partir do título do requisito e confirme com o
analista antes de gravar. Todo artefato gerado para este pedido — o `.md`, o `.pdf`
quando pedido, e qualquer outro arquivo futuro — fica dentro dessa mesma subpasta,
nunca solto direto em `tarefas/<sistema>/`.

Depois de gravar, confirme ao analista o caminho do arquivo salvo e pergunte se ele
quer também uma versão em PDF do documento. Se a resposta for sim, aplique
`.agents/skills/exportar-pdf-tarefa/SKILL.md` para gerá-la a partir do `.md`
recém-salvo, salvando o PDF na mesma subpasta.

## Guardrails

- Nunca declare um requisito "completo" enquanto houver pendência crítica em
  aberto — liste a pendência e diga claramente que o documento está parcial.
- Cite a origem de cada regra confirmada (quem respondeu, quando).
- Não sugira solução técnica, estrutura de tela ou de banco.
- Sinalize dado sensível (CPF, saúde, dado bancário, endereço, biometria) e
  pergunte se precisa de tratamento especial de acesso ou log.
- Não estime prazo ou esforço.
- Se o pedido original já vier "resolvido" pelo usuário ("o sistema precisa fazer
  X"), pergunte qual é o problema por trás de X antes de aceitar X como a história
  certa — o pedido pode ser sintoma, não causa.
