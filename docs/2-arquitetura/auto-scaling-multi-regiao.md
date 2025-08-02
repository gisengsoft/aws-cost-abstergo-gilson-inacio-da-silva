# Manual de Políticas de Auto Scaling e Arquitetura Multi-Região

## Abstergo Industries

**Data:** 01/08/2025
**Responsável:** Gilson Inacio da Silva

## 1. Introdução

Este manual descreve as políticas de Auto Scaling implementadas na infraestrutura AWS da Abstergo Industries, bem como a arquitetura multi-região adotada para garantir alta disponibilidade e resiliência dos serviços de distribuição farmacêutica.

## 2. Políticas de Auto Scaling

### 2.1 Grupos de Auto Scaling

A Abstergo Industries implementou os seguintes grupos de Auto Scaling:

| Grupo | Aplicação | Região Principal | Regiões Secundárias |
| --- | --- | --- | --- |
| ASG-Pedidos | Sistema de Processamento de Pedidos | us-east-1 | us-west-2, sa-east-1 |
| ASG-Inventario | Sistema de Gerenciamento de Inventário | us-east-1 | us-west-2, sa-east-1 |
| ASG-Frontend | Portal de Parceiros | us-east-1 | us-west-2, sa-east-1 |

### 2.2 Políticas de Escalonamento

#### 2.2.1 Política de Escalonamento Dinâmico

Para atender aos picos de demanda identificados no relatório, foram configuradas as seguintes políticas:

- **Escalonamento Baseado em Utilização de CPU**:
  - Aumento de capacidade quando a utilização de CPU atinge 70% por 3 minutos consecutivos
  - Redução de capacidade quando a utilização de CPU permanece abaixo de 30% por 15 minutos
- **Escalonamento Baseado em Número de Requisições**:
  - Aumento de capacidade quando o número de requisições por instância ultrapassa 1000 por minuto
  - Redução de capacidade quando o número de requisições por instância fica abaixo de 200 por minuto por 20 minutos
- **Escalonamento Programado**:
  - Aumento automático de capacidade nos dias 1 a 5 de cada mês (período identificado de maior demanda)
  - Redução durante finais de semana e feriados nacionais

#### 2.2.2 Configurações de Capacidade

| Grupo | Capacidade Mínima | Capacidade Desejada | Capacidade Máxima |
| --- | --- | --- | --- |
| ASG-Pedidos | 2   | 4   | 12  |
| ASG-Inventario | 2   | 3   | 8   |
| ASG-Frontend | 3   | 5   | 15  |

### 2.3 Launch Templates

Cada grupo de Auto Scaling utiliza um Launch Template com as seguintes configurações:

- **Template-Pedidos**:
  - Tipo de instância: c5.large (principal), t3.large (fallback)
  - AMI: ami-abstergo-pedidos-v2.3.1
  - Volume raiz: 50GB gp3
  - Grupos de segurança: sg-pedidos-prod
  - Perfil IAM: role-ec2-pedidos-prod
- **Template-Inventario**:
  - Tipo de instância: r5.large (principal), r5.xlarge (períodos de pico)
  - AMI: ami-abstergo-inv-v1.8.5
  - Volume raiz: 100GB gp3
  - Volume adicional: 500GB st1 para cache de operações
  - Grupos de segurança: sg-inventario-prod
  - Perfil IAM: role-ec2-inventario-prod
- **Template-Frontend**:
  - Tipo de instância: t3.large (principal), c5.large (períodos de pico)
  - AMI: ami-abstergo-frontend-v3.2.1
  - Volume raiz: 30GB gp3
  - Grupos de segurança: sg-frontend-prod
  - Perfil IAM: role-ec2-frontend-prod

## 3. Arquitetura Multi-Região

### 3.1 Distribuição Regional

A Abstergo Industries adotou uma estratégia multi-região com a seguinte configuração:

- **Região Principal**: US East (N. Virginia) - us-east-1
  - Hospeda a infraestrutura primária e processa a maioria das cargas de trabalho em condições normais
- **Região Secundária (Ativa)**: US West (Oregon) - us-west-2
  - Mantém réplicas ativas de todos os serviços críticos
  - Recebe parte do tráfego do Route 53 para distribuição de carga
  - Capacidade para assumir 70% da carga total em caso de falha da região principal
- **Região Secundária (Disaster Recovery)**: South America (São Paulo) - sa-east-1
  - Mantém infraestrutura mínima com Auto Scaling configurado para rápida expansão
  - Capacidade para assumir 100% da carga em caso de falha das outras regiões
  - Proximidade geográfica com a maior parte dos clientes farmacêuticos

### 3.2 Estratégia de Replicação e Failover

- **Replicação de Dados**:
  - Banco de dados: Aurora Global Database com clusters em todas as regiões
  - Objetos S3: Replicação Cross-Region habilitada para buckets críticos
  - DynamoDB: Tabelas globais com replicação automática entre regiões
- **Estratégia de Failover**:
  - Monitoramento contínuo via CloudWatch e Route 53 Health Checks
  - Failover automático via Route 53 para tráfego de API e portal de parceiros
  - RTO (Recovery Time Objective): 15 minutos
  - RPO (Recovery Point Objective): 5 minutos

### 3.3 Configurações de Route 53

- **Política de Roteamento**:
  - Roteamento baseado em latência para tráfego normal
  - Verificações de integridade a cada 30 segundos
  - Failover automático configurado para direcionar tráfego em caso de falha regional

## 4. Monitoramento e Alertas

### 4.1 Métricas de CloudWatch

- Utilização de CPU e memória de instâncias EC2
- Número de instâncias por estado (pending, running, terminating)
- Número de solicitações por instância
- Tempo de resposta das aplicações
- Taxa de erros HTTP

### 4.2 Alertas Configurados

- Alertas Críticos
  - Redução não planejada de capacidade abaixo do mínimo
  - Falha na expansão durante períodos de pico
  - Falha de verificação de integridade em mais de 50% das instâncias
- Alertas de Atenção
  - Utilização contínua próxima à capacidade máxima por mais de 30 minutos
  - Oscilações rápidas de escalonamento (indicando configuração subótima)
  - Custos diários excedendo 20% da média dos últimos 7 dias

## 5. Procedimentos Operacionais

### 5.1 Atualização de Launch Templates

1. Criar uma nova versão do Launch Template
2. Realizar testes na nova versão em ambiente de homologação
3. Atualizar gradualmente o grupo de Auto Scaling (25%, 50%, 100%)
4. Monitorar métricas de performance e erro após cada etapa
5. Procedimento de rollback caso necessário

### 5.2 Modificação de Políticas de Escalonamento

1. Analisar dados históricos de utilização no CloudWatch
2. Implementar mudanças em ambiente de homologação
3. Após validação, aplicar em produção durante janela de baixa utilização
4. Monitorar comportamento por 7 dias após a mudança

## 6. Considerações de Custo e Eficiência

- Uso de instâncias spot para cargas não críticas durante expansão
  
- Reserva de instâncias para capacidade basal (mínima) em todas as regiões
  
- Implementação de rightsizing periódico baseado em análise de métricas
  
- Orçamento mensal definido com alertas de aproximação de limites
  

---

© 2025 Gilson Inacio da Silva. Todos os direitos reservados.