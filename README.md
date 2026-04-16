# Pipeline de Engenharia de Dados — Cibersegurança

## Descrição

Projeto de ciência de dados que implementa um pipeline de engenharia de dados seguindo a **Arquitetura Medallion** (Bronze → Silver → Gold) para um dataset de cibersegurança. O pipeline organiza, valida, limpa e prepara os dados para uso em modelos de Machine Learning.

## Arquitetura de Dados

Este projeto segue o padrão **Medallion Architecture**:

- **Bronze**: Dados brutos e rastreáveis — mínima transformação, preservação da origem. Bronze **não toma decisões**, apenas organiza.
- **Silver**: Dados limpos e validados — quality flags, features derivadas, tabelas separadas por domínio. Registros inválidos **não são removidos**, recebem `is_valid = False` e `quality_flags`.
- **Gold**: Dados prontos para consumo específico (ML) — merge das tabelas, criação do label (`severity_label`), remoção de colunas com data leakage.

## Estrutura de Pastas

```
Projeto-ciencia-dados/
├── data/
│   ├── bronze/                          # Camada Bronze (CSV/Parquet + metadados)
│   │   ├── incidents_master.csv
│   │   ├── incidents_master.parquet
│   │   ├── financial_impact.csv
│   │   ├── financial_impact.parquet
│   │   ├── market_impact.csv
│   │   ├── market_impact.parquet
│   │   └── metadata_ingestao.json
│   ├── silver/                          # Camada Silver (CSV/Parquet por domínio)
│   │   ├── incidents_silver.csv
│   │   ├── incidents_silver.parquet
│   │   ├── financial_silver.csv
│   │   ├── financial_silver.parquet
│   │   └── market_silver.csv
│   │   └── market_silver.parquet
│   └── gold/                            # Camada Gold (dataset ML)
│       └── dataset_ml_final.csv
│       └── dataset_ml_final.parquet
├── reports/
│   ├── data_quality_report.md
│   ├── data_lineage.md
│   └── anti_leakage_checklist.md
├── notebooks/
│   └── main_pipeline.ipynb              # Notebook principal
├── financial_impact.csv                 # Dados brutos originais
├── incidents_master.csv
├── market_impact.csv
└── README.md                            # Este arquivo
```

## Dados de Entrada

| Arquivo | Registros | Colunas | Descrição |
|---|---|---|---|
| `incidents_master.csv` | ~850 | 32 | Tabela principal de incidentes (vetor de ataque, indústria, empresa, etc.) |
| `financial_impact.csv` | ~778 | 19 | Impacto financeiro (perdas, resgate, custos de recuperação) |
| `market_impact.csv` | ~358 | 31 | Impacto no mercado de ações (somente empresas públicas) |

**Chave de ligação**: `incident_id` (presente nas 3 tabelas)

## Instruções de Execução

### Pré-requisitos

- Python 3.10+
- pip

### Instalação

```bash
# Criar ambiente virtual (opcional, mas recomendado)
python -m venv .venv

# Ativar ambiente (Windows)
.venv\Scripts\activate

# Instalar dependências
pip install pandas pyarrow plotly matplotlib seaborn scikit-learn kaleido
```

### Execução

1. Abrir o notebook `notebooks/main_pipeline.ipynb` no VS Code ou Jupyter
2. Executar todas as células em ordem (Run All)
3. Os arquivos Bronze, Silver e Gold serão gerados em `data/bronze/`, `data/silver/` e `data/gold/` em CSV e Parquet
4. Os relatórios e documentação do pipeline ficam em `reports/`

## Pipeline — Etapas

### 1. Camada Bronze
- Leitura dos CSVs originais
- Padronização de nomes de colunas (snake_case)
- Conversão segura de tipos (bool, date, datetime) — se falhar, mantém como string
- Salvamento em CSV e Parquet
- Registro de metadados (hash SHA-256, contagem de linhas, timestamp)

### 2. Análise de Qualidade
- 11 regras automáticas de validação (QUALITY_RULES)
- Análise de nulos, duplicatas, categorias inconsistentes, datas inválidas
- Relatório consolidado de qualidade

### 3. Camada Silver
- Deduplicação por `incident_id`
- Geração de `quality_flags` (lista padronizada de strings) e `is_valid`
- Features derivadas: `days_to_discovery`, `days_to_disclosure`, `incident_year`, `incident_month`, `incident_dow`, `loss_ratio`
- Remoção de colunas administrativas (`created_at`, `updated_at`, `notes`)
- Tabelas mantidas **separadas** por domínio
- Persistência da Silver em CSV e Parquet por domínio

### 4. Camada Gold
- Merge via LEFT JOIN (incidents ← financial ← market)
- Criação do `severity_label` baseado em quartis de `total_loss_usd`:
  - `low` (≤ Q25), `medium` (≤ Q50), `high` (≤ Q75), `critical` (> Q75)
- Remoção de colunas com data leakage (checklist documentado no notebook)
- Validação pós-merge (% de NaN por coluna)

### 5. EDA (6 gráficos)
1. Distribuição por vetor de ataque
2. Evolução temporal dos incidentes
3. Impacto financeiro por indústria (boxplot)
4. Heatmap de correlação
5. Distribuição do severity_label
6. Severidade vs vetor de ataque

### 6. Modelo ML (Extra)
- Random Forest e Gradient Boosting (comparativo)
- Métricas: accuracy, classification report, confusion matrix
- Feature importance

## Checklist de Entregáveis

| # | Entregável | Localização |
|---|---|---|
| 1 | Notebook principal | `notebooks/main_pipeline.ipynb` |
| 2 | Bronze em Parquet | `data/bronze/*.parquet` |
| 3 | Silver em Parquet | `data/silver/*.parquet` |
| 4 | Dataset Gold | `data/gold/dataset_ml_final.parquet` |
| 5 | Metadados de ingestão | `data/bronze/metadata_ingestao.json` |
| 6 | Relatório de qualidade | `reports/data_quality_report.md` |
| 7 | Data lineage (Mermaid) | `reports/data_lineage.md` |
| 8 | Checklist anti-leakage | `reports/anti_leakage_checklist.md` |
| 9 | EDA (6 gráficos) | Seção 7 do notebook |
| 10 | README | Este arquivo |
| 11 | **Extra**: Modelo ML | Seção 8 do notebook |

## Regras de Qualidade (QUALITY_RULES)

| Regra | Descrição |
|---|---|
| `invalid_date_order` | incident_date > discovery_date ou discovery_date > disclosure_date |
| `negative_price` | Preço de ação negativo |
| `missing_total_loss` | total_loss_usd ausente |
| `loss_out_of_bounds` | total_loss fora dos limites [lower_bound, upper_bound] |
| `ransom_paid_exceeds_demanded` | ransom_paid_usd > ransom_demanded_usd |
| `quality_score_out_of_range` | quality_score fora de [0, 100] |
| `confidence_tier_invalid` | confidence_tier fora de [1, 4] |
| `duplicate_incident_id` | incident_id duplicado |
| `invalid_date_format` | Data em formato inválido |
| `negative_financial_value` | Valor financeiro negativo |
| `missing_incident_id` | incident_id ausente |
