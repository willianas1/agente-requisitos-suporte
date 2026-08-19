# contextos/

Cada sistema que este agente vai apoiar tem sua própria pasta aqui, em kebab-case
(ex.: `sigrh-ap/`, `sicon/`). É isso que permite operar mais de um sistema com o
mesmo agente — inclusive em sessões simultâneas — sem misturar o vocabulário de um
sistema com o de outro.

```
contextos/
├── _template-contexto.md     → modelo usado pela skill carregar-contexto-sistema
├── <sistema-a>/
│   ├── CONTEXTO.md           → conhecimento curado, gerado pela skill
│   └── fontes/                → documentos brutos que você coloca (entrada)
└── <sistema-b>/
    ├── CONTEXTO.md
    └── fontes/
```

## Como adicionar um sistema novo

Não crie a pasta à mão. Peça ao agente para rodar a skill
`.agents/skills/carregar-contexto-sistema/SKILL.md`, informe o nome do sistema, e
depois coloque os documentos-fonte em `contextos/<sistema>/fontes/` — antes ou
depois de rodar a skill, as duas ordens funcionam.

## Como usar mais de um sistema ao mesmo tempo

As skills `refinar-requisito-customizacao` e `refinar-tarefa-sustentacao`
perguntam, logo no início de cada refinamento, qual sistema está em jogo, e só
carregam o `CONTEXTO.md` daquele sistema. Isso permite:

- Ter duas janelas/instâncias do seu harness de IA abertas nesta mesma pasta, uma
  tratando de um sistema e outra de outro, em paralelo, sem risco de mistura.
- Ou usar uma sessão só, trocando de sistema entre um pedido e outro — desde que
  confirme a troca quando o agente perguntar.
