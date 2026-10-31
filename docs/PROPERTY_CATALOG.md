# Property Catalog

This document lists the properties currently present in the ontology and their intended domains and ranges.

## Object properties

| Property | Domain | Range | Meaning |
|---|---|---|---|
| `hasFaculty` | University | Faculty | Connects a university to a faculty |
| `hasDepartment` | Faculty | Department | Connects a faculty to a department |
| `worksInDepartment` | Professor | Department | Indicates the department in which a professor works |
| `offersCourse` | Department | Course | Connects a department to a course it offers |
| `teaches` | Professor | Course | Connects a professor to a course they teach |
| `enrolledIn` | Student | StudyProgramme | Connects a student to a study programme |
| `hasPrerequisite` | Course | Course | Connects a course to a prerequisite course |
| `supervises` | Professor | ResearchProject | Connects a professor to a research project they supervise |
| `isSupervisedBy` | ResearchProject | Professor | Inverse of `supervises` |
| `offeredIn` | Course | Semester | Connects a course to an academic semester |
| `hasExamination` | Course | Examination | Connects a course to an examination |
| `conductedIn` | Course | Classroom | Connects a course to a classroom in this model |
| `writesThesis` | Student | Thesis | Connects a student to a thesis |
| `leadsToDegree` | StudyProgramme | Degree | Connects a study programme to a degree |
| `hasStudyMaterial` | Course | StudyMaterial | Connects a course to supporting study material |
| `participatesIn` | Student | ResearchProject | Connects a student to a research project |

## Datatype properties

| Property | Domain | Range | Meaning |
|---|---|---|---|
| `hasName` | University | `xsd:string` | Name of the university |
| `hasEmail` | Professor | `xsd:string` | Institutional email of a professor |
| `hasStudentID` | Student | `xsd:string` | Student identifier; string preserves letters/leading zeros |
| `hasCourseCode` | Course | `xsd:string` | Course identifier |
| `hasECTS` | Course | `xsd:decimal` | ECTS credit amount |
| `hasSemesterName` | Semester | `xsd:string` | Academic semester name |
| `hasRoomNumber` | Classroom | `xsd:string` | Room identifier |
| `hasCapacity` | Classroom | `xsd:integer` | Maximum classroom capacity |
| `hasExaminationDate` | Examination | `xsd:date` | Calendar date of an examination |
| `hasProjectTitle` | ResearchProject | `xsd:string` | Research project title |

## Property modelling notes

- Identifier-like values (`hasStudentID`, `hasCourseCode`, `hasRoomNumber`) are strings because they may contain letters, punctuation or leading zeros.
- `hasECTS` uses `xsd:decimal` to support fractional credits.
- `hasCapacity` uses `xsd:integer` because it is a whole-number count.
- `hasExaminationDate` uses `xsd:date` rather than `xsd:dateTime` because it records a calendar date only.
- A domain/range axiom is a semantic statement. In RDF/OWL, it can cause type inferences; it is not a form-level validation rule.
