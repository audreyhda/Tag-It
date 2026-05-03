# 🎨 Tag It

> **A full-stack iOS app to discover, document, and save street art — built in 5 weeks as part of the Apple Foundation Program Advanced.**

<div align="center">
<br>
<a href="https://www.balystick.fr/Github/Tag%20It.mp4">
    <img src="https://www.balystick.fr/Github/Tag%20It%20logo.png" alt="Tag It App" style="width:340px">
</a>
<br><br>
<em>▶ Click the logo to watch the demo video</em>
</div>

--- 

## 📌 Problem Statement

Street art is ephemeral — murals get painted over, pieces disappear, and there is no centralised, accessible way to document or find them. Artists and enthusiasts lack a tool to reference, share, and locate works. Tag It addresses this gap with a mobile app that lets anyone contribute to a living, geo-located catalogue of street art, making the invisible visible.

---

## 🧩 Project Overview

Tag It is a group project built during **Mini Challenge 3 (Zeus)** of the [Apple Foundation Program Advanced](https://www.apple.com/fr/newsroom/2020/10/apple-and-france-partner-to-expand-the-apple-foundation-program-for-developers/) in Paris (18 September – 22 October 2024, 5 weeks). The challenge brief required us to design a coherent data flow, build a REST API with its own database using Vapor, apply concurrency practices for smooth UX, and follow AGILE methods throughout.

The result is a SwiftUI iOS app backed by a Vapor server: users can browse street art works in a list or on an interactive map, view individual work details, save favourites, and add new works.

---

## 🛠️ Tech Stack

### 🌐 Languages
- Swift 5
- SQL

### 📦 Frameworks & Libraries
- SwiftUI — Declarative iOS UI
- MapKit — Interactive geo-located map
- Vapor — Swift server-side REST API framework
- Fluent — Vapor's ORM for database modelling
- JWT (JSON Web Tokens) — Stateless authentication

### 🗄️ Databases & Storage
- PostgreSQL / SQLite — Via Vapor Fluent migrations

### ☁️ Infrastructure & DevOps
- AGILE / Scrum — Sprint planning, daily standups
- Git / GitHub — Version control and collaboration

### 🔧 Tools & Other
- UML & Merise — Data and system modelling (challenge requirement)
- CBLD framework — User needs identification
- Swift Concurrency (`async`/`await`) — Smooth, non-blocking API calls

---

## ✨ Features

- **Works List** — Browse all referenced street art works; tap any to view full details (title, artist, description, photo).
- **Work Detail** — Individual view with high-res image, metadata, and location.
- **Favourites** — Add or remove works from a dedicated favourites section, persisted per user.
- **Interactive Map** — Geo-located pins for every work; browse by location and tap pins to preview.
- **REST API Backend** — Full CRUD endpoints served by Vapor, with JWT-secured routes for user-specific actions.

---

## 🏗️ Architecture & Technical Details

### System Design

The app follows a **client-server architecture**: the SwiftUI iOS client communicates with a Vapor backend over REST. The backend owns the database and enforces authentication via JWT; the client is stateless between sessions. On the iOS side, views are driven by observable ViewModels that call async service layers.

### Data Flow

User action → SwiftUI View → ViewModel calls `async` service function → URLSession HTTP request to Vapor API → Fluent query against database → JSON response → decoded model → `@Published` property update → view re-renders.

### Key Technical Decisions

- **Vapor over a hosted BaaS**: The challenge explicitly required building a REST API with its own database. Vapor let us write server-side Swift, keeping the codebase in one language across client and server.
- **JWT authentication**: Stateless tokens avoid server-side session storage and scale cleanly — a natural fit for a REST API and a challenge-specified requirement.
- **Swift Concurrency (`async`/`await`)**: Required by the challenge to handle concurrent requests smoothly. Avoids callback pyramids and integrates naturally with SwiftUI's `Task {}` and `.task {}` view modifiers.
- **MapKit for geolocation**: Native, zero-dependency map with annotation clustering — fits seamlessly into SwiftUI and requires no third-party map SDK.
- **AGILE sprints**: With a 5-week timeline and a group, we ran weekly sprints with backlog grooming and reviews to stay on scope.

### Database Schema

```
User ──< Favourite >── Work
  id          user_id       id
  username    work_id       title
  passwordHash              artist
                            description
                            imageURL
                            latitude
                            longitude
```

### API Overview

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/api/works` | Public | List all street art works |
| GET | `/api/works/:id` | Public | Get a single work by ID |
| POST | `/api/works` | JWT | Add a new work |
| DELETE | `/api/works/:id` | JWT | Delete a work |
| GET | `/api/favourites` | JWT | Get current user's favourites |
| POST | `/api/favourites/:workId` | JWT | Add a work to favourites |
| DELETE | `/api/favourites/:workId` | JWT | Remove a work from favourites |
| POST | `/api/register` | Public | Register a new user |
| POST | `/api/login` | Public | Authenticate and receive JWT |

---

## 📁 Project Structure

```
Tag-It/
├── iOS/                        # SwiftUI client
│   ├── Views/
│   │   ├── WorkListView.swift  # Browse all works
│   │   ├── WorkDetailView.swift# Individual work detail
│   │   ├── FavouritesView.swift# Saved works
│   │   └── MapView.swift       # Geo-located map
│   ├── ViewModels/             # ObservableObjects driving views
│   ├── Models/                 # Codable data models
│   ├── Services/               # Async API call layer
│   └── TagItApp.swift          # App entry point
└── Server/                     # Vapor backend
    ├── Sources/App/
    │   ├── Controllers/        # Route handlers
    │   ├── Models/             # Fluent database models
    │   ├── Migrations/         # Schema migrations
    │   └── configure.swift     # App configuration & middleware
    └── Package.swift           # Swift Package Manager manifest
```

---

## ⚙️ Getting Started

### Prerequisites

- Xcode 15+
- Swift 5.9+
- [Vapor toolbox](https://docs.vapor.codes/install/macos/) (`brew install vapor`)
- PostgreSQL or SQLite (for local backend)

### Running the iOS App

```bash
# 1. Clone the repository
git clone https://github.com/audreyhda/Tag-It.git
cd Tag-It

# 2. Open the Xcode project
open iOS/TagIt.xcodeproj

# 3. Select a simulator or device and press Run (⌘R)
```

### Running the Vapor Server

https://github.com/audreyhda/Tag-It-Server

```bash
cd Server

# Install dependencies
swift package resolve

# Run migrations
swift run App migrate

# Start the server (default: http://localhost:8080)
swift run
```

### Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `DATABASE_URL` | PostgreSQL connection string | ✅ |
| `JWT_SECRET` | Secret used to sign JWT tokens | ✅ |
| `PORT` | Server port (default: 8080) | ⬜ optional |

---

## 🗺️ Roadmap

- [x] Works list with detail view
- [x] Favourites system with dedicated section
- [x] Interactive geo-located map (MapKit)
- [x] Vapor REST API with Fluent ORM
- [x] JWT authentication
- [x] Swift Concurrency throughout
- [ ] Image upload (direct from camera)
- [ ] User profiles and artist pages
- [ ] Search and filter by neighbourhood or style

---

## 👥 Team

Built in group during Mini Challenge 3 (Zeus) of the **Apple Foundation Program Advanced**, Paris — September–October 2024.

- GitHub: [@audreyhda](https://github.com/audreyhda)

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

*Apple Foundation Program Advanced · Mini Challenge 3: Zeus · Built with ❤️ and SwiftUI in Paris*

