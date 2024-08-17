## Comunicação com SignalR | WebSocket em C# ASP.NET
[Tutorial](https://www.youtube.com/watch?v=ZjAG465lpcA)

Adicione os pacotes:

1. Microsoft.AspNetCore.SignalR

Crie uma classe que extenda de `Hub`;

```csharp
class MyHub : Hub {}
```

Adicione no `Program.cs`

```csharp
builder.Services.AddSignalR();
// -- app = builder.build();

app.MapHub<MyHub>("/chat");
```

Adicione um método na classe que herda `Hub`

```csharp
public async IAsyncEnumerable<DateTime> Streaming(CancellationToken cancellationToken) { TODO };
```

No lado do cliente execute os seguintes códigos:

```csharp
await using var connection = new HubConnectionBuilder()
    .withUrl(url)
    .Build();

await connection.StartAsync();

await foreach (var date in connection.StreamAsync<DateTime>("Streaming"))
    Console.WriteLine(date);
```