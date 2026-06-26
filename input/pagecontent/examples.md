
This page provides minimal self-contained examples of every feature of the [FHIR Mapping Language](fml.html).
Each example is a complete, parseable FML file demonstrating a single feature.

> **Note:** There are several bugs in the IG Publisher that prevent some features from being used, and as such they do not render correctly on this page.

### Feature Coverage

| Feature | Example |
|---------|---------|
| [Metadata](#metadata) | [Basic](#metadata-basic) |
| | [Extended](#metadata-extended) |
| [Uses](#uses) | [Source and Target](#uses-source-target) |
| | [Alias](#uses-alias) |
| | [Queried and Produced](#uses-queried-produced) |
| [Other Headers/Syntax](#other) | [Imports](#imports) |
|  | [Constants](#constants) |
|  | [Embedded ConceptMap](#conceptmap-embedded) |
|  | [Backtick Identifiers](#syntax-backticks) |
|  | [Round-trippable comments](#comments) |
| [Group](#group) | [Basic](#group-basic) |
| | [Extends](#group-extends) |
| | [Types](#group-types) |
| | [Type Plus](#group-type-plus) |
| [Simple](#simple) | [Identity Transform](#simple-identity) |
|  | [Batch Identity](#simple-batch) |
| [Source](#source) | [Simple Copy](#source-copy) |
|  | [Type Filter](#source-type-filter) |
|  | [Cardinality](#source-cardinality) |
|  | [Default Value](#source-default) |
|  | [List First](#source-list-first) |
|  | [List Last](#source-list-last) |
|  | [List Not First](#source-list-not-first) |
|  | [List Not Last](#source-list-not-last) |
|  | [List Only One](#source-list-only-one) |
|  | [Where Condition](#source-where) |
|  | [Check Condition](#source-check) |
|  | [Log Statement](#source-log) |
|  | [Multiple Sources](#source-multiple) |
| [Target](#target) | [List Modes](#target-list-modes) |
|  | [Multiple Targets](#target-multiple) |
|  | [Sub-Element](#target-subelement) |
|  | [Using FHIRPath as a source](#tgt-fhirpath-source) |
| [Transform](#transform) | [create](#transform-create) |
|  | [truncate](#transform-truncate) |
|  | [cast](#transform-cast) |
|  | [append](#transform-append) |
|  | [translate](#transform-translate) |
|  | [reference](#transform-reference) |
|  | [evaluate](#transform-evaluate) |
|  | [cc](#transform-cc) |
|  | [c](#transform-c) |
|  | [qty](#transform-qty) |
|  | [id](#transform-id) |
|  | [cp](#transform-cp) |
|  | [uuid](#transform-uuid) |
| [Dependent](#dependent) | [Inline Rules](#dependent-inline) |
|  | [Group Invocation](#dependent-group) |
|  | [Multiple Groups](#dependent-multi-group) |
| [Structure](#structure) | [Property to Array](#struct-prop-to-array) |
|  | [Array to Property](#struct-array-to-prop) |
|  | [Array to Array](#struct-array-to-array) |
|  | [Nested to Top Level](#struct-nested-to-top) |
|  | [Top Level to Nested](#struct-top-to-nested) |
{:.list}

---

<a name="metadata"></a>
### Metadata

<a name="metadata-basic"></a>
#### Metadata - Basic
example: [metadata-basic.fml](StructureMap-MetadataBasic.html)

Every map requires `url`, `name`, and `status` metadata.

```fml
{% fragment StructureMap/MetadataBasic fml %}
```

---

<a name="metadata-extended"></a>
#### Metadata - Extended
example: [metadata-extended.fml](StructureMap-MetadataExtended.html)

Maps can include title, experimental flag, jurisdiction, and multi-line markdown description.

```fml
{% fragment StructureMap/MetadataExtended fml %}
```

---

<a name="uses"></a>
### Uses

<a name="uses-source-target"></a>
#### Uses - Source and Target
example: [uses-source-target.fml](StructureMap-UsesSourceTarget.html)

The `uses` declaration references structure definitions and declares their role as `source` or `target`.

```fml
{% fragment StructureMap/UsesSourceTarget fml %}
```

---

<a name="uses-alias"></a>
#### Uses - Alias
example: [uses-alias.fml](StructureMap-UsesAlias.html)

An `alias` provides an alternate name for a type when source and target share the same type name.

```fml
{% fragment StructureMap/UsesAlias fml %}
```

---

<a name="uses-queried-produced"></a>
#### Uses - Queried and Produced
example: [uses-queried-produced.fml](StructureMap-UsesQueriedProduced.html)

The `queried` mode references lookups during mapping; `produced` references types created as side-effects.

```fml
{% fragment StructureMap/UsesQueriedProduced fml %}
```

---

<a name="other"></a>
### Other Headers/Syntax

<a name="imports"></a>
#### Imports
example: [imports.fml](StructureMap-Imports.html)

The `imports` statement includes other maps; wildcards (`*`) allow bulk import.

```fml
{% fragment StructureMap/Imports fml %}
```

---

<a name="constants"></a>
#### Constants
example: [constants.fml](StructureMap-Constants.html)

The `let` keyword defines reusable constants available as variables in all rules.

```fml
{% fragment StructureMap/Constants fml %}
```

---

<a name="conceptmap-embedded"></a>
#### Embedded ConceptMap
example: [conceptmap-embedded.fml](StructureMap-ConceptMapEmbedded.html)

A ConceptMap can be embedded directly in the mapping file and referenced by local id.

```fml
{% fragment StructureMap/ConceptMapEmbedded fml %}
```

---

<a name="syntax-backticks"></a>
#### Syntax - Backtick Identifiers
example: [syntax-backticks.fml](StructureMap-SyntaxBackticks.html)

Element names from the data model that conflict with reserved words or contain special characters can be escaped with backticks.

```fml
{% fragment StructureMap/SyntaxBackticks fml %}
```

---

<a name="comments"></a>
#### Comments - where FML supports round trippable comments with StructureMap
example: [comments.fml](StructureMap-Comments.html)

Although FML can have comments pretty much anywhere, the StructureMap resource doesn't handle that, and this example shows all the places it can hold documentation in comments

```fml
{% fragment StructureMap/Comments fml %}
```

---

<a name="group"></a>
### Group

<a name="group-basic"></a>
#### Group - Basic
example: [group-basic.fml](StructureMap-GroupBasic.html)

A group defines a named set of rules with typed source and target parameters.

```fml
{% fragment StructureMap/GroupBasic fml %}
```

---

<a name="group-extends"></a>
#### Group - Extends
example: [group-extends.fml](StructureMap-GroupExtends.html)

A group can `extends` another group, inheriting its rules.

```fml
{% fragment StructureMap/GroupExtends fml %}
```

---

<a name="group-types"></a>
#### Group - Types
example: [group-types.fml](StructureMap-GroupTypes.html)

The `<<types>>` stereotype marks a group as the default mapping for a source/target type pair.

```fml
{% fragment StructureMap/GroupTypes fml %}
```

---

<a name="group-type-plus"></a>
#### Group - Type Plus
example: [group-type-plus.fml](StructureMap-GroupTypePlus.html)

The `<<type+>>` stereotype is like `<<types>>` but also creates the target type when not fixed.

```fml
{% fragment StructureMap/GroupTypePlus fml %}
```

---

<a name="simple"></a>
### Simple

<a name="simple-identity"></a>
#### Simple - Identity Transform
example: [simple-identity.fml](StructureMap-SimpleIdentity.html)

When source and target types match, the short form `src.element -> tgt.element` invokes the default mapping.
> Note that this is similar to the Source - Simple Copy (below)

```fml
{% fragment StructureMap/SimpleIdentity fml %}
```

---

<a name="simple-batch"></a>
#### Simple - Batch Identity
example: [simple-batch.fml](StructureMap-SimpleBatch.html)

The batch short form copies multiple elements in a single declaration.

```fml
{% fragment StructureMap/SimpleBatch fml %}
```

---

<a name="source"></a>
### Source

<a name="source-copy"></a>
#### Source - Simple Copy
example: [source-copy.fml](StructureMap-SourceCopy.html)

Copies a source element to a target element using a named variable.
> Note that this is very similar to the Simple - Identity Transform (above)

```fml
{% fragment StructureMap/SourceCopy fml %}
```

---

<a name="source-type-filter"></a>
#### Source - Type Filter
example: [source-type-filter.fml](StructureMap-SourceTypeFilter.html)

A source can be filtered by type using the `: type` syntax, selecting only matching elements.

```fml
{% fragment StructureMap/SourceTypeFilter fml %}
```

---

<a name="source-cardinality"></a>
#### Source - Cardinality
example: [source-cardinality.fml](StructureMap-SourceCardinality.html)

Specifying `min..max` on a source raises an error if the actual cardinality is outside the range.

```fml
{% fragment StructureMap/SourceCardinality fml %}
```

---

<a name="source-default"></a>
#### Source - Default Value
example: [source-default.fml](StructureMap-SourceDefault.html)

The `default` keyword provides a fallback value when the source element is absent.

```fml
{% fragment StructureMap/SourceDefault fml %}
```

---

<a name="source-list-first"></a>
#### Source - List First
example: [source-list-first.fml](StructureMap-SourceListFirst.html)

The `first` list option selects only the first element from a repeating source.

```fml
{% fragment StructureMap/SourceListFirst fml %}
```

---

<a name="source-list-last"></a>
#### Source - List Last
example: [source-list-last.fml](StructureMap-SourceListLast.html)

The `last` list option selects only the last element from a repeating source.

```fml
{% fragment StructureMap/SourceListLast fml %}
```

---

<a name="source-list-not-first"></a>
#### Source - List Not First
example: [source-list-not-first.fml](StructureMap-SourceListNotFirst.html)

The `not_first` list option skips the first element and processes all remaining.

```fml
{% fragment StructureMap/SourceListNotFirst fml %}
```

---

<a name="source-list-not-last"></a>
#### Source - List Not Last
example: [source-list-not-last.fml](StructureMap-SourceListNotLast.html)

The `not_last` list option processes all elements except the last one.

```fml
{% fragment StructureMap/SourceListNotLast fml %}
```

---

<a name="source-list-only-one"></a>
#### Source - List Only One
example: [source-list-only-one.fml](StructureMap-SourceListOnlyOne.html)

The `only_one` list option raises an error if the source contains more than one element.

```fml
{% fragment StructureMap/SourceListOnlyOne fml %}
```

---

<a name="source-where"></a>
#### Source - Where Condition
example: [source-where.fml](StructureMap-SourceWhere.html)

The `where` clause filters source elements using a FHIRPath expression; non-matching elements are skipped.

```fml
{% fragment StructureMap/SourceWhere fml %}
```

---

<a name="source-check"></a>
#### Source - Check Condition
example: [source-check.fml](StructureMap-SourceCheck.html)

The `check` clause raises an error if the FHIRPath expression returns false for a matched source.

```fml
{% fragment StructureMap/SourceCheck fml %}
```

---

<a name="source-log"></a>
#### Source - Log Statement
example: [source-log.fml](StructureMap-SourceLog.html)

The `log` statement writes a FHIRPath expression result to the application log.

```fml
{% fragment StructureMap/SourceLog fml %}
```

---

<a name="source-multiple"></a>
#### Source - Multiple Sources
example: [source-multiple.fml](StructureMap-SourceMultiple.html)

Multiple source statements produce a cross-product; the rule fires for each combination.

```fml
{% fragment StructureMap/SourceMultiple fml %}
```

---

<a name="target"></a>
### Target

<a name="target-list-modes"></a>
#### Target - List Modes
example: [target-list-modes.fml](StructureMap-TargetListModes.html)

Target list modes (`first`, `last`, `share`, `single`) control how repeating target elements are managed.

```fml
{% fragment StructureMap/TargetListModes fml %}
```

---

<a name="target-multiple"></a>
#### Target - Multiple Targets
example: [target-multiple.fml](StructureMap-TargetMultiple.html)

A single rule can produce multiple target assignments separated by commas.

```fml
{% fragment StructureMap/TargetMultiple fml %}
```

---

<a name="target-subelement"></a>
#### Target - Sub-Element
example: [target-subelement.fml](StructureMap-TargetSubelement.html)

Target elements can be qualified with sub-elements using dot notation.

```fml
{% fragment StructureMap/TargetSubelement fml %}
```

---

<a name="tgt-fhirpath-source"></a>
#### Target - Using FHIRPath as a source
example: [fhirpath-source-as-var.fml](StructureMap-FhirPathSourceAsVar.html)

Although you can't currently use a fhirpath expression as a source directly,
there is a workaround where you can use the target side to create a variable and populate it with a fhirpath expression, then use that
value in a `then` dependent set of rules.

```fml
{% fragment StructureMap/FhirPathSourceAsVar fml %}
```

---

<a name="transform"></a>
### Transform

<a name="transform-create"></a>
#### Transform - create
example: [transform-create.fml](StructureMap-TransformCreate.html)

The `create` transform constructs a new instance of a specified type.

```fml
{% fragment StructureMap/TransformCreate fml %}
```

---

<a name="transform-truncate"></a>
#### Transform - truncate
example: [transform-truncate.fml](StructureMap-TransformTruncate.html)

The `truncate` transform limits a string to a specified maximum length.

```fml
{% fragment StructureMap/TransformTruncate fml %}
```

---

<a name="transform-cast"></a>
#### Transform - cast
example: [transform-cast.fml](StructureMap-TransformCast.html)

The `cast` transform converts a value from one type to another.

```fml
{% fragment StructureMap/TransformCast fml %}
```

---

<a name="transform-append"></a>
#### Transform - append
example: [transform-append.fml](StructureMap-TransformAppend.html)

The `append` transform concatenates multiple string values together.

```fml
{% fragment StructureMap/TransformAppend fml %}
```

---

<a name="transform-translate"></a>
#### Transform - translate
example: [transform-translate.fml](StructureMap-TransformTranslate.html)

The `translate` transform maps codes using a ConceptMap via the `$translate` operation.

```fml
{% fragment StructureMap/TransformTranslate fml %}
```

---

<a name="transform-reference"></a>
#### Transform - reference
example: [transform-reference.fml](StructureMap-TransformReference.html)

The `reference` transform produces a reference string pointing to the specified resource.

```fml
{% fragment StructureMap/TransformReference fml %}
```

---

<a name="transform-evaluate"></a>
#### Transform - evaluate
example: [transform-evaluate.fml](StructureMap-TransformEvaluate.html)

The `evaluate` transform executes a FHIRPath expression and uses the result as the target value.

```fml
{% fragment StructureMap/TransformEvaluate fml %}
```

---

<a name="transform-cc"></a>
#### Transform - cc
example: [transform-cc.fml](StructureMap-TransformCc.html)

The `cc` transform creates a CodeableConcept from system, code, and optional display parameters.

```fml
{% fragment StructureMap/TransformCc fml %}
```

---

<a name="transform-c"></a>
#### Transform - c
example: [transform-c.fml](StructureMap-TransformC.html)

The `c` transform creates a Coding from system, code, and optional display parameters.

```fml
{% fragment StructureMap/TransformC fml %}
```

---

<a name="transform-qty"></a>
#### Transform - qty
example: [transform-qty.fml](StructureMap-TransformQty.html)

The `qty` transform creates a Quantity from value, unit, and optional system/code.

```fml
{% fragment StructureMap/TransformQty fml %}
```

---

<a name="transform-id"></a>
#### Transform - id
example: [transform-id.fml](StructureMap-TransformId.html)

The `id` transform creates an Identifier from system, value, and optional type.

```fml
{% fragment StructureMap/TransformId fml %}
```

---

<a name="transform-cp"></a>
#### Transform - cp
example: [transform-cp.fml](StructureMap-TransformCp.html)

The `cp` transform creates a ContactPoint from system and value.

```fml
{% fragment StructureMap/TransformCp fml %}
```

---

<a name="transform-uuid"></a>
#### Transform - uuid
example: [transform-uuid.fml](StructureMap-TransformUuid.html)

The `uuid` transform generates a random UUID.

```fml
{% fragment StructureMap/TransformUuid fml %}
```

---

<a name="dependent"></a>
### Dependent

<a name="dependent-inline"></a>
#### Dependent - Inline Rules
example: [dependent-inline.fml](StructureMap-DependentInline.html)

Rules can contain inline dependent rules in a `then { }` block.

```fml
{% fragment StructureMap/DependentInline fml %}
```

---

<a name="dependent-group"></a>
#### Dependent - Group Invocation
example: [dependent-group.fml](StructureMap-DependentGroup.html)

A rule can invoke another group by name using `then GroupName(params)`.

```fml
{% fragment StructureMap/DependentGroup fml %}
```

---

<a name="dependent-multi-group"></a>
#### Dependent - Multiple Groups
example: [dependent-multi-group.fml](StructureMap-DependentMultiGroup.html)

A rule can invoke multiple groups in sequence, separated by commas.

```fml
{% fragment StructureMap/DependentMultiGroup fml %}
```

---

<a name="structure"></a>
### Structure

<a name="struct-prop-to-array"></a>
#### Structure - Property to Array
example: [struct-prop-to-array.fml](StructureMap-StructPropToArray.html)

Maps a scalar property into a repeating array element by wrapping it in a new structure.

```fml
{% fragment StructureMap/StructPropToArray fml %}
```

---

<a name="struct-array-to-prop"></a>
#### Structure - Array to Property
example: [struct-array-to-prop.fml](StructureMap-StructArrayToProp.html)

Extracts a single value from a repeating array using `first` or `last` and maps it to a scalar property.

```fml
{% fragment StructureMap/StructArrayToProp fml %}
```

---

<a name="struct-array-to-array"></a>
#### Structure - Array to Array
example: [struct-array-to-array.fml](StructureMap-StructArrayToArray.html)

Maps each element of a repeating source array to a corresponding element in a repeating target array.

```fml
{% fragment StructureMap/StructArrayToArray fml %}
```

---

<a name="struct-nested-to-top"></a>
#### Structure - Nested to Top Level
example: [struct-nested-to-top.fml](StructureMap-StructNestedToTop.html)

Pulls a value from a nested child element up to a top-level property.

```fml
{% fragment StructureMap/StructNestedToTop fml %}
```

---

<a name="struct-top-to-nested"></a>
#### Structure - Top Level to Nested
example: [struct-top-to-nested.fml](StructureMap-StructTopToNested.html)

Pushes a top-level property value down into a nested structure.

```fml
{% fragment StructureMap/StructTopToNested fml %}
```
