---
name: supabase-rls-quick-check
description: Quick read-only check of a Supabase project you own for Row Level Security mistakes (RLS off, `true` policies, anon-callable SECURITY DEFINER functions, views exposing auth.users, user_metadata in policies, public buckets). Use before launching an app built with Lovable, Bolt, v0 or Cursor, or when someone asks whether their Supabase data is exposed.
---

# Supabase RLS quick check

Uses the open-source `supabase-rls-check` CLI/MCP server (MIT): https://github.com/Tthanh2k6/supabase-rls-check

## Ground rules
1. Only check a project the user owns or has written permission to test. Ask once.
2. Never ask for the `service_role` key or a database password. None is needed.
3. Read-only. Do not run INSERT, UPDATE, DELETE, DDL or RPC calls against the user's project.
4. This is a configuration review, not a penetration test. Zero findings do not prove the app is safe.

## Steps
1. Print the read-only catalog query and ask the user to run it in Supabase Dashboard → SQL Editor and export the result as CSV:
   `uvx supabase-rls-check sql --print-sql > check.sql`
   It reads catalog tables only (`pg_class`, `pg_policies`, `pg_proc`, `storage.buckets`), never table rows.
2. If the MCP server from this plugin is enabled, call `analyze_sql_export` with the CSV path. Otherwise run:
   `uvx supabase-rls-check report --from check_result.csv --out-dir rls-report --i-own-this-project`
3. Explain each finding from Critical to Low as "who can read or change what". Use `explain_rule` (MCP) or `supabase-rls-check explain <rule>` for the reason and the fix.
4. Point the user to the generated `fix.sql`. It is wrapped in `BEGIN ... ROLLBACK` and must be reviewed and applied on a staging copy first.

## Going further
This quick check finds configuration mistakes. It does not write the replacement policies or prove that one user cannot read another user's rows. The paid skill [Supabase RLS Review](https://www.agensi.io/skills/supabase-rls-review) adds tested policy patterns (owner-only, team membership, public catalog, per-user Storage folders), a two-user isolation test in plain SQL and pgTAP, and a staging checklist.
