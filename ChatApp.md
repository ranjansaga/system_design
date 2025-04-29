# 📱 Real-Time Chat App – MVP Requirements

---

## ✅ Functional Requirements (Backend)

| Feature                    | Description                                                                 |
|---------------------------|-----------------------------------------------------------------------------|
| User Authentication        | Login and logout with JWT-based authentication.                            |
| Real-time Messaging        | Send/receive messages via WebSocket in real time.                           |
| Message Persistence        | Store all chat messages in MongoDB.                                         |
| Message History            | Load past messages via REST API (with pagination).                          |
| Presence Tracking          | Show online/offline status of users via Redis.                              |
| Typing Indicator           | Indicate when the other user is typing (optional).                          |
| Delivery Acknowledgement   | Show sent/delivered status to sender.                                       |
| WebSocket Gateway          | Handle bi-directional communication between client and backend.             |

---

## 🚀 Non-Functional Requirements (Backend)

| Category           | Requirement                                                                 |
|--------------------|------------------------------------------------------------------------------|
| Scalability        | Horizontal scaling of WebSocket servers and Kafka consumers.                 |
| Availability       | No single point of failure. Kafka and Redis configured for HA.               |
| Performance        | <100ms latency for message delivery in real time.                            |
| Durability         | All messages stored durably in MongoDB and Kafka.                            |
| Security           | HTTPS, secure WebSockets, JWT validation.                                    |
| Observability      | Logging, metrics, error tracking for WebSocket server and Kafka flows.       |
| Extensibility      | Modular design to support group chats, media messages, etc.                  |
| Maintainability    | Clean, modular codebase with CI/CD and testing.                              |

---

## 🎨 Functional Requirements (UI)

| Feature                  | Description                                                                  |
|--------------------------|-------------------------------------------------------------------------------|
| Login Page               | Username/password input, loading state, error handling.                       |
| Chat List                | List of recent chats with presence status.                                    |
| Chat Window              | Shows message history, input box, sent/delivered status.                      |
| Real-Time Messaging      | Messages appear instantly via WebSocket.                                     |
| Typing Indicator         | Show "typing..." status (optional for MVP).                                  |
| Message History          | Load past messages with scroll-based pagination.                             |
| Online/Offline Status    | Display user status based on WebSocket/Redis presence tracking.              |
| Responsive Design        | Works across mobile, tablet, and desktop screens.                            |
| Logout Option            | Allow user to securely log out of the app.                                   |

---

## 📐 Non-Functional Requirements (UI)

| Category           | Requirement                                                                 |
|--------------------|------------------------------------------------------------------------------|
| Performance        | Fast UI rendering, lazy loading messages, efficient DOM updates.            |
| Accessibility      | WCAG-compliant: ARIA labels, keyboard navigation, color contrast.            |
| Security           | Escape user input to prevent XSS; secure JWT storage.                        |
| Offline Handling   | Graceful handling of disconnection with reconnect attempts.                  |
| Observability      | Client-side error tracking via tools like Sentry or LogRocket.               |
| Maintainability    | Modular components, TypeScript, proper folder structure.                     |
| Dev Experience     | Hot reload, ESLint, Prettier, Tailwind CSS for styling consistency.          |
