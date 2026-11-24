<div align="center">

# AgentRisk

<h3>One risk score per AI agent</h3>

[![Status](https://img.shields.io/badge/status-design%20stage-orange?style=flat-square)](ROADMAP.md)
[![Go 1.26+](https://img.shields.io/badge/go-1.26+-00ADD8?style=flat-square)](https://go.dev/dl/)
[![License](https://img.shields.io/badge/license-Apache%202.0-blue?style=flat-square)](LICENSE)

---

[Thesis](THESIS.md) · [Roadmap](ROADMAP.md) · [Contributing](CONTRIBUTING.md) · [Security](SECURITY.md)

</div>

---

## What is AgentRisk?

One risk score per AI agent, so a fleet can be ranked. `agentrisk` scores an
agent deployment on four factors (how much untrusted input reaches it, how
sensitive its tools are, how much it does without a human, and how much of
what it does can be undone) and explains the score in terms you can act on.

> **Status: design stage.** There is no code yet. The scoring model and output
> below are the proposal, and they will change. Part of the
> [OpenAstra](https://openastra.org) research agenda, area VII: AI Threat
> Modeling & Risk Assessment.

---

## The problem

Organizations now run dozens to hundreds of agents. Security teams can't
review them all with the same depth, and there's no common way to say which
ones to look at first. Vulnerability management solved the same problem with
scores like CVSS and EPSS; agents have nothing equivalent.

---

## The four factors

| Factor | Question | Examples of inputs |
|--------|----------|--------------------|
| **Exposure** | How much untrusted content reaches the agent? | Public web, inbound email, user uploads, other agents' outputs |
| **Capability** | How sensitive are the tools it can reach? | Read-only search vs. shell, payments, IAM, production deploys |
| **Autonomy** | How much does it do without a human? | Approval on every call, on sensitive calls only, or never |
| **Reversibility** | How much of what it does can be undone? | Draft vs. sent email, staged vs. applied change, refundable vs. final payment |

The score combines them so that one strong mitigation counts: an agent with
dangerous tools but approval on every sensitive call scores lower than the
same agent running unattended.

---

## Example (proposed)

```console
$ agentrisk score agents/
AGENT              SCORE  EXPOSURE  CAPABILITY  AUTONOMY  REVERSIBILITY
support-agent       8.7   high      critical    full      low
release-bot         6.1   low       critical    partial   medium
research-agent      3.2   high      low         full      high
docs-summarizer     1.4   medium    none        full      high

support-agent: biggest single reduction is an approval step on send_payment (8.7 -> 4.9)
```

---

## Planned surfaces

- CLI over a directory of agent configs, reusing [ThreatDiff](https://github.com/sadayamuthu/threatdiff)'s parsers
- JSON output with a full breakdown, for dashboards and inventories
- "What-if" mode: the score after a proposed mitigation
- A published, versioned scoring specification, so other tools can compute the same score

---

## Design principles

- **Explainable over precise.** Every score comes with the factor values and
  the single change that would lower it most.
- **Open model.** The weights and formula are published and versioned; no
  hidden tuning.
- **Calibrate in the open.** The scoring model will be checked against
  public incident reports and red-team results, and the method published.

---

## Non-goals

- Scoring model quality or bias.
- Replacing a threat model. The score tells you where to look; ThreatDiff
  tells you what you'll find.

---

## Roadmap

The plan, from the first working version to 1.0, is in [ROADMAP.md](ROADMAP.md). The reasoning behind the project, and what would make us stop, is in [THESIS.md](THESIS.md).

---

## Related OpenAstra projects

- [ThreatDiff](https://github.com/sadayamuthu/threatdiff): the capability graph the score is computed from
- [AgentRewind](https://github.com/sadayamuthu/agentrewind): provides the reversibility data
- [ExposureGate](https://github.com/sadayamuthu/exposuregate): the same "rank by real risk" idea, for dependencies

---

## Contributing

Design feedback, real-world cases and fixes are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for how to help while the project is at the design stage, and [GOVERNANCE.md](GOVERNANCE.md) for how decisions are made.

---

## Security

Please report vulnerabilities privately through [GitHub security advisories](https://github.com/sadayamuthu/agentrisk/security/advisories/new), not in a public issue. See [SECURITY.md](SECURITY.md) for what to include and what's in scope.

---

## License

[Apache License 2.0](LICENSE)

---

<div align="center">

**AgentRisk: one risk score per AI agent**

AgentRisk is managed by [OpenAstra](https://openastra.org)

[GitHub](https://github.com/sadayamuthu/agentrisk) · [Thesis](THESIS.md) · [Roadmap](ROADMAP.md)

</div>
