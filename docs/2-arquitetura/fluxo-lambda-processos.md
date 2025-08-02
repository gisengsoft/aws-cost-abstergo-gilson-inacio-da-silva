# Fluxos de Processos Automatizados com AWS Lambda

O diagrama abaixo ilustra três fluxos de automação implementados com **AWS Lambda**, **AWS CloudWatch**, **AWS KMS** e outros serviços. Esses fluxos foram desenvolvidos para atender diferentes processos internos da Abstergo Industries, como processamento de pedidos, agendamento de compras e atualização regulatória.

![Fluxo de funções Lambda e processos](/assets/img/diagramas/fluxo-lambda.png)

1. **Processamento de novos pedidos**: após a autenticação com IAM e decriptografia de dados via KMS, o pedido é validado. Se houver estoque e o pagamento for aprovado, os itens são reservados, a nota fiscal é gerada e o cliente é notificado. Casos de falha geram backorder e notificações específicas. CloudWatch monitora tempos de processamento e gera alarmes de atraso.
  
2. **Agendamento de reposição de estoque via CloudWatch**: a partir de limites de estoque configurados, o fluxo calcula a necessidade de reposição, verifica históricos de vendas e sazonalidade, e gera ordens de compra para aprovação automática ou manual. Tudo é registrado em logs de auditoria.
  
3. **Atualização regulatória**: garante a atualização de bulas e documentos. Após validação da fonte de dados, metadados são extraídos e versionados. Nova versão é atualizada no banco de dados, arquivada no S3 (classe S3‑IA ou Glacier, conforme política) e notificada à equipe. CloudWatch acompanha alterações e alarmes de falhas.

Esses fluxos aproveitam a característica **pay‑per‑use** do AWS Lambda, reduzindo custos de processamento e eliminando servidores dedicados, conforme mencionado no relatório de otimização de custos.
