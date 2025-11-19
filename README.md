PL/SQL Inventory Management Demonstration
This project demonstrates core concepts in Oracle PL/SQL, focusing on dynamic data manipulation, in-memory data structures, and procedural control flow using two primary stored procedures.

1. Data Creation and Initialization (setup_inventory_simple)
This procedure is responsible for setting up the necessary database structure and populating it with initial data.

Dynamic SQL for DDL and DML
The key concept used here is Dynamic SQL, facilitated by the EXECUTE IMMEDIATE command. This is essential because standard PL/SQL procedures cannot directly execute Data Definition Language (DDL) commands like CREATE TABLE.

Handling DDL: To create the permanent inventory table from within the procedure, the CREATE TABLE statement must be executed dynamically as a string.
Handling DML Visibility: Since the DDL command performs an implicit COMMIT, the new table structure is committed to the database. However, for immediate data insertion (DML) into that newly created table within the same procedural block, the INSERT statements must also be executed dynamically. This forces the SQL engine to re-parse and recognize the new table at runtime, preventing the "table or view does not exist" error.
2. Data Structures and Processing (process_inventory_data)
This procedure focuses on retrieving, structuring, and processing the inventory data in memory using advanced PL/SQL types and control statements.

PL/SQL Record and Collection
This section utilizes in-memory data structures to handle multiple rows of data efficiently.

PL/SQL Record: A Record is a single, structured variable that acts as a template for a row of data, holding multiple related fields (e.g., item_name and stock_level) in memory. It's used to define the structure of the row we fetch from the database.
PL/SQL Collection (Associative Array): A Collection is a flexible, variable-sized array capable of storing multiple instances of the defined Record. In this demonstration, an Associative Array (indexed by an integer) is used to hold the entire result set of the inventory query in memory.
Bulk Data Retrieval
BULK COLLECT: Instead of fetching one row at a time, the SELECT statement uses BULK COLLECT to retrieve all relevant inventory data rows and populate the entire PL/SQL Collection in a single operation. This drastically reduces the number of calls between the SQL and PL/SQL engines, leading to significant performance gains.
Control Flow: GOTO
GOTO Statement: The procedure iterates through the Collection and uses a GOTO statement to explicitly control the flow of execution within the loop. When a specific condition is met (e.g., low stock for an item), GOTO immediately jumps execution to a predefined label (<<next_item_label>>), skipping the remaining code in the loop iteration for that particular record.
