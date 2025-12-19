📈 Photon – Real-Time Stock Ticker (WebSocket Based)

📅 Day 2 – Real-Time Data Streaming (Backend ↔ Frontend)

📌 Objective (Day 2)

The goal of Day 2 was to enable real-time communication between the backend and frontend so that stock prices update live in the browser without refreshing the page.

This was achieved using WebSockets.


---

🧠 What Was Implemented

✅ 1. Backend: Real-Time Stock Price Generator

Used FastAPI WebSockets to push live stock prices

Simulated stock price changes using Python’s random module

Continuously sent updated stock prices every few seconds


✅ 2. Frontend: Live UI Updates

Used JavaScript WebSocket API

Connected to backend WebSocket endpoint

Dynamically updated the webpage with incoming stock prices



---

🔄 Why “Waiting for data…” Changed Automatically

Initially, the frontend displays:

📈 Photon Stock Ticker
Waiting for data...

Once the WebSocket connection is established and data starts streaming,
the text automatically updates to live prices like:

PHOTON: ₹234.56
PHOTON: ₹311.22
PHOTON: ₹498.01

🔹 This confirms real-time data flow is working correctly
🔹 No refresh required
🔹 Data updates instantly from backend → frontend


---

🧩 Backend Code (FastAPI – WebSocket)

📁 File: backend/main.py

from fastapi import FastAPI, WebSocket
import random
import asyncio

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Photon Backend Running"}

@app.websocket("/ws/stock")
async def stock_price(websocket: WebSocket):
    await websocket.accept()
    while True:
        price = round(random.uniform(200, 500), 2)
        await websocket.send_text(f"PHOTON: ₹{price}")
        await asyncio.sleep(2)


---

🎨 Frontend Code (HTML + JavaScript)

📁 File: frontend/index.html

<!DOCTYPE html>
<html>
<head>
    <title>Photon Stock Ticker</title>
</head>
<body>
    <h1>📈 Photon Stock Ticker</h1>
    <div id="price">Waiting for data...</div>

    <script>
        const ws = new WebSocket("ws://127.0.0.1:8000/ws/stock");

        ws.onmessage = function(event) {
            document.getElementById("price").innerText = event.data;
        };
    </script>
</body>
</html>


---

▶️ How to Run (Day 2)

1️⃣ Start Backend

uvicorn backend.main:app --reload

2️⃣ Open Frontend

Open frontend/index.html in browser

You will see live price updates



---

📊 Sample Output

Browser Output:

📈 Photon Stock Ticker
PHOTON: ₹234.56
PHOTON: ₹311.22
PHOTON: ₹498.01

Backend Console Logs:

INFO: WebSocket connection accepted
INFO: Sending stock prices...


---

⭐ Key Learnings (Day 2)

Implemented WebSocket-based real-time streaming

Understood difference between HTTP vs WebSocket

Built live-updating UI without refresh

Strengthened backend–frontend integration skills
