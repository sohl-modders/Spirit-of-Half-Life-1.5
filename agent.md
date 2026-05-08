# Agent-Anleitung – Spirit of Half-Life 1.5

Dieses Dokument beschreibt das Projekt, die Verzeichnisstruktur, Konventionen und wichtige Hinweise für KI-Agenten, die an diesem Repository arbeiten.

---

## Projektübersicht

**Spirit of Half-Life (SoHL) 1.5 alpha4** ist eine clientseitige Half-Life-Modifikation auf Basis des Valve Half-Life SDK. Sie erweitert die Original-Engine um neue Entities, Systeme und Gameplay-Mechaniken (Inventar, Regen/Schnee, Spiegel-Rendering, Third-Person-Kamera, benutzerdefinierte Todesnachrichten u. v. m.).

- **Versionsstring:** `"Spirit of Half-Life 1.5 alpha4"` (definiert in `SpiritSource/dlls/gamerules.h`)
- **Basis:** Valve Half-Life SDK (GoldSrc / WON-Ära, C++, MSVC 6)
- **Sprache:** C / C++
- **Build-System:** Visual Studio 6 Workspace (`.dsw` / `.dsp`-Dateien)

---

## Verzeichnisstruktur

```
Spirit-of-Half-Life-1.5/
├── SpiritSource/
│   ├── cl_dll/          # Client-DLL (HUD, Rendering, Eingaben, Effekte)
│   ├── dlls/            # Server-DLL (Entities, Waffen, Monster, Physik-Logik)
│   ├── common/          # Gemeinsame Header (client/server)
│   ├── engine/          # Engine-Header (studio.h, etc.)
│   ├── game_shared/     # Geteilter Spielcode (Voice-Status etc.)
│   ├── pm_shared/       # Physik-Shared (Bewegungsberechnung, pm_shared.c)
│   ├── network/         # Netzwerk-Header
│   ├── utils/           # Hilfsprogramme (vgui, common, wadlib etc.)
│   ├── hl.dsw           # Visual Studio 6 Workspace
│   └── hl.opt           # Visual Studio Optionen
├── Spirit/              # Spielinhalte (Ressourcen, Maps, Sounds)
├── spirit.fgd           # Entity-Definitionen für Hammer-Editor (1.5a4)
├── README.md
├── ReadMe.txt
├── changelog.md         # Änderungsprotokoll 1.4 → 1.5
├── todo.md              # Gesammelte TODO/FIXME/UNDONE-Kommentare aus dem Quellcode
└── plan_1.5.md          # Entwicklungsplan für 1.5
```

---

## Wichtige Dateien

| Datei | Zweck |
|---|---|
| `SpiritSource/dlls/gamerules.h` | Versionsstring `GAME_NAME` |
| `SpiritSource/dlls/cbase.h` | Basis-Entity-Klassen, `MAX_ITEMS`, `RADIATION_DURATION` |
| `SpiritSource/dlls/cdll_dll.h` | Gemeinsame Client/Server-Konstanten (z. B. `MAX_ITEMS`) |
| `SpiritSource/dlls/player.h` | Spieler-Klasse und -Member |
| `SpiritSource/dlls/items.h` | Inventar-Item-Klassen |
| `SpiritSource/dlls/effects.h` | Regen-Entity-Deklarationen |
| `SpiritSource/cl_dll/hud.h` | HUD-Definitionen, Inventar-Slots |
| `SpiritSource/cl_dll/rain.h` | Regen/Schnee-Rendering |
| `SpiritSource/cl_dll/CustomMenu.cpp` | VGUI-Klassenauswahlmenü |
| `spirit.fgd` | Entity-Definitionen für den Hammer-Editor |

---

## Konventionen

- **Sprache:** C++ (Server-DLL) und C (pm_shared). Kein modernes C++ (kein STL, kein C++11+), da MSVC 6-kompatibel.
- **Namenskonvention:** `C`-Präfix für Entity-Klassen (z. B. `CBaseEntity`, `CItemMedicalKit`), `pev->` für Entity-Properties.
- **Kommentare im Code:** Englisch, oft mit Tags wie `UNDONE`, `FIXME`, `HACK`, `TODO`. Alle bekannten Stellen sind in `todo.md` erfasst.
- **Dokumentation:** Markdown-Dateien im Repo-Root sind auf **Deutsch**.
- **MAX_ITEMS:** Definiert in `SpiritSource/dlls/cdll_dll.h` (geteilt von Client und Server), **nicht** in `cbase.h`.
- **Keine externe Build-Infrastruktur:** Es gibt kein CMake, kein `npm`, kein Makefile für Windows – nur die `.dsp`/`.dsw`-Projektdateien für MSVC 6. Unter Linux existiert ein `Makefile` in `SpiritSource/dlls/`.

---

## Build-Hinweise

- **Windows:** Visual Studio 6 / MSVC 6 wird benötigt. Workspace: `SpiritSource/hl.dsw`.
- **Linux (Server-DLL):** `make` im Verzeichnis `SpiritSource/dlls/` (experimentell).
- Es gibt **keine automatisierten Tests** in diesem Repository.
- Linting oder CI ist **nicht konfiguriert**.

---

## Bekannte Probleme & Offene Punkte

Alle bekannten `TODO`, `FIXME`, `UNDONE` und `HACK`-Kommentare aus dem Quellcode sind in [`todo.md`](todo.md) tabellarisch aufgeführt. Vor der Arbeit an einem bestimmten Bereich bitte dort nachschlagen.

---

## Netzwerk-Nachrichten (SoHL-spezifisch)

| Nachricht | Beschreibung |
|---|---|
| `RainData` | Überträgt Regen-Parameter vom Server zum Client |
| `Inventory` | Aktualisiert Item-Zähler beim Client (10-Slot-Inventar) |

---

## SoHL-spezifische Systeme

| System | Dateien |
|---|---|
| Regen/Schnee | `cl_dll/rain.cpp`, `cl_dll/rain.h`, `dlls/effects.cpp` |
| Inventar (10 Slots) | `dlls/items.cpp`, `dlls/items.h`, `cl_dll/hud.cpp` |
| Third-Person-Kamera | `cl_dll/in_camera.cpp`, `cl_dll/view.cpp` |
| Spiegel-Rendering | `cl_dll/StudioModelRenderer.cpp`, `cl_dll/tri.cpp` |
| Locus-System | `dlls/locus.cpp`, `dlls/locus.h` |
| FMOD-Audio | `cl_dll/mp3.cpp`, `cl_dll/fmod.h` |
| Custom-Klassenmenü | `cl_dll/CustomMenu.cpp`, `cl_dll/vgui_TeamFortressViewport.cpp` |
| Benutzerdefinierte Todesnachrichten | `dlls/multiplay_gamerules.cpp`, `dlls/cbase.h` (`killname`/`killmethod`) |

---

## Hinweise für Agenten

- Ändere **keine** Build-Dateien (`.dsp`, `.dsw`, `.opt`) manuell – diese sind MSVC-spezifisch und binär/halbstrukturiert.
- Neue Entities müssen in `spirit.fgd` eingetragen werden, damit sie im Hammer-Editor sichtbar sind.
- Halte Änderungen **MSVC 6-kompatibel** (kein `auto`, kein range-for, keine initializer lists).
- Für neue Inventar-Items: `MAX_ITEMS` ist in `SpiritSource/dlls/cdll_dll.h` definiert (aktuell `10`).
- Die Dokumentation im Repo-Root ist auf Deutsch; Code-Kommentare sind auf Englisch.
