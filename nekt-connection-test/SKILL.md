---
name: nekt-connection-test
description: Testa a conexão do MCP da Nekt — verifica se o endpoint está respondendo e lista os layers disponíveis.
version: 1.0.0
---

# Nekt Connection Test

Teste rápido de conectividade com o MCP da Nekt.

## Uso

Quando o usuário pedir para testar a conexão Nekt, execute:

1. **Testar list_layers** — chamada mais simples pra validar auth + endpoint:
   ```
   mcp_nekt_list_layers()
   ```

2. **Resultado esperado** — se funcionar, retorna algo como:
   ```
   ✅ **Conexão Nekt OK!**
   Layers: Bronze, Silver, Gold
   Databases: allsetcomunicacao_bronze, allsetcomunicacao_silver, allsetcomunicacao_gold
   ```

3. **Se falhar** — reportar o erro:
   - Timeout → verificar se `NEKT_MCP_URL` está acessível
   - 401/403 → verificar se `NEKT_MCP_TOKEN` expirou
   - Connection refused → Nekt pode estar fora do ar

## Profiles

Disponível em todos os profiles que têm o MCP Nekt configurado (`default` e `analista`).

## Variáveis de ambiente necessárias

- `NEKT_MCP_URL` — endpoint HTTP do MCP Nekt
- `NEKT_MCP_TOKEN` — JWT Bearer token
