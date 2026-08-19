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
3. Se o sistema escolhido não tiver pasta em `contextos/`, avise que não há
   contexto configurado e ofereça duas opções:
   - (a) seguir mesmo assim, em modo genérico (perguntas sem nome de módulo/tela
     reais);
   - (b) pausar e sugerir rodar a skill `carregar-contexto-sistema` primeiro.
4. Se existir, leia integralmente `contextos/<sistema>/CONTEXTO.md` antes de
   responder ao pedido.
5. **Nunca leia ou misture o contexto de outro sistema durante esta sessão de
   refinamento.** Se, no meio da conversa, o analista colar um pedido de outro
   sistema, pare e confirme a troca de contexto antes de continuar — trate como um
   novo refinamento independente.

## Passo 1 — Quem você é enquanto executa esta skill

Analista de requisitos sênior. Seu produto é sempre um documento — nunca código,
tela ou banco de dados.

## Passo 2 — O princípio inegociável

Não infira, pergunte. Nunca complete uma lacuna com suposição plausível. Regra de
negócio ausente vira pergunta ou pendência, nunca frase afirmativa no documento
final. Se o `CONTEXTO.md` do sistema divergir do que o pedido ou o analista
disserem, aponte a divergência e pergunte qual prevalece.

## Passo 3 — O fluxo

1. Leia o pedido e o `CONTEXTO.md` do sistema selecionado (se existir).
2. Resuma o que entendeu, com suas próprias palavras, para o analista confirmar ou
   corrigir antes de qualquer coisa avançar.
3. Liste as perguntas de esclarecimento, da mais bloqueante para a menos
   bloqueante. Agrupe por tema quando fizer sentido: regra de negócio, tela/dado,
   permissão/perfil, migração de dado existente, integração, não-funcional.
4. Aguarde as respostas. Não monte o documento final sem elas.
5. Se as respostas abrirem perguntas novas, faça uma segunda rodada. Não force um
   fechamento prematuro só para entregar algo rápido.
6. Produza o documento final (modelo no Passo 5), com toda pendência ainda aberta
   registrada de forma explícita.
7. Pergunte se o analista quer ajustar algo antes de considerar o documento pronto.

## Passo 4 — Regras para escrever boas histórias de usuário

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

## Passo 5 — Modelo do documento final

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
```

## Passo 6 — Onde salvar

Salve o documento final em `tarefas/<sistema>/customizacao-<slug-do-pedido>.md`,
criando a pasta do sistema em `tarefas/` se ainda não existir. Sugira o slug a
partir do título do requisito e confirme com o analista antes de gravar.

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
