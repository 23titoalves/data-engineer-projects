**Video Game Sales Pipeline - Nexus 23 🎮**

Este projeto implementa um pipeline de dados completo utilizando a arquitetura Medallion no Databricks. O objetivo é processar dados brutos de vendas de video games, transformando-os em um modelo dimensional (Star Schema) otimizado para análise de BI na camada Gold.

**🏗️ Arquitetura do Projeto**

O pipeline segue o fluxo de medalhas para garantir a qualidade e a linhagem dos dados:

⤇ Bronze (Staging): 

⤍ Ingestão dos dados brutos com limpeza inicial de strings via Regex e geração de hashes de controle (id_lote).

⤇ Silver (Modelagem):

⤍ Criação de dimensões (dim_console, dim_publisher, dim_genre) utilizando SCD Tipo 2 (Slowly Changing Dimensions) para rastreabilidade histórica.

⤍ Geração de Surrogate Keys (SK) numéricas via IDENTITY para garantir performance de JOIN.

⤇ Gold (Consumo):

⤍ Consolidação da tabela fato (fact_vg_sales) substituindo hashes por chaves numéricas.

⤍ Otimização via Particionamento por release_year.

⤍ Carga incremental utilizando o comando MERGE.

**🛠️ Tecnologias Utilizadas**
Databricks (Runtime 13.x+)

Apache Spark (PySpark & Spark SQL)

Delta Lake (Acid transactions, Schema Evolution, Merge)

Git Integration (Repositório sincronizado via Databricks Repos)

**📂 Estrutura de Notebooks**
A execução deve seguir a ordem numérica para respeitar as dependências entre as camadas:

01_setup_database: DDL de criação dos schemas e tabelas físicas (Silver e Gold).

02_bronze_to_staging: Processo de limpeza inicial e geração de hashes de integração.

03_create_dims: Processamento e MERGE das tabelas de dimensão na Silver.

04_silver_to_gold: Join final e carga incremental da tabela fato na Gold.

**🚀 Destaques Técnicos**
Integridade Referencial: Uso de SHA256 para hashes de integração e IDs numéricos estáveis para consumo em ferramentas de BI.

Performance: Implementação de Partition Pruning na camada Gold para acelerar consultas por ano de lançamento.

Idempotência: Todo o pipeline foi desenhado para ser executado múltiplas vezes sem duplicar dados, utilizando a lógica de MERGE INTO.