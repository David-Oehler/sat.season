# CargoBeamer Opportunity Map – Umsetzungsplan

## 1) Zielbild
Eine interne Web-App, in der Sales und Terminal Operations auf einer Europakarte:
- potenzielle Kunden verorten,
- Interessen an neuen Routen dokumentieren,
- potenzielle neue Terminals bewerten,
- und den bisherigen Recherche- und Gesprächsverlauf transparent nachverfolgen.

Die App soll Entscheidungen zu neuen CargoBeamer-Routen datenbasiert unterstützen.

## 2) Kernanforderungen (MVP)

### Nutzer & Rollen
- **Sales**: Kunden anlegen, Interessen dokumentieren, Nachfrage bewerten.
- **Terminal Ops**: Terminal-Potenziale bewerten, Routenmachbarkeit ergänzen.
- **Admin**: Stammdaten pflegen, Rechte verwalten.

### Domänenobjekte
- **Customer**
  - Name, Branche, Standort (Geo), Region/Land
  - Potenzielle Transportmenge (z. B. TEU/Jahr)
  - Produktfit (1–5), Status (Lead/Qualified/Active)
- **Terminal**
  - Name, Standort (Geo), Status (bestehend/geplant/potentiell)
  - Infrastrukturparameter (z. B. Gleislänge, Slots, Restriktionen)
- **Route Opportunity**
  - Startterminal, Zielterminal
  - Nachfrageindikator (1–5), geschätztes Volumen
  - Zeitrahmen, Reifegrad, Verantwortliche
- **Interaction/History**
  - Wer hat wann mit welchem Kunden/zu welcher Route gesprochen
  - Notizen, nächste Schritte, Quelle

### Kartenfunktionen
- Marker-Layer für Kunden, Terminals, Routen.
- Filter (Land, Region, Interesse, Volumen, Status, Team).
- Cluster bei hoher Marker-Dichte.
- Detail-Drawer bei Klick mit Historie und Kennzahlen.

## 3) Empfohlene Architektur (Best Practice)

### Frontend
- **React + TypeScript + Vite**
- Karten-Stack: **MapLibre GL JS** (Open-Source, gut mit OSM/OpenRail/Vector Tiles)
- UI: z. B. MUI oder Tailwind + Headless UI
- Datenabfrage: React Query

### Backend
- **NestJS** oder **FastAPI** (beide sehr geeignet)
- REST API (später optional GraphQL)
- AuthN/AuthZ via OpenID Connect (z. B. Azure AD / Entra ID)

### Datenbank
- **PostgreSQL + PostGIS** für Geodaten
- Audit-Trail Tabellen für Änderungen und Verlauf

### Betrieb
- Docker-Container für Frontend/Backend/DB
- CI/CD (GitHub Actions)
- Hosting z. B. Azure App Service / AKS / Render / Fly.io
- Monitoring mit OpenTelemetry + zentralem Logging

## 4) Datenmodell (erste Version)

### Tabellen
- `users`
- `customers`
- `terminals`
- `route_opportunities`
- `customer_route_interest` (N:M)
- `customer_terminal_interest` (N:M)
- `interactions`
- `attachments`
- `audit_log`

### Geofelder
- `geom POINT (SRID 4326)` für Customer/Terminal
- optionale `LINESTRING` für Route-Geometrien

## 5) Schritt-für-Schritt Roadmap

### Phase 0 – Discovery (1–2 Wochen)
1. Workshop mit Sales + Terminal Ops: Datenfelder finalisieren.
2. KPI-Definition: Welche Kennzahlen steuern die Priorisierung?
3. Datenschutz-Check (personenbezogene Notizen, Zugriffsrechte).

### Phase 1 – MVP-Backend (1–2 Wochen)
1. Datenmodell in PostgreSQL/PostGIS aufsetzen.
2. CRUD-APIs für Customer, Terminal, Route, Interaction.
3. Rollen- und Rechtemodell implementieren.
4. Seed-Daten + API-Dokumentation (OpenAPI).

### Phase 2 – MVP-Frontend (2–3 Wochen)
1. Kartenansicht mit Layern (Customer/Terminal/Route).
2. Formularflows zum Anlegen/Bearbeiten.
3. Filter, Suche, Detail-Drawer.
4. Verlaufsansicht je Kunde/Route/Terminal.

### Phase 3 – Qualität & Transparenz (1 Woche)
1. Audit-Log im UI sichtbar machen.
2. Export (CSV/Excel) für Reports.
3. Dashboard (Heatmap, Top-Regionen, Top-Routen).

### Phase 4 – Pilot & Rollout (1–2 Wochen)
1. Pilotregion definieren (z. B. DACH + Benelux).
2. Feedback-Schleifen mit Key Usern.
3. Iterative Verbesserungen + Go-live.

## 6) Priorisierungslogik (Beispiel)
Ein Opportunity-Score (0–100):
- Nachfrageinteresse (1–5) → 30%
- Potenzielles Volumen → 30%
- Machbarkeit Terminal/Route → 20%
- Strategischer Fit (Region/Branche) → 20%

So werden Regionen und neue Routen objektiver vergleichbar.

## 7) Sicherheits- und Governance-Standards
- SSO + rollenbasierte Rechte (Least Privilege)
- Pflichtfelder + Validierungen für Datenqualität
- Versionierung kritischer Felder (Interesse, Volumen, Status)
- Löschkonzept & Backup-Strategie

## 8) Konkreter Start (Nächster Schritt)
1. Entscheiden: **FastAPI oder NestJS**.
2. Datenfelder gemeinsam finalisieren (60–90 Min Workshop).
3. Ich erstelle danach:
   - initiales ERD,
   - API-Spezifikation,
   - und ein lauffähiges MVP-Grundgerüst.

---

Wenn du möchtest, gehen wir im nächsten Schritt direkt in **Phase 0** und ich führe dich durch einen strukturierten Anforderungsslot (inkl. fertiger Fragenliste), damit wir sofort mit dem technischen Setup starten können.
