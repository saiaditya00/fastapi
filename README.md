
FastAPI WebSocket Chat ApplicationThis repository contains a simple yet functional chat application built with FastAPI, leveraging WebSockets for real-time communication. It demonstrates basic API endpoints and a live chat interface accessible via a web browser.✨ FeaturesFastAPI Backend: A robust and high-performance Python web framework.WebSocket Communication: Real-time bidirectional communication for chat messages.Dynamic Client IDs: Each new client connecting to the chat gets a unique ID.Broadcast Messaging: Messages sent by one client are broadcast to all connected clients.Join/Leave Notifications: System messages inform users when someone joins or leaves the chat.Interactive API Docs: Automatic Swagger UI documentation for API endpoints.Basic UI: A simple HTML/CSS/JavaScript frontend for the chat interface, styled with Bootstrap for a clean look.Responsive Design: The chat interface is designed to be usable on various screen sizes.🚀 Technologies UsedBackend:FastAPIUvicorn (ASGI server)Pydantic (for data validation and serialization)Frontend:HTML5CSS3 (with Bootstrap 5)JavaScript (Vanilla JS for WebSocket handling and UI updates)📦 Setup and InstallationFollow these steps to get the project up and running on your local machine.PrerequisitesPython 3.8+pip (Python package installer)StepsClone the repository:git clone https://github.com/saiaditya00/fastapi.git
cd fastapi/main # Navigate into the main project directory
Create a virtual environment (recommended):python -m venv venv
Activate the virtual environment:On Windows:.\venv\Scripts\activate
On macOS/Linux:source venv/bin/activate
Install dependencies:pip install -r requirements.txt
# If requirements.txt is not present, you can install manually:
# pip install fastapi uvicorn[standard] pydantic
(Note: The provided repository does not include a requirements.txt. You might need to create one or install packages manually as shown above.)▶️ Running the ApplicationOnce the dependencies are installed, you can start the FastAPI server:uvicorn main:app --reload
main: Refers to your Python file named main.py.app: Refers to the FastAPI() instance within main.py.--reload: Enables hot-reloading, so the server restarts automatically on code changes (useful for development).The application will typically run on http://127.0.0.1:8000.🌐 UsageAPI EndpointsYou can interact with the API endpoints via your browser or tools like curl, Postman, or FastAPI's interactive documentation.Root Endpoint (GET)URL: http://127.0.0.1:8000/Method: GETDescription: Returns a simple "Hello World" message.Example Response:"Hello World"
Create Item (POST)URL: http://127.0.0.1:8000/itemsMethod: POSTDescription: Adds a new item to an in-memory list.Request Body (JSON):{
  "text": "My new task",
  "is_done": false
}
Example curl command:curl -X POST \
  http://127.0.0.1:8000/items \
  -H "Content-Type: application/json" \
  -d '{ "text": "Learn FastAPI", "is_done": false }'
List Items (GET)URL: http://127.0.0.1:8000/itemsMethod: GETDescription: Retrieves a list of previously added items.Query Parameters:limit (optional, integer): Maximum number of items to return (default: 10).Example Response:[
  {
    "text": "Learn FastAPI",
    "is_done": false
  },
  {
    "text": "Buy groceries",
    "is_done": false
  }
]
Get Single Item (GET)URL: http://127.0.0.1:8000/items/{item_id}Method: GETDescription: Retrieves a single item by its index in the list.Path Parameters:item_id (integer): The 0-based index of the item.Example Response (for item_id=0):{
  "text": "Learn FastAPI",
  "is_done": false
}
Error Response (if item_id is out of bounds):{
  "detail": "item 404 not found"
}
WebSocket ChatOpen in Browser: Navigate to http://127.0.0.1:8000/ in your web browser.Multiple Tabs: Open multiple tabs or windows to simulate multiple users in the chat. Each tab will get a unique "Your ID".Send Messages: Type a message in the input field and press "Send". Your message will appear in all connected chat windows, prefixed with your client ID.Join/Leave Notifications: Observe system messages when new clients join or existing clients close their browser tabs.Interactive API Documentation (Swagger UI)While your server is running, you can access the automatic interactive API documentation at:http://127.0.0.1:8000/docsThis interface allows you to view all your API endpoints, their expected parameters, and even test them directly from the browser.🤝 ContributingContributions are welcome! If you have suggestions for improvements or find any issues, please open an issue or submit a pull request.📄 LicenseThis project is open-source and available under the MIT License.(Note: A LICENSE file is not present in the provided repository. It's good practice to add one.)



<img width="960" height="445" alt="image" src="https://github.com/user-attachments/assets/9eaa9858-5122-4b03-bc26-049cfbbe87ec" />
