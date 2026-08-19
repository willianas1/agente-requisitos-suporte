# Exemplo completo: de um chamado de suporte a uma tarefa de sustentação pronta

Caso **fictício**, no estilo de um chamado real de suporte em um sistema de gestão
de recursos humanos (como o SIGRH). Usa a skill `refinar-tarefa-sustentacao` — para
um exemplo de pedido de customização, veja `exemplo-customizacao-sigrh.md`.

Neste exemplo, o sistema `sigrh-ap` ainda não tem `CONTEXTO.md` configurado em
`contextos/`. Se tivesse uma seção "Comportamentos conhecidos que parecem bug e não
são", o agente teria consultado ali primeiro — e talvez o chamado nem precisasse
virar pendência técnica.

---

## Passo 0 — Seleção de sistema e tipo de tarefa

> Antes de começar: a qual sistema esse relato pertence? E — só confirmando — isso é
> algo que já deveria funcionar de um jeito e não está, ou é um pedido de algo novo?

> **Analista:** SIGRH-AP. É um problema, não é pedido novo — algo que já devia
> funcionar certo.

O agente identifica que é caso de `refinar-tarefa-sustentacao` e segue.

## Passo 1 — O relato bruto

> "A tela de acompanhamento de solicitações está mostrando um status diferente do
> relatório mensal. Pra alguns processos aparece 'Em análise' na tela, mas no
> relatório aparece 'Concluído'. Precisa corrigir isso o quanto antes."

## Passo 2 — Primeira resposta do agente

> **Meu entendimento (sintoma, sem causa ainda):** para pelo menos alguns processos,
> a tela de acompanhamento de solicitações mostra um status diferente do que
> aparece no relatório mensal — "Em análise" na tela contra "Concluído" no
> relatório.
>
> Antes de seguir, preciso confirmar:
>
> **Esperado vs. observado**
> 1. Qual dos dois hoje reflete o status real do processo — a tela ou o relatório?
> 2. Existe uma ordem conhecida de status (ex.: Em análise → Concluído)? Um processo
>    concluído pode voltar a "Em análise"?
>
> **Reprodução**
> 3. Isso acontece com todos os processos, ou só com um subconjunto? Consegue citar
>    1 ou 2 exemplos de protocolo pra eu ter algo concreto?
> 4. Já foi comparado o mesmo processo, na tela e no relatório, no mesmo momento?
>
> **Quando/ambiente**
> 5. Desde quando isso é percebido — é recente, ou pode já existir há mais tempo e
>    só agora foi notado?
> 6. Sabe se o relatório mensal é gerado automaticamente (agendado) ou sob demanda?
>
> **Impacto e urgência**
> 7. Esse relatório é usado em algo formal (gestão, auditoria, prestação de contas)?
> 8. Quantos processos, aproximadamente, já foram percebidos com essa divergência?
>
> **O que já foi verificado**
> 9. A comparação foi feita com o mesmo usuário/perfil nos dois lugares, ou pode ser
>    diferença de permissão entre quem vê a tela e quem gera o relatório?

## Passo 3 — Respostas do analista (simuladas)

> 1. A tela reflete o status real — é o que o processo tem hoje.
> 2. Sim: normalmente Em análise → Concluído. Mas um processo reaberto volta pra Em
>    análise.
> 3. Só notei em processos que foram reabertos depois de já estarem concluídos.
>    Tenho dois exemplos: 2026/000123 e 2026/000456.
> 4. Sim, consultei os dois ao mesmo tempo, pros dois exemplos, e a divergência se
>    repete nos dois.
> 5. Comecei a notar essa semana, mas pode já existir há mais tempo — só não tinha
>    reparado antes.
> 6. Não sei dizer com certeza. Acho que é agendado, mas nunca perguntei pro time
>    técnico.
> 7. Sim, é usado pela gestão pra acompanhar volume de processos concluídos no mês.
> 8. Só percebi esses 2 até agora — não fiz uma busca sistemática nos demais.
> 9. Não, usei meu próprio usuário nos dois. Mas não sei se o relatório aplica
>    alguma regra de permissão diferente da tela.

## Passo 4 — Segunda rodada

A resposta 6 (relatório talvez seja agendado) mais o padrão "só em processos
reabertos" da resposta 3 sugerem uma hipótese técnica — mas o agente não a declara
como causa confirmada:

> Juntando a resposta 3 (só acontece em processos reabertos) com a 6 (relatório
> pode ser agendado), a hipótese mais provável é que o relatório usa um dado gerado
> antes da reabertura e não é recalculado depois. **Isso é uma hipótese, não um
> fato confirmado** — só o time técnico consegue validar se existe essa defasagem.
> Vou registrar como pendência e seguir com o restante do documento. Confirma?

> **Analista:** Confirmo, pode registrar como pendência. Isso eu realmente não sei
> responder.

## Passo 5 — Documento final produzido pelo agente

```markdown
# Tarefa de sustentação: divergência de status entre tela de acompanhamento e relatório gerencial mensal

## 1. Contexto
Relatado pelo setor de protocolo, via chamado, nesta semana. Sistema: SIGRH-AP
(contexto de sistema não carregado nesta sessão — perguntas genéricas).

## 2. Sintoma relatado
"A tela de acompanhamento de solicitações mostra um status diferente do relatório
mensal. Em alguns processos aparece 'Em análise' na tela, mas 'Concluído' no
relatório."

## 3. Comportamento esperado vs. observado
| Esperado | Observado |
|---|---|
| Tela e relatório devem mostrar o mesmo status para o mesmo processo — a tela é a fonte do status real, confirmado pelo analista | Em processos reabertos após já terem sido concluídos, a tela mostra "Em análise" mas o relatório mensal ainda mostra "Concluído" — confirmado com 2 exemplos |

## 4. Passos para reproduzir
1. Localizar um processo já concluído e depois reaberto (ex.: protocolo
   2026/000123).
2. Consultar o status na tela de acompanhamento de solicitações.
3. Consultar o mesmo processo no relatório gerencial mensal do período
   correspondente.
4. Comparar os dois status.

Reproduzido com o mesmo usuário/perfil nas duas consultas — descarta diferença de
permissão entre tela e relatório nos 2 casos testados.

## 5. Hipóteses e evidências
- **Hipótese (não confirmada):** o relatório mensal pode ser gerado por rotina
  agendada que não recalcula em tempo real, ficando desatualizado quando um
  processo é reaberto após a última geração. Requer confirmação técnica.
- Descartado: diferença de permissão/perfil entre quem consulta a tela e quem gera
  o relatório — mesmo usuário testado nos dois.
- Não verificado: se o padrão se repete em processos reabertos que **não** estejam
  entre os 2 exemplos citados.

## 6. Impacto e urgência
- Relatório mensal é usado pela gestão para acompanhar volume de processos
  concluídos no mês — divergência pode inflar esse número.
- Casos identificados até agora: 2 (sem busca sistemática ainda).
- Sem contorno conhecido enquanto não for corrigido — a divergência não é visível
  para quem consulta só o relatório.

## 7. Fora de escopo
- Este chamado não cobre revisão de todos os relatórios do sistema — só o
  relatório gerencial mensal de protocolos.
- Não cobre mudança na regra de reabertura de processo, só a divergência de
  exibição entre tela e relatório.

## 8. Pendências
| Pergunta | Por quê importa | Responsável | Status |
|---|---|---|---|
| O relatório mensal é gerado por rotina agendada ou sob demanda? Existe defasagem conhecida entre o dado da tela e o do relatório? | Confirma ou descarta a hipótese principal de causa | Time técnico | Aberta |
| Quantos processos, no total, estão hoje com essa divergência? | Dimensiona o impacto real, além dos 2 exemplos relatados | Setor de protocolo / time técnico | Aberta |

## 9. Critério de resolução
- Para um processo reaberto após conclusão, a tela e o relatório mensal do período
  corrente devem mostrar o mesmo status.
- Processos históricos já divergentes devem ser identificados e, no mínimo,
  listados — critério final depende da resposta à segunda pendência.
```

O agente salva esse conteúdo em
`tarefas/sigrh-ap/sustentacao-divergencia-status-tela-relatorio.md`, confirmando o
nome do arquivo com o analista antes de gravar.

---

## O que observar neste exemplo, em contraste com o de customização

- O agente **não declarou uma causa técnica** (ex.: "é problema de cache" ou "é
  falha de sincronização") em nenhum momento — mesmo tendo elementos suficientes
  para "parecer" óbvio. A hipótese ficou marcada como hipótese.
- O documento final tem seção própria de **hipóteses e evidências**, que não existe
  no modelo de customização — porque aqui o trabalho é diagnóstico, não desenho de
  comportamento novo.
- Assim como no exemplo de customização, a segunda rodada não foi pulada: uma
  combinação de respostas sugeriu algo novo, e isso virou pendência explícita, não
  suposição silenciosa.
- Se `contextos/sigrh-ap/CONTEXTO.md` já tivesse uma nota do tipo "relatórios
  agendados podem ficar até X horas defasados da tela", essa pendência inteira
  poderia ter sido resolvida na hora, sem precisar acionar o time técnico.
