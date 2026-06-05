
This page provides minimal self-contained examples of every feature of the [FHIR Mapping Language](fml.html).
Each example is a complete, parseable FML file demonstrating a single feature.

#### Feature Coverage

| # | Feature | Example |
|---|---------|---------|
| 1 | [Metadata - Basic](#metadata-basic) | [metadata-basic.fml](StructureMap-MetadataBasic.html) |
| 2 | [Metadata - Extended](#metadata-extended) | [metadata-extended.fml](StructureMap-MetadataExtended.html) |
| 3 | [Uses - Source and Target](#uses-source-target) | [uses-source-target.fml](StructureMap-UsesSourceTarget.html) |
| 4 | [Uses - Alias](#uses-alias) | [uses-alias.fml](StructureMap-UsesAlias.html) |
| 5 | [Uses - Queried and Produced](#uses-queried-produced) | [uses-queried-produced.fml](StructureMap-UsesQueriedProduced.html) |
| 6 | [Imports](#imports) | [imports.fml](StructureMap-Imports.html) |
| 7 | [Constants](#constants) | [constants.fml](StructureMap-Constants.html) |
| 8 | [Group - Basic](#group-basic) | [group-basic.fml](StructureMap-GroupBasic.html) |
| 9 | [Group - Extends](#group-extends) | [group-extends.fml](StructureMap-GroupExtends.html) |
| 10 | [Group - Types](#group-types) | [group-types.fml](StructureMap-GroupTypes.html) |
| 11 | [Group - Type Plus](#group-type-plus) | [group-type-plus.fml](StructureMap-GroupTypePlus.html) |
| 12 | [Source - Simple Copy](#source-copy) | [source-copy.fml](StructureMap-SourceCopy.html) |
| 13 | [Source - Type Filter](#source-type-filter) | [source-type-filter.fml](StructureMap-SourceTypeFilter.html) |
| 14 | [Source - Cardinality](#source-cardinality) | [source-cardinality.fml](StructureMap-SourceCardinality.html) |
| 15 | [Source - Default Value](#source-default) | [source-default.fml](StructureMap-SourceDefault.html) |
| 16 | [Source - List First](#source-list-first) | [source-list-first.fml](StructureMap-SourceListFirst.html) |
| 17 | [Source - List Last](#source-list-last) | [source-list-last.fml](StructureMap-SourceListLast.html) |
| 18 | [Source - List Not First](#source-list-not-first) | [source-list-not-first.fml](StructureMap-SourceListNotFirst.html) |
| 19 | [Source - List Not Last](#source-list-not-last) | [source-list-not-last.fml](StructureMap-SourceListNotLast.html) |
| 20 | [Source - List Only One](#source-list-only-one) | [source-list-only-one.fml](StructureMap-SourceListOnlyOne.html) |
| 21 | [Source - Where Condition](#source-where) | [source-where.fml](StructureMap-SourceWhere.html) |
| 22 | [Source - Check Condition](#source-check) | [source-check.fml](StructureMap-SourceCheck.html) |
| 23 | [Source - Log Statement](#source-log) | [source-log.fml](StructureMap-SourceLog.html) |
| 24 | [Source - Multiple Sources](#source-multiple) | [source-multiple.fml](StructureMap-SourceMultiple.html) |
| 25 | [Transform - create](#transform-create) | [transform-create.fml](StructureMap-TransformCreate.html) |
| 26 | [Transform - truncate](#transform-truncate) | [transform-truncate.fml](StructureMap-TransformTruncate.html) |
| 27 | [Transform - cast](#transform-cast) | [transform-cast.fml](StructureMap-TransformCast.html) |
| 28 | [Transform - append](#transform-append) | [transform-append.fml](StructureMap-TransformAppend.html) |
| 29 | [Transform - translate](#transform-translate) | [transform-translate.fml](StructureMap-TransformTranslate.html) |
| 30 | [Transform - reference](#transform-reference) | [transform-reference.fml](StructureMap-TransformReference.html) |
| 31 | [Transform - evaluate](#transform-evaluate) | [transform-evaluate.fml](StructureMap-TransformEvaluate.html) |
| 32 | [Transform - cc](#transform-cc) | [transform-cc.fml](StructureMap-TransformCc.html) |
| 33 | [Transform - c](#transform-c) | [transform-c.fml](StructureMap-TransformC.html) |
| 34 | [Transform - qty](#transform-qty) | [transform-qty.fml](StructureMap-TransformQty.html) |
| 35 | [Transform - id](#transform-id) | [transform-id.fml](StructureMap-TransformId.html) |
| 36 | [Transform - cp](#transform-cp) | [transform-cp.fml](StructureMap-TransformCp.html) |
| 37 | [Transform - uuid](#transform-uuid) | [transform-uuid.fml](StructureMap-TransformUuid.html) |
| 41 | [Target - List Modes](#target-list-modes) | [target-list-modes.fml](StructureMap-TargetListModes.html) |
| 42 | [Target - Multiple Targets](#target-multiple) | [target-multiple.fml](StructureMap-TargetMultiple.html) |
| 43 | [Target - Sub-Element](#target-subelement) | [target-subelement.fml](StructureMap-TargetSubelement.html) |
| 44 | [Dependent - Inline Rules](#dependent-inline) | [dependent-inline.fml](StructureMap-DependentInline.html) |
| 45 | [Dependent - Group Invocation](#dependent-group) | [dependent-group.fml](StructureMap-DependentGroup.html) |
| 46 | [Dependent - Multiple Groups](#dependent-multi-group) | [dependent-multi-group.fml](StructureMap-DependentMultiGroup.html) |
| 47 | [Simple - Identity Transform](#simple-identity) | [simple-identity.fml](StructureMap-SimpleIdentity.html) |
| 48 | [Simple - Batch Identity](#simple-batch) | [simple-batch.fml](StructureMap-SimpleBatch.html) |
| 49 | [Embedded ConceptMap](#conceptmap-embedded) | [conceptmap-embedded.fml](StructureMap-ConceptMapEmbedded.html) |
| 50 | [Syntax - Backtick Identifiers](#syntax-backticks) | [syntax-backticks.fml](StructureMap-SyntaxBackticks.html) |
| 51 | [Structure - Property to Array](#struct-prop-to-array) | [struct-prop-to-array.fml](StructureMap-StructPropToArray.html) |
| 52 | [Structure - Array to Property](#struct-array-to-prop) | [struct-array-to-prop.fml](StructureMap-StructArrayToProp.html) |
| 53 | [Structure - Array to Array](#struct-array-to-array) | [struct-array-to-array.fml](StructureMap-StructArrayToArray.html) |
| 54 | [Structure - Nested to Top Level](#struct-nested-to-top) | [struct-nested-to-top.fml](StructureMap-StructNestedToTop.html) |
| 55 | [Structure - Top Level to Nested](#struct-top-to-nested) | [struct-top-to-nested.fml](StructureMap-StructTopToNested.html) |

---

<a name="metadata-basic"></a>
#### 1. Metadata - Basic

Every map requires `url`, `name`, and `status` metadata.

```fml
{% fragment StructureMap/MetadataBasic fml %}
```

---

<a name="metadata-extended"></a>
#### 2. Metadata - Extended

Maps can include title, experimental flag, jurisdiction, and multi-line markdown description.

```fml
{% fragment StructureMap/MetadataExtended fml %}

/// url = "http://hl7.org/fhir/uv/fml/StructureMap/MetadataExtended"
/// name = "MetadataExtended"
/// status = "active"
/// title = "Extended Metadata Example"
/// experimental = true
/// jurisdiction =
/// jurisdiction.coding =
/// jurisdiction.coding.system = "urn:iso:std:iso:3166"
/// jurisdiction.coding.code = "US"
/// description = """
This is a **multi-line** markdown description.
It demonstrates the extended metadata capabilities.
"""
```

---

<a name="uses-source-target"></a>
#### 3. Uses - Source and Target

The `uses` declaration references structure definitions and declares their role as `source` or `target`.

```fml
{% fragment StructureMap/UsesSourceTarget fml %}
```

---

<a name="uses-alias"></a>
#### 4. Uses - Alias

An `alias` provides an alternate name for a type when source and target share the same type name.

```fml
{% fragment StructureMap/UsesAlias fml %}
```

---

<a name="uses-queried-produced"></a>
#### 5. Uses - Queried and Produced

The `queried` mode references lookups during mapping; `produced` references types created as side-effects.

```fml
{% fragment StructureMap/UsesQueriedProduced fml %}
```

---

<a name="imports"></a>
#### 6. Imports

The `imports` statement includes other maps; wildcards (`*`) allow bulk import.

```fml
{% fragment StructureMap/Imports fml %}
```

---

<a name="constants"></a>
#### 7. Constants

The `let` keyword defines reusable constants available as variables in all rules.

```fml
{% fragment StructureMap/Constants fml %}
```

---

<a name="group-basic"></a>
#### 8. Group - Basic

A group defines a named set of rules with typed source and target parameters.

```fml
{% fragment StructureMap/GroupBasic fml %}
```

---

<a name="group-extends"></a>
#### 9. Group - Extends

A group can `extends` another group, inheriting its rules.

```fml
{% fragment StructureMap/GroupExtends fml %}
```

---

<a name="group-types"></a>
#### 10. Group - Types

The `<<types>>` stereotype marks a group as the default mapping for a source/target type pair.

```fml
{% fragment StructureMap/GroupTypes fml %}
```

---

<a name="group-type-plus"></a>
#### 11. Group - Type Plus

The `<<type+>>` stereotype is like `<<types>>` but also creates the target type when not fixed.

```fml
{% fragment StructureMap/GroupTypePlus fml %}
```

---

<a name="source-copy"></a>
#### 12. Source - Simple Copy

Copies a source element to a target element using a named variable.
> Note that this is very similar to the Simple - Identity Transform (below)

```fml
{% fragment StructureMap/SourceCopy fml %}
```

---

<a name="source-type-filter"></a>
#### 13. Source - Type Filter

A source can be filtered by type using the `: type` syntax, selecting only matching elements.

```fml
{% fragment StructureMap/SourceTypeFilter fml %}
```

---

<a name="source-cardinality"></a>
#### 14. Source - Cardinality

Specifying `min..max` on a source raises an error if the actual cardinality is outside the range.

```fml
{% fragment StructureMap/SourceCardinality fml %}
```

---

<a name="source-default"></a>
#### 15. Source - Default Value

The `default` keyword provides a fallback value when the source element is absent.

```fml
{% fragment StructureMap/SourceDefault fml %}
```

---

<a name="source-list-first"></a>
#### 16. Source - List First

The `first` list option selects only the first element from a repeating source.

```fml
{% fragment StructureMap/SourceListFirst fml %}
```

---

<a name="source-list-last"></a>
#### 17. Source - List Last

The `last` list option selects only the last element from a repeating source.

```fml
{% fragment StructureMap/SourceListLast fml %}
```

---

<a name="source-list-not-first"></a>
#### 18. Source - List Not First

The `not_first` list option skips the first element and processes all remaining.

```fml
{% fragment StructureMap/SourceListNotFirst fml %}
```

---

<a name="source-list-not-last"></a>
#### 19. Source - List Not Last

The `not_last` list option processes all elements except the last one.

```fml
{% fragment StructureMap/SourceListNotLast fml %}
```

---

<a name="source-list-only-one"></a>
#### 20. Source - List Only One

The `only_one` list option raises an error if the source contains more than one element.

```fml
{% fragment StructureMap/SourceListOnlyOne fml %}
```

---

<a name="source-where"></a>
#### 21. Source - Where Condition

The `where` clause filters source elements using a FHIRPath expression; non-matching elements are skipped.

```fml
{% fragment StructureMap/SourceWhere fml %}
```

---

<a name="source-check"></a>
#### 22. Source - Check Condition

The `check` clause raises an error if the FHIRPath expression returns false for a matched source.

```fml
{% fragment StructureMap/SourceCheck fml %}
```

---

<a name="source-log"></a>
#### 23. Source - Log Statement

The `log` statement writes a FHIRPath expression result to the application log.

```fml
{% fragment StructureMap/SourceLog fml %}
```

---

<a name="source-multiple"></a>
#### 24. Source - Multiple Sources

Multiple source statements produce a cross-product; the rule fires for each combination.

```fml
{% fragment StructureMap/SourceMultiple fml %}
```

---

<a name="transform-create"></a>
#### 25. Transform - create

The `create` transform constructs a new instance of a specified type.

```fml
{% fragment StructureMap/TransformCreate fml %}
```

---

<a name="transform-truncate"></a>
#### 26. Transform - truncate

The `truncate` transform limits a string to a specified maximum length.

```fml
{% fragment StructureMap/TransformTruncate fml %}
```

---

<a name="transform-cast"></a>
#### 27. Transform - cast

The `cast` transform converts a value from one type to another.

```fml
{% fragment StructureMap/TransformCast fml %}
```

---

<a name="transform-append"></a>
#### 28. Transform - append

The `append` transform concatenates multiple string values together.

```fml
{% fragment StructureMap/TransformAppend fml %}
```

---

<a name="transform-translate"></a>
#### 29. Transform - translate

The `translate` transform maps codes using a ConceptMap via the `$translate` operation.

```fml
{% fragment StructureMap/TransformTranslate fml %}
```

---

<a name="transform-reference"></a>
#### 30. Transform - reference

The `reference` transform produces a reference string pointing to the specified resource.

```fml
{% fragment StructureMap/TransformReference fml %}
```

---

<a name="transform-evaluate"></a>
#### 31. Transform - evaluate

The `evaluate` transform executes a FHIRPath expression and uses the result as the target value.

```fml
{% fragment StructureMap/TransformEvaluate fml %}
```

---

<a name="transform-cc"></a>
#### 32. Transform - cc

The `cc` transform creates a CodeableConcept from system, code, and optional display parameters.

```fml
{% fragment StructureMap/TransformCc fml %}
```

---

<a name="transform-c"></a>
#### 33. Transform - c

The `c` transform creates a Coding from system, code, and optional display parameters.

```fml
{% fragment StructureMap/TransformC fml %}
```

---

<a name="transform-qty"></a>
#### 34. Transform - qty

The `qty` transform creates a Quantity from value, unit, and optional system/code.

```fml
{% fragment StructureMap/TransformQty fml %}
```

---

<a name="transform-id"></a>
#### 35. Transform - id

The `id` transform creates an Identifier from system, value, and optional type.

```fml
{% fragment StructureMap/TransformId fml %}
```

---

<a name="transform-cp"></a>
#### 36. Transform - cp

The `cp` transform creates a ContactPoint from system and value.

```fml
{% fragment StructureMap/TransformCp fml %}
```

---

<a name="transform-uuid"></a>
#### 37. Transform - uuid

The `uuid` transform generates a random UUID.

```fml
{% fragment StructureMap/TransformUuid fml %}
```

---


<a name="target-list-modes"></a>
#### 41. Target - List Modes

Target list modes (`first`, `last`, `share`, `single`) control how repeating target elements are managed.

```fml
{% fragment StructureMap/TargetListModes fml %}
```

---

<a name="target-multiple"></a>
#### 42. Target - Multiple Targets

A single rule can produce multiple target assignments separated by commas.

```fml
{% fragment StructureMap/TargetMultiple fml %}
```

---

<a name="target-subelement"></a>
#### 43. Target - Sub-Element

Target elements can be qualified with sub-elements using dot notation.

```fml
{% fragment StructureMap/TargetSubelement fml %}
```

---

<a name="dependent-inline"></a>
#### 44. Dependent - Inline Rules

Rules can contain inline dependent rules in a `then { }` block.

```fml
{% fragment StructureMap/DependentInline fml %}
```

---

<a name="dependent-group"></a>
#### 45. Dependent - Group Invocation

A rule can invoke another group by name using `then GroupName(params)`.

```fml
{% fragment StructureMap/DependentGroup fml %}
```

---

<a name="dependent-multi-group"></a>
#### 46. Dependent - Multiple Groups

A rule can invoke multiple groups in sequence, separated by commas.

```fml
{% fragment StructureMap/DependentMultiGroup fml %}
```

---

<a name="simple-identity"></a>
#### 47. Simple - Identity Transform

When source and target types match, the short form `src.element -> tgt.element` invokes the default mapping.
> Note that this is similar to the Source - Simple Copy (above)

```fml
{% fragment StructureMap/SimpleIdentity fml %}
```

---

<a name="simple-batch"></a>
#### 48. Simple - Batch Identity

The batch short form copies multiple elements in a single declaration.

```fml
{% fragment StructureMap/SimpleBatch fml %}
```

---

<a name="conceptmap-embedded"></a>
#### 49. Embedded ConceptMap

A ConceptMap can be embedded directly in the mapping file and referenced by local id.

```fml
{% fragment StructureMap/ConceptMapEmbedded fml %}
```

---

<a name="syntax-backticks"></a>
#### 50. Syntax - Backtick Identifiers

Element names from the data model that conflict with reserved words or contain special characters can be escaped with backticks.

```fml
{% fragment StructureMap/SyntaxBackticks fml %}
```

---

<a name="struct-prop-to-array"></a>
#### 51. Structure - Property to Array

Maps a scalar property into a repeating array element by wrapping it in a new structure.

```fml
{% fragment StructureMap/StructPropToArray fml %}
```

---

<a name="struct-array-to-prop"></a>
#### 52. Structure - Array to Property

Extracts a single value from a repeating array using `first` or `last` and maps it to a scalar property.

```fml
{% fragment StructureMap/StructArrayToProp fml %}
```

---

<a name="struct-array-to-array"></a>
#### 53. Structure - Array to Array

Maps each element of a repeating source array to a corresponding element in a repeating target array.

```fml
{% fragment StructureMap/StructArrayToArray fml %}
```

---

<a name="struct-nested-to-top"></a>
#### 54. Structure - Nested to Top Level

Pulls a value from a nested child element up to a top-level property.

```fml
{% fragment StructureMap/StructNestedToTop fml %}
```

---

<a name="struct-top-to-nested"></a>
#### 55. Structure - Top Level to Nested

Pushes a top-level property value down into a nested structure.

```fml
{% fragment StructureMap/StructTopToNested fml %}
```

