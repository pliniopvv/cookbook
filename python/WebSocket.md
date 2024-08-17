## Chat com flask-socketio
[Tutorial](https://www.youtube.com/watch?v=q0VqmrGapPw)

 Instale os seguintes pacotes

 1. flask
 2. flask-socketio
 3. gevent
 4. gevent-websocket

Crie uma rota para o `html` que será renderizado

```python
app = Flask(__name__)
io = SocketIO(app)

messages = []

@app.route("/")
def home():
    return render_template("chat.html")

@io.on('sendMessage')
def send_message_handler(msg):
    messages.append(messages)
    emit('getMessage', msg, broadcast=True) // pacote flask_socketIO

@io.on('message')
def message_handler(msg):
    send(messages) // pacote flask_socketIO

if __name__ == "__main__":
    io.run(app, debug=True)
```

```html
<body>

    <div>
        <form>
            <input type="text" placeholder="insira seu nome">
            <input type="text" placeholder="insira sua mensagem">
            <button type="submit">Enviar</button>
        </form>
    </div>
    <div id="msgs">
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/4.7.5/socket.io.js"></script>
    <script>
        window.onload = function() {
            const socket = io('http://127.0.0.1:5000');

            socket.on('connect', () => {
                socket.send('Usuário conectado ao socket!');
            });

            document.querySelector("form").addEventListener("submit", function(event) {
                event.preventDefault();

                socket.emit('sendMessage', {
                    nome: event.target[0].value,
                    message: event.target[1].value
                });
            });

            const msgs = document.querySelector("#msgs");
            socket.on('getMessage', (msg) => {
                msgs.innerHTML += `<p><span><strong>${msg.name}:</strong>${msg.message}</span></p>`;
            });

            socket.on('message', (backlogMgs) => {
                for (const msg of backlogMgs)
                    msgs.innerHTML += `<p><span><strong>${msg.name}:</strong>${msg.message}</span></p>`;
            });
        }

    </script>
</body>
```