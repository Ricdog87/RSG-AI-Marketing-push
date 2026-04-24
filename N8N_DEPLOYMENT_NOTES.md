# n8n Deployment Notes – RSG AI Social Auto Posting

## Zweck
Diese Datei beschreibt den **GitHub-Source-of-Truth** für den produktionsreifen Workflow.
Sie behauptet **kein bereits erfolgtes Live-Deployment**, sondern dokumentiert den Import-/Go-live-Ablauf sauber und reproduzierbar.

## Enthaltene Workflow-Datei
- `n8n_rsg_social_autopost_workflow.json`
- Workflow-Name im Export: `RSG AI - Auto Social Posting (LinkedIn + IG + FB)`
- Status im Export: `active: false` (absichtlich, um vor Go-live Konfiguration zu erzwingen)

## Kanalabdeckung
Der Workflow publisht automatisch auf:
1. Facebook Page (Feed Post)
2. Instagram Business (Container + Publish)
3. LinkedIn Company Page (UGC Post)

## Ablauf im Workflow
1. **Schedule Trigger** (täglich um 09:00).
2. **Build Runtime Config**: zentrale Runtime-Variablen (Page Name, Tokens, LinkedIn Org URN, URLs).
3. **Validate Config**: stoppt den Lauf, wenn Platzhalter (`SET_ME_*`) noch gesetzt sind.
4. **Generate Content**: erstellt kanal-spezifische Texte (FB, IG, LinkedIn) inkl. CTA.
5. **Facebook/Instagram-Zweig**:
   - `Get FB Pages`
   - `Select Page`
   - `Publish Facebook Post`
   - `Get IG Business Account`
   - `Create IG Media Container`
   - `Publish IG Post`
6. **LinkedIn-Zweig**:
   - `Publish LinkedIn Post` via `https://api.linkedin.com/v2/ugcPosts`

## Pflicht-Konfiguration vor Aktivierung
In Node **Build Runtime Config** setzen:
- `meta_access_token` (Long-Lived Token)
- `linkedin_access_token`
- `linkedin_org_urn` (`urn:li:organization:<ID>`)
- optional: `image_url`, `target_facebook_page`, `website_url`

## Benötigte Berechtigungen
### Meta (Facebook + Instagram)
- `pages_manage_posts`
- `pages_read_engagement`
- `instagram_basic`
- `instagram_content_publish`
- `business_management`

### LinkedIn
- Für Organisation-Posting benötigst du einen App-Flow mit gültigem Access Token und Page-Rechten.
- Übliche Scopes (abhängig von App-Freigabe): `w_organization_social`, ggf. `r_organization_social`.

## Go-live Checkliste
1. Workflow importieren.
2. Konfigurations-Platzhalter ersetzen.
3. Einmal manuell ausführen.
4. Prüfen, dass 3 API-Zweige grün laufen (FB, IG, LinkedIn).
5. Erst dann Workflow aktivieren.

## Troubleshooting (Kurz)
- **`Get FB Pages` 400**: Token/Scopes/Business-Verknüpfung prüfen.
- **IG Publish fail**: `instagram_business_account` fehlt meist bei falscher Page-IG-Verknüpfung.
- **LinkedIn 401/403**: falscher Scope oder kein Adminrecht auf Company Page.
