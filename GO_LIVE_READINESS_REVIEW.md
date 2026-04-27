# SaaS Go-Live Readiness Review (Senior-Architekt-Check)

> Stand: 2026-04-27
> Scope in diesem Repo: Der Codebestand ist aktuell leer (nur Git-Metadaten).
> Ergebnis: Für ein echtes Go-Live fehlt die gesamte technische Lieferbasis.

## 1) Executive Summary

**Go-Live Entscheidung:** ❌ **NO-GO**

Ein produktiver Release ist aktuell nicht möglich, da keine lauffähige Anwendung, keine Infrastrukturdefinition, keine Sicherheitskontrollen, keine Tests und keine Betriebsdokumentation vorhanden sind.

## 2) Befund (Ist-Zustand)

- Kein Anwendungscode vorhanden.
- Keine CI/CD-Pipeline definiert.
- Keine IaC/Deployment-Artefakte (z. B. Terraform, Helm, Docker Compose, K8s Manifeste).
- Keine Security-Baseline (Secrets-Management, SAST/DAST, Dependency-Scanning, Policies).
- Keine Monitoring/Alerting-Konfiguration.
- Keine Runbooks, SLAs/SLOs, Incident-Prozess.

## 3) Go-Live Gates (müssen grün sein)

### Gate A — Product & Architektur
- [ ] Zielarchitektur dokumentiert (C4: Kontext, Container, Komponenten).
- [ ] NFRs definiert (Verfügbarkeit, Performance, Recovery, Compliance).
- [ ] Datenklassifizierung und Schutzbedarf je Datenobjekt festgelegt.

### Gate B — Engineering Quality
- [ ] Unit-, Integrations-, E2E-Tests vorhanden.
- [ ] Mindest-Testabdeckung festgelegt und erzwungen.
- [ ] Linting/Format/Static Analysis obligatorisch im CI.

### Gate C — Security & Compliance
- [ ] Threat Modeling durchgeführt (Top-Angriffsflächen dokumentiert).
- [ ] Secrets niemals im Repo; Vault/KMS-Anbindung vorhanden.
- [ ] Dependency Scans (SCA), SAST und Container Scans aktiv.
- [ ] AuthN/AuthZ mit Rollenmodell und Least Privilege.
- [ ] DSGVO/Privacy-Funktionen (Auskunft/Löschung/Retention) spezifiziert.

### Gate D — Platform & Operations
- [ ] Reproduzierbare Deployments (CI/CD + IaC).
- [ ] Blue/Green oder Canary Rollout mit schnellem Rollback.
- [ ] Observability: Logs, Metriken, Traces mit Correlation-ID.
- [ ] SLOs + Alerting + On-Call Routing definiert.

### Gate E — Business Readiness
- [ ] Supportprozess und Eskalationspfad.
- [ ] Status Page + Kundenkommunikation für Incidents.
- [ ] Billing/Entitlements/Auditierbarkeit geprüft.

## 4) Priorisierte Maßnahmen (0–30 Tage)

## 0–7 Tage (P0)
1. Projektgrundlage erstellen: Runtime, Framework, Service-Topologie.
2. CI-Pipeline aufsetzen: Build, Test, Lint, Security Scans.
3. Umgebungsstrategie definieren: dev/stage/prod + Secrets-Handling.
4. Basis-Observability integrieren (OpenTelemetry + zentraler Log Sink).

## 8–14 Tage (P1)
1. Kern-Use-Cases als API + Persistenz implementieren.
2. AuthN/AuthZ inkl. Rollen und Session/Token-Strategie.
3. Datenbankmigrationen + Backup/Restore-Test.
4. Lasttest-Szenarien für kritische Endpunkte.

## 15–30 Tage (P1/P2)
1. Hardening: Rate Limits, WAF/CDN, DDoS-Baseline.
2. DR-Übung (RTO/RPO gegen Zielwerte validieren).
3. Chaos/Failure-Tests für zentrale Abhängigkeiten.
4. Betriebsübergabe mit Runbooks und Bereitschaft.

## 5) Minimaler Zielzustand vor erstem Kundenverkehr

- Erfolgreicher Deployment-Run in Stage und Production.
- Sicherheits-Scan ohne kritische Findings.
- P95-Latenz und Error-Budget innerhalb SLO.
- Nachweisbarer Rollback in < 15 Minuten.
- Dokumentierter Incident Drill mit Team.

## 6) Risikobewertung

| Risiko | Eintritt | Impact | Bewertung | Gegenmaßnahme |
|---|---:|---:|---:|---|
| Fehlender Code/Architektur | Hoch | Sehr hoch | Kritisch | Entwicklungsbasis + Architektur sofort aufsetzen |
| Keine Security-Kontrollen | Hoch | Sehr hoch | Kritisch | CI Security Gates + Secret Management |
| Kein Betriebskonzept | Hoch | Hoch | Kritisch | SLO/Alerting/Runbooks + On-Call |
| Unklare Compliance | Mittel | Hoch | Hoch | Datenschutz- und Audit-Anforderungen implementieren |

## 7) Abnahme-Kriterien für GO

**GO nur wenn alle Bedingungen erfüllt sind:**
1. Alle Gates A–E sind grün.
2. Kein kritischer/hoher Security Finding offen.
3. Lasttest und Failover-Test bestanden.
4. Stakeholder-Signoff (Engineering, Security, Product, Operations).

---

Wenn du willst, kann ich im nächsten Schritt direkt ein **konkretes Ziel-Setup** (z. B. Node.js + Postgres + Redis + Terraform + GitHub Actions) mit vollständiger Ordnerstruktur und Start-Pipeline in diesem Repo anlegen.
