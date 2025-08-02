# Estrutura de Contas e Unidades Organizacionais

Este documento descreve a estrutura de contas da Abstergo Industries na AWS, com base no diagrama de **AWS Organizations**. A organização está dividida em três Unidades Organizacionais (OUs): **Governança**, **Workloads** e **Sandbox**. Cada OU possui contas separadas para funções específicas, o que permite isolamento de ambientes, aplicação de políticas de controle de serviço (SCPs) e melhor gestão de custos.

![Estrutura de contas AWS Organizations](../../assets/img/diagramas/aws-organizations.png)

## Descrição das Unidades Organizacionais (OUs)

- **OU Governança**: concentra contas de segurança (CloudTrail/GuardDuty), logs (CloudWatch e S3), rede (Transit Gateway, VPC Endpoints) e outras funções centrais. As SCPs desta OU impõem restrições de região e protegem recursos críticos.

- **OU Workloads**: inclui contas de produção, homologação, disaster recovery (DR) e desenvolvimento. É onde residem as workloads farmacêuticas; as SCPs definem permissões de serviço específicas para a indústria farmacêutica.

- **OU Sandbox**: destinada a provas de conceito (PoC) e experimentação. Possui SCPs que limitam custos e restringem recursos, reduzindo riscos e desperdícios.

Essas divisões facilitam a aplicação de boas práticas de governança e permitem delegar responsabilidade de custos por ambiente.
