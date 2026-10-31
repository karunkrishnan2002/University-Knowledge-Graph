# Competency Question to Ontology Mapping

This mapping indicates which model elements are intended to support each question. It is a design mapping, not proof that the question can currently be answered: the repository contains no individuals.

| # | Competency question | Main properties |
|---:|---|---|
| 1 | Which faculties belong to a university? | `hasFaculty` |
| 2 | Which departments belong to a faculty? | `hasDepartment` |
| 3 | Which professors work in a department? | `worksInDepartment` |
| 4 | Which courses are offered by a department? | `offersCourse` |
| 5 | Which professors teach a course? | `teaches` |
| 6 | Which students are enrolled in a study programme? | `enrolledIn` |
| 7 | What prerequisites must be completed before a course? | `hasPrerequisite` |
| 8 | Which research projects are supervised by a professor? | `supervises`, `isSupervisedBy` |

## Future query implementation

After adding instance data, each question can be implemented as a SPARQL `SELECT` query. The query patterns should be tested against the actual data and the ontology's chosen modelling assumptions.
