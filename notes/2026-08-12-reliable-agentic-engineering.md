# Notes on Reliable Agentic Engineering

*Published August 12, 2026*

> These are my personal reflections on designing dependable AI-assisted workflows. All examples have been generalized and are unrelated to any particular employer, product, or private system. The opinions are my own.

## The central lesson

Running an AI agent repeatedly does not, by itself, make the outcome dependable. What matters most is the structure surrounding that repetition:

- **A specification** establishes the objective, boundaries, and criteria for success before the work starts.
- **A verification harness** evaluates the result after every cycle through tests and other automated checks.

My current view is that constructing the loop is usually straightforward. The harder—and more valuable—work is writing a precise specification and building checks that can determine whether the result is actually acceptable.

## Three levels of agentic workflows

I currently group these workflows into three levels of maturity:

1. **An open-ended loop** keeps telling an agent to continue. It is simple to create, but it lacks a dependable way to know when to stop.
2. **A controlled loop** introduces a maximum number of attempts, measurable checks, and clearly defined success and failure outcomes.
3. **A workflow with distinct roles** divides responsibility among planning, implementation, review, and integration. The reviewing role can reject an inadequate result and return it for another attempt.

Introducing additional roles does not increase the model’s intelligence. The benefit comes from clearer responsibilities, independent validation, and more deliberate handling of failure.

## Give the agent the support a new teammate would need

One helpful analogy is to treat an agent as a new engineer who needs enough structure to work independently. That structure includes:

- a task with a realistic and manageable scope;
- tests that surface failures clearly;
- a review checkpoint before the work is accepted; and
- feedback that identifies the specific problem.

Guardrails do not improve the model’s underlying reasoning ability. They limit the space in which it operates and expose mistakes early, when correcting them is still relatively inexpensive.

## Clarify the work before asking for implementation

For work beyond a small change, I favor a specification-first sequence:

```text
initial idea → requirements → design → implementation plan → execution
```

The value of this process is not measured by the number or names of the documents it produces. Its purpose is to resolve important uncertainty before the agent reaches a point where it would otherwise have to make assumptions.

Whenever practical, acceptance criteria should be testable by a machine. Given-When-Then is one useful way to express them:

> Given a collection that spans multiple pages, when another page is requested, then the correct remaining entries are returned and the response accurately reports whether additional entries are available.

The criterion focuses on observable behavior while remaining independent of any particular organization, product, or data structure.

## Independent review improves reliability

The agent performing the implementation should not be the sole judge of whether the task is finished. An agent can give a persuasive account of success even when the required proof is incomplete.

For changes with a wider scope, an independent reviewer should look for concrete evidence, including:

- successful test results;
- passing static-analysis and type-checking results;
- satisfaction of any required coverage or quality thresholds; and
- verification of the agreed acceptance criteria.

Ideally, the reviewer examines the output from a fresh perspective instead of inheriting all of the implementer’s reasoning. That separation makes it easier to question unsupported claims and send incomplete work back for revision.

This extra review role is not necessary for every task. If a change is small enough for a person to inspect completely, human review may be sufficient. Review effort should be proportional to both the size of the change and the consequences of getting it wrong.

## Apply human judgment before changes become expensive

If the first meaningful human checkpoint comes after a large implementation is complete, review can become a bottleneck and a change in direction can be costly.

For appropriate tasks, I prefer this general flow:

```text
a person reviews and approves the plan
                 ↓
the agent performs the implementation
                 ↓
automated validation and independent review occur
                 ↓
a person remains accountable for the outcome
```

Early plan review gives a person the opportunity to correct the direction before substantial implementation begins. It does not replace final review—particularly for high-risk changes—but it can reduce the amount of uncertainty left for that final checkpoint.

## Select the right way to collaborate with AI

The best interaction style depends on the nature of the task:

- **Pairing:** A person follows and guides every step. This works well when requirements are unclear or mistakes would have significant consequences.
- **Repeated prompting:** A person reviews the same workstream and redirects it as needed. This provides flexibility, although repeated corrections can create avoidable rework.
- **A controlled agentic workflow:** A person defines the specification and validation up front, then allows the system to iterate within explicit boundaries.

The controlled workflow does not make the underlying model more capable. Instead, it shifts some ongoing supervision into up-front preparation and repeatable verification. This trade is worthwhile only when the task has clear boundaries and the validation process is credible.

## Common traits of more dependable loops

The patterns that seem most broadly reusable are:

1. **An objectively verifiable stopping condition.** The workflow needs observable proof that the requested work has been completed.
2. **A separate review mechanism.** A reviewer must be able to reject an unsupported completion claim and initiate another attempt.
3. **Clarification before execution.** Important ambiguities should be addressed before the system begins working without supervision.
4. **A legitimate failure path.** A safe system can stop after reaching its limit, report that it cannot complete the task, or decline to continue instead of manufacturing a successful outcome.

## Caveats and questions to investigate

I see these ideas as practical engineering guidelines rather than established laws. A run that reaches completion has not necessarily produced a correct result. Additional review also consumes time and resources, while automated checks can validate only the properties they were designed to measure.

The questions I want to examine next include:

- How should the amount of review vary according to risk and the size of the affected code surface?
- What kinds of acceptance criteria work best for ML and data systems whose outputs are probabilistic?
- How can teams evaluate actual correctness instead of treating successful execution as a substitute?
- At what point does the overhead of a multi-role workflow exceed the reliability it adds?

## A checklist for my next experiment

- [ ] State the boundaries of the task and what is explicitly out of scope.
- [ ] Define observable acceptance criteria before execution starts.
- [ ] Establish a limit on iterations or cost.
- [ ] Specify separate success and failure outcomes.
- [ ] Use independent review when a change is broad or cannot be inspected easily.
- [ ] Preserve human accountability for consequential decisions.
- [ ] Document unsuccessful as well as successful attempts without recording proprietary information.

My main takeaway is that autonomy becomes useful when it operates within explicit limits, is evaluated through objective evidence, and is allowed to conclude honestly that the work is not yet complete.
