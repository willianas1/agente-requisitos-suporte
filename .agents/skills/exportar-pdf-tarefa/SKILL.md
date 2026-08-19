---
name: exportar-pdf-tarefa
description: Gera a versão em PDF de um documento de tarefa já refinado e salvo em tarefas/<sistema>/<tipo>-<slug>/*.md (requisito de customização ou tarefa de sustentação). Use depois que o documento final já foi salvo em .md pela skill de refinamento correspondente e o analista confirmar explicitamente que quer também um PDF. Também serve para gerar o PDF de um documento salvo anteriormente, quando pedido depois, fora do fluxo de refinamento. Nunca gera PDF de um documento ainda incompleto ou não aprovado pelo analista.
---

# exportar-pdf-tarefa

## Quando usar

- Logo depois que `refinar-requisito-customizacao` ou `refinar-tarefa-sustentacao`
  salvarem o documento final em `.md` e o analista confirmar que quer também uma
  versão em PDF.
- Quando o analista pedir, em qualquer momento depois, para gerar (ou regerar) o
  PDF de uma tarefa que já está salva em `tarefas/<sistema>/<tipo>-<slug>/`.
- **Nunca** rode esta skill por conta própria, sem o analista ter pedido ou aceito
  explicitamente — gerar PDF não é o padrão automático, é uma oferta.

## Passo 1 — Confirme qual documento

- Se não estiver óbvio pelo contexto da conversa, pergunte qual arquivo `.md` em
  `tarefas/<sistema>/<tipo>-<slug>/` deve virar PDF.
- Se o documento ainda tiver pendência crítica em aberto na seção "Pendências", ou
  não tiver sido explicitamente aprovado pelo analista como versão final da sessão,
  avise que essas pendências também vão aparecer no PDF e confirme que ele quer
  gerar assim mesmo.

## Passo 2 — Escolha o método disponível no harness atual, nesta ordem

1. **Capacidade nativa de gerar PDF/documento do harness em uso** — por exemplo, a
   skill de PDF já embutida no ambiente (como a skill `pdf` do Claude Code), o Code
   Interpreter do ChatGPT, ou recurso equivalente de outro harness. Se disponível,
   use-a para converter o conteúdo do `.md`, preservando títulos, tabelas e listas
   como estão no documento — é o caminho preferido, porque não depende de instalar
   nada.
2. **`pandoc`**, se o harness puder rodar comando de terminal e o pandoc estiver
   instalado no ambiente:
   ```bash
   pandoc "tarefas/<sistema>/<tipo>-<slug>/<arquivo>.md" -o "tarefas/<sistema>/<tipo>-<slug>/<arquivo>.pdf" --standalone -V lang=pt-BR
   ```
   Se faltar motor de PDF (erro citando `xelatex`, `wkhtmltopdf` ou similar), tente:
   ```bash
   pandoc "tarefas/<sistema>/<tipo>-<slug>/<arquivo>.md" -o "tarefas/<sistema>/<tipo>-<slug>/<arquivo>.pdf" --pdf-engine=wkhtmltopdf
   ```
   Se nenhum motor estiver disponível, informe ao analista qual dependência falta —
   não instale nada no sistema sem avisar e obter confirmação antes.
3. **Sem nenhuma capacidade de gerar PDF neste ambiente**: diga isso claramente ao
   analista, não finja ter gerado o arquivo, e ofereça alternativas manuais:
   - abrir o `.md` num editor com exportação para PDF (ex.: VS Code com a extensão
     "Markdown PDF", Typora, Obsidian);
   - colar o conteúdo num editor de texto (Word, Google Docs) e exportar para PDF
     por lá.

## Passo 3 — Onde salvar o PDF

Salve com o mesmo nome-base do `.md`, na mesma subpasta da tarefa
`tarefas/<sistema>/<tipo>-<slug>/`, trocando só a extensão — ex.:
`tarefas/sigrh/customizacao-anexo-multiplo/customizacao-anexo-multiplo.md` →
`tarefas/sigrh/customizacao-anexo-multiplo/customizacao-anexo-multiplo.pdf`. Nunca
salve o PDF solto direto em `tarefas/<sistema>/`, fora da subpasta da tarefa.
Confirme ao analista o caminho do arquivo gerado.

## Guardrails

- Nunca gere PDF de um documento que o analista ainda não aprovou como a versão
  final da sessão atual.
- Nunca invente ou "melhore" conteúdo ao converter — o PDF deve ser fiel ao `.md`
  salvo, sem resumir, reformatar sentido ou omitir pendências.
- Se o `.md` de origem for editado depois de gerado o PDF, o PDF antigo fica
  desatualizado — avise o analista e ofereça gerar de novo, não deixe os dois
  arquivos divergirem silenciosamente.
- Não instale software ou dependência de sistema (pandoc, motor de PDF etc.) sem
  avisar e confirmar com o analista antes.
