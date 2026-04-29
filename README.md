# Pipeline de Engenharia de Dados — Cibersegurança

## Descrição

Projeto de ciência de dados que implementa um pipeline de engenharia de dados seguindo a **Arquitetura Medallion** (Bronze → Silver → Gold) para um dataset de cibersegurança. O pipeline organiza, valida, limpa e prepara os dados para uso em modelos de Machine Learning.

## 🎯 Objetivo do Projeto (CRÍTICO)

**Problema que resolve:** transformar dados brutos de incidentes de cibersegurança em um dataset confiável para análise e modelagem, reduzindo inconsistências, vazamentos de informação e ruído.

**Aplicação real:** apoiar times de risco, compliance e resposta a incidentes a priorizar ações com base na severidade estimada, mesmo antes da divulgação pública completa.

## 🗂️ Contexto do Dataset

- **Tipo:** dados tabulares de incidentes com impactos financeiros e de mercado.
- **Origem:** arquivos CSV fornecidos no repositório (a documentação não indica se é um dataset real ou sintético).
- **Uso:** estudo e prototipação de pipeline e modelos de classificação de severidade.
- **Chave principal:** `incident_id` (presente nas 3 tabelas).

## 🧰 Stack Tecnológica (IMPORTANTE)

- **Python 3.10+**
- **Pandas** (transformações e validações)
- **NumPy** (operações vetorizadas)
- **Scikit-learn** (modelos e métricas)
- **PyArrow** (Parquet)
- **Plotly + Matplotlib + Seaborn** (visualizações e EDA)
- **Jupyter/VS Code** (execução do pipeline)

## ⭐ Diferenciais do Projeto (ALTO IMPACTO)

- **Arquitetura Medallion** aplicada end-to-end (Bronze → Silver → Gold)
- **Data Quality Rules** com regras automáticas e relatório dedicado
- **Anti-data leakage** com checklist explícito de colunas removidas
- **Data lineage** documentada em Mermaid
- **Artefatos de ML** persistidos com metadados de modelo

## 📊 Resultados do Modelo (OBRIGATÓRIO)

- **Melhor modelo:** Gradient Boosting (definido automaticamente na comparação)
- **Accuracy (teste):** 0.9295
- **F1 (macro / weighted):** gerado no notebook na seção 8 (classification report)
- **Interpretação simples:** o modelo atinge ~93% de acurácia no conjunto de teste; para avaliação mais robusta, utilize F1 macro/weighted e matriz de confusão no notebook.

> Fonte: [data/model/model_metadata.json](data/model/model_metadata.json)

## 🧠 Insights ou Conclusões (MUITO IMPORTANTE)

- **Padrões encontrados (EDA):** distribuição por vetor de ataque, evolução temporal dos incidentes, impacto financeiro por indústria e correlações entre variáveis (ver seção 7 do notebook).
- **Impacto dos dados:** qualidade e consistência dos registros melhoram a estabilidade do modelo e reduzem vieses de decisão.
- **O que o modelo revela:** sinais financeiros e temporais contribuem para a previsão de severidade (ver gráfico de feature importance na seção 8).

## 📈 Evidências Visuais (DIFERENCIAL FORTE)

- **Data lineage (Mermaid):** [reports/data_lineage.md](reports/data_lineage.md)
- **EDA (HTML interativo):** [notebooks/iframe_figures/figure_22.html](notebooks/iframe_figures/figure_22.html), [notebooks/iframe_figures/figure_27.html](notebooks/iframe_figures/figure_27.html), [notebooks/iframe_figures/figure_33.html](notebooks/iframe_figures/figure_33.html)
- **Notebook principal (pipeline completo):** [notebooks/main_pipeline.ipynb](notebooks/main_pipeline.ipynb)

## ⚠️ Limitações do Projeto (NÍVEL AVANÇADO)

- **Tamanho do dataset:** base relativamente pequena (centenas de registros), o que limita generalização.
- **Cobertura de mercado:** `market_impact` existe apenas para empresas públicas.
- **Valores ausentes:** há percentuais relevantes de nulos em algumas tabelas (ver relatório de qualidade).
- **Modelo baseline:** sem tuning extensivo e sem validação externa.

## 🚀 Possíveis Melhorias Futuras

- **Deploy do modelo** via API (FastAPI/Flask) com versionamento de artefatos.
- **Pipeline automatizado** (Airflow/Prefect) com agendamento e monitoramento.
- **Cloud** (storage e compute) para ingestão escalável.
- **Validação contínua** com drift e métricas de produção.

## 📌 Exemplo de Saída (BÔNUS)

Trecho do dataset Gold (`dataset_ml_final.csv`):

```csv
incident_id,company_revenue_usd,country_hq,industry_primary,industry_secondary,employee_count,is_public_company,attack_vector_primary,attack_vector_secondary,attribution_confidence,data_compromised_records,data_type,systems_affected,downtime_hours,data_source_type,confidence_tier,incident_year,incident_month,incident_dow,days_to_discovery,days_to_disclosure,direct_loss_usd,direct_loss_method,ransom_demanded_usd,ransom_paid_usd,ransom_source,recovery_cost_usd,legal_fees_usd,regulatory_fine_usd,insurance_payout_usd,price_7d_before,price_disclosure_day,volume_avg_30d_baseline,sector_index,sector_return_same_period,earnings_announcement_within_7d,market_cap_at_disclosure,pre_incident_volatility_30d,severity_label
2021-0508-001,1343769307.79,US,52,54,3940,True,ransomware,,suspected,26888.0,mixed,"payment_systems,HR_systems",16.52,sec_filing,1,2021,5,5,4,39,12600000.0,disclosed,13802654.69,,,9455354.49,2496545.93,90695.25,6756288.97,,,,,,,,,high
2025-1211-001,63670593.18,GB,51,,250,False,phishing,,probable,41426.0,mixed,"file_servers,backup_systems,HR_systems,ERP",,verified_media,3,2025,12,3,64,3,7640471.18,disclosed,,,,5857150.47,1809188.41,,2691027.33,,,,,,,,,medium
2023-0115-001,24806189094.88,US,51,,71369,True,trojan,ddos,,,,HR_systems,,verified_media,4,2023,1,6,184,47,34881599.59,calculated,,,,26404111.95,10330703.43,,31759649.99,262.07,251.95,19782288.0,S&P 500 Information Technology,18.777,True,118198823773.06,27.705,critical
2021-0315-001,139825903.53,US,44-45,,912,True,phishing,,,1107796.0,mixed,"HR_systems,CRM,VPN",,company_pr,2,2021,3,0,19,91,4682151.47,disclosed,,,,3642946.48,1029035.85,,1772460.33,9.37,9.09,458826.0,S&P 500 Consumer Discretionary,2.086,False,648911379.54,17.116,medium
2021-1204-001,691697737.03,US,51,,1662,True,data_breach,,,621233.0,mixed,"SCADA,VPN",,company_pr,2,2021,12,5,7,40,2684607.92,estimated,,,,2574871.33,206822.23,,,14.6,13.36,230932.0,S&P 500 Information Technology,-14.242,False,4735163815.54,38.209,low
```

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
