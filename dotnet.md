# Create solutions and projects

Create a new solution

```
mkdir SolutionPath
cd SolutionPath
dotnet new sln
```

Create a console application project

- With a given framework version:

```
dotnet new console --framework net6.0
```

- Inside a given project directory name:

```
dotnet new console -o ConsoleApp
```

Create a class library project

```
dotnet new classlib -o LibraryName
```

Create a MSTest project

```
dotnet new mstest -o LibraryNameTest
```

Add a project to a solution

```
dotnet sln add .\ConsoleApp\ConsoleApp.csproj
dotnet sln add .\LibraryName\LibraryName.csproj
dotnet sln add .\LibraryNameTest\LibraryNameTest.csproj
```

Add a library reference to a project

```
dotnet add .\ConsoleApp\ConsoleApp.csproj reference .\LibraryName\LibraryName.csproj
dotnet add .\LibraryNameTest\LibraryNameTest.csproj reference .\LibraryName\LibraryName.csproj
```

# Build, run and publish

Build a solution

```
dotnet build
```

Run a project

- In the current path:

```
dotnet run --project .\ConsoleApp\ConsoleApp.csproj
```

- A specific project path:

```
dotnet run --project .\ConsoleApp\ConsoleApp.csproj
```

- Run in release mode:

```
dotnet run --configuration Release
```

- A compiled project:

```
.\ConsoleApp.exe
-- or --
dotnet .\ConsoleApp.dll
```

Run unit tests in a MStest project

- Run unit tests:

```
dotnet test .\LibraryNameTest\LibraryNameTest.csproj
```

- Run tests in **release** configuration:

```
dotnet test .\StringLibraryTest\StringLibraryTest.csproj --configuration Release
```

Publish

```
dotnet publish --configuration Release
```

# Debugging

Use integrated VScode terminal while debugging

Inside `.vscode/launch.json`, change `console` setting from `internalConsole` to:

```json
"console": "integratedTerminal",
```

# Templates

Create a folder to have your template. Inside that folder, create a subfolder `.template.config` and a file named `template.json` with the content below:

For an `item` template:

```json
{
    "$schema": "http://json.schemastore.org/template",
    "author": "Me",
    "classifications": [
        "Common",
        "Code"
    ],
    "identity": "ExampleTemplate.StringExtensions",
    "name": "Example templates: string extensions",
    "shortName": "stringext",
    "tags": {
        "language": "C#",
        "type": "item"
    }
}
```

For a `project` template:

```json
{
    "$schema": "http://json.schemastore.org/template",
    "author": "Me",
    "classifications": [
        "Common",
        "Console",
        "C#9"
    ],
    "identity": "ExampleTemplate.AsyncProject",
    "name": "Example templates: async project",
    "shortName": "consoleasync",
    "tags": {
        "language": "C#",
        "type": "project"
    }
}
```

Install current template using:

```
dotnet new --install .\
```

List installed templates:

```
dotnet new --uninstall
```

Uninstall current template using:

```
dotnet new --install .\
```

List available templates:

```
dotnet new --list
```

# ASP.Net

Create a Razor Pages web app:

```
dotnet new webapp -o RazorPagesMovie
```

Trust SSL certificates for local development:

```
dotnet dev-certs https --trust
```

Run application in watch mode (server watches source code and recompile code on change and refresh pages automatically):

```
dotnet watch run
```

## Localization

To support validations and entering decimal point, date format and other localized data, install the following scripts on `wwwroot\lib`:

- [CLDR (unicode.org)](http://cldr.unicode.org/)
- [Globalize](https://github.com/globalizejs/globalize)
- [jquery-validation-globalize](https://github.com/johnnyreilly/jquery-validation-globalize)

Add the following to `_ValidationScriptsPartial.html`:

```html
<!-- cldr scripts (needed for globalize) -->
<script src="~/lib/cldrjs/dist/cldr.js"></script>
<script src="~/lib/cldrjs/dist/cldr/event.js"></script>
<script src="~/lib/cldrjs/dist/cldr/supplemental.js"></script>

<!-- globalize scripts -->
<script src="~/lib/globalize/dist/globalize.js"></script>
<script src="~/lib/globalize/dist/globalize/number.js"></script>
<script src="~/lib/globalize/dist/globalize/date.js"></script>

<script src="~/lib/jquery-validation-globalize/jquery.validate.globalize.js"></script>
```

## Scaffolding

Install generators and add dependencies to project:

```
dotnet tool uninstall -g dotnet-aspnet-codegenerator
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet tool uninstall -g dotnet-ef
dotnet tool install -g dotnet-ef
dotnet add package Microsoft.EntityFrameworkCore.Design;
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer```
```

Scaffold CRUD pages for a model:

```
dotnet-aspnet-codegenerator razorpage --model Movie --dataContext RazorPageMovieContext --useDefaultLayout -outDir Pages\Movies --referenceScriptLibraries -sqlite
```

# Entity Framework

## Ensure database creation

While developing, models are rapidly changing and it is more convinient to recreate database schema instead of using migrations:

```C#
using (var scope = app.Services.CreateScope())
{
    var services = scope.ServiceProvider;
    var context = services.GetRequiredService<EXAMPLE_DB_Context>();
    context.Database.EnsureCreated();
    // Add here calls to data seeding/initialization routines
}
```

## Using raw SQL

Using a query that returns an Entity:

```C#
var query = "SELECT * FROM Department WHERE DepartmentID = {0}";

var department = await _context.Departments
    .FromSqlRaw(query, id)
    .Include(d => d.Administrator)
    .FirstOrDefaultAsync(m => m.DepartmentID == id);
```

Using a query that returns other types of objects:

```C#
var query = "SELECT EnrollmentDate, COUNT(*) AS StudentCount FROM Student GROUP BY EnrollmentDate";
var data = db.Database.SqlQuery<EnrollmentDateGroup>(query);
```

Execute a raw SQL query using ADO.net directly

```C#
var groups = new List<EnrollmentDateGroup>();

// Get the DB connection from Entity Framework
var conn = _context.Database.GetDbConnection();
try
{
    await conn.OpenAsync();
    using (var cmd = conn.CreateCommand())
    {
        string query = "SELECT EnrollmentDate, COUNT(*) AS StudentCount FROM Student GROUP BY EnrollmentDate";
        cmd.CommandText = query;

        var reader = await cmd.ExecuteReaderAsync();
        if (reader.HasRows)
        {
            while (await reader.ReadAsync())
            {
                var row = new EnrollmentDateGroup
                {
                    EnrollmentDate = reader.GetDateTime(0),
                    StudentCount = reader.GetInt32(1)
                };
                groups.Add(row);
            }
        }
        reader.Dispose();
    }
}
finally
{
    conn.Close();
}
```

Execute a SQL command:

```C#
_context.Database.ExecuteSqlRawAsync("UPDATE Course SET Credits = Credits * {0}", multiplier);
```

## Migrations

Create a migration to update database structure:
```
dotnet ef migrations add SomeDescriptiveNameForMigration
```

Apply migrations:
```
dotnet ef database update
```

Recreate database from scratch using current model:

- First, delete the `migrations` folder
- Then run:
```
dotnet ef database drop
dotnet ef migrations add InitialCreate
dotnet ef database update
```

Generate a SQL script for migrations (--idempotent will generate a script that check which migrations have been applied):

```
dotnet ef migrations script --idempotent --output migrations.sql
```

Backup database before applying migrations:

- Using `sqlcmd` or some other SQL Server client (tested with SQL Server extension for VScode):

```sql
BACKUP DATABASE [YourDatabaseName] TO DISK = 'C:\Backups\YourDatabaseName_YYYYMMDD_HHMM.bak'
```

Restore database:

- First get the logical names from the backup file:

```sql
RESTORE FILELISTONLY
FROM DISK = 'C:\Backups\YourDatabaseName_YYYYMMDD_HHMM.bak'
```

- First get the logical names from the backup file:

```sql
RESTORE DATABASE [YourDatabaseName-Restored]
FROM DISK = 'C:\Backups\YourDatabaseName_YYYYMMDD_HHMM.bak'
WITH MOVE 'YourDatabaseName' to 'C:\Users\USERNAME\YourDatabaseName-Restored.mdf',
MOVE 'YourDatabaseName_log' to 'C:\Users\USERNAME\YourDatabaseName-Restored_log.ldf',
REPLACE
```

## Models

Scaffold models from existing database

- Add packages to support Entity Framework and code generators:

```
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore
```

- Add connection string to `appsettings.json`:

```json
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=YourDatabaseName;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

- Generate DB context and models from given database *CONNECTION* and *PROVIDER*:

```
dotnet ef dbcontext scaffold Name=ConnectionStrings:DefaultConnection Microsoft.EntityFrameworkCore.SqlServer --context ApplicationDbContext --data-annotations --context-dir Data --output-dir Models
```

## MVC

Create app with authentication using EF Identity:

```
dotnet new mvc --auth Individual -o MyApp -f net5.0:45
```

## Identity

Create an user account on data seed:

```csharp
public static class DbInitializer
{
    public static void SeedData(ApplicationDbContext context, IPasswordHasher<IdentityUser> passwordHasher)
    {
        context.Database.EnsureCreated();
        
        // ...

        var userEmail = "user@test.com";
        var user = new Microsoft.AspNetCore.Identity.IdentityUser
        {
            Id = Guid.NewGuid().ToString(),
            UserName = userEmail,
            NormalizedUserName = userEmail,
            Email = userEmail,
            NormalizedEmail = userEmail,
            EmailConfirmed = true
        };
        user.PasswordHash = passwordHasher.HashPassword(user, "P@ssw0rd");
        user.SecurityStamp = Guid.NewGuid().ToString();
        context.Users.Add(user);
        
        // ...
        
    }
}
```

# Monogame

Fixes some warnings on VS code for a Monogame project.

```xml
  <ItemGroup>
    <Import Include="Microsoft.Xna.Framework"/>
  </ItemGroup>
```


# xUnit

Suggested folder structure:

```
/
|
+- YourSolution
   |
   +-- YourProject
   |
   +-- YourProject.Tests
```

Create solution, project and tests:

```
mkdir YourSolution
cd YourSolution
dotnet new sln
dotnet new TYPE_OF_PROJECT -o YourProject
dotnet new xunit -o YourProject.Tests
dotnet sln add YourProject\YourProject.csproj
dotnet sln add YourProject\YourProject.Tests.csproj
dotnet add YourProject\YourProject.Tests.csproj reference YourProject\YourProject.csproj
```

Example unit test:

```csharp
using Xunit;
using YourProject.Xyz;

namespace YourProject.Test;

public class NameOfUnitBeingTested_Tests
{
    [Fact]
    public void MethodName_SomeParamDescription_ExpectedResultDescription()
    {
        // Arrange
        var obj = new NameOfUnitBeingTested();
        var param = 123;
        var expected = 456;
        
        // Act
        var actual = obj.MethodName(param);

        // Assert        
        Assert.Equal(expected, actual);
    }
    
    [Theory]
    [InlineData(123, 456)]
    [InlineData(456, 789)]
    public void MethodName_MultipleTestCases(int param, int expected)
    {
        // Arrange
        var obj = new NameOfUnitBeingTested();
        
        // Act
        var actual = obj.MethodName(param);

        // Assert        
        Assert.Equal(expected, actual);
    }
}
```
