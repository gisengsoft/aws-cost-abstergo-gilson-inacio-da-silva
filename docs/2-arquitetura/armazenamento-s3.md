# Documentação das Classes de Armazenamento S3 e Políticas de Ciclo de Vida

## Abstergo Industries

**Data:** 01/08/2025
 **Responsável:** Gilson Inacio da Silva

## 1. Visão Geral

Este documento detalha as classes de armazenamento utilizadas nos buckets Amazon S3 da Abstergo Industries e as políticas de ciclo de vida implementadas para otimização de custos, conforme mencionado no relatório de implementação. A estratégia de armazenamento foi projetada para equilibrar disponibilidade, desempenho e custo, considerando a natureza dos dados farmacêuticos.

## 2. Inventário de Buckets S3

| Nome do Bucket | Propósito | Região Principal | Replicação |
| --- | --- | --- | --- |
| abstergo-docs-regulatorios | Documentação regulatória de produtos | us-east-1 | us-west-2, sa-east-1 |
| abstergo-imagens-produtos | Imagens de produtos farmacêuticos | us-east-1 | us-west-2, CloudFront |
| abstergo-bulas-tecnicas | Bulas e informações técnicas | us-east-1 | us-west-2, CloudFront |
| abstergo-relatorios-vendas | Relatórios de vendas e distribuição | us-east-1 | us-west-2 |
| abstergo-backups-sistemas | Backups de bancos de dados e sistemas | us-east-1 | us-west-2 |
| abstergo-logs-auditoria | Logs de auditoria para compliance | us-east-1 | us-west-2, sa-east-1 |

## 3. Classes de Armazenamento Utilizadas

### 3.1 Amazon S3 Standard

**Buckets e objetos aplicáveis:**

- Todos os novos uploads em todos os buckets (classe inicial)
- Permanentemente para objetos em `abstergo-imagens-produtos` acessados com frequência
- Permanentemente para os últimos 90 dias de bulas em `abstergo-bulas-tecnicas`

**Características principais:**

- Alta durabilidade (99,999999999%) e disponibilidade (99,99%)
- Baixa latência e alto throughput
- Projetado para dados acessados frequentemente
- Suporte a acesso multi-região via replicação
- Taxa de armazenamento mais alta em comparação às outras classes

**Caso de uso na Abstergo:** Utilizado para dados que exigem acesso imediato e frequente, como imagens de produtos ativos no catálogo e documentação regulatória em revisão ou recém-aprovada.

### 3.2 Amazon S3 Standard-IA (Infrequent Access)

**Buckets e objetos aplicáveis:**

- Objetos em `abstergo-docs-regulatorios` após 90 dias do último acesso
- Objetos em `abstergo-bulas-tecnicas` entre 90 e 365 dias
- Objetos em `abstergo-relatorios-vendas` entre 30 e 180 dias

**Características principais:**

- Mesma durabilidade que S3 Standard (99,999999999%)
- Disponibilidade ligeiramente menor (99,9%)
- Custos de armazenamento reduzidos em aproximadamente 40% em relação ao S3 Standard
- Taxa de recuperação por GB acessado
- Tempo mínimo de armazenamento de 30 dias

**Caso de uso na Abstergo:** Utilizado para documentos que ainda precisam estar prontamente disponíveis, mas são acessados com menor frequência, como documentação regulatória de produtos já aprovados ou relatórios de períodos anteriores.

### 3.3 Amazon S3 One Zone-IA

**Buckets e objetos aplicáveis:**

- Cópias secundárias de objetos em `abstergo-imagens-produtos`
- Objetos não críticos em `abstergo-backups-sistemas` que possuem redundância

**Características principais:**

- Armazenamento em uma única zona de disponibilidade (ao invés de três ou mais)
- Mesma durabilidade para falhas de objetos (99,999999999%)
- Menor proteção contra desastres físicos na zona
- Aproximadamente 20% mais econômico que Standard-IA

**Caso de uso na Abstergo:** Utilizado para dados que podem ser facilmente recriados ou que já possuem múltiplas cópias em outras classes de armazenamento, proporcionando economia adicional.

### 3.4 Amazon S3 Glacier Instant Retrieval

**Buckets e objetos aplicáveis:**

- Objetos em `abstergo-docs-regulatorios` entre 1 e 5 anos
- Objetos em `abstergo-relatorios-vendas` entre 180 dias e 2 anos
- Objetos em `abstergo-bulas-tecnicas` de produtos ainda comercializados com idade entre 1 e 7 anos

**Características principais:**

- Acesso em milissegundos aos dados
- Custos de armazenamento aproximadamente 68% menores que S3 Standard
- Taxas de recuperação mais altas
- Período mínimo de armazenamento de 90 dias

**Caso de uso na Abstergo:** Utilizado para arquivamento de dados que raramente precisam ser acessados, mas quando necessário, requerem recuperação rápida, como documentação regulatória histórica que pode ser solicitada em auditorias.

### 3.5 Amazon S3 Glacier Flexible Retrieval

**Buckets e objetos aplicáveis:**

- Objetos em `abstergo-backups-sistemas` após 90 dias
- Objetos em `abstergo-logs-auditoria` entre 1 e 7 anos
- Objetos em `abstergo-relatorios-vendas` após 2 anos

**Características principais:**

- Tempos de recuperação variáveis (minutos a horas)
- Três opções de recuperação: Expedited (1-5 minutos), Standard (3-5 horas), Bulk (5-12 horas)
- Custos aproximadamente 75% menores que S3 Standard
- Período mínimo de armazenamento de 90 dias

**Caso de uso na Abstergo:** Utilizado para arquivamento de longo prazo de dados que raramente precisam ser acessados e podem tolerar tempos de recuperação mais longos, como backups antigos e logs de auditoria para compliance.

### 3.6 Amazon S3 Glacier Deep Archive

**Buckets e objetos aplicáveis:**

- Objetos em `abstergo-docs-regulatorios` após 5 anos
- Objetos em `abstergo-logs-auditoria` após 7 anos
- Objetos em `abstergo-bulas-tecnicas` de produtos descontinuados

**Características principais:**

- Classe de armazenamento mais econômica (até 95% menor que S3 Standard)
- Tempo de recuperação de 12 a 48 horas
- Período mínimo de armazenamento de 180 dias
- Projetado para retenção de longo prazo e preservação digital

**Caso de uso na Abstergo:** Utilizado para arquivamento de dados que precisam ser mantidos por conformidade regulatória ou histórico corporativo, mas que são raramente acessados, como documentação de produtos descontinuados e registros históricos.

## 4. Políticas de Ciclo de Vida

### 4.1 Política para Documentação Regulatória

**Bucket:** `abstergo-docs-regulatorios`

| Regra | Descrição | Ação | Tempo |
| --- | --- | --- | --- |
| DocsReg-Rule1 | Transição para IA | Transição para Standard-IA | 90 dias após criação |
| DocsReg-Rule2 | Transição para Glacier IR | Transição para Glacier Instant Retrieval | 1 ano após criação |
| DocsReg-Rule3 | Transição para Deep Archive | Transição para Glacier Deep Archive | 5 anos após criação |
| DocsReg-Rule4 | Expiração de documentos obsoletos | Expiração | 10 anos após criação para produtos descontinuados |

**Lógica adicional:**

- Documentos marcados com tag `retention=indefinite` são excluídos de políticas de expiração
- Tags específicas para requisitos regulatórios de diferentes países controlam regras de retenção específicas

### 4.2 Política para Imagens de Produtos

**Bucket:** `abstergo-imagens-produtos`

| Regra | Descrição | Ação | Tempo |
| --- | --- | --- | --- |
| Imagens-Rule1 | Manter imagens de produtos ativos | Nenhuma transição | Permanece em S3 Standard |
| Imagens-Rule2 | Transição para produtos descontinuados | Transição para Standard-IA | 30 dias após marcação como descontinuado |
| Imagens-Rule3 | Arquivamento de produtos descontinuados | Transição para Glacier Flexible Retrieval | 1 ano após marcação como descontinuado |
| Imagens-Rule4 | Expiração de imagens obsoletas | Expiração | 3 anos após marcação como obsoleto |

**Lógica adicional:**

- Baseado em tags de produto (`status=active`, `status=discontinued`, `status=obsolete`)
- Imagens de alta resolução seguem regras de transição mais agressivas
- Miniaturas permanecem em Standard por mais tempo

### 4.3 Política para Bulas e Informações Técnicas

**Bucket:** `abstergo-bulas-tecnicas`

| Regra | Descrição | Ação | Tempo |
| --- | --- | --- | --- |
| Bulas-Rule1 | Transição para IA | Transição para Standard-IA | 90 dias após criação |
| Bulas-Rule2 | Transição para Glacier IR | Transição para Glacier Instant Retrieval | 1 ano após criação |
| Bulas-Rule3 | Transição para Glacier FR | Transição para Glacier Flexible Retrieval | 5 anos após criação |
| Bulas-Rule4 | Transição para Deep Archive | Transição para Glacier Deep Archive | 7 anos após criação para produtos ainda ativos |
| Bulas-Rule5 | Expiração de bulas obsoletas | Expiração | Nunca (mantidas indefinidamente por requisitos regulatórios) |

**Lógica adicional:**

- Versões anteriores de bulas seguem regras de transição mais agressivas
- Versões atuais permanecem mais tempo em classes de acesso rápido

### 4.4 Política para Relatórios de Vendas

**Bucket:** `abstergo-relatorios-vendas`

| Regra | Descrição | Ação | Tempo |
| --- | --- | --- | --- |
| Relatorios-Rule1 | Transição para IA | Transição para Standard-IA | 30 dias após criação |
| Relatorios-Rule2 | Transição para Glacier IR | Transição para Glacier Instant Retrieval | 180 dias após criação |
| Relatorios-Rule3 | Transição para Glacier FR | Transição para Glacier Flexible Retrieval | 2 anos após criação |
| Relatorios-Rule4 | Expiração de relatórios antigos | Expiração | 7 anos após criação (requisito fiscal) |

### 4.5 Política para Backups de Sistemas

**Bucket:** `abstergo-backups-sistemas`

| Regra | Descrição | Ação | Tempo |
| --- | --- | --- | --- |
| Backups-Rule1 | Transição para IA | Transição para Standard-IA | 30 dias após criação |
| Backups-Rule2 | Transição para One Zone-IA | Transição para One Zone-IA | 60 dias após criação |
| Backups-Rule3 | Transição para Glacier FR | Transição para Glacier Flexible Retrieval | 90 dias após criação |
| Backups-Rule4 | Expiração de backups | Expiração | 1 ano após criação (backups diários) |
| Backups-Rule5 | Retenção de backups mensais | Transição para Deep Archive | 1 ano após criação (apenas backups mensais) |
| Backups-Rule6 | Expiração de backups mensais | Expiração | 5 anos após criação (backups mensais) |

**Lógica adicional:**

- Regras diferenciadas baseadas em prefixo do objeto (`daily/`, `weekly/`, `monthly/`)
- Backups críticos marcados com tag `critical=true` têm políticas de retenção estendidas

### 4.6 Política para Logs de Auditoria

**Bucket:** `abstergo-logs-auditoria`

| Regra | Descrição | Ação | Tempo |
| --- | --- | --- | --- |
| Logs-Rule1 | Transição para IA | Transição para Standard-IA | 30 dias após criação |
| Logs-Rule2 | Transição para Glacier FR | Transição para Glacier Flexible Retrieval | 1 ano após criação |
| Logs-Rule3 | Transição para Deep Archive | Transição para Glacier Deep Archive | 7 anos após criação |
| Logs-Rule4 | Expiração de logs | Expiração | 10 anos após criação |

**Lógica adicional:**

- Logs relacionados a incidentes de segurança são marcados com tag `security-incident=true` e retidos indefinidamente
- Logs de acesso a dados sensíveis (`sensitive=true`) têm política de retenção estendida

## 5. Economia de Custos Alcançada

| Bucket | Custo Anterior (Mensal) | Custo Atual (Mensal) | Economia |
| --- | --- | --- | --- |
| abstergo-docs-regulatorios | R$ 8.500 | R$ 3.400 | 60% |
| abstergo-imagens-produtos | R$ 12.000 | R$ 5.500 | 54% |
| abstergo-bulas-tecnicas | R$ 7.200 | R$ 2.700 | 62% |
| abstergo-relatorios-vendas | R$ 5.800 | R$ 1.900 | 67% |
| abstergo-backups-sistemas | R$ 23.000 | R$ 8.000 | 65% |
| abstergo-logs-auditoria | R$ 7.500 | R$ 2.800 | 63% |
| **Total** | **R$ 64.000** | **R$ 24.300** | **62%** |

## 6. Monitoramento e Relatórios

### 6.1 Métricas Monitoradas

- Distribuição de armazenamento por classe
- Número de transições por política de ciclo de vida
- Custos de armazenamento por bucket e classe
- Frequência de acesso a objetos por classe de armazenamento
- Custos de recuperação de dados de classes Glacier

### 6.2 Relatórios Automatizados

- Relatório semanal de distribuição de armazenamento
- Relatório mensal de economia gerada
- Alerta para objetos que deveriam ter transitado mas não o fizeram
- Alerta para custos de recuperação acima do esperado

## 7. Considerações de Conformidade e Governança

- Objetos sob retenção legal são excluídos de políticas de transição e expiração
  
- Logs de auditoria para todas as transições e expirações são mantidos por 7 anos
  
- Versioning habilitado em todos os buckets para proteção contra exclusão acidental
  
- Políticas de ciclo de vida testadas e validadas pelo departamento jurídico e de compliance
  

---

© 2025 Gilson Inacio da Silva. Todos os direitos reservados.