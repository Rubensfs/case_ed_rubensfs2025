# Guia de Configuração do Ambiente Databricks

Este documento descreve, de forma padronizada e objetiva, os procedimentos necessários para configurar o ambiente no **Databricks** e executar as pipelines do projeto **Amazônia Monitoramento**.

---

## 1. Criação do Catálogo de Dados

1. Acesse o **Databricks Workspace**.
2. Navegue até o menu **Catalog**.
3. Selecione a opção **Create Catalog**.
4. Preencha os campos conforme abaixo:

   * **Catalog Name**: `datamasters`
   * **Type**: `Standard`

> **Observação:** O catálogo `datamasters` será utilizado como base para armazenamento dos volumes, schemas e tabelas do projeto.

---

## 2. Criação de Volumes e Schemas

1. Execute o notebook responsável pela configuração inicial do ambiente:

```
prj_amazonia_mon/notebooks/config_creaters_cartalog
```

2. A execução deste notebook realiza automaticamente:

   * A criação dos diretórios necessários em `/Volumes/`.
   * A criação dos **schemas** utilizados pelas tabelas das camadas RAW, BRONZE, SILVER e GOLD.

> **Importante:** Este passo é obrigatório antes da criação e execução das pipelines.

---

## 3. Criação das Pipelines (Jobs)

1. Localize o arquivo de definição da pipeline no formato **.yaml** dentro do repositório do projeto.
2. No Databricks, acesse o menu **Jobs & Pipelines**.
3. Clique em **Create** → **Job**.
4. Selecione a opção **Edit as YAML**.
5. Cole o conteúdo do arquivo **.yaml** correspondente à pipeline.
6. Salve as alterações e finalize a criação do Job.

---

## 4. Execução da Pipeline

1. Com o Job devidamente configurado, clique em **Run Now**.
2. Monitore a execução por meio da aba **Runs**.
3. Verifique logs e status para garantir a execução bem-sucedida das etapas da pipeline.

---

## 5. Considerações Finais

Após a conclusão dos passos acima, o ambiente estará devidamente configurado e as pipelines prontas para execução conforme a agenda definida ou sob demanda.

---

✔ **Configuração concluída com sucesso**
