```
# ⚙️ Technische Architektur – AI Outbound CRM

---

## 🎯 Ziel

Modulares System zur:
- Generierung & Anreicherung von Leads
- Automatisierten Outbound-Kommunikation
- Kalendergesteuerter Terminvereinbarung

---

## 🧱 Microservices

### 1. `lead-service`
- Nutzt Puppeteer für Scraping von Internetseiten
- Führt gezielte Google-Suche nach Webseiten durch
- Scrapt Inhalte wie Impressum, Home, About
- Extrahiert Informationen wie Name, Branche oder Mitarbeiteranzahl

→ Output: Rohdaten für AI-Service

---

### 2. `ai-service`
- GPT-Anbindung via OpenAI API
- Promptet Inhalte aus Webseiten
- Erstellt Zusammenfassung + Kontaktdaten
- Gibt JSON zurück: Name, Rolle, Email, Tel., Beschreibung

---

### 3. `email-service`
- Erstellt Outbound-Kampagnen auf Basis von AI-Daten und Templates
- Stellt Tempates, Emails und Kmapgnen bereit
- Nutzt eigene SMTP/Provider für Versand

---

### 4. `calendar-service`
- Google Calendar API (OAuth2)
- Empfängt Request bei Buchung
- Erstellt Termin und sendet Bestätigung an Admin & Lead

---

### 5. `crm-frontend`
- React + Tailwind
- Kampagnen und Templates anlegen, Kontakte sehen, Notizen schreiben
- Erstellen und schicken von Ai-Prompts um Emails und/oder Kampagnen zu enrichen
- Starten und abbrechen von Kampagnen
- E-Mailverlauf & Status pro Lead einsehbar
- Abbildung der Kontakthistorie von Unternehmen und Personen

---

### 6. `user-service`
- JWT Auth & API-Gateway-Funktion
- Microservice-Berechtigungen
- Login fürs CRM

---

### 5. `crm-frontend`
- React + Tailwind
- Kampagnen und Templates anlegen, Kontakte sehen, Notizen schreiben
- Erstellen und schicken von Ai-Prompts sind deaktiviert
- Starten und abbrechen von Kampagnen sind deaktiviert
- E-Mailverlauf & Status pro Lead einsehbar
- Abbildung der Kontakthistorie von Unternehmen und Personen
- Die Datenbank wird regelmäßig per Cronjob zurückgesetzt

---

## 🔁 Ablauf: Vom Lead bis zum Termin

```plaintext
[Puppeteer] => [AI Enrichment] => [Kampagne erstellen] => [E-Mail senden] => [Landingpage mit Kalender] => [Google API]
