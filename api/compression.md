## Compression <tag type="community">Community</tag>

We highly recommend reading the [blog post][blog-compression] and
[tutorial][using-compression] about compression before trying to set it up
for the first time.

Setting up compression on TimescaleDB requires users to first [configure the
hypertable for compression](/compression/alter_table_compression/) and then [set up a
policy](/compression/add_compression_policy/) for when to compress chunks.

Advanced usage of compression allows users to [compress chunks
manually](/compression/compress_chunk), instead of automatically as they age.

### Restrictions

The current version does not fully support altering or inserting data into
compressed chunks. The data can be queried without any modifications, however
if you need to backfill or update data in a compressed chunk you will need to
decompress the chunk(s) first.

Starting with TimescaleDB 2.1, users have the ability to modify the schema
of hypertables that have compressed chunks.
Specifically, you can add columns to and rename existing columns of 
such compressed hypertables.

Starting with TimescaleDB 2.3, users have the ability to insert data into
compressed chunks, and enable compression policies on distributed hypertables.

Altering data of compressed chunks still has some limitations:
  - You cannot execute `UPDATE` or `DELETE` statements on compressed chunks.
  - `INSERT` is not fully supported on compressed chunks:
    - you cannot use the `ON CONFLICT` clause (upserts).
    - you cannot use the `OVERRIDING` clause.
    - you cannot use the `RETURNING` clause.
  - Triggers are not fully supported when inserting into compressed chunks:
    - you cannot use `AFTER INSERT` row-level triggers (`FOR EACH ROW`).
  - Constraints are not fully supported:
    - Unique constraints (`UNIQUE`) are restricted to compression `SEGMENTBY`
      and `ORDER BY` columns.
    - Primary Keys (`PRIMARY KEY`) are restricted to compression `SEGMENTBY`
      and `ORDER BY` columns.
    - All other types of constraints are restricted to compression `SEGMENTBY`
     columns when applicable. Otherwise they are unsupported.
  - `ROW LEVEL SECURITY` is not supported.
