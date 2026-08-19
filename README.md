# Agente Especializado em Análise de Requisitos

**Este é um Agente Especializado em Análise de Requisitos** — não um assistente
genérico. Seu único produto é documento: requisito de customização ou tarefa de
sustentação, sempre construído a partir de pergunta ao analista, nunca de
suposição. Ele não escreve código, não desenha tela, não decide banco de dados e
não estima prazo — isso fica para a etapa seguinte, com outro dono.

Projeto **agnóstico de harness**: funciona com qualquer ferramenta de IA agêntica
capaz de ler arquivo de contexto e navegar pastas — Claude Code, Codex, Cursor,
Windsurf — e também, colando o conteúdo manualmente como instrução de projeto, com
ChatGPT ou Claude.ai em modo Projeto. **Não precisa de acesso a nenhum repositório
de sistema.**

Pensado para operar **mais de um sistema ao mesmo tempo**: o mesmo agente pode
refinar tarefas do Sistema A numa sessão e do Sistema B em outra, sem misturar
contexto entre eles.

## Estrutura

| Caminho | Para quê |
|---|---|
| `AGENTS.md` | Identidade, princípio "não infira, pergunte" e a regra de nunca misturar sistemas |
| `CLAUDE.md` | Redireciona para `AGENTS.md` — compatibilidade com harnesses que carregam esse nome de arquivo automaticamente (Claude Code); os demais leem `AGENTS.md` direto |
| `.agents/skills/refinar-requisito-customizacao/` | Pedido de algo **novo** → documento de requisito com histórias de usuário |
| `.agents/skills/refinar-tarefa-sustentacao/` | Relato de algo que **já deveria funcionar** e não está → documento de investigação/resolução |
| `.agents/skills/carregar-contexto-sistema/` | Skill de onboarding: documentos brutos de um sistema → `CONTEXTO.md` curado |
| `.agents/skills/exportar-pdf-tarefa/` | Gera a versão em PDF de uma tarefa já salva em `.md`, sob confirmação do analista |
| `contextos/<sistema>/` | Um por sistema: `CONTEXTO.md` (curado) + `fontes/` (bruto) |
| `tarefas/<sistema>/` | Onde os documentos finais são salvos, por sistema — sempre em `.md`, e em `.pdf` também quando pedido |
| `exemplos/` | Passo a passo narrado, do pedido curto ao documento final |

## Duas skills de refinamento — como escolher

| | `refinar-requisito-customizacao` | `refinar-tarefa-sustentacao` |
|---|---|---|
| Quando | Pedido de algo que **ainda não existe** | Relato de algo que **já deveria funcionar** e não está |
| Exemplo | "Precisamos que dê pra anexar mais de um arquivo aqui" | "A tela mostra um status e o relatório mostra outro" |
| Produto | Regras de negócio + histórias de usuário + critérios de aceite | Sintoma + esperado×observado + hipóteses + impacto |

Na dúvida, o próprio agente pergunta: *"isso é algo que ainda não existe, ou um
problema com algo que já deveria funcionar?"* — não precisa decidir sozinho antes
de colar o pedido.

## Como começar

1. Copie esta pasta inteira (ou clone o repositório) para o seu computador.
2. Abra o harness de IA da sua escolha dentro desta pasta (Claude Code, Codex,
   Cursor...) — ou, usando ChatGPT/Claude.ai, crie um Projeto e cole o conteúdo de
   `AGENTS.md` como instrução, adicionando os arquivos de `contextos/` como anexos.
3. Peça para configurar o primeiro sistema: *"carrega o contexto do sistema X"* — a
   skill `carregar-contexto-sistema` entra em ação. Pode pular este passo e seguir
   sem contexto de sistema (fica com perguntas mais genéricas).
4. Cole um pedido ou relato — mesmo curto e incompleto — e peça para refinar. O
   agente identifica (ou pergunta) se é customização ou sustentação, confirma o
   sistema, e conduz o fluxo de esclarecimento até o documento final.
5. O documento final é sempre salvo como `.md` em `tarefas/<sistema>/` — o agente
   confirma o caminho salvo e, em seguida, pergunta se você também quer uma versão
   em PDF. Aceitando, ele gera o PDF ao lado do `.md`, no mesmo nome de arquivo.

Veja `exemplos/` para dois casos completos narrados — um de cada tipo.

## Prompts prontos para copiar e colar

Não é obrigatório usar exatamente estes textos — uma frase natural também aciona a
skill certa. Mas, para começar rápido, copie o bloco da skill que precisa, troque o
que está entre `[colchetes]` e cole direto na janela do agente.

### Carregar (ou atualizar) o contexto de um sistema

Aciona `carregar-contexto-sistema`.

```
Quero carregar (ou atualizar) o contexto do sistema abaixo neste agente.

Sistema: [nome do sistema, ex.: SIGRH]

Vou colar ou anexar os documentos-fonte que você deve usar para montar o
CONTEXTO.md (manuais, prints de tela, FAQs internas, respostas antigas de dúvida
etc.).

Depois de ler tudo, ANTES de gravar qualquer coisa, me mostre:
1. os fatos que você conseguiu confirmar, e de qual documento cada um veio;
2. contradições entre os documentos, se houver;
3. lacunas que um analista de requisitos ou suporte provavelmente vai precisar
   saber e que nenhum documento ainda cobre.

Só grave o CONTEXTO.md depois que eu aprovar a proposta.

--- DOCUMENTOS-FONTE ---
[cole aqui o conteúdo dos documentos, ou liste/anexe os arquivos]
```

### Refinar um pedido de algo novo (customização)

Aciona `refinar-requisito-customizacao`. Use quando o pedido é sobre algo que o
sistema **ainda não faz**.

```
Quero refinar o pedido abaixo como REQUISITO DE CUSTOMIZAÇÃO — é algo que o
sistema ainda não faz hoje.

Sistema: [nome do sistema, ex.: SIGRH]
Quem pediu: [nome / cargo / área, se souber]
Canal: [nº do chamado / e-mail / conversa verbal / outro]
Data do pedido: [dd/mm/aaaa]

Pedido original, na íntegra e sem edição:
"""
[cole aqui o texto literal do chamado, e-mail ou mensagem do usuário]
"""

Siga o fluxo normal: resuma o que você entendeu para eu confirmar, faça as
perguntas de esclarecimento que faltarem (uma rodada de cada vez, das mais
bloqueantes para as menos), e só monte o documento final depois que eu responder.
Não assuma nada que eu não tiver confirmado.
```

### Refinar um relato de algo que já deveria funcionar (sustentação)

Aciona `refinar-tarefa-sustentacao`. Use quando o relato é sobre algo que **já
existe** e não está se comportando como deveria.

```
Quero refinar o relato abaixo como TAREFA DE SUSTENTAÇÃO — é algo que já deveria
funcionar de um jeito e não está.

Sistema: [nome do sistema, ex.: SIGRH]
Quem relatou: [nome / cargo / área, se souber]
Canal: [nº do chamado / e-mail / conversa verbal / outro]
Data do relato: [dd/mm/aaaa]
Módulo/tela, se souber: [ex.: Folha de Pagamento > Rescisão]

Relato original, na íntegra e sem edição:
"""
[cole aqui o texto literal do chamado, e-mail ou mensagem do usuário]
"""

Siga o fluxo normal: resuma o sintoma com suas próprias palavras, sem nomear uma
causa ainda, faça as perguntas de esclarecimento priorizadas, e só monte o
documento final depois que eu responder. Toda hipótese de causa fica marcada como
hipótese até eu confirmar com evidência técnica.
```

### Gerar o PDF de uma tarefa já salva

Aciona `exportar-pdf-tarefa`. Use depois que o `.md` final já existir em
`tarefas/<sistema>/`.

```
Já salvamos o documento final da tarefa abaixo em .md. Quero também a versão em
PDF, ao lado do arquivo original, com o mesmo nome.

Arquivo: tarefas/[sistema]/[nome-do-arquivo].md

Depois de gerar, me confirme o caminho do PDF.
```

Mais contexto sobre quando cada skill é acionada: `AGENTS.md`, seção 4.

## Geração de PDF

A skill `exportar-pdf-tarefa` converte o `.md` de uma tarefa já salva para PDF,
tentando nesta ordem: (1) a capacidade nativa de gerar PDF/documento do harness em
uso, se houver; (2) `pandoc` via terminal, se instalado no ambiente; (3), na falta
das duas, o agente avisa e sugere exportar manualmente (VS Code com extensão
Markdown PDF, Word/Google Docs). O PDF nunca é gerado sem confirmação explícita do
analista, nem de um documento ainda com pendência crítica não assumida. Detalhes em
`.agents/skills/exportar-pdf-tarefa/SKILL.md`.

## Operando dois (ou mais) sistemas em paralelo

Abra uma sessão/instância do seu harness por sistema, apontando as duas para esta
mesma pasta — cada sessão pergunta qual sistema está em jogo e só lê o
`CONTEXTO.md` daquele sistema. Ou use uma sessão só e confirme a troca de sistema
sempre que o agente perguntar. Detalhes em `contextos/README.md`.

## Funciona sem nenhum sistema configurado ainda?

Funciona — as perguntas ficam mais genéricas, sem nome de módulo ou tela reais. O
ganho de ter pergunta certa em vez de suposição já existe desde o primeiro uso.
`contextos/<sistema>/` é o que faz as perguntas ficarem cada vez mais afiadas para
cada sistema — e cresce aos poucos, não precisa estar completo no dia 1.

## Um lembrete importante

Este agente refina a **tarefa** — requisito de customização ou chamado de
sustentação. Ele não decide, não aprova, e não substitui a revisão humana antes de
qualquer decisão ser tomada com base no documento gerado. Pendência registrada é o
agente fazendo bem o trabalho dele — não é ele "não terminando" o serviço.
