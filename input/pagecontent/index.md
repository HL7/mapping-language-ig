### Introduction
The FHIR Mapping Language specification was previously a part of the core FHIR Specification, however as of FHIR version 6.0 it has been removed into it's own Implementation Guide. This is consistent with other similar specifications like [FHIRPath](https://hl7.org/fhirpath) and[CQL](https://cql.hl7.org), which are both used intimitely with FHIR content, however are defined outside the specification, and have their own lifecycle outside the core FHIR balloting process.

This specification can be used with ANY version of fhir, and one of its primary use cases is mapping between versions of FHIR, and not just the version that the mapping language was defined for.

The content of the mapping language specification is to be considered normative with respect to the content that was included in previous versions of FHIR. Clarifications to remove ambiguity, and also additional functionality can be added to the language, however any existing maps there were conformant with the FHIR R5 FML text representation must be able to be parsed and executed with any new version.

We welcome and encourage all feedback on this content.

* [Specification](fml.html)
* [Tutorial](tutorial.html)
* [Examples](artifacts.html)

### Cross Version Analysis
{% lang-fragment cross-version-analysis.xhtml %}

### Dependency Table
{% lang-fragment dependency-table.xhtml %}

### Global Profiles Table
{% lang-fragment globals-table.xhtml %}

### IP Statements
{% lang-fragment ip-statements.xhtml %}
