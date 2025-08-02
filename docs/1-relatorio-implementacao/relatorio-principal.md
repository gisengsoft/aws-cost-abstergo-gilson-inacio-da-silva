# Redução de custos em farmácias com AWS 

**Contexto**

A Abstergo Industries é uma farmacêutica fictícia que atua como hub de distribuição e ainda não utiliza serviços em nuvem. O gestor financeiro da empresa procura soluções para reduzir custos operacionais e quer entender como a AWS pode ajudar.  Com base nos conhecimentos da certificação **AWS Cloud Practitioner (CLF‑C02)** e no relatório de implementação fornecido, foi realizada uma pesquisa de fontes recentes sobre serviços da AWS que proporcionam **economia de custos**. A seguir são apresentados três serviços/ ferramentas que podem gerar economia imediata para a Abstergo Industries e como cada um deles pode ser aplicado na realidade de uma farmácia distribuidora.

## 1. AWS Cost Explorer – visibilidade e gestão de custos

**Foco da ferramenta:** o AWS Cost Explorer é um serviço de análise de custos que permite **visualizar, entender e gerenciar** as despesas na AWS ao longo do tempo. Ele cria relatórios personalizados, mostra a distribuição de gastos por serviço/conta e projeta custos futuros. As visualizações incluem gráficos e tabelas interativos que facilitam a identificação de tendências de gastos e anomalias.  

**Como reduzir custos com a ferramenta:**

- **Identificação de recursos subutilizados:** ao agregar dados do Cost and Usage Report, o Cost Explorer sinaliza instâncias que estão ociosas ou superdimensionadas.  O gestor pode encerrar ou reduzir esses recursos para evitar desperdício.  
- **Relatórios e previsões:** os relatórios personalizados e o recurso de previsão ajudam a planejar orçamentos.  É possível simular diferentes cenários de uso e tomar decisões informadas antes de contratar novos serviços.  
- **Recomendações de otimização:** o serviço oferece insights e recomendações acionáveis para reduzir custos, como sugerir o redimensionamento de instâncias ou a adoção de instâncias reservadas.  
- **Gestão de orçamentos:** permite configurar alertas de orçamento para cada departamento ou projeto, evitando surpresas e garantindo que os gastos fiquem dentro do planejado.

**Ganhos para a Abstergo Industries:**

- **Controle financeiro:** maior transparência nas despesas da empresa, facilitando a alocação de custos por centro de distribuição ou filial.  
- **Previsibilidade:** previsão de gastos futuros possibilita negociar melhor com fornecedores e planejar investimentos.  
- **Fomento à cultura de FinOps:** incentiva as equipes técnicas e financeiras a trabalharem juntas para otimizar recursos, reforçando a governança de custos.

## 2. AWS Trusted Advisor – recomendações automáticas e boas práticas

**Foco da ferramenta:** o AWS Trusted Advisor atua como um “consultor virtual” que avalia a infraestrutura da AWS e oferece recomendações baseadas no **AWS Well‑Architected Framework**. Ele abrange cinco pilares: otimização de custos, desempenho, segurança, tolerância a falhas e limites de serviço. A ferramenta analisa configurações e padrões de uso, comparando-os às boas práticas para sugerir melhorias.  

**Como reduzir custos com a ferramenta:**

- **Detecção de recursos ociosos ou subutilizados:** o Trusted Advisor identifica instâncias de computação subutilizadas, volumes EBS com baixo uso e balanceadores de carga inativos, recomendando o desligamento ou a redimensionamento desses recursos.  
- **Sugestão de planos de compromisso:** orienta sobre a compra de **Reserved Instances ou Savings Plans** para workloads previsíveis, possibilitando descontos significativos em relação ao preço sob demanda.  
- **Priorização de ações:** fornece uma lista de recomendações priorizadas, de modo que as equipes saibam quais intervenções geram maior economia.  
- **Alertas de limites e segurança:** além de custo, recomenda práticas para evitar gastos causados por falhas de segurança ou indisponibilidade (como configurar multi‑AZ ou ajustar limites de serviço).

**Ganhos para a Abstergo Industries:**

- **Economia automática:** reduz custos ao eliminar recursos subutilizados e orientar a aquisição de planos com desconto.  
- **Ambiente otimizado:** melhora o desempenho e a tolerância a falhas, reduzindo o risco de interrupções que poderiam gerar prejuízos.  
- **Conformidade e segurança:** reforça a segurança de dados sensíveis (por exemplo, registros de pacientes) e auxilia a empresa a cumprir requisitos regulatórios da área farmacêutica.

## 3. Amazon S3 Intelligent‑Tiering e S3 Glacier Instant Retrieval – armazenamento econômico

**Foco da ferramenta:** o Amazon S3 Intelligent‑Tiering é um serviço de armazenamento de objetos que move automaticamente os dados entre camadas (frequente, infrequente e de arquivamento) com base no padrão de acesso. A camada **S3 Glacier Instant Retrieval**, lançada em 2021, fornece armazenamento de **custo muito baixo** (cerca de US$ 0,004 por GB‑mês) com **acesso em milissegundos** para dados raramente acessados. A camada de **Archive Instant Access** dentro do Intelligent‑Tiering pode **economizar até 68 %** em custos de armazenamento.

**Como reduzir custos com a ferramenta:**

- **Automação de classes de armazenamento:** o S3 Intelligent‑Tiering analisa o acesso aos objetos e os migra automaticamente para a camada de menor custo sem penalizar a latência.  Isso elimina a necessidade de prever padrões de acesso e reduz custos para dados pouco acessados.  
- **Armazenamento de arquivos de longo prazo:** registros médicos, notas fiscais, relatórios de pesquisa clínica ou históricos de vendas podem ser mantidos em S3 Glacier Instant Retrieval, que oferece durabilidade de 99,999999999 % e custo por GB muito baixo com disponibilidade de 99,9 %.  
- **Economia automática:** ao configurar a camada Archive Instant Access, a Abstergo Industries pode reduzir até 68 % dos custos de armazenamento de dados que não precisam ser acessados frequentemente.  
- **Escalabilidade:** o S3 oferece escalabilidade praticamente ilimitada; a empresa paga apenas pelo que usa e pode crescer sem investir em infraestrutura física.

**Ganhos para a Abstergo Industries:**

- **Redução significativa de custos de armazenamento:** fundamental para um hub de distribuição que lida com grandes volumes de dados de inventário, pesquisas farmacêuticas e registros de clientes.  
- **Simplificação de operações de TI:** elimina a necessidade de gerenciar hardware local e políticas de arquivamento manuais; a equipe pode focar em atividades estratégicas.  
- **Segurança e conformidade:** o S3 oferece criptografia nativa e durabilidade elevada, garantindo integridade dos dados sensíveis.

## Conclusão

Os serviços selecionados — **AWS Cost Explorer**, **AWS Trusted Advisor** e **Amazon S3 Intelligent‑Tiering/Glacier Instant Retrieval** — alinham‑se ao conteúdo da certificação **AWS Cloud Practitioner (CLF‑C02)** e cumprem a finalidade de redução de custos para a Abstergo Industries.  

O **Cost Explorer** fornece visibilidade detalhada dos gastos e permite planejar orçamentos e identificar recursos desperdiçados.  O **Trusted Advisor** complementa essa visão com recomendações automáticas baseadas em boas práticas, indicando onde reduzir custos, melhorar desempenho e reforçar a segurança.  Por fim, o **S3 Intelligent‑Tiering** e o **Glacier Instant Retrieval** promovem uma gestão eficiente de armazenamento, oferecendo durabilidade e acesso rápido com valores até 68 % mais baratos que classes padrão.  

Ao adotar essas ferramentas, a Abstergo Industries poderá modernizar suas operações, implementar práticas de **FinOps** e obter uma economia significativa sem abrir mão de performance ou segurança, demonstrando a vantagem real da computação em nuvem para o setor farmacêutico.
