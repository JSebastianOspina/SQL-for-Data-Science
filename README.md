# SQL-for-Data-Science
UC Davis's SQL for Data Science course notes

# Creating Tables
- Specifiying if the field is nullable or not is required.
- A primary key MUST exist. It can't not be null.

CREATE TABLE table_name 
(
Id int(10) PRIMARY KEY,
name string NOT NULL,
description varchar(100) NULL
)
## Temporary Tables
- They work exactly the same as normal tables. The information will be only avaliable until the SQL sesion is open.
- They are useful when a more complex query is required. 

# Selecting information
- Multiple relational operators are avaliable  >, <, >=, !

The sintaxis is simple. 
SELECT <columnas>
FROM <tabla>
LIMIT <numero de resultados>
  
## Limiting (not filtering) results

The sintaxis is simple. 
SELECT <columnas>
FROM <tabla>
LIMIT <numero de resultados>

## Advanced Filtering: IN, OR, and NOT


# Adding registers.
There are two ways to add register to a SQL database, it can be the short way (this depends on the created order)

INSER INTO nombre_tabla 
VALUES
(
'1',
'Nombre 1',
'Descripcion 1',
);

This specifict way, specify the order of the columns and then the information to ve added
Do not use quotes for the column names, however, the information does require them.

INSER INTO nombre_tabla
(
Id,
name,
description
)
VALUES
(
'1',
'Nombre 1',
'Descripcion 1',
)

