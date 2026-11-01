# Email Triage — Classificação Inteligente de E-mails

Automação de triagem de e-mails de suporte utilizando n8n, DeepSeek API e JavaScript. O fluxo classifica mensagens, valida a saída estruturada da IA, registra os resultados e encaminha alertas de alta urgência para análise humana.

## Objetivos

- Classificar e-mails por categoria e nível de urgência.
- Identificar o sentimento da mensagem.
- Gerar resumos e rascunhos de resposta para revisão humana.
- Validar os dados retornados pelo modelo de IA com JavaScript.
- Registrar os resultados no Google Sheets.
- Enviar alertas de alta urgência pelo Telegram.
- Prevenir o reprocessamento de e-mails já registrados por meio de uma Data Table do n8n.

## Tecnologias

- **n8n:** orquestração da automação.
- **DeepSeek API:** classificação e geração de conteúdo.
- **JavaScript:** validação da saída estruturada.
- **Gmail:** recebimento dos e-mails.
- **Google Sheets:** histórico das mensagens processadas.
- **Telegram:** notificação de casos urgentes.
- **Data Tables do n8n:** controle de IDs processados.
- **Docker Compose:** execução do ambiente.
- **Git e GitHub:** versionamento do projeto.

## Fluxo de processamento

1. O Gmail Trigger identifica novos e-mails.
2. O fluxo obtém o conteúdo da mensagem e organiza os campos necessários.
3. A Data Table verifica se o ID do e-mail já foi registrado.
4. A DeepSeek API classifica a mensagem e gera um resumo e um rascunho de resposta.
5. Um nó JavaScript valida os campos e os valores permitidos.
6. E-mails de alta urgência geram uma notificação no Telegram.
7. Os resultados são registrados no Google Sheets.
8. O ID processado é registrado na Data Table.

## Categorias de classificação

- Cobrança (`cobranca`)
- Suporte técnico (`suporte_tecnico`)
- Dúvida sobre produto (`duvida_produto`)
- Cancelamento (`cancelamento`)
- Entrega de pedido (`entrega_pedido`)
- Elogio (`elogio`)
- Outros (`outros`)

A urgência é classificada como `alta`, `media` ou `baixa`; o sentimento, como `positivo`, `neutro` ou `negativo`.

## Validação e confiabilidade

A saída da IA é interpretada como JSON e validada antes de seguir para os nós de notificação e registro. Valores inválidos geram erro no processamento, evitando que uma classificação fora dos valores permitidos seja aceita silenciosamente.

O fluxo também utiliza uma Data Table para verificar IDs previamente processados. Esse mecanismo reduz duplicidades, mas não garante processamento exatamente uma vez em todos os cenários de falha.

## Configuração

1. Instale Docker Desktop e execute o ambiente definido em `docker-compose.yml`.
2. Configure no n8n as credenciais necessárias para Gmail, DeepSeek, Google Sheets e Telegram.
3. Configure o acesso à planilha de histórico e à Data Table `processed_emails`.
4. Importe `workflows/email-triage.json` no n8n.
5. Revise as credenciais e os parâmetros específicos do seu ambiente antes de ativar o fluxo.

**Importante:** não publique tokens, chaves de API, dados pessoais de clientes ou identificadores privados nos arquivos do repositório.

## Status

**Protótipo funcional em testes.** Foram testados os caminhos de alta urgência e de urgência não alta, a notificação pelo Telegram, o registro no Google Sheets e o controle de IDs processados.

Ainda há oportunidades de melhoria em tratamento de falhas, prevenção de duplicidades em cenários de erro e avaliação sistemática da qualidade das classificações.

## Próximas melhorias

- Criar um conjunto de e-mails fictícios rotulados para avaliar a precisão da classificação.
- Medir precisão, recall e matriz de confusão por categoria e urgência.
- Aprimorar o tratamento de erros e a recuperação de execuções.
- Documentar os prompts e os critérios de validação.
- Avaliar custos, privacidade e desempenho do modelo.
