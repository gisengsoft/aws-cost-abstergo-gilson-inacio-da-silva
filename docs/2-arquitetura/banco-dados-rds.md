# DOCUMENTAÇÃO DA ARQUITETURA DE BANCO DE DADOS RDS E CONTROLES DE SEGURANÇA

Data: 01/08/2025
 Empresa: Abstergo Industries
 Responsável: GILSON INACIO DA SILVA

## 1. INTRODUÇÃO

Este documento apresenta a arquitetura de banco de dados implementada utilizando o Amazon RDS (Relational Database Service) e os controles de segurança adotados na empresa Abstergo Industries farmacêutica. Esta implementação complementa as outras soluções AWS já implementadas (EC2 Auto Scaling, S3 e Lambda), fornecendo uma infraestrutura de banco de dados gerenciada, resiliente e segura para dar suporte às operações críticas da empresa.

## 2. ARQUITETURA DO AMAZON RDS

### 2.1 Visão Geral da Solução

A Abstergo Industries implementou o Amazon RDS para PostgreSQL como sistema de gerenciamento de banco de dados principal, substituindo servidores de banco de dados locais que apresentavam limitações de escalabilidade e altos custos de manutenção. A nova arquitetura foi projetada para atender aos requisitos de alta disponibilidade, performance e segurança exigidos por uma empresa farmacêutica.

### 2.2 Componentes da Arquitetura

#### 2.2.1 Instâncias de Banco de Dados

- **Instância Principal**: db.m5.2xlarge (8 vCPU, 32 GB RAM)
  - Ambiente: Produção
  - Multi-AZ: Habilitado para alta disponibilidade
  - Storage: 500 GB gp3 com IOPS provisionado de 3000
- **Instância de Réplica de Leitura**: db.m5.xlarge (4 vCPU, 16 GB RAM)
  - Ambiente: Produção (leitura)
  - Região: Mesma região da instância principal
  - Storage: 500 GB gp3
- **Instância de Desenvolvimento/Teste**: db.m5.large (2 vCPU, 8 GB RAM)
  - Ambiente: Desenvolvimento
  - Multi-AZ: Desabilitado
  - Storage: 200 GB gp3

#### 2.2.2 Configuração de Rede

- **VPC Dedicada**: Isolamento de rede para os recursos de banco de dados
- **Subnet Groups**: Distribuição em múltiplas zonas de disponibilidade
- **Security Groups**: Controle granular de tráfego de entrada e saída
- **Network ACLs**: Regras adicionais de segurança no nível de subnet

#### 2.2.3 Recursos de Backup e Recuperação

- **Backups Automáticos**: Realizados diariamente com retenção de 14 dias
- **Snapshots Manuais**: Executados antes de atualizações críticas ou mudanças significativas
- **Point-in-Time Recovery**: Habilitado com capacidade de restauração para qualquer momento dentro do período de retenção
- **Réplica Multi-Região**: Configurada para disaster recovery em região secundária

## 3. CONTROLES DE SEGURANÇA

### 3.1 Segurança de Acesso e Autenticação

#### 3.1.1 IAM Database Authentication

- Autenticação baseada em IAM para controle centralizado de acessos ao banco de dados
- Integração com políticas de IAM existentes na AWS para gerenciamento de privilégios
- Rotação automática de credenciais usando AWS Secrets Manager

#### 3.1.2 Controles de Acesso ao Banco de Dados

- Implementação de RBAC (Role-Based Access Control) no PostgreSQL
- Criação de roles específicas para diferentes funções (leitura, escrita, administração)
- Auditoria regular de privilégios usando consultas SQL automatizadas

### 3.2 Criptografia e Proteção de Dados

#### 3.2.1 Criptografia em Repouso

- Todos os volumes de armazenamento criptografados com AWS KMS
- Chaves KMS gerenciadas com rotação anual automatizada
- Snapshots e backups também criptografados com as mesmas chaves

#### 3.2.2 Criptografia em Trânsito

- Conexões SSL/TLS forçadas para todas as comunicações com o banco de dados
- Certificados renovados automaticamente antes da expiração
- Monitoramento de tentativas de conexão não criptografadas

#### 3.2.3 Mascaramento de Dados Sensíveis

- Implementação de views e funções para mascaramento de dados sensíveis (PII)
- Controles de acesso para dados críticos conforme regulamentações farmacêuticas
- Logs de acesso a dados sensíveis para fins de compliance

### 3.3 Monitoramento e Detecção

#### 3.3.1 Integração com AWS CloudWatch

- Métricas de performance coletadas em intervalos de 1 minuto
- Alarmes configurados para notificação imediata em caso de anomalias
- Dashboards personalizados para visualização de métricas críticas

#### 3.3.2 Auditoria de Atividades

- Enhanced Monitoring habilitado para visibilidade do sistema operacional
- Database Activity Streams configurado para auditoria em tempo real
- Integração com AWS CloudTrail para registro de operações administrativas

#### 3.3.3 Análise de Vulnerabilidades

- Verificações automáticas semanais de vulnerabilidades no banco de dados
- Aplicação automática de atualizações de segurança do sistema
- Avaliação mensal de configurações contra melhores práticas de segurança

## 4. COMPLIANCE E REGULAMENTAÇÕES

### 4.1 Conformidade Regulatória

- Implementação de controles específicos para atender a LGPD (Lei Geral de Proteção de Dados)
- Documentação detalhada de todos os controles para auditorias regulatórias farmacêuticas
- Procedimentos para retenção e exclusão de dados conforme requisitos legais

### 4.2 Procedimentos de Validação

- Processos de validação de sistemas computadorizados conforme GxP
- Documentação IQ/OQ/PQ para o ambiente de banco de dados
- Rastreabilidade completa de alterações para fins de auditoria

## 5. OTIMIZAÇÃO DE CUSTOS

### 5.1 Estratégias Implementadas

- Utilização de instâncias Reserved Instances com compromisso de 3 anos
- Implementação de Storage Autoscaling para crescimento automático
- Agendamento de ambientes não-produtivos para desligamento em períodos ociosos

### 5.2 Economia Alcançada

- Redução de aproximadamente 40% nos custos de banco de dados em comparação com a infraestrutura on-premises anterior
- Diminuição de 30% nas horas de administração de DBA graças aos recursos gerenciados
- Economia adicional de 15% com a estratégia de réplicas de leitura para balanceamento de carga

## 6. PLANOS DE CONTINUIDADE E RECUPERAÇÃO

### 6.1 Estratégia de Disaster Recovery

- RTO (Recovery Time Objective): 1 hora
- RPO (Recovery Point Objective): 5 minutos
- Processo automatizado de failover para região secundária

### 6.2 Testes de Recuperação

- Simulações trimestrais de falha e recuperação
- Documentação detalhada dos procedimentos de recuperação
- Equipe treinada para execução de procedimentos de emergência

## 7. PRÓXIMOS PASSOS E MELHORIAS FUTURAS

### 7.1 Roadmap Tecnológico

- Avaliação de migração para Aurora PostgreSQL para benefícios adicionais de performance
- Implementação de particionamento de dados para tabelas de grande volume
- Expansão de réplicas de leitura para otimização adicional de performance

### 7.2 Melhorias de Segurança Planejadas

- Implementação de Database Firewall adicional com AWS Network Firewall
- Expansão do monitoramento de segurança com AWS GuardDuty
- Implementação de detecção de ameaças avançada com machine learning

---

*Este documento é confidencial e de uso exclusivo da Abstergo Industries. A implementação descrita foi realizada por GILSON INACIO DA SILVA como parte do projeto de modernização da infraestrutura de TI da empresa.*

---

© 2025 Gilson Inacio da Silva. Todos os direitos reservados.