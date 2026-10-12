# Thinkboard – MERN Stack Notes Application

A full-stack notes application built with the MERN stack that allows users to create, view, edit, and delete notes through a web interface.

Thinkboard demonstrates how a React frontend communicates with a Node.js and Express backend, how notes are stored in MongoDB, and how Redis with Upstash can be used for API rate limiting.

## Table of Contents

1. Project Overview
2. Features
3. Technology Stack
4. Project Architecture
5. Project Folder Structure
6. Technologies Explained
7. File-by-File Explanation
8. How the Application Works
9. Installation and Setup
10. Environment Variables
11. Running the Application
12. Using the Application
13. Testing the APIs
14. Troubleshooting
15. Security Best Practices
16. Learning Outcomes

---

## 1. Project Overview

Thinkboard is a full-stack web application for managing personal notes. It provides a frontend where users can interact with notes and a backend that handles requests, business logic, database operations, and API protection.

The project uses the MERN stack:

- **MongoDB:** Stores note information.
- **Express.js:** Provides backend API routes and request handling.
- **React.js:** Builds the user interface.
- **Node.js:** Runs the backend JavaScript application.

Mongoose is used to communicate with MongoDB, Axios handles HTTP requests from the frontend, and Upstash Redis is used for rate limiting.

## 2. Features

- Create new notes.
- View saved notes.
- Edit existing notes.
- Delete notes.
- Communicate between the frontend and backend through REST APIs.
- Store note data in MongoDB Atlas.
- Apply API rate limiting using Redis and Upstash.
- Display application states through reusable React components.
- Use a responsive interface for managing notes.

The exact features available depend on the current implementation of the source code.

## 3. Technology Stack

### Frontend

| Technology | Purpose |
|---|---|
| React.js | Builds reusable UI components and application pages. |
| Vite | Runs the frontend development server and builds the frontend. |
| JavaScript | Implements application logic and user interactions. |
| Axios | Sends HTTP requests to the backend API. |
| CSS | Styles the application interface. |
| Tailwind CSS | Provides utility-based styling if configured in the project. |

### Backend

| Technology | Purpose |
|---|---|
| Node.js | Runs JavaScript on the server. |
| Express.js | Handles HTTP requests and defines API routes. |
| Mongoose | Defines note models and communicates with MongoDB. |
| MongoDB Atlas | Hosts the application's cloud database. |
| Upstash Redis | Provides a Redis service for rate limiting. |
| Nodemon | Restarts the backend during development when files change. |

## 4. Project Architecture

The application follows a client-server architecture.

```text
                 USER
                   |
                   v
           REACT FRONTEND
         (Vite development server)
                   |
                   v
                AXIOS
          HTTP API Requests
                   |
                   v
          NODE.JS + EXPRESS
            BACKEND SERVER
                   |
          +--------+--------+
          |                 |
          v                 v
      MONGOOSE         UPSTASH REDIS
          |             Rate Limiting
          v
      MONGODB ATLAS
        Notes Data
```

### How the architecture works

1. The user interacts with the React interface.
2. Axios sends an HTTP request to the Express backend.
3. Express matches the request to the appropriate API route.
4. The backend performs the requested operation.
5. Mongoose communicates with MongoDB when note data needs to be read or changed.
6. Redis supports rate limiting when a request passes through the configured rate-limiting middleware.
7. The backend returns a response to the frontend.
8. React updates the displayed information.

The React frontend does not connect directly to MongoDB. Database access happens through the backend.

## 5. Project Folder Structure

```text
mern-thinkboard-master/
│
├── backend/
│   ├── src/
│   │   ├── config/
│   │   │   ├── db.js
│   │   │   └── upstash.js
│   │   │
│   │   ├── controllers/
│   │   │   └── notesController.js
│   │   │
│   │   ├── middleware/
│   │   │   └── rateLimiter.js
│   │   │
│   │   ├── models/
│   │   │   └── Note.js
│   │   │
│   │   ├── routes/
│   │   │   └── notesRoutes.js
│   │   │
│   │   └── server.js
│   │
│   ├── package.json
│   └── package-lock.json
│
├── frontend/
│   ├── public/
│   │   ├── screenshot-for-readme.png
│   │   └── vite.svg
│   │
│   ├── src/
│   │   ├── components/
│   │   │   ├── Navbar.jsx
│   │   │   ├── NoteCard.jsx
│   │   │   ├── NotesNotFound.jsx
│   │   │   └── RateLimitedUI.jsx
│   │   │
│   │   ├── lib/
│   │   │   ├── axios.js
│   │   │   └── utils.js
│   │   │
│   │   ├── pages/
│   │   │   ├── CreatePage.jsx
│   │   │   ├── HomePage.jsx
│   │   │   └── NoteDetailPage.jsx
│   │   │
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   └── index.css
│   │
│   ├── index.html
│   ├── package.json
│   ├── package-lock.json
│   ├── postcss.config.js
│   ├── tailwind.config.js
│   └── vite.config.js
│
├── .gitignore
├── package.json
└── README.md
```

The `node_modules` directories and environment files are intentionally excluded from the repository. They are installed or configured separately on each developer's machine.

## 6. Technologies Explained

### 6.1 Node.js

Node.js is a runtime that allows JavaScript to run outside a web browser.

In this project, Node.js runs the backend server and executes the code that processes API requests, connects to services, and coordinates database operations.

### 6.2 Express.js

Express.js is a Node.js web framework.

It helps the backend:

- Receive HTTP requests.
- Define API endpoints.
- Run middleware.
- Call controller functions.
- Return JSON responses.

For example, an API endpoint might receive a request to create a note and pass it to the appropriate controller.

### 6.3 React.js

React is a JavaScript library for building user interfaces.

Thinkboard uses React components to divide the interface into manageable parts, such as the navigation bar, individual note cards, and note creation page.

When application data changes, React can update the relevant parts of the interface.

### 6.4 MongoDB Atlas

MongoDB is a document-oriented NoSQL database. It stores data as documents grouped into collections.

MongoDB Atlas is the managed cloud service used to host the database.

In Thinkboard, MongoDB stores the notes created through the application. A note can contain fields such as a title, description, and timestamps, depending on the model defined in `backend/src/models/Note.js`.

Mongoose connects the backend to MongoDB and provides a schema for the note data.

### 6.5 Redis and Upstash

Redis is an in-memory data store often used for caching, counters, and other fast data operations.

Upstash provides a managed Redis service that applications can access without maintaining their own Redis server.

In Thinkboard, Redis is used for **API rate limiting**. Rate limiting restricts how frequently a client can make requests within a configured period.

For example, if a user or client sends too many requests in a short time, the rate limiter can reject additional requests temporarily.