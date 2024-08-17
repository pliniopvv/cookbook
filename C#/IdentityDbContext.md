
## Configurando ASP.NET Identity

Dependências necessárias:

1. Microsoft.AspNetCore.Identity
1. Microsoft.AspNetCore.Identity.EntityFrameworkCore


Adicione uma classe `User` que extende de `IdentityUser`
Adicione um `DbContext` que extende de `IdentityDbContext`

```csharp
    public class AuthenticationContext(DbContextOptions options)
        : IdentityDbContext<User>(options);
```

Configurações do arquivo `Program.cs`

```csharp
builder.Services.AddAuthentication();
builder.Services.AddAuthorization();

builder.Services.AddIdentityApiEndpoints<User>()
    .AddEntityFrameworkStores<AuthenticationBase>();

// -- app = builder.build();

app.MapIdentityApi<User>();
```

Criar e executar primeira migration

```powershell
dotnet ef migrations add v1
dotnet ef database update
```

<details>
<summary>EF + Identity</summary>
public class ContextBase : IdentityDbContext<`User`>
</details>