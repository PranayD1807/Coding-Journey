# Indexing in DBMS

Indexing is a method to enhance database performance by reducing the disk accesses required for query processing. Indexes are data structures that allow faster access to data, particularly useful for speeding up SELECT queries with WHERE clauses.

## Key Concepts in Indexing

1. **Search Key**: This is the attribute used for searching, which can be a Primary Key, Candidate Key, or any other attribute.
2. **Data Reference**: A pointer that stores the location (disk address) of the data related to a particular search key.

**Note:** Index files are always sorted for efficient searching.

## Indexing Methods

### Primary Index

A primary index is also known as a clustering index. It can be used only if the data in the file is sorted based on a specific attribute, often referred to as the search key of the primary index. Importantly, the primary index does not have to be created on the Primary Key attribute; any sorted attribute in the data file can be used.

#### Types of Primary Indexing

1. **Dense Indexing**:

   - Contains an index record for every search key value in the data file.
   - Each index record stores the address of the first occurrence of the search key in the data file, with all subsequent records stored sequentially.
   - _Example_: In a database sorted by "Employee ID", a dense index would store an index entry for every unique Employee ID, pointing to the exact data record.
   - **Drawback**: Requires more storage space.

2. **Sparse Indexing**:
   - Stores only a subset of search key values.
   - Reduces space requirements compared to dense indexing.
   - Contains an index entry for only some of the records, each pointing to a block rather than a single record.
   - _Example_: In the same employee database, sparse indexing might have an entry only for the first Employee ID in each block of records.
   - **Advantage**: Optimizes storage and speed by reducing the index size.

#### Primary Index Based on Key Attribute

- The data file is sorted on the primary key.
- Sparse indexing is typically used in this scenario.
- The number of index entries equals the number of blocks in the data file.

#### Primary Index Based on Non-Key Attribute

- The data file is sorted based on a non-key attribute (e.g., department).
- Dense indexing is used, as duplicate values of the non-key attribute may exist.
- The number of index entries equals the number of unique non-key values.

#### Multi-Level Indexing

When a single-level index becomes too large, multi-level indexing can be used to break it down into multiple levels, making searches more efficient.

- _Example_: A three-level index may contain a root level, an intermediate level, and a bottom level that points to the actual data records.

![Multi Level Indexing](<https://media.geeksforgeeks.org/wp-content/uploads/20230620132941/ezgifcom-gif-maker-(4).webp>)

### Secondary Index

Secondary indexes are used when the data file is unsorted, making primary indexing impractical. Secondary indexes can be created on both key and non-key attributes.

- Generally, primary indexing is applied first, and then secondary indexing can be added.
- Secondary indexes are typically dense, with the number of index entries equal to the number of records in the data file.

_Example_: In a database with an unsorted "Employee Name" column, a secondary index could be created on the "Department" attribute to enable quick lookups by department.

## Advantages of Indexing

- Faster access and retrieval of data, especially for search-heavy queries.
- Reduces the number of I/O operations.

## Limitations of Indexing

- Indexes require additional storage space.
- Performance decreases for INSERT, DELETE, and UPDATE operations due to the need to update the index structure.
