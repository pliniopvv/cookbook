## Configurando Entity Framework

Dependências necessárias:

1. Microsoft.EntityFrameworkCore
2. Microsoft.EntityFrameworkCore.Design

Adicione em `Program.cs`

```csharp
var dbConfig = builder.Configuration.GetConnectionString("SqlServer");
builder.Services.AddDbContext<ContextBase>(options => options.UseSqlServer(dbConfig));
```

Adicione em `appsettings.json`
```json
  "ConnectionStrings": {
    "SqlServer": "Server=localhost\\SQLEXPRESS; Database=MyApp; Integrated Security=True; trustServerCertificate=true"
  }
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
            Configure.... - Seguir com mapeamento;
        }
    }
```