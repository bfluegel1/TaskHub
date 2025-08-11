# TaskHub

> **DE** – Leichte, offline-fähige Aufgaben-PWA für schnelle persönliche Organisation.  
> **EN** – Lightweight, offline-capable task PWA for fast personal tasking.

[Demo ](https://bfluegel1.github.io/TaskHub/index.html)

---

## Status

- **MVP nutzbar** (Single-User, PWA-Client).  
- **Geplant:** Multi-User (Authentifizierung, Rollen, Team-Projekte, Sync/Realtime).

> Hinweis: Diese README trennt bewusst den **Ist-Stand** (MVP) von der **Roadmap** (Multi-User).
> So bleibt das Repo sofort einsetzbar und gleichzeitig klar ausbaubar.

---

## Features (MVP)

- Aufgaben erstellen, bearbeiten, löschen  
- Schneller, fokussierter UI-Flow  
- PWA-Grundlagen (statisches Hosting; Offline-Support abhängig von manifest/service worker Konfiguration)

> Details können je nach Branch/Deployment variieren.

---

## Roadmap (Multi-User)

- [ ] **Auth**: E-Mail + Passwort, JWT (Access/Refresh)  
- [ ] **RBAC**: Owner, Admin, Editor, Viewer (projektbezogen)  
- [ ] **Projekte & Mitglieder**: Einladen, Rechte verwalten  
- [ ] **Sync & Offline**: Delta-Sync (`since`), Queue & Konfliktlösung  
- [ ] **Realtime**: SSE oder WebSocket für Task-Events  
- [ ] **Struktur**: Subtasks, Labels, Kommentare, Aktivitätslog  
- [ ] **Import**: Lokale Daten → Server (Mapping, Dubletten)  
- [ ] **Exports**: CSV/JSON (ggf. PDF)  
- [ ] **Tests**: API-E2E, UI-Smoke, Load (k6)  
- [ ] **Deployment**: Docker Compose, Nginx, HTTPS, Monitoring

---

## Architektur

### Ist (MVP)
- **Frontend**: Statisches Web (HTML/CSS/JS) als **PWA**, Hosting z. B. via GitHub Pages.  
- **Persistenz (lokal)**: Browser-Speicher (z. B. localStorage/IndexedDB).  
- **Offline**: Service Worker (sofern konfiguriert).

### Ziel (Multi-User)
- **Backend**: Node.js/Express (oder Alternative)  
- **DB**: PostgreSQL (oder MySQL)  
- **Auth**: JWT (Access/Refresh), Passwort-Hashing (argon2/bcrypt)  
- **RBAC**: Rollen je Projekt  
- **Sync**: Delta-Pull/Push, ETags/If-Match, Konfliktstrategie  
- **Realtime**: SSE/WebSocket

**Kern-Entitäten (Auszug):**
users(id, email, password_hash, display_name, created_at)
projects(id, name, owner_id, archived, created_at)
members(project_id, user_id, role)
tasks(id, project_id, title, description, status, priority, due_on, assigned_to, created_by, version, updated_at)
subtasks(id, task_id, title, status)
dependencies(id, task_id, depends_on_task_id, type)
comments(id, task_id, author_id, body, created_at)
labels(id, project_id, name, color)
task_labels(task_id, label_id)
activity_log(id, actor_id, entity_type, entity_id, action, payload, ts)

---

## Quickstart (lokal)

> Für PWA/Service-Worker brauchst du **https** oder einen lokalen Static-Server.

```bash
# 1) Repo klonen
git clone https://github.com/<USER>/<REPO>.git
cd <REPO>

# 2) Static-Server starten (eine Option)
# npm i -g http-server
http-server . -p 5173

# 3) Öffnen
# http://localhost:5173/pwa/taskhub2/
# TaskHub
