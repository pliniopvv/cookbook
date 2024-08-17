## Configurando Entity Framework

Dependências necessárias:

1. Microsoft.EntityFrameworkCore
1. Microsoft.EntityFrameworkCore.Design
1. Driver do banco de dados `Microsoft.EntityFrameworkCore.SqlServer` ou `Npgsql.EntityFrameworkCore.PostgreSQL`

Adicione em `appsettings.json`
```json
  "ConnectionStrings": {
    "SqlServer": "Server=localhost\\SQLEXPRESS; Database=MyApp; Integrated Security=True; trustServerCertificate=true",
    "PostgreSQL": "Host=localhost; Database=dotnet-6-crud-api; Username=postgres; Password=mysecretpassword"
  }
```

Adicione em `Program.cs`

```csharp
var dbConfig = builder.Configuration.GetConnectionString("SqlServer/PostgreSQL");
builder.Services.AddDbContext<ContextBase>(options => options.UseSqlServer(dbConfig));
builder.Services.AddDbContext<ContextBase>(options => options.UseNpgsql(dbConfig));
```

Crie a seguinte classe `DbContext`

```csharp
    public class ContextBase : DbContext
    {
        public ContextBase(DbContextOptions<ContextBase> options) : base(options)
        {
        }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            base.OnModelCreating(modelBuilder); // necessário para IdentityUser
            Configure.... - Seguir com mapeamento;
        }
    }
```

<details>
<summary>SqlServer - Docker</summary>
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong@Passw0rd>" -p 1433:1433 --name sql1 --hostname sql1 -d mcr.microsoft.com/mssql/server:2022-latest
</details>