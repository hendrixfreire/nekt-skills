---
name: nekt-data-freshness-check
description: Valida se os dados no data lake estão atualizados — alerta se alguma fonte ficou mais de 24h sem atualizar.
version: 1.0.0
---

# Nekt Data Freshness Check

Verifica a atualidade dos dados carregados no data lake Nekt, garantindo que as fontes estão sendo ingeridas corretamente.

## Uso

Quando o usuário pedir para verificar se os dados estão atualizados:

1. **Descobrir quais tabelas monitorar** — priorizar tabelas de insights e performance:
   ```
   mcp_nekt_get_relevant_tables_ddl(
       question="tables with date columns for freshness monitoring",
   )
   ```

2. **Gerar query de freshness**:
   ```
   mcp_nekt_generate_sql(
       question="latest date available in each table for [cliente], group by table",
       selected_tables=["monitored_tables"]
   )
   ```
   ou fazer uma query customizada:
   ```sql
   SELECT
     'meta_trx_adsinsights' AS tabela,
     MAX(CAST(date_stop AS DATE)) AS ultima_data
   FROM `allsetcomunicacao_bronze`.`meta_trx_adsinsights`
   UNION ALL
   SELECT
     'aw_trx_campaign_performance' AS tabela,
     MAX(CAST(segments_date AS DATE)) AS ultima_data
   FROM `allsetcomunicacao_bronze`.`aw_trx_campaign_performance`
   ...
   ```

3. **Comparar com data/hora atual**:
   - ✅ **OK** — última atualização < 24h
   - ⚠️ **Atraso** — última atualização entre 24h e 48h
   - 🔴 **Crítico** — última atualização > 48h

4. **Apresentar relatório**:
   - Tabela por cliente/plataforma com status
   - Se tudo OK: "✅ Todas as fontes atualizadas"
   - Se há atrasos: destacar quais fontes e há quanto tempo

## Exemplo de saída

```
📊 Freshness Report — 30/04/2026 10:35

TRX:
  ✅ meta_trx_adsinsights          — 5 min atrás
  ✅ aw_trx_campaign_performance   — 2h atrás
  ✅ ig_trx_media                  — 15 min atrás
  ⚠️ rds_trx_contacts_details     — 26h atrás

Unico Skill:
  ✅ meta_unicoskill_adsinsights   — 10 min atrás
  ✅ aw_unicoskill_campaigns       — 1h atrás

ProBeef:
  ✅ meta_probeef_adsinsights      — 30 min atrás
```

## Profiles

Disponível em profiles com MCP Nekt (`default` e `analista`).
