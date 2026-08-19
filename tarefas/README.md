# tarefas/

Documentos já refinados pelas skills de refinamento ficam aqui, organizados por
sistema — mesma lógica de `contextos/`:

```
tarefas/
├── <sistema-a>/
│   ├── customizacao-<slug-do-pedido>/
│   │   ├── customizacao-<slug-do-pedido>.md
│   │   └── customizacao-<slug-do-pedido>.pdf
│   └── sustentacao-<slug-do-relato>/
│       └── sustentacao-<slug-do-relato>.md
└── <sistema-b>/
    └── ...
```

O nome da pasta é genérico (`tarefas`, não `requisitos`) porque nem tudo que passa
por este agente é um pedido de customização — parte é chamado de suporte/
sustentação, refinado pela skill `refinar-tarefa-sustentacao`. O prefixo no nome do
arquivo (`customizacao-` ou `sustentacao-`) é só uma convenção para diferenciar os
dois tipos dentro da mesma pasta; ajuste à vontade para o fluxo do seu time (ex.: um
documento por tarefa Jira, integração com uma ferramenta externa de backlog etc.).

Cada pedido/relato ganha sua própria subpasta dentro de `tarefas/<sistema>/`, nomeada
com o mesmo slug do documento (`<tipo>-<slug>/`). Todo artefato gerado para aquela
tarefa — o `.md`, o `.pdf` quando pedido, e qualquer outro arquivo futuro — fica
dentro dessa subpasta, nunca solto direto em `tarefas/<sistema>/`.
