# GearGuard

GearGuard is a role-based equipment and maintenance management web app built with React + Vite (frontend) and a small Node backend. It helps organizations track equipment, file maintenance requests, manage teams and users, and present role-specific dashboards (Admin / Manager / Technician / Employee).

## Key features
- Role-based dashboards (admin, manager, technician, employee) with automatic routing
- Equipment inventory view and creation
- Maintenance request creation, batch requests, and status workflows
- Team and user management (add/remove team members)
- Calendar view for maintenance events
- Simple auth (login / signup) and client-side persistence (localStorage)

## Stack
- Language: JavaScript (React)
- Frontend: Vite + React + react-router
- Backend: lightweight Node server in /server (API endpoints & DB setup files included)
- Notable code files: src/context/DataContext.jsx (central data / API client), src/App.jsx (route & layout), server/index.js, server/setupDb.js, server/seed.js

## Repo layout
```
/public/                  Static assets served by Vite
/src/
  App.jsx                 Routes & app entry points (Landing, Auth, protected routes)
  main.jsx                Frontend entry
  index.css, App.css      Styling
  context/
    DataContext.jsx       Central data provider, API calls, auth helpers
  components/
    layout/                Layout components (MainLayout)
    dashboard/             Dashboard components by role
    equipment/             Equipment UI components
    maintenance/           Maintenance UI components
  pages/
    LandingPage.jsx
    CalendarPage.jsx
    pages/auth/            LoginPage.jsx, SignupPage.jsx
    pages/dashboards/      Admin/Manager/Technician/Employee dashboards
    pages/admin/           UserManagementPage.jsx, TeamManagementPage.jsx
/server/
  index.js                Express-style API server entry (check this file)
  db.js                   DB connection / helpers
  setupDb.js              DB schema / initialization helpers
  seed.js                 Script to seed development data
  routes/                 Server route handlers
package.json              Root (frontend) package manifest
vite.config.js            Vite config for dev/build
README.md                 (this file)
```

## How to run (development)
1. Clone the repo:
   ```
   git clone https://github.com/Vivek-26-12/GearGuard.git
   cd GearGuard
   ```

2. Start the backend API
   - Install and run the server:
     ```
     cd server
     npm install
     node index.js
     ```
     (If `npm run start` is available in server/package.json you may use it instead.)
   - By default the frontend expects the API at: `http://localhost:3000`. See `src/context/DataContext.jsx` (API_BASE) and `server/index.js` for the backend port.

3. (Optional) Seed development data:
   ```
   node server/seed.js
   ```
   This will populate the local DB with sample teams, users, equipment, departments, categories and more (see `server/seed.js` and `server/setupDb.js`).

4. Start the frontend
   ```
   # from repo root
   npm install
   npm run dev
   ```
   This runs the Vite dev server. Open the URL printed by Vite (usually http://localhost:5173).

## Frontend notes
- src/context/DataContext.jsx is the main client-side data layer. It:
  - Loads teams, users, equipment, requests, departments, categories, locations, teamMembers
  - Exposes auth helpers: login(email, password), signup(userData), logout()
  - Exposes data actions: createRequest, createBatchRequests, updateRequestStatus, createEquipment, createTeam, createCategory, addTeamMember, removeTeamMember, updateUser, deleteUser
- Protected routes redirect to /login when no currentUser is present. Current user is persisted in localStorage under the key `user`.
- Role-based dashboard routing is implemented in App.jsx via DashboardRouter and the role property on currentUser.

## Backend (API) endpoints referenced by the frontend
The frontend expects these endpoints (see DataContext.jsx):
- GET /api/teams
- GET /api/users
- GET /api/equipment
- GET /api/requests
- GET /api/departments
- GET /api/equipment-categories
- GET /api/equipment-locations
- GET /api/team-members
- POST /auth/login
- POST /auth/signup
- POST /api/requests
- POST /api/requests/batch
- PUT /api/requests/:id
- POST /api/equipment
- POST /api/equipment-categories
- POST /api/teams
- POST /api/teams/:teamId/members
- DELETE /api/teams/:teamId/members/:userId
- PUT /api/users/:id
- DELETE /api/users/:id

Inspect `server/routes/` and `server/index.js` for the actual route implementations and any differences to the above list.

## Development & contribution
- Use the DataContext provider to call or add data actions so UI components share one source of truth.
- If you add new API endpoints, update DataContext.jsx to consume them and update client pages/components accordingly.
- Add unit or integration tests (not included) to cover critical flows: auth, request lifecycle, equipment CRUD, and team management.

## Troubleshooting
- If the frontend fails to fetch data, confirm the backend is running and the API_BASE in `src/context/DataContext.jsx` matches your backend URL/port.
- For DB issues, check `server/db.js` and run `node server/setupDb.js` or `node server/seed.js` to initialize/seed the database.

## License
This repository does not include a license file. Add a LICENSE file to clarify terms (MIT, Apache-2.0, etc.) if you plan to make the project public.

## Contact / Maintainer
Repository: https://github.com/Vivek-26-12/GearGuard  
If you have questions or want to contribute, open an issue or a pull request on the repo.
```
