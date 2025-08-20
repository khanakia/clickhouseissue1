# clickhouseissue1

I'm experiencing a strange issue with ClickHouse. When I filter my table by pfid_productname, I get 7 rows as expected. But as soon as I add a VendorCode filter, the query suddenly returns only 1 row.

Here’s where it gets weirder: if I make an exact copy of the table (using `INSERT INTO inventory2 SELECT * FROM inventory1`), and run the same queries on this new table, both filters together return all 7 rows as they should.

Has anyone run into this before, or can anyone explain why this is happening? Any help would be really appreciated!

```sql
create table inventory1
(
    StoreInventoryID          Int64,
    SKU                       Nullable(Int64),
    OnHand                    Nullable(Int64),
    VendorCode                Nullable(String),
    pfid_productname          Nullable(String),
    event_time                DateTime,
    is_deleted                UInt8
)
    engine = ReplacingMergeTree(event_time, is_deleted)
        PRIMARY KEY StoreInventoryID
        ORDER BY StoreInventoryID
        SETTINGS allow_experimental_replacing_merge_with_cleanup = 1, index_granularity = 8192;
```

```sql
Select * from inventory1 si where pfid_productname='test';
Select * where pfid_productname='test' and VendorCode='2332';
```

Also the screenshots from queries
<img width="1455" height="124" alt="Screenshot 2025-08-20 at 6 38 34 PM" src="https://github.com/user-attachments/assets/5ebb918d-9b1e-4c23-bba8-6d15b4a4dfb6" />


<img width="1422" height="287" alt="Screenshot 2025-08-20 at 6 38 42 PM" src="https://github.com/user-attachments/assets/972a2560-a3b2-4e3b-96ca-d20adb188858" />
