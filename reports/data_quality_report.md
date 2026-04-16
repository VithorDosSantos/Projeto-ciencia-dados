# Relatório de Qualidade dos Dados

## Resumo por camada Bronze

          Tabela  Linhas  Colunas  Células Totais  Células Nulas  % Nulos  Duplicatas (linhas)  Duplicatas (ID)
incidents_master     850       32           27200           5591    20.56                    0                0
financial_impact     778       19           14782           3475    23.51                    0                0
   market_impact     358       31           11098            302     2.72                    0                0

## Regras automáticas

- invalid_date_order: 0
- quality_score_out_of_range: 0
- confidence_tier_invalid: 0
- duplicate_incident_id_incidents: 0
- missing_incident_id_incidents: 0
- missing_total_loss: 0
- loss_out_of_bounds: 0
- ransom_paid_exceeds_demanded: 0
- negative_direct_loss_usd: 0
- negative_recovery_cost_usd: 0
- negative_legal_fees_usd: 0
- negative_price_7d_before: 0
- negative_price_disclosure_day: 0
- negative_price_1d_after: 0
- negative_price_7d_after: 0
- negative_price_30d_after: 0

## Observações

- Bronze preserva os arquivos originais com padronização mínima.
- Silver mantém quality_flags e is_valid por registro.
- Reexecute este relatório após atualizar os CSVs de origem.
