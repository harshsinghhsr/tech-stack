# Database Normalization

## 1. First Normal Form (1NF)
- Eliminate repeating groups
- Create separate tables for each set of related data
- Identify each set of related data with a primary key

### Example: 1NF
Before 1NF:
| StudentID | Name    | Courses                |
|-----------|---------|------------------------|
| 1         | John    | Math, Physics, Chemistry |
| 2         | Emma    | Biology, Math          |

After 1NF:
Students:
| StudentID | Name    |
|-----------|---------|
| 1         | John    |
| 2         | Emma    |

StudentCourses:
| StudentID | Course    |
|-----------|-----------|
| 1         | Math      |
| 1         | Physics   |
| 1         | Chemistry |
| 2         | Biology   |
| 2         | Math      |

## 2. Second Normal Form (2NF)
- Meet all 1NF requirements
- Remove subsets of data that apply to multiple rows
- Create separate tables for these subsets
- Use foreign keys to link them

### Example: 2NF
Before 2NF (Already in 1NF):
| OrderID | ProductID | ProductName | Price  | Quantity |
|---------|-----------|-------------|--------|----------|
| 1       | 101       | Widget A    | 10.00  | 5        |
| 1       | 102       | Widget B    | 15.00  | 3        |
| 2       | 101       | Widget A    | 10.00  | 2        |

After 2NF:
Orders:
| OrderID | ProductID | Quantity |
|---------|-----------|----------|
| 1       | 101       | 5        |
| 1       | 102       | 3        |
| 2       | 101       | 2        |

Products:
| ProductID | ProductName | Price  |
|-----------|-------------|--------|
| 101       | Widget A    | 10.00  |
| 102       | Widget B    | 15.00  |

## 3. Third Normal Form (3NF)
- Meet all 2NF requirements
- Remove columns not dependent on the primary key
- If a group of columns are dependent, move them to a separate table

### Example: 3NF
Before 3NF:
| EmployeeID | DepartmentID | DepartmentName | ManagerID |
|------------|--------------|----------------|-----------|
| 1          | 10           | Sales          | 101       |
| 2          | 20           | Marketing      | 102       |

After 3NF:
Employees:
| EmployeeID | DepartmentID |
|------------|--------------|
| 1          | 10           |
| 2          | 20           |

Departments:
| DepartmentID | DepartmentName | ManagerID |
|--------------|----------------|-----------|
| 10           | Sales          | 101       |
| 20           | Marketing      | 102       |

## 4. Boyce-Codd Normal Form (BCNF)
BCNF is a stricter version of 3NF. A table is in BCNF if, for every dependency A → B, A is a superkey of the table.

### Example: Student Advisor System
Consider a university system tracking student advisors:

| StudentID | Course | Advisor |
|-----------|--------|---------|
| 1         | Math   | Dr. Smith |
| 2         | Physics| Dr. Johnson |
| 3         | Math   | Dr. Brown |
| 4         | Physics| Dr. Johnson |

In this table, we have the following functional dependencies:
1. (StudentID, Course) → Advisor
2. Course → Advisor

The second dependency violates BCNF because 'Course' is not a superkey (it doesn't uniquely identify each row), yet it determines 'Advisor'.

To achieve BCNF, we split this into two tables:

Courses:
| Course | Advisor |
|--------|---------|
| Math   | Dr. Smith |
| Physics| Dr. Johnson |
| Math   | Dr. Brown |

Students:
| StudentID | Course |
|-----------|--------|
| 1         | Math   |
| 2         | Physics|
| 3         | Math   |
| 4         | Physics|

Now, in the Courses table, 'Course' is a part of the primary key, making it a superkey. In the Students table, StudentID is the primary key.

## 5. Fourth Normal Form (4NF)
4NF deals with multi-valued dependencies. A table is in 4NF if it's in BCNF and has no multi-valued dependencies.

### Example:
Consider a table of students and their extracurricular activities:

| Student | Sport  | Instrument |
|---------|--------|------------|
| John    | Soccer | Piano      |
| John    | Tennis | Guitar     |
| Mary    | Soccer | Flute      |

This table suggests that if John plays Soccer and Piano, and also Tennis and Guitar, he must also play Soccer and Guitar, and Tennis and Piano. This is likely not true and is a multi-valued dependency.

To achieve 4NF:

StudentSports:
| Student | Sport  |
|---------|--------|
| John    | Soccer |
| John    | Tennis |
| Mary    | Soccer |

StudentInstruments:
| Student | Instrument |
|---------|------------|
| John    | Piano      |
| John    | Guitar     |
| Mary    | Flute      |

## 6. Fifth Normal Form (5NF)
5NF, also known as Project-Join Normal Form (PJ/NF), deals with join dependencies. A table is in 5NF if it cannot be losslessly decomposed into any number of smaller tables.

### Example:
Consider a table representing suppliers, parts, and projects:

| Supplier | Part  | Project |
|----------|-------|---------|
| S1       | P1    | J1      |
| S1       | P2    | J2      |
| S2       | P1    | J1      |

This might seem fine, but it can lead to incorrect assumptions. For instance, if S1 supplies P1 and P2, and is involved in projects J1 and J2, we might incorrectly assume S1 supplies P2 for J1.

To achieve 5NF, we decompose into three tables:

SupplierParts:
| Supplier | Part |
|----------|------|
| S1       | P1   |
| S1       | P2   |
| S2       | P1   |

SupplierProjects:
| Supplier | Project |
|----------|---------|
| S1       | J1      |
| S1       | J2      |
| S2       | J1      |

PartProjects:
| Part | Project |
|------|---------|
| P1   | J1      |
| P2   | J2      |

## 7. Domain-Key Normal Form (DKNF)
DKNF is the highest form of normalization. A table is in DKNF if every constraint on the table is a logical consequence of the definition of keys and domains.

This form is largely theoretical and is often impractical to implement fully. It essentially means that all constraints should be enforceable by simply ensuring the keys are unique and the domain constraints are met.

In practice, most databases aim for 3NF or BCNF, as the higher normal forms can lead to complex designs that may impact performance or usability.