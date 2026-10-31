# Competency Questions

The University Knowledge Graph was scoped around the following eight competency questions.

1. **University structure:** Which faculties belong to a particular university?
2. **Faculty structure:** Which departments belong to a particular faculty?
3. **Academic staff:** Which professors work in a particular department?
4. **Course catalogue:** Which courses are offered by a particular department?
5. **Teaching assignment:** Which professors teach a particular course?
6. **Student programme:** Which students are enrolled in a particular study programme?
7. **Course prerequisites:** What prerequisites must be completed before enrolling in a particular course?
8. **Research supervision:** Which research projects are supervised by a particular professor?

## Why these questions matter

Together, the questions cover organisational structure, teaching, enrolment, prerequisite discovery and research supervision. They are intended to guide the vocabulary and relationships of the ontology.

## Current coverage and limitation

The ontology contains properties intended to support these questions:

- `hasFaculty`, `hasDepartment`
- `worksInDepartment`, `offersCourse`, `teaches`
- `enrolledIn`, `hasPrerequisite`
- `supervises` and its inverse `isSupervisedBy`

The current repository contains the schema only; it does not contain individuals or real university records. Therefore, the questions describe intended query capabilities and cannot yet return real university answers. Adding instance data and SPARQL queries is future work.
