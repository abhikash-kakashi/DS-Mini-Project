# CampusRide — Smart Campus Mobility

> Smart Mobility. Zero Campus Hassle.

A full-stack, campus-exclusive ride booking and transportation management web platform built as an academic Data Structures & Algorithms showcase. The application coordinates campus electric scooters, electric bikes, and bicycles to provide safe, affordable, and punctual transit across university grounds without third-party API dependencies or keys.

---

## 🌟 Key Features

1. **Passenger Ride Booking**:
   - Interactive selection across 14 predefined university landmarks.
   - 3 Ride tiers: **Standard Ride**, **Shared Ride (Pool)**, and **Priority Ride**.
   - Real-time pathfinding, distance estimation, and dynamic fare computation.
   - Live trip tracking with status progression: `Requested` → `Searching` → `Accepted` → `Driver Arriving` → `In Progress` → `Completed`.

2. **Rider / Driver Operations**:
   - Status toggle: `Available`, `Busy`, `Offline`.
   - Incoming FIFO request feed with Accept and Reject actions.
   - Dedicated trip execution workflow with route map visualization.
   - Driver profile metrics: rating, completed trips, and daily earnings.

3. **Administrator Control Center**:
   - Comprehensive mobility metrics: active rides, fleet distribution, student counts.
   - Fleet management: onboard new drivers, toggle accounts, and view vehicle numbers.
   - Booking records search and status filtering.
   - One-click demo controls: **Reset Demo Data** and **Run Simulation**.

4. **Interactive SVG Campus Map**:
   - Custom illustrated campus map with 14 buildings and 23 bidirectional pathways.
   - Real-time animated route lines calculated using Dijkstra's algorithm.
   - Visual radar beacons indicating available EV drivers and vehicle positions.

5. **DSA Lab & Algorithm Visualizers**:
   - Dedicated dashboard showcasing the 6 core data structures driving the platform.
   - Interactive visual manipulation tools with complexity tables and live metrics.

---

## 🧠 Data Structures & Algorithms (Core Implementation)

| Data Structure | CampusRide Real-World Role | Time Complexity | Space Complexity |
| :--- | :--- | :--- | :--- |
| **FIFO Queue** | Buffers incoming passenger ride requests in arrival order to guarantee fairness. | Enqueue: $O(1)$<br>Dequeue: $O(1)$ | $O(n)$ |
| **Graph (Adjacency List)** | Models campus topology; runs Dijkstra's algorithm for shortest walkable routes and BFS/DFS for node exploration. | Dijkstra: $O((V+E)\log V)$<br>BFS/DFS: $O(V+E)$ | $O(V+E)$ |
| **Hash Table** | Provides instant key lookup for students, fleet drivers, and active trip IDs using polynomial rolling hashing and separate chaining. | Search: $O(1)$ avg<br>Insert: $O(1)$ avg | $O(n+k)$ |
| **Priority Queue (Binary Max-Heap)** | Dispatches the optimal driver based on proximity, arrival ETA, rating, and requested ride priority score. | Insert: $O(\log n)$<br>Extract-Max: $O(\log n)$ | $O(n)$ |
| **Singly Linked List** | Maintains a dynamic chronological timeline of completed trips with $O(1)$ head insertion. | Insert Head: $O(1)$<br>Traverse: $O(n)$ | $O(n)$ |
| **Stack (LIFO)** | Records audit logs of state transitions and powers instant **Undo** for reversible actions. | Push: $O(1)$<br>Pop: $O(1)$ | $O(n)$ |

### Priority Score Formula
The Binary Max-Heap ranks available drivers using the following equation:
$$\text{Priority Score} = \text{Distance Score} + \text{ETA Score} + \text{Rating Bonus} + \text{Tier Bonus}$$
- **Distance Score**: Up to 40 pts, decreasing with distance from pickup.
- **ETA Score**: Up to 30 pts, decreasing with arrival minutes.
- **Rating Bonus**: Up to 20 pts based on driver rating out of 5.0.
- **Tier Bonus**: Priority Ride (+15), Shared Pool (+8), Standard (+3).

---

## 🛠️ Technology Stack

- **Frontend**:
  - React 18, TypeScript, Vite
  - Tailwind CSS, Lucide React icons
  - React Router DOM v6
  - Recharts (mobility analytics & route volume)
  - Custom SVG Campus Cartography Engine
- **Backend**:
  - Node.js, Express.js, TypeScript
  - JSON-based persistent disk storage (`server/src/data/db.json`)
  - Pure TypeScript DSA engine shared between client and server
- **Testing**:
  - Automated test runner verifying all 6 data structures, pathfinding, and state machines.

---

## 📂 Project Structure

```
campusride/
├── client/
│   ├── src/
│   │   ├── components/
│   │   │   ├── common/       # Buttons, cards, badges, modals, KPI widgets
│   │   │   ├── layout/       # Sidebar, TopNav, BottomNav
│   │   │   ├── map/          # Interactive SVG CampusMap
│   │   │   └── dsa/          # 6 interactive DSA visualizers
│   │   ├── context/          # AuthContext, RideContext, ThemeContext, ToastContext
│   │   ├── dsa/              # Client-side DSA algorithms
│   │   ├── data/             # Campus coordinates & road network topology
│   │   ├── pages/            # Dashboard, Booking, RiderPortal, Admin, Map, DsaLab, Settings, Login
│   │   ├── services/         # REST API client
│   │   └── types/            # TypeScript data definitions
│   └── index.html
├── server/
│   ├── src/
│   │   ├── controllers/      # REST endpoint controllers
│   │   ├── data/             # Campus topology & initial JSON fixtures
│   │   ├── dsa/              # Server-side DSA algorithms
│   │   ├── routes/           # Express API route declarations
│   │   ├── services/         # DispatchService, GraphService, StorageService
│   │   ├── tests/            # Automated test suite (57 unit tests)
│   │   └── index.ts          # Express server entry point
│   └── package.json
├── package.json              # Monorepo root workspace scripts
├── LICENSE                   # MIT License
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites
- **Node.js** (v18.0 or higher)
- **npm** (v8.0 or higher)

### Installation
```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/campusride.git
cd campusride

# Install root, client, and server dependencies
npm run install:all
```

### Running Locally
To launch both the Express backend API (Port 5000) and the Vite React frontend (Port 5173) simultaneously:
```bash
npm run dev
```

Then open your browser at:
```
http://localhost:5173
```

### Running the Test Suite
Execute the automated DSA test suite verifying all 6 data structures and dispatch workflows:
```bash
npm test
```

---

## 👥 Demo Accounts

The login screen provides 1-click authentication buttons for testing each role:

| Role | Email | Password | Preloaded Identity |
| :--- | :--- | :--- | :--- |
| **Passenger** | `passenger@campusride.demo` | `passenger123` | Arjun Kumar (STU1001, CSE) |
| **Rider** | `rider@campusride.demo` | `rider123` | Santhosh (RID2001, Electric Scooter CR-101) |
| **Admin** | `admin@campusride.demo` | `admin123` | University Mobility Administrator |

> *Note: Demo authentication is implemented for academic presentation and prototype evaluation.*

---

## 📡 REST API Reference

### Mobility & Rides
- `GET /api/dashboard` — Aggregated KPI metrics, route popularity, and hourly trends.
- `GET /api/rides` — List all rides (with optional status/passenger/rider filters).
- `POST /api/rides` — Create a new ride request (enqueues into FIFO Queue).
- `GET /api/rides/:id` — Retrieve single ride details.
- `PATCH /api/rides/:id/status` — Advance ride state machine (`Accepted`, `In Progress`, `Completed`, `Cancelled`).
- `POST /api/rides/:id/assign` — Assign optimal driver via Binary Max-Heap Priority Queue.
- `DELETE /api/rides/:id` — Cancel a pending ride.

### Campus Navigation & Routing
- `GET /api/campus/locations` — 14 campus hubs with SVG coordinates and category details.
- `GET /api/campus/routes` — Campus adjacency list with weighted pathway distances.
- `POST /api/campus/calculate-route` — Compute shortest path using Dijkstra's algorithm.

### DSA Lab Endpoints
- `GET /api/dsa/queue` — FIFO Queue inspection (front, rear, items array).
- `GET /api/dsa/priority-queue` — Binary Max-Heap inspection and rider ranking scores.
- `GET /api/dsa/hash-table` — Hash Table buckets, chaining lists, and load factor stats.
- `GET /api/dsa/graph` — Graph adjacency lists and vertices.
- `GET /api/dsa/linked-list` — Singly Linked List history chain with node pointers.
- `GET /api/dsa/stack` — Action stack and reversible audit logs.
- `POST /api/dsa/undo` — Revert the most recent reversible action using Stack LIFO.
- `POST /api/demo/reset` — Restore default demo dataset.
- `POST /api/demo/simulate` — Execute end-to-end multi-DSA lifecycle simulation.

---

## 📤 Uploading to GitHub

```bash
# Initialize git if not already present
git init
git add .
git commit -m "feat: complete CampusRide mobility platform with DSA implementations"

# Add your GitHub repository remote
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/campusride.git
git push -u origin main
```

---

## 📄 License
This project is open-source under the MIT License.
