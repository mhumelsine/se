<div align="center">
  <h1>Software Engineering Playbooks</h1>
  <p>
    Templates to bootstrap projects and give you guidance.
  </p>
</div>

<!-- Table of Contents -->

# :notebook_with_decorative_cover: Table of Contents

- [About the Project](#star2-about-the-project)
- [Getting Started](#toolbox-getting-started)
    - [Development](#bangbang-prerequisites)
        - [Standards](#straight_ruler-standards)
    * [Repository Structure](#file_cabinet-repository-structure)
    * [Architecture](#building_construction-architecture)
    * [General Coding Standards](#triangular_ruler-general-coding-standards)
    * [Client/Front-end Standards](#newspaper-clientfront-end-standards)
    * [Server/Back-end Standards](#file_cabinet-serverback-end-standards)
    * [Database Standards](#floppy_disk-database-standards)
- [SDLC](#card_index_dividers-sdlc)
    * [Source Code Management](#books-source-code-management)
    * [Environments](#cityscape-environments)
    * [CI/CD](#station-cicd)

<!-- About the Project -->

## :star2: About the Project

<!-- TechStack -->

### :space_invader: Tech Stack

#### Client

  <ul>
    <li><a href="https://www.typescriptlang.org/">Typescript</a></li>
    <li><a href="https://reactjs.org/">React.js</a></li>
    <li><a href="https://redux.js.org/">Redux.js</a></li>
    <li><a href="https://getbootstrap.com/">Bootstrap CSS</a></li>
  </ul>

#### Server

  <ul> 
    <li><a href="https://dotnet.microsoft.com/en-us/apps/aspnet/apis">ASP.NET Web API</a></li>
    <li><a href="https://github.com/DapperLib/Dapper">Dapper</a></li>
    <li><a href="https://masstransit.io/">Mass Transit</a></li>
    <li><a href="https://learn.microsoft.com/en-us/ef/core/">Entity Framework Core</a></li>
  </ul>

#### Database

  <ul>
    <li><a href="https://www.microsoft.com/en-us/sql-server">Microsoft SQL Server</a></li>
    <li><a href="https://azure.microsoft.com/en-us/products/cosmos-db">Cosmos DB</a></li>
  </ul>

#### DevOps

  <ul>
    <li><a href="https://azure.microsoft.com/en-us/products/devops">Azure DevOps</a></li>
  </ul>

<!-- Major Features -->

### :dart: Features

- Feature 1
- Feature 2
- Feature 3

<!-- Color Reference -->

### :art: Color Reference

| Color           | Hex                                                                |
|-----------------|--------------------------------------------------------------------|
| Primary Color   | ![#00B2C5](https://via.placeholder.com/10/00B2C5?text=+) `#00B2C5` |
| Secondary Color | ![#2B3D51](https://via.placeholder.com/10/2B3D51?text=+) `#2B3D51` |
| Accent Color    | ![#ff9c00](https://via.placeholder.com/10/ff9c00?text=+) `#ff9c00` |
| Text Color      | ![#000000](https://via.placeholder.com/10/000000?text=+) `#000000` |

### :control_knobs: Configuration

Secrets are stored in the environment using environment variables.  
For AKS deployments these are environment variables passed directly to the container on start.  
For IIS deployments, the environment variables are set in the `web.config` file, which is not tracked in source control
and is not deployed.

Application settings are stored in `appsettings.json`. Environment specific configuration files exist in the format:
`appsettings.{env}.json`. The `ASPNETCORE_ENVIRONMENT` environment variable is used to control which app settings file
will take effect.

<!-- Env Variables -->

### :key: Environment Variables

To run this project, you will need to add the following environment variables to your .env file

| Name                                   | Purpose                                                                                                                | Default                 |
|----------------------------------------|------------------------------------------------------------------------------------------------------------------------|-------------------------|
| `AzureAd:ClientSecret`                 | Client secret authenicating the application to connect to Azure resources                                              | secret                  |,
| `ASPNETCORE_ENVIRONMENT`               | Sets the environment the application is running as.  Selects the configuration for that environment                    | Development             |
| `Jwt:Key`                              | The private key used to sign JWT tokens. Secret stored in each environment.                                            | Any 32-character string |
| `ServiceBusMessageEncryptionKey`       | Key used to encrypt messages sent over service bux to protect the message content.                                     | secret                  |
| `Cosmos:Key`                           | Secret used for authenticating to Cosmos DB                                                                            | secret                  |
| `SasUser`                              | User ID of the account used for private, service to service (REST) calls                                               | AutoBot                 |
| `SasKey`                               | The private signing key used to sign SAS (shared access signature) tokens to perform service to service authentication | secret                  |
| `EntraAccessTokenOptions:ClientSecret` | Will match `AzureAd:ClientSecret` but for a separate client library that uses a different key name.                    | secret                  |
| `PATH_BASE`                            | Configures the path prefix for when the application is hosted on a sub directory or path other than root.              | /MerlinCore             |

### :locked: Authentication

| Type                      | Information                                                                                                    |
|---------------------------|----------------------------------------------------------------------------------------------------------------|
| Entra ID (OpenID Connect) | Identity provider                                                                                              |
| Bearer Token              | JWT access token issued by the application.  Requires a validated id token from Entra ID or a valid SAS token. |
| SAS Token                 | For service to service calls, SAS tokens are used to obtain an access token                                    |
| Cookie Authentication     | Used to pass authentication to Merlin proper (legacy app) and handle redirects to the application              |

<!-- Getting Started -->

## :toolbox: Getting Started

<!-- Prerequisites -->

### :bangbang: Prerequisites

<ul> 
  <li><a href="https://git-scm.com/download/win">Git</a></li>
  <li><a href="https://dotnet.microsoft.com/en-us/download">dotnet 8 SDK</a></li>
  <li><a href="https://nodejs.org/en/download">Node.js</a></li>
</ul>

<!-- Run Locally -->

### :running: Run Locally

Clone the project

```bash
git clone https://dev.azure.com/isf-merlin/Merlin/_git/merlin-core
```

Go to the project directory

```bash
cd merlin-core
```

Install dependencies

```bash
dotnet restore
cd Merlin.Web/client-app
npm install -f --legacy-peer-deps
```

Start the application (ensure you are in the project root folder)

```bash
dotnet run --project Merlin.Web/Merlin.Web.csproj
```

Note:  You can also start via your IDE.

<!-- Running Tests -->

### :test_tube: Running Tests

To run tests, run the following command

```bash
dotnet test
```

<!-- Deployment -->

### :triangular_flag_on_post: Deployment

To deploy this project run

```bash
dotnet publish -c Release -o dist
```

Note:  All deployments are preformed through CI/CD described in the [CI/CD section](#cicd).

### :floppy_disk: Database

| Database name  | schema     | Description                                                                                              |
|----------------|------------|----------------------------------------------------------------------------------------------------------|
| MERLIN         | dbo        | Primary database                                                                                         |
| MERLIN         | case       | New case related functionality                                                                           |
| MERLIN         | ecr        | ECR related functionality                                                                                |
| MERLIN         | AutoImport | auto import related functionality                                                                        |
| ELR            | dbo        | We do not maintain this database, owned by another system but we get electronic lab reports from here.   |
| ECR            | dbo        | We do not maintain this database, owned by another system, but we get electronic case reports from here. |
| MerlinDataLake | dbo        | Time-series data to feed data warehouse and handle ad-hoc analysis                                       |
| MerlinDW       | dbo        | Star-schema data warehouse to support analytics                                                          |

### :identification_card: Setting up an Account

- Find an existing admin user without an email address and with a DT_END of null and take note of the ID_USER column (
  pick one):

```sql
SELECT *
FROM dbo.EPI_USER
WHERE DS_EMAIL = ''
  AND DT_END is null
  AND cd_access = 'ADMIN'
```

- Add your email to the user you picked:

```sql
UPDATE dbo.EPI_USER
SET DS_EMAIL = @yourEmail
where ID_USER = @selectedUserId
```

- Run the 3 scripts below to update your new user’s Roles, Privileges (for security) and Preferences; GOODINK is
  Garrett’s user and should have most things you’ll need

```sql
declare @userId varchar(8) = 'youruserid';

insert into dbo.EPI_USER_PRIVILEGES(ID_USER, CD_TYPE, DS_PRIVILEGES_TYPE)
select @userId as id_user, cd_type, DS_PRIVILEGES_TYPE
from dbo.EPI_USER_PRIVILEGES
where ID_USER = 'GOODINK'
  and DS_PRIVILEGES_TYPE not in (select DS_PRIVILEGES_TYPE
                                 from dbo.EPI_USER_PRIVILEGES
                                 where ID_USER = @userId)

insert into dbo.USER_ROLE(ID_USER, ID_CODE)
select @userId as id_user, id_code
from dbo.USER_ROLE
where ID_USER = 'GOODINK'
  and ID_CODE not in (select ID_CODE
                      from dbo.USER_ROLE
                      where ID_USER = @userId)

IF NOT EXISTS (select *
               from dbo.USER_PREFERENCES
               where id_user = @userId)
    BEGIN
        insert into dbo.USER_PREFERENCES(id_user, js_preference, id_added, dt_added)
        select @userId as id_user, js_preference, 'GOODINK', GETDATE()
        from dbo.USER_PREFERENCES
        where id_user = 'GOODINK';
    END; 
```

<!-- Standards -->

## :straight_ruler: Standards

### :file_cabinet: Repository Structure

```bash
# example apps, demos, examples of how to interact with the application 
- demos
# scripts, kubernetes, docker, bicep, IaC, or other artifacts required to deploy the application
- deploy
# contains md files that document design choices, how to run the application, or other required documentation
- docs
# required artifacts to support CI/CD on supported platforms
- pipelines
# source code that makes up the application
- src
# unit and integration tests
- tests
- .gitignore
- .dockerignore
- azurepipelines-coverage.yml
- Solution.Sln
- README.md
```

### :building_construction: Architecture

#### API Logical Architecture

![api-logical-architecture.png](docs%2Fimg%2Fapi-logical-architecture.png)

#### Current Physical Architecture

![current-physical-architecture.png](docs%2Fimg%2Fcurrent-physical-architecture.png)

#### Future Physical Architecture

![future-physical-architecture.png](docs%2Fimg%2Ffuture-physical-architecture.png)

#### Full Stack Interactions

[Full stack interaction diagram](docs%2Fimg%2FFullStackMerlinExample.pdf)

### :triangular_ruler: General Coding Standards

- Optimize for developers first, machines second.
- Write declaritive, easy to reason about code.
- Use a functional approach when available. Functional approaches remove state mutation, to cause of many race
  conditions and bugs and makes the code easier to reason about.
- When using method chaining, place each method call on its own line.
- Avoid nested conditionals. Avoid walls of conditionals. Use your judgement, if it is easy to see what is going on, it
  is probably OK. If it is not obvious, comment or refactor.
- Avoid long methods. Prefer using short methods that label activities so the code your teammates have to read is short
  and easy to get a handle on. For complex processes, consider making each step a separate method.

```
public void Execute(Command command)
{
    UpdateNote(command);
    UpdateLab(command);
    UpdateCase(command);
    UpdateTaskList(command);
}
```

Developers coming behind you can ignore large portions of the code that are out of scope of the changes they are working
on. When in a larger method, they are forced to understand it all and worry about how their changes may cause unintended
consequences.

- Use the best data type / algorithm for the problem. Check:  [Big O Cheatsheet](https://www.bigocheatsheet.com/).

#### Bad Example

```
class FooSummary 
{
    string Name { get; set; }
    decimal Value { get; set; }
}

List<Foo> unfilteredFoos = new List<Foo>();
List<Foo> filteredFoos = new List<Foo>();

for(int i = 0; i < unfilteredFoos.Count; i++)
{
    if(unfilteredFoos[i].Name.StartsWith("Example"))
    {
        if(filteredFoos.Where(f => f.Name == unfilteredFos[i].Name).FirstOrDefault() == null)
        {
            filteredFoos.Add(unfilteredFoos[i]);

            if(filteredFoos.Count == 15)
            {
                break;
            }
        }
    }
}

List<FooSummary> topTenFoos = new List<FooSummary>();

for(int i = 4; i < Math.Min(15, filteredFoos.Count - 5); i++)
{
    topTenFoos.Add(new FooSummary 
    {
        Name = filteredFoos[i].Name,
        Value = SomeCalculation(filteredFoos[i])
    });
}

return topTenFoos;
```

This example has about everything we want to avoid. First, we loop using `for`, while very performant, it sucks to
read.  `foreach` is better, but still suffers the same issues. We have nested conditionals. We want to look for `Foos`
that have a name starting with "Example" avoid adding duplicate search results so we use Linq to check if there are any
existing matches in the list. If there are, make sure that `Name` isn't already in the results. After all are found, we
want to skip the first 5 results, then show the next 10 results. If you look closely at the code, how easy is it to very
that code does all of those things?

#### Good Example

```
List<Foo> unfilteredFoos = new List<Foo>();

return unfilteredFoos
    .Where(foo => foo.Name.StartsWith("Example"))
    .Distinct()
    .Skip(5)
    .Take(10)
    .Select(foo => new 
    {
        Name = foo.Name,
        Value = SomeCalculation(foo)
    });
```

We have chained methods on their own line. A declaritive, functional approach. All shared state variables (lists in the
above example) are completely gone. No possibility of indexing errors. The meaning of the skip 5, show next 10 is clear.
Your teammates can read this in one pass and understand the meaning. The above example may execute faster, but is much
more difficult to read and understand.

### :newspaper: Client/Front-end Standards

- Menu components are suffixed with `Menu`.
- Page level components are suffixed with `Page`.
- Components should be function components
- Event handlers declared inside the function component should be named `handle[EventName]`, e.g. `handleClick`
  or `handleSubmit`.
- Event handlers passed as props are named `on[EventName]`, e.g. `onClick` or `onSave`.
- Functions declared inside function component should use the correct memoization hook (`useCallback`, `useMemo`, etc.)
  or comment as to why.
    - `useMemo` when expensive calculation and/or there will be many renders and the computed value will not change
      often.
    - `useCallback` used by default for event handlers. This is not necessarily the "best" solution, but the easiest to
      follow.
    - Sometimes the cost of memoizing the function isn't worth the benefit. For our app, that is more the exception, but
      it is OK to skip this when you know it to be the case, just should document that it was a choice and the
      reasoning.
- `useState` for simple state changes.  `useReducer` to minimize re-renders and ensure changes become visible at the
  same time.
- Do not include stable functions in the dependency array of `useCallback`.  `dispatch`, `setState`, and static
  functions should not be included.
- Folders should be lower case. Word separators should use, e.g. `-` `task-list`. This is to avoid issues with case
  sensitive file systems.
- Typescript (`.ts`) files with multiple defintions (types, functions) should be lower case (ex: `utils.ts`).
- Typescript (`.ts`) files that contain a single object constructor (class) should be uppercase (ex: `Paging.ts`).
- Typescript JSX (`.tsx`) files should be upper case. React requires components to start with an uppercase letter.
- No use of `any` type. Exceptions should be rare and must be documented.
- State management:
    - Default to component state using the `useState` hook.
    - Move to component state using the `useReducer` hook when:
        - 3 or more simultaneous updates or
        - State updates must occur at the same time or
        - A stable function reference is required, e.g. `dispatch` does not need to be in a dependency array
          but `setState({...oldState, key: value})` does and can cause cascading re-renders in child components if part
          of the child's props.
    - Use Redux when the state should be remembered when the user revisits the page.
    - Redux when state needs to be shared between components that are not part of the same tree (do not share a parent)
      or, the implementation can be simplified by using Redux for state management.
    - `useContext` can be used instead of Redux if objects descend from the same parent.
- Components have a single responsibility.
- `type` and `interface` are declared in `index.ts` inside the module if need to be exported.

### :file_cabinet: Server/Back-end Standards

- No "action" words/verbs in the url. HTTP verbs should be used to convey the action to be taken on the resource
  identified by the URL.
- URLs uniquely identifies the resource using a logical file-system path syntax
    - `profile/12` uniquely identifies a profile with id 12.
    - `profile/12/labs` lists the labs for the profile with id 12.
    - `profile/12/labs/75` uniquely identifies a lab with the id 75 that belongs to a profile with the id of 12.
- `///` comments should be placed on all public members.
- Cancellation tokens are forwarded to all methods that support cancellation.
- Correct HTTP status code is returned.
    - `400 - BadRequest` is returned when the request fails validation.
    - `201 - Created` is returned when a resource is created by the API.
    - `204 - NoContent` is returned when a resource is not found and it is not an error condition (the resource is not
      assumed to be there). Examples: Load the page help, load user settings, load default filters, etc. Places where
      content being missed is not an error. This is also the default response for PUT or other requests with no response
      body.
    - `404 - NotFound` is returned when a resource is expected an missing. Examples: `profile/12` that profile is
      expected.  `case/54` that case is expected.
- DTOs bound to HTTP requests that represent write operations: PUT, POST, PATCH, DELETE are called *Commands* and should
  be in a *Commands* folder.
- DTOs bound to HTTP requests that represent read operations: GET, POST are called *Queries* and should be in a
  *Queries* folder.
- Commands and Queuies should be validated before being passed to the service or repository layer; e.g. no validations
  in the service layer; validate beforehand.
- Use separate database connections for read and write requests; e.g. `MerlinReadContext` or `MerlinWriteContext` for
  EF. Supports offloading reads to a read replica and setting optimizations when read-only is known.
- Prefer Linq and EF when possible. Use Dapper as an escape hatch.
- Use method syntax only for Linq; e.g. `.Where(x => x.Name == "John")` not `from x where x.Name == "John"`
- When chaining methods, each new method should be places on a new line, e.g.

```csharp
context.People
    .Where(x => x.Name == "John")
    .OrderBy(x => x.Name)
    .Take(100)
    .ToListAsync();
```

- Read the smallest amount of data possible, .e.g prefer `AnycAsync()` over `FirstOrDefaultAsync()` to check if exists.
- Use data annotations for validation when possible and fallback to rules when needed.
- Suffix `async` methods with Async, e.g. `Task<int> GetIdAsync()` not `Task<int> GetId()`

### :floppy_disk: Database Standards

- Snake case naming.
- All uppercase.
- PKs are named `ID_TABLE_NAME`
- Indexes are named `IDX_TABLE_NAME__KEYCOL1_KEYCOL2_KEYCOL3`
- All scripts must be idempotent.

## :card_index_dividers: SDLC

| Key               | Value                                      |
|-------------------|--------------------------------------------|
| Release Cadence   | every 6-8 weeks                            |
| Versioning Scheme | <a href="https://semver.org/">Semantic</a> |
| Team Process      | SCRUM, 2 week sprints                      |

### :books: Source Code Management

![branching-model.png](docs%2Fimg%2Fbranching-model.png)

| Branch         | Use Case                                                                                                                                                                                                                                             |
|----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `dev`          | This is the development branch which contains the most up-to-date code for the current version the team is working on. If you are starting a new task or bug for the current version, most likely you want to branch from this branch to start with. |
| `master`       | This branch represents the latest code that has been deployed to production.                                                                                                                                                                         |
| `task/{task#}` | Branches following this pattern represent work for a single task work item. If you are starting a new task, name your branch using this pattern.                                                                                                     |

- `rebase` is the preferred method of integrating changes from `dev` into `task/*` provided only one person is working
  with the branch.
- Merges into `dev` and `master` should be squash merges to simplify history.
- When a `cherry-pick` is needed use the `-cp` suffix.
- Each merge commit on `master` that marks a production release should be tagged using the
  scheme `v{major}.{minor}.{patch}`.

### :cityscape: Environments

#### Production

| Resource                | Detail                                                | Version         |
|-------------------------|-------------------------------------------------------|-----------------|
| Database                | merlindbprod.doh.ad.state.fl.us (host: doh-wpdb003)   | SQL Server 2016 |
| Database (read replica) | merlindbreport.doh.ad.state.fl.us (host: doh-wpdb004) | SQL Server 2016 |
| Web 1                   | merlin.doh.ad.state.fl.us (host: dchp00pws023)        | Windows 2019    |
| Web 2                   | merlin2.doh.ad.state.fl.us (host: dchp00pws024)       | Windows 2019    |
| Web 3                   | merlin3.doh.ad.state.fl.us (host: dchp00pws025)       | Windows 2019    |
| Resource Group          | rg_dchp_merlin_prod                                   |                 |
| URL                     | merlin.doh.ad.state.fl.us/MerlinCore                  |

#### Staging

| Resource       | Detail                                                    | Version         |
|----------------|-----------------------------------------------------------|-----------------|
| Database       | merlindbstaging.doh.ad.state.fl.us (host: DCHP00TDBS004)  | SQL Server 2019 |
| Web 1          | merlinvmsstaging.doh.ad.state.fl.us (host: dchp00tws004)  | Windows 2019    |
| Web 2          | merlinvmsstaging2.doh.ad.state.fl.us (host: dchp00tws005) | Windows 2019    |
| Resource Group | rg_dchp_merlin_staging                                    |                 |
| URL            | merlinvmsstaging.doh.ad.state.fl.us/MerlinCore            |

#### Test

| Resource       | Detail                                                | Version         |
|----------------|-------------------------------------------------------|-----------------|
| Database       | merlindbtest.doh.ad.state.fl.us (host: DCHP00TDBS003) | SQL Server 2019 |
| Web 1          | merlin.doh.ad.state.fl.us (host: DCHP00TWS003)        | Windows 2019    |
| Resource Group | rg_dchp_merlin_test                                   |                 |
| URL            | merlinvmstest.doh.ad.state.fl.us/MerlinCore           |

#### Dev

| Resource       | Detail                                               | Version         |
|----------------|------------------------------------------------------|-----------------|
| Database       | merlindbdev.doh.ad.state.fl.us (host: DCHP00DDBS003) | SQL Server 2019 |
| Web 1          | merlin.doh.ad.state.fl.us (host: DCHP00DWS003)       | Windows 2019    |
| Resource Group | rg_dchp_merlin_dev                                   |                 |
| URL            | merlinvmsdev.doh.ad.state.fl.us/MerlinCore           |

#### ISF

| Resource       | Detail                       | Version                 |
|----------------|------------------------------|-------------------------|
| Database       | merlindb.isf.com             | SQL Server 2019         |
| Web 1          | merlindev.isf.com            | Ubuntu 20.04 on AKS     |
| Resource Group | isfmerlindev                 | isf.com/Merlin Test/Dev |
| URL            | merlindev.isf.com/MerlinCore |

### :station: CI/CD

| Environment | Pipeline              | Trigger                            | Branch    | Target | DevOps                                                             | 
|-------------|-----------------------|------------------------------------|-----------|--------|--------------------------------------------------------------------|
| Production  | merlin-core           | Approval of pending release        | release/* | IIS    | <a href="https://dev.azure.com/fdoh-dchp/Merlin/">DOH Dev Ops</a>  |
| Staging     | merlin-core-dev-ci-cd | commit on release/*                | release/* | IIS    | <a href="https://dev.azure.com/fdoh-dchp/Merlin/">DOH Dev Ops</a>  |
| Test        | merlin-core-dev-ci-cd | commit on dev (after dev succeeds) | dev       | IIS    | <a href="https://dev.azure.com/fdoh-dchp/Merlin/">DOH Dev Ops</a>  |
| Dev         | merlin-core-dev-ci-cd | commit on dev (pushed from ISF)    | dev       | IIS    | <a href="https://dev.azure.com/fdoh-dchp/Merlin/">DOH Dev Ops</a>  |
| ISF         | merlin-core-dev-ci-cd | commit on dev                      | dev       | AKS    | <a href="https://dev.azure.com/isf-merlin/Merlin/">ISF Dev Ops</a> |
