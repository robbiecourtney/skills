# VB.NET WinForms Discovery — skill authoring brief

## Handoff instruction

Use Matt Pocock's `writing-for-agents` skill and its `SKILL-MECHANICS.md` reference to turn this brief into a reusable skill named `vbnet-winforms-discovery` for GitHub Copilot in VS Code. Apply that skill's writing and packaging guidance while preserving the behavioral requirements below. This document is the authoring input; it is not an installed discovery skill.

Create a concise `SKILL.md` with an ordered discovery procedure and checkable completion criteria. Put conditional VB.NET/WinForms guidance and output templates in referenced files. Treat the reference layout suggested below as a starting point. Include all supporting files and a short usage example. Report any requirement you cannot implement instead of silently dropping it.

The authoring task is to create the skill package. Run discovery on an application only when the user subsequently requests it or explicitly includes that in the authoring task. The acceptance cases below are evaluation scenarios, not evidence that the skill has already been tested.

## Purpose and project context

The team maintains several Windows Forms applications written in VB.NET on .NET Framework. The intended replacements use .NET 10, C# ASP.NET Core APIs, and Blazor UIs. GitHub Copilot in VS Code is the primary AI environment; Visual Studio can be used by developers to inspect, run, and debug the legacy applications.

The skill discovers what an existing application does, where its behavior is implemented, and which dependencies matter to a rebuild. Its outputs serve developers, the Senior Product Owner, business experts, and later specification or implementation agents. Keep business explanations readable and put technical evidence close to the finding it supports.

Discovery describes current behavior and its limits. The planned replacement architecture supplies context; it does not establish current behavior or approve a new design. Record desired changes supplied by the user separately from observations of the legacy system.

## Invocation and inputs

Prefer explicit user invocation for a potentially lengthy repository analysis. Use the current Copilot-compatible invocation mechanism supported by the installation. Document both name-based invocation in ordinary chat and any supported command; invocation must not depend on a slash command alone. Have `writing-for-agents` choose compatible frontmatter and explain any host-specific limits.

Accept these inputs in ordinary language; avoid requiring a structured form:

| Input | Default or handling |
| --- | --- |
| Source repository or solution | Use the explicitly identified source or the single unambiguous legacy solution in the workspace. Resolve ambiguity before tracing the wrong application. |
| Mode | `overview`, `workflow`, or `update`. Default: `overview`. |
| Workflow scope | A form, menu action, business process, or named use case. A workflow can span several forms and projects. |
| Output destination | Follow repository conventions; otherwise use `docs/discovery/<app-id>/`. |
| Database and integration source | Use supplied schemas, SQL projects, exports, contracts, or other available source. Record external dependencies whose implementations are unavailable. |
| Existing discovery | Reuse relevant reports and their evidence; validate freshness in `update` mode. |

Inspect the repository before asking questions the source can answer. Ask a small, targeted question only when an ambiguity materially changes the work. Continue independent discovery where possible. Missing runtime or database access is an evidence gap, not a reason to abandon source discovery.

### Mode boundaries

- **Overview:** inventory every discovered project and candidate form in the declared application scope, summarize dependencies and startup/navigation, and trace one representative workflow. Select a workflow with a clear entry point and meaningful behavior when the user has not named one; record why it was selected. Mark remaining workflows as inventoried or deferred.
- **Workflow:** perform a detailed trace of the specified business operation and the relevant dependencies. Create enough local context to explain it; reuse a current inventory when available.
- **Update:** identify source changes since the previous recorded revision, check their impact on existing findings, and refresh affected reports and diagrams. Where the baseline cannot be established, label the refresh partial and identify what was rechecked.

## Operating boundaries

Keep application code, project configuration, dependencies, and databases unchanged. Write only discovery deliverables in the agreed output location. Respect existing work and repository instructions. Preserve unrelated report content when updating.

Use static inspection by default. Follow calls across VB.NET, C#, SQL, configuration, and available shared-library source as needed. Use existing search and navigation tools; scripts are optional aids, not prerequisites. An unavailable VB language service must not prevent textual source analysis.

Launching the legacy app, executing build or test scripts, contacting databases or external services, restoring dependencies, and making application edits are separate activities requiring authorization appropriate to their effects. Reuse explicit session authorization when it already covers the activity. If runtime verification is unavailable, supply focused developer verification steps and label the behavior source-confirmed only.

Represent credentials and personal or production data with redacted placeholders. Document configuration keys and dependency purpose without copying secret values into reports. Use synthetic examples for proposed verification cases.

## Discovery procedure and completion criteria

### 1. Establish the baseline

Resolve application roots, solution/project relationships, source revision, requested mode, and scope. Record branch and commit when available and whether relevant files have local changes; otherwise record a snapshot identifier and its limits. Inspect existing documentation as context and check material claims against implementation.

**Complete when:** the report identifies the source snapshot, application, requested scope, output location, available evidence, and access limitations. Any ambiguity that prevents identifying the source application is resolved.

### 2. Inventory the application

Find solution and project files, target frameworks, project references, package metadata, startup configuration, application events, forms, inherited forms, user controls, shared modules, data access components, and integration boundaries. Exclude build outputs and vendor binaries from ordinary source scanning; account for required binary dependencies in the inventory.

Classify forms using inheritance and project/designer evidence, rather than relying only on filenames. Inspect partial classes together. Record candidates that cannot be resolved statically. Discover startup and navigation from configuration, startup code, menus, form creation, `Show`/`ShowDialog`, default form instances, and other relevant entry points. Mark possible reachability separately from observed runtime navigation.

**Complete when:** every discovered in-scope project and form candidate has a classification and review status, startup is traced or explicitly unresolved, and major data/integration boundaries are listed. Counts and exclusions describe the scan's actual scope; they do not imply every file was deeply reviewed.

### 3. Trace the selected workflow

Identify the user goal, actor, entry point, preconditions, inputs, and expected business outcome. Follow event wiring and executable branches through validation, business logic, database operations, external calls, and UI outcomes. Include initialization and binding events that affect the operation, alternative paths, cancellation, failure handling, cleanup, and relevant transaction boundaries.

For each material action, record its trigger, condition, called symbols, state changes, side effects, and observable result. Follow dependencies outside the initial form until the relevant behavior is explained or an unavailable boundary is reached. At each unavailable boundary record the call/contract, what is known, and which downstream effects remain unknown.

**Complete when:** each identified entry point and material branch in the selected workflow is traced, deferred with a reason, or blocked by a named evidence gap. The trace includes success, validation failure, and technical failure paths where present. A method list alone is insufficient.

### 4. Extract behavior and dependencies

Identify business rules, permissions, calculations, defaults, data transformations, state transitions, and side effects. Record the source condition and its business meaning. If the meaning is uncertain, retain the literal behavior and ask a precise domain question instead of inventing intent.

Map data reads/writes and integrations at the level the source supports: objects, stored procedures, operation types, parameters, transaction handling, and error behavior. Inspect available SQL definitions and triggers that influence traced writes. When definitions are missing, document only observed calls and contracts. Separate UI permission checks from service/database enforcement.

**Complete when:** material rules and side effects in the traced branches have evidence, dependencies resolve to available source or explicit boundaries, and contradictions are preserved for review. Inferred business purpose is labeled as inference.

### 5. Capture implications for the rebuild

Describe which discovered behaviors need consideration in a C# API/Blazor replacement. Classify likely responsibilities as UI interaction, server-enforced business behavior, data/integration access, or a desktop capability requiring a design decision. These are proposed mappings with rationale, not approved architecture or API contracts.

Identify VB language semantics and desktop assumptions relevant to the traced code using the conditional reference below. Prepare representative verification examples: initial state, action/input, expected output, errors, permissions, and data side effects. Keep observed behavior, intended behavior confirmed by a person, known defects, and proposed improvements distinguishable.

**Complete when:** every identified material behavior has a proposed verification case or an explicit reason it cannot yet be tested. Each rebuild concern links to a finding or source location. Questions state who could resolve them and what evidence is needed, without inventing owners.

### 6. Publish and check the discovery

Write or update the outputs defined below. Ensure diagrams agree with the findings and represent unavailable boundaries. Verify links and evidence locations against the recorded snapshot, reconcile coverage counts, and check for unsupported claims. Summarize what was discovered, what remains unverified, and the next useful investigation.

**Complete when:** the declared mode's deliverables exist, required record fields are populated or explicitly unknown, diagram relationships are supported, and the coverage ledger accounts for the scoped work. Report `complete for declared scope` or `partial`, with reasons. Never describe the whole application as understood because one workflow was traced.

## Conditional technical reference requirements

The final package should load these checklists when the corresponding patterns occur. Investigate their effect on the selected workflow; avoid turning every possible pattern into a mandatory application-wide audit.

### WinForms behavior

- Partial classes, `.Designer.vb`, `.resx`, inherited forms, user controls, and third-party controls.
- `Handles`, `WithEvents`, `AddHandler`/`RemoveHandler`, overrides, and indirect or dynamically selected handlers.
- Constructors, `Load`, `Shown`, `Activated`, validation, save/cancel, `FormClosing`, and disposal; ordering and re-entrant events where relevant.
- Control defaults, enabled/visible/read-only state, binding sources, current-row selection, validation and error providers, and implicit saves through binding.
- Modal results, cross-form references, default form instances, shared modules, static/shared fields, and mutable session-like state.
- Timers, background workers, asynchronous work, and UI-thread assumptions.
- `My.Application`, application events, `My.Settings`, per-user settings, Windows identity, and local execution assumptions.

### Data and integrations

- ADO.NET, typed DataSets and `.xsd` definitions, TableAdapters, DataAdapters, ORM mappings, inline SQL, dynamic SQL, and stored procedure calls.
- Parameters, `DBNull`, defaults, validation, concurrency checks, transactions, retries, audit writes, triggers, and partial failures where visible.
- Available SQL projects, schema scripts, migrations, reporting definitions, configuration, and API/service contracts.
- Local/network files, shares, registry, COM/Office automation, printing, scanners, serial devices, native libraries, reports, and third-party licensing dependencies when encountered.
- Authentication/authorization enforcement and the identity used for each downstream operation. A hidden button alone is evidence of a UI restriction.

### VB.NET semantics relevant to C# translation

- `Option Strict`, `Option Explicit`, `Option Infer`, and `Option Compare`, including project defaults and file overrides.
- Implicit conversions, late binding, `Object`/`Variant`-like usage, default properties, `ByRef`, and overload resolution.
- `Nothing`, empty strings, nullable values, `DBNull`, and comparison behavior.
- Numeric/date conversions, rounding, division, culture-sensitive parsing, and formatting.
- `On Error`, `Resume`, swallowed exceptions, implicit error recovery, and cleanup paths.

For each relevant semantic concern record the actual expression and settings, why translation could alter behavior, and a verification example. Use compiler/runtime evidence or authoritative language documentation when exact semantics are uncertain; otherwise mark the question unresolved.

## Evidence and record contracts

Use stable application-scoped IDs for findings, rules, dependencies, workflows, and open questions. Preserve IDs across updates. Evidence can be linked once and reused rather than repeating source excerpts throughout the report.

| Record | Required contents |
| --- | --- |
| Evidence | Repository/root ID; relative path; symbol or SQL object; line range where available; source revision/snapshot; concise explanation of what it supports |
| Finding/rule | Stable ID; plain-language behavior; trigger/condition; outcome; evidence; evidence status; unresolved qualification, if any |
| Workflow | Goal and actor; entry points; preconditions/inputs; ordered trace and branches; rules; reads/writes/integrations; failures; verification examples |
| Dependency | Type and purpose; caller; observed operation/contract; available implementation; unavailable downstream effects; evidence |
| Open question | Specific ambiguity; business or technical impact; conflicting/absent evidence; suggested verification or person/role to consult if known |
| Coverage row | Project/form/workflow; status; source reference; review depth; reason for deferral or blockage |

Evidence statuses:

- **Source-confirmed:** the cited implementation establishes the stated fact; runtime execution has not thereby been proven.
- **Runtime-observed:** a supplied or authorized observation establishes behavior for a named build/environment and scenario. Record provenance and conditions.
- **User-reported:** an identified source reports the behavior; source/runtime corroboration remains explicit.
- **Inferred:** a plausible interpretation with supporting evidence and the remaining uncertainty.
- **Unknown:** evidence is absent or unavailable; record what would resolve it.
- **Conflicting:** sources disagree; retain the competing evidence and question.

Use factual statements bounded by evidence. For negative findings, name the scope searched, such as “No authorization check found in the traced handler or its available callees.” Keep unavailable code visible. A database call does not establish the stored procedure's internals; a designer does not establish runtime visibility; method names do not establish business intent.

## Deliverables

Use the repository's established discovery layout when one exists. Otherwise:

| File | Contents |
| --- | --- |
| `README.md` | Business overview, source baseline, scope/mode, discovery status, coverage summary, links, and next investigations |
| `inventory.md` | Projects, forms/control families, startup/navigation, technology/dependency inventory, and coverage ledger |
| `workflows/<workflow-id>.md` | Detailed workflow trace, rule and dependency records, evidence, rebuild implications, and verification examples |
| `open-questions.md` | Consolidated unresolved and conflicting findings, with links to their authoritative records |

Keep rule and dependency details in one authoritative location; other files link to them. Introduce a shared register only when several workflows need to reference the same records. Create only files with substantive content; use a short explicit “none identified within scope” entry where a required category has no findings.

Include small Mermaid diagrams inline: an application/dependency view for overview mode and a focused flow or sequence view for the selected workflow when it improves comprehension. Label protocols and uncertain relationships only as supported by evidence. Use a diagram capability available in the environment; the skill must remain usable without a particular third-party visualization skill. Mark rendering unverified when no renderer is available.

## Suggested package structure

`vbnet-winforms-discovery/SKILL.md` — invocation, scope resolution, ordered procedure, universal evidence rules, completion, and pointers.

`vbnet-winforms-discovery/references/legacy-patterns.md` — conditional WinForms, data/integration, and VB semantics guidance.

`vbnet-winforms-discovery/references/report-contracts.md` — record definitions and minimal report templates.

`vbnet-winforms-discovery/references/evaluation-cases.md` — the acceptance scenarios below and how to judge a pilot.

Use the installation location supported by the target agent; `.github/skills/vbnet-winforms-discovery/` is the proposed team repository location. Keep a single authoritative copy of the skill. This brief does not require changes to existing `AGENTS.md`, Copilot instructions, or other skills.

## Acceptance scenarios for the authored skill

Evaluate on small fixtures or a representative application; report which scenarios were actually exercised. These cases judge discovery quality rather than requiring exact prose or a particular number of files.

1. **Save handler with inline SQL:** finds validation, SQL parameters, writes, error handling, and UI outcome; supplies evidence and synthetic verification cases.
2. **Inherited form with dynamic event wiring:** resolves available base/partial code and handler registration; marks unresolved dynamic reachability rather than silently omitting it.
3. **Typed DataSet calling an unavailable procedure:** traces binding and command metadata; reports the procedure boundary and unknown side effects without inventing SQL behavior.
4. **VB comparison/conversion edge case:** records actual settings and expressions; identifies a translation concern and a focused parity case without claiming conversion occurred.
5. **UI-only permission check plus Office automation:** distinguishes control visibility from downstream enforcement and identifies a desktop dependency requiring a new design.
6. **Large multi-project solution:** inventories scope, completes one declared trace, lists deferred work, and avoids claiming full application coverage.
7. **No build or runtime access:** produces useful static findings, explicit limitations, and developer verification steps; keeps execution and observed behavior distinguishable.
8. **Existing report after source changes:** validates the prior baseline, preserves record IDs, updates affected findings, and labels unchecked or stale evidence.
9. **Missing integration source or binary-only library:** records calls/contracts and unavailable implementation; carries the gap into affected branches and diagrams.
10. **Documentation conflicts with code:** reports both sources, distinguishes implemented behavior from reported intent, and frames the decision for a reviewer.

Across cases, verify that application files and databases are unchanged, sensitive values are excluded, material findings have evidence/status, questions are specific, and the coverage ledger reconciles with the report.

## Example requests for the finished skill

“Use vbnet-winforms-discovery to inventory this legacy VB.NET solution and trace one representative workflow. Save the findings under docs/discovery.”

“Use vbnet-winforms-discovery in workflow mode for the Save action in frmCustomerMaintenance. Follow its validation, database writes, permissions, and error paths.”

“Use vbnet-winforms-discovery to update the existing discovery after the changes to the billing forms. Identify which previous findings and verification examples are affected.”

## References consulted for this authoring brief

- Matt Pocock, `writing-for-agents`: https://github.com/mattpocock/skills/blob/main/skills/productivity/writing-for-agents/SKILL.md
- Companion invocation and packaging guidance: https://github.com/mattpocock/skills/blob/main/skills/productivity/writing-for-agents/SKILL-MECHANICS.md

This brief defines requested behavior and proposed defaults. It contains no findings about a particular legacy repository, and no discovery execution or skill evaluation has yet taken place.
