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

# REST API
- Routes should never contain verbs or action words (unless there is an entity named that way).  
- APIs should always be versioned.
- Publish API docs, like Open API and fully document parameters and operations.
- Use POST for read requests if there is sensitive data.  `GET` requests are cachable by proxies and other internest appliances.  If the URI contains sensitive data, if may exist in logs or cache several places, HTTP spec allows for using POST, where the request is contained in the body, for this purpose.

## Bad Examples
```
GET: /products/GetById/{id}
GET: /products/search
POST: /products/Update
```

## Good Examples
```
GET: /v1/products/{id} (read)
GET: /v1/products?name=searchTerm (nonsenstive search)
PUT: /v1/products/{id} (update)
POST: /v1/products/list (sensitive search)
POST: /v1/products (create)
```

- Use informative response codes

### Common Success Codes

- 200 - processed successful and contains response body.
- 201 - entity was created.
- 202 - request accepted/validated and queued for processing.
- 204 - processed successfully but no response body

#### Scenarios
- `PUT: /v1/products/13` -> `204`  You replaced the entity on the server with entity in the request body.  No need to return the entity, the client already has the latest copy.
- `POST: /v1/products` -> `201` You created an entity in response to the request.  Return the created entity and a `location` header where the client and send a `GET` to read it.
- `POST: /v1/products` -> `202` Request was validated and accepted but is queued to be processed by a background service.  It is also common to return a status check URI when returning a 202.

# Authentication
- Use API keys, personal access tokens, and other "magic string" credentials sparingly.  Should be used only when the scope of permissions is very small, signatures are rotated, and both systems are trusted.  In all cases, authorization policies must limit access to the minimum set of allowable actions. Examples:
    - Producer sends data to a single endpoint.
    - Consumer of a webhook or periodic poll to a single endpoint.
    - Both systems are trusted and keys are stored in secret storage like Azure Key Vault or Kubernetes Secrets.
    - Other forms of authentication are not supported by both platforms.
- Use an identity platform if possible, fallback to a trusted identity management library, such as identity server, when that is not possible.




# Versioning 
- Follow [semantic versioning](https://semver.org/).
- Development builds use the scheme `0.0.0-{yyyy.MM.dd}+{buildId}` ex: `0.0.0-2025.01.01+4501`
- Release builds use the scheme `{major}.{minor}.{patch}-{yyyy.MM.dd}+{buildId}` ex: `5.0.1-2025.02.12+3212`
- The build date and build Id allow teams to quickly check if newer builds have been published since QA was perfomred and to confirm the latest code is published.