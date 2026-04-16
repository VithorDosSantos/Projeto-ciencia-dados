# Checklist Anti-Leakage

                      Coluna Presente no dataset    Ação                                                    Justificativa
              total_loss_usd                   ✅ REMOVER                 Variável usada para criar o label severity_label
           total_loss_method                   ✅ REMOVER                                       Metadado do total_loss_usd
      total_loss_lower_bound                   ✅ REMOVER                           Derivada/relacionada ao total_loss_usd
      total_loss_upper_bound                   ✅ REMOVER                           Derivada/relacionada ao total_loss_usd
      inflation_adjusted_usd                   ✅ REMOVER                 Derivada do total_loss_usd ajustada por inflação
              cpi_index_used                   ✅ REMOVER                                   Metadado do ajuste de inflação
                  loss_ratio                   ✅ REMOVER   Derivada de direct_loss/total_loss (total_loss define o label)
                 review_flag                   ✅ REMOVER Flag de revisão interna — não disponível no momento do incidente
               quality_score                   ✅ REMOVER               Score de curadoria do dataset — não é feature real
               quality_grade                   ✅ REMOVER                                  Grade derivada do quality_score
           quality_flags_str                   ✅ REMOVER                          Flags de qualidade internas do pipeline
                    is_valid                   ✅ REMOVER                            Flag de validação interna do pipeline
          abnormal_return_1d                   ✅ REMOVER                       Retorno anormal — informação pós-incidente
          abnormal_return_7d                   ✅ REMOVER                       Retorno anormal — informação pós-incidente
         abnormal_return_30d                   ✅ REMOVER                       Retorno anormal — informação pós-incidente
            car_neg1_to_pos1                   ✅ REMOVER                                   CAR — informação pós-incidente
                  car_0_to_7                   ✅ REMOVER                                   CAR — informação pós-incidente
                 car_0_to_30                   ✅ REMOVER                                   CAR — informação pós-incidente
                 car_0_to_90                   ✅ REMOVER                                   CAR — informação pós-incidente
              t_statistic_1d                   ✅ REMOVER                                        Estatística pós-incidente
                  p_value_1d                   ✅ REMOVER                                        Estatística pós-incidente
             t_statistic_30d                   ✅ REMOVER                                        Estatística pós-incidente
                 p_value_30d                   ✅ REMOVER                                        Estatística pós-incidente
              price_1d_after                   ✅ REMOVER                                              Preço pós-incidente
              price_7d_after                   ✅ REMOVER                                              Preço pós-incidente
             price_30d_after                   ✅ REMOVER                                              Preço pós-incidente
post_incident_volatility_30d                   ✅ REMOVER                                       Volatilidade pós-incidente
      days_to_price_recovery                   ✅ REMOVER                         Tempo de recuperação — informação futura
       volume_disclosure_day                   ✅ REMOVER             Volume no dia da divulgação — pode ser pós-incidente
     volume_ratio_disclosure                   ✅ REMOVER                    Ratio de volume na divulgação — pós-incidente
                company_name                   ✅ REMOVER               Identificador — vazamento de informação específica
                stock_ticker                   ✅ REMOVER               Identificador — vazamento de informação específica
                attack_chain                   ✅ REMOVER           Texto narrativo longo — não estruturado para ML direto
         data_source_primary                   ✅ REMOVER                                    URL — não é feature preditiva
       data_source_secondary                   ✅ REMOVER                                    URL — não é feature preditiva
            attributed_group                   ✅ REMOVER Grupo atribuído — pode não ser conhecido no momento do incidente
               incident_date                   ✅ REMOVER                                    Já extraímos year, month, dow
              discovery_date                   ✅ REMOVER                                   Já extraímos days_to_discovery
             disclosure_date                   ✅ REMOVER                                  Já extraímos days_to_disclosure
     incident_date_estimated                   ✅ REMOVER                                                 Metadado da data