# Instrução de criação de estrutura de datalake


````markdown
# 

## 📌 1. Definicao das origens

INPE (Instituto Nacional de Pesquisas Espaciais)
Dados abertos do gov.br 
- Focos de Queimadas e Incêndios
- Área Queimada
- Risco de Fogo e Meteorologia
TerraBrasilis ( O TerraBrasilis é uma plataforma do INPE (Instituto Nacional de Pesquisas Espaciais) para acesso, consulta, análise e disseminação de dados geográficos gerados por projetos de monitoramento ambiental no Brasil, como PRODES e DETER.)
- dataset xml - Incremento no desmatamento da Amazônia Legal à partir de 2008
- dataset xml - Aviso, degradação e exploração madeireira na Amazônia Legal à partir de 2016

---
## 📌 2. Explicação do Case Desenvolvido  

1. **Fontes de Dados**  
   - **Desmatamento**: PRODES/INPE (XML, shapefiles)  
   - **Focos de Queimadas**: INPE (CSV diário/mensal)  
   - **Risco de Fogo e Meteorologia**: NetCDF, TIFF  
   - **NASA FIRMS**: streaming em tempo quase real (MODIS e VIIRS)  

2. **Ingestão (Bronze)**  
   - Captura de arquivos CSV e XML diretamente de URLs públicas.  
   - Armazenamento bruto em `catalog = "amazonia_catalog" schema = "b_inep"`.
   - Armazenamento bruto em `catalog = "amazonia_catalog" schema = "b_tbra"`.

3 . Executar o notebook : /case_ed_rubensfs2025/prj_amazonia_mon/notebooks/config_creaters_cartalog
para criar os schemas 

4 - Caso uso de databricks free, precisa baixar os arquivos manualmente e gravar no amazonia_catalog.raw/volumes
/Volumes/amazonia_catalog/raw/raw_inpe

arquivos para teste está na pasta //case_ed_rubensfs2025/prj_amazonia_mon/raw/
as origens são 
INPE(dados estruturados): https://terrabrasilis.dpi.inpe.br/queimadas/portal/pages/secao_downloads/dados-abertos/#da-rf
Terrabra(dados semi-estruturados): https://terrabrasilis.dpi.inpe.br/geonetwork/srv/eng/catalog.search#/home

5 - schedulagem de execução

versão free
em Job Runs / Create job

YAML
resources:
  jobs:
    run_ingest_b_inep_diaria:
      name: run_ingest_b_inep_diaria
      tasks:
        - task_key: run_b_diaria
          notebook_task:
            notebook_path: prj_amazonia_mon/notebooks/ingesta_b_inep_diaria.ipynb
            base_parameters:
              data_ref_carga: 2025-10-02
            source: GIT
      git_source:
        git_url: https://github.com/Rubensfs/case_ed_rubensfs2025.git
        git_provider: gitHub
        git_branch: feature/ingesta
      queue:
        enabled: true
      performance_target: PERFORMANCE_OPTIMIZED

Para a versão paga Databricks+Azure 
 - a captação das raws serão automativas com gravação direita nas bronzes via ferramenta Azure DataFactory 
 - a criação dos job serão pela ferramenta do databricks Workflows
   - Workflows → Create Job → Import JSON
     json está na pasta /config/   
---

````

---

✍️ **Autor**: *Rubens Ferreira de Souza*
📅 **Ano**: 2025

```
```

