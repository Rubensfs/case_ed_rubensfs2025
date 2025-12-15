# case_ed_rubensfs2025
Case para processo de certificação DataMasters_Engenharia de Dados do Santander - 2025
---
🌱 Case de Engenharia de Dados
Monitoramento Ambiental da Amazônia com Dados Públicos do INPE

📌 Visão Geral

Este projeto apresenta uma solução completa de engenharia de dados, desenvolvida para ingestão, processamento, armazenamento e análise de dados ambientais públicos relacionados ao monitoramento da Floresta Amazônica.

A solução foi construída utilizando arquitetura Lakehouse, pipelines batch, eventuais e near real-time, processamento distribuído com PySpark, armazenamento em Delta Lake e execução em cloud Microsoft Azure (Databricks).

O projeto demonstra a aplicação prática de boas práticas de engenharia de dados, incluindo:

Arquitetura escalável e resiliente

Padronização e governança de dados

Observabilidade de pipelines

Segurança e conformidade com LGPD

Reprodutibilidade da solução


I. 🎯 Objetivo do Case

Projetar e implementar uma arquitetura de dados capaz de:

Consumir datasets públicos confiáveis

Processar grandes volumes de dados ambientais

Disponibilizar dados consolidados para análises

Permitir evolução para cenários de processamento em tempo quase real

Tema Escolhido

Meio Ambiente — Monitoramento da Floresta Amazônica

A escolha do tema se justifica pela relevância ambiental, social e econômica, além da ampla disponibilidade de dados governamentais abertos, mantidos pelo Instituto Nacional de Pesquisas Espaciais (INPE).


II. 🌍 Fontes de Dados

Os dados utilizados no projeto são obtidos a partir do portal oficial TerraBrasilis / INPE:

🔗 https://terrabrasilis.dpi.inpe.br/


📊 Conjuntos de Dados Utilizados
1. Desmatamento — PRODES / INPE

Formatos: XML, Shapefile

Descrição:
Dados e mapas de desmatamento para todo o Brasil e por biomas, com calendários de publicação independentes.

2. Focos de Queimadas

Formato: CSV

Periodicidade:

Batch diário e mensal

Near real-time (atualizações a cada 10 minutos)

Descrição:
Informações sobre focos ativos de queimadas e incêndios florestais.

3. Risco de Fogo e Meteorologia

Formatos: NetCDF (.nc), TIF

Descrição:
Dados observados diariamente e previsões meteorológicas de curto prazo, utilizados para cálculo do risco de fogo.


III. 🏗️ Arquitetura da Solução
Visão Geral da Arquitetura

A solução foi projetada seguindo o padrão Lakehouse, combinando:

Data Lake para armazenamento de dados brutos e históricos

Camadas analíticas para consumo confiável e estruturado

Tecnologias Utilizadas
Camada	Tecnologia
Cloud	Microsoft Azure
Processamento	Databricks + PySpark
Armazenamento	Delta Lake
Orquestração	Databricks Jobs (YAML)
Versionamento	GitHub
Monitoramento	Databricks Jobs UI
IV. 📐 Diagramas de Arquitetura (C4 Model)

A documentação visual da solução segue o C4 Model, facilitando o entendimento da arquitetura em diferentes níveis de detalhe.

🔹 C4 – Nível 1: Contexto

Apresenta a interação entre:

Fontes externas (INPE / TerraBrasilis)

Plataforma de Engenharia de Dados

Usuários e ferramentas analíticas

📁 Arquivo:
diagrams/c4_context_amazonia.drawio

🔹 C4 – Nível 2: Containers

Demonstra os principais componentes da solução:

Azure Blob Storage (camada RAW)

Databricks (processamento distribuído)

Delta Lake (Bronze, Silver e Gold)

Consumo analítico

📁 Arquivo:
diagrams/c4_container_architecture.drawio

🔹 C4 – Nível 3: Componentes

Detalha os pipelines e suas etapas internas:

Extração de dados

Ingestão Bronze

Processamento Silver

Agregações Gold

📁 Arquivo:
diagrams/c4_pipeline_components.drawio


V. 🧩 Arquitetura de Dados (Medalhão)

O projeto adota o padrão Medallion Architecture, organizando os dados em camadas bem definidas:

Camada	Descrição
RAW	Dados brutos, sem qualquer transformação
BRONZE	Padronização inicial e versionamento
SILVER	Dados tratados, tipados e confiáveis
GOLD	Dados agregados e prontos para análise

VI. 🔄 Pipelines Implementados

Esta seção descreve os pipelines de ingestão e processamento desenvolvidos no projeto, contemplando execuções batch, eventuais e near real-time, todos organizados segundo a arquitetura medalhão.

🔥 Pipeline — Focos de Queimadas (Batch Diário)

Pipeline: Pipeline_INPE_FocosQueimadas_Diaria
Formato dos dados: CSV
Periodicidade: Diária

🔹 Extração (RAW)

Job: capture_raw_focos_diario_d

Descrição:
Download diário do arquivo focos_diario_br_aaaammdd.csv a partir da API pública do INPE.

Armazenamento RAW:
/Volumes/datamasters/raw/raw_inpe

🔹 Ingestão Bronze

Job: ingesta_d_foco_queim

Tabela Bronze:
datamasters.b_inep.focos_queimadas_diario

🔹 Processamento Silver

Job: process_d_foco_queim_silver

Tabela Silver:
datamasters.s_inep.d_foco_queim_format

🔹 Agregação Gold

Job: d_foco_queim_gold_agregado

Tabela Gold:
datamasters.g_inep.d_focos_queimadas_agg

🌡️ Pipeline — Risco de Fogo (Batch Diário)

Pipeline: Pipeline_INPE_RiscoFogo_Diaria
Formato dos dados: NetCDF (.nc)
Periodicidade: Diária

🔹 Extração (RAW)

Job: capture_ingesta_inpe_risco_fogo_diario

Armazenamento RAW:
/Volumes/datamasters/raw/raw_inpe

🔹 Ingestão Bronze

Job: ingesta_d_risco_fogo

Tabela Bronze:
datamasters.b_inep.ingesta_d_risco_fogo

🔹 Processamento Silver

Job: d_firerisk_inc_silver

Tabela Silver:
datamasters.s_inep.d_firerisk_inc_silver

🔹 Agregação Gold

Job: d_risco_fogo_gold_agg

Tabela Gold:
datamasters.g_inep.d_risco_fogo_gold_agg

🌳 Pipeline — Desmatamento PRODES (Execução Eventual)

Pipeline: Pipeline_tbra_xml_Eventual.yaml
Formato dos dados: XML
Periodicidade: Eventual (sob demanda)

🔹 Extração (RAW)

Job: INPE_Raw_Download

Armazenamento RAW:
/Volumes/datamasters/raw/raw_tbra

🔹 Ingestão Bronze

Job: ingesta_raw_xml

Tabela Bronze:
datamasters.b_tbra.e_prodes_brasil

🔹 Processamento Silver

Job: process_silver_xml

Tabela Silver:
datamasters.s_tbra.prodes_brasil_process

🔹 Agregação Gold

Job: e_tbras_xml_gold_valor

Tabela Gold:
datamasters.g_tbra.prodes_brasil_valor

⏱️ Pipeline — Focos de Queimadas (Streaming 10 Min)

Pipeline: Pipeline_inpe_focos_on.yaml
Formato dos dados: CSV
Execução: Manual (iniciar e cancelar)

🔹 Captura Near Real-Time (RAW)

Notebook: Inpe_Focos_queim_Stream_10.ipynb

Fonte:
https://dataserver-coids.inpe.br/queimadas/queimadas/focos/csv/10min/

Armazenamento RAW:
/Volumes/datamasters/raw/raw_inpe/inpe_in/


**EM docs/ em a evidencia de teste intergrado,com a execuções completas das pipelines**


VII. 📊 Observabilidade

A observabilidade da solução é garantida por:

Monitoramento de execuções via Databricks Jobs

Logs de falhas e tempo de execução

Métricas de consumo de recursos

Base para integração futura com Azure Monitor


VIII. 🔐 Segurança e LGPD

Controle de acesso baseado em RBAC do Azure

Segregação de workspaces, volumes e tabelas

Criptografia de dados at rest e in transit

Práticas alinhadas à Lei Geral de Proteção de Dados (LGPD)


IX. 🕶️ Mascaramento de Dados

Quando aplicável:

Criptografia de campos sensíveis via PySpark

Mascaramento lógico nas camadas Silver e Gold

Acesso restrito via APIs ou visões controladas


X. 📈 Escalabilidade

A solução foi projetada para escalar de forma eficiente por meio de:

Auto Scaling de clusters Databricks

Processamento distribuído Spark

Ajuste dinâmico de recursos conforme custo e demanda

Preparação para expansão com arquiteturas de streaming mais robustas


XI. 🔁 Reprodutibilidade

Para garantir reprodutibilidade da arquitetura:

Todo o código está versionado em GitHub

Pipelines definidos em YAML

Scripts de ingestão e processamento incluídos

Documentação clara para execução em outro ambiente

Pré-requisitos

Conta ativa na Microsoft Azure

Workspace Databricks configurado

Cluster com suporte a PySpark e Delta Lake


XII. 🚀 Melhorias Futuras

Integração com Apache Kafka

Ingestão direta de imagens de satélite

Dashboards analíticos (Power BI)

Catálogo de dados e lineage

Modelos preditivos de risco ambiental


XIII. 📌 Considerações Finais

Este projeto demonstra a aplicação prática de conceitos modernos de engenharia de dados em um cenário real e de alto impacto social. O uso de dados públicos ambientais, aliado a uma arquitetura escalável e segura, possibilita análises relevantes para o monitoramento e preservação da Floresta Amazônica.

📎 Repositório do Projeto

🔗 GitHub
https://github.com/Rubensfs/case_ed_rubensfs2025
✍️ **Autor**: *Rubens Ferreira de Souza*
📅 **Ano**: 2025

```
```

