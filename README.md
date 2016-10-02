# `myd` Query MySQL with the concision of Tutorial D

`myd` is a Python library providing a Tutorial D-like interface into MySQL.

**NOTE** `myd` is currently in the planning & proof of concept stages. _Id est_, no working code yet.

The elegant semantics of [Tutorial D][ThirdManifestoBooks] make ad-hoc queries easier to write than they would be in SQL. That can make this library nice for doing production support on a large system, where you often have to query dozens of tables across multiple databases to understand how an entity has broken.

The name `myd` is short for `MySQL D` or `MySQL via Tutorial D`, but the name is a work in progress.

Note that this interface is _inspired by_ Tutorial D, but it is **not** an implementation of the Tutorial D language.

# Example

Assuming we have defined our `Part`, `Supplier`, and `Inventory` relations already (we'll show you how later):

    >>> print((Part & Supplier(supplier_id=9001) & Inventory).sql)
    SELECT
        part_part.id AS 'part_id'
        , part_part.name
        , supplier_supplier.id AS 'supplier_id'
        , supplier_supplier.name
        , supplier_supplier.url
        , inventory_item.quantity
    FROM
        (SELECT
            9001 AS 'supplier_id'
        ) _abc123
            JOIN
        parts_db.part_part
            JOIN
        supplier_db.supplier_supplier
            ON
                supplier_supplier.supplier_id = _abc123.supplier_id
            JOIN
        supplier_db.inventory_item
            ON
                inventory_item.part_id = part_part.id
                AND inventory_item.supplier_id = supplier_supplier.id
    ;

Tutorial D saved us a lot of typing. Witness truly relational operators at work.

The mappings are not so bad either:

    >>> from myd import Mapping
    >>> Part = Mapping(
    ...     database='parts_db',
    ...     table='part_part',
    ...     renames={
    ...         'part_id': 'id',
    ...     },
    ... )
    >>> Supplier = Mapping(
    ...     database='supplier_db',
    ...     table='supplier_supplier',
    ...     renames={
    ...       'supplier_id': 'id',
    ...     },
    ... )
    >>> Inventory = Mapping(
    ...     database='supplier_db',
    ...     table='invetory_item',
    ...     renames={
    ...         'inventory_id': 'id',
    ...     },
    ... )

You don't have to bother defining the column names that you're not renaming; `myd` will inspect your database and infer your columns automatically.

# Getting Started

TODO step through an increasingly complex sequence of examples.

- TODO show how relational composition combines with variables to make reusable parts
- TODO show how `REMOVE` makes for clutter-free projections w/ low syntax overhead
- TODO show `Mapping(..., views={...})` and how `myd's` `views` simplify awkward data models

# Relational Operators

TODO review each relational operator from Tutorial D; document our Python syntax for it; and show an example.

Note: Some of these won't translate to SQL, so we'll have to fudge it. Maybe apply them in-memory to queried results if there really, truly is no way to translate an expression? We'll see.

## a JOIN b

## a UNION b

## RELATION { TUPLE { ks }, ... }

## a PROJECT ks

## a REMOVE ks

## EXTEND a ADD e AS k

## a WHERE p

## a MINUS b

## a INTERSECT b

## WITH e r

## a RENAME ks AS js

## SUMMARIZE a PER b ADD e AS k

## a GROUP ks AS k

## a UNGROUP k

## EXPAND a

## COLLAPSE a

## PACK r ON a

## UNPACK r ON a

## USING ks ◀︎ a MINUS b ▶

## USING ks ◀︎ a UNION b ▶

## USING ks ◀︎ a INTERSECT b ▶

## USING ks ◀︎ a WHERE p ▶

## USING ks ◀︎ a PROJECT ks ▶

## USING ks ◀︎ a JOIN b ▶

## USING ks ◀︎ EXTEND a ADD e AS k ▶

## USING ks ◀︎ a GROUP ks AS k ▶

## USING ks ◀︎ a UNGROUP k ▶

# Other Relational Libraries

- [Dee](https://github.com/ggaughan/dee)
- [relational.js](https://github.com/erikolson186/relational.js)
- [The Third Manifesto projects page](http://www.dcs.warwick.ac.uk/~hugh/TTM/projects.html)

[ThirdManifestoBooks]: http://www.dcs.warwick.ac.uk/~hugh/TTM/documents_and_books.html
