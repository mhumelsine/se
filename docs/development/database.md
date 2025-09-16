# Database
- Use `DateTime2` for all new development.  It uses the same storage as `DateTime` but provides more precision. 
- Use lower case, snake case.  Outside windows, most databases are case sensitive and many drivers/adapters will normalize casing to prevent bugs so pascel and camel case `AuditLastModifiedOn` becomes `auditlastmodifiedon` where lower snake would be `audit_last_modified_on`, clearly more readible.  Use a package like [EF Core Naming Conventions](https://github.com/efcore/EFCore.NamingConventions) to make life easier.
- If you use non-sequential clustered indexes, make sure you use a fill-factor less than 100%.  Example, you use `Guid` as a primary key/clustered index, you should use 80-90% fill factor and rebuild indexes if fragmented > 1%.  Sequential clustered indexes can use 100% fill factor.
- Be aware of expansive updates.  If the data is filled to 100% with the status column of `PENDING` and you update it to `PROCESSING COMPLETE`, that will cause a physical data move/copy to make room for the larger data.  The best approach is used fixed size columns for things you expect to change `char(1)` of `'P'` -> `'C'` or int/enum of `0` -> `1` do not change the size of data and do not cause expansive updates.  You can always join if you need descriptions.
- Write all scripts to be idempotent or use the `--idempotent` flag if generated using Entity Framework:
    ```sql
    drop index if exists idx_table_1__column_1 on dbo.table_1;

    --or

    if not exists (
        select *
        from information_schema.columns
        where table_name = 'table_1'
        and table_schema = 'dbo'
        and column_name = 'column_1'
    )
    begin
        alter table dbo.table_1 add column_1 int null;
    end
    ```
- Always schema qualify statements; e.g. `dbo.table_1` not `table_1`.
- Be mindful of large object storage, anything over 8kb, including `varchar(max)` and `varbinary(max)`.  There is no single prescription.  In-row will be faster but could cause expansive updates.  You may want to move to a separate partition to help with backups and indexing. 
- Use named constraints and not generated ones.  Generated constraints will not match between environments and make testing scripts more difficult.
```sql
-- bad; generates something like PK_id4320854032
create table dbo.table_1(
    id int not null primary key identity
);

--good; predictable name
create table dbo.table_1(
    id int not null identity,

    constraint pk_dbo_table_1__id primary key
);

```