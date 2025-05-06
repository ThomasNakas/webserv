# 🧠 High-Performance HTTP Server

This project implements a robust and scalable server using **C++** and low-level networking tools such as **sockets**, **poll**, and manual read/write per client. It is capable of handling multiple simultaneous connections under heavy stress with careful resource and error management.

---

## 🚀 Server

### ✅ Core Technologies
- **C++** (C++17)
- **POSIX Sockets**
- **`poll` system call**
- **Manual `read` / `write` per client**
- **Signal handling (`SIGINT`)**
- **File I/O for dynamic content delivery**

### 🛠 Implementation Details
- Each connection is **manually managed** using `poll()` with flags:
  - `POLLIN`: waits for incoming data
  - `POLLOUT`: waits for socket readiness to write
  - `POLLHUP`: detects closed connections
- One `read()` and `write()` operation per client for low-level control.
- Handles **partial reads/writes**, buffer accumulation, and fragmented requests.
- Detects and manages **file descriptors cleanup** on disconnection or crash.
- Server gracefully shuts down on `Ctrl+C` and cleans up all resources.

### 🔁 Stress Test Setup
- 7 containerized server instances running in **Docker**.
- Tested with **non-empty files** served to multiple clients.
- Load generation using **`siege` from a different machine**.
- Benchmark target: sustain 100 concurrent connections with stable memory and CPU usage.

---

## ⚙️ Config

### 🔄 Dynamic Paths
- `root`: dynamically defined per server config
- `uploads`: dynamically routed and created if missing
- `errors`: custom error pages loaded per status code (e.g. 404.html, 413.html)

### 🗑 DELETE Method Rules
- Can only delete files **within the `www` root**
- **Forbidden** from deleting:
  - Files marked as "protected"
  - Folders/directories

### 🧪 Bash Test Suite
- Includes scripts for:
  - Invalid requests
  - Over-sized uploads (`413 Payload Too Large`)
  - Unsupported MIME types (`415 Unsupported Media Type`)
  - Invalid HTTP verbs
  - Tricky corner cases (e.g., malformed headers, slowloris)

### 💽 Storage Awareness
- Server monitors storage space:
  - Shuts down gracefully on low disk space
  - Sends `503 Service Unavailable` if it cannot store data

---

## 📡 Request & Response

### 🔍 Request Parsing
- Works with both:
  - Browser requests
  - Raw input (e.g. via `telnet`)
- Detects and handles malformed headers with appropriate `400 Bad Request` responses.

### 🔄 Response Strategy
- Dynamically determines:
  - MIME types
  - File size
  - Connection status
- On completion of response:
  - Sets `Connection: close`
  - Closes socket

### 🚫 Error Management
- Handles:
  - `413` – Payload too large
  - `415` – Unsupported file type
  - `403` – Forbidden deletes
  - `404` – File not found
  - `500` – Internal server errors

---

## 🔥 STRESS TESTS (CRITICAL)

- **Stress tests are run under strict conditions**:
  - 7 Dockerized servers running simultaneously
  - Requests made from a separate machine using **`siege`**
  - **Files served are non-empty and randomly selected**
  - All edge cases simulated in parallel (invalid paths, huge uploads, unsupported types)
  - Resource usage monitored in real-time to ensure **no crash under 100 concurrent clients**

---

## 🛑 Closing Notes

This server is designed to simulate real-world conditions with complex edge-case handling. It’s battle-tested, dynamically configurable, and built to fail gracefully.

**Important**: Always run this with appropriate `ulimit` and Docker volume configs to allow concurrent file writing and full I/O simulation.
