# 🧠 AI Outbound CRM – Microservice Architektur für automatisierte Leadgenerierung

Ein modulares CRM-System zur automatisierten Lead-Generierung, KI-Anreicherung und Terminbuchung.

> Dieses Projekt dient als Architektur-Showcase – produktiver Code wurde anonymisiert.

---

## 🚀 Übersicht

Dieses CRM-System automatisiert den B2B-Outbound-Prozess:
1. Webdaten werden mit Puppeteer gesammelt.
2. Die Daten werden per GPT-Integration angereichert.
3. Automatisierte Kampagnen versenden zielgerichtete E-Mails.
4. Interessenten buchen direkt einen Call über Google Calendar.

---

## 🧱 Services (Beispielarchitektur)

| Service          | Aufgabe                                             |
|------------------|-----------------------------------------------------|
| `lead-service`   | Web Scraping + Firmendaten-Erhebung                |
| `ai-service`     | GPT-gestützte Datenanreicherung                    |
| `email-service`  | Kampagnen-Management & automatisierter Versand     |
| `calendar-service` | Google Calendar API – Terminbuchung              |
| `user-service`   | Authentifizierung & Rechte                         |
| `crm-frontend`   | Kampagnen, Kontakte, Gesprächsnotizen              |
| `crm-demo-frontend`   | Kampagnen, Kontakte, Gesprächsnotizen         |
Mehr Details: [ARCHITECTURE.md](./ARCHITECTURE.md)

---

## 📸 Demo

- Live-Demo: [Demo-CRM öffnen](https://demo.crm.v2202210185651204820.powersrv.de/login)
- Use Case: [DEMO.md](./DEMO.md)

---

## 🛠️ Techstack

- **Backend**: Node.js, Express, Sequelize, MySQL, Puppeteer, OpenAI, Google Cloud
- **Frontend**: React, Tailwind, Vite
- **Cross-Stack Tools**: TypeScript, Zod, Jest
- **DevOps**: GitHub Actions, CI/CD, Monorepo mit Nx
- **Auth**: JWT, OAuth2

---

## 📂 Ordnerstruktur

```bash
/apps                            # Enthält alle produktiven Microservices & Frontends
│
├── ai-service/                  # GPT-Anbindung, Kommunikation mit OpenAI API
│   ├── config/                  # OpenAI-Konfigurationen, Model-Settings
│   ├── migrations/              # DB-Migrationen
│   ├── postHelper/              # Prompt-Logik & Verarbeitungshilfen
│   ├── seeders/                 # Beispielhafte Seed-Daten für AI-Tests
│   └── src/                     # Hauptlogik des Services
│
├── lead-service/                # Puppeteer-Scraping + Google-Suche
├── email-service/               # Kampagnen-Handling + E-Mail-Versand
├── calendar-service/            # Google Calendar API (Terminbuchung)
├── user-service/                # Authentifizierung & Rechtevergabe
│
├── website-crm/                 # Haupt-CRM-Frontend (Admin-UI)
│   ├── src/                     # React-Frontend für Kampagnenverwaltung
│   ├── assets/                  # Logos, Bilder, Icons
│   └── tailwind.config.js       # Styling-Konfiguration
│
├── website-thesis/              # Zweite Landingpage / Lead-In (z. B. für Kampagnen)
│
├── website-crm/                 # Öffentliche CRM-Demo für Showcase-Zwecke

/apps/libs                        # Wiederverwendbare Logik, modularisiert durch Nx
├── shared/                       # Globale, serviceübergreifende Bausteine
│   └── src/
│       ├── constants/            # Zentrale Enums, Statuscodes etc.
│       ├── helper/               # Utility-Funktionen, z. B. zodToDto(), sleep()
│       ├── jwt/                  # Token-Verarbeitung
│       ├── errorHandling/        # Fehler-Formatter, zentrale Handler
│       ├── pagination/           # Abstraktion von Paginierung
│       ├── userService/          # DTOs, Validierungen
│       ├── emailService/         # geteilte Typen & Validierung für E-Mail-Logik
│       ├── calendarService/      # Typen, Kalender-Hilfen
│       ├── aiService/            # Prompt-Schemas, OpenAI-Contracts
│       └── websiteThesis/        # Reusable Logic für die Landingpage

├── services/                      # Service-Bausteine, domainbezogen
│   └── src/
│       ├── database/             # Prisma / Sequelize / Raw-Queries
│       ├── oauth/                # OAuth2-Flow-Handling
│       ├── middlewares/         # Express Middlewares (z. B. errorGuard, logUser)
│       ├── services/             # Business-Logik (z. B. userService.ts, leadEnricher.ts)
│       ├── util/                 # kleinere reine Funktionen

├── ui/                            # Wiederverwendbare UI-Komponenten
│   └── src/
│       ├── components/           # z. B. Button, Modal, LeadTable
│       ├── layout/               # z. B. Sidebar, Header, AuthWrapper
│       └── hooks/                # useAuth(), usePagination(), useLeads()


.gitignore                      # Ignoriert sensible Dateien & Node_Modules
.env.example                    # Beispielhafte ENV-Konfigurationen
nx.json                         # Nx-Konfiguration (Monorepo-Struktur)
package.json                    # Root-Abhängigkeiten
```

## 📂 Projektstruktur (Monorepo mit Nx)

### /apps – Services & Frontends
- `ai-service` → GPT-Prompting & Datenanreicherung
- `lead-service` → Unternehmensdaten & Web-Scraping
- `email-service` → Kampagnen & Versand
- `calendar-service` → Google Calendar Integration
- `user-service` → JWT & OAuth2-Authentifizierung
- `website-crm` → CRM-Frontend (React)
- `crm-demo-frontend` → Demo-Variante für Showcase
- `website-thesis` → Landingpage-Frontend

---

### /libs – Wiederverwendbare Module & Logik
- `shared/` → globale Logik wie Zod-Schemas, DTOs, JWT, Error Handling
- `services/` → DB-Access, Middlewares, OAuth, Business-Logik
- `ui/` → wiederverwendbare UI-Komponenten, Hooks, Layouts
