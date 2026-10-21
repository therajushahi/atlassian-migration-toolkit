# Migration readiness and evidence MVP

**Status: proposed, pre-implementation.** The criteria below define intended work. They are not passed checks or a statement of released functionality.

## Goal

Produce a useful, repeatable readiness review from approved Jira and Confluence inventories. A reviewer should be able to see the proposed scope, which evidence exists, what remains uncertain and who must make each decision.

The first MVP is an offline reporting tool. It will not transfer product data or replace JCMA or CCMA.

## First supported workflow

1. Load a migration manifest and synthetic or explicitly approved inventory files.
2. Validate the input format and record the supplied evidence and its origin.
3. Apply a small, versioned set of readiness rules.
4. Render local JSON and HTML reports.
5. Have a person review the blockers, unknowns, exclusions and decisions before planning a rehearsal.

The initial input contract should stay small. Its implementation and schema version are still to be defined:

| Input | Intended information |
| --- | --- |
| Migration manifest | Engagement-local identifier, declared product and version, source and destination identifiers, selected projects or spaces, scope exclusions and capture time. |
| Inventory | Supplied project or space identifiers, relevant counts, installed apps, declared integrations and evidence references. |
| Mapping and decisions | Source-to-target mapping proposals, app strategy, identity/group decisions, owner, decision state and rationale. |
| Assistant findings | Explicitly imported findings with their product, assistant version, collection time and evidence reference. Missing reports remain missing. |

Public examples must contain invented identifiers and synthetic records. Real exports belong in an approved private location, outside this repository.

## Initial checks

- Required manifest fields are present and the declared product is supported by the implemented schema.
- Every selected project or space has an inventory record or an explicit coverage gap.
- Every supplied app has an owner and an explicit strategy: use its supported migration route, replace it, exclude it with approval, or investigate it.
- Every declared integration has a decision owner and a validation task.
- Proposed source-to-target mappings do not contain unexplained duplicate targets or unmapped in-scope entities.
- Supplied identity/group collision findings have an owner and remain unresolved until reviewed. This is a decision register, not proof of effective permissions.
- Imported assistant failures and unknowns remain visible in the output.

The MVP must not manufacture app compatibility or permission findings when the required inventory is absent.

## Result semantics

| Result | Meaning |
| --- | --- |
| PASS | The specific implemented rule was evaluated against sufficient supplied evidence and met its declared condition. |
| FAIL | The evidence establishes that the rule's condition was not met. |
| UNKNOWN | Evidence is missing, unsupported, unreadable or insufficient to evaluate the rule. |

Each result needs a stable rule identifier, rule version, evidence references, explanation and suggested next action. Decisions also need an owner. Reports must identify unsupported checks and scope exclusions separately.

An all-PASS report means only that the evaluated rules passed. It does not establish full migration coverage, customer acceptance or permission equivalence.

## MVP acceptance criteria

All of these must be demonstrated before describing the first MVP as usable:

1. A documented, versioned input schema accepts synthetic Jira and Confluence examples and rejects malformed inputs with actionable errors.
2. Input files are not modified, and the default workflow makes no network requests or production changes.
3. Each initial rule has a synthetic passing, failing and missing-evidence example where applicable. Missing evidence cannot produce PASS.
4. The report records product/version declarations, input provenance, input digests, rule versions, capture times, coverage and exclusions.
5. Equivalent inputs produce equivalent findings and stable result ordering; report-generation timestamps are kept separate from those findings.
6. Both JSON and HTML show blockers, unknowns and decision owners. HTML safely renders supplied text as data, including hostile markup examples.
7. The app/integration decision register does not infer compatibility or treat an unreviewed mapping as approved.
8. A reviewer can trace a result back to supplied evidence and can distinguish an imported assistant finding from a toolkit check.
9. Documentation includes a synthetic walkthrough, limitations and an example of a report that remains incomplete because evidence is missing.
10. Repository examples and generated public demonstration reports contain no credentials, customer records or personal information.

No criterion is marked complete at this stage. Runtime, commands, storage format and release packaging remain implementation decisions.

## Later phases

### Phase 2: approved inventory collection and validation

Add narrowly scoped, read-only collectors only after confirming supported endpoints and permissions for the relevant Jira or Confluence versions. Record collection failures and pagination gaps explicitly.

Planned comparisons include source-to-target entity mapping, selected field values, counts, attachments, relationships and representative permissions. A counts-only comparison is insufficient. Content transformations and Cloud behavior differences need explicit comparison rules and human acceptance tests.

Identity checks deserve particular attention: Atlassian documents that group matching and re-migration can retain or expand access. The tool should flag potential expansion for review rather than rename groups or change membership. [Jira users and groups](https://support.atlassian.com/migration/docs/migrate-users-and-groups/)

App data and Confluence macros may depend on vendor support. Global Confluence permissions/settings also need separate handling. Unsupported items become explicit work, not an automatic success. [JCMA coverage](https://support.atlassian.com/migration/docs/what-gets-migrated-with-the-jira-cloud-migration-assistant/), [CCMA coverage](https://support.atlassian.com/migration/docs/what-migrates-with-the-confluence-cloud-migration-assistant/)

### Phase 3: official-assistant integration

Evaluate creating pre-check jobs through Atlassian's public Migration API, then collecting task status and available artifacts. Creating job/task records is a control-plane write even when the selected pre-check does not move product data. Keep it separate from read-only inventory collection.

Official job definitions are immutable, destination attachment expires and progress snapshots are point-in-time. Integration design must account for those properties and preserve the evidence needed for the engagement. [Migration API workflow](https://developer.atlassian.com/platform/migration-api/guides/migrate-jira-or-confluence-to-cloud-using-api/)

Do not assume report parity: the documented pre/post-migration API reports are JCMA-only; CCMA needs an alternative evidence path. Treat artifact availability and assistant versions as capabilities to verify. Never automatically exclude or override a failed assistant check. [Advanced features and artifacts](https://developer.atlassian.com/platform/migration-api/guides/advanced-features/)

Production execution, if pursued later, needs a separately approved scope, target identity, source snapshot, change window, successful rehearsal, evidence review and explicit operator authorization. The toolkit must not promise that a Cloud migration can be automatically reversed.

## Boundaries throughout development

- Keep collection, proposed changes and execution as distinct operations.
- Use the least privilege available for each operation and keep secrets out of inputs, reports and logs.
- Store real engagement evidence only in its agreed environment with an agreed retention policy.
- Respect the relevant APIs' limits and handle partial results without silently dropping them.
- Do not automatically delete or rewrite content, grant access, alter groups, accept warnings or run production migrations.
- Treat app conversion, custom-script porting, SSO/identity changes and recovery procedures as separately scoped work until specifically implemented and verified.

These boundaries define the intended product behavior. They do not claim that an enforcement mechanism has already been built.
