# Data Lineage

```mermaid
flowchart TD
    subgraph Origem["Origem"]
        A1[incidents_master.csv]
        A2[financial_impact.csv]
        A3[market_impact.csv]
    end

    subgraph Bronze["Bronze"]
        B1[incidents_master.parquet]
        B2[financial_impact.parquet]
        B3[market_impact.parquet]
        B4[metadata_ingestao.json]
    end

    subgraph Validação["Qualidade e Validação"]
        V1[Nulos]
        V2[Duplicatas]
        V3[Categorias inconsistentes]
        V4[Datas fora do padrão]
        V5[Regras automáticas]
        V6[Relatório de qualidade]
    end

    subgraph Silver["Silver"]
        S1[incidents_silver.parquet]
        S2[financial_silver.parquet]
        S3[market_silver.parquet]
        S4[quality_flags + is_valid]
        S5[Features temporais e loss_ratio]
        S6[Remoção de colunas administrativas]
    end

    subgraph Gold["Gold"]
        G1[dataset_ml_final.parquet]
        G2[severity_label]
        G3[Checklist anti-leakage]
        G4[Colunas sensíveis removidas]
    end

    subgraph Consumo["Consumo"]
        C1[EDA]
        C2[Treino de ML]
        C3[Métricas e modelo salvo]
    end

    A1 --> B1
    A2 --> B2
    A3 --> B3
    A1 --> B4
    A2 --> B4
    A3 --> B4

    B1 --> V1
    B1 --> V2
    B1 --> V3
    B1 --> V4
    B1 --> V5
    V5 --> V6

    B1 --> S1
    B2 --> S2
    B3 --> S3
    S1 --> S4
    S2 --> S4
    S3 --> S4
    S1 --> S5
    S2 --> S5
    S1 --> S6
    S2 --> S6
    S3 --> S6

    S1 --> G1
    S2 --> G1
    S3 --> G1
    G1 --> G2
    G1 --> G3
    G1 --> G4

    G1 --> C1
    G1 --> C2
    C2 --> C3
```

## Transformações principais

- Entrada: `incidents_master.csv`, `financial_impact.csv`, `market_impact.csv`
- Bronze: padronização de nomes para `snake_case`, conversão segura de booleanos, datas e timestamps quando possível, persistência em Parquet e registro de metadados com hash SHA-256
- Validação: análise de nulos, duplicatas, categorias inconsistentes, datas fora do padrão e regras automáticas de qualidade
- Silver: deduplicação por `incident_id`, geração de `quality_flags` e `is_valid`, criação de `incident_year`, `incident_month`, `incident_dow`, `days_to_discovery`, `days_to_disclosure` e `loss_ratio`, remoção de colunas administrativas
- Gold: merge das três tabelas, criação de `severity_label`, remoção de colunas com data leakage e salvamento do dataset final para ML
- Consumo: EDA, treino dos modelos, métricas e persistência do artefato final

## Artefatos Gerados

- `data/bronze/*.parquet`
- `data/bronze/metadata_ingestao.json`
- `data/silver/*.parquet`
- `data/gold/dataset_ml_final.parquet`
- `reports/data_quality_report.md`
- `reports/anti_leakage_checklist.md`
