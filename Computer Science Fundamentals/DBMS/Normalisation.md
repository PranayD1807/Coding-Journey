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

**Example:**
Consider a table with `Student_ID`, `Course_ID`, and `Instructor`:

| Student_ID | Course_ID | Instructor | Instructor_Phone |
| ---------- | --------- | ---------- | ---------------- |
| 101        | CS101     | Dr. Smith  | 123-4567         |
| 102        | CS102     | Dr. Clark  | 234-5678         |

The primary key is a combination of `Student_ID` and `Course_ID`, but `Instructor_Phone` depends only on `Instructor`, not the entire primary key. To convert to 2NF, we split the table:

**Student_Courses Table:**
| Student_ID | Course_ID | Instructor |
| ---------- | --------- | ---------- |
| 101 | CS101 | Dr. Smith |
| 102 | CS102 | Dr. Clark |

**Instructor_Phone Table:**
| Instructor | Instructor_Phone |
| ----------- | ---------------- |
| Dr. Smith | 123-4567 |
| Dr. Clark | 234-5678 |

---

### 3. Third Normal Form (3NF)

- Must be in 2NF.
- Non-prime attributes should not depend on other non-prime attributes.

**Example:**
| Student_ID | Student_Name | Course_ID | Instructor | Instructor_Phone |
| ---------- | ------------ | --------- | ---------- | ---------------- |
| 101 | John | CS101 | Dr. Smith | 123-4567 |
| 102 | Alice | CS102 | Dr. Clark | 234-5678 |

Here, `Instructor_Phone` is dependent on `Instructor`, which is a non-prime attribute. To convert to 3NF, we split the table as follows:

**Student_Courses Table:**
| Student_ID | Course_ID | Instructor |
| ---------- | --------- | ---------- |
| 101 | CS101 | Dr. Smith |
| 102 | CS102 | Dr. Clark |

**Instructor_Phone Table:**
| Instructor | Instructor_Phone |
| ----------- | ---------------- |
| Dr. Smith | 123-4567 |
| Dr. Clark | 234-5678 |

---

### 4. Boyce-Codd Normal Form (BCNF)

- Must be in 3NF.
- For every functional dependency `A → B`, A must be a superkey.
- A superkey is an attribute or set of attributes that uniquely identifies a record.

**Example:**
Consider a table with the following columns:

| Course_ID | Instructor | Instructor_Phone |
| --------- | ---------- | ---------------- |
| CS101     | Dr. Smith  | 123-4567         |
| CS102     | Dr. Clark  | 234-5678         |

In this case, `Instructor → Instructor_Phone`, but `Instructor` is not a superkey. To convert this table into BCNF, we need to split it:

**Course Table:**
| Course_ID | Instructor |
| --------- | ----------- |
| CS101 | Dr. Smith |
| CS102 | Dr. Clark |

**Instructor_Phone Table:**
| Instructor | Instructor_Phone |
| ----------- | ---------------- |
| Dr. Smith | 123-4567 |
| Dr. Clark | 234-5678 |

This ensures that all functional dependencies have a superkey as the determinant, satisfying BCNF.
