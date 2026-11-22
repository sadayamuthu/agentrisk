# Security Policy

## Reporting a vulnerability

Please don't report security problems in a public issue, pull request or
discussion. Use
[**Report a vulnerability**](https://github.com/sadayamuthu/agentrisk/security/advisories/new)
on the repository's Security tab instead. Only the maintainers can see the
report.

Include what you can of:

- the AgentRisk version (`agentrisk version`) or commit, your Go version and OS
- how AgentRisk was used, and its configuration with secrets removed
- steps to reproduce
- what happened and what you expected

You'll get a reply when the report has been read, and again when a fix is
ready. A fix ships as a patch release; the advisory names you unless you'd
rather it didn't.

## Supported versions

AgentRisk is at the design stage and has no releases yet. Once it does,
fixes land in the latest release only, until 1.0.

## What counts

AgentRisk reads agent configuration files and writes scores. It does not run agents or call models.

AgentRisk is a security tool, so anything that breaks one of these promises
is a security issue:

- **Read-only and offline.** AgentRisk reads the files you point it at and makes no network calls. A path that writes elsewhere or sends data is in scope.
- **The published formula is the one used.** A score that differs from what the versioned specification gives for the same inputs is in scope.

These are by design, and not vulnerabilities:

- A low score is not a security guarantee. It says where to look first, not that an agent is safe.
- Scores are only as good as the configuration they're computed from.

A wrong result that isn't covered above is a bug, not a vulnerability. Report
it in a normal issue; see [CONTRIBUTING.md](CONTRIBUTING.md).
