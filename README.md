# clickhouseissue1

having a weird issue with Clickhosue when i filter by pfid_productname there are 7 Rows
but the moment i add a VendorCode filter it returns only 1 rows

But if i create a copy of the table to another table let say INSERT INTO inventory2 Select * from inventory1 then same queries below works fine and it returns 7 rows for both filters

Can anybody guide me or help me to resolve this issue ?

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
