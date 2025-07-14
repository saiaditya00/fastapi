
Repository: saiaditya00/fastapi
Files analyzed: 2

Estimated tokens: 2.3k

Directory structure:
└── saiaditya00-fastapi/
    ├── README.md
    └── chatapp/
        ├── main.py
        └── __pycache__/


================================================
FILE: README.md
================================================
<img width="960" height="445" alt="image" src="https://github.com/user-attachments/assets/9eaa9858-5122-4b03-bc26-049cfbbe87ec" />



================================================
FILE: chatapp/main.py
================================================



from fastapi import FastAPI, WebSocket, WebSocketDisconnect
from fastapi.responses import HTMLResponse
from typing import List, Dict

app = FastAPI()

html = """
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FastAPI WebSocket Chat</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.6/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body {
            background-color: #f0f2f5; /* Light grey background */
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
        }
        .chat-container {
            max-width: 700px;
            width: 95%;
            background-color: #ffffff;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 8px 30px rgba(0,0,0,0.1);
            display: flex;
            flex-direction: column;
            overflow: hidden;
            min-height: 600px; /* Minimum height for the chat box */
            max-height: 90vh; /* Max height to fit on screen */
        }
        h1, h2 {
            color: #333;
            text-align: center;
            margin-bottom: 25px;
        }
        #ws-id {
            font-size: 1.1rem;
            font-weight: 600;
        }
        .message-input-form {
            display: flex;
            margin-top: 25px;
        }
        #messageText {
            flex-grow: 1;
            border-radius: 20px;
            padding: 10px 18px;
            border: 1px solid #ced4da;
            transition: all 0.2s ease-in-out;
        }
        #messageText:focus {
            box-shadow: 0 0 0 0.25rem rgba(13,110,253,.25);
            border-color: #86b7fe;
        }
        .btn-send {
            border-radius: 20px;
            padding: 10px 25px;
            margin-left: 10px;
            background-color: #007bff;
            border-color: #007bff;
            transition: background-color 0.2s ease;
        }
        .btn-send:hover {
            background-color: #0056b3;
            border-color: #004085;
        }
        #messages {
            list-style-type: none;
            padding: 0;
            margin: 0;
            flex-grow: 1; /* Allow messages list to take available space */
            overflow-y: auto; /* Enable scrolling for messages */
            border: 1px solid #e0e0e0;
            border-radius: 8px;
            background-color: #fafafa;
            padding: 15px;
            margin-bottom: 20px;
        }
        #messages li {
            padding: 8px 10px;
            margin-bottom: 10px;
            border-radius: 8px;
            word-wrap: break-word; /* Prevents long words from breaking layout */
            line-height: 1.4;
            color: #333;
            background-color: #e9ecef; /* Default background for messages */
            border-left: 4px solid #007bff; /* Indicator for messages */
        }
        #messages li.system-message {
            background-color: #e6f7ff; /* Light blue for system messages */
            border-left-color: #007bff;
            font-style: italic;
            color: #555;
            text-align: center;
        }
        #messages li.user-message {
            background-color: #d1e7dd; /* Light green for user's own messages */
            border-left-color: #28a745;
            text-align: right; /* Align user messages to the right */
        }
        #messages li.other-user-message {
            background-color: #e9ecef; /* Light grey for other users' messages */
            border-left-color: #6c757d;
            text-align: left; /* Align other user messages to the left */
        }
        /* Scrollbar styles for better aesthetics */
        #messages::-webkit-scrollbar {
            width: 8px;
        }
        #messages::-webkit-scrollbar-track {
            background: #f1f1f1;
            border-radius: 10px;
        }
        #messages::-webkit-scrollbar-thumb {
            background: #888;
            border-radius: 10px;
        }
        #messages::-webkit-scrollbar-thumb:hover {
            background: #555;
        }

        /* Responsive adjustments */
        @media (max-width: 576px) {
            .chat-container {
                padding: 15px;
            }
            .btn-send {
                padding: 10px 15px;
            }
        }
    </style>
</head>
<body>
    <div class="chat-container">
        <h1>FastAPI WebSocket Chat</h1>
        <h2>Your ID: <span id="ws-id" class="badge bg-primary"></span></h2>
        
        <ul id="messages"></ul>

        <form action="" onsubmit="sendMessage(event)" class="message-input-form">
            <input type="text" id="messageText" class="form-control" placeholder="Type your message here..." autocomplete="off" required>
            <button class="btn btn-primary btn-send">Send</button>
        </form>
    </div>
    <script>
        var client_id = Math.floor(Math.random() * 1000000);
        document.getElementById('ws-id').textContent = client_id;

        var hostname = window.location.hostname;
        var port = window.location.port ? ':' + window.location.port : '';
        var ws_protocol = window.location.protocol === 'https:' ? 'wss:' : 'ws:';
        var ws = new WebSocket(ws_protocol + '//' + hostname + port + '/ws/' + client_id);

        ws.onmessage = function(event) {
            var messagesList = document.getElementById('messages');
            var messageItem = document.createElement('li');
            var messageContent = event.data;

            // Determine message type and apply class
            if (messageContent.includes("has joined the chat") || messageContent.includes("has left the chat") || messageContent.includes("encountered an error")) {
                messageItem.classList.add('system-message');
            } else if (messageContent.startsWith(`Client #${client_id} says:`)) {
                messageItem.classList.add('user-message');
                // Optional: remove "Client #ID says: " for own messages
                messageContent = messageContent.replace(`Client #${client_id} says: `, 'You: ');
            } else if (messageContent.includes("says:")) {
                messageItem.classList.add('other-user-message');
            }

            messageItem.appendChild(document.createTextNode(messageContent));
            messagesList.appendChild(messageItem);
            messagesList.scrollTop = messagesList.scrollHeight; // Auto-scroll to bottom
        };

        ws.onerror = function(error) {
            console.error("WebSocket Error: ", error);
            var messagesList = document.getElementById('messages');
            var messageItem = document.createElement('li');
            messageItem.classList.add('system-message');
            messageItem.style.color = 'red';
            messageItem.textContent = "Connection error. Please refresh.";
            messagesList.appendChild(messageItem);
            messagesList.scrollTop = messagesList.scrollHeight;
        };

        ws.onclose = function(event) {
            console.log("WebSocket Closed: ", event);
            var messagesList = document.getElementById('messages');
            var messageItem = document.createElement('li');
            messageItem.classList.add('system-message');
            messageItem.style.color = 'orange';
            messageItem.textContent = "Connection closed. " + (event.reason ? `Reason: ${event.reason}` : "Attempting to reconnect...");
            messagesList.appendChild(messageItem);
            messagesList.scrollTop = messagesList.scrollHeight;
            // Optional: Implement auto-reconnect logic here
        };

        function sendMessage(event){
            var input = document.getElementById('messageText');
            if (input.value.trim() !== '') {
                ws.send(input.value);
                input.value = '';
            }
            event.preventDefault();
        }
    </script>
</body>
</html>
"""

class ConnectionManager:
    def __init__(self):
        self.active_connections: List[WebSocket] = []

    async def connect(self, websocket: WebSocket):
        await websocket.accept()
        self.active_connections.append(websocket)

    def disconnect(self, websocket: WebSocket):
        if websocket in self.active_connections:
            self.active_connections.remove(websocket)

    async def send_personal_message(self, message: str, websocket: WebSocket):
        try:
            await websocket.send_text(message)
        except Exception as e:
            print(f"Error sending personal message: {e}")

    async def broadcast(self, message: str):
        disconnected_clients = []
        for connection in self.active_connections:
            try:
                await connection.send_text(message)
            except RuntimeError as e:
                print(f"Failed to send to client (likely disconnected): {e}")
                disconnected_clients.append(connection)
            except Exception as e:
                print(f"Unexpected error broadcasting to a client: {e}")
                disconnected_clients.append(connection)
        
        for client in disconnected_clients:
            self.disconnect(client)

manager = ConnectionManager()

@app.get("/", response_class=HTMLResponse)
async def get():
    return html

@app.websocket("/ws/{client_id}")
async def websocket_endpoint(websocket: WebSocket, client_id: int):
    await manager.connect(websocket)
    try:
        await manager.broadcast(f"Client #{client_id} has joined the chat.")
        
        while True:
            data = await websocket.receive_text()
            await manager.broadcast(f"Client #{client_id} says: {data}")
    except WebSocketDisconnect:
        manager.disconnect(websocket)
        await manager.broadcast(f"Client #{client_id} has left the chat.")
    except Exception as e:
        print(f"WebSocket error for client {client_id}: {e}")
        manager.disconnect(websocket)
        await manager.broadcast(f"Client #{client_id} encountered an error and left.")



<img width="960" height="445" alt="image" src="https://github.com/user-attachments/assets/9eaa9858-5122-4b03-bc26-049cfbbe87ec" />
