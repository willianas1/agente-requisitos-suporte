---
name: carregar-contexto-sistema
description: Cria ou atualiza o contexto de um sistema específico em contextos/<sistema>/CONTEXTO.md, a partir dos documentos-fonte em contextos/<sistema>/fontes/. Use sempre que o analista disser que quer carregar, configurar, adicionar ou atualizar contexto de um sistema, apontar para documentos novos, ou registrar um sistema novo neste agente. Nunca decide sozinho: lista fatos confirmados, contradições e lacunas, e só grava após aprovação explícita do analista.
---

# carregar-contexto-sistema

## Quando usar

- O analista pede para carregar, configurar, adicionar ou atualizar o contexto de
  um sistema.
- O analista aponta para documentos novos e pede para o agente "aprender" sobre um
  sistema.
- É a primeira vez que um sistema vai ser usado neste agente.

## Passo 0 — Sistema novo ou existente?

1. Pergunte o nome do sistema e derive um slug em kebab-case (ex.: "SIGRH-AP" →
   `sigrh-ap`; "SIGA" → `siga`).
2. Se `contextos/<slug>/` **não existir**: crie `contextos/<slug>/` e
   `contextos/<slug>/fontes/`, e inicialize `contextos/<slug>/CONTEXTO.md` a partir
   da estrutura de `contextos/_template-contexto.md`.
3. Se `contextos/<slug>/` **já existir**: confirme com o analista que é uma
   atualização, não uma criação, e informe a data da última atualização registrada
   no rodapé do `CONTEXTO.md` atual, se houver.

## Passo 1 — Leia as fontes

Leia todos os arquivos em `contextos/<slug>/fontes/`, incluindo os novos ou
alterados desde a última vez que esta skill processou esse sistema.

## Passo 2 — Separe fato de lacuna

1. Liste os fatos que você conseguiu confirmar, indicando de qual arquivo cada um
   veio.
2. Liste contradições entre documentos diferentes, se houver — sem tentar
   resolvê-las sozinho.
3. Liste lacunas: o que um analista de requisitos ou de suporte provavelmente vai precisar saber
   e que ainda não está coberto por nenhum documento (ex.: sigla de módulo sem
   explicação, integração citada mas não descrita, tela mencionada sem contexto).
4. Não preencha as lacunas com suposição — apresente cada uma como pergunta para o
   time responder, mesmo que a resposta só venha depois desta sessão.

## Passo 3 — Proponha a atualização

Monte a proposta de `contextos/<slug>/CONTEXTO.md`, organizada por tema:

- visão geral do sistema;
- glossário de módulos/siglas;
- telas e relatórios relevantes para quem atende usuário;
- integrações com outros sistemas, quando existirem;
- comportamentos conhecidos que parecem bug e não são;
- outras notas relevantes para refinamento de requisito.

Mostre a proposta completa ao analista antes de gravar qualquer coisa.

## Passo 4 — Aguarde aprovação

Só grave em `contextos/<slug>/CONTEXTO.md` depois de aprovação explícita do
analista. Nunca sobrescreva silenciosamente um `CONTEXTO.md` já existente. Ao
gravar, atualize a tabela "Histórico de atualizações" no fim do arquivo com a data
e um resumo do que mudou.

## Guardrails

- Nunca misture fontes de sistemas diferentes num mesmo `CONTEXTO.md` — se um
  documento em `fontes/` parecer pertencer a outro sistema, avise e pergunte antes
  de usá-lo.
- Nunca copie dado pessoal real de usuário (CPF, nome completo, endereço, dado de
  saúde) para dentro de `fontes/` ou `CONTEXTO.md` — se um documento-fonte tiver
  isso, avise e peça para anonimizar antes.
- Contexto desatualizado é pior do que contexto ausente, porque parece confiável
  sem ser — sugira revisão periódica (ex.: a cada trimestre, ou sempre que um
  documento-fonte mudar).

## Dica de uso contínuo

Rode esta skill de novo sempre que o time perceber, durante o uso real, que uma
pergunta genérica se repete e já poderia ter resposta fixa no contexto — adicione
essa resposta como um documento novo em `contextos/<slug>/fontes/` e rode a skill
outra vez. O `CONTEXTO.md` deve crescer pelo uso, não só pela criação inicial.
