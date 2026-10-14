This page describes a possible output for a debug trace in the parameters result of the [$transform](OperationDefinition-StructureMap-transform.html) operation, alongside the actual result (in the `return` parameter).
To indicate the desire to capture this detailed debugging information, a new input parameter `debug=true` is added to activate the feature. As with other operations, this could be either a URL parameter, or in the parameters resource in the body.

*Note: To support more isolated FML debugging, some other additional parameters are included for StructureDefinitions and ConceptMaps (as resource or Bundles) via a `model` input parameter.*

The format records evaluation for post-mortem debugging and trace replay; it is not an interactive debugger protocol. Although described here for FML, its events are language-neutral and can also represent CQL, FHIRPath, and other evaluation languages.

Each step in the trace represents an evaluation unit and will include the following information:
  * Source reference
    * File Name/reference/indicator (0..1) - identifies the source artifact, particularly when multiple artifacts are evaluated
    * Position information (start/end/expression)
  * TracePhase - enter, step, exit
  * TraceDepth - call depth (easing step in/out)
  * Variables - name, contextType, value, data-type
  * Log message

### Debugger Trace Additional Output Parameters

| Parameter Name | Type | Card | Description |
| - | - | - | - |
| traceStatus | code | 0..1 | Status of the trace recording: `complete`, `truncated`, or `stopped`, as defined below. If absent, assume `complete`. Producers SHOULD provide this explicitly. |
| trace | *Parameter / Part* | 0..* | The collector for a specific evaluation step |
| trace.source | string | 0..1 | Identifier of the source artifact containing the evaluation unit, such as a document URI, file name, library identifier, or versioned canonical. Its resolution is defined by the evaluation environment. May be omitted when the evaluation environment provides a single unambiguous source artifact. |
| trace.startRow | integer | 0..1 | The start row in the source file (0-based). Must be accompanied by `startColumn`. |
| trace.startColumn | integer | 0..1 | The start column on the start row (0-based UTF-16 code units). Must be accompanied by `startRow`. |
| trace.endRow | integer | 0..1 | The end row in the source file (0-based). Must be accompanied by `endColumn` and a start position. If no end position is available, a caller may infer the end based on the context. |
| trace.endColumn | integer | 0..1 | The exclusive end column on the end row (0-based UTF-16 code units). Must be accompanied by `endRow`. |
| trace.expressionPosition | string | 0..1 | A simple FHIRPath expression identifying a location within a structured source artifact, consistent with OperationOutcome.issue.expression.<br/> e.g. `StructureMap.group[0].rule[2].source[0]`. May be provided alongside a textual range. |
| trace.phase | code | 1..1 | the debugger phase: `enter`, `exit`, `step` |
| trace.depth | integer | 0..1 | Event nesting depth. The outermost scope's `enter` and `exit` have depth `0`; its immediate children, including `step` events, have depth `1`. If omitted, depth is derived from the event phases and scope stack. See the stack rules below. |
| trace.variable | *(Parts)* | 0..* | The complete set of variables in scope at this point. Each entry represents one item in a variable's collection; entries with the same name form an ordered collection.<br/> An entry without an inline value or retrieval `path` represents an empty collection; `contextType` and optional `dataType` metadata may still be present. An entry with a retrieval `path` but no inline value does not imply an empty collection. |
| trace.variable.name | string | 1..1 | the name of the variable |
| trace.variable.contextType | string | 0..1 | Engine-provided classification of the variable's context, such as input, output, local, global, parameter, context, expression, query-alias, let, accumulator, or implicit. Consumers may display unfamiliar values; this is not a closed vocabulary. Often used as a "grouping" mechanism for organizing variables in the debugger interface. |
| trace.variable.value | *FHIR datatype* | 0..1 | One item's value, carried in the named part's `value[x]`. Mutually exclusive with `resource` and `jsonValue`. |
| trace.variable.resource | Resource | 0..1 | One item's resource value, carried in the named part's `resource`. Mutually exclusive with `value` and `jsonValue`. |
| trace.variable.jsonValue | string | 0..1 | One item's JSON representation for debugger visualization, used when it cannot be represented in `value` or `resource`, such as a custom logical model or backbone element. Contains valid JSON text; it is not required to be FHIR JSON or the source system's native serialization. Mutually exclusive with `value` and `resource`. |
| trace.variable.dataType | string | 0..1 | Engine-provided type label for display, reflecting internal modelling information. Informational only; it is not used to interpret or deserialize the value. |
| trace.variable.path | string | 0..1 | A simple FHIRPath expression containing property names and indexers that directly locates the item in an immutable input resource. A debugger may retrieve the item using this path when no inline value is supplied. The evaluation environment must identify the resource against which it is evaluated (for `$transform`, this is the single input `content` resource). |
| trace.message | *(Parts)* | 0..* | A log message to report e.g. from FML `log()`, fhirpath `trace()` |
| trace.message.name | string | 0..1 | The name of the log message *(from fhirpath.trace(name, ...))* |
| trace.message.text | string | 0..* | The text of the log message |
| outcome | OperationOutcome | 0..1 | Errors, warnings, or informational messages resulting from evaluation, including exceptions and fatal evaluation failures. May accompany a result or a trace of a failed evaluation. |
{:.grid}

> **Note:** When outputting variables ensure that a snapshot of the variable's state is captured at the point of the trace, such that later mutations are not reflected in the trace.

Variable entries are snapshots, not changes relative to an earlier event. Absence of a variable name means that it is not in the recorded scope. A retrieval path is not itself a historical snapshot: if the referenced source resource can change, replay must use its state at the recorded point or an inline captured value.

Within each trace event:

* A variable name MUST identify a single visible binding. Shadowed bindings MUST NOT be included as additional entries with the same name.
* An entry representing an empty collection MUST be the only entry with that variable name.

#### Trace Phase Code Values

| Code | Description |
|------|-------------|
| enter | The debugger has entered a new scope. Events with this phase are emitted immediately after entering a new evaluation scope, before any evaluation steps in that scope. Variables present the initial state visible in the new scope. |
| step  | One evaluation unit has completed in the current scope. Variables capture the state after evaluation. For replay, the preceding trace event supplies the before-state, so a separate set of before variables is not recorded. |
| exit  | Evaluation in the current scope has ended normally. Emitted before removing that scope, with its final visible variable state. |
{:.list}

#### Ordering and Stack Rules

Repeated `trace` parameters MUST appear in evaluation order. This order is meaningful and MUST be preserved by consumers. No parallel evaluation or merging of independent recordings is defined, and no sequence numbers are required.

For a `complete` recording:

* Every `enter` MUST have a corresponding `exit`.
* The outermost `enter` and its matching `exit` have depth `0`.
* Each nested `enter` has depth equal to its enclosing `enter`'s depth plus `1`, and its matching `exit` has the same depth.
* A scope's own `step` events are immediate children of that scope and MUST have depth equal to its enclosing `enter`'s depth plus `1`.
* All events inside a scope's matching `enter` and `exit` MUST have a greater depth than those boundary events.
* Scopes MUST close in last-in, first-out order. All child scopes close before their parent's `exit`.
* Where depth is provided, it MUST agree with the depth derived from the event sequence.

For example, the following events show a group containing two rules, where the first rule invokes a dependent group. The labels are illustrative source descriptions, not additional trace properties:

```text
enter group 0
  enter rule 1
    step source 2
    step target 2
    enter dependent-group 2
      enter dependent-rule 3
        step source 4
        step target 4
      exit dependent-rule 3
    exit dependent-group 2
  exit rule 1
  enter rule 1
    step source 2
    step target 2
  exit rule 1
exit group 0
```

For trace replay, stepping over from an event at depth `d` skips subsequent events with depth greater than `d` and advances to the next event with depth less than or equal to `d`, if one exists. This remains at the same level or moves higher in the stack (a smaller numeric depth).

If recording is `stopped` or `truncated`, the returned trace is a prefix of the evaluation's event sequence. Events are not filtered from the middle, and retained events contain complete variable snapshots. The recorded prefix may end within an open scope without its corresponding `exit`, preserving the active call stack and variables in scope at the point where recording or evaluation stopped so a debugger can jump directly to the end of the trace and step backwards.

#### Trace Completeness Code Values

| Code | Description |
|------|-------------|
| complete | Recording reached the normal end of evaluation without omissions caused by recording limits or early termination. |
| truncated | Evaluation continued beyond the returned events, and the returned trace retains only a prefix because of response size limits, event count limits, or other recording limits. |
| stopped | Evaluation terminated before normal completion—for example because of an evaluation error, failed `check` condition, terminology failure, or stack overflow. Open scopes are not unwound with `exit` events, leaving the last recorded event at the point where evaluation stopped. |

Completeness is relative to the requested capture: supplying a retrieval path instead of an inline value does not by itself make a trace incomplete. Reasons for truncation or early termination MUST be reported in `outcome`, with sufficient detail for a consumer to understand why the trace ended. A producer MUST NOT report an incomplete or terminated recording as `complete` or omit `traceStatus` in that case.

### Error/Warning/Information reporting
Diagnostics, including evaluation exceptions, are reported through `outcome`, not through a separate trace phase. Locations can be identified using `OperationOutcome.issue.expression` and applicable location extensions.

The presence of `outcome` alone does not indicate failure. Issues with severity `warning` or `information` may accompany a successful evaluation. Consumers MUST inspect `OperationOutcome.issue.severity`: an issue with severity `error` or `fatal` indicates evaluation failure, even if a `return` parameter is present or `traceStatus` is `complete`.

When debugging is requested and evaluation fails after recording begins, this proposed response contract returns a Parameters resource containing `outcome`, `traceStatus` (typically `stopped`), and the recorded `trace` parameters up to the point of failure, rather than replacing the response with a standalone `OperationOutcome`. A `return` parameter is included only if a valid result was produced. This contract must also be reflected in the operation definition and its documented HTTP failure responses.

When an evaluation unit fails and terminates evaluation, the trace ends at the point of failure without emitting `exit` events for open scopes, and the failure location and details are supplied in `outcome`. Failures before evaluation or recording begins may follow the operation's usual standalone OperationOutcome response.

### Other Considerations
#### Sensitive Data

Trace variables may contain the same sensitive clinical information as the operation's input and output. Implementations should apply the same access controls and data-handling protections, including when debugger clients retain or export traces. Avoid unintended duplication in routine logs, and apply appropriate retention and disposal policies to saved recordings.

#### Large Data Handling
When handling large input resources, and/or many trace events the size
of the recorded trace data can be large. This is unavoidable in many cases, and as such might not fit into an operations response payload comfortably. Currently the model only supports a way to truncate that content and indicate this via the `traceStatus` property.


#### Questions


Include an InstanceId? (what is this, how could it be populated)
