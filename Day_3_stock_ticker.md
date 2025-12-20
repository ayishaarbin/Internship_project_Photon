# Day 3: Real-Time Stock Ticker Implementation

## Goals for Day 3
- Stream real-time stock prices from backend to frontend using WebSockets.
- Display continuous updates on the frontend (`index.html`).
- Ensure the backend (`main.py`) handles multiple clients and streams data efficiently.
- Test and debug the stock ticker display.

---

## Backend Implementation

- Used **FastAPI** and **Uvicorn** for the backend server.
- Created `/ws` WebSocket endpoint in `backend/main.py`.
- Backend sends **simulated stock prices** every 1 second.

```python
@app.websocket("/ws")
async def websocket_endpoint(websocket: WebSocket):
    await websocket.accept()
    try:
        while True:
            stock_price = round(random.uniform(200, 500), 2)
            await websocket.send_text(f"PHOTON: ₹{stock_price}")
            await asyncio.sleep(1)
    except Exception as e:
        print("Client disconnected")
Frontend Implementation
Connected frontend HTML page to backend using WebSockets.
Dynamically displayed incoming stock prices in frontend/index.html.
Example snippet:
Copy code
Javascript
const ws = new WebSocket('ws://127.0.0.1:8000/ws');
ws.onmessage = function(event) {
    const message = document.createElement('div');
    message.className = 'stock';
    message.textContent = event.data;
    tickerDiv.appendChild(message);
};
Output
When running the backend (uvicorn backend.main:app --reload) and opening index.html:
Copy code

📈 Photon Stock Ticker
PHOTON: ₹378.59
PHOTON: ₹476.72
PHOTON: ₹250.47
PHOTON: ₹359.33
PHOTON: ₹265.16
...
The stock prices update every second.
Initial page shows: 📈 Photon Stock Ticker Waiting for data... until WebSocket connects.
Issues Faced
WebSocket warnings:
Copy code

WARNING: No supported WebSocket library detected.
Resolved by installing uvicorn[standard] and websockets.
Continuous stock updates:
Real-time ticker never stops by design.
To stop updates, frontend page must be closed or server stopped (CTRL+C).
Initial “Waiting for data…” message:
Appears until WebSocket successfully connects to backend.
Key Learnings
Real-time communication using WebSockets.
FastAPI backend integration with frontend.
Handling continuous data streams efficiently.
Debugging common WebSocket errors in Python.
