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
     - d_foco_queim_inc
     - d_risco_fogo
     - d_risco_fogo_prec
     - m_area_queim
   - Armazenamento bruto em `catalog = "amazonia_catalog" schema = "b_tbra"`.
     - e_limites_biomas

A - https://terrabrasilis.dpi.inpe.br/queimadas/portal/pages/secao_downloads/dados-abertos/#da-rf

 "Focos de Queimadas e Incêndios"
   - "Arquivos disponibilizados contendo informações sobre focos de queimadas e incêndios florestais em vários intervalos, desde anuais até em tempo quase real (a cada 10 minutos). Esses dados estão disponíveis nos formatos CSV (Comma-Separated Values)"
 "Área Queimada"
   Arquivos contendo informações sobre áreas queimadas. Esses dados estão disponíveis nos formatos TIFF (Tagged Image File Format)
 "Risco de Fogo e Meteorologia"
   Os dados de Risco de Fogo e Meteorologia são categorizados como observados diariamente e previstos para os próximos 3 dias
    Risco de Fogo Observado
     - Risco de Fogo
     - Precipitação
B -  Limites dos Biomas - (dados não estruturado)

https://terrabrasilis.dpi.inpe.br/geonetwork/srv/eng/catalog.search#/metadata/0d88678e-4cdb-44f3-9b1d-8edc00bc4122


"Polígonos dos novos limites dos biomas, para uso auxiliar, provenientes do dado original composto pelos limites dos Biomas do Brasil.

Os limites dos biomas brasileiros foram alterados conforme publicação do IBGE de 30/10/2019. Este conjunto de dados foi ajustado para o novo recorte. https://agenciadenoticias.ibge.gov.br/agencia-sala-de-imprensa/2013-agencia-de-noticias/releases/25798-ibge-lanca-mapa-inedito-de-biomas-e-sistema-costeiro-marinho"



