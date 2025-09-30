# case_ed_rubensfs2025
Case para processo de certificação DataMasters_Engenharia de Dados do Santander - 2025

````markdown
# 🌍 Projeto de Engenharia de Dados - Meio Ambiente - Analise e monitoramento da Amazonia brasileira 

## 📌 1. Objetivo do Case  
Este projeto tem como objetivo desenvolver uma arquitetura de **engenharia de dados escalável** para ingestão, processamento e análise de dados relacionados ao **monitoramento ambiental da Amazônia**, com foco em:  
- **Desmatamento** (PRODES/INPE)  
- **Focos de queimadas e incêndios florestais**  
- **Risco de fogo e meteorologia**  
- **Áreas queimadas e monitoramento em tempo real via satélite (NASA FIRMS)**  

A solução final deve permitir responder questões como:  
- Como evoluiu o desmatamento e as queimadas na Amazônia ao longo do tempo  
- Quais estados e regiões apresentam maior concentração de queimadas
- Existe relação entre meteorologia (chuvas, dias sem chuva, risco de fogo) e aumento de focos de incêndio
- Qual a distribuição temporal e espacial das áreas afetadas

---

## 📌 2. Arquitetura da Solução  

A arquitetura segue o **modelo de camadas Medalhão (Bronze → Silver → Gold)**:  

- **Bronze**: ingestão de dados brutos (CSV, XML, NetCDF, TIFF, APIs, streaming).  
- **Silver**: padronização e limpeza dos dados usando PySpark.  
- **Gold**: tabelas analíticas para dashboards e relatórios.  

📊 **Fluxo da Solução**:  

![Arquitetura](docs/architecture_diagram.png)  

### Tecnologias utilizadas  
- **Ingestão**: Python (requests, xmltodict, pandas), PySpark  
- **Armazenamento**: Data Lake (parquet em diretórios Bronze/Silver/Gold)  
- **Processamento**: PySpark (ETL distribuído)  
- **Observabilidade**: Loguru (monitoramento de logs)  
- **Segurança**: Criptografia, mascaramento de dados sensíveis (Faker)  
- **Dashboards**: Apache Superset ou Metabase  

---

## 📌 3. Explicação do Case Desenvolvido  

1. **Fontes de Dados**  
   - **Desmatamento**: PRODES/INPE (XML, shapefiles)  
   - **Focos de Queimadas**: INPE (CSV diário/mensal)  
   - **Risco de Fogo e Meteorologia**: NetCDF, TIFF  
   - **NASA FIRMS**: streaming em tempo quase real (MODIS e VIIRS)  

2. **Ingestão (Bronze)**  
   - Captura de arquivos CSV e XML diretamente de URLs públicas.  
   - Armazenamento bruto em `/data/bronze`.  
   - Suporte a ingestão batch e near-real-time.  

3. **Transformação (Silver)**  
   - Padronização de datas, estados e coordenadas.  
   - Normalização de atributos (e.g. `datahora`, `lat`, `lon`).  
   - Salvamento em formato **parquet** para eficiência.  

4. **Análises (Gold)**  
   - Agregações por estado, município, ano e mês.  
   - Integração com variáveis meteorológicas (chuva, dias sem chuva).  
   - Construção de tabelas analíticas prontas para dashboards.  

5. **Dashboards**  
   - Indicadores visuais de queimadas por estado/ano.  
   - Evolução temporal do desmatamento.  
   - Correlação entre meteorologia e risco de incêndio.  

---

## 📌 4. Melhorias e Considerações Finais  

- **Escalabilidade**: possibilidade de rodar no **Databricks** para grandes volumes.  
- **Streaming**: integração com **Kafka** para ingestão contínua.  
- **Observabilidade avançada**: integração com **Prometheus + Grafana**.  
- **Segurança**: uso de **criptografia em repouso (AES)** e em trânsito (TLS).  
- **Governança de dados**: catálogo de dados com **Hive Metastore**.  

---

## 📂 Estrutura do Repositório  

```bash
case-amazonia-queimadas/
│── README.md
│── requirements.txt
│── docs/
│   └── architecture_diagram.png
│── data/
│   ├── bronze/
│   ├── silver/
│   └── gold/
│── pipelines/
│   ├── ingestao_bronze.py
│   ├── etl_silver.py
│   └── etl_gold.py
│── notebooks/
│   ├── analise_exploratoria.ipynb
│   └── dashboard_mockup.ipynb
│── config/
│   └── settings.yaml
└── dashboards/
````

---

✍️ **Autor**: *Rubens Ferreira de Souza*
📅 **Ano**: 2025

```
```

