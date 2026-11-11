# Prompts — Email Triage AI

Esta pasta contém os prompts utilizados pelo agente de Inteligência Artificial responsável pela classificação e análise de e-mails no projeto **Email Triage AI**.

O objetivo é manter as instruções do modelo organizadas, documentadas e versionadas separadamente do workflow de automação.

## Estrutura

```text
prompts/
├── email-classification.md
└── README.md
```

## Prompt de classificação

### `email-classification.md`

Contém o prompt principal utilizado na integração com a API DeepSeek.

Suas responsabilidades incluem:

- Definir o papel e as instruções do agente.
- Analisar o conteúdo dos e-mails recebidos.
- Identificar a categoria da mensagem.
- Classificar o nível de urgência.
- Identificar o sentimento predominante.
- Gerar um resumo objetivo.
- Elaborar uma sugestão de resposta.
- Retornar os resultados em formato JSON estruturado.

## Critérios de classificação

O agente trabalha com três níveis de urgência:

| Nível | Descrição |
|---|---|
| Alta | Problemas críticos que exigem atenção prioritária. |
| Média | Solicitações que precisam de acompanhamento, mas não exigem intervenção imediata. |
| Baixa | Mensagens informativas, elogios e situações sem necessidade de prioridade. |

A classificação também considera categorias relacionadas ao conteúdo da mensagem e o sentimento predominante.

## Integração com o workflow

O prompt é utilizado pelo node responsável pela comunicação com a API DeepSeek no n8n.

O resultado estruturado é encaminhado às etapas seguintes do workflow, responsáveis pelo processamento das informações e pela aplicação das condições de notificação.

Mensagens classificadas como urgência alta podem acionar uma notificação pelo Telegram, conforme as regras definidas no fluxo.

## Testes e avaliação

Os testes funcionais iniciais estão documentados na pasta `data/`:

- `test_emails.json`: mensagens utilizadas nos testes.
- `expected_results.json`: resultados esperados.
- `evaluation_results.json`: resultados observados durante a execução.

Os testes contemplam três cenários: cobrança duplicada, atraso na entrega de um pedido e elogio pelo atendimento.

## Versionamento

Alterações no prompt devem ser registradas no Git para manter o histórico de evolução das instruções do agente.

Recomenda-se testar novamente os cenários documentados sempre que houver alterações relevantes, verificando se as classificações e o comportamento do workflow continuam consistentes.

O objetivo é permitir a manutenção e evolução do agente sem comprometer seu comportamento esperado.