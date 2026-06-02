# Output Targets

Notion is optional. Markdown is the guaranteed fallback.

This repo must be usable without Notion, without a private workspace, and without any MCP connector.

---

## Detection Rule

At runtime, the agent should inspect available tools / connectors / MCP resources.

If a Notion MCP or connector is available:

1. Tell the user Notion output is available.
2. Ask whether to create or update a structured Notion page.
3. If the user agrees, write prep page and Gap Report to Notion.
4. Also keep local markdown if the user asks for a local copy.

If Notion is not available:

1. Do not error.
2. Write markdown files in `output_dir`.
3. Report the file paths.

---

## Markdown Outputs

Default files:

```text
<output_dir>/intel.md
<output_dir>/pi_perspective.md
<output_dir>/candidate_twin.md
<output_dir>/interview_prep.md
<output_dir>/gap_report.md
```

`candidate_twin.md` is only produced in `mode=twin`.

---

## Notion Page Shape

When Notion is available, use the same content sections as the markdown prep page:

1. PI quick profile
2. Interview priorities
3. Question practice
4. Candidate evidence map
5. Questions to ask the PI
6. Final checklist
7. Gap Report, after mock or companion rounds

The Notion implementation should be thin:

- no hard-coded database IDs
- no private workspace names
- no required template page
- no hidden dependency on a specific Notion schema

If the connector requires a parent page or database and none is configured, ask the user for the destination. Otherwise fall back to markdown.

---

## Failure Handling

| Failure | Behavior |
|---|---|
| Notion connector absent | Markdown fallback |
| Notion auth expired | Markdown fallback and tell user |
| Parent page missing | Ask once; if unavailable, markdown fallback |
| API validation error | Write markdown fallback and include a short error note |
| Partial write (e.g. prep page created, Gap Report append fails mid-run) | Write the remaining sections as markdown and report which sections landed in Notion vs markdown |

The workflow should finish even when Notion does not.
