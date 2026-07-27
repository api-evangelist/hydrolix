---
name: Explore databases and run a SELECT query
description: Discover Hydrolix databases and tables, inspect a table schema, then run a read-only ClickHouse SQL query — via the official Hydrolix MCP server.
api: mcp/hydrolix-mcp.yml
method: generated
generated: '2026-07-19'
operations:
  - list_databases
  - list_tables
  - get_table_info
  - run_select_query
---

# Explore databases and run a SELECT query

Hydrolix is a real-time log-analytics / streaming data lake. Query it with the
ClickHouse SQL dialect through the official Hydrolix MCP server
(`uvx mcp-hydrolix`), which connects to your cluster with a service-account or
user token.

## Prerequisites
- A running Hydrolix cluster hostname and query credentials (or a service account).
- The `mcp-hydrolix` server installed and configured (see `mcp/hydrolix-mcp.yml`).

## Steps
1. **List databases** — call `list_databases` to see the databases on the cluster.
2. **List tables** — call `list_tables` for the target database.
3. **Inspect the schema** — call `get_table_info` on the chosen table to get its
   columns/types before writing SQL. Never guess column names.
4. **Query** — call `run_select_query` with a read-only `SELECT`. This surface is
   read-oriented; scope every query with a time filter on the table's primary
   timestamp column and a `LIMIT`, since Hydrolix retains full-fidelity logs over
   long windows and unbounded scans are expensive.

## Rules
- Use ClickHouse SQL syntax and functions.
- Prefer explicit time-range predicates; avoid `SELECT *` on wide tables.
- If a query times out or hits an out-of-memory / circuit-breaker error, narrow the
  time range and columns — see the official `debugging-hydrolix-queries` skill in the
  Hydrolix AI Toolkit (`claude plugin install hydrolix/hydrolix-ai-toolkit`).
