# AZURE-GLOBAL
# Databricks Medallion Pipeline
### Azure Blob Storage · PySpark · Medallion Architecture

Pipeline de dados construído com **Databricks Community Edition** e **Azure Blob Storage**, seguindo a arquitectura Medallion (Bronze → Silver → Gold) para processar dados de performance de estudantes.

Desenvolvido para demonstração de pipeline de dados com Databricks e Medallion Architecture.


---

## Arquitectura

```
Azure Blob Storage
       │
       ▼
┌─────────────────────────────────────────────────────────┐
│                  Databricks Workspace                    │
│                                                         │
│  ┌──────────┐   ┌──────────┐   ┌──────────┐           │
│  │  BRONZE  │ → │  SILVER  │ → │   GOLD   │           │
│  │  RAW     │   │  CLEAN   │   │  ANALYTICS│           │
│  └──────────┘   └──────────┘   └──────────┘           │
└─────────────────────────────────────────────────────────┘
       │
       ▼
Azure Blob Storage (processed/)
├── bronze/  → dados raw em Parquet
├── silver/  → dados limpos em Parquet
└── gold/    → agregações em Parquet
```

### Camadas

| Camada | Pasta | Descrição |
|--------|-------|-----------|
| **00 · Integração** | `notebooks/00_integracao/` | Leitura do CSV original do Azure |
| **01 · Bronze (RAW)** | `notebooks/01_bronze/` | Ingestão fiel sem transformação |
| **02 · Silver** | `notebooks/02_silver/` | Limpeza, validação e normalização |
| **03 · Gold** | `notebooks/03_gold/` | Agregações analíticas prontas a consumir |

---

## Estrutura do Repositório

```
databricks-medallion-pipeline/
│
├── README.md
├── configs/
│   └── pipeline_config.py        # Configurações centralizadas
│
├── notebooks/
│   ├── 00_integracao/
│   │   └── 00_leitura_azure.py
│   ├── 01_bronze/
│   │   └── 01_bronze_ingestion.py
│   ├── 02_silver/
│   │   └── 02_silver_cleaning.py
│   └── 03_gold/
│       └── 03_gold_aggregations.py
│
└── docs/
    ├── SETUP.md                  # Guia de instalação e configuração
    └── DATA_DICTIONARY.md        # Dicionário de dados
```

---


### 1. Pré-requisitos

- Conta [Databricks Community Edition](https://community.cloud.databricks.com/)
- Conta Azure com Blob Storage criado
- Python 3.8+
- Biblioteca `azure-storage-blob`

### 2. Configurar Secrets no Databricks

```bash
# Instalar Databricks CLI
pip install databricks-cli

# Autenticar
databricks configure --token
# Host: https://community.cloud.databricks.com
# Token: (gerar em User Settings > Access Tokens)

# Criar scope e guardar chave
databricks secrets create-scope --scope azure-storage
databricks secrets put --scope azure-storage --key storage-key
```

### 3. Instalar dependências no cluster

```python
%pip install azure-storage-blob pyarrow
```

### 4. Executar notebooks por ordem

```
00_leitura_azure.py  →  01_bronze_ingestion.py  →  02_silver_cleaning.py  →  03_gold_aggregations.py
```

---

## Dataset

Link do KAGGLE: https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce?select=olist_geolocation_dataset.csv




## RELACIONAMENTO DA TABELAS 

<img width="2486" height="1496" alt="image" src="https://github.com/user-attachments/assets/86ed4827-e63a-4fb1-8ac8-8c5a90fde75c" />



---

## Stack Tecnológico

| Tecnologia | Versão | Uso |
|------------|--------|-----|
| Databricks | Community Edition | Plataforma de processamento |
| Apache Spark | 3.x | Motor de processamento distribuído |
| PySpark | 3.x | API Python para Spark |
| Azure Blob Storage | — | Armazenamento de dados |
| azure-storage-blob | 12.x | SDK Python para Azure |
| PyArrow | 12.x | Serialização Parquet |
| pandas | 2.x | Manipulação de dados |


---

## Documentação adicional

- [Guia de Setup completo](docs/SETUP.md)
- [Dicionário de Dados](docs/DATA_DICTIONARY.md)

---



RESULTADO DA EXECUÇÃO DO PIPELINE

<img width="1656" height="812" alt="image" src="https://github.com/user-attachments/assets/c295479d-9061-4671-8d5e-293554b1db1c" />


## Autor: Eng. LAURINDO DUMBA
