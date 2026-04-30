---
name: nekt-campaign-diagnostics
description: Diagnostica campanhas com performance atípica — gasto alto com baixo retorno, CPAs fora da meta, criativos com problema.
version: 1.0.0
---

# Nekt Campaign Diagnostics

Identifica campanhas com performance fora do esperado, ajudando a evitar desperdício de verba.

## Uso

Quando o usuário pedir diagnóstico de campanhas ou identificar problemas de performance:

1. **Identificar o cliente e plataforma** — perguntar se necessário

2. **Descobrir tabelas de insights do cliente**:
   ```
   mcp_nekt_get_relevant_tables_ddl(
       question="[cliente] campaign performance diagnostics",
   )
   ```

3. **Executar análise de anomalias** — gerar SQL e executar:
   - Campanhas com **gasto > R$5k e CPA > R$100** (ajustar thresholds conforme cliente)
   - Campanhas com **CTR abaixo da média** do cliente
   - Campanhas com **ROAS < 1** (prejuízo)
   - Anúncios com **alto gasto e zero conversões**
   - Comparação com **período anterior** (WoW/MoM)

   ```
   mcp_nekt_generate_sql(
       question="[query de diagnóstico]",
       selected_tables=["insights_tables"]
   )
   ```
   ```
   mcp_nekt_execute_sql(sql_query=sql)
   ```

4. **Apresentar resultados**:
   - 🔴 **Crítico** — campanhas com desperdício confirmado
   - 🟡 **Atenção** — performance degradando
   - 🟢 **Saudável** — dentro da meta
   - Sugerir ações: pausar criativo, ajustar lance, revisar segmentação

## Thresholds padrão

| Indicador | Alerta | Crítico |
|---|---|---|
| CPA | > meta do cliente × 1,5 | > meta × 3 |
| ROAS | < 1,5 | < 1 |
| CTR | < média do cliente × 0,5 | < média × 0,3 |
| Gasto sem conversão | > R$ 1.000 | > R$ 5.000 |

## Profiles

Disponível em profiles com MCP Nekt (`default` e `analista`).
