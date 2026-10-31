# Ontology Guide

## Ontology identity

- **Ontology IRI:** `https://example.org/university-kg`
- **Serialization:** Turtle
- **Main file:** `ontology/university-knowledge-graph.ttl`

## Conceptual overview

The ontology describes a university domain using classes, object properties, datatype properties, annotations and OWL axioms.

### Classes

| Class | Intended meaning |
|---|---|
| University | Higher education institution |
| Faculty | Academic organisational unit grouping departments |
| Department | Academic unit focused on a discipline |
| Professor | Academic staff member involved in teaching/research |
| Student | Person enrolled at a university |
| Course | Academic subject or unit of study |
| StudyProgramme | Structured programme of study |
| Semester | Academic period |
| Examination | Assessment associated with a course |
| Classroom | Physical teaching space |
| ResearchProject | Structured academic research activity |
| Thesis | Student research or academic written work |
| Degree | Academic qualification |
| Enrollment | Record or concept representing enrolment |
| StudyMaterial | Educational resource supporting learning |

Each class has an English `rdfs:label` and `rdfs:comment`.

## How to read a property

An object property connects one resource to another resource. For example:

`Professor --teaches--> Course`

A datatype property connects a resource to a literal value. For example:

`Course --hasCourseCode--> "CS101"`

Domains and ranges document the intended subject and object/value types. In RDF/OWL, domain and range axioms also have inferential semantics; they are not database-style input validation.

## OWL axioms

### 1. Disjointness

`Course` is disjoint with `Examination`. This says that no individual can be an instance of both classes under this model.

### 2. Inverse properties

`supervises` and `isSupervisedBy` are inverses. If a professor supervises a research project, the inverse relationship connects that project back to the professor.

### 3. Functional datatype property

`hasCourseCode` is functional. A resource can have at most one distinct course-code value under OWL's functional-property semantics.

### 4. Existential restriction

`StudyProgramme SubClassOf leadsToDegree some Degree`

Every instance of `StudyProgramme` must be related through `leadsToDegree` to at least one `Degree`.

### 5. Qualified exact cardinality

`Course SubClassOf hasCourseCode exactly 1 xsd:string`

Every instance of `Course` must have exactly one `hasCourseCode` value in the `xsd:string` datatype.

## Open-world assumption

OWL generally follows the open-world assumption: information not stated is not automatically treated as false. For example, if no course code is recorded, that absence does not by itself prove that the course has no code. The exact-cardinality axiom is a logical requirement, and a reasoner may detect inconsistency when explicit facts conflict with it.

## Scope cautions

- The current model has no individuals.
- `hasName` applies to `University` in the current model, not to all named entities.
- `hasEmail` applies to `Professor`.
- `conductedIn` currently has `Course` as domain and `Classroom` as range. For modelling an individual lecture/session location, introduce a teaching-session class in a future revision.
- The ontology is an educational prototype and should be reviewed before use with real university data.
