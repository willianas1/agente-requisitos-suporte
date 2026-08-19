# Exemplo completo: de um pedido pobre a um requisito de customização pronto

Caso **fictício**, no estilo de um pedido real de customização em um sistema de
gestão de recursos humanos (como o SIGRH). Usa a skill
`refinar-requisito-customizacao` — para um exemplo do outro tipo (chamado de
sustentação), veja `exemplo-sustentacao-sigrh.md`.

Serve para treinar o fluxo antes de usar com um caso real do seu time. Neste
exemplo, o sistema `sigrh-ap` ainda não tem `CONTEXTO.md` configurado em
`contextos/` — por isso o agente atua como analista de requisitos generalista,
sem nome de tela ou módulo real. Com contexto carregado (via skill
`carregar-contexto-sistema`), as perguntas tendem a ficar mais específicas para
o sistema.

---

## Passo 0 — Seleção de sistema

O analista abre o harness de IA de sua escolha na pasta do projeto e cola o pedido.
Antes de responder, a skill `refinar-requisito-customizacao` pergunta:

> Antes de começar: a qual sistema esse pedido pertence? Não encontrei nenhuma pasta
> em `contextos/` ainda — posso seguir normalmente como analista generalista, ou
> prefere que a gente carregue o contexto desse sistema primeiro?

> **Analista:** Segue como generalista mesmo por enquanto. É o SIGRH-AP.

O agente registra que esta sessão trata do sistema `sigrh-ap` e segue.

## Passo 1 — O pedido bruto

Isto é tudo que chegou, por chamado, do setor de RH:

> "Precisamos que dê pra anexar mais de um comprovante de residência no cadastro do
> servidor. Hoje só aceita um arquivo."

Uma frase. Nenhuma regra, nenhum limite, nenhuma tela citada, nenhuma justificativa.

## Passo 2 — Primeira resposta do agente

> **Meu entendimento:** hoje o cadastro do servidor aceita apenas 1 (um) arquivo
> como comprovante de residência. A demanda é permitir múltiplos arquivos nesse
> mesmo campo.
>
> Antes de seguir, preciso confirmar:
>
> **Regra de negócio**
> 1. Existe um número máximo de comprovantes permitidos, ou pode ser ilimitado?
> 2. Ao anexar um novo comprovante, o(s) anterior(es) deve(m) ser substituído(s) ou
>    mantido(s) como histórico?
> 3. O servidor pode remover um comprovante já anexado, ou só adicionar?
>
> **Dados e validação**
> 4. Quais tipos de arquivo devem ser aceitos (PDF, imagem, os dois)? Há limite de
>    tamanho por arquivo?
> 5. A validação que existe hoje (tipo, tamanho) deve continuar valendo para os
>    novos anexos, ou muda?
>
> **Dado já existente**
> 6. Servidores que já têm 1 comprovante cadastrado precisam de alguma ação, ou o
>    comprovante existente passa a ser simplesmente "o primeiro da lista"?
>
> **Permissão**
> 7. Quem pode anexar — só o próprio servidor pelo portal, ou também um operador do
>    RH pela tela interna?
>
> **Motivação**
> 8. Existe algum prazo ou norma específica motivando esse pedido, ou é uma melhoria
>    operacional sem prazo formal?

## Passo 3 — Respostas do analista (simuladas)

> 1. Vamos limitar a 3 comprovantes.
> 2. Mantidos como histórico, não substitui.
> 3. Sim, pode remover — mas só se ainda não tiver sido usado em nenhum processo
>    formal. Não sei se dá pra verificar isso tecnicamente.
> 4. PDF e imagem (jpg/png), até 5MB cada — igual já é hoje.
> 5. Sim, mesma validação de hoje.
> 6. Não precisa de ação. O comprovante existente vira o primeiro da lista.
> 7. Só o próprio servidor, pelo portal. O RH não mexe nisso.
> 8. Não tem norma específica — foi pedido informal da Secretaria, porque há
>    servidor com endereço dividido entre duas residências (função em cidade
>    diferente da residência principal).

## Passo 4 — Segunda rodada (o agente não fecha no primeiro round)

A resposta 3 abre uma pergunta nova em vez de fechar o assunto — e é isso que o
guardrail de "não infira" deveria produzir:

> A resposta 3 traz uma condição nova: "só pode remover se não tiver sido usado em
> nenhum processo formal". Preciso entender melhor antes de transformar isso em
> critério de aceite:
>
> 3a. O que conta como "processo formal" aqui — algum processo específico que
>     consome esse comprovante (ex.: recadastramento, auditoria)? Existe hoje algum
>     jeito de saber, no sistema, se um documento já foi "usado" nesse sentido?
>
> Se essa informação não existir hoje, vou registrar como **pendência crítica** e
> seguir com o restante do requisito, já que as demais respostas fecham o essencial.
> Você confirma que posso seguir assim?

> **Analista:** Confirmo. Isso realmente não dá pra responder agora — precisa
> validar com o time técnico se existe rastreabilidade de uso. Registra como
> pendência mesmo.

## Passo 5 — Documento final produzido pelo agente

```markdown
# Requisito: múltiplos comprovantes de residência no cadastro do servidor

## 1. Contexto
Pedido informal da Secretaria, via chamado do setor de RH: servidores com função em
cidade diferente da residência principal têm mais de um endereço válido e precisam
anexar mais de um comprovante de residência. Hoje o cadastro aceita apenas 1 arquivo
nesse campo.

## 2. Comportamento atual
O cadastro do servidor permite anexar exatamente 1 (um) arquivo como comprovante de
residência. Um novo upload substitui o anterior.

## 3. Regras de negócio
1. O sistema deve aceitar até 3 comprovantes de residência por servidor —
   confirmado pelo analista de RH, nesta sessão.
2. Um novo anexo é somado à lista, não substitui os anteriores (histórico) —
   confirmado pelo analista de RH, nesta sessão.
3. O servidor pode remover um comprovante já anexado — confirmado pelo analista de
   RH, nesta sessão. **Condição da remoção depende da pendência #1** (ver seção 7).
4. Tipos de arquivo aceitos: PDF, JPG, PNG. Tamanho máximo: 5MB por arquivo — mesma
   regra já aplicada hoje, confirmado pelo analista de RH.
5. Comprovantes já cadastrados antes desta mudança não precisam de nenhuma ação de
   migração; passam a ser o primeiro item da lista — confirmado pelo analista de RH.
6. Apenas o próprio servidor, pelo portal, pode anexar ou remover comprovantes. Não
   há ação equivalente para operador de RH pela tela interna — confirmado pelo
   analista de RH.

## 4. Histórias de usuário

### História 1 — Anexar novo comprovante de residência
Como servidor, quero anexar um novo comprovante de residência sem perder os que já
cadastrei, para manter comprovação válida de mais de um endereço.

Critérios de aceite:
- Dado que já tenho 1 ou 2 comprovantes cadastrados, quando anexo um novo arquivo
  válido (PDF/JPG/PNG, até 5MB), então o novo arquivo é somado à lista, sem remover
  os anteriores.
- Dado que já tenho 3 comprovantes cadastrados, quando tento anexar um quarto,
  então o sistema bloqueia o envio e exibe mensagem informando o limite.
- Dado que envio um arquivo fora do tipo ou tamanho permitido, então o sistema
  rejeita o envio com a mesma mensagem de validação já usada hoje.

### História 2 — Remover comprovante de residência
Como servidor, quero remover um comprovante que anexei por engano ou que não uso
mais, para manter minha lista de comprovantes atualizada.
**Pendente da resposta à pendência #1 (ver seção 7) — critérios de aceite abaixo
são provisórios.**

Critérios de aceite (provisórios):
- Dado um comprovante que não foi usado em nenhum processo formal, quando o
  servidor solicita a remoção, então o comprovante é removido da lista.
- Dado um comprovante já usado em processo formal, quando o servidor tenta remover,
  então o sistema bloqueia a remoção — **regra de bloqueio ainda não definida, ver
  pendência #1**.

### História 3 — Visualizar histórico de comprovantes
Como servidor, quero ver todos os comprovantes de residência que já anexei, para
saber quais estão válidos hoje.

Critérios de aceite:
- Dado que tenho 1 a 3 comprovantes cadastrados, quando acesso a tela de cadastro,
  então vejo a lista completa, com data de anexo de cada um.

## 5. Fora de escopo
- Este pedido não inclui nenhuma ação para operadores de RH pela tela interna.
- Não inclui exigência legal formal — o pedido é operacional, sem norma vinculada.
- Não inclui alteração em processos que hoje consomem o comprovante de residência
  (esses processos são apenas referenciados na pendência #1, não alterados aqui).

## 6. Riscos e impactos identificados
- Comprovante de residência é documento com dado pessoal (endereço do servidor) —
  deve seguir a mesma política de acesso e retenção já aplicada a outros anexos do
  cadastro. Nenhuma mudança de política foi solicitada; sinalizado para revisão do
  time responsável por dados sensíveis, se aplicável.
- Aumento de storage por servidor (até 3 arquivos de 5MB, ante 1 hoje) — sem
  decisão sobre isso neste requisito; sinalizar para quem avalia capacidade.

## 7. Pendências
| Pergunta | Por quê importa | Responsável | Status |
|---|---|---|---|
| O que conta como "processo formal" que usa o comprovante, e existe rastreabilidade técnica desse uso hoje? | Sem isso, a regra de remoção (História 2) não pode ser implementada como descrita | Time técnico / negócio | Aberta |

## 8. Definição de pronto
- [ ] Pendência #1 respondida e história 2 atualizada com critério de aceite final.
- [ ] Regras de negócio revisadas e confirmadas por quem tem autoridade para
      aprovar regra de RH (se diferente do analista que respondeu nesta sessão).
- [ ] Documento revisado por quem for assumir a especificação técnica.
```

O agente salva esse conteúdo em
`tarefas/sigrh-ap/customizacao-multiplos-comprovantes-residencia/customizacao-multiplos-comprovantes-residencia.md`,
confirmando o nome da subpasta e do arquivo com o analista antes de gravar.

---

## O que observar neste exemplo

- A sessão começou confirmando o sistema (`sigrh-ap`), mesmo sendo o único
  configurado — hábito que evita erro quando um segundo sistema existir.
- O agente **não assumiu** o limite de comprovantes, nem o comportamento de
  substituição — ambos vieram de pergunta respondida, não de suposição.
- Quando a resposta 3 abriu uma pergunta nova, o agente **não ignorou** para fechar
  mais rápido — fez a segunda rodada.
- O documento final **não finge que está 100% fechado**: a História 2 está marcada
  como provisória e a pendência aparece de forma explícita, com dono e status.
- Nada neste fluxo exigiu acesso a código, banco de dados ou repositório do sistema.
- Se `contextos/sigrh-ap/CONTEXTO.md` já existisse (com glossário de módulos, telas
  e comportamentos conhecidos), várias dessas 8 perguntas iniciais poderiam ter
  vindo pré-respondidas ou mais específicas — esse é o ganho de rodar a skill
  `carregar-contexto-sistema` antes de usar o agente no dia a dia.
