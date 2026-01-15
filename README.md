# Chat App

A realtime chat application with a React + Vite frontend and an Express + Socket.IO backend. Built to demonstrate user authentication, real-time messaging, and presence (online users). The repository contains two main folders: `frontend` (client) and `backend` (server).

One-liner: Realtime chat app with authentication, messaging, and online presence using Socket.IO and MongoDB.

---

## Table of Contents
- <a>Demo</a>
- <a>Tech stack</a>
- <a>Features</a>
- <a>Repository layout</a>
- <a>Prerequisites</a>
- <a>Local development</a>
  - <a>Backend</a>
  - <a>Frontend</a>
  - <a>Run both (dev)</a>
- <a>Production build</a>
- <a>Configuration / Environment variables</a>
- <a>API &amp; Socket overview</a>
- <a>Testing</a>
- <a>Contributing</a>
- <a>License</a>
- <a>Acknowledgements</a>

## Demo
(Optionally add screenshots or a hosted demo link here.)

## Tech stack
- Frontend: React 18, Vite, TailwindCSS + DaisyUI, Zustand, react-router, socket.io-client
- Backend: Node.js, Express, Socket.IO, Mongoose (MongoDB), jsonwebtoken, bcryptjs
- Other: Cloudinary is included as a dependency for image handling (optional)

## Features
- User signup &amp; login (JWT-based)
- Real-time one-to-one messaging with Socket.IO
- Online users / presence indicator
- Profile and settings pages
- Static build served by the backend in production

## Repository layout
- frontend/ — React client (Vite)
  - package.json (dev scripts: `dev`, `build`, `preview`)
  - src/ (App.jsx, pages, components, stores)
- backend/ — Express server + Socket.IO
  - package.json (scripts: `dev`, `start`)
  - src/
    - index.js — server entry (registers API routes, serves frontend in production)
    - lib/socket.js — Socket.IO server &amp; online user tracking
    - routes/ — auth and message routes

## Prerequisites
- Node.js (v18+ recommended)
- npm or yarn
- MongoDB (local or managed)
- (Optional) Cloudinary account if you intend to use image uploads

## Local development

### Backend
1. Open a terminal:
```bash
cd backend
npm install
```

2. Create a `.env` file in `backend/` (see the <a>Configuration</a> section below).

3. Start the backend in development mode:
```bash
npm run dev
```
This runs `nodemon src/index.js`. The backend uses the PORT value from your `.env`. The server's CORS is configured to allow the frontend dev server at `http://localhost:5173`.

### Frontend
1. In another terminal:
```bash
cd frontend
npm install
npm run dev
```
Vite starts a dev server (by default on http://localhost:5173). The frontend depends on the backend being reachable (see env/config).

### Run both (dev)
Start backend and frontend in two terminals as above. Alternatively, you can use a tool like `concurrently` from the root (not provided by default) to run both commands together.

## Production build
To serve the built frontend from the backend (index.js already handles serving `../frontend/dist` in production):

1. Build the frontend:
```bash
cd frontend
npm run build
```

2. Start the backend with NODE_ENV=production (ensure the frontend `dist` exists and `backend` can serve it):
```bash
cd backend
# set NODE_ENV=production and supply your .env values
npm start
```

When in production the backend serves static files from `../frontend/dist` and will return `index.html` for unknown routes.

## Configuration / Environment variables
Create `backend/.env` (example):
```
PORT=5000
NODE_ENV=development
MONGO_URI=mongodb://localhost:27017/chat-app
JWT_SECRET=your_jwt_secret
# Cloudinary variables (optional, if image uploads are used)
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
```

Notes:
- The backend reads `PORT` (index.js) and connects to the DB via `connectDB()`; ensure `MONGO_URI` is set.
- CORS in the backend is currently configured to allow `http://localhost:5173`. Update as needed for your deployment domain.

If you prefer, add frontend environment variables in Vite format (e.g., `VITE_API_BASE_URL`) and ensure the frontend reads them. The current repository uses axios — check the client code if you want to centralize the backend base URL in an env var.

## API &amp; Socket overview
- Registered API routes:
  - `POST /api/auth/...` — authentication routes (signup/login, etc.)
  - `GET/POST /api/messages/...` — messaging endpoints
- Socket events:
  - Server emits `getOnlineUsers` with a list of online user IDs (see `backend/src/lib/socket.js`).
  - Client connects to the socket server and passes a `userId` in `handshake.query` (see `socket.js`).

To find exact route signatures and socket events, inspect:
- `backend/src/routes/auth.route.js`
- `backend/src/routes/message.route.js`
- `backend/src/lib/socket.js`
- `frontend/src` (client-side socket usage and stores)

## Testing
No tests were found in the repository. Recommendations:
- Add unit tests for backend routes using Jest or Mocha + Supertest.
- Add component/unit tests for frontend with Vitest / React Testing Library.

## Contributing
Contributions are welcome.
- Fork the repo and create a feature branch.
- Open a PR describing changes and link related issues.
- Run linting and tests before opening a PR (if added).

Simple PR checklist:
- [ ] Tests added/updated
- [ ] Linter passes
- [ ] Documentation updated

## License
Add a LICENSE file to the repository if you want to declare a license (MIT recommended for open-source). This README assumes no license file currently; update as appropriate.

## Acknowledgements
- Built with React, Vite, Express, Socket.IO, and MongoDB.
- UI powered by TailwindCSS + DaisyUI.

---
