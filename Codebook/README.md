# Codebook

This codebook defines the categories and decision rules used to code the corpus of *Proactive, Robust Security at Scale: A Systematization of Intent-Driven Security Policy Automation*. Category names match the paper and its tables.

| File | Content |
|---|---|
| `README.md` | Coding rules, stage boundaries, categories, and table notation |
| `codebook.csv` | One row per category value, in machine-readable form |

Example studies are BibTeX keys from the paper's survey section. Corpus counts are in the paper's aggregate tables and are not repeated here. The corpus was not independently double-coded, and no inter-coder agreement measure is available.

## General coding rules

1. **Code the reported function.** Assign categories from what the source reports the system doing, not from its terminology. A paper's own labels, including words such as "assurance" or "closed-loop", are not evidence for a category.
2. **Tool names are not evidence.** A capability normally associated with a named platform or tool is not coded unless the source states it.
3. **Design-level mechanisms.** A mechanism described but not implemented is coded if the source makes a clear choice and defines the relevant axes. Otherwise the stage or axis is not coded.
4. **Related publications.** Successive papers from one group that describe the same pipeline are coded consistently. Different realizations of a similar architecture are coded separately.
5. **Secondary candidates.** Where a value is single-choice, a serious alternative is recorded in the coding rationale and not counted.

## Two coding schemes

The four stages use two schemes by design.

| Stages | Scheme | Coding |
|---|---|---|
| Intent Capture, Intent Translation | Mechanism families | One dominant family per study, plus one dominant artifact: the IR for Capture and the output form for Translation |
| Policy Enforcement, Policy Assurance | Independent facets | Values on separate axes that may co-occur. No family is assigned, so distribution claims are made per axis |

## Stage applicability and missing values

- A study that defines no mechanism for a stage is omitted from that stage's table. Each stage reports its own denominator, and denominators need not match across stages.
- **Absent and not reported are different.** An empty Operational Integrity cell means the system as described has no such safeguard. An empty Resolution Routing group means the system detects deviations without routing them (open loop). Both are findings. A value the source does not allow us to determine is marked separately (see Notation).

## Stage boundaries

| Stage | Begins with | Ends with |
|---|---|---|
| Intent Capture | A human-originated objective | The artifact handed to Translation: an IR or direct text |
| Intent Translation | An IR or direct text | A finalized policy artifact ready for a target enforcement point |
| Policy Enforcement | A finalized policy artifact | The artifact applied and, where supported, maintained on the target |
| Policy Assurance | A policy that has become operational | A conformance finding, an optional diagnosis, and, where supported, a resolution route |

**Capture and Translation.** Structuring what the user supplied is Capture, including an interactive dialogue that collects missing parameters. An artifact produced using topology, inventory, resource, knowledge-base, or live platform data is a Translation output, not an IR. An artifact the target can consume as it is, with no further contextual binding, is also a Translation output.

**Machine-originated goals.** A machine-originated goal without a human input channel receives no Capture mechanism. Its downstream stages may still be coded.

**Where a check belongs.** A check is assigned by what it evaluates and when it operates.

| The check evaluates | Timing | Stage | Coded as |
|---|---|---|---|
| A candidate policy, and can accept, revise, or reject it | During artifact production | Intent Translation | Verifier, if the Hybrid conditions hold |
| A finalized artifact, deciding whether it may be activated on a particular target | Before activation | Policy Enforcement | Pre-deployment gate |
| Observed operational state | After activation | Policy Assurance | Scope and mechanism values |

- *Pre-deployment validation* is an inter-stage label for checks before activation, not a category. *Runtime assurance* evaluates observed operational state.
- A gate remains in Enforcement even if rejection later triggers a Translation pass.
- If the same apparatus, such as a digital twin, performs two of these functions, the functions are recorded separately. This is not double counting.
- Assurance records observation, detection, diagnosis, and routing. Rollback or re-actuation is executed through Enforcement, and regeneration through Translation.
- Deployment only to an emulator is not coded as Enforcement.

## Corpus strata

Studies marked `§` belong to the security-oriented stratum: the system turns a human-expressed security requirement into an enforceable security policy. The stratum affects which table block a study appears in, not whether it is included.

- **Privacy and compliance:** included when the generated artifact is an access or confidentiality control, and excluded when privacy is only a property of the architecture.
- **What is enforced:** a policy governing network communication is included, such as firewall rules, ACLs, Kubernetes NetworkPolicy, service-mesh authorization, or cloud security groups. Application-level RBAC, database permissions, and cloud IAM roles are excluded.
- **Security of the IBN system itself:** attacks on, or the integrity of, the intent pipeline do not place a study in this stratum.
- **Security as one use case:** a multi-purpose framework for which security is one evaluation scenario stays in general IBN.

## Intent Capture

Each study receives one dominant family, based on its principal user-facing input channel, and one dominant IR. Sub-mechanisms are recorded only when they operate during Capture. They are mutually exclusive except within LLMs, where several may co-occur.

### Capture family

| Value | Definition | Decision rule | Examples |
|---|---|---|---|
| Structured Interfaces | Users express intents through a predefined format, such as a GUI form or schema-conformant JSON or YAML submitted through an API. Mapping to the IR is deterministic. | A schema-conformant document delivered directly by file or API belongs here, not under Formal Language. |  |
| Formal Language | Users write intents directly in a language with formal syntax and a restricted vocabulary. Input is checked for well-formedness under that language. | Requires direct authoring in at least one first-class input channel. If the formal language exists only as Capture output, code the family by the input channel and record the language as the IR. |  |
| Classical NLP | A linguistic pipeline processes free natural language without a pretrained generative model. Encoder-only models belong here. | A paper calling its encoder-only model an LLM does not change the family. |  |
| LLMs | A pretrained generative language model, including a small generative model, converts free natural language into the IR. | Model size is not a criterion. A generative model used only to produce training data does not make the family LLMs. |  |
| Not specified | The family cannot be determined from the source. |  |  |

### Capture sub-mechanism

| Group | Value | Definition | Decision rule | Examples |
|---|---|---|---|---|
| Structured Interfaces | Rules-based Mapping | Fixed rules or templates map fields or document elements to IR elements. |  | `lessi20245g`, `gu2024intent` |
| Structured Interfaces | Hierarchical Goal Decomposition | A predefined hierarchy refines a high-level goal into sub-goals or required capabilities. |  | `wang2024autonomous` |
| Formal Language | Policy Language Specification | A dedicated policy language expresses subject-action-object relations or permissions without its own inference semantics. | Layered abstraction levels do not change the sub-mechanism. | `murcia2025bastion`, `basile2026formal` |
| Formal Language | Logic-based Specification | Statements such as Prolog facts or temporal-logic formulas are passed directly to a reasoner or solver. | The defining test is that an inference engine or solver consumes the semantics of the statements. No corpus study uses this value. |  |
| Formal Language | Query and Dataflow Specification | Declarative queries or operator compositions over network state use a closed operator algebra and are compiled. | No corpus study uses this value. |  |
| Formal Language | Controlled Natural Language Specification | Natural-language-like input follows a published grammar; input that violates it is rejected during parsing. | Mandatory published grammar with rejection at language level. Free text processed by hand-written rules is Rule-based Extraction. | `settanni2024dynamic`, `bensalem2021role` |
| Classical NLP | Structured Sequence Labelling | A task-trained sequence model labels each token, and the resulting slots form the IR. |  | `jacobs2025establishing`, `mehmood2025knowledge` |
| Classical NLP | Encoder-based Extraction | A pretrained encoder, such as BERT, classifies the request or extracts its elements. |  | `njah2023toward`, `lyu2026intentbased` |
| Classical NLP | Joint Intent Modeling | One model jointly predicts the intent class and its slots. |  | `detrizio2024novel`, `asif2025leveraging` |
| Classical NLP | Rule-based Extraction | Hand-written rules extract intent elements from free text; unlike controlled natural language, the input need not follow a grammar. | Requires free natural-language input, hand-written rules with no learned model, and deterministic output. | `fuad2024intent`, `orozco2025intent` |
| LLMs | Prompt-based Inference | Instructions and examples are supplied in the prompt without updating model weights. | The test is weight updating. Examples placed only in the prompt stay here even if the paper says few-shot or in-context learning. | `mekrache2024management`, `wang2024netconfeval` |
| LLMs | Domain-Specific Fine-tuning | Model weights are updated on domain data, for example through supervised fine-tuning or LoRA. | Mark only when weights are updated. | `asif2025evaluating`, `han2025network` |
| LLMs | RAG | Similar earlier request-output pairs are retrieved and added to the prompt as examples. | Mark only for retrieval on the Capture path. Retrieval of a system capability catalog, deterministic selection from a small fixed template set, and embedding a static knowledge file do not count. | `mekrache2024llm`, `natarajan2025rag` |
| LLMs | Agentic Refinement | An evaluate-and-correct cycle revises the Capture artifact before it is handed to Translation. | A loop that changes the artifact counts, including a clarification question in the described running system. A single accept-or-reject approval does not count, nor does a loop triggered by deployed state or a training-time feedback loop. | `zheng2025intent`, `wang2025confucius` |

### IR family

The IR family describes the structure of the artifact handed to Translation, independently of the Capture family that produced it.

| Value | Definition | Decision rule | Examples |
|---|---|---|---|
| Schema-based | Structured serialization, such as JSON, YANG, or standardized intent models. | A constraint embedded in a reward or scoring function, not passed to a solver, stays here. | `lessi20245g`, `murcia2025bastion` |
| Graph-based | Nodes, edges, and typed relations that support graph reasoning. |  | `kou2025gia`, `ooi2023intent` |
| Programmatic | A logical structure that can be analyzed and compiled. | Consumed by compilation or lowering. An artifact loaded directly by a policy engine is a Translation output, not a Programmatic IR. | `martini2023intent`, `detrizio2024novel` |
| Constraint-based | Rules or constraints passed directly to a solver or reasoner. | The consumer must be a solver or reasoner. | `ojaghi2026intentbased`, `sun2026neurosymbolic` |
| Direct-text/Unstructured | The natural-language request reaches Translation without a formal IR. | Also applies when the request and network context enter one model together, since no IR holding only user information exists. | `mekrache2024management`, `dzeparoska2023policy` |
| Not specified | The IR cannot be determined from the source. |  |  |

## Intent Translation

Each Translation-applicable study receives one dominant family and one output form. Static, Constraint-based, and Probabilistic studies receive one sub-mechanism. Hybrid studies are characterized by their verifier's paradigm.

Six cross-cutting responsibilities describe functions within Translation: binding, parameter synthesis, decomposition, translation fidelity, translation provenance, and conflict management. They are an analytical lens and are not coded per study.

### Translation family

| Value | Definition | Decision rule | Examples |
|---|---|---|---|
| Static Rule-based Mapping | Hand-built rules, templates, or formal-language compilation convert the IR into policy. | In a multi-stage pipeline, the dominant family is the component that closes the semantic gap. |  |
| Constraint-based Synthesis | The IR defines goals and constraints; algorithmic search finds a policy that satisfies them. | Search or inference over a formal model belongs here. Learned generalization is Probabilistic Policy Generation. |  |
| Probabilistic Policy Generation | A learned or generative model produces the policy without a distinct component controlling the final output. | A format or syntax check alone does not change the family. If blocking authority is unclear, stay here. |  |
| Hybrid Intent Translation | A generative proposer produces candidates. A distinct verifier grounded in topology, inventory, feasibility, or similar context can accept, repair, constrain, or reject them, thereby controlling the deployable artifact. | A repair loop is optional and determinism is not required. Passive warnings, syntax or schema checks, unit tests, and human approval alone do not qualify as a verifier. |  |
| Not specified | The current coding does not identify a family. |  |  |

### Translation sub-mechanism

| Group | Value | Definition | Decision rule | Examples |
|---|---|---|---|---|
| Static Rule-based Mapping | Template- or Blueprint-based Compilation | IR parameters fill predefined templates or blueprints that produce policy. |  | `antonakoglou2025camino`, `zheng2025intent` |
| Static Rule-based Mapping | Rule Engine-based Mapping | A rule engine matches IR elements to condition-action rules and emits policy elements. |  | `vantu2024towards`, `gu2024intent` |
| Static Rule-based Mapping | State Machine-based Compilation | Fixed states or compiler stages transform the IR under a predefined state machine or language compiler. |  | `jacobs2025establishing`, `lingga2024spt` |
| Static Rule-based Mapping | Model-driven Transformation | Meta-model transformations map the IR model to a policy model. |  | `brodimas2024towards`, `basile2026formal` |
| Constraint-based Synthesis | Graph-based Reasoning | Search over topology or intent-relation graphs derives the policy. |  | `ooi2023intent`, `hussein2025privacy` |
| Constraint-based Synthesis | Planning-based Approaches | A planner finds actions that move the system from its current state to the goal state. | No corpus study uses this value. |  |
| Constraint-based Synthesis | General Constraint Solving | An SMT, ASP, or similar solver returns an assignment satisfying the encoded requirements. |  | `murcia2025bastion`, `parashar2026aspbased` |
| Constraint-based Synthesis | Multi-objective Optimization | One or more objective functions are optimized under the constraints to select among feasible solutions. |  | `filinis2024intent`, `zhou2026intentbased` |
| Probabilistic Policy Generation | Intent Classification | A learned model selects from predefined policies or configurations. |  | `mehmood2025knowledge` |
| Probabilistic Policy Generation | Neural Intent Translation | A task-trained neural model maps the IR to policy parameters or structures. |  | `wu2025explainable` |
| Probabilistic Policy Generation | Generative Intent Translation | A pretrained generative model, typically an LLM, generates the policy artifact. |  | `wang2024netconfeval`, `han2025network` |
| Hybrid Intent Translation | Static-aligned | The verifier uses rules or compilation. | Hybrid studies are characterized by the paradigm of their verifier rather than by a sub-mechanism. | `wang2025confucius`, `samdaliri2025pace` |
| Hybrid Intent Translation | Constraint-aligned | The verifier uses a solver. |  | `kou2025gia` |
| Hybrid Intent Translation | Judgment-aligned | A judgment-based component, such as a context-grounded model, acts as verifier. | The verifier needs context-grounded judgment and blocking authority over the deployable artifact. | `li2026multiagentic` |

### Output form

| Value | Definition | Decision rule | Examples |
|---|---|---|---|
| Orchestration specification | States the target state without a procedure; realization is needed before Enforcement. | Output form follows how the downstream target consumes the final binding artifact, not how the artifact was produced. | `kou2025gia`, `filinis2024intent` |
| Enforceable policy | Loads directly onto a device, controller, or policy engine without further realization, as with ACLs, flow rules, or device configurations. | Declarative artifacts consumed by a reconciler, such as Kubernetes manifests, belong here. | `antonakoglou2025camino`, `murcia2025bastion` |
| Executable logic | A program, script, or workflow executed by a runtime. |  | `zheng2025intent`, `dzeparoska2023policy` |
| Invocation payload | A structured message to an existing service interface, using that interface's schema. | The schema source decides. A schema defined by the receiving interface gives Invocation payload. A schema defined by an independent standard gives Orchestration specification or Enforceable policy according to content. | `mekrache2025oss`, `feng2025secllm` |
| Not specified | The source does not report an output form. |  |  |

## Policy Enforcement

Each study receives one value for granularity, control structure, and each deployment-strategy facet. Operational Integrity safeguards may co-occur, and a study may carry none.

### Implementation Granularity

| Value | Definition | Decision rule | Examples |
|---|---|---|---|
| Device-Level Configuration | Device or flow settings, such as ACLs, flow rules, port settings, or CLI commands. | The test is the work the actuation performs, not the artifact type. Record a secondary level in the rationale. | `jacobs2025establishing`, `kim2025kubeteus` |
| Resource-Level Allocation | Capacity or resource allocation, usually dependent on topology and runtime state. | The test is the work the actuation performs, not the artifact type. Record a secondary level in the rationale. | `filinis2024intent`, `iorio2023computing` |
| Service-Level Orchestration | Deployment and coordination of VNFs, service chains, slices, or multi-domain services. | The test is the work the actuation performs, not the artifact type. Record a secondary level in the rationale. | `abbas2024ai`, `murcia2025bastion` |

### Deployment Strategy

**Propagation**

| Value | Definition | Decision rule | Examples |
|---|---|---|---|
| Push | An external component initiates delivery. | Fails the drift test described under Reconciliation. | `brodimas2025intent` |
| Pull | The enforcement point requests an artifact or decision as needed. | The enforcement point itself must fetch the artifact. | `bhatia2026zerotouch` |
| Reconciliation | A co-located component repeatedly compares desired and observed state and applies idempotent corrections, reversing manual changes without a new decision. | Drift test: if the applied state is changed externally, does it return without a new decision from the initiating component? The source's own mechanism description must pass the test; naming a tool such as ArgoCD is not enough. Implies Continuous. | `antonakoglou2025camino`, `iorio2023computing` |

**Staging**

| Value | Definition | Decision rule | Examples |
|---|---|---|---|
| Single-step | Applies a deployment at once. | Concerns the inside of one deployment, not whether deployment repeats. | `jacobs2025establishing` |
| Staged | Orders and progressively commits interdependent parts. |  | `zheng2025intent` |

**Continuity**

| Value | Definition | Decision rule | Examples |
|---|---|---|---|
| One-time | Deployment ends on completion. |  | `lessi20245g` |
| Continuous | Deployment repeats to maintain target state. | Reconciliation implies Continuous, but not the reverse. Re-actuation triggered by a closed loop is recorded here. | `filinis2024intent` |

### Control Structure

These are architectural descriptions. A control structure does not by itself demonstrate robustness, scalability, consistency, or safe deployment.

| Value | Definition | Decision rule | Examples |
|---|---|---|---|
| Centralized | One controller with global visibility decides. | A lower layer that only relays or translates does not make control hierarchical. Multi-agent designs that split functions without delegating authority stay here. | `dzeparoska2024intent`, `lingga2024spt` |
| Distributed | Enforcement points or local agents decide from local state. |  | `braghin2025mimo`, `samdaliri2025pace` |
| Hierarchical | Authority is layered and delegated across levels or domains. | The lower layer must make enforcement decisions within its own scope. | `antonakoglou2025camino`, `abbas2024ai` |

### Operational Integrity

| Value | Definition | Decision rule | Examples |
|---|---|---|---|
| Pre-deployment gate | A check in the actuation path decides whether a finalized artifact may be activated on a target. | Checks that revise or regenerate the artifact belong to Translation. A human approval step counts; an operator decision that only starts the process does not. In a staged rollout, an intermediate check counts only if failing it stops or rejects the deployment. If unclear, do not mark. | `antonakoglou2025camino`, `jacobs2025establishing` |
| Mid-deployment durability | State is preserved during execution through checkpointed resumption, atomic commit, idempotent application, or partial rollback. | Rebuilding from scratch does not count. Do not infer from a tool's known capabilities; the source must state it. Resilience of the deployed service, such as failover, is not deployment durability. | `zheng2025intent`, `kim2025kubeteus` |
| Post-deployment handover | Resulting state or metadata is passed on after execution, for example through telemetry, monitoring hooks, digital-twin updates, or orchestrator feedback. | The object handed over must be identifiable. Building monitoring because the intent asks for it is not enough. | `vilalta2023applying`, `murcia2025bastion` |

## Policy Assurance

Assurance applies only after a policy is operational. Each study receives one dominant scope. Mechanisms and resolution routes may co-occur.

**Reference definitions, not tallied in the paper:**

- A complete corrective loop consists of detection, diagnosis, decision, rollback or remediation, and confirmation.
- A system is *self-healing* only when it automatically decides on and executes a corrective action and subsequently confirms restored conformance. Detection alone is open-loop assurance. Human escalation is a resolution route but not automated self-healing.

### Assurance Scope

| Value | Definition | Decision rule | Examples |
|---|---|---|---|
| Configuration/State Conformance | Whether deployed configuration or state still matches its expected definition, as in configuration-drift detection. | Code the dominant target. Record a secondary target in the rationale. | `antonakoglou2025camino`, `rivera2023blockchain` |
| Behavioral Compliance | Whether observed reachability, isolation, traffic paths, or other network behavior meets the intent's requirements. | Code the dominant target. | `hossain2025netintent`, `castanares2024intent` |
| Service-Level Outcome | Whether committed KPIs or SLAs, such as latency, capacity, or availability, are met. | Code the dominant target. | `filinis2024intent`, `abbas2024ai` |

### Assurance Mechanism

| Value | Definition | Decision rule | Examples |
|---|---|---|---|
| State Conformance Checking | Rules, schemas, or diffs deterministically compare deployed and expected state. |  | `antonakoglou2025camino`, `rivera2023blockchain` |
| Model-Based Evaluation | Simulation or symbolic analysis uses an explicit system model synchronized with live telemetry, such as a digital twin or topology model. | Formal verification of the deployed data plane belongs here. The same analysis on a candidate before activation is a Translation or Enforcement function. | `vilalta2023applying`, `li2023kano` |
| Specification-Based Monitoring | Live measurements are continuously checked against thresholds, KPIs, SLAs, or temporal-logic specifications. |  | `martini2023intent`, `castanares2024intent` |
| Data-Driven Prediction | Statistical or machine-learning models detect anomalies or predict drift, yielding a risk signal rather than proof of correctness. | Feedback from a training environment is not operational assurance. | `abbas2024ai`, `violos2026detecting` |
| Generative Diagnostic Reasoning | A generative component, such as an LLM agent, diagnoses deviations or reasons about corrective action. | There is no hybrid value on this axis. Multi-mechanism systems carry several marks. | `dzeparoska2024intent`, `akbari2025intentcontinuum` |

### Resolution Routing

| Value | Definition | Decision rule | Examples |
|---|---|---|---|
| Local Re-actuation | The policy stays in place while infrastructure is adjusted, for example by scaling, migration, rerouting, or rollback to an earlier configuration. | Execution itself is recorded under Enforcement. | `antonakoglou2025camino`, `zhang2024intent` |
| Policy Regeneration | The policy is updated or regenerated, with feedback returning to Translation. | The trigger must come from deployed state. A rejection during artifact production stays within Translation. | `castanares2024intent`, `dzeparoska2023policy` |
| Human Escalation | Automated correction is suspended and the diagnosis is passed to an operator. | Only post-deployment escalation counts. An approval step before activation is a Pre-deployment gate. | `wang2025confucius`, `li2023kano` |

## Notation in the study-level tables

Symbols are defined per table.

| Table | Symbol | Meaning |
|---|---|---|
| All | `§` | Study in the security-oriented stratum |
| Intent Capture | `†` | Family identified; sub-mechanism undetermined or outside the listed subtypes. The mark sits in the family's first column |
| Intent Translation | `†` | Family and sub-mechanism not identified in the current coding |
| Intent Translation | `‡` | Output form not reported in the source |
| Policy Enforcement | `‡` | Axis not reported in the source |
| Policy Enforcement | Empty integrity cell | Safeguard absent from the system as described |
| Policy Assurance | `†` | Boundary case retained under the observation-only convention |
| Policy Assurance | `‡` | A post-deployment observation function is present, but no identifiable mechanism is reported |
| Policy Assurance | Empty resolution group | Open loop: deviations are evaluated but not routed |
