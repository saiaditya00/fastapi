
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
