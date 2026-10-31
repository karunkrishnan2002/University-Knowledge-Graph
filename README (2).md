# University Knowledge Graph

A teaching-oriented ontology that models the core structure and academic activities of a university. The project uses **RDF, RDFS and OWL**, is authored in **Protégé**, and is serialized as **Turtle**.

> **Project status:** ontology model / academic prototype. It defines concepts, relationships, datatype properties and OWL axioms. It does not contain populated university records, a deployed application, a SPARQL endpoint, or a user interface.

## At a glance

| Item | Current model |
|---|---:|
| Ontology IRI | `https://example.org/university-kg` |
| Classes | 15 |
| Object properties | 16 |
| Datatype properties | 10 |
| Total properties | 26 |
| OWL feature categories demonstrated | 5 |
| Individuals / instance data | 0 |

The ontology file is [`ontology/university-knowledge-graph.ttl`](ontology/university-knowledge-graph.ttl). It is Turtle syntax; the `.ttl` extension is used here to make the serialization explicit. If an assignment portal specifically expects `.owl`, the same Turtle content can be saved with an `.owl` extension from Protégé.

## Why this project exists

University information is often distributed across separate systems: organisational directories, course catalogues, study programme pages, student administration, examination schedules and research pages. A knowledge graph can provide a shared semantic model for connecting these information areas.

This ontology provides a vocabulary for describing:
- the university's organisational structure;
- students, professors and academic programmes;
- courses, semesters, examinations and classrooms;
- research projects, theses, degrees and study materials.

The model is a foundation for a future academic information discovery application. It is not evidence of an integrated production system.

## Repository structure

```text
University-Knowledge-Graph/
├── README.md
├── ontology/
│   └── university-knowledge-graph.ttl
├── docs/
│   ├── COMPETENCY_QUESTIONS.md
│   ├── ONTOLOGY_GUIDE.md
│   ├── PROPERTY_CATALOG.md
│   └── DEVELOPMENT_WALKTHROUGH.md
├── examples/
│   └── competency-question-mapping.md
├── CITATION.cff
└── .gitignore
```

## Quick start

### Option A — Open in Protégé (recommended)

1. Install **Protégé Desktop** from the official site: <https://protege.stanford.edu/software/>.
2. Download or clone this repository.
3. In Protégé, choose **File → Open**.
4. Open `ontology/university-knowledge-graph.ttl`.
5. Inspect **Entities → Classes**, **Object properties**, and **Data properties**.
6. Open **Reasoner**, choose **HermiT** (if available), and start the reasoner.
7. Check the reasoner output and the class/property descriptions.

### Option B — Parse the Turtle file with Python

Install RDFLib:

```bash
python -m pip install rdflib
```

Run:

```python
from rdflib import Graph

graph = Graph()
graph.parse("ontology/university-knowledge-graph.ttl", format="turtle")

print(f"Triples: {len(graph)}")
```

A successful parse confirms that the file is syntactically valid Turtle. It does **not**, by itself, prove that all modelling choices are correct or that the ontology is logically consistent.

## Competency questions

The model was designed around these questions:

1. Which faculties belong to a particular university?
2. Which departments belong to a particular faculty?
3. Which professors work in a particular department?
4. Which courses are offered by a particular department?
5. Which professors teach a particular course?
6. Which students are enrolled in a particular study programme?
7. What prerequisites must be completed before enrolling in a particular course?
8. Which research projects are supervised by a particular professor?

See [`docs/COMPETENCY_QUESTIONS.md`](docs/COMPETENCY_QUESTIONS.md) and the mapping notes in [`examples/competency-question-mapping.md`](examples/competency-question-mapping.md).

## Model contents

### Classes (15)

`University`, `Faculty`, `Department`, `Professor`, `Student`, `Course`, `StudyProgramme`, `Semester`, `Examination`, `Classroom`, `ResearchProject`, `Thesis`, `Degree`, `Enrollment`, `StudyMaterial`.

Each class is annotated with an English `rdfs:label` and `rdfs:comment`.

### Object properties (16)

| Property | Domain | Range |
|---|---|---|
| `hasFaculty` | University | Faculty |
| `hasDepartment` | Faculty | Department |
| `worksInDepartment` | Professor | Department |
| `offersCourse` | Department | Course |
| `teaches` | Professor | Course |
| `enrolledIn` | Student | StudyProgramme |
| `hasPrerequisite` | Course | Course |
| `supervises` | Professor | ResearchProject |
| `isSupervisedBy` | ResearchProject | Professor |
| `offeredIn` | Course | Semester |
| `hasExamination` | Course | Examination |
| `conductedIn` | Course | Classroom |
| `writesThesis` | Student | Thesis |
| `leadsToDegree` | StudyProgramme | Degree |
| `hasStudyMaterial` | Course | StudyMaterial |
| `participatesIn` | Student | ResearchProject |

### Datatype properties (10)

| Property | Domain | Range |
|---|---|---|
| `hasName` | University | `xsd:string` |
| `hasEmail` | Professor | `xsd:string` |
| `hasStudentID` | Student | `xsd:string` |
| `hasCourseCode` | Course | `xsd:string` |
| `hasECTS` | Course | `xsd:decimal` |
| `hasSemesterName` | Semester | `xsd:string` |
| `hasRoomNumber` | Classroom | `xsd:string` |
| `hasCapacity` | Classroom | `xsd:integer` |
| `hasExaminationDate` | Examination | `xsd:date` |
| `hasProjectTitle` | ResearchProject | `xsd:string` |

See [`docs/PROPERTY_CATALOG.md`](docs/PROPERTY_CATALOG.md) for definitions and modelling notes.

## OWL features demonstrated

The current ontology includes these five feature categories:

1. **Disjoint classes:** `Course` is disjoint with `Examination`.
2. **Inverse object properties:** `supervises` and `isSupervisedBy` are inverse properties.
3. **Functional data property:** `hasCourseCode` is functional (at most one distinct value).
4. **Existential restriction:** `StudyProgramme SubClassOf leadsToDegree some Degree`.
5. **Qualified cardinality restriction:** `Course SubClassOf hasCourseCode exactly 1 xsd:string`.

The functional property and exact-cardinality restriction both apply to `hasCourseCode`. Functionality alone means *at most one* value; the exact-cardinality restriction specifies *exactly one* string value.

These are logical axioms. They do not generate values or act as a database form-validation system.

## Development workflow

The project was developed in this order:

1. Define the university domain and competency questions.
2. Create an ontology with IRI `https://example.org/university-kg`.
3. Add the 15 classes and annotate each with a label and comment.
4. Add object properties and specify their domain and range.
5. Add datatype properties and specify their domain and datatype range.
6. Add the OWL axioms listed above.
7. Open the ontology in Protégé and run HermiT.
8. Save/export the ontology in Turtle syntax.

A detailed, reproducible walkthrough is in [`docs/DEVELOPMENT_WALKTHROUGH.md`](docs/DEVELOPMENT_WALKTHROUGH.md).

## Important modelling scope and limitations

- No individuals or real university records are included. The model is a schema, not a populated knowledge graph.
- `hasName` currently has `University` as its domain; it is not a general name property for every class.
- `hasEmail` currently has `Professor` as its domain.
- The exact-one course-code restriction is a modelling assumption: every `Course` is expected to have exactly one string course code.
- The `StudyProgramme` existential restriction assumes each represented programme leads to at least one `Degree`.
- The `conductedIn` property currently connects `Course` to `Classroom`; if the intended meaning is the location of a particular teaching session, a separate `Lecture` or `TeachingSession` class would be a more precise future extension.
- Competency questions are design requirements. Without individuals, the current ontology cannot return real university answers yet.
- A successful Turtle parse and a reasoner run are separate checks. Always rerun the reasoner after editing the ontology.

## Validation

### Syntax validation

```python
from rdflib import Graph

g = Graph()
g.parse("ontology/university-knowledge-graph.ttl", format="turtle")
print("Turtle parsed. Triple count:", len(g))
```

### Logical validation in Protégé

1. Open the file in Protégé.
2. Select HermiT from the **Reasoner** menu.
3. Start the reasoner.
4. Check for an explicit consistency result and inspect any reported errors.
5. Re-run after making changes.

Do not claim that the ontology has been formally verified by a reasoner unless the consistency result is explicitly confirmed in the current Protégé session.

## How to extend this project

Possible next steps (not implemented in this repository):
- add carefully sourced university instances;
- model teaching sessions separately from courses;
- add additional shared person/organisation concepts where justified;
- write SPARQL competency-question queries once instance data exists;
- build a small search interface or connect the graph to an academic information service;
- add automated validation tests and continuous integration.

## Tools

- Protégé Desktop — ontology authoring and reasoning interface.
- RDF / RDFS / OWL — semantic representation and modelling.
- Turtle — RDF serialization.
- HermiT — OWL reasoner available through Protégé.
- RDFLib — optional Python parsing and inspection.

## Author and academic context

Prepared by **Karun Krishnan** as an ontology-modelling project for **Introduction to Semantic Systems 2026W**.

Student identifier and the personal assignment report are intentionally not embedded in this public project README. Consider whether you want your student number or university submission PDF to be publicly visible before adding them to a public GitHub repository.

## License

No open-source license is included. Unless you add a license, people may view the repository on GitHub but do not automatically receive permission to reuse, modify or redistribute the contents. Add a license only after deciding which terms you want.
