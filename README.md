
# Project: Video Conferencing Platform

This project is a full-stack video conferencing web application that demonstrates real-time peer-to-peer media communication, signaling, and basic user session management.

Theory and Architecture
- Client (Frontend): Built with React. The client handles user interactions, UI, and real-time media capture using the WebRTC APIs (`getUserMedia`, `RTCPeerConnection`). It connects to the server using Socket.IO for signaling and chat.
- Server (Backend): Built with Node.js and Express. The server provides REST endpoints for user authentication and meeting history, and runs a Socket.IO signaling service for exchanging SDP and ICE candidates between peers.
- Signaling: Socket.IO is used only for signaling (session negotiation, user join/leave events, and chat messages). Once peers exchange SDP and ICE candidates, media streams flow directly between clients (peer-to-peer) subject to NAT traversal (STUN/TURN).
- Persistence: MongoDB stores user records and meeting history (meeting codes, timestamps). Meeting codes are generated server-side to ensure uniqueness and are saved in the `Meeting` collection.

High-level Flow
1. A user authenticates and can create or join meetings.
2. When a meeting is created the server generates a unique meeting code and stores it with the user's history.
3. Peers join the same meeting URL (or enter a meeting code). The client connects to the Socket.IO server and emits a `join-call` event.
4. The server notifies other participants in the same meeting and the peers exchange `signal` events (SDP / ICE) to establish WebRTC connections.
5. Chat messages and presence updates are relayed via the Socket.IO channel.

Security and Best Practices
- Keep credentials and database URIs in environment variables (`.env`) — do not commit secrets in source files.
- Consider adding authentication/authorization checks on signaling endpoints to prevent unauthorized use.
- For production, add TURN servers to improve connectivity across restrictive NATs.

Quick Run (development)
```powershell
# Backend
cd backend
npm install
npm run dev

# Frontend
cd frontend
npm install
npm start
```

Files of interest
- `backend/src/controllers/socketManager.js` — Socket.IO signaling logic
- `backend/src/controllers/user.controller.js` — user and meeting endpoints
- `frontend/src/pages/VideoMeet.jsx` — WebRTC and UI for meetings
- `frontend/src/pages/home.jsx` — create/join meeting UI 
