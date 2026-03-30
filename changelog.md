# Spirit of Half-Life 1.5 – Changelog

Dieses Dokument beschreibt alle Änderungen vom Update **SOHL 1.4** auf **SOHL 1.5 alpha4**.

> **Commit:** `d0e2a63` – *Update to SOHL 1.5*
> **Autor:** Lucas (godkiller_nt@hammermaps.de)
> **Datum:** 22. Juni 2020
> **Statistik:** 77 Dateien geändert, 4.023 Zeilen hinzugefügt, 3.064 Zeilen entfernt

---

## Neue Features

### Dynamisches Wettersystem (Regen/Schnee)

- Komplettes clientseitiges Rendering-System für Regen und Schnee
- Konfigurierbar über Map-spezifische `.pcs`-Dateien
- Unterstützung für Wind (X/Y), zufällige Streuung und globale Höhe
- Regen-Modus (Geschwindigkeit 900) und Schnee-Modus (Geschwindigkeit 200)
- Wasserring-Effekte wenn Regen auf Wasseroberflächen trifft
- Maximal 2.000 Regentropfen und 3.000 Wasserring-Effekte gleichzeitig
- **Neue Server-Entities:**
  - `CRainSettings` – Definiert Regen-Distanz und Modus für das Level
  - `CRainModify` – Steuert Regen-Parameter (Tropfenrate, Wind, Zufälligkeit) mit Fade-Übergängen
- **Neue Client-Dateien:** `rain.cpp`, `rain.h`
- **Neue CVar:** `cl_raininfo` – Debug-Ausgabe für das Regensystem
- **Neue Netzwerknachricht:** `RainData` – Überträgt Regen-Parameter vom Server zum Client

### Custom Class Menu (HUD)

- Interaktives Klassenauswahl-Menü mit scrollbarem Panel
- Zeigt Klassenbeschreibungen, Team-Grafiken und Spieleranzahl pro Klasse
- Lädt Beschreibungstexte aus `classes/short_[classname].txt`
- Unterstützt Rot/Blau Team-Varianten für Grafiken
- Auflösungsbewusst (deaktiviert Grafiken unter 640×480)
- **Neue Datei:** `CustomMenu.cpp`
- **Neue Definition:** `MENU_CUSTOM 10`
- **Neue Eingabe-Befehle:** `+hud`/`-hud`, `+briefing`/`-briefing`

### Inventar-System

- 10-Slot Inventarsystem (erweitert von 5 Slots in SOHL 1.4)
- **Neue Inventar-Items:**
  - `CItemMedicalKit` – Tragbares Medkit mit Ladungssystem (konfigurierbar über `max_medkit` CVar, Standard: 200)
  - `CItemAntiRad` – Anti-Strahlungs-Spritze
  - `CItemAntidote` – Gegengift-Injektion (refaktorisiert zu eigenständiger Klasse)
  - `CItemFlare` – Leuchtfackel mit anpassbaren Lichteigenschaften
  - `CItemCamera` – Fernkamera-Gerät mit Platzierungs-, Betrachtungs- und Zerstörungs-Mechaniken
- Kamera-System erlaubt Spielern mehrere Kameras zu tragen (limitiert durch `max_cameras` CVar, Standard: 2)
- Inventar-Vorschau im VGUI-Menü mit `!SHOWINVENTORY`-Button
- **Neue Netzwerknachricht:** `Inventory` – Aktualisiert Item-Zähler beim Client
- **Neuer Konsolenbefehl:** `inventory`

### Kamera-HUD-Anzeige

- Blinkendes Kamera-Aktivitäts-Symbol (1 FPS Blink-Rate)
- Kamera-Fadenkreuz-Sprites an 4 Ecken (oben-links, oben-rechts, unten-links, unten-rechts)
- Sprites: `camera_active`, `camera_rect_tl`, `camera_rect_tr`, `camera_rect_bl`, `camera_rect_br`
- Aktiv wenn Custom-View und Kamera-Flag gesetzt sind

### Benutzerdefinierte Todesnachrichten

- Entities können eigene Kill-Nachrichten über `killname` und `killmethod` Felder definieren
- Todesnachrichten unterstützen `%`-Trennzeichen für formatierte Ausgabe
- Beispiel: Statt „Player killed Player with train" → „Player was crushed by giant sawblade"
- Multiplayer-Todesbenachrichtigungen senden jetzt Waffen- und Technik-Name

---

## Verbesserungen

### Kamera-System (Komplett überarbeitet)

- **Neues Third-Person-Kamerasystem** – basierend auf Original-Idee von Xwider (Clipping-Style)
- Alte Third-Person-Kamera-Logik vollständig entfernt (~570 Zeilen)
- Neue `V_CalcThirdPersonRefdef()` Funktion für Third-Person-Berechnung
- **Neue Konsolenbefehle:** `thirdperson`, `firstperson`, `drawplayer`, `hideplayer`
- Kamera-Modus über `m_iCameraMode` Bitfeld gesteuert
- Trigger-Viewset-Unterstützung erweitert (Spieler-Darstellung in Kamera-Ansicht, X-Achsen-Invertierung)
- `V_GetChasePos()` Signatur geändert von `int target` zu `cl_entity_t *ent` für bessere Entity-Verwaltung

### Spiegel-Rendering

- Volle 3-Achsen Spiegel-Unterstützung (X, Y, Z) mit korrekten Matrix-Transformationen
- Mehrere Spiegel pro Frame unterstützt
- `env_mirror` Entity-Fixes:
  - `FL_WORLDBRUSH` Flag hinzugefügt – Monster können jetzt auf Spiegeln laufen
  - Spieler-Marker Positionierung korrigiert (verwendet `VecBModelOrigin()`)
  - Modell-Setup vor Spieler-Marker-Erstellung verschoben

### Sky/Parallax-Rendering

- Konfigurierbarer Sky-Parallax-Effekt über `m_iSkyScale`
- `env_sky` Entity sendet jetzt Skalierungsparameter für Parallax-Steuerung
- Sky-Zustandsmaschine: `SKY_OFF` → `SKY_ON` → `SKY_ON_DRAWING` → `SKY_ON`
- Modell-Rendering in `SKY_ON_DRAWING`-Modus optimiert (überspringt nicht-PVS Entities)

### Rotierende Brushes

- `CFuncWall` unterstützt jetzt Rotation über „message"-Key statt Spawnflags
- `CFuncRotating` erkennt automatisch Rotationsmittelpunkt wenn kein Origin gesetzt
- Rotierende Brushes starten sofort wenn kein Targetname vergeben

### FMOD Audio

- FMOD-Bibliothek-Support aktualisiert
- Funktions-Erkennung: prüft neuere `Stream_Open` vor alter `Stream_OpenFile`
- Pfadbehandlung vereinfacht (kein hartcodierter `sound/fmod/`-Prefix mehr)
- CD-Track-Wiedergabe (`PlayCDTrack()`) unterstützt jetzt Audio-Dateipfade über `pev->message`
- `CTargetFMODAudio` hat jetzt Save/Restore-Unterstützung für den Wiedergabe-Status

### Monster Maker

- Signifikantes Refactoring: Temporärer `pWhere`-Entity-Pointer entfernt
- Verwendet jetzt `pev->vuser1/2/3` für Position/Winkel/Geschwindigkeit direkt am MonsterMaker
- Unterstützt dynamische Werte: Origin (`pev->noise`), Offset (`pev->noise1`), Winkel (`pev->noise2`), Geschwindigkeit (`pev->noise3`)
- Bessere Kompatibilität mit Save/Restore-System

### Locus-System

- `CCalcSubVelocity` hat neue Spawnflags zum Verwerfen einzelner Achsen:
  - `SF_CALCVELOCITY_DISCARDX` / `SF_CALCVELOCITY_DISCARDY` / `SF_CALCVELOCITY_DISCARDZ`
  - Ersetzt alte Tausch-Funktionalität für bessere Nutzbarkeit
  - Ermöglicht Extraktion von horizontalen oder vertikalen Vektor-Komponenten

### Voice-Status

- `cam_thirdperson`-Variable entfernt
- Voice-Icon-Anzeige für lokalen Spieler unabhängig vom Kamera-Modus (immer sichtbar)

---

## Waffen-Änderungen

### Schnelleres Waffenwechseln

- **Holster-Zeit für alle Waffen von 1,0 auf 0,5 Sekunden reduziert:**
  - Armbrust (`crossbow.cpp`)
  - Brechstange (`crowbar.cpp`)
  - Debugger (`debugger.cpp`)
  - Egon-Gun (`egon.cpp`)
  - Gauss-Gun (`gauss.cpp`)
  - Glock (`glock.cpp`)
  - Hornet-Gun (`hornetgun.cpp`) – von 2,0 auf 0,5 Sekunden
  - MP5 (`mp5.cpp`)
  - Python (`python.cpp`)
  - RPG (`rpg.cpp`)
  - Satchel (`satchel.cpp`)
  - Schrotflinte (`shotgun.cpp`)
  - Snark (`squeakgrenade.cpp`)
  - Tripmine (`tripmine.cpp`)

---

## Spielmechanik-Änderungen

### Strahlung

- `RADIATION_DURATION` von 2 auf 10 Sekunden erhöht – Strahlung wirkt deutlich länger
- Zeitbasierte Schäden (Nervengas/Strahlung) über `timed_damage` CVar ein-/ausschaltbar
- Gegengift und Anti-Strahlung Items können gegen entsprechende Schadensarten eingesetzt werden

### Trigger-basierte Kills

- `CTriggerHurt` schreibt jetzt dem Aktivator die Frags zu (nicht mehr der Welt-Entity)
- `CBaseTrigger::ToggleUse` speichert Aktivator für korrekte Frag-Zuordnung
- Gesundheits-Items geben jetzt die tatsächlich geheilte Menge zurück (statt nur 0/1)

### Gib-Shooter

- Verzögerungs-Logik korrigiert: alle Gibs werden auf einmal geschossen wenn Delay 0 ist
- Body-Initialisierung für korrekte Model-Frame-Zählung repariert

### Partikel-System

- Partikel aktivieren sich standardmäßig nur wenn kein Name vergeben, respektieren ansonsten Spawnflags

---

## Entfernte Dateien / Code

### Gelöschte Quelldateien

| Datei | Beschreibung |
|-------|-------------|
| `dlls/islave_deamon.cpp` | Alien Slave/Vortigaunt Daemon Resurrektion (~876 Zeilen) |
| `dlls/wpn_shared/hl_wpn_glock.cpp` | Glock Waffen-Implementierung (~274 Zeilen, verschoben) |
| `dlls/Makefile.old` | Veraltete Linux-Build-Konfiguration |

### Gelöschte IDE-Dateien

| Datei | Beschreibung |
|-------|-------------|
| `cl_dll/cl_dll.ncb` | Visual Studio IntelliSense Cache |
| `cl_dll/cl_dll.opt` | Visual Studio IDE Optionen |
| `cl_dll/cl_dll.plg` | Visual Studio Build Log |
| `cl_dll/hl/hl_baseentity.~cp` | Temporäre Backup-Datei |
| `dlls/hl.ncb` | MSVC Workspace Symbol-Datei |
| `dlls/hl.opt` | MSVC Options-Datei |

---

## Projekt-/Build-Änderungen

- **Versionsstring** aktualisiert: `"Spirit of Half-Life 1.4"` → `"Spirit of Half-Life 1.5 alpha4"`
- **Neues Workspace:** `SpiritSource/hl.dsw` hinzugefügt (Visual Studio Workspace)
- **Neues Options-File:** `SpiritSource/hl.opt` hinzugefügt
- **Projekt-Dateien** (`cl_dll.dsp`, `hl.dsp`) aktualisiert für neue Quelldateien und Build-Pfade
- **MAX_ITEMS** von 5 auf 10 erweitert (`cbase.h`)

---

## Bug-Fixes

- **Plattformen (`plats.cpp`):** Sound-Neustart-Logik korrigiert – Sound wird nur bei Zustandsänderung neugestartet, nicht jeden Frame
- **Scripted Sequences (`scripted.cpp`):** PossessEntity-Fixes für Script-Entity-Verhalten und korrektes Effect-State-Handling
- **Monster-Events (`monsters.cpp`):** `SCRIPT_EVENT_SENTENCE_RND1` bricht nicht mehr vorzeitig ab
- **Leech (`leech.cpp`):** Debug-Alert-Nachricht auskommentiert
- **Func_Tank (`func_tank.cpp`):** Konsolen-Alerts für Tank-Steuerung-Aktivierung deaktiviert
- **StudioModelRenderer:** Spieler-Blend-Berechnung von `*pPitch * 3` auf `*pPitch * -6` geändert für genauere vertikale Blickwinkel-Berechnung
- **Death Messages:** Buffer-Größe für Waffennamen von 32 auf 29 korrigiert (strncat-Grenzwertfix)
- **Motion System (`triggers.cpp`):** Workaround für Null-Pointer-Problem nach Save/Restore bei CMotionManager
- **Env_Light:** Licht-Abklingparameter verwendet jetzt Entity-Health-Feld statt berechneter Zeit

---

## Vollständige Dateiliste

<details>
<summary>Alle 77 geänderten Dateien anzeigen</summary>

### Neue Dateien (5)

- `SpiritSource/cl_dll/CustomMenu.cpp`
- `SpiritSource/cl_dll/rain.cpp`
- `SpiritSource/cl_dll/rain.h`
- `SpiritSource/hl.dsw`
- `SpiritSource/hl.opt`

### Gelöschte Dateien (9)

- `SpiritSource/cl_dll/cl_dll.ncb`
- `SpiritSource/cl_dll/cl_dll.opt`
- `SpiritSource/cl_dll/cl_dll.plg`
- `SpiritSource/cl_dll/hl/hl_baseentity.~cp`
- `SpiritSource/dlls/Makefile.old`
- `SpiritSource/dlls/hl.ncb`
- `SpiritSource/dlls/hl.opt`
- `SpiritSource/dlls/islave_deamon.cpp`
- `SpiritSource/dlls/wpn_shared/hl_wpn_glock.cpp`

### Geänderte Dateien (63)

**Client-seitig (cl_dll/):**
- `StudioModelRenderer.cpp` – Spiegel-Rendering, Null-Model, Pitch-Blending
- `StudioModelRenderer.h` – Neue Member-Variablen
- `cl_dll.dsp` – Projekt-Datei aktualisiert
- `death.cpp` – Custom-Todesnachrichten
- `hl/hl_weapons.cpp` – Kleinere Anpassungen
- `hud.cpp` – Regen/Inventar/Kamera-Integration
- `hud.h` – Neue Definitionen und Deklarationen
- `hud_msg.cpp` – Regen- und Inventar-Nachrichtenhandler
- `hud_redraw.cpp` – Kamera-HUD-Anzeige
- `hud_spectator.cpp` – V_GetChasePos Signaturänderung
- `in_camera.cpp` – Kamera-System komplett überarbeitet
- `input.cpp` – Custom-HUD und Briefing Eingaben
- `mp3.cpp` – FMOD-Updates
- `tf_defs.h` – MENU_CUSTOM Definition
- `tri.cpp` – Regen/Schnee-Rendering
- `vgui_TeamFortressViewport.cpp` – Inventar-UI und Custom-Menu
- `vgui_TeamFortressViewport.h` – Neue Member und Methoden
- `view.cpp` – Third-Person-Kamera, Sky-Parallax, Custom-View

**Server-seitig (dlls/):**
- `bmodels.cpp` – Rotierende Brushes
- `cbase.cpp` – TakeHealth Refactoring
- `cbase.h` – killname/killmethod, MAX_ITEMS, RADIATION_DURATION
- `cdll_dll.h` – Kleinere Änderungen
- `client.cpp` – Trigger-Viewset Bereinigung
- `crossbow.cpp` – Holster-Zeit
- `crowbar.cpp` – Holster-Zeit
- `debugger.cpp` – Holster-Zeit
- `effects.cpp` – Regen-Entities, Env-Fixes, Gib-Shooter
- `effects.h` – Regen-Entity Deklarationen
- `egon.cpp` – Holster-Zeit
- `func_break.cpp` – Formatierung
- `func_tank.cpp` – Alert-Deaktivierung
- `game.cpp` – Neue CVars
- `gamerules.h` – Versionsstring
- `gauss.cpp` – Holster-Zeit
- `glock.cpp` – Holster-Zeit
- `h_cycler.cpp` – Kleinere Ergänzung
- `hl.dsp` – Projekt-Datei aktualisiert
- `hl.plg` – Build-Log aktualisiert
- `hornetgun.cpp` – Holster-Zeit
- `items.cpp` – Komplettes Inventar-System
- `items.h` – Inventar-Item Klassen
- `leech.cpp` – Debug-Alert entfernt
- `locus.cpp` – Achsen-Verwerfen Spawnflags
- `monstermaker.cpp` – Refactoring
- `monsters.cpp` – Event-Fix
- `mp5.cpp` – Holster-Zeit
- `multiplay_gamerules.cpp` – Custom-Todesnachrichten
- `plats.cpp` – Sound-Fix
- `player.cpp` – Inventar, Regen, Schaden
- `player.h` – Neue Member-Variablen
- `python.cpp` – Holster-Zeit
- `rpg.cpp` – Holster-Zeit
- `satchel.cpp` – Holster-Zeit
- `scripted.cpp` – PossessEntity-Fix
- `shotgun.cpp` – Holster-Zeit
- `singleplay_gamerules.cpp` – Kleinere Änderung
- `squeakgrenade.cpp` – Holster-Zeit
- `triggers.cpp` – Trigger-Kills, FMOD, Motion-Fixes
- `tripmine.cpp` – Holster-Zeit
- `weapons.cpp` – Waffen-Änderungen
- `weapons.h` – Waffen-Deklarationen
- `world.cpp` – I_Precache Aufruf

**Sonstige:**
- `game_shared/voice_status.cpp` – Kamera-Variable entfernt

</details>
