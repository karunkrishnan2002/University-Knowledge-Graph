# Development Walkthrough

This guide documents how the ontology was built in Protégé Desktop. It is intended to help a future reader understand the modelling process and reproduce the main workflow.

## 1. Install Protégé

1. Download Protégé Desktop from <https://protege.stanford.edu/software/>.
2. Install or extract the desktop application for your operating system.
3. Start Protégé Desktop.

The interface can vary slightly by Protégé version.

## 2. Create an ontology

1. Choose **File → New ontology**.
2. Set the ontology IRI to `https://example.org/university-kg`.
3. Save the working ontology.
4. Use **Entities** to edit classes and properties.

## 3. Create classes

Create these 15 classes:

`University`, `Faculty`, `Department`, `Professor`, `Student`, `Course`, `StudyProgramme`, `Semester`, `Examination`, `Classroom`, `ResearchProject`, `Thesis`, `Degree`, `Enrollment`, `StudyMaterial`.

For each class:
1. Select `owl:Thing` and add a subclass.
2. Enter the class name.
3. Add an `rdfs:label` annotation in English.
4. Add an `rdfs:comment` describing the class.
5. Save.

The current ontology uses a flat initial class hierarchy with these classes directly under `owl:Thing`.

## 4. Create object properties

In **Entities → Object properties**:
1. Select `owl:topObjectProperty`.
2. Add a subproperty.
3. Enter the property name.
4. Add an English label and a meaningful comment.
5. Add its intended domain and range.
6. Save.

Use the complete inventory in `PROPERTY_CATALOG.md`. The inverse property `isSupervisedBy` is the reverse of `supervises`.

## 5. Create datatype properties

In **Entities → Data properties**:
1. Select `owl:topDataProperty`.
2. Add a subproperty.
3. Enter the property name.
4. Add an English label and comment.
5. Set the domain class.
6. Set the datatype range (`xsd:string`, `xsd:decimal`, `xsd:integer` or `xsd:date`).
7. Save.

## 6. Add OWL axioms

The implemented axioms are:

- Select `Course`; add `Examination` under **Disjoint With**.
- Select `isSupervisedBy`; set **Inverse Of** to `supervises` (or vice versa).
- Select datatype property `hasCourseCode`; enable **Functional**.
- Select `StudyProgramme`; add this class restriction under **SubClass Of**: `leadsToDegree some Degree`.
- Select `Course`; add this class restriction under **SubClass Of**: `hasCourseCode exactly 1 xsd:string`.

Manchester Syntax is used for the class expressions shown above.

## 7. Save and export Turtle

1. Save the ontology.
2. Use **File → Save As**.
3. Select **Turtle Syntax** in the format/syntax selector.
4. Save the file. The `.ttl` extension is conventional for Turtle; an assignment may explicitly request `.owl`, in which case use that requested extension while selecting Turtle as the serialization.
5. Reopen the exported file to confirm it loads.

## 8. Run a reasoner

1. Open the **Reasoner** menu.
2. Select HermiT if available.
3. Start the reasoner.
4. Wait for classification to finish.
5. Check the explicit consistency status or any reported error.
6. Stop and rerun the reasoner after making changes.

A log line saying that the ontology was processed is evidence that reasoning ran, but the explicit consistency result should be checked separately.

## 9. Validate the Turtle syntax with RDFLib (optional)

```bash
python -m pip install rdflib
```

```python
from rdflib import Graph

graph = Graph()
graph.parse("ontology/university-knowledge-graph.ttl", format="turtle")
print("Parsed triples:", len(graph))
```

Parsing checks syntax, not all OWL semantics. Use Protégé and a reasoner for logical checks.

## 10. Current scope and possible improvements

The current deliverable is a schema with no individuals. To answer competency questions against real records, future work must add individuals and query logic. A future version could add a `TeachingSession` class so classroom assignment is attached to a lecture/session rather than to a course generally.
