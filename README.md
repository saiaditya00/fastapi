# FastAPI WebSocket Chat 🚀

A simple yet functional real-time chat application built with FastAPI and WebSockets. Each client receives a unique ID, messages are broadcast to all connected users, and notifications appear when users join or leave. The app includes interactive API docs and a basic responsive chat UI.

<img width="960" height="445" alt="image" src="https://github.com/user-attachments/assets/9eaa9858-5122-4b03-bc26-049cfbbe87ec" />

---

## 🔧 Features

- **FastAPI Backend**: High-performance, async-ready Python web framework.
- **WebSocket Communication**: Real-time, bidirectional messaging.
- **Dynamic Client IDs**: Unique ID assigned to each connected client.
- **Broadcast Messaging**: Messages from one client reach all connected clients.
- **Join/Leave Notifications**: System alerts on user connect/disconnect.
- **Interactive API Docs**: Swagger UI at `/docs`.
- **Basic UI**: Clean chat interface styled with Bootstrap.
- **Responsive Design**: Works on various devices and screen sizes.

---

## 🛠️ Tech Stack

- **Backend**: FastAPI, Uvicorn, Pydantic  
- **Frontend**: HTML5, CSS3 (Bootstrap 5), Vanilla JavaScript (WebSocket handling)  
- **Dependencies** (add in `requirements.txt`):

  ```text
  fastapi
  uvicorn[standard]
  pydantic
  ```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- pip

### Installation & Setup

```bash
git clone https://github.com/saiaditya00/fastapi.git
cd fastapi/main
python -m venv venv
source venv/bin/activate    # macOS/Linux
# .\venv\Scripts\activate   # Windows
pip install -r requirements.txt
```

> If `requirements.txt` is missing, run:
> ```bash
> pip install fastapi uvicorn[standard] pydantic
> ```

### Running the App

```bash
uvicorn main:app --reload
```

Visit `http://127.0.0.1:8000` in your browser.

---

## 📄 Usage

### API Endpoints

Use Swagger UI at `http://127.0.0.1:8000/docs` or external tools (curl, Postman).

- **GET /**  
  - Returns: `"Hello World"`

- **POST /items**  
  - Request:
    ```json
    { "text": "My task", "is_done": false }
    ```
  - Response: Item object returned with `text` and `is_done`

- **GET /items?limit=10**  
  - Retrieves up to `limit` items (default 10)

- **GET /items/{item_id}**  
  - Returns a single item by index  
  - Returns 404 if out of bounds

### 🗨️ WebSocket Chat

1. Open multiple browser tabs to `http://127.0.0.1:8000`.
2. Each session receives a unique ID.
3. Send messages—see them broadcast across tabs.
4. Join/leave system messages appear in all sessions.

---

## 🧩 Suggested Enhancements

- ✅ Add `requirements.txt`
- 📸 Include UI screenshots
- 🧪 Add automated tests using `pytest` & `httpx`
- 🧱 Implement data persistence (in-memory → database)
- 🔐 Add user authentication
- 🎨 Enhance UI (e.g. avatars, timestamps)
- 🗄️ Add Docker support
- 🌍 Deploy using Uvicorn with Gunicorn or on cloud platforms

---

## Contributing

Contributions welcome! Feel free to:

- Fork the repo
- Create feature branches (`git checkout -b feature/my-feature`)
- Commit changes (`git commit -am 'Add feature'`)
- Push and open a pull request 🎉

---

## 📝 License

This project is open-source under the **MIT License**. *(Include `LICENSE` file in repo)*

---

## 📧 Contact

For feedback or questions, reach me at: kamarsuaditya@gmail.com 

---

## 📚 Resources

- [FastAPI Documentation](https://fastapi.tiangolo.com)
- [FastAPI WebSocket Guide](https://fastapi.tiangolo.com/advanced/websockets/)

---

**Enjoy building your real-time chat app! 🎉**


