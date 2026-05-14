The target transform section of a [mapping rule](fml.html#rules) specifies how source content is used to create target content. Each transform uses a specific transform function to produce the target value.

The general syntax for a target transform is:

```
context.element = transform_code(parameters...) as variable
```

The following sections describe each available transform function, detailing the expected parameters, behavior, and providing examples.

* Although the function parameters are defined with a specific type, they are expressed as FHIRPath expressions that will return the specific type (where a type is defined) - often the expression is a simple constant.
* Square bracket notation `[]` is used in function signatures to indicate optional parameters.

### create(type : String) : any

Creates a new instance of the specified type using the standard API. Where [structure definitions](fml.html#typing) have been provided, the `type` parameter must be a string which is a known type of a root element. Where they haven't been provided, the application must know the name somehow.

If no transform is provided on a target statement, the element is autocreated. It is an error if auto-created elements are primitive types, or have more than one possible type.

For example:
``` fml
src.value as vs -> tgt.value = create("CodeableConcept") as vt then CodeableConcept(vs, vt)
src.entry as entry -> tgt.entry = create("Bundle.entry") as newEntry
```

### copy(source) : any

Simply copies the source to the target as-is. This is only allowed when the types in source and target match - typically for primitive types.

In the concrete syntax, this is simply represented as the source variable:

For example:
``` fml
src.status as v -> tgt.status = v          // implicit copy via variable reference
src.value as v -> tgt.value = copy(v)      // explicit copy
```

### truncate(source : String, length : Integer) : String

Returns the first `length` characters of the `source` string. The `source` must be some string type that has some meaningful length property.

**FHIRPath equivalent:** [substring](http://hl7.org/fhirpath/N1/#substringstart-integer-length-integer-string)

For example:
``` fml
src.description as v -> tgt.text = truncate(v, 100)
```

### escape(source : String, format1 : String, format2 : String) : String

Changes the internal escaping of a string element from `format1` to `format2`.

> **Note:** This is not often needed, as mostly the escaping is done on the base format.

For example:
``` fml
src.text as v -> tgt.text = escape(v, 'html', 'xml')
```

### cast(source, type : String) : any

Casts `source` from one type to another. The target `type` can be left as implicit if there is one and only one target type known. The default namespace for the type is `FHIR` (see [FHIRPath type specifiers](http://hl7.org/fhirpath/N1/#is-type-specifier)).

For example:
``` fml
src.value as v -> tgt.value = cast(v, "string")
src.value as v -> tgt.value = cast(v, "decimal")
```

### append(source...) : String

Concatenates all provided source values together. Each source parameter is an element or string. Multiple parameters are simply appended in order.

**FHIRPath equivalent:** [& (String concatenation)](http://hl7.org/fhirpath/N1/#string-concatenation)

For example:
``` fml
src.given as g, src.family as f -> tgt.display = append(g, ' ', f)
```

### translate(source, map_uri : String, output : String) : any

Uses the [translate operation](https://hl7.org/fhir/conceptmap-operation-translate.html) to translate coded values. The `source` is some type of code or coded datatype, and the `source` and `map_uri` are passed to the translate operation.

The `output` parameter determines what value from the translate operation is used for the result. Valid values are: `code`, `system`, `display`, `Coding`, or `CodeableConcept`.

**FHIRPath equivalent:** `%terminologies.translate()`

For example:
``` fml
src.code as v -> tgt.code = translate(v, 'http://example.org/fhir/ConceptMap/my-map', 'code')
src.status as v -> tgt.status = translate(v, 'http://example.org/fhir/ConceptMap/status-map', 'CodeableConcept')
```

### reference(source) : String

Returns a string that references the provided tree properly. This produces a reference string appropriate for the target context.

For example:
``` fml
src.subject as v -> tgt.subject = reference(v)
```

<a name="fn-todatetime"></a>
### toDateTime(source : String [, format : String]) : DateTime

Parses a string into a DateTime value. The optional `format` parameter specifies the format of the input string using the format codes defined in [Date Time Transformation Details](#date-op-details).

If no `format` is provided, the default FHIR DateTime format is assumed (`yyyy-MM-DDThh:mm:ss.fff(+|-)hh:mm`).

If the `source` string does not match the specified format, the mapping engine raises an error.

For example:
``` fml
src.recordedDate as v -> tgt.recorded = toDateTime(v, 'yyyy-MM-dd')
src.effectiveTime as v -> tgt.effective = toDateTime(v, 'yyyyMMddHHmmss')
src.dateStr as v -> tgt.date = toDateTime(v)    // uses default FHIR format
```

<a name="fn-unixtodatetime"></a>
### unixToDateTime(source : Integer [, timezone : String]) : DateTime

Converts a Unix timestamp (ticks since epoch) into a DateTime value, applying the specified timezone information. If no timezone is provided, the value is converted to UTC.

> **Note:** If a different source epoch is required, this can be achieved by adding or subtracting the difference in ticks prior to calling this transform.

For example:
``` fml
src.timestamp as v -> tgt.recorded = unixToDateTime(v)                      // UTC
src.timestamp as v -> tgt.recorded = unixToDateTime(v, '+10:00')            // Australian Eastern Standard Time
```

<a name="fn-todate"></a>
### toDate(source : String [, format : String]) : Date

Parses a string into a Date value. The optional `format` parameter specifies the format of the input string using the format codes defined in [Date Time Transformation Details](#date-op-details).

If no `format` is provided, the default FHIR Date format is assumed (`yyyy-MM-DD`).

If the `source` string does not match the specified format, the mapping engine raises an error.

For example:
``` fml
src.birthDate as v -> tgt.birthDate = toDate(v, 'dd/MM/yyyy')
src.dateValue as v -> tgt.date = toDate(v, 'yyyyMMdd')
src.dateStr as v -> tgt.date = toDate(v)    // uses default FHIR format
```

<a name="fn-unixtodate"></a>
### unixToDate(source : Integer [, timezone : String]) : Date

Converts a Unix timestamp (ticks since epoch) into a Date value, applying the specified timezone information. If no timezone is provided, the value is converted to UTC.

> **Note:** If a different source epoch is required, this can be achieved by adding or subtracting the difference in ticks prior to calling this transform.

For example:
``` fml
src.timestamp as v -> tgt.birthDate = unixToDate(v)                  // UTC
src.timestamp as v -> tgt.birthDate = unixToDate(v, '-05:00')        // US Eastern Standard Time
```

<a name="fn-totime"></a>
### toTime(source : String [, format : String]) : Time

Parses a string into a Time value. The optional `format` parameter specifies the format of the input string using the format codes defined in [Date Time Transformation Details](#date-op-details).

If no `format` is provided, the default FHIR Time format is assumed (`hh:mm:ss.fff`).

If the `source` string does not match the specified format, the mapping engine raises an error.

For example:
``` fml
src.timeValue as v -> tgt.time = toTime(v, 'HHmmss')
src.timeStr as v -> tgt.time = toTime(v, 'HH:mm')
src.timeStr as v -> tgt.time = toTime(v)    // uses default FHIR format
```

<a name="fn-unixtotime"></a>
### unixToTime(source : Integer [, timezone : String]) : Time

Converts a Unix timestamp (ticks since epoch) into a Time value, applying the specified timezone information. If no timezone is provided, the value is converted to UTC.

> **Note:** If a different source epoch is required, this can be achieved by adding or subtracting the difference in ticks prior to calling this transform.

For example:
``` fml
src.timestamp as v -> tgt.administeredTime = unixToTime(v)            // UTC
src.timestamp as v -> tgt.administeredTime = unixToTime(v, '+05:30')  // India Standard Time
```

<a name="fn-uuid"></a>
### uuid() : String

Generates a random UUID (in lowercase). This function takes no parameters.

For example:
``` fml
src -> tgt.id = uuid()
```

<a name="fn-pointer"></a>
### pointer(resource) : String

Returns the appropriate string to put in a Reference that refers to the resource provided as a parameter.

**Related:** [resolve()](http://hl7.org/fhir/fhirpath.html#functions)

For example:
``` fml
src.subject as v -> tgt.subject = pointer(v)
```

<a name="fn-evaluate"></a>
### evaluate(context, expression : String) : any

Executes the supplied FHIRPath expression and uses the value returned by that expression. The `expression` (2nd parameter) is evaluated in the context of the first parameter (`context`), and the result is used as the value.

* If the outcome of the evaluation of the FHIRPath expression is an empty collection, no element is created in the target.
* If the outcome has a single value, the target is created with that value.
* If the outcome has more than one value, and the element is repeating, a separate target instance will be created for each value.
* If there is more than one value and the element is non-repeating, this is treated as an error.

In the concrete syntax, there is a shorthand for this operation, by supplying `()` around the parameter. In this case, there is no explicit context for the FHIRPath expression, and any context is implicit through references to existing variables such as `$this`.

For example:
``` fml
src -> tgt.count = evaluate(src, 'item.count()')
src.name as v -> tgt.display = evaluate(v, 'given.join(\' \') & \' \' & family')
src -> tgt.total = (src.item.count())    // shorthand form
```

<a name="fn-cc"></a>
### cc(system : String, code : String [, display : String]) : CodeableConcept

Creates a CodeableConcept from the parameters provided. Can also be called with a single `text` parameter to create a CodeableConcept with only a text value.

**FHIRPath equivalent:** [%factory.CodeableConcept()](https://hl7.org/fhir/fhirpath.html#fn-factory-CodeableConcept)

For example:
``` fml
src -> tgt.code = cc('http://loinc.org', '12345-6', 'Test Result')
src -> tgt.category = cc('http://terminology.hl7.org/CodeSystem/observation-category', 'vital-signs')
src.text as v -> tgt.code = cc(v)    // text-only CodeableConcept
```

<a name="fn-c"></a>
### c(system : String, code : String [, display : String]) : Coding

Creates a Coding from the parameters provided.

**FHIRPath equivalent:** [%factory.Coding()](https://hl7.org/fhir/fhirpath.html#fn-factory-Coding)

For example:
``` fml
src -> tgt.code = c('http://loinc.org', '12345-6', 'Test Result')
src -> tgt.type = c('http://terminology.hl7.org/CodeSystem/v2-0203', 'MR')
```

<a name="fn-qty"></a>
### qty(value : Decimal, unit : String [, system : String, code : String]) : Quantity

Creates a Quantity from the parameters provided. Can also be called with a single `text` parameter where `text` is the natural representation, e.g. `[comparator]value[space]unit`.

**FHIRPath equivalent:** [%factory.Quantity()](https://hl7.org/fhir/fhirpath.html#fn-factory-Quantity)

For example:
``` fml
src -> tgt.value = qty(5.5, 'mg', 'http://unitsofmeasure.org', 'mg')
src -> tgt.dose = qty(100, 'mg')
src.value as v -> tgt.quantity = qty(v)    // text form
```

<a name="fn-id"></a>
### id(system : String, value : String [, type : String]) : Identifier

Creates an Identifier from the parameters provided. The optional `type` parameter is a code from the [identifier type value set](https://hl7.org/fhir/valueset-identifier-type.html).

**FHIRPath equivalent:** [%factory.Identifier()](https://hl7.org/fhir/fhirpath.html#fn-factory-Identifier)

For example:
``` fml
src -> tgt.identifier = id('http://example.org/mrn', '12345', 'MR')
src.mrn as v -> tgt.identifier = id('http://hospital.example.org', v)
```

<a name="fn-cp"></a>
### cp(value : String) : ContactPoint
### cp(system : String, value : String) : ContactPoint

Creates a ContactPoint from the parameters provided. If no `system` is provided, the system should be inferred from the content of the value.

**FHIRPath equivalent:** [%factory.ContactPoint()](https://hl7.org/fhir/fhirpath.html#fn-factory-ContactPoint)

For example:
``` fml
src.phone as v -> tgt.telecom = cp('phone', v)
src.email as v -> tgt.telecom = cp('email', v)
src.contact as v -> tgt.telecom = cp(v)    // system inferred from value
```

<a name="date-op-details"></a>
### Date Time Transformation Details

Parsing textual content into date/time values is a complex task, given the wide variety of formats in use in the real world. The date and time transforms ([toDateTime](#fn-todatetime), [toDate](#fn-todate), [toTime](#fn-totime)) provide a way to parse a wide variety of date/time formats into FHIR date/time types.

Unfortunately, there is no single standard for date/time formats across the contexts where FHIR is used (e.g., most programming languages have their own date/time formats), so this specification defines the following format codes.

Note that:
* Format codes are case-sensitive.
* Any character not represented by a format code is treated as a literal and must match exactly in the input string.
* Only a subset of the codes are *required*; implementations may vary.

| Format Code | Support | Description |
| --- | --- | --- |
| `yy` | optional | 2-digit year (e.g., 80 for 1980). Implementations have discretion on how to interpret the century for 2-digit years (e.g., based on contextual knowledge). A common approach is to interpret values 00-49 as 2000-2049 and 50-99 as 1950-1999. Note that this format code is discouraged due to the ambiguity it introduces. |
| `yyyy` | required | 4-digit year (e.g., 2024) |
| `M` | optional | 1- or 2-digit month of year (1=January, etc.) |
| `MM` | required | 2-digit month of year (01=January, etc.) |
| `MMM` | optional | The localized abbreviated name of the month (e.g., 'Jun' for en-US, 'juin' for fr-FR) |
| `MMMM` | optional | The localized full name of the month (e.g., 'June' for en-US, 'juni' for da-DK) |
| `d` | optional | 1- or 2-digit day of month (1 through 31) |
| `dd` | required | 2-digit day of month (01 through 31) |
| `h` | optional | 1- or 2-digit hour of AM/PM (1 through 12) |
| `hh` | required | 2-digit hour of AM/PM (01 through 12) |
| `H` | optional | 1- or 2-digit hour of day (00 through 23) |
| `HH` | required | 2-digit hour of day (00 through 23) |
| `m` | optional | 1- or 2-digit minute of hour (0 through 59) |
| `mm` | required | 2-digit minute of hour (00 through 59) |
| `s` | optional | 1- or 2-digit second of minute (0 through 59) |
| `ss` | required | 2-digit second of minute (00 through 59) |
| `S[+]` | required | 1-digit fraction of second (0 through 9). Note that consecutive fractional seconds are grouped together, e.g. `SSS` for milliseconds. Implementations MUST support at least 3 digits (milliseconds); support for additional digits is optional. |
| `a` | required | 1- or 2-letter localized AM/PM specifier. e.g., en-US: `A`, `AM`, `P`, etc. e.g., ja-JP: `午`, `午前`, etc. |
| `z` | optional | Time zone literal (name or id). E.g., `America/Los_Angeles`, `Pacific Standard Time`, or `PST`. |
| `Z` | required | Time zone offset from UTC (e.g., `+0200`, `-0500`) or `Z` literal for UTC |
{: .grid}
