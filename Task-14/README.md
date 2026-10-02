# Real-Time Go — Stage 1 WebSocket Chat Room

A real-time WebSocket chat application built using Go, Gorilla WebSocket, Docker, and AWS EC2.

This project demonstrates how WebSockets enable instant bidirectional communication between multiple browser clients without page refreshes. The application uses Go’s concurrency model with goroutines and channels to build a scalable real-time chat system. 

---

# 📄 Documentation

Detailed implementation steps with screenshots are available inside the `docs/` folder.

Document Name:

* `14.Stage1_WebSocket_Documentation.docx`

Repository Structure:

```text id="jlwmr1"
realtime-go/
│
├── README.md
│
├── docs/
│   └── 14.Stage1_WebSocket_Documentation.docx
│
├── client/
│   └── index.html
│
├── server/
│   ├── main.go
│   ├── Dockerfile
│   ├── go.mod
│   └── go.sum
│
└── docker-compose.yml
```

---

# 📌 Project Overview

This project covers Stage 1 of the Real-Time Go workshop — a live WebSocket chat room where multiple users can join and exchange messages instantly in real time.

Features:

* Real-time messaging
* Multiple user support
* WebSocket communication
* Dockerized deployment
* AWS EC2 hosting
* Auto reconnect support
* Dark terminal-style UI



---

# ⚡ What is WebSocket?

Traditional HTTP works using a request-response model:

```text id="jlwm8v"
Browser → Server → Response → Connection Closed
```

WebSocket upgrades the connection into a persistent full-duplex communication channel where both client and server can send data anytime.

Advantages:

* Instant communication
* Low latency
* Persistent connection
* No page refresh needed
* Efficient for live applications



---

# 🏗️ Architecture Overview

```text id="jlwm2n"
Browser Tab 1
      │
      ▼
 WebSocket Connection
      │
      ▼
 ┌─────────────────────┐
 │      Go Server      │
 │       (Hub)         │
 └─────────────────────┘
      ▲
      │ Broadcast
      ▼
Browser Tab 2
```

The application uses the Hub pattern where a single hub manages all connected clients and broadcasts messages safely using Go channels. 

---

# ☁️ Technologies Used

| Technology        | Purpose               |
| ----------------- | --------------------- |
| Go 1.21           | Backend server        |
| gorilla/websocket | WebSocket library     |
| Docker            | Containerization      |
| Docker Compose    | Service orchestration |
| AWS EC2           | Cloud deployment      |
| HTML/CSS/JS       | Frontend UI           |



---

# 🧠 Key Go Concepts Used

This project demonstrates several important Go concepts:

* Goroutines
* Channels
* Hub Pattern
* sync.Mutex
* select statement
* Concurrent client handling

Each connected client gets:

* `readPump()` goroutine
* `writePump()` goroutine

This keeps reading and writing operations fully independent. 

---

# 📂 Project Structure

```text id="jlwmf7"
realtime-go/
├── client/
│   └── index.html
│
├── server/
│   ├── main.go
│   ├── Dockerfile
│   ├── go.mod
│   └── go.sum
│
├── docker-compose.yml
└── README.md
```

Description:

| File               | Purpose                  |
| ------------------ | ------------------------ |
| index.html         | Chat UI                  |
| main.go            | WebSocket server         |
| Dockerfile         | Multi-stage Docker build |
| docker-compose.yml | Container setup          |
| README.md          | Project documentation    |



---

# 🚀 Server Architecture

The backend is implemented in a single:

```text id="7jlwm5"
main.go
```

Core structures:

| Structure | Purpose                    |
| --------- | -------------------------- |
| Message   | JSON message format        |
| Client    | Connected WebSocket client |
| Hub       | Broadcast engine           |

Message Structure:

```go id="jlwmh9"
type Message struct {
    Type     string
    Username string
    Text     string
    Time     string
    Count    int
}
```



---

# 🔄 Hub Pattern

The Hub manages:

* Client registration
* Client removal
* Message broadcasting

Core channels:

```go id="jlwmj4"
register   chan *Client
unregister chan *Client
broadcast  chan Message
```

Advantages:

* Thread-safe communication
* Centralized client management
* Race-condition prevention
* Scalable architecture



---

# ⚙️ Goroutines Per Client

Each client creates two goroutines:

## readPump()

* Reads incoming messages
* Pushes messages to broadcast channel

## writePump()

* Sends outgoing messages
* Maintains WebSocket connection
* Sends periodic ping messages

This prevents slow clients from blocking others. 

---

# 🐳 Docker Multi-Stage Build

The application uses a multi-stage Docker build for smaller and optimized images.

Build Flow:

```text id="jlwmx2"
golang:1.21-alpine
        ↓
Compile Go Binary
        ↓
alpine:3.19
        ↓
Minimal Runtime Image
```

Benefits:

* Smaller image size
* Better security
* Faster deployment
* No Go compiler in final image



---

# 📜 Dockerfile Highlights

Important build steps:

```dockerfile id="jlwmc1"
FROM golang:1.21-alpine AS builder
RUN go mod download
RUN go build -o chatserver

FROM alpine:3.19
COPY --from=builder /build/chatserver .
EXPOSE 8080
CMD ["./chatserver"]
```



---

# 🌐 Frontend Architecture

Frontend is implemented as a single:

```text id="jlwmm0"
index.html
```

Features:

* Pure HTML/CSS/JS
* No frameworks
* Auto reconnect
* Dark terminal UI
* Real-time online count
* Live message rendering



---

# 🔌 WebSocket Client Code

Connection setup:

```javascript id="jlwmw6"
const ws = new WebSocket('ws://localhost:8080/ws?username=rohith');

ws.onopen = () => setStatus(true);

ws.onmessage = (event) => {
    renderMessage(JSON.parse(event.data));
};

ws.onclose = () => {
    setStatus(false);
    setTimeout(connect, 3000);
};
```

Features:

* Auto reconnect every 3 seconds
* Real-time updates
* Native browser WebSocket API



---

# 🐳 Docker Compose Setup

The project uses:

```text id="0jlwm7"
docker-compose.yml
```

Features:

* Port mapping
* Volume mounting
* Auto restart
* Health checks

Key configuration:

```yaml id="jlwmt1"
ports:
  - "8080:8080"

restart: unless-stopped
```



---

# ▶️ Run the Application

## Build and Start

```bash id="jlwms8"
docker-compose up --build
```

## Run in Background

```bash id="jlwme3"
docker-compose up -d
```

## View Logs

```bash id="4jlwm8"
docker-compose logs -f
```

## Stop Containers

```bash id="jlwmq3"
docker-compose down
```



---

# 🌍 Access Application

Open browser:

```text id="jlwml7"
http://localhost:8080
```

For AWS EC2:

```text id="jlwmu8"
http://<EC2-PUBLIC-IP>:8080
```

---

# 💬 Real-Time Chat Demonstration

Demonstrated workflow:

1. User `rohith` joins room
2. User `ajith` joins from another tab
3. Both users appear online
4. Messages broadcast instantly
5. No page refresh required

The hub broadcasts all events to every connected client in real time. 

---

# 📚 Student Exercises

Included advanced learning exercises:

| Exercise                     | Difficulty  |
| ---------------------------- | ----------- |
| Count goroutines             | Easy        |
| Break slow client protection | Medium      |
| Typing indicators            | Medium      |
| Message history              | Medium-Hard |
| Multiple chat rooms          | Hard        |

These exercises help students understand Go concurrency and scalable real-time systems. 

---

# 🔐 Key Features

✅ Real-time messaging

✅ Concurrent client handling

✅ WebSocket architecture

✅ Dockerized deployment

✅ AWS-ready

✅ Auto reconnect

✅ Multi-user support

✅ Minimal frontend

✅ Optimized Docker images

---

# 📊 Technical Summary

| Item              | Value                    |
| ----------------- | ------------------------ |
| Language          | Go 1.21                  |
| WebSocket Library | gorilla/websocket v1.5.1 |
| Server Port       | 8080                     |
| Concurrency       | 2 goroutines/client      |
| Deployment        | Docker + AWS EC2         |



---

# 🏢 Real-World Applications

This architecture pattern is commonly used for:

* Chat applications
* Gaming servers
* Stock market dashboards
* Live collaboration tools
* Notifications systems
* Monitoring dashboards
* Multiplayer applications

---

# 📚 Key Learning Outcomes

After completing this project, you will understand:

* WebSocket architecture
* Real-time communication
* Go concurrency model
* Goroutines & channels
* Docker multi-stage builds
* AWS EC2 deployment
* Real-time system design

---

# 👨‍💻 Author

Rohith

---

# ⭐ Summary

This project demonstrates a scalable real-time WebSocket chat application using:

* Go concurrency
* WebSockets
* Docker
* AWS EC2

The Hub pattern combined with goroutines and channels provides a clean, race-free, and production-ready real-time communication architecture.
