# AgentRisk: thesis

**In one line:** four factors (exposure, capability, autonomy,
reversibility) are enough to rank a fleet of agents by risk, if the score is
open and explains itself.

## The problem

Organizations run dozens to hundreds of agents and can't review all of them
deeply. There is no shared way to decide which to look at first.
Vulnerability management solved the same triage problem with open scores
like CVSS and EPSS; agents have nothing comparable.

## The bet

1. **Four factors capture most of the variance.** How much untrusted input
   reaches an agent, how dangerous its tools are, how much it does without a
   human, and how much of it can be undone together explain which agents
   produce serious incidents.
2. **Mitigations should count multiplicatively.** One strong control (an
   approval step on the dangerous tool, a reversible action) should lower the
   score a lot, which matches how incidents are actually prevented.
3. **Explainability drives adoption.** A score that names the single change
   that lowers it most gets acted on; an opaque number gets argued with.
4. **An open, versioned spec wins over a better closed one**, because
   inventories, dashboards and vendors can all compute the same number.

## Why now

Agent fleets are growing faster than security teams, and the inputs for
scoring now exist: capability graphs (ThreatDiff), reversibility data
(Rewind), and public incident and red-team reports to calibrate against.

## What success looks like

- Rankings agree with experienced reviewers on which agents to examine first.
- The calibration study against public incidents is published with its
  method and data.
- The spec is used by at least one tool outside OpenAstra.

## What would prove us wrong

- Rankings don't match expert judgement or incident history, even after
  calibration.
- Factor values can't be derived automatically, and manual entry kills
  adoption.
- The score is easy to game (configs that look safe without being safe).

## Not a bet on

Scoring model quality or bias, or replacing a threat model: the score says
where to look, ThreatDiff says what you'll find.

## Where it fits

Area VII, AI Threat Modeling & Risk Assessment. Built on
[ThreatDiff](https://github.com/sadayamuthu/threatdiff)'s graph and
[Rewind](https://github.com/sadayamuthu/rewind)'s reversibility catalog.
