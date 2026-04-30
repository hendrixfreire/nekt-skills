---
name: nekt-client-report
description: Gera relatórios consolidados de performance de um cliente específico usando SQL no Gold layer do Nekt.
version: 1.0.0
---

# Nekt Client Report

Gera relatórios de performance de mídia para um cliente, consultando dados prontos da camada Gold.

## Uso

Quando o usuário pedir um relatório de um cliente específico:

1. **Identificar o cliente** — perguntar qual cliente (Unico Skill, TRX, Iguatemi, ProBeef, etc.)

2. **Descobrir tabelas Gold do cliente**:
   ```
   mcp_nekt_get_relevant_tables_ddl(
       question="gold layer tables for [cliente]",
       selected_tables=["gold_table_names"]
   )
   ```

3. **Gerar SQL para o relatório**:
   ```
   mcp_nekt_generate_sql(
       question="[descrição do relatório — gasto, leads, CPA, campanhas]",
       selected_tables=["gold_cliente_tables"]
   )
   ```

4. **Executar e apresentar**:
   ```
   mcp_nekt_execute_sql(sql_query=resultado["sql"])
   ```
   - Formatar resposta como tabela ou bullets
   - Incluir período, métricas principais (impressões, cliques, CTR, gasto, CPA, ROAS)
   - Destacar tendências (↑/↓) e anomalias

## Dicas

- Para **relatório semanal**: comparar semana atual vs. anterior
- Para **relatório mensal**: MTD vs. mês anterior
- Sempre perguntar o período se o usuário não especificar
- Clientes conhecidos: Unico Skill (Meta+Google), TRX (Meta+Google+Inst), Iguatemi (Meta+Google), ProBeef (Meta), ANH (Google), Fundação Cargill (Google)

## Profiles

Disponível em profiles com MCP Nekt (`default` e `analista`).
