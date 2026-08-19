# tarefas/

Documentos já refinados pelas skills de refinamento ficam aqui, organizados por
sistema — mesma lógica de `contextos/`:

```
tarefas/
├── <sistema-a>/
│   ├── customizacao-<slug-do-pedido>.md
│   └── sustentacao-<slug-do-relato>.md
└── <sistema-b>/
    └── ...
```

O nome da pasta é genérico (`tarefas`, não `requisitos`) porque nem tudo que passa
por este agente é um pedido de customização — parte é chamado de suporte/
sustentação, refinado pela skill `refinar-tarefa-sustentacao`. O prefixo no nome do
arquivo (`customizacao-` ou `sustentacao-`) é só uma convenção para diferenciar os
dois tipos dentro da mesma pasta; ajuste à vontade para o fluxo do seu time (ex.: um
documento por tarefa Jira, integração com uma ferramenta externa de backlog etc.).
