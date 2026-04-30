---
name: nekt-schema-explorer
description: Descobre e documenta a estrutura de tabelas de um cliente nas 3 camadas (Bronze, Silver, Gold) do Nekt.
version: 1.0.0
---

# Nekt Schema Explorer

Explora a estrutura de dados de um cliente através das 3 camadas medalhão (Bronze → Silver → Gold), documentando tabelas, colunas e relações.

## Uso

Quando o usuário pedir para explorar/esquematizar a estrutura de dados de um cliente:

1. **Identificar o cliente** — perguntar qual cliente explorar

2. **Descobrir tabelas relevantes em cada camada**:
   ```
   mcp_nekt_get_relevant_tables_ddl(
       question="[cliente] tables in bronze silver gold layers",
   )
   ```
   Ou buscar diretamente com `selected_tables` se já souber os nomes.

3. **Pré-visualizar dados de tabelas-chave**:
   ```
   mcp_nekt_get_table_preview(table_name="`allsetcomunicacao_bronze`.`meta_<cliente>_adsinsights`")
   ```

4. **Apresentar resultado**:
   - Listar tabelas encontradas por camada (Bronze/Silver/Gold)
   - Mostrar colunas principais de cada tabela
   - Destacar relações entre tabelas (ex: `campaigns` → `ads` → `adsinsights`)
   - Sugerir queries comuns baseadas no schema

## Exemplo

> "Explora a estrutura da TRX"
>
> **Bronze:**
> - `meta_trx_adsinsights` — insights de anúncios (impressões, cliques, gasto, CTR)
> - `aw_trx_campaign_performance` — performance de campanhas Google Ads
> - `ig_trx_media` — métricas de mídia Instagram
> - `rds_trx_contacts_details` — detalhes de contatos RD Station
>
> **Silver:**
> - Tabelas transformadas/limpas prontas para análise
>
> **Gold:**
> - Tabelas agregadas prontas para consumo

## Profiles

Disponível em profiles com MCP Nekt (`default` e `analista`).
