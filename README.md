# Projeto AWS – Abstergo Industries (Redução de Custos)

![Status do Projeto](https://img.shields.io/badge/status-em%20andamento-brightgreen)
![Licença](https://img.shields.io/badge/licença-MIT-blue)
![Autor](https://img.shields.io/badge/autor-Gilson%20Inacio%20da%20Silva-orange)

## Visão Geral

Este repositório documenta a concepção e implementação de uma plataforma em nuvem
para a **Abstergo Industries**, uma empresa farmacêutica fictícia que atua como
hub de distribuição. O objetivo do projeto é aplicar os conhecimentos da
certificação **AWS Cloud Practitioner (CLF‑C02)** para migrar a empresa para a
AWS, com foco em **redução de custos**, alta disponibilidade e conformidade
regulatória.  As entregas aqui servem como um portfólio prático para
profissionais de DevOps e Cloud que estejam iniciando na computação em nuvem.

## Objetivo

* Entender o contexto de uma farmacêutica sem ambiente em nuvem e propor uma
  arquitetura AWS que reduza custos imediatos.
* Selecionar **três serviços AWS** (Cost Explorer, Trusted Advisor e
  S3 Intelligent‑Tiering/Glacier Instant Retrieval) que gerem economia, e
  demonstrar seus benefícios ao gestor financeiro da empresa.
* Detalhar, por meio de documentos técnicos, diagramas e análises
  comparativas, as políticas de Auto Scaling, estratégias de banco de dados,
  armazenamento e conformidade adotadas.

## Estrutura do Repositório

```
aws-cost-abstergo-gilson-inacio-da-silva/
├── README.md # Este documento
├── docs/ # Documentação em Markdown
│ ├── 1-relatorio-implementacao/ # Relatório de implementação e redução de custos
│ │ └── relatorio-principal.md # Relatório principal (conteúdo textual)
│ ├── 2-arquitetura/ # Guias e manuais de arquitetura
│ │ ├── auto-scaling-multi-regiao.md # Manual de políticas de Auto Scaling e multi‑região
│ │ ├── banco-dados-rds.md # Arquitetura de banco de dados RDS e segurança
│ │ ├── armazenamento-s3.md # Estratégia de armazenamento S3 e classes de armazenamento
│ │ ├── organizacao-contas.md # Explicação da estrutura de contas e OUs (AWS Organizations)
│ │ └── fluxo-lambda-processos.md # Descrição dos fluxos de processos automatizados via Lambda
│ ├── 3-compliance/
│ │ └── relatorio-conformidade.md # Relatório de conformidade e requisitos regulatórios
│ └── 4-analise-financeira/
│ └── analise-comparativa-custos.md # Análise comparativa de custos pré e pós‑migração
├── assets/
│ ├── img/
│ │ └── diagramas/
│ │ ├── aws-organizations.png # Diagrama de estrutura de contas e OUs
│ │ └── fluxo-lambda.png # Diagrama de fluxos de processos automatizados
│ └── pdf/
│ └── reducao-de-custos-aws.pdf # Versão em PDF do relatório de redução de custos
└── LICENSE # Licença MIT
```

### Descrição das pastas

- **docs/1-relatorio-implementacao** – contém o relatório principal em Markdown.
  Nele são listadas as três ferramentas AWS utilizadas para a redução de custos,
  seus focos, formas de aplicação e benefícios específicos para a Abstergo
  Industries.  O PDF em `assets/pdf/` replica esse conteúdo para consulta
  offline.

- **docs/2-arquitetura** – reúne manuais e guias técnicos que detalham a
  implementação de políticas de Auto Scaling, a arquitetura de banco de dados
  Amazon RDS, a estratégia de armazenamento S3 (incluindo
  Intelligent‑Tiering/Glacier) e a estrutura de contas criada via AWS
  Organizations.  Também inclui a descrição dos fluxos de processos
  automatizados com Lambda e CloudWatch, ilustrados pelo diagrama
  `fluxo-lambda.png`.

- **docs/3-compliance** – guarda o relatório de conformidade, que mapeia
  requisitos regulatórios do setor farmacêutico e explica como os serviços
  implementados atendem a essas exigências.

- **docs/4-analise-financeira** – contém a análise comparativa de custos
  pré‑migração (infraestrutura on‑premises) e pós‑migração para a AWS,
  evidenciando a economia alcançada.

- **assets/img/diagramas** – armazena os diagramas utilizados.  O diagrama
  **aws‑organizations.png** demonstra como as contas foram organizadas em
  Unidades Organizacionais (Governança, Workloads e Sandbox) com diferentes
  políticas de controle de serviço.  O diagrama **fluxo‑lambda.png** mostra
  três fluxos de processos automatizados (processamento de pedidos,
  agendamento de reposição e atualização regulatória) que utilizam serviços
  como AWS Lambda, IAM, KMS, CloudWatch, S3 e DynamoDB.

- **assets/pdf** – guarda a versão em PDF do relatório de implementação,
  permitindo leitura consolidada fora do ambiente de desenvolvimento.

## Principais Entregas e Benefícios

1. **Relatório de redução de custos** – identifica três serviços fundamentais
   para otimizar despesas em nuvem: **AWS Cost Explorer**, **AWS Trusted
   Advisor** e **Amazon S3 Intelligent‑Tiering/Glacier Instant Retrieval**.  O
   relatório explica o foco de cada serviço, como aplicá‑los no contexto da
   Abstergo e os ganhos esperados, como visibilidade granular de gastos,
   recomendações automáticas de otimização e economia de até 68 % em
   armazenamento de dados frios.

2. **Documentos de arquitetura** – detalham a infraestrutura construída com
   **Auto Scaling**, **RDS**, **Lambda** e **S3**, incluindo parâmetros de
   escalonamento dinâmico, configurações de multi‑região, segurança de banco
   de dados, políticas de backup, estratégias de armazenamento e criptografia.

3. **Diagramas** – complementam os textos ao ilustrar a organização de contas
   e OUs na AWS e os fluxos de processos internos automatizados.  Eles
   facilitam a compreensão da arquitetura e demonstram boas práticas de
   governança e automação.

4. **Relatório de conformidade** – mapeia normas do setor farmacêutico e
   mostra como os controles de IAM, KMS, criptografia em repouso e em trânsito
   atendem aos requisitos de segurança e auditoria.

5. **Análise de custos** – compara os custos da infraestrutura legada com os
   custos na AWS após a adoção das boas práticas e serviços listados,
   quantificando a economia obtida.

## Como Navegar e Contribuir

Todos os documentos estão em formato Markdown para facilitar a leitura no
GitHub.  Utilize a barra lateral de navegação do repositório para abrir cada
arquivo.  O relatório em PDF pode ser baixado e lido localmente a partir de
`assets/pdf/reducao-de-custos-aws.pdf`.

Para reproduzir este projeto ou adaptá‑lo a outras empresas fictícias,
considere criar novas subpastas em `docs` seguindo a mesma convenção de
organização (implementação, arquitetura, compliance e análise financeira).

Contribuições são bem‑vindas!  Para sugerir melhorias ou corrigir erros,
abra uma *issue* ou faça um *fork* e envie um *pull request*.

## Tecnologias e Serviços Utilizados

* **AWS Cost Explorer** – análise de gastos e relatórios personalizados de
  uso e custos.
* **AWS Trusted Advisor** – recomendações de otimização de custos, segurança
  e desempenho baseadas em boas práticas.
* **Amazon S3 Intelligent‑Tiering & Glacier Instant Retrieval** – classes de
  armazenamento que migram dados automaticamente para camadas de custo mais
  baixo.
* **AWS Lambda & CloudWatch** – automação de processos por demanda com
  monitoramento e alarmes.
* **Amazon EC2 Auto Scaling** – ajuste automático de capacidade conforme a
  demanda.
* **Amazon RDS** – banco de dados gerenciado com multi‑AZ e replicação.
* **AWS IAM & KMS** – controle de acessos e criptografia de dados.

## Autor

**Gilson Inacio da Silva**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Perfil-blue)](https://www.linkedin.com/in/gilsoninsilva/)
[![GitHub](https://img.shields.io/badge/GitHub-Perfil-black)](https://github.com/gisengsoft)

## Licença

Este projeto está licenciado sob a Licença MIT.  Consulte o arquivo
[`LICENSE`](LICENSE) para mais detalhes.
