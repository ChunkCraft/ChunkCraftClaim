# SimpleClaimSystem – Differenzen Free vs. Premium
## Bewertungs- und Implementierungsplan für CityCore

> **Dokumenttyp:** maschinenlesbare Feature-Matrix für LLMs und technische Planung  
> **Projekt:** CityCore SMP  
> **Basis:** Xyness/SimpleClaimSystem  
> **Stand der Recherche:** 2026-08-08  
> **Zweck:** Erfassung der öffentlich verfügbaren Unterschiede zwischen der kostenlosen/open-source Version und SimpleClaimSystem V2 (Premium) sowie Priorisierung für eine eigenständige Implementierung in CityCore.

---

## 0. Quellenlage

### Primärquellen

- Öffentliches SimpleClaimSystem Repository: https://github.com/Xyness/SimpleClaimSystem
- SimpleClaimSystem auf Modrinth: https://modrinth.com/plugin/simpleclaimsystem
- SimpleClaimSystem V2 Premium-Seite: https://builtbybit.com/resources/simpleclaimsystem.92437/
- Vom Nutzer bereitgestellter Screenshot des Free/Premium-Vergleichs

### Quelleninterpretation

Die **konkreten Free-vs-Premium-Differenzen** in diesem Dokument stammen primär aus dem bereitgestellten Vergleichsscreenshot.

Die Premium-Seite wird zusätzlich verwendet, um die Funktionalität der Premium-Features fachlich zu konkretisieren.

Die aktuelle öffentliche Repository-/Modrinth-Beschreibung bestätigt, dass die Free-Version bereits zahlreiche Claim-, GUI-, Permission-, Flag- und Integrationsfunktionen besitzt. Die Premium-Seite beschreibt darüber hinaus u. a. 130+ Permissions, 70+ Flags, vier Webkarten, Custom Roles, Audit Logs, Web Dashboard, Discord Webhooks, Claim Respawn, Claim Back, Favorites, Rent, QuickShop-Hikari und weitere Funktionen.

**Wichtig:** Eine Funktion wird hier nur dann als "Free-vs-Premium-Differenz" markiert, wenn der Screenshot sie ausdrücklich als Premium-only ausweist. Zusätzliche Premium-Funktionen aus der aktuellen Premium-Produktbeschreibung werden separat als `ADDITIONAL_PREMIUM_FEATURE` gekennzeichnet, sofern sie im Screenshot nicht enthalten sind.

---

# 1. Prioritätssystem

Die Priorität bezieht sich auf **CityCore**, nicht darauf, wie wichtig das Feature für SimpleClaimSystem allgemein ist.

## Prioritätsklassen

| Priorität | Bedeutung |
|---|---|
| P0 | Fundament / vor produktivem Claim-System zwingend |
| P1 | Hohe Priorität / wichtig für das geplante Gameplay |
| P2 | Sinnvolle Erweiterung / mittlere Priorität |
| P3 | Komfort / Administration / spätere Erweiterung |
| P4 | Nice-to-have / derzeit nicht erforderlich |

## Bewertungsdimensionen

- `gameplay_relevance`: Einfluss auf das eigentliche CityCore-Gameplay
- `architecture_relevance`: Einfluss auf die technische Architektur
- `admin_value`: Nutzen für Moderation/Administration
- `player_value`: direkter Nutzen für Spieler
- `integration_value`: Nutzen durch externe Systeme
- `implementation_complexity`: erwarteter Entwicklungsaufwand
- `priority`: Gesamtpriorität

Bewertungsskala:

```text
0 = irrelevant
1 = gering
2 = mittel
3 = hoch
4 = sehr hoch
5 = kritisch
```

---

# 2. Executive Summary

```yaml
summary:
  total_screenshot_differences: 15
  priority_distribution:
    P0: 0
    P1: 4
    P2: 6
    P3: 4
    P4: 1

  recommendation:
    strategy: "Premium-Funktionalität nicht kopieren; gewünschte Funktionalität eigenständig im MIT-basierten Fork implementieren."
    initial_focus:
      - "Permissions erweitern"
      - "Claim Flags erweitern"
      - "Custom Roles"
      - "Audit Logs"
      - "Custom Claim Icons"
      - "Discord Webhooks"
      - "Default Claim Radius"
      - "Claim Back"
      - "Claim Respawn"
      - "Rent System"

  citycore_specific:
    most_important:
      - "Custom Roles"
      - "Claim Flags"
      - "Permissions"
      - "Rent System"
      - "Audit Logs"
    lower_priority:
      - "SquareMap"
      - "Web Dashboard"
      - "Favorites"
      - "QuickShop-Hikari"
```

---

# 3. Feature-Matrix

## 3.1 SquareMap Support

```yaml
id: SCS-PREM-001
name: "SquareMap Support"
category: "integration"
free: false
premium: true
premium_delta: "Free: nicht enthalten; Premium: SquareMap-Integration"
priority: P2

citycore:
  gameplay_relevance: 2
  architecture_relevance: 2
  admin_value: 2
  player_value: 3
  integration_value: 5
  implementation_complexity: 3

implementation:
  feasible: true
  recommended: true
  approach:
    - "SquareMap API/Extension anbinden"
    - "Claim-Geometrien als Kartenflächen darstellen"
    - "Owner/City/Claim-Metadaten als Popup bereitstellen"
    - "Darstellung für normale, Stadt- und ggf. umkämpfte Claims unterscheiden"

reason:
  "CityCore wird langfristig stark von der visuellen Darstellung von Städten und Claims profitieren."

source:
  screenshot: true
  premium_description: true
```

## 3.2 Erweiterte Permissions

```yaml
id: SCS-PREM-002
name: "Erweiterte Permissions"
category: "security"
free: true
premium: true
free_scope: "ca. 30 Permissions"
premium_scope: "ca. 120 Permissions"
premium_delta: "+ca. 90 zusätzliche Permissions"
priority: P1

citycore:
  gameplay_relevance: 5
  architecture_relevance: 5
  admin_value: 5
  player_value: 5
  integration_value: 3
  implementation_complexity: 3

implementation:
  feasible: true
  recommended: true
  approach:
    - "Permission-Key/Enum zentralisieren"
    - "ClaimRole -> PermissionSet modellieren"
    - "Permission Registry für erweiterbare Permissions"
    - "Events und Protection-Checks auf zentrale Permission-Abfrage umstellen"

reason:
  "Permissions sind Kernbestandteil des Claim- und späteren City-Systems."

source:
  screenshot: true
  premium_description: true
```

## 3.3 Erweiterte Claim Flags

```yaml
id: SCS-PREM-003
name: "Claim Flags"
category: "protection"
free: true
premium: true
free_scope: "Basis-Flags vorhanden"
premium_scope: "ca. 60 zusätzliche/erweiterte Flags"
premium_delta: "+ca. 60 Flags laut Vergleich"
priority: P1

citycore:
  gameplay_relevance: 5
  architecture_relevance: 5
  admin_value: 5
  player_value: 5
  integration_value: 3
  implementation_complexity: 4

implementation:
  feasible: true
  recommended: true
  approach:
    - "ClaimFlag als stabile Domänenabstraktion"
    - "Flag Registry statt hart codierter Einzelprüfungen"
    - "Default-Werte je Claim/Role"
    - "City-/Raid-spezifische Flags vorbereiten"
    - "World-spezifische Overrides ermöglichen"

recommended_initial_flags:
  - "BUILD"
  - "BREAK"
  - "CONTAINERS"
  - "DOORS"
  - "BUTTONS"
  - "LEVER"
  - "REDSTONE"
  - "PISTON"
  - "HOPPER"
  - "FIRE_SPREAD"
  - "EXPLOSIONS"
  - "PVP"
  - "ENTITY_DAMAGE"
  - "MOB_SPAWN"
  - "PROJECTILES"
  - "TELEPORT"
  - "ENTER"
  - "VEHICLES"

reason:
  "Flags bilden einen großen Teil der eigentlichen Claim-Schutzlogik."

source:
  screenshot: true
  premium_description: true
  free_feature_list: true
```

## 3.4 Custom Icon pro Claim

```yaml
id: SCS-PREM-004
name: "Custom Icon per Claim"
category: "ui"
free: false
premium: true
priority: P2

citycore:
  gameplay_relevance: 2
  architecture_relevance: 2
  admin_value: 1
  player_value: 4
  integration_value: 1
  implementation_complexity: 1

implementation:
  feasible: true
  recommended: true
  approach:
    - "Claim.icon als Material/Item-Definition"
    - "GUI verwendet Claim.icon"
    - "Optional später CustomModelData/Item Model"

reason:
  "Sehr geringer Aufwand und hoher UX-Nutzen."

source:
  screenshot: true
  premium_description: true
```

## 3.5 Advanced Actions in GUIs

```yaml
id: SCS-PREM-005
name: "Advanced GUI Actions"
category: "ui"
free: false
premium: true
priority: P2

citycore:
  gameplay_relevance: 2
  architecture_relevance: 4
  admin_value: 4
  player_value: 4
  integration_value: 2
  implementation_complexity: 3

implementation:
  feasible: true
  recommended: true
  approach:
    - "GUI Action Registry"
    - "Click-Type Mapping"
    - "Command Action"
    - "Sound Action"
    - "Open GUI Action"
    - "Permission-gated Actions"
    - "MiniMessage-Unterstützung"

reason:
  "Eine deklarative GUI-Action-Schicht reduziert später den Entwicklungsaufwand für City-, Claim- und Admin-GUIs."

source:
  screenshot: true
  premium_description: true
```

## 3.6 Discord Webhook

```yaml
id: SCS-PREM-006
name: "Discord Webhook"
category: "integration"
free: false
premium: true
priority: P2

citycore:
  gameplay_relevance: 2
  architecture_relevance: 2
  admin_value: 4
  player_value: 3
  integration_value: 5
  implementation_complexity: 1

implementation:
  feasible: true
  recommended: true
  approach:
    - "HTTP Webhook Client"
    - "Event-basierte Benachrichtigungen"
    - "Asynchroner Versand"
    - "Rate-Limit/Retry-Handling"
    - "Konfigurierbare Embeds"

recommended_events:
  - "CLAIM_CREATED"
  - "CLAIM_DELETED"
  - "CLAIM_TRANSFERRED"
  - "CLAIM_SOLD"
  - "CLAIM_PURCHASED"
  - "MEMBER_ADDED"
  - "MEMBER_REMOVED"
  - "PLAYER_BANNED"
  - "PLAYER_KICKED"
  - "CITY_CREATED"
  - "CITY_DISSOLVED"
  - "RAID_STARTED"
  - "RAID_ENDED"

source:
  screenshot: true
  premium_description: true
```

## 3.7 Web Dashboard

```yaml
id: SCS-PREM-007
name: "Web Dashboard"
category: "web"
free: false
premium: true
priority: P3

citycore:
  gameplay_relevance: 2
  architecture_relevance: 4
  admin_value: 5
  player_value: 3
  integration_value: 5
  implementation_complexity: 5

implementation:
  feasible: true
  recommended: true
  phase: "später"
  approach:
    - "REST API über Application Services"
    - "JWT oder sichere Session Tokens"
    - "Web Frontend getrennt vom Paper Plugin"
    - "RBAC für Administration"
    - "Read-heavy Dashboard zuerst"
    - "HTTPS über Reverse Proxy"

recommended_modules:
  - "Claims"
  - "Cities"
  - "Players"
  - "Audit Logs"
  - "Economy"
  - "Raids"
  - "Server Statistics"

reason:
  "Sehr wertvoll für Administration, aber kein Bestandteil des initialen Gameplay-Kerns."

source:
  screenshot: true
  premium_description: true
```

## 3.8 Audit Logs

```yaml
id: SCS-PREM-008
name: "Audit Logs"
category: "administration"
free: false
premium: true
priority: P1

citycore:
  gameplay_relevance: 3
  architecture_relevance: 5
  admin_value: 5
  player_value: 2
  integration_value: 4
  implementation_complexity: 3

implementation:
  feasible: true
  recommended: true
  approach:
    - "Domain Events oder Audit Events"
    - "Asynchrones Persistieren"
    - "Actor"
    - "Action"
    - "Target"
    - "Timestamp"
    - "Before/After Values"
    - "IP/Session-Daten nur wenn rechtlich und technisch erforderlich"

recommended_actions:
  - "CLAIM_CREATE"
  - "CLAIM_DELETE"
  - "CLAIM_EXPAND"
  - "CLAIM_SHRINK"
  - "OWNER_CHANGE"
  - "ROLE_CREATE"
  - "ROLE_DELETE"
  - "ROLE_ASSIGN"
  - "PERMISSION_CHANGE"
  - "FLAG_CHANGE"
  - "CITY_CLAIM_ASSIGN"
  - "CITY_CLAIM_REMOVE"
  - "RAID_ACTION"

reason:
  "Für Moderation, Support und wirtschaftlich relevante Aktionen sehr wertvoll."

source:
  screenshot: true
  premium_description: true
```

## 3.9 Custom Roles

```yaml
id: SCS-PREM-009
name: "Custom Roles"
category: "authorization"
free: false
premium: true
priority: P1

citycore:
  gameplay_relevance: 5
  architecture_relevance: 5
  admin_value: 5
  player_value: 5
  integration_value: 3
  implementation_complexity: 4

implementation:
  feasible: true
  recommended: true
  approach:
    - "Role als eigene Domain Entity"
    - "Role -> PermissionSet"
    - "Hierarchie oder Priorität"
    - "Owner bleibt Sonderrolle"
    - "City-Rollen von Claim-Rollen trennen"
    - "Optional Templates für Rollen"

example_claim_roles:
  - "OWNER"
  - "MODERATOR"
  - "MEMBER"
  - "VISITOR"
  - "BUILDER"
  - "TREASURER"

example_city_roles:
  - "MAYOR"
  - "COUNCIL"
  - "TREASURER"
  - "BUILDER"
  - "WAR_MANAGER"

reason:
  "Passt direkt zu CityCore, da Städte Organisationen mit eigenen Verantwortlichkeiten werden."

source:
  screenshot: true
  premium_description: true
  citycore_concept: true
```

## 3.10 Claim Respawn

```yaml
id: SCS-PREM-010
name: "Claim Respawn"
category: "gameplay"
free: false
premium: true
priority: P3

citycore:
  gameplay_relevance: 3
  architecture_relevance: 2
  admin_value: 1
  player_value: 4
  integration_value: 1
  implementation_complexity: 1

implementation:
  feasible: true
  recommended: true
  approach:
    - "RespawnLocation am Claim"
    - "Optional pro Spieler"
    - "Fallback auf normalen Spawn"
    - "Raid-/PVP-Ausnahmen definieren"

reason:
  "Nützlicher Komfort, aber kein Kernbestandteil des CityCore-Gameplays."

source:
  screenshot: true
  premium_description: true
```

## 3.11 Favorites System

```yaml
id: SCS-PREM-011
name: "Favorites System"
category: "ui"
free: false
premium: true
priority: P4

citycore:
  gameplay_relevance: 1
  architecture_relevance: 1
  admin_value: 1
  player_value: 3
  integration_value: 1
  implementation_complexity: 1

implementation:
  feasible: true
  recommended: false
  approach:
    - "Player -> Set<ClaimId>"
    - "Favorites-GUI"
    - "Teleport/Navigation"

reason:
  "Komfortfunktion ohne wesentlichen Einfluss auf CityCore."

source:
  screenshot: true
  premium_description: true
```

## 3.12 Default Claim Radius

```yaml
id: SCS-PREM-012
name: "Default Claim Radius"
category: "claiming"
free: false
premium: true
priority: P2

citycore:
  gameplay_relevance: 3
  architecture_relevance: 3
  admin_value: 3
  player_value: 4
  integration_value: 1
  implementation_complexity: 1

implementation:
  feasible: true
  recommended: true
  approach:
    - "defaultClaimRadius im Player-Profile"
    - "maxRadius als harte Obergrenze"
    - "Claim-Preview vor tatsächlicher Übernahme"
    - "CityCore adjacency rules berücksichtigen"

reason:
  "Passt gut zum geplanten Wachstum von Claims, muss aber mit dem CityCore-Prinzip angrenzender Claims vereinbar sein."

source:
  screenshot: true
  premium_description: true
  citycore_concept: true
```

## 3.13 Rent System

```yaml
id: SCS-PREM-013
name: "Rent System"
category: "economy"
free: false
premium: true
priority: P1

citycore:
  gameplay_relevance: 5
  architecture_relevance: 4
  admin_value: 4
  player_value: 5
  integration_value: 5
  implementation_complexity: 4

implementation:
  feasible: true
  recommended: true
  approach:
    - "RentContract oder ClaimRent"
    - "periodic billing"
    - "Grace Period"
    - "Payment Failure State"
    - "Claim Lock/Release"
    - "Vault/Economy Service"
    - "Audit Events"
    - "City tax später davon getrennt modellieren"

recommended_model:
  claim_rent:
    amount: "BigDecimal"
    period: "Duration"
    grace_period: "Duration"
    payer: "UUID"
    beneficiary: "UUID"
    status: "ACTIVE|OVERDUE|TERMINATED"

reason:
  "Grundstückswirtschaft ist direkt mit dem geplanten CityCore-Economy-System vereinbar."

source:
  screenshot: true
  premium_description: true
  citycore_concept: true
```

## 3.14 Claim Back

```yaml
id: SCS-PREM-014
name: "Claim Back"
category: "navigation"
free: false
premium: true
priority: P3

citycore:
  gameplay_relevance: 1
  architecture_relevance: 1
  admin_value: 1
  player_value: 4
  integration_value: 1
  implementation_complexity: 1

implementation:
  feasible: true
  recommended: true
  approach:
    - "LastVisitedClaim pro Spieler"
    - "Teleport zur gespeicherten Claim-Location"
    - "Invalidierung bei gelöschtem Claim"
    - "Cooldown/Teleport-Delay optional"

reason:
  "Sehr geringer Aufwand, guter Komfort."

source:
  screenshot: true
  premium_description: true
```

## 3.15 QuickShop-Hikari Support

```yaml
id: SCS-PREM-015
name: "QuickShop-Hikari Support"
category: "integration"
free: false
premium: true
priority: P2

citycore:
  gameplay_relevance: 4
  architecture_relevance: 3
  admin_value: 2
  player_value: 4
  integration_value: 5
  implementation_complexity: 3

implementation:
  feasible: true
  recommended: true
  approach:
    - "QuickShop-Hikari API Hook"
    - "Shop-Erstellung nur wenn Claim-Permissions dies erlauben"
    - "Shop-Interaktionen durch Claim-Schutz kontrollieren"
    - "Owner/Member-Rechte berücksichtigen"
    - "City-/Marktplatzregeln später integrierbar"

reason:
  "Handel und Wirtschaft sind ein Kernbestandteil des CityCore-Konzepts."

source:
  screenshot: true
  premium_description: true
  citycore_concept: true
```

---

# 4. Priorisierte Roadmap

## P1 – Muss in die eigene Claim-Infrastruktur

```yaml
P1:
  - id: SCS-PREM-002
    feature: "Erweiterte Permissions"
    reason: "Kern der Claim-Autorisierung"

  - id: SCS-PREM-003
    feature: "Erweiterte Claim Flags"
    reason: "Kern der Schutzlogik"

  - id: SCS-PREM-008
    feature: "Audit Logs"
    reason: "Administration, Support und Nachvollziehbarkeit"

  - id: SCS-PREM-009
    feature: "Custom Roles"
    reason: "Direkte Grundlage für Claim- und City-Rollen"

  - id: SCS-PREM-013
    feature: "Rent System"
    reason: "Passt direkt zur Grundstücks- und Wirtschaftsidee"
```

## P2 – Nach dem stabilen Claim-Kern

```yaml
P2:
  - id: SCS-PREM-001
    feature: "SquareMap Support"

  - id: SCS-PREM-004
    feature: "Custom Claim Icons"

  - id: SCS-PREM-005
    feature: "Advanced GUI Actions"

  - id: SCS-PREM-006
    feature: "Discord Webhooks"

  - id: SCS-PREM-012
    feature: "Default Claim Radius"

  - id: SCS-PREM-015
    feature: "QuickShop-Hikari Support"
```

## P3 – Später

```yaml
P3:
  - id: SCS-PREM-007
    feature: "Web Dashboard"
    note: "Sehr hoher Nutzen, aber hoher Implementierungsaufwand"

  - id: SCS-PREM-010
    feature: "Claim Respawn"

  - id: SCS-PREM-014
    feature: "Claim Back"
```

## P4 – derzeit zurückstellen

```yaml
P4:
  - id: SCS-PREM-011
    feature: "Favorites System"
```

---

# 5. CityCore-spezifische Anpassungen

Die Premium-Funktionen sollten **nicht 1:1 kopiert** werden. Sie sollten in das CityCore-Domainmodell integriert werden.

## 5.1 Claims

```text
Claim
├── id
├── owner
├── chunks
├── core
├── permissions
├── flags
├── roles
├── rent
├── city
└── metadata
```

## 5.2 Städte

```text
City
├── id
├── name
├── mayor
├── members
├── roles
├── prestige
├── level
├── claims
└── center
```

## 5.3 Trennung von Claim- und City-Rechten

```yaml
authorization:
  claim_scope:
    owner:
    moderator:
    member:
    visitor:

  city_scope:
    mayor:
    council:
    treasurer:
    war_manager:
    builder:
```

Ein City-Rollenmodell darf nicht einfach mit Claim-Rollen vermischt werden.

---

# 6. Architektur-Empfehlung

## Ziel

Die Premium-Funktionalität sollte als **eigene CityCore-Implementierung** entstehen.

Nicht:

```text
Premium-Code kopieren
```

Sondern:

```text
MIT Open Source Basis
        |
        v
CityCore Claim Core
        |
        +-- Permission System
        +-- Flag System
        +-- Role System
        +-- Audit System
        +-- Economy System
        +-- GUI System
        +-- Integration Layer
        +-- Web API
```

## Empfohlene Module

```yaml
modules:
  claim-core:
    priority: P0
    responsibility:
      - "Claims"
      - "Chunks"
      - "Ownership"
      - "ClaimCore"

  claim-authorization:
    priority: P1
    responsibility:
      - "Permissions"
      - "Roles"
      - "Flags"

  claim-audit:
    priority: P1
    responsibility:
      - "Audit Events"
      - "Persistence"
      - "Moderation"

  claim-economy:
    priority: P1
    responsibility:
      - "Rent"
      - "Buying"
      - "Selling"
      - "Vault"

  claim-integrations:
    priority: P2
    responsibility:
      - "SquareMap"
      - "QuickShop-Hikari"
      - "Discord"

  claim-ui:
    priority: P2
    responsibility:
      - "Custom Icons"
      - "Advanced GUI Actions"

  web:
    priority: P3
    responsibility:
      - "REST API"
      - "Web Dashboard"
```

---

# 7. Nicht als Premium-Differenz zu behandeln

Die Premium-Produktseite enthält viele weitere Funktionen, die im bereitgestellten Vergleichsscreenshot **nicht explizit als Free-vs-Premium-Differenz aufgeführt** sind.

Beispiele:

```yaml
additional_premium_features:
  - "Claim Templates"
  - "Live Claim Statistics"
  - "Public Claim Browser"
  - "Public Warps"
  - "Per-role BossBar Colours"
  - "Claim Holograms"
  - "Per-world configuration"
  - "Mob Stacker"
  - "Item Stacker"
  - "WorldGuard integration enhancements"
  - "Nexo integration"
  - "Oraxen integration"
  - "PvPManager integration"
  - "Lands import"
  - "Towny import"
  - "SQLite/MySQL transfer"
  - "Admin toolbox"
  - "Social spy"
  - "Custom command aliases"
  - "Configurable sounds"
  - "Protected Areas"
  - "Developer API"
```

Diese Liste ist **keine Aussage darüber, dass diese Funktionen ausschließlich Premium sind**. Sie dokumentiert lediglich weitere Funktionen, die auf der aktuellen Premium-Produktseite beschrieben werden.

---

# 8. Lizenz-/Entwicklungsnotiz

```yaml
license:
  upstream_free_project:
    repository: "Xyness/SimpleClaimSystem"
    license: "MIT"

  development_rule:
    - "MIT-lizenzierten Code darf der Fork grundsätzlich verwenden und verändern."
    - "Originale MIT-Lizenz-/Copyright-Hinweise müssen bei entsprechenden übernommenen Codebestandteilen erhalten bleiben."
    - "Premium-Code soll nicht als Quelle für eine Codekopie verwendet werden."
    - "Die gewünschte Funktionalität kann eigenständig implementiert werden."
```

---

# 9. LLM Instructions / Machine-readable Context

```yaml
llm_context:
  project_name: "CityCore"

  primary_goal:
    "Entwicklung eines professionellen Minecraft-SMP-Servers mit spielergetriebenen Städten, Claims, Wirtschaft, Prestige und Raids."

  claim_principles:
    - "Jeder Claim gehört einem Spieler."
    - "Claims bestehen aus Chunks."
    - "Claims können Städten zugeordnet werden."
    - "Städte besitzen nicht automatisch das Eigentum an Spielerclaims."
    - "Claim Cores sind Bestandteil des Raid-Systems."
    - "Gebäude werden nicht automatisch erzeugt."
    - "Raids zerstören keine Gebäude oder Claims."

  implementation_policy:
    "Premium-Funktionalität eigenständig implementieren und an CityCore anpassen."

  priority_rule:
    "Gameplay- und Domain-Funktionen vor Komfort- und Web-Funktionen implementieren."

  do_not:
    - "Premium-Code kopieren"
    - "Claim- und City-Rollen ohne klare Scope-Trennung vermischen"
    - "Web Dashboard vor stabiler Domain-/Service-Schicht implementieren"
    - "GUI direkt mit Datenbanklogik koppeln"

  preferred_architecture:
    - "Domain"
    - "Application Services"
    - "Managers/Caches"
    - "Repository/Persistence"
    - "Integration Layer"
    - "GUI"
    - "Web/API"
```

---

# 10. Recherche-Evidenz

## Free/Open-Source-Version

Die aktuelle öffentliche Projektbeschreibung bestätigt unter anderem:

- Chunk-basierte Claims
- Multi-Chunk Claims
- Radius Claiming
- Claim-Merging
- 30+ Claim-Permission-Einstellungen
- Vault Economy
- Dynmap / BlueMap / Pl3xMap
- Bedrock-Unterstützung
- PlaceholderAPI
- Auto-Claim / Auto-Map
- Protected Areas
- GriefPrevention Migration
- öffentliche API

## Premium-Version

Die aktuelle Premium-Produktseite beschreibt unter anderem:

- 130+ Permissions
- 70+ Flags
- Custom Roles
- Custom Icons
- Claim Back
- Claim Respawn
- Favorites
- Default Radius
- Rent
- QuickShop-Hikari
- Web Dashboard
- Audit Logs
- SquareMap
- Discord Webhooks

---

# 11. Finales Fazit

```yaml
decision:
  fork_strategy: "RECOMMENDED"

  reason:
    - "Die Free-Version stellt eine brauchbare MIT-lizenzierte technische Basis dar."
    - "Die Premium-Funktionen können hinsichtlich ihrer Funktionalität eigenständig nachgebildet werden."
    - "CityCore benötigt ohnehin zusätzliche Domainlogik für Cities, Prestige, Economy und Raids."
    - "Mehrere Premium-Funktionen passen direkt zu CityCore."

  highest_priority:
    - "Permissions"
    - "Flags"
    - "Custom Roles"
    - "Audit Logs"
    - "Rent"

  second_wave:
    - "SquareMap"
    - "Custom Icons"
    - "GUI Actions"
    - "Discord Webhooks"
    - "Default Radius"
    - "QuickShop-Hikari"

  later:
    - "Web Dashboard"
    - "Claim Respawn"
    - "Claim Back"

  lowest_priority:
    - "Favorites"

  strategic_recommendation:
    "SimpleClaimSystem als technische Claim-Basis forken, anschließend die Architektur schrittweise auf CityCore ausrichten und die benötigte Premium-Funktionalität als eigene Implementierung ergänzen."
```

---

## Quellen

- Xyness/SimpleClaimSystem: https://github.com/Xyness/SimpleClaimSystem
- SimpleClaimSystem Modrinth: https://modrinth.com/plugin/simpleclaimsystem
- SimpleClaimSystem V2 Premium: https://builtbybit.com/resources/simpleclaimsystem.92437/
