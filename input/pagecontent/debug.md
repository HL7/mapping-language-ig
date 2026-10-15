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

#### Relationship to the Debug Adapter Protocol (DAP)

The [Debug Adapter Protocol (DAP)](https://microsoft.github.io/debug-adapter-protocol/) is the standard interactive protocol used by IDEs (such as VS Code) to communicate with live language debuggers. DAP is a stateful, bidirectional session protocol where a debugger pauses an active execution thread at breakpoints and exchanges requests for stack frames, scopes, variable expansion handles (`variablesReference`), and stepping commands.

Because FHIR operations such as `$transform` execute as stateless HTTP requests on multi-tenant servers, pausing a live server thread for interactive DAP communication is not practical. At the same time, FML transformations (and CQL/FHIRPath evaluations) are deterministic, single-pass evaluations over a fixed set of inputs that typically complete very quickly and do not involve interactive user input during execution. Capturing a self-contained replay recording in a single stateless response is therefore both lightweight and natural for the execution model, and a local IDE or web tool can then expose that recording through a local DAP replay adapter (including reverse stepping):

| FHIR Debug Trace Concept | Debug Adapter Protocol (DAP) Mapping |
| - | - |
| `trace` sequence + `phase` (`enter`, `step`, `exit`) + `depth` | Forward and reverse stepping (`stepIn` advances one event; `next` skips events with `depth > d`; `stepOut` advances to `depth < d`; `stepBack` / `reverseContinue` move backwards) |
| Active enclosing `enter` events at the current trace index | `stackTrace` response (`StackFrame[]`) reconstructed from the trace history |
| `trace.source` + `startRow`/`startColumn`–`endRow`/`endColumn` | `StackFrame.source` and 1-based line/column ranges (converted from 0-based trace coordinates) |
| `trace.variable.contextType` (`input`, `output`, `local`, `parameter`, etc.) | `scopes` response (`Scope[]`) grouping variables in the debugger UI |
| `trace.variable` (`name`, `value`/`resource`/`jsonValue`/`path`, `dataType`) | `variables` and `evaluate` responses (`Variable[]`), with local `variablesReference` handles generated by the replay adapter to expand complex JSON/Resource trees |
| `trace.message` | `output` event (`console` / `stdout`) |
| `outcome` (`OperationOutcome`) + `traceStatus = stopped` | `stopped` event (`reason: 'exception'`) and `exceptionInfo` response at the point of failure |
{:.grid}


### Questions

#### Include an InstanceId? (as found in Java debuggers)

Should `trace.variable.instanceId` be included to identify the underlying object instance of a variable's value across steps and stack frames?

| Parameter Name | Type | Card | Description |
| - | - | - | - |
| trace.variable.instanceId | string | 0..1 | Engine-assigned identifier for the underlying object instance of the variable's value, if applicable (e.g., populated using `System.identityHashCode(value)` in Java; not a FHIR `Resource.id` or `Element.id`). Not all engines or languages will support this, and it is omitted for empty collections or values without object identity. See [Include an InstanceId?](#include-an-instanceid-as-found-in-java-debuggers) for details. |
{:.grid}

* **Where it comes from and how Java populates it:**
  * In Java IDEs (such as Eclipse and IntelliJ IDEA, which build on the Java Debug Interface), non-primitive objects in the Variables view are displayed with an object identity (e.g., `Patient (id=87)` in Eclipse or `{Patient@4821}` in IntelliJ). This allows a developer to see when two variables (such as a rule variable `tgt` and a dependent group parameter) point to the same heap instance.
  * In the Java FHIR reference implementation, every value—including primitives—is a heap-allocated `Base` object, so an engine can populate `instanceId` using `System.identityHashCode(value)` (captured on the live object before any deep-cloning for the trace snapshot, rather than on the `Variable` wrapper, which is re-instantiated when variables are passed into dependent groups).
* **Placement (Value vs. Variable):**
  * Because this tracks the data instance rather than the variable declaration, it belongs alongside each item value on `trace.variable` (so each item in a collection can have its own `instanceId`, and an empty collection has none).
* **Cross-language and architectural considerations:**
  * **Unboxed primitives and transient navigators:** In JavaScript, Python, and .NET, primitive strings, numbers, and booleans are unboxed values rather than heap instances with distinct object identities (and in Python, `id()` on small integers, booleans, or `None` returns shared singletons, while freed memory addresses can be reused). In addition, engines that navigate source data using lightweight transient cursors (such as Firely `ITypedElement` or JSON navigators) do not maintain a single heap object per element.
  * **Mutable vs. immutable evaluation models:** Object instance tracking assumes that target objects are mutated in place while keeping the same object identity. In functional engines or implementations that use immutable/copy-on-write state transitions on each step, every update produces a new instance, which would cause `instanceId` to change on each step and could be misleading.
  * **Debugger UI support (DAP / VS Code vs. Eclipse / IntelliJ):** Unlike Eclipse and IntelliJ, the Debug Adapter Protocol (DAP) and VS Code's Variables view do not have a dedicated field or column for an object instance ID. Displaying it in a DAP-based UI requires concatenating it into the variable's `value` or `type` display string, which can interfere with VS Code's built-in change highlighting when variable values update across steps.
