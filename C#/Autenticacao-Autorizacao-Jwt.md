# Implementando Autenticação e Autorização com Tokens JWT (Json Web Token) - ASP.NET
[Tutorial](https://www.youtube.com/watch?v=nv5xGOXZxJc)

Implemente normalmente um `DbContext` com uma classe `User` que pode receber cadastros de usuários.

Instale o seguinte pacote:

1. Microsoft.AspNetCore.Authentication.JwtBearer

Sugestão de Classe `User`:

```csharp
public class User {
    public string UserName { get;. set; }
    public string Password { get;. set; }
    public string Role { get;. set; }
}
```

## Criar classe `ITokenService`

```csharp
public interface ITokenService {
    public string GenerateToken(LoginDto login);
}

public class TokenService : ITokenService {
    private readonly IConfiguration _configuration;
    private readonly IUserRepository repository;

    // add constructor with DI;

    public async Task<string> GenerateToken(LoginDto login) {
        var userDb = await repository.GetByUserName(login.UserName);
        if (userDb.UserName != login.UserName || login.Password != userDb.Password)
        {
            return String.Empty;
        }

        var secretKey = ne SymmetricSecurityKey(Encoding.UTF8.GetBytes(_configuration["Jwt:Key"] ?? string.Empty));
        var issuer = _configuration["Jwt:Issuer"];
        var audience = _configuration["Jwt:Audience"];

        var signinCredentials = new SigningCredentials(secretKey, SecurityAlgorithms.HmacSha256);
        var tokenOptions = new JwtSecurityToken(
            issuer: issuer,
            audience: audience,
            claims: new [],
            {
                new Claim(type: ClaimTypes.Name, userDb.UserName),
                new Claim(type: ClaimTypes.Role, userDb.Role)
            },
            expires: DateTime.Now.AddHours(2),
            signingCredentials: signinCredentials);

            var token = new JwtSecurityTokenHandler().WriteToken(tokenOptions);
            return token;
    }    
}
```

## Classe `AuthenticationController`

Criar um controller de autenticação, por exemplo: `AuthenticationController`

```csharp
    public class AuthenticationController : ControllerBase
    {
        private readonly ITokenService _tokenService;
        
        // add constructor with DI;

        [HttpPost("login", Name = "login")]
        [ProducesResponseType(StatusCodes.Status200OK)]
        [ProducesResponseType(StatusCodes.Status500InternalServerError)]
        public ActionResult Login(LoginDTO login) {
            var token = _tokenService.GenerateToken(login);
            if (token == "")
                return Unauthorized();
            return Ok(token);
        }
    }
```

## Modificações no `Program.cs`

Adicione um `Authentication` no `Program.cs`:

```csharp
// ------ services
builder.Services.TryAddScoped<ITokenService, TokenService>();

// ...

builder.Services.AddAuthentication(x => {
    x.DefaultAuthenticationScheme = JwtBearerDefaults.AuthenticationScheme;
    x.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;

})
    .AddJwtBearer(options => {
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = true,
            ValidateAudience = true,
            ValidateLIfetime = true,
            ValidateIssuerSigningKey = true,
            ValidIssuer = builder.Configuration["Jwt:Issuer"],
            ValidAudience = builder.Configuration["Jwt:Audience"],
            IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(builder.Configuration["Jwt:Key"]))
        };
    });

    // app = builder.build();

    app.UserAuthentication();
    app.UserAuthorization();
```

Property `[Authorization(Roles = "<Claim>,<Claim>")]`, configurada.