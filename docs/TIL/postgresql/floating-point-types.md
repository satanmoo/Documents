# Floating-Point Types

- [original-docs](https://www.postgresql.org/docs/7.4/datatype.html#DATATYPE-FLOAT)

> The data types real and double precision are inexact, variable-precision numeric types. In practice, these types are
> usually implementations of IEEE Standard 754 for Binary Floating-Point Arithmetic (single and double precision,
> respectively), to the extent that the underlying processor, operating system, and compiler support it.

- `real` and `double precision` are inexact
    - reason 1:
        - implementations of IEEE Standard 754
    - reason 2:
        - platform(processor, os, compiler)
- This paragraph is intended to inform you cautions and disclaimers.

> Inexact means that some values cannot be converted exactly to the internal format and are stored as approximations, so
> that storing and printing back out a value may show slight discrepancies. Managing these errors and how they propagate
> through calculations is the subject of an entire branch of mathematics and computer science and will not be discussed
> further here, except for the following points:

- `internal format` means IEEE Standard 754 implementation.

> PostgreSQL also supports the SQL-standard notations float and float(p) for specifying inexact numeric types. Here, p
> specifies the minimum acceptable precision in binary digits. PostgreSQL accepts float(1) to float(24) as selecting the
> real type, while float(25) to float(53) select double precision. Values of p outside the allowed range draw an error.
> float with no precision specified is taken to mean double precision.

- In `float(p)` notation, the parentheses are a formality to define the type more specifically. It acts as a type
  parameter.

- We can check the actual postgresql data type with the test below.

```text
CREATE TABLE test (
    tc float(20) not null,
);
```

```text
SELECT 
    column_name,
    data_type,
    numeric_precision
FROM 
    information_schema.columns
WHERE 
    table_name = 'test';
```

- output:

| column_name | data_type | numeric_precision |
|-------------|-----------|-------------------|
| an          | real      | 24                |
