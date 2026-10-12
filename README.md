# Apna Video Call — Video Conferencing Web App

Apna Video Call is a full-stack video-conferencing project built with React, Express, MongoDB, Socket.IO, and WebRTC browser APIs. It includes a landing page, user registration and login, meeting-code navigation, meeting history, and a video-meeting interface with media controls and chat UI.

> **Portfolio status:** This README describes the structure and features visible in the source code. Configure the backend environment, secure database credentials, and test the full call flow before describing the app as production-ready.

## Features

- Landing page with guest, login, and registration navigation
- Registration and login backed by an Express API
- Password hashing with `bcrypt`
- Meeting-code entry and navigation
- Meeting activity/history stored in MongoDB
- Socket.IO signaling events for joining calls, exchanging signaling data, chat messages, and participant departures
- Video/audio permission handling and local media stream controls
- Screen-sharing UI and chat interface in the meeting page
- Protected home route wrapper
- Material UI components and icons

## Tech Stack

### Frontend
- React 18
- React Router
- Material UI (MUI)
- Axios
- Socket.IO Client
- Browser media APIs and WebRTC peer connections

### Backend
- Node.js with ES modules
- Express
- MongoDB and Mongoose
- Socket.IO
- bcrypt for password hashing
- CORS

## Project Structure

```text
Zoom/
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── contexts/
│   │   │   └── AuthContext.jsx
│   │   ├── pages/
│   │   │   ├── landing.jsx
│   │   │   ├── authentication.jsx
│   │   │   ├── home.jsx
│   │   │   ├── history.jsx
│   │   │   └── VideoMeet.jsx
│   │   ├── styles/
│   │   ├── utils/
│   │   ├── App.js
│   │   └── environment.js
│   └── package.json
├── backend/
│   ├── src/
│   │   ├── controllers/
│   │   ├── models/
│   │   ├── routes/
│   │   └── app.js
│   └── package.json
└── README.md
```

## Prerequisites

- Node.js and npm
- A MongoDB database (local or MongoDB Atlas)
- A modern browser with camera/microphone permissions
- HTTPS for deployed camera/microphone and screen-sharing access (localhost is generally treated as a secure context)

## Setup

### 1. Clone the repository

```bash
git clone <your-github-repository-url>
cd Zoom
```

### 2. Configure the backend

```bash
cd backend
npm install
```

Create a `.env` file in `backend/`:

```env
PORT=8000
MONGODB_URI=your_mongodb_connection_string
```

**Security requirement:** The current source has a MongoDB connection string hard-coded in `backend/src/app.js`. Before publishing this repository, remove it from the source and read `process.env.MONGODB_URI` instead. If the credential was real, rotate the database password and revoke the old credential. Never commit `.env`.

Update the database connection in `src/app.js` to use the environment variable, for example:

```js
await mongoose.connect(process.env.MONGODB_URI);
```

Then start the backend:

```bash
npm run dev
```

The backend defaults to port `8000`.

### 3. Configure and start the frontend

Open another terminal:

```bash
cd frontend
npm install
npm start
```

The Create React App development server normally opens at `http://localhost:3000`.

The frontend server URL is configured in `frontend/src/environment.js`. For local development, set it to:

```js
const server = "http://localhost:8000";
export default server;
```

For deployment, use your own backend URL rather than a third-party/demo server.

## API Routes

The Express router is mounted at `/api/v1/users`.

| Method | Endpoint | Purpose |
|---|---|---|
| `POST` | `/api/v1/users/register` | Register a user |
| `POST` | `/api/v1/users/login` | Authenticate a user |
| `POST` | `/api/v1/users/add_to_activity` | Save a meeting code to history |
| `GET` | `/api/v1/users/get_all_activity` | Retrieve meeting history |

## Real-Time Meeting Events

The Socket.IO server includes events for:

- `join-call` — join a meeting room identified by a path/code
- `user-joined` — notify participants about connected users
- `signal` — relay WebRTC signaling payloads between peers
- `chat-message` — relay chat messages within a meeting
- `user-left` — notify participants when someone disconnects

The meeting page uses browser media APIs and peer-connection signaling. Successful calls depend on correct signaling, camera/microphone permissions, network conditions, and suitable STUN/TURN configuration.

## Important Notes Before Publishing

- **Remove credentials:** Never expose MongoDB usernames, passwords, tokens, or other secrets in GitHub. Rotate any credential that has already been committed.
- **Review Git history:** Removing a secret from the latest file does not remove it from previous commits. Rotate the credential first and clean repository history if necessary.
- **Check routing:** Review the routes in `frontend/src/App.js`; verify the home route path and all imports before running the app.
- **Review authentication:** The current token-based flow is a learning implementation. Review authorization checks, token expiry, error handling, and secure storage before production use.
- **Review Socket.IO access:** The current Socket.IO CORS configuration is permissive. Restrict origins and validate room membership for deployment.
- **Configure production networking:** Use HTTPS and configure STUN/TURN servers as appropriate for reliable WebRTC connectivity.
- **Test the complete flow:** Verify registration, login, meeting join, multi-user audio/video, screen sharing, chat, and history with separate browser sessions.

## Future Improvements

- Add environment validation and centralized error handling.
- Add meeting-room authorization and stronger session management.
- Improve responsive design, accessibility, and connection-status feedback.
- Add robust reconnect/disconnect handling and cleanup for media tracks and peer connections.
- Add tests for authentication, history APIs, and meeting signaling.
- Deploy the frontend and backend with documented environment variables.

## License

This project is intended for learning and portfolio demonstration. Add a license file if you choose to distribute it publicly.
