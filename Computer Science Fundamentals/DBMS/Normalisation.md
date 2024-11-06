# Normalisation

Normalisation is used for Database optimization.

## Functional Dependency (FD)

Functional dependency in DBMS refers to a relationship between two sets of attributes in a table, where one set of attributes (the determinant) uniquely determines the value of another set of attributes.

### Example:

Consider a table with the following columns:

| Student_ID | Student_Name | Course_ID | Instructor |
| ---------- | ------------ | --------- | ---------- |
| 101        | John         | CS101     | Dr. Smith  |
| 102        | Alice        | CS102     | Dr. Clark  |

In this case:

- **Student_ID → Student_Name** means that the `Student_ID` uniquely determines the `Student_Name`. If you know the `Student_ID`, you can find the corresponding `Student_Name`.
- **Course_ID → Instructor** means that the `Course_ID` uniquely determines the `Instructor`. If you know the `Course_ID`, you can find the Instructor.

Thus, **Student_ID** functionally determines **Student_Name**, and **Course_ID** functionally determines **Instructor**.

### Types of FD (A → B)

1. **Trivial FD:** If B is a subset of A, e.g., `Student_ID, Student_Name → Student_ID`
2. **Non-Trivial FD:** `Student_ID → Student_Name`

### Rules of FD (Armstrong's Axioms)

1. **Reflexive:** If A is a set of attributes and B is a subset of A, then A → B holds.  
   Example: If `A = {Student_ID, Student_Name}` and `B = {Student_ID}`, then `A → B` holds.
2. **Augmentation:** If we can find B from A, then adding any new attributes to this dependency will not affect the FD.  
   Example: If `A → B`, then `AX → BX` is also valid.

3. **Transitivity:** If A → B and B → C, then A → C will also be true.

## Why Normalisation?

Normalisation is used to avoid redundancy and anomalies in the database.

### Anomalies:

Anomalies are abnormal or non-ideal behaviors arising due to data redundancy in the database.

1. **Insertion Anomaly:** New data cannot be inserted into the database without the presence of other data.
2. **Deletion Anomaly:** Unintended loss of valuable information after deleting some data (e.g., statistics).
3. **Updation Anomaly:** When updating a single data point requires updating multiple rows, and inconsistency may arise if not all rows are updated successfully.

Redundancy increases database size and can cause slow performance due to these anomalies.

## What is Normalisation?

Normalisation is the process of minimizing redundancy in the database and avoiding anomalies like insertion, updation, and deletion anomalies. It involves dividing larger tables into smaller tables and linking them using relationships.

### Types of Normal Forms:

- **Prime Attribute:** An attribute that is part of a candidate key.
- **Non-Prime Attribute:** An attribute that is not part of any candidate key.

---

### 1. First Normal Form (1NF)

- Every attribute value must be atomic (i.e., no arrays or multiple values in a single field).
- The relation must not have multi-valued attributes.

**Example:**
| Student_ID | Student_Name | Courses |
| ---------- | ------------ | ------------------- |
| 101 | John | CS101, CS102 |
| 102 | Alice | CS103, CS104 |

To convert this into 1NF, we split the `Courses` column into individual rows:

| Student_ID | Student_Name | Course_ID |
| ---------- | ------------ | --------- |
| 101        | John         | CS101     |
| 101        | John         | CS102     |
| 102        | Alice        | CS103     |
| 102        | Alice        | CS104     |

---

### 2. Second Normal Form (2NF)

- Must be in 1NF.
- Must not have partial dependency (all prime attributes must fully depend on the primary key).
- Non-prime attributes cannot depend on part of the primary key.

**Explanation:**
Consider a relation **R(A, B, C, D)**, where **A, B, C, D** are attributes.

- Primary key: `{A, B}`
- Prime attributes: `A`, `B`
- Non-prime attributes: `C`, `D`

**Functional Dependencies:**

- `AB -> C` (Fully dependent FD)
- `AB -> D` (Fully dependent FD)
- `B -> C` (Partially dependent FD) — This violates 2NF.

**Problem with Partial Dependency (B → C):**

| Case | A    | B    |
| ---- | ---- | ---- |
| 1    | Null | 1    |
| 2    | 2    | Null |
| 3    | 3    | 4    |

If `B -> C` holds, then in Case 2, `Null` would be used to determine the value of `C`, which is not possible. This problem is resolved by converting to 2NF.

**2NF Conversion:**
To achieve 2NF, decompose the relation as follows:

- **R1(A, B, D):** with `AB -> D`
- **R2(B, C):** with `B -> C`

**Example:**

| Student_ID | Proj_ID | Student Name | Project Name |
| ---------- | ------- | ------------ | ------------ |
| 589        | P09     | Olivia       | Geo          |
| 554        | P07     | Jacob        | Sites        |
| 525        | P02     | Ava          | Iot          |
| 578        | P05     | Alex         | Cloud        |

- Primary Key (P.K): `{Student_ID, Proj_ID}`
- Functional Dependencies (FD): These are partial dependencies:
  - `Student_ID -> Student Name`
  - `Proj_ID -> Project Name`

**2NF Conversion:**
Split the original relation into two tables to remove partial dependencies:

1. **Student Table**  
   Attributes: `Student_ID (PK), Student Name, Proj_ID (FK)`

   | Student_ID | Proj_ID | Student Name |
   | ---------- | ------- | ------------ |
   | 589        | P09     | Olivia       |
   | 554        | P07     | Jacob        |
   | 525        | P02     | Ava          |
   | 578        | P05     | Alex         |

2. **Project Table**  
   Attributes: `Proj_ID (PK), Project Name`

   | Proj_ID | Project Name |
   | ------- | ------------ |
   | P09     | Geo          |
   | P07     | Sites        |
   | P02     | Iot          |
   | P05     | Cloud        |

---

### 3. Third Normal Form (3NF)

- Must be in 2NF.
- Transitivity should not exist (A -> B & B -> C then A -> C).
- Non-prime attributes should not depend on other non-prime attributes.

**Explanation:**
Consider a relation **R(A, B, C)**, where **A, B, C** are attributes.

- Primary key: `{A}`
- Prime attributes: `A`,
- Non-prime attributes: `B` ,`C`

**Functional Dependencies:**

- `A -> B`
- `B -> C`

**Problem:**

Above FD satisfies 2NF.

| A   | B   | C   |
| --- | --- | --- |
| a   | 1   | x   |
| b   | 1   | x   |
| c   | 1   | x   |
| d   | 2   | y   |
| e   | 2   | y   |
| f   | 3   | z   |
| g   | 4   | z   |

In Case: B -> C
Here, A non-prime attribute is able to find non-prime attribute
This has introduced data redundancy, repeated data.

**3NF Conversion:**
To achieve 3NF, decompose the relation as follows:

- **R1(A, B):** with `A -> B`
- **R2(B, C):** with `B -> C`

Split the original table into two tables.

1. Table 1 (R1)

   | A   | B   |
   | --- | --- |
   | a   | 1   |
   | b   | 1   |
   | c   | 1   |
   | d   | 2   |
   | e   | 2   |
   | f   | 3   |
   | g   | 3   |

2. Table 2 (R2)

   | B   | C   |
   | --- | --- |
   | 1   | x   |
   | 2   | y   |
   | 3   | z   |

---

### 4. Boyce-Codd Normal Form (BCNF)

- Must be in 3NF.
- For every functional dependency `A → B`, `A` must be a superkey.
- We must not derive prime attributes from any prime or non-prime attributes.
- A superkey is an attribute or set of attributes that uniquely identifies a record.

**Example:**
Consider a table with the following columns:

| Student_ID | Subject | Professor |
| ---------- | ------- | --------- |
| CS101      | Java    | PJ1       |
| CS101      | CPP     | PC        |
| CS102      | Java    | PJ2       |
| CS103      | C#      | PC#       |
| CS104      | Java    | PJ        |

**Conditions:**

- One student can enroll in multiple subjects.
- For each subject, a professor is assigned to a student.
- Multiple professors can teach a single subject.
- One professor can teach only one subject.

**Relation:**

- **Primary Key:** `{Student_ID, Subject}`
- **Prime Attributes:** `Student_ID`, `Subject`
- **Non-prime Attribute:** `Professor`

**Functional Dependencies:**

1. `{Student_ID, Subject} → Professor`
2. `Professor → Subject`

The functional dependency `Professor → Subject` violates BCNF because `Professor` is not a superkey.

**BCNF Conversion:**
To satisfy BCNF, we split the original relation into two tables:

1. **Student_Professor Table**  
   Attributes: `Student_ID (PK), P_ID (FK)`

   | Student_ID | P_ID |
   | ---------- | ---- |
   | CS101      | P1   |
   | CS101      | P2   |
   | CS102      | P3   |
   | CS103      | P4   |
   | CS104      | P5   |

2. **Professor_Subject Table**  
   Attributes: `P_ID (PK), Professor (Non-Prime), Subject (Non-Prime)`

   | P_ID | Professor | Subject |
   | ---- | --------- | ------- |
   | P1   | PJ1       | Java    |
   | P2   | PC        | CPP     |
   | P3   | PJ2       | Java    |
   | P4   | PC#       | C#      |
   | P5   | PJ        | Java    |

The two tables now satisfy BCNF by eliminating the transitive dependency of `Professor → Subject`.

# Links

https://www.geeksforgeeks.org/normal-forms-in-dbms/
https://www.youtube.com/watch?v=nweGaymEwGM&list=PLDzeHZWIZsTpukecmA2p5rhHM14bl2dHU&index=12
