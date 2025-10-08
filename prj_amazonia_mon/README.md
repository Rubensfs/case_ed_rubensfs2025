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
arquivos para teste está na pasta /massas
as origens são 
INPE(dados estruturados): https://terrabrasilis.dpi.inpe.br/queimadas/portal/pages/secao_downloads/dados-abertos/#da-rf
Terrabra(dados semi-estruturados): https://terrabrasilis.dpi.inpe.br/geonetwork/srv/eng/catalog.search#/home

5 - schedulagem de execução

versão free
em Job Runs / Create job 


---

````

---

✍️ **Autor**: *Rubens Ferreira de Souza*
📅 **Ano**: 2025

```
```

