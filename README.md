# agentrisk

One risk score per AI agent, so a fleet can be ranked. `agentrisk` scores an
agent deployment on four factors (how much untrusted input reaches it, how
sensitive its tools are, how much it does without a human, and how much of
what it does can be undone) and explains the score in terms you can act on.

> **Status: design stage.** There is no code yet. The scoring model and output
> below are the proposal, and they will change. Part of the
> [OpenAstra](https://openastra.org) research agenda, area VII: AI Threat
> Modeling & Risk Assessment.

## The problem

Organizations now run dozens to hundreds of agents. Security teams can't
review them all with the same depth, and there's no common way to say which
ones to look at first. Vulnerability management solved the same problem with
scores like CVSS and EPSS; agents have nothing equivalent.

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

## Planned surfaces

- CLI over a directory of agent configs, reusing [ThreatDiff](https://github.com/sadayamuthu/threatdiff)'s parsers
- JSON output with a full breakdown, for dashboards and inventories
- "What-if" mode: the score after a proposed mitigation
- A published, versioned scoring specification, so other tools can compute the same score

## Design principles

- **Explainable over precise.** Every score comes with the factor values and
  the single change that would lower it most.
- **Open model.** The weights and formula are published and versioned; no
  hidden tuning.
- **Calibrate in the open.** The scoring model will be checked against
  public incident reports and red-team results, and the method published.

## Non-goals

- Scoring model quality or bias.
- Replacing a threat model. The score tells you where to look; ThreatDiff
  tells you what you'll find.

## Roadmap

1. Factor definitions and a draft scoring specification
2. Score from ThreatDiff's capability graph
3. What-if mode and mitigation suggestions
4. Calibration study and specification v1

## Related OpenAstra projects

- [ThreatDiff](https://github.com/sadayamuthu/threatdiff): the capability graph the score is computed from
- [Rewind](https://github.com/sadayamuthu/rewind): provides the reversibility data
- [ExposureGate](https://github.com/sadayamuthu/exposuregate): the same "rank by real risk" idea, for dependencies

## License

To be decided before the first release.
