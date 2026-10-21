# Atlassian Migration Toolkit

**Status: planned, pre-implementation.** This repository describes a proposed toolkit. It does not yet provide a supported migration tool or a released CLI.

The goal is to make Jira and Confluence migrations easier to prepare, review and verify. The first MVP will turn approved inventories and migration-assistant findings into a consistent readiness report, with clear gaps, owners and evidence.

The toolkit will complement the Jira Cloud Migration Assistant (JCMA) and Confluence Cloud Migration Assistant (CCMA). Atlassian's assistants remain responsible for the supported data transfer. This project does not claim Atlassian partner status or endorsement.

## Planned first MVP

- Import synthetic fixtures or explicitly approved inventory exports.
- Record the migration scope, source and destination, apps, integrations and mapping decisions.
- Report each supported check as **PASS**, **FAIL** or **UNKNOWN**, with the reason and supporting evidence.
- Produce local machine-readable and HTML reports, including unresolved decisions and the work that was not checked.

The first version will be an offline readiness and reporting tool. It will not collect credentials, connect to production, change users or permissions, or start a migration.

## Planned next steps

1. **Readiness and reporting:** define the input contract, use synthetic examples and implement deterministic checks.
2. **Inventory and validation:** add separately approved read-only collectors and source-to-target comparisons after proving the report model.
3. **Assistant integration:** evaluate official APIs for pre-checks, progress and evidence. Any later migration execution will require an explicit, separately approved scope and a successful rehearsal.

These are roadmap items, not available features. See [the MVP scope and acceptance criteria](docs/mvp.md).

## Why build around the assistants?

Atlassian already exposes APIs for migration definitions, pre-checks, execution and progress. The useful addition is a consistent view of customer-specific decisions, coverage and validation across the process. [Official Migration API guide](https://developer.atlassian.com/platform/migration-api/guides/migrate-jira-or-confluence-to-cloud-using-api/)

The two assistants have different evidence capabilities. Atlassian documents downloadable pre- and post-migration API reports for JCMA, with no equivalent CCMA report support through that API. The toolkit must represent those differences rather than assume parity. [Official advanced features guide](https://developer.atlassian.com/platform/migration-api/guides/advanced-features/)

## Current limits

- No migration execution, production integrations or completed validation capability is claimed.
- A readiness report cannot prove that every app, script, integration or permission will behave identically in Cloud.
- Missing evidence must remain visible; it must not become a successful check.
- There is no automatic remediation, permission modification, failed-check bypass or automatic rollback.
- Examples and public reports must use synthetic data. Do not contribute credentials, customer exports, personal information or private project evidence.

## Repository security checks

The [security workflow](.github/workflows/security.yml) scans the working tree and fetched Git history with Gitleaks, and runs a Trivy filesystem vulnerability/configuration scan. Scanner images are pinned by digest; checkout and artifact actions are pinned by commit. Reports include the checked-out commit and are retained as workflow artifacts for seven days.

This is currently a documentation repository. Application SAST, unit/coverage, integration, runtime and DAST checks are **not applicable**, not passed. There are no application dependency manifests or deployable infrastructure targets yet; an empty Trivy inventory does not prove application security. The workflow also stops when common executable source, dependency or container files appear without a corresponding update to the gates.

Before adding the first implementation, choose and pin its runtime/dependencies, configure appropriate SAST and lint/build checks, and add unit tests with at least 70% coverage plus relevant integration and security tests. Add container scanning when an image exists and DAST when a staging target exists. The current file-pattern guard is a reminder to configure those gates, not a universal programming-language detector.

## Reference documentation

- [What JCMA migrates](https://support.atlassian.com/migration/docs/what-gets-migrated-with-the-jira-cloud-migration-assistant/)
- [What CCMA migrates](https://support.atlassian.com/migration/docs/what-migrates-with-the-confluence-cloud-migration-assistant/)
- [Jira user and group migration behavior](https://support.atlassian.com/migration/docs/migrate-users-and-groups/)

Product and API behavior must be checked against the versions used for each migration. The documentation above was reviewed on 21 October 2026.
