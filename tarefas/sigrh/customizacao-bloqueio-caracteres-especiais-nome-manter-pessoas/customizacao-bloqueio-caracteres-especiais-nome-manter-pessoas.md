# Requisito: Bloqueio de Caracteres Proibidos no Campo Nome — Manter Pessoas (SIGRH)

## 1. Contexto
Sistema: SIGRH. Tela: Manter Pessoas, campo Nome.

Pedido original: *"A tela de Manter Pessoas não deve aceitar caracteres especiais no campo Nome."*

Motivação de negócio: ao montar o XML de envio ao eSocial, caractere especial presente no campo Nome quebra o processamento do arquivo — confirmado pela analista, em 19/08/2026.

Área/pessoa que originou o pedido e data do pedido original não foram informadas — registrado como pendência não bloqueante (seção 7).

## 2. Comportamento atual
Hoje o campo Nome, na tela Manter Pessoas, aceita qualquer caractere, sem nenhuma validação de formato — confirmado pela analista, em 19/08/2026.

## 3. Regras de negócio
1. O campo Nome deve rejeitar, ao salvar, os seguintes caracteres: números (0-9), apóstrofo ('), hífen (-), e os símbolos `! @ # $ % & * / \ < >` — confirmado pela analista, 19/08/2026.
2. Letras (maiúsculas e minúsculas), com ou sem acentuação (ex.: á, ã, ç, é, ê, ü), são permitidas — confirmado, 19/08/2026.
3. Espaço é permitido — confirmado, 19/08/2026.
4. A lista da regra 1 é uma **lista fechada de proibidos**, não uma lista de permitidos: qualquer caractere que não esteja nela é aceito por padrão, mesmo sem ter sido testado contra a geração do XML do eSocial (ex.: `% + _ = " ;`) — decisão explícita da analista, 19/08/2026. Ver risco associado na seção 6.
5. A validação ocorre no momento de salvar o registro; não bloqueia a digitação em tempo real — confirmado, 19/08/2026.
6. A regra vale exclusivamente para o campo Nome — confirmado, 19/08/2026.
7. A regra vale exclusivamente para a tela Manter Pessoas — confirmado, 19/08/2026.
8. A regra vale para todos os perfis de usuário que editam essa tela, sem exceção — confirmado, 19/08/2026.
9. Registros já existentes com caractere proibido no Nome não são alterados retroativamente — confirmado, 19/08/2026.
10. Não há texto de mensagem de erro definido pela analista — fica livre para quem desenvolver — confirmado, 19/08/2026.

## 4. Histórias de usuário

### História 1 — Bloquear caracteres proibidos no campo Nome ao salvar
Como usuário, quero que a tela Manter Pessoas rejeite números, apóstrofo, hífen e os símbolos `! @ # $ % & * / \ < >` no campo Nome ao tentar salvar, para que o XML de envio ao eSocial não quebre por causa desses caracteres no nome da pessoa.

Critérios de aceite:
- Dado que estou na tela Manter Pessoas, quando digito um Nome contendo apenas letras (com ou sem acento) e espaços e tento salvar, então o sistema salva normalmente.
- Dado que estou na tela Manter Pessoas, quando digito um Nome contendo algum número (0-9) e tento salvar, então o sistema rejeita o salvamento e exibe uma mensagem de erro.
- Dado que estou na tela Manter Pessoas, quando digito um Nome contendo apóstrofo (') e tento salvar, então o sistema rejeita o salvamento e exibe uma mensagem de erro.
- Dado que estou na tela Manter Pessoas, quando digito um Nome contendo hífen (-) e tento salvar, então o sistema rejeita o salvamento e exibe uma mensagem de erro.
- Dado que estou na tela Manter Pessoas, quando digito um Nome contendo qualquer um dos símbolos `! @ # $ % & * / \ < >` e tento salvar, então o sistema rejeita o salvamento e exibe uma mensagem de erro.
- Dado que estou na tela Manter Pessoas, quando digito um Nome contendo um símbolo fora da lista de proibidos (ex.: `% + _ =`) e tento salvar, então o sistema aceita o salvamento normalmente.
- Dado que estou na tela Manter Pessoas, quando ainda estou digitando o Nome, então o sistema não bloqueia a digitação em tempo real — a validação só ocorre ao salvar.
- Dado um registro de pessoa já existente hoje com caractere proibido no campo Nome, quando esse registro não é editado, então ele permanece inalterado no sistema.

## 5. Fora de escopo
- Outros campos de nome na tela (Nome Social, Apelido etc.), caso existam.
- Outras telas, importação em lote ou integrações que também gravam o campo Nome.
- Correção retroativa de registros já existentes com caractere proibido no Nome.
- Texto específico da mensagem de erro.
- Bloqueio de qualquer caractere que não conste explicitamente na lista de proibidos da regra 1 (ex.: `% + _ = " ;`) — esses continuam aceitos por decisão da analista.

## 6. Riscos e impactos identificados
- **Lista fechada de proibidos, não lista de permitidos**: símbolos não testados contra a geração do XML do eSocial (ex.: `% + _ = " ; : { } [ ]`) continuarão passando pela validação e podem reproduzir o mesmo problema de quebra no XML que motivou este requisito. Risco assumido conscientemente pela analista após alerta explícito, 19/08/2026.
- Nomes legítimos que usam hífen ou apóstrofo (sobrenomes compostos, nomes de origem estrangeira) serão rejeitados pela tela. Risco assumido conscientemente pela analista, 19/08/2026.
- Registros já existentes com caractere proibido no Nome continuarão gerando o mesmo problema no XML do eSocial até correção manual — fora do escopo deste requisito, assumido pela analista, 19/08/2026.
- Campo Nome contém dado pessoal, mas não está na lista de dados sensíveis que exigem tratamento especial de acesso/log (CPF, saúde, dado bancário, endereço, biometria) — não sinalizado como sensível para fins deste requisito.

## 7. Pendências
| Pergunta | Por quê importa | Responsável | Status |
|---|---|---|---|
| Quem originou o pedido (área/pessoa) e em que data? | Rastreabilidade do requisito | Analista | Aberta — não bloqueante |

## 8. Definição de pronto
- [ ] Validação implementada exatamente conforme a lista fechada de proibidos da regra 1 (não uma lista de permitidos).
- [ ] Mensagem de erro exibida ao usuário quando o salvamento é rejeitado (texto livre).
- [ ] Validação dispara só ao tentar salvar, sem bloquear digitação em tempo real.
- [ ] Registros existentes não são alterados retroativamente.
- [ ] Regra restrita ao campo Nome, tela Manter Pessoas — nenhuma outra tela/campo afetado.
- [ ] Riscos da seção 6 (símbolos não cobertos e nomes com hífen/apóstrofo rejeitados) comunicados e aceitos pela área solicitante antes do desenvolvimento.
