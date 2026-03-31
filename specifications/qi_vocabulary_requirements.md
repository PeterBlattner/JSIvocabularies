# SKOS requirements for vocabularies of JSI signatories

**Status:** draft for discussion · JSI Digital Transformation Workshop, Geneva, 30–31 March 2026  
**Applies to:** controlled vocabularies, thesauri, and terminology resources published by organisations JSI roundtable network, including BIPM, OIML, IEC, ISO, CIE, IUPAC, JCGM,...

---

## 1. Purpose and scope

These requirements define the minimum and recommended metadata structures for QI vocabularies expressed using the W3C Simple Knowledge Organization System (SKOS). They are organised into two mandatory levels and one optional level:

- **Level 1 — minimum SKOS:** the floor below which a resource cannot claim to be machine-readable
- **Level 2 — rich metadata:** the target for resources actively seeking FAIR compliance and inter-vocabulary interoperability
- **Level 3 — OWL extension:** optional formal axioms enabling automated reasoning, applicable only where a concrete reasoning use case exists. Level 3 is not yet addressed in this document

Requirements are labelled `[JSI-nn-Ln]` where `n` is the level and `nn` is the sequence number. Conformance verbs follow ISO/IEC Guidelines: **SHALL**, **SHOULD**, **MAY**.

These requirements align with and extend the following existing specifications:

| Reference | Scope |
|-----------|-------|
| W3C SKOS Reference (Miles & Bechhofer, 2009) | Core SKOS vocabulary and integrity conditions |
| W3C SKOS Primer (Isaac & Summers, 2009) | Usage guidelines for SKOS |
| Cox et al. (2021), *PLOS Comput Biol* 17(6):e1009041 | Ten simple rules for making a vocabulary FAIR |
| DCMI Metadata Terms (DCMI, 2020) | Dublin Core property set for resource description |
| W3C PROV-O (Lebo et al., 2013) | Provenance ontology |
| W3C DCAT v3 (Albertoni et al., 2023) | Data catalogue vocabulary |
| W3C SKOS-XL (Miles & Bechhofer, 2009) | Extended label representation |
| ISO 25964-1:2011 | Thesauri and interoperability: structure |
| ISO 25964-2:2013 | Thesauri and interoperability: interoperability with other vocabularies |
| FAIR Principles (Wilkinson et al., 2016), *Sci Data* 3:160018 | Findable, Accessible, Interoperable, Reusable |
| FOOPS! (Garijo, Corcho & Poveda-Villalón, ISWC 2021) | Automated FAIR assessment for ontologies and vocabularies |
| Semantic Versioning 2.0.0 (semver.org) | Version numbering scheme |
| BCP 47 / IANA Language Subtag Registry | Language tags for literals |

---

## 2. Vocabulary-level requirements

These requirements apply to the `skos:ConceptScheme` (or `owl:Ontology`) resource that represents the vocabulary as a whole.

### 2.1 Identity and persistence

**[JSI-01-L1]** The vocabulary **SHALL** be identified by a single, globally unique IRI that resolves to a machine-readable representation via HTTP or HTTPS.

**[JSI-02-L1]** The vocabulary IRI **SHALL** be persistent. Persistence **SHALL** be ensured either through a recognised persistence infrastructure (w3id.org, purl.org) or through a documented institutional policy committing to IRI stability for a minimum of ten years. *Rationale: the identifier incident discussed at the March 2026 workshop demonstrates the downstream cost of IRI instability.*

**[JSI-03-L1]** A version IRI **SHALL** exist and resolve. The version IRI **SHALL** be stable: once published, the content it resolves to **SHALL NOT** change. *Reference: FOOPS! FIND3; Cox et al. Rule 1c.*

**[JSI-04-L1]** The vocabulary IRI **SHALL** match the IRI declared within the vocabulary itself (via `skos:ConceptScheme` or `owl:ontologyIRI`). *Reference: FOOPS! FIND4.*

**[JSI-05-L2]** The vocabulary **SHOULD** register its namespace prefix in a public prefix registry (prefix.cc, the Linked Open Vocabularies catalogue or a registery to be built up in the JSI community). *Reference: FOOPS! FIND6.*

**[JSI-06-L2]** The vocabulary **SHOULD** be registered in at least one publicly accessible vocabulary catalogue (LOV, a domain registry, or a national or international data portal) to support discovery by systems that do not already know the IRI. *Reference: Cox et al. Rule 2; FOOPS! FIND7.*

### 2.2 Minimum descriptive metadata

**[JSI-07-L1]** The vocabulary resource **SHALL** carry the following metadata properties:

| Property | Requirement | Note |
|----------|-------------|------|
| `dcterms:title` | SHALL | Plain literal; at minimum in English with language tag `@en` |
| `dcterms:description` | SHALL | Free-text scope statement; `@en` required |
| `owl:versionInfo` | SHALL | Semantic version string, e.g. `"3.1.0"` |
| `dcterms:created` | SHALL | `xsd:date` literal |
| `dcterms:modified` | SHALL | `xsd:date` literal; updated on every release |
| `dcterms:license` | SHALL | Resolvable URI, e.g. `<https://creativecommons.org/licenses/by/4.0/>` |

*Reference: FOOPS! FIND5, REUSE1; Cox et al. Rule 6.*

**[JSI-08-L1]** The license **SHALL** be expressed as a resolvable URI pointing to a machine-readable license document, not as a plain-text string. A plain-text statement such as "freely usable" does not satisfy this requirement. *Rationale: automated systems treat unlicensed resources as restricted by default.*

### 2.3 Rich provenance metadata (Level 2)

**[JSI-09-L2]** The vocabulary **SHOULD** carry the following additional metadata:

| Property | Requirement | Note |
|----------|-------------|------|
| `dcterms:creator` | SHOULD | URI of the responsible organisation (ROR preferred) |
| `dcterms:contributor` | SHOULD | URIs of contributing organisations or persons |
| `dcterms:publisher` | SHOULD | URI of the publishing organisation |
| `owl:versionIRI` | SHOULD | Stable, resolvable IRI for this specific release `<https://w3id.org/myvocab/1.0.0/>` |
| `dcterms:source` | SHOULD | URI of the source document or standard |
| `prov:wasDerivedFrom` | SHOULD | URI of a prior version or source publication |
| `dcat:keyword` | SHOULD | Keywords as plain literals with language tags |
| `dcat:landingPage` | MAY | URI of the human-readable documentation page |
| `dcterms:bibliographicCitation` | MAY | A bibliographic reference for the resource |

**[JSI-10-L2]** The vocabulary **SHOULD** declare the scope of what it covers and, where relevant, what it explicitly does not cover, using `dcterms:description` or a `skos:scopeNote` on the `skos:ConceptScheme`. *Reference: Cox et al. Rule 9.*

**[JSI-11-L2]** A named custodian or authority **SHOULD** be identified using `dcterms:creator` or a custom property, with documented responsibility for creating, modifying, and retiring terms. *Reference: Cox et al. Rule 9b.*

**[JSI-12-L2]** A documented change process **SHOULD** be referenced or described, including how community feedback is accepted and how changes are released. *Reference: Cox et al. Rule 10.*

### 2.4 Versioning

**[JSI-13-L1]** Version numbers **SHALL** follow Semantic Versioning 2.0.0 (Major.Minor.Patch). A change that removes or substantially redefines an existing term increments the Major version. Addition of new terms increments Minor. Corrections and metadata-only changes increment Patch.

**[JSI-14-L2]** A changelog **SHOULD** be maintained and linked from the vocabulary, recording additions, deprecations, definition changes, and IRI changes per release.

---

## 3. Access and publication requirements

### 3.1 Content negotiation

**[JSI-15-L1]** The vocabulary IRI **SHALL** be publicly accessible via HTTP or HTTPS without authentication.

**[JSI-16-L2]** The vocabulary **SHOULD** support content negotiation: the same IRI **SHOULD** return an HTML representation when accessed by a browser (Accept: text/html) and an RDF representation when accessed by a machine (Accept: application/turtle or Accept: application/rdf+xml). *Reference: FOOPS! ACC2.*

**[JSI-17-L2]** A human-readable HTML documentation page **SHOULD** be published, covering the vocabulary as a whole and each term individually. Tools such as WIDOCO may be used to generate this page automatically from the RDF source. *Reference: FOOPS! REUSE5.*

### 3.2 Serialisation formats

**[JSI-18-L1]** The vocabulary **SHALL** be available in at least one W3C-standard RDF serialisation. Turtle (`.ttl`) is the preferred format for human authoring; JSON-LD (`.jsonld`) is preferred for web publication and API delivery. *Reference: FOOPS! INT1.*

**[JSI-19-L2]** The vocabulary **SHOULD** be available in at least two serialisation formats. A SPARQL endpoint or a downloadable RDF dump **SHOULD** be provided for large vocabularies. *Reference: FOOPS! INT6.*

---

## 4. Concept-level requirements

These requirements apply to each individual `skos:Concept` within the vocabulary.

### 4.1 Identity

**[JSI-20-L1]** Every concept **SHALL** be identified by a persistent, resolvable IRI following the same persistence policy as the vocabulary IRI. *Reference: Cox et al. Rule 1; FOOPS! FIND1.*

**[JSI-21-L1]** IRI patterns **SHALL** be opaque (e.g. numeric or UUID-based) and **SHALL NOT** encode natural language strings that could change when definitions are revised. Changing a concept IRI is a breaking change and **SHALL** increment the Major version. *Rationale: IRI instability propagates breakage to every vocabulary that uses the identifier as a cross-reference.*

### 4.2 Labels

**[JSI-22-L1]** Every concept **SHALL** carry exactly one `skos:prefLabel` per language, tagged with a BCP 47 language tag (e.g. `@en`, `@fr`, `@de`). *Reference: W3C SKOS Reference, S14.*

**[JSI-23-L1]** The primary working language of the vocabulary **SHALL** be English. A `skos:prefLabel @en` is therefore mandatory for every concept regardless of whether labels in other languages are provided.

**[JSI-24-L2]** Labels in additional working languages of the relevant standards bodies (French, German, Spanish, Russian, Japanese, Arabic) **SHOULD** be provided where they exist in the source publication, using `skos:prefLabel` with the appropriate language tag.

**[JSI-25-L2]** Synonyms and alternative designations **SHOULD** be recorded using `skos:altLabel` with language tags.

**[JSI-26-L2]** Acronyms and symbol-based identifiers (e.g. SI unit symbols) **SHOULD** be recorded as `skos:altLabel` or, where richer metadata is required, using `skosxl:Label` with `skosxl:literalForm`. *Reference: SKOS-XL specification.*

### 4.3 Definitions and notes

**[JSI-27-L1]** Every concept **SHALL** carry a `skos:definition` in English. The definition **SHALL** be self-sufficient: a reader unfamiliar with the resource should be able to understand the concept's meaning without consulting additional documents. *Reference: Cox et al. Rule 7; FOOPS! REUSE3.*

**[JSI-28-L1]** Definitions **SHALL** use genus-differentia structure where applicable: a definition that identifies the broader class of the concept (genus) and the distinguishing characteristics (differentia) is preferred over a purely enumerative or circular definition.

**[JSI-29-L2]** Notes from the source publication (notes to entry, examples, usage restrictions) **SHOULD** be recorded using the appropriate SKOS note property:

| SKOS property | Use |
|---------------|-----|
| `skos:scopeNote` | Clarifications, usage guidance, applicability restrictions |
| `skos:example` | Worked examples of the concept in use |
| `skos:historyNote` | Historical notes, legacy numbering, superseded designations |
| `skos:editorialNote` | Internal notes for vocabulary editors (not for end users) |
| `skos:changeNote` | Record of what changed and when |

**[JSI-30-L2]** The source of each definition **SHOULD** be recorded using `dcterms:source` pointing to the URI of the source document or its section. Where no URI exists, a bibliographic reference in plain text is acceptable as a fallback.

### 4.4 Hierarchy and structure

**[JSI-31-L1]** Hierarchical relationships between concepts **SHALL** be expressed using `skos:broader` and `skos:narrower`. Both directions **SHALL** be declared or derivable via SKOS inference rules. Cycles are forbidden. *Reference: W3C SKOS Reference, S26–S36.*

**[JSI-32-L2]** Associative (non-hierarchical) relationships between concepts within the same vocabulary **SHOULD** be expressed using `skos:related`. *Reference: ISO 25964-1, clause 11.*

**[JSI-33-L2]** Every concept **SHOULD** be assigned to at least one `skos:ConceptScheme` via `skos:inScheme`. Top-level concepts **SHOULD** also be declared with `skos:topConceptOf`. *Reference: W3C SKOS Reference, S4.*

### 4.5 Provenance at concept level

**[JSI-34-L2]** Each concept **SHOULD** carry `dcterms:created` and `dcterms:modified` dates as `xsd:date` literals to enable fine-grained change tracking at the term level.

**[JSI-35-L2]** The source document or publication from which the term originates **SHOULD** be recorded using `dcterms:source` or `prov:wasDerivedFrom` at the concept level.

### 4.6 Deprecation

**[JSI-36-L1]** When a concept is superseded or withdrawn, it **SHALL NOT** be deleted from the vocabulary. It **SHALL** be marked with `owl:deprecated "true"^^xsd:boolean` and a `skos:changeNote` explaining the reason and pointing to any successor concept. *Reference: Cox et al. Rule 8; rationale: deletion breaks any external resource that references the deprecated IRI.*

**[JSI-37-L1]** If a concept is replaced by another concept, the relationship **SHOULD** be expressed using `dcterms:isReplacedBy` pointing to the successor IRI.

---

## 5. Inter-vocabulary alignment requirements

**[JSI-38-L2]** Mappings between concepts in different JSI vocabularies **SHOULD** be expressed using the SKOS mapping properties. The appropriate property SHALL be chosen carefully:

| Property | Semantics | Use |
|----------|-----------|-----|
| `skos:exactMatch` | Concepts are equivalent across vocabularies | Same concept, different namespace |
| `skos:closeMatch` | Concepts are similar but not identical | Overlapping scope or near-synonym |
| `skos:broadMatch` | The mapped concept is broader | The external concept subsumes this one |
| `skos:narrowMatch` | The mapped concept is narrower | This concept subsumes the external one |
| `skos:relatedMatch` | Associative cross-vocabulary link | Related but no hierarchical relationship |

*Reference: ISO 25964-2; W3C SKOS Reference, S54–S57.*

**[JSI-39-L2]** `skos:exactMatch` **SHALL NOT** be used where definitional differences exist between the two concepts. Where doubt exists, `skos:closeMatch` is the safer choice.

**[JSI-40-L2]** At least one mapping to a concept in a related JSI vocabulary or an upper vocabulary (QUDT, Schema.org, a relevant ISO standard) **SHOULD** be provided for each concept where such a mapping exists. *Reference: Cox et al. Rule 5.*

**[JSI-41-L2]** Before introducing a new concept into a JSI vocabulary, existing JSI vocabularies and relevant external resources **SHOULD** be surveyed to determine whether the concept already exists and can be reused or referenced. *Reference: Cox et al. Rule 0.*

---

## 6. Turtle template: Level 1 (minimum)

The following template illustrates the minimum structure required under Level 1. It is adapted from the spectral line example produced during the March 2026 workshop.

```turtle
@prefix skos:    <http://www.w3.org/2004/02/skos/core#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix owl:     <http://www.w3.org/2002/07/owl#> .
@prefix xsd:     <http://www.w3.org/2001/XMLSchema#> .
@prefix JSI:      <https://w3id.org/JSI/vocab/example/> .

## ── Vocabulary (ConceptScheme) ─────────────────────────────────

JSI:ExampleVocabulary
    a skos:ConceptScheme ;
    dcterms:title        "Example JSI vocabulary"@en ;
    dcterms:description  "A minimal vocabulary illustrating Level 1 requirements."@en ;
    owl:versionInfo      "1.0.0" ;
    dcterms:created      "2026-03-31"^^xsd:date ;
    dcterms:modified     "2026-03-31"^^xsd:date ;
    dcterms:license      <https://creativecommons.org/licenses/by/4.0/> ;
    skos:hasTopConcept   JSI:C001 .

## ── Concept ────────────────────────────────────────────────────

JSI:C001
    a skos:Concept ;
    skos:inScheme        JSI:ExampleVocabulary ;
    skos:topConceptOf    JSI:ExampleVocabulary ;
    skos:prefLabel       "example term"@en ;
    skos:definition      "A concept serving as a template for Level 1 encoding."@en ;
    skos:scopeNote       "Replace this with the relevant note from the source document."@en .
```

---

## 7. Turtle template: Level 2 (rich metadata)

The following template illustrates the recommended structure under Level 2. It extends the Level 1 template with full provenance, multilingual labels, cross-vocabulary mappings, and concept-level metadata.

```turtle
@prefix skos:    <http://www.w3.org/2004/02/skos/core#> .
@prefix skosxl:  <http://www.w3.org/2008/05/skos-xl#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix dcat:    <http://www.w3.org/ns/dcat#> .
@prefix prov:    <http://www.w3.org/ns/prov#> .
@prefix owl:     <http://www.w3.org/2002/07/owl#> .
@prefix xsd:     <http://www.w3.org/2001/XMLSchema#> .
@prefix JSI:      <https://w3id.org/JSI/vocab/example/> .

## ── Vocabulary (ConceptScheme) ─────────────────────────────────

JSI:ExampleVocabulary
    a skos:ConceptScheme ;

    ## Identity
    owl:versionIRI       <https://w3id.org/JSI/vocab/example/1.0.0/> ;
    owl:versionInfo      "1.0.0" ;
    owl:versionIRI       <https://w3id.org/myvocab/1.0.0/> ;

    ## Descriptive metadata
    dcterms:title        "Example JSI vocabulary"@en ;
    dcterms:title        "Vocabulaire JSI exemple"@fr ;
    dcterms:description  "Illustrative vocabulary for the quality infrastructure."@en ;
    dcat:keyword         "metrology"@en , "quality infrastructure"@en ;
    dcat:landingPage     <https://w3id.org/JSI/vocab/example/docs/> ;

    ## Dates
    dcterms:created      "2026-03-31"^^xsd:date ;
    dcterms:modified     "2026-03-31"^^xsd:date ;

    ## Responsibility
    dcterms:creator      <https://ror.org/00ya7p057> ;   ## BIPM ROR identifier
    dcterms:publisher    <https://ror.org/00ya7p057> ;
    dcterms:source       <https://doi.org/10.example/source-publication> ;
    prov:wasDerivedFrom  <https://w3id.org/JSI/vocab/example/0.9.0/> ;

    ## Licence — machine-readable URI, not plain text
    dcterms:license      <https://creativecommons.org/licenses/by/4.0/> ;

    skos:hasTopConcept   JSI:C001 .

## ── Concept ────────────────────────────────────────────────────

JSI:C001
    a skos:Concept ;
    skos:inScheme        JSI:ExampleVocabulary ;
    skos:topConceptOf    JSI:ExampleVocabulary ;

    ## Labels — primary language
    skos:prefLabel       "example term"@en ;
    skos:altLabel        "sample concept"@en ;
    skos:prefLabel       "terme exemple"@fr ;
    skos:prefLabel       "Beispielbegriff"@de ;

    ## Definition (genus-differentia preferred)
    skos:definition
        "A concept that belongs to the class of illustrative examples and is
         distinguished by its role as a template for SKOS encoding."@en ;

    ## Notes
    skos:scopeNote       "Use this pattern for all concepts in the vocabulary."@en ;
    skos:example         "Any term in a controlled vocabulary can serve as an example."@en ;
    skos:historyNote     "This entry was introduced in version 1.0.0."@en ;
    skos:changeNote      "Definition revised to follow genus-differentia structure — 2026-03-31."@en ;

    ## Hierarchy
    skos:narrower        JSI:C002 ;

    ## Term-level provenance
    dcterms:source       <https://doi.org/10.example/source-publication> ;
    dcterms:created      "2026-03-31"^^xsd:date ;
    dcterms:modified     "2026-03-31"^^xsd:date ;

    ## Cross-vocabulary alignment
    skos:exactMatch      <https://goldbook.iupac.org/terms/view/E02115> ;
    skos:closeMatch      <http://qudt.org/vocab/quantitykind/Energy> .
```


---

## 9. Notes on implementation for JSI vocabularies

**On IRI persistence.** The March 2026 workshop surfaced a concrete case where one organisation plans to change term identifiers without informing other organisations that were referencing those identifiers. Level 1 requirement [JSI-02-L1] directly addresses this: persistence is not a technical nicety but a commitment to the network of organisations that depend on the identifiers. Any planned change to existing concept IRIs SHALL be communicated to all known consumers before the change is made, and deprecated IRIs SHALL continue to resolve.

**On definition quality.** Many JSI vocabulary definitions were written for human experts reading a printed standard. Translating them into SKOS does not automatically make them self-sufficient. Requirement [JSI-27-L1] calls for definitions that work without the surrounding context of the source document. This will often require editorial work beyond simple copy-paste.

**On multilingual labels.** Several JSI organisations operate across multiple official languages. Where translations already exist in source publications, they should be captured in SKOS from the outset — retrofitting translations later is significantly more expensive. The priority languages for JSI vocabularies are English (mandatory), French, German, Spanish, Russian, Japanese, and Arabic.

**On the FAIR assessment gap.** The March 2026 workshop observed a 15-percentage-point difference between internal and external FAIR assessments of the same vocabulary. This suggests that conformance verification should routinely include an external assessment alongside the self-assessment. The FOOPS! automated validator (foops.linkeddata.es) provides an objective baseline independent of both perspectives.

**On reuse before creation.** Requirement [JSI-41-L2] reflects a key finding from the workshop: the quality infrastructure already contains substantial definitional overlap across vocabularies. Before introducing a new concept, the existing JSI vocabulary landscape should be surveyed — using the relationship mapping approach developed during the workshop — to determine whether the concept already exists elsewhere and can be referenced rather than duplicated.

---

*This document should be read alongside the Turtle templates for JSI vocabularies (spectral_line_ontology.ttl) and the FAIR maturity assessment grids (FOOPS! and Cox et al.) developed during the March 2026 workshop.*
