---
name: nekt-connection-test
description: Testa a conexão do MCP da Nekt e retorna apenas Conexão OK ou Conexão falhou, com sugestão de correção se houver erro.
version: 1.2.0
---

# Nekt Connection Test

Teste rápido de conectividade com o MCP da Nekt.

## Uso

Quando o usuário pedir para testar a conexão Nekt, execute:

1. **Testar conexão** — usar a chamada mais simples para validar endpoint + autenticação:
   ```
   mcp_nekt_list_layers()
   ```
   Esse tool é o ideal porque testa o endpoint HTTP, o bearer token e a capacidade de acessar dados — tudo numa chamada só.

2. **Se funcionar** — responder **exclusivamente**:
   ```
   ✅ Conexão OK
   ```
   **NUNCA** inclua layers, databases, tabelas, schemas ou qualquer dado retornado pelo tool. O tool retorna dados, você os descarta completamente. O usuário só quer saber se a conexão funciona.

3. **Se falhar** — responder no formato:
   ```
   ❌ Conexão falhou
   Sugestão: <diagnóstico + ação>
   ```

## Regras de resposta

- **Sucesso = só "✅ Conexão OK".** Nada mais. Zero detalhes. Mesmo que o tool retorne layers, databases, nomes — ignore tudo.
- **Falha = "❌ Conexão falhou" + sugestão.** Uma frase de diagnóstico + ação prática.
- O objetivo da skill é um health check binário. Não é exploração de schema, não é diagnóstico aprofundado.
- Em caso de falha, dar uma sugestão curta e prática baseada no erro.

## Diagnóstico por tipo de erro

Use o erro retornado pelo tool `mcp_nekt_list_layers()` para gerar a sugestão:

- **Timeout / sem resposta**
  → `NEKT_MCP_URL` pode estar errado ou o MCP Server está offline.
  Ação: confira se a URL foi copiada exatamente da página do MCP Server na Nekt (Settings → MCP Server). Teste com `curl <NEKT_MCP_URL>` no terminal.

- **401 / 403 / Unauthorized / Forbidden**
  → Token inválido, expirado ou header mal formatado.
  Ação: verifique se `NEKT_MCP_TOKEN` é o token atual (gere um novo em Settings → MCP Server se necessário). Confirme que o config.yaml do profile tem `Authorization: Bearer ${NEKT_MCP_TOKEN}` exatamente nesse formato — sem aspas extras, sem espaços antes do Bearer.

- **Connection refused / DNS / host unreachable**
  → O endpoint do MCP Server não está acessível da sua rede.
  Ação: verifique se a URL está correta e se o MCP Server está habilitado (Settings → MCP Server → Enable). Se estiver correto, pode ser firewall/VPN — teste com `curl -v`.

- **Erro interno / 500**
  → Problema no lado do servidor Nekt.
  Ação: aguardar e tentar novamente. Se persistir, contatar suporte Nekt.

- **Qualquer outro erro**
  → Ação: revisar o config.yaml do profile conforme o formato abaixo. Comparar com a doc oficial: https://docs.nekt.com/build-with-ai.md

## Formato de configuração esperado (Hermes profile config.yaml)

```json
{
  "mcpServers": {
    "Nekt": {
      "type": "http",
      "url": "<MCP_ENDPOINT>",
      "headers": {
        "Authorization": "Bearer <MCP_TOKEN>"
      }
    }
  }
}
```

## Profiles

Disponível em todos os profiles que têm o MCP Nekt configurado (`default` e `analista`).

## Variáveis de ambiente necessárias

- `NEKT_MCP_URL` — endpoint HTTP do MCP Nekt
- `NEKT_MCP_TOKEN` — bearer token do MCP Nekt
