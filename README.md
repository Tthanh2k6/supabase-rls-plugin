# Supabase RLS Check (Cursor plugin)

A small plugin that helps Cursor and other agents avoid and catch Supabase Row Level Security mistakes.

| Component | What it does |
| --- | --- |
| `rules/supabase-rls.mdc` | Applied when you edit SQL migrations: RLS on every table, no `true` policies, `(select auth.uid())`, no `user_metadata` in policies, safe `security definer` functions and views. |
| `skills/supabase-rls-quick-check` | Walks the agent through a read-only check of a project you own and explains each finding. |
| `.mcp.json` | Registers the open-source [`supabase-rls-check`](https://github.com/Tthanh2k6/supabase-rls-check) MCP server (four read-only, local-only tools: `analyze_sql_export`, `explain_rule`, `scan_public_env`, `diff_results`). Needs [uv](https://docs.astral.sh/uv/). |

The check reads catalog metadata from a query you run yourself in the Supabase SQL Editor. It needs no `service_role` key or database password and never writes to your database. It is a configuration review, not a penetration test.

Need replacement policies and a two-user isolation test? See [Supabase RLS Review on Agensi](https://www.agensi.io/skills/supabase-rls-review).

License: MIT.
