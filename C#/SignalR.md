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

## Chat em tempo real com VueJS + SignalR C#
[Tutorial](https://www.youtube.com/watch?v=q0VqmrGapPw)

### Backend

Adicione os pacotes:

1. Microsoft.AspNetCore.SignalR.Core

Crie uma interface `IHubProvider` com um método:

```csharp
Task ReceivedMessage(Message message);

public class Message {
    public string Name;
    public string Body;
    public string Date;
}
```

Crie uma classe `HubProvider` que herda `Hub<IHubProvider>`

```csharp
public class HubProvider : Hub<IHubProvider> {
    public async Task SendMessage(Message message) {
        await Clients.All.ReceivedMessage(message);
    }
}
```

Adicione o serviço do SignalR no arquivo  `Program.cs`:

```csharp
builder.Services.AddSignalR();

// -- app = builder.Build();

app.UseCors(cors => {
    cors.AllowAnyMethod()
    .AllowAnyHeader()
    .AllowCredentials()
    .withOrigins("http://localhost:8080");
});
app.MapHub<HubProvider>("/Hub");
```

### Frontend

Adicionar a dependência: 

1. @aspnet/signalr

criar a classe `Hub.js`

```javascript
export default class Hub {
    constructor() {
        this.connection = new HubConnectionBuilder()
        .withUrl("localhost:port/Hub")
        .configureLogging(LogLevel.Information)
        .build();
    }
}
```

Instancie um objeto `Hub` e inicie a conexão

```javascript
const hub = new Hub();
hub.connection.start()
.then(() => {
    console.log("Connected");
    hub.connection.on("ReceivedMessage", (msg) => console.log(msg));
}).catch(e => console.log("Connection failed", e));
```

Forma de enviar mensagens

```javascript
hub.connection.invoke("SendMessage", message);
```