---
layout: default
title: API and MCP
description: Holdout as a REST API and an MCP server. Measure how AI names a company, have fixes written and checked, and read a holdout lift verdict. $0.015 per measured answer. Not launched yet.
---
<p class="lead">Everything the app does, your product or agent can do: measure how AI answers name a company, have the fixes written and checked, and read a holdout lift verdict. $0.015 per measured answer, with no monthly minimum.</p>

> Holdout has not launched yet. The API and MCP server are built, but there is no public address and no way to get a key today. Key sign-up is coming soon.

## The same four steps a customer takes

1. **Check a website.** `POST /v1/audits` (tool `create_audit`). It returns at once. Read the audit until it is done.
2. **Attach it to your plan.** `POST /v1/workspaces` (tool `create_workspace`). The first full measurement starts and the holdout split is frozen.
3. **Connect the site, once.** `POST /v1/workspaces/{id}/connections` (tool `connect_site`). Checked fixes are then written and published with no further call, while Autopilot is on.
4. **See what changed, and undo one change.** `GET /v1/workspaces/{id}/autopilot` (tool `list_activity`) and `POST /v1/workspaces/{id}/fixes/{fixId}/undo` (tool `undo_fix`).

Buying the first plan is a step for a person, at checkout. A key only exists once an account has a plan, so an agent with a key always acts inside a plan someone agreed to.

## Keys and scopes

Add an `Authorization: Bearer` header to every request. Keys are shown once and stored only as a hash. Give each key the least it needs.

<dl class="results">
  <dt><code>read</code></dt>
  <dd>Read audits, stats and fixes.</dd>
  <dt><code>measure</code></dt>
  <dd>Start audits and measurement runs. Billed per measured answer.</dd>
  <dt><code>approve</code></dt>
  <dd>Approve drafted fixes. Connecting a site, changing the mode, pausing, approving and undoing all need this scope, because they can change a website once it is connected.</dd>
</dl>

## Endpoints

Every MCP tool has the same name and does the same thing as the endpoint beside it.

<div class="table-wrap" role="region" aria-label="API endpoints" tabindex="0">
<table>
<thead><tr><th scope="col">Endpoint</th><th scope="col">MCP tool</th><th scope="col">Scope</th></tr></thead>
<tbody>
<tr><td><code>POST /v1/audits</code></td><td><code>create_audit</code></td><td><code>measure</code></td></tr>
<tr><td><code>GET /v1/audits/{id}</code></td><td><code>get_audit</code></td><td><code>read</code></td></tr>
<tr><td><code>POST /v1/workspaces</code></td><td><code>create_workspace</code></td><td><code>measure</code></td></tr>
<tr><td><code>GET /v1/workspaces</code></td><td><code>list_workspaces</code></td><td><code>read</code></td></tr>
<tr><td><code>POST /v1/workspaces/{id}/connections</code></td><td><code>connect_site</code></td><td><code>approve</code></td></tr>
<tr><td><code>POST /v1/workspaces/{id}/implement</code></td><td><code>implement_fixes</code></td><td><code>approve</code></td></tr>
<tr><td><code>GET /v1/workspaces/{id}/autopilot</code></td><td><code>list_activity</code></td><td><code>read</code></td></tr>
<tr><td><code>POST /v1/workspaces/{id}/autopilot</code></td><td><code>set_autopilot</code></td><td><code>approve</code></td></tr>
<tr><td><code>POST /v1/workspaces/{id}/autopilot/pause</code></td><td><code>pause_autopilot</code></td><td><code>approve</code></td></tr>
<tr><td><code>POST /v1/workspaces/{id}/fixes/{fixId}/undo</code></td><td><code>undo_fix</code></td><td><code>approve</code></td></tr>
<tr><td><code>GET /v1/workspaces/{id}/fixes</code></td><td><code>list_fixes</code></td><td><code>read</code></td></tr>
<tr><td><code>POST /v1/workspaces/{id}/fixes/{fixId}/approve</code></td><td><code>approve_fix</code></td><td><code>approve</code></td></tr>
<tr><td><code>POST /v1/workspaces/{id}/measure</code></td><td><code>measure_workspace</code></td><td><code>measure</code></td></tr>
<tr><td><code>GET /v1/workspaces/{id}/stats</code></td><td><code>get_stats</code></td><td><code>read</code></td></tr>
<tr><td><code>GET /v1/workspaces/{id}/ai-visits</code></td><td><code>get_ai_visits</code></td><td><code>read</code></td></tr>
</tbody>
</table>
</div>

## Safe retries

Add an `Idempotency-Key` header to any POST. A repeat with the same key and body returns the first response, and nothing runs or is billed twice. The same key with a different body is refused. Long work (audits, measurements, publishing) returns at once and runs in the background, so a timeout never leaves you guessing. Read the resource again.

## Errors

Every error has a stable code, a sentence a person can read and, where it helps, a hint that says what to do next.

```json
{
  "error": {
    "code": "missing_scope",
    "message": "This key does not have the \"approve\" scope.",
    "hint": "Create a key with that scope in Settings."
  }
}
```

## Metering

You pay for measured answers only. One measured answer is one question asked once on one engine. An audit is 16 answers. A workspace measurement is questions times engines times repeats. Reading, connecting and undoing are free, and so is every fix we make to your site.

## MCP server

A stateless MCP server over streamable HTTP, with the same key and scopes as the REST API. It has 15 tools. Tools that only read are marked read-only, so a client can let an agent call them without asking.

An agent can run the whole service for a site: check it, attach it, connect it and let the fixes ship. It can also pause Autopilot and undo any change. It cannot message other websites. No key can, by design.

## What the numbers mean

Every rate comes with its count and a 95% Wilson interval. There is no rank and no single-run result. Each response names the engines and the access method, because answers from an API can differ from the consumer app. The lift verdict is one of `lift_detected`, `no_lift_detected` or `not_enough_data`, with the number of further runs needed. See the [method]({{ '/methodology/' | relative_url }}).
