# 🗂️ PlanVerse

A modern, collaborative task management application inspired by Trello. PlanVerse enables teams to organize projects using Kanban-style boards with real-time updates, role-based access control, and seamless collaboration features.

![Go](https://img.shields.io/badge/Go-1.21-00ADD8?style=flat&logo=go)
![React](https://img.shields.io/badge/React-18-61DAFB?style=flat&logo=react)
![TypeScript](https://img.shields.io/badge/TypeScript-5.0-3178C6?style=flat&logo=typescript)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Latest-336791?style=flat&logo=postgresql)
![Redis](https://img.shields.io/badge/Redis-6-DC382D?style=flat&logo=redis)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=flat&logo=docker)

---

## ✨ Features

### 📋 Project Management
- Create and manage multiple projects
- Customize project backgrounds and descriptions
- Share projects via invite links
- Email invitations for team members

### 📊 Kanban Boards
- Create customizable columns (states) for workflow organization
- Drag-and-drop task management between states
- Color-coded columns for visual organization
- Admin-only access control for specific columns

### ✅ Task Management
- Create tasks with rich details (title, description, deadlines)
- Set task priority levels
- Assign multiple performers to tasks
- Track estimated vs. actual time
- Color-coded task cards
- Labels for categorization
- Comments for team discussions

### 👥 Team Collaboration
- Role-based access control (Admin/Member)
- Promote/demote team members
- Real-time updates via WebSocket
- Member management per project

### 🔐 Authentication & Security
- User registration with email verification (OTP)
- JWT-based authentication
- Secure password handling with bcrypt
- Profile customization with avatars

### 📈 Monitoring & Observability
- Prometheus metrics integration
- Grafana dashboards for visualization
- Health checks for all services

---

## 🏗️ Architecture

```
PlanVerse/
├── khoshk/          # Backend (Go)
│   ├── configs/     # Database & Redis configuration
│   ├── controllers/ # HTTP request handlers
│   ├── helpers/     # Utility functions
│   ├── middlewares/ # Auth, admin verification
│   ├── models/      # Data models & DTOs
│   ├── routes/      # API route definitions
│   ├── services/    # Email service
│   └── tests/       # Unit tests
│
├── tar/             # Frontend (React)
│   ├── public/      # Static assets
│   └── src/
│       ├── api/         # API client & proxy
│       ├── components/  # React components
│       ├── redux/       # State management
│       ├── ui/          # Reusable UI components
│       └── utils/       # Utilities & types
│
└── docker-compose.yml
```

---

## 🛠️ Tech Stack

### Backend (khoshk)
| Technology | Purpose |
|------------|---------|
| **Go 1.21** | Core language |
| **Echo v4** | HTTP framework |
| **GORM** | ORM for PostgreSQL |
| **PostgreSQL** | Primary database |
| **Redis** | Caching & sessions |
| **JWT** | Authentication tokens |
| **WebSocket** | Real-time communication |
| **Prometheus** | Metrics collection |

### Frontend (tar)
| Technology | Purpose |
|------------|---------|
| **React 18** | UI framework |
| **TypeScript** | Type safety |
| **Vite** | Build tool |
| **Redux Toolkit** | State management |
| **Redux Observable** | Side effects with RxJS |
| **Material UI** | Component library |
| **SCSS Modules** | Scoped styling |
| **Axios** | HTTP client |
| **React Router v6** | Client-side routing |

---

## 🚀 Getting Started

### Prerequisites
- [Docker](https://docs.docker.com/get-docker/) & Docker Compose
- [Node.js](https://nodejs.org/) (v18+) & npm/yarn (for local frontend development)
- [Go](https://golang.org/dl/) 1.21+ (for local backend development)

### Quick Start with Docker

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/PlanVerse.git
   cd PlanVerse
   ```

2. **Create environment file**
   ```bash
   # Create .env file in root directory
   cp .env.example .env
   ```
   
   Configure the following environment variables:
   ```env
   # Database
   DB_HOST=db
   DB_PORT=5432
   DB_USER=root
   DB_PASSWORD=strong-password
   DB_NAME=postgres
   
   # Redis
   REDIS_HOST=redis
   REDIS_PORT=6379
   REDIS_DB=0
   
   # JWT
   JWT_SECRET=your-secret-key
   
   # Email (for verification)
   GMAIL_PASSWORD=your-app-password
   ```

3. **Start all services**
   ```bash
   docker-compose up --build
   ```

4. **Access the application**
   - Frontend: http://localhost:5173
   - Backend API: http://localhost:8080
   - Prometheus: http://localhost:9090
   - Grafana: http://localhost:3000

---

## 💻 Local Development

### Backend (khoshk)

```bash
cd khoshk

# Install dependencies
go mod download

# Run the server
go run main.go
```

The backend server starts on `http://localhost:8080`

### Frontend (tar)

```bash
cd tar

# Install dependencies
npm install
# or
yarn install

# Start development server
npm run dev
# or
yarn dev
```

The frontend development server starts on `http://localhost:5173`

### Running Tests

```bash
cd khoshk
go test ./tests/...
```

---

## 📡 API Endpoints

### Authentication
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/register` | Register new user |
| POST | `/verify` | Verify email with OTP |
| POST | `/login` | User login |
| POST | `/refresh` | Refresh JWT token |
| POST | `/resend-email` | Resend verification email |

### User Management
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/get-user/:user-id` | Get user by ID |
| GET | `/get-my-user` | Get current user |
| POST | `/edit-profile` | Update user profile |
| POST | `/delete-account` | Delete user account |

### Projects
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/list-project` | List user's projects |
| POST | `/create-project` | Create new project |
| GET | `/get-project/:project-id` | Get project details |
| POST | `/edit-project/:project-id` | Edit project (Admin) |
| POST | `/delete-project/:project-id` | Delete project |
| POST | `/share-link/:project-id` | Share project via email |
| POST | `/show-project` | Preview project from link |
| POST | `/join-project/:project-id` | Join project |
| POST | `/leave-project/:project-id` | Leave project |
| GET | `/get-project-members/:project-id` | Get project members |
| POST | `/promote/:project-id/:user-id` | Promote to admin |
| POST | `/demote/:project-id/:user-id` | Demote from admin |

### States (Columns)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/list-state/:project-id` | List states with tasks |
| POST | `/create-state/:project-id` | Create state (Admin) |
| GET | `/get-state/:project-id/:state-id` | Get state details |
| POST | `/edit-state/:project-id/:state-id` | Edit state (Admin) |
| POST | `/delete-state/:project-id/:state-id` | Delete state (Admin) |

### Tasks
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/create-task/:project-id/:state-id` | Create task (Admin) |
| GET | `/get-task/:task-id` | Get task details |
| POST | `/edit-task/:project-id/:task-id` | Edit task (Admin) |
| POST | `/delete-task/:project-id/:task-id` | Delete task (Admin) |
| POST | `/change-state/:project-id/:task-id` | Move task to state |
| POST | `/add-performer/:project-id/:task-id` | Assign performer (Admin) |
| POST | `/remove-performer/:project-id/:task-id` | Remove performer (Admin) |

### WebSocket
| Endpoint | Description |
|----------|-------------|
| `/ws` | Real-time updates connection |

### Monitoring
| Endpoint | Description |
|----------|-------------|
| `/metrics` | Prometheus metrics |

---

## 🐳 Docker Services

| Service | Port | Description |
|---------|------|-------------|
| `back-end` | 8080 | Go API server |
| `db` | 5435 | PostgreSQL database |
| `redis` | 6379 | Redis cache |
| `prometheus` | 9090 | Metrics collection |
| `grafana` | 3000 | Metrics visualization |

---

## 📁 Data Models

### User
- Username, Email, Password
- Profile picture
- Email verification status
- Project memberships

### Project
- Title, Description
- Background picture
- Owner and members
- Invite link
- States (columns)

### State (Column)
- Title, Background color
- Admin-only access flag
- Ordered tasks

### Task
- Title, Description
- Background color
- Priority level
- Deadline
- Estimated/Actual time
- Assigned performers
- Comments
- Labels

---

## 🎨 Frontend Features

- **Dark/Light Mode**: Automatic theme detection based on system preferences
- **Responsive Design**: Adapts to different screen sizes
- **Real-time Updates**: Live synchronization via WebSocket
- **Toast Notifications**: User feedback with react-toastify
- **Form Validation**: Client-side input validation

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

## 👥 Team

Built with ❤️ by the PlanVerse team.
