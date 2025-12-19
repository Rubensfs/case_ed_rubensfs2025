# Case de Engenharia de Dados — Monitoramento Ambiental da Amazônia

**Projeto:** case_ed_rubensfs2025
**Certificação:** DataMasters — Engenharia de Dados (Santander | 2025)

---

## Resumo Executivo

Este projeto apresenta uma solução corporativa de **engenharia de dados em ambiente cloud**, desenvolvida para ingestão, processamento, armazenamento e análise de dados ambientais públicos do **INPE**, com foco no monitoramento da Floresta Amazônica.

A solução adota uma **arquitetura Lakehouse**, combinando pipelines batch, eventuais e near real-time, utilizando **Databricks, PySpark e Delta Lake** na **Microsoft Azure**. O objetivo é demonstrar, de forma prática, competências técnicas, arquiteturais e de governança de dados exigidas em ambientes corporativos de alta criticidade, como o setor financeiro.

---

## 1. Objetivo do Case

Projetar e implementar uma arquitetura de dados escalável, segura e reprodutível, capaz de:

* Consumir dados públicos confiáveis
* Processar grandes volumes de dados ambientais
* Disponibilizar informações consolidadas para análise
* Permitir evolução para cenários de processamento near real-time

Este case foi desenvolvido como parte do processo de **certificação DataMasters — Engenharia de Dados**, evidenciando boas práticas aplicáveis a ambientes corporativos e regulados.

---

## 2. Tema

**Meio Ambiente — Monitoramento da Floresta Amazônica**

A escolha do tema se justifica pela relevância ambiental, social e econômica, aliada à ampla disponibilidade de dados governamentais abertos mantidos pelo **Instituto Nacional de Pesquisas Espaciais (INPE)**.

---

## 3. Fontes de Dados

Os dados utilizados são obtidos a partir do portal oficial **TerraBrasilis / INPE**:

🔗 [https://terrabrasilis.dpi.inpe.br/](https://terrabrasilis.dpi.inpe.br/)

### Conjuntos de Dados Utilizados

**1. Desmatamento — PRODES / INPE**

* Formatos: XML, Shapefile
* Descrição: Dados e mapas de desmatamento por bioma e por período

**2. Focos de Queimadas**

* Formato: CSV
* Periodicidade:

  * Batch diário e mensal
  * Near real-time (atualizações a cada 10 minutos)
* Descrição: Informações sobre focos ativos de queimadas e incêndios florestais

**3. Risco de Fogo e Meteorologia**

* Formatos: NetCDF (.nc), TIF
* Descrição: Dados observados e previsões meteorológicas utilizadas para cálculo de risco de fogo

---

## 4. Arquitetura da Solução

### Visão Geral

A solução segue o padrão **Lakehouse**, integrando Data Lake e camadas analíticas confiáveis:

```
INPE → RAW → BRONZE → SILVER → GOLD → Consumo Analítico
```

### Tecnologias Utilizadas

| Camada        | Tecnologia             |
| ------------- | ---------------------- |
| Cloud         | Microsoft Azure        |
| Processamento | Databricks + PySpark   |
| Armazenamento | Delta Lake             |
| Orquestração  | Databricks Jobs (YAML) |
| Versionamento | GitHub                 |
| Monitoramento | Databricks Jobs UI     |

---

## 5. Diagramas de Arquitetura (C4 Model)

A documentação visual segue o **C4 Model**, facilitando o entendimento da solução em diferentes níveis de abstração.

* **C4 – Nível 1 (Contexto):** `diagrams/c4_context_amazonia.drawio`
* **C4 – Nível 2 (Containers):** `diagrams/c4_container_architecture.drawio`
* **C4 – Nível 3 (Componentes):** `diagrams/c4_pipeline_components.drawio`

---

## 6. Arquitetura de Dados — Medalhão

| Camada | Descrição                              |
| ------ | -------------------------------------- |
| RAW    | Dados brutos, sem transformações       |
| BRONZE | Padronização inicial e versionamento   |
| SILVER | Dados tratados, tipados e confiáveis   |
| GOLD   | Dados agregados e prontos para análise |

---

## 7. Pipelines Implementados

As pipelines foram desenvolvidas considerando padrões corporativos, com separação clara de responsabilidades, controle de falhas e reprocessamento.

### Pipeline — Focos de Queimadas (Batch Diário)

* **Pipeline:** Pipeline_INPE_FocosQueimadas_Diaria
* **Formato:** CSV
* **Periodicidade:** Diária

Camadas impactadas:

* RAW: `/Volumes/datamasters/raw/raw_inpe`
* BRONZE: `datamasters.b_inep.focos_queimadas_diario`
* SILVER: `datamasters.s_inep.d_foco_queim_format`
* GOLD: `datamasters.g_inep.d_focos_queimadas_agg`

---

### Pipeline — Risco de Fogo (Batch Diário)

* **Pipeline:** Pipeline_INPE_RiscoFogo_Diaria
* **Formato:** NetCDF (.nc)
* **Periodicidade:** Diária

Camadas impactadas:

* RAW: `/Volumes/datamasters/raw/raw_inpe`
* BRONZE: `datamasters.b_inep.ingesta_d_risco_fogo`
* SILVER: `datamasters.s_inep.d_firerisk_inc_silver`
* GOLD: `datamasters.g_inep.d_risco_fogo_gold_agg`

---

### Pipeline — Desmatamento PRODES (Execução Eventual)

* **Pipeline:** Pipeline_tbra_xml_Eventual
* **Formato:** XML
* **Execução:** Sob demanda

Camadas impactadas:

* RAW: `/Volumes/datamasters/raw/raw_tbra`
* BRONZE: `datamasters.b_tbra.e_prodes_brasil`
* SILVER: `datamasters.s_tbra.prodes_brasil_process`
* GOLD: `datamasters.g_tbra.prodes_brasil_valor`

---

### Pipeline — Focos de Queimadas (Near Real-Time)

* **Pipeline:** Pipeline_inpe_focos_on
* **Formato:** CSV
* **Execução:** Manual (start / stop)
* **Frequência:** A cada 10 minutos

Fonte:
[https://dataserver-coids.inpe.br/queimadas/queimadas/focos/csv/10min/](https://dataserver-coids.inpe.br/queimadas/queimadas/focos/csv/10min/)

RAW:
`/Volumes/datamasters/raw/raw_inpe/inpe_in/`

---

## 8. Observabilidade e Custos

* Monitoramento de execuções via Databricks Jobs
* Logs de falhas e tempo de execução
* Uso de clusters com auto scaling e auto-terminate
* Base preparada para integração com Azure Monitor

---

## 9. Segurança e LGPD

* Controle de acesso baseado em RBAC
* Segregação de ambientes, volumes e tabelas
* Criptografia de dados at rest e in transit
* Práticas alinhadas à LGPD

---

## 10. Reprodutibilidade

* Código versionado em GitHub
* Pipelines definidas em YAML
* Scripts de ingestão e processamento documentados
* **Evidências de testes integrados disponíveis em : prj_amazonia_mon/docs/[ETI] – Evidencia de Teste Intergrado.odt**

---

## 11. Melhorias Futuras

* Integração com Apache Kafka
* Ingestão de imagens de satélite
* Dashboards analíticos (Power BI)
* Catálogo de dados e lineage
* Modelos preditivos de risco ambiental

---

## Considerações Finais

Este projeto demonstra a aplicação prática de engenharia de dados moderna em um cenário real, crítico e de alto impacto social. A arquitetura proposta é escalável, segura e alinhada a padrões corporativos exigidos por instituições financeiras e ambientes regulados.

---

## Repositório do Projeto

🔗 [https://github.com/Rubensfs/case_ed_rubensfs2025](https://github.com/Rubensfs/case_ed_rubensfs2025)

**Autor:** Rubens Ferreira de Souza
**Ano:** 2025
