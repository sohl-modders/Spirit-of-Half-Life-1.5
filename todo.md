# TODO – Spirit of Half-Life 1.5

Automatisch aus dem Quellcode gesammelte `TODO`, `FIXME`, `UNDONE` und `HACK`-Kommentare sowie nicht fertiggestellte Funktionen / Stubs.

---

## Inhaltsverzeichnis

1. [Server-DLL (`SpiritSource/dlls/`)](#server-dll)
2. [Client-DLL (`SpiritSource/cl_dll/`)](#client-dll)
3. [Physik-Shared (`SpiritSource/pm_shared/`)](#physik-shared)
4. [Engine-Header (`SpiritSource/engine/`)](#engine-header)
5. [VGUI-Utils (`SpiritSource/utils/vgui/`)](#vgui-utils)
6. [Sonstige Utils (`SpiritSource/utils/common/`)](#sonstige-utils)

---

## Server-DLL

### AI / Monster-Logik

| Datei | Zeile | Typ | Beschreibung |
|---|---|---|---|
| `dlls/AI_BaseNPC_Schedule.cpp` | 211 | UNDONE | Wert `10` für Schleifenlimit nicht sauber festgelegt, nur Notlösung gegen Endlosschleifen |
| `dlls/AI_BaseNPC_Schedule.cpp` | 278 | UNDONE | Schedule wird zweimal ausgeführt – unklar warum |
| `dlls/AI_BaseNPC_Schedule.cpp` | 295 | UNDONE | Animation-Set wird manuell gesetzt, damit Blend-Übergänge nicht brechen |
| `dlls/AI_BaseNPC_Schedule.cpp` | 1166 | UNDONE | Wenn ein Monster nicht rennen kann, sollte es stattdessen gehen |
| `dlls/h_ai.cpp` | 39 | UNDONE | `CBaseMonster`-Funktionen sollten nach `monsters.cpp` verschoben werden |
| `dlls/h_ai.cpp` | 46 | UNDONE | Datei-Funktion möglicherweise zu `CBaseMonster` machen |
| `dlls/h_ai.cpp` | 102 | UNDONE | Z-Unterschiede zwischen Wegpunkten nicht normalisiert (Dreieck nicht rechtwinklig) |
| `dlls/h_ai.cpp` | 149 | UNDONE | Traceline ignoriert Monster / trifft den Feind des Monsters |
| `dlls/monsters.cpp` | 51 | UNDONE | Schedule-Daten können nicht gespeichert/wiederhergestellt werden |
| `dlls/monsters.cpp` | 228 | UNDONE | Felder beim Clear unklar |
| `dlls/monsters.cpp` | 1131 | UNDONE | `pev->oldorigin` könnte statt berechnetem Ursprung genutzt werden |
| `dlls/monsters.cpp` | 1162 | UNDONE | Maximale Sichtweite für Monster-Override (80 units) per Magic Number |
| `dlls/monsters.cpp` | 1186/1206 | UNDONE | Erinnerungs-Stack für Geräusche ist ein flacher Hack; richtige Stack-Impl. fehlt |
| `dlls/monsters.cpp` | 1386 | UNDONE | Magic Number `64` für minimale Höhe; sollte monster-spezifisch sein |
| `dlls/monsters.cpp` | 2157 | UNDONE | Scripted-Sequence-Monster ohne Idle-Animation nicht sauber behandelt |
| `dlls/monsters.cpp` | 2249 | UNDONE | Nächsten Node finden statt festem Startpunkt |
| `dlls/monsters.cpp` | 2425 | UNDONE | Suche nach nächstem Feind bricht zu früh ab |
| `dlls/monsters.cpp` | 2443 | UNDONE | Gibt immer nur den nächsten Feind zurück, keine Priorisierung |
| `dlls/monsters.cpp` | 3081 | UNDONE | Kein persistenter Spielstand zum Tracken von zwei Variablen |
| `dlls/basemonster.h` | 105 | HACK | `m_HackedGunPos` – temporäre Lösung bis Abfrage am Laufende der Waffe möglich ist |

### Barney (NPC)

| Datei | Zeile | Typ | Beschreibung |
|---|---|---|---|
| `dlls/barney.cpp` | 18 | UNDONE | Waffe wird nicht weggesteckt (Holster) |
| `dlls/barney.cpp` | 86 | UNDONE | Unbekanntes ungenutztes Feld |
| `dlls/barney.cpp` | 392 | UNDONE | Nachladen nicht implementiert |
| `dlls/barney.cpp` | 537–539 | UNDONE | Sprach-Gruppen `BA_PHELLO`, `BA_PIDLE` deaktiviert / noch nicht gesetzt |
| `dlls/barney.cpp` | 846 | UNDONE | Kommentar über kürzlich gestorbenen Spieler fehlt |

### Wissenschaftler (NPC)

| Datei | Zeile | Typ | Beschreibung |
|---|---|---|---|
| `dlls/scientist.cpp` | 338 | FIXME | Crouch-Idle-Animation sieht schlecht aus |
| `dlls/scientist.cpp` | 970 | UNDONE | Kein Kommentar über toten Spieler |
| `dlls/scientist.cpp` | 981 | UNDONE | Angst-Modell nicht vollständig implementiert; fehlende R_FR-Level |
| `dlls/scientist.cpp` | 994 | UNDONE | Verängstigter Wissenschaftler weicht Spieler nicht aus |
| `dlls/scientist.cpp` | 1176 | UNDONE | Blutiger Skin auskommentiert – kein entsprechendes Skin-Asset vorhanden |

### Spieler

| Datei | Zeile | Typ | Beschreibung |
|---|---|---|---|
| `dlls/player.cpp` | 600–602 | UNDONE | Keine Sounds für bestimmte Schadensarten (Verbrennung, Schock, Schnitt) |
| `dlls/player.cpp` | 1132 | UNDONE | `FFADE_PERMANENT` und 8.8-Format für Fadetime nicht verwendet |
| `dlls/player.cpp` | 1636 | TODO | HUD-Update nach Tode senden |
| `dlls/player.cpp` | 1720 | UNDONE | Keine Traceline um USE durch Wände zu verhindern |
| `dlls/player.cpp` | 1734 | UNDONE | Kein separater USE-Code für ON/OFF; letztes USE-Objekt nicht gecacht |
| `dlls/player.cpp` | 2003 | UNDONE | Auto-Repeat für Eingaben unklar |
| `dlls/player.cpp` | 2230 | UNDONE | Halbe Geschwindigkeit nicht geflaggt |
| `dlls/player.cpp` | 3465 | UNDONE | SprayCan-Entität ultra-temporär |
| `dlls/player.cpp` | 3674 | UNDONE | `TraceResult` temporär aus PreAlpha-Zeit |
| `dlls/player.cpp` | 4018/4045 | FIXME | Deathmatch-Test-Code entfernen |
| `dlls/player.cpp` | 4741/4779 | UNDONE | Server-Variable zur Modell-Auswahl nicht verwendet |
| `dlls/player.cpp` | 4949 | UNDONE | Frame-Limit `8` ist nur ein Platzhalter |

### Waffen

| Datei | Zeile | Typ | Beschreibung |
|---|---|---|---|
| `dlls/weapons.cpp` | 132 | UNDONE | Falscher Angreifer in `ApplyMultiDamage` |
| `dlls/weapons.cpp` | 222 | UNDONE | Funktion möglicherweise nicht mehr verwendet |
| `dlls/weapons.cpp` | 645 | UNDONE | Zeitpunkt für `SUB_UseTargets` unklar |
| `dlls/weapons.cpp` | 1049 | UNDONE | Nachlad-Sound fehlt (`!!UNDONE -- reload sound goes here`) |
| `dlls/crossbow.cpp` | 30 | UNDONE | Save/Restore für Bolt-Entität fehlt |
| `dlls/crossbow.cpp` | 109 | UNDONE | Soll `TraceAttack` statt direktem Schaden aufrufen |
| `dlls/hornetgun.cpp` | 124 | HACK | Hornissenwaffe kann nicht ausgewählt werden wenn leer |
| `dlls/glock.cpp` | 43 | HACK | Klassennamen-Hack für alte Map-Kompatibilität |
| `dlls/python.cpp` | 71 | HACK | Klassennamen-Hack für alte Map-Kompatibilität |
| `dlls/mp5.cpp` | 54 | HACK | Klassennamen-Hack für alte Map-Kompatibilität |

### Türen / Plattformen / Trigger

| Datei | Zeile | Typ | Beschreibung |
|---|---|---|---|
| `dlls/doors.cpp` | 1224 | FIXME | `PostSpawn` statt `Spawn` verwenden (LRC) |
| `dlls/doors.cpp` | 1391 | FIXME | Bewegung in Gegenrichtung mit gleicher Geschwindigkeit nicht unterstützt |
| `dlls/plats.cpp` | 309 | UNDONE | Variable muss gespeichert werden – Klasse & Verkettung fehlen |
| `dlls/plats.cpp` | 1767 | FIXME | Feld `built facing` für Drehrichtung fehlt (LRC) |
| `dlls/plats.cpp` | 1938 | FIXME | Teleport-Flag-Unterstützung in `func_tracktrain` fehlt (LRC) |
| `dlls/plats.cpp` | 2360 | UNDONE | Touch-Events vor Train-Neuberechnung filtern |
| `dlls/triggers.cpp` | 289 | FIXME | `m_flDelay` für Alternativziel nicht genutzt |
| `dlls/triggers.cpp` | 554 | UNDONE | Trigger-Logik unfertig kommentiert |
| `dlls/triggers.cpp` | 1982 | FIXME | `env_model` stoppt seine eigene Framerate nicht |
| `dlls/triggers.cpp` | 2263 | FIXME | `pTarget` ist nicht zwingend ein `CBaseAnimating` |
| `dlls/triggers.cpp` | 2513 | UNDONE | `DMG_PARALYZE` sollte Bewegung auf 50 % reduzieren |
| `dlls/triggers.cpp` | 2658 | FIXME | Multiplayer-Fix aus `trigger_hurt` fehlt |
| `dlls/triggers.cpp` | 2672 | FIXME | `target once`-Flag nicht unterstützt |
| `dlls/triggers.cpp` | 2752 | UNDONE | Trigger wird nur temporär deaktiviert |
| `dlls/triggers.cpp` | 3394/3406 | UNDONE | Quake-Nachrichten ggf. entfernen |
| `dlls/triggers.cpp` | 4006 | UNDONE | Bessere Methode als `health` um physikalische Entitäten zu erkennen |
| `dlls/buttons.cpp` | 894 | UNDONE | `ButtonResponseToTouch()` ggf. verwenden |
| `dlls/pathcorner.cpp` | 102 | UNDONE | `flWait != 0` nicht unterstützt |

### Nodes / Wegfindung

| Datei | Zeile | Typ | Beschreibung |
|---|---|---|---|
| `dlls/nodes.cpp` | 160 | UNDONE | TOGGLE / STAY-Türen bei Node-Generierung nicht geprüft |
| `dlls/nodes.cpp` | 1475 | UNDONE | `EF_NODRAW` statt Sichtbarkeits-Hack verwenden |
| `dlls/nodes.cpp` | 2076 | UNDONE | Hull-Konstante statt fest kodierter `0` verwenden |

### Effekte / Welt

| Datei | Zeile | Typ | Beschreibung |
|---|---|---|---|
| `dlls/effects.cpp` | 457 | UNDONE | Test-Code von Jay – nur temporär |
| `dlls/effects.cpp` | 730 | FIXME | Fehlermeldung für ungültiges Setup fehlt (LRC) |
| `dlls/effects.cpp` | 1187 | HACK | Dateiname-Erkennung über Punkt – unsauber |
| `dlls/effects.cpp` | 2686 | UNDONE | Bestimmte Effekte funktionieren noch nicht |
| `dlls/effects.cpp` | 3353 | FIXME | Designer brauchen mehr Kontrolle über Parameter |
| `dlls/world.cpp` | 118 | UNDONE | Daten werden nicht an nachrückende Multiplayer-Spieler gesendet |
| `dlls/world.cpp` | 514 | UNDONE | Spawn-Code in `Precache()` gehört nach `Spawn()` |
| `dlls/world.cpp` | 701 | UNDONE | CVAR nicht über Client/Server-Link übertragen |
| `dlls/airtank.cpp` | 82 | UNDONE | Explosion sollte Blasenwolke erzeugen |
| `dlls/ggrenade.cpp` | 50 | UNDONE | Temporäre Versengung aus PreAlpha – saubere Lösung fehlt |

### Fliegende Monster / Spezial-NPCs

| Datei | Zeile | Typ | Beschreibung |
|---|---|---|---|
| `dlls/flyingmonster.cpp` | 30 | UNDONE | Nur Endpunkt der Bewegung wird geprüft |
| `dlls/flyingmonster.cpp` | 69 | UNDONE | Boden-Idle nicht implementiert |
| `dlls/flyingmonster.h` | 42 | UNDONE | Save/Restore für fliegende Monster fehlt |
| `dlls/ichthyosaur.cpp` | 51 | UNDONE | Save/Restore fehlt |
| `dlls/ichthyosaur.cpp` | 845 | UNDONE | Boden-Idle nicht implementiert |
| `dlls/leech.cpp` | 19/124 | UNDONE | Boid-Variablen ungenutzt; Gruppenverhalten fehlt |
| `dlls/bigmomma.cpp` | 174 | UNDONE | Leerkommentar – Funktion unfertig |
| `dlls/bigmomma.cpp` | 1223 | UNDONE | Säure-Spit ist Kopie des Tintenfisch-Spit, nur leicht angepasst |
| `dlls/apache.cpp` | 27 | UNDONE | `SF_WAITFORTRIGGER` Flag-Kombination muss gefixt werden |
| `dlls/apache.cpp` | 425 | UNDONE | Kein echtes Abprallen implementiert |
| `dlls/apache.cpp` | 725 | UNDONE | Unterschiedliche Sounds für jeden Multiplayer-Spieler fehlen |
| `dlls/hornet.cpp` | 265 | UNDONE | Spieler-Pointer nach Level-Rückkehr nicht wiederhergestellt |
| `dlls/zombie.cpp` | 19 | UNDONE | Zombie zuckt bei jedem Treffer – sollte nur manchmal passieren |
| `dlls/turret.cpp` | 22 | TODO | Leer – Turret-TODO ohne Beschreibung |
| `dlls/turret.cpp` | 67 | UNDONE | Turret-Gibs nicht implementiert |
| `dlls/xen.cpp` | 421 | UNDONE | Xen-Entitäten erzeugen keinen Rauch bei Schaden |

### Kampf / Schaden

| Datei | Zeile | Typ | Beschreibung |
|---|---|---|---|
| `dlls/combat.cpp` | 358 | UNDONE | Nur Menschen werfen Schädel; Monster-Gibs fehlen |
| `dlls/combat.cpp` | 863–864 | UNDONE | Generische Heilung und zeitbasierter Schaden unfertig |
| `dlls/combat.cpp` | 926 | TODO | Schrotflinte: nach Zusammenfassen der Schüsse entfernen |
| `dlls/combat.cpp` | 1132 | UNDONE | Schaden-Maske statt `ignore` verwenden |
| `dlls/gargantua.cpp` | 627 | UNDONE | Schaden-Maske statt `ignore` |
| `dlls/gargantua.cpp` | 844 | UNDONE | Treffergruppen-spezifischer Schaden fehlt |
| `dlls/gargantua.cpp` | 88 | UNDONE | Sprite-Liste wird immer neu erzeugt statt wiederverwendet |
| `dlls/crowbar.cpp` | 273 | UNDONE | Korrekter Schnittpunkt bei Hull-Treffern nicht berechnet |

### Sonstige Server-DLL

| Datei | Zeile | Typ | Beschreibung |
|---|---|---|---|
| `dlls/cbase.cpp` | 148 | UNDONE | `Spawn()` hat keinen Rückgabe-Code zum Löschen der Entität |
| `dlls/cbase.cpp` | 724 | UNDONE | Immun/Resistenz gegen Schadenstypen nicht implementiert |
| `dlls/cbase.cpp` | 804 | UNDONE | Lookup-Tabelle für `m_pfnThink` fehlt |
| `dlls/cbase.h` | 52 | UNDONE | Transition-Volumes werden ignoriert (nur PVS) |
| `dlls/client.cpp` | 794 | UNDONE | Zweifacher Step-Sound beim Landen nicht implementiert |
| `dlls/client.cpp` | 935 | UNDONE | Nur Anzahl der Spray-Frames gesetzt, kein vollständiges Logo |
| `dlls/client.cpp` | 1369/1523 | FIXME | Code sollte in Skripte ausgelagert werden |
| `dlls/func_break.cpp` | 483 | UNDONE | Kein Deckenkachel-Scherbensound |
| `dlls/func_tank.cpp` | 1108 | FIXME | Toleranzwert für Schussrichtung nicht verwendet |
| `dlls/items.cpp` | 411 | UNDONE | Korrekter Langesprung-Sound für Suit unbekannt |
| `dlls/items.cpp` | 563 | TODO | Richtiger Suit-Benachrichtigungstyp für `env_det5` finden |
| `dlls/maprules.cpp` | 475 | UNDONE | Team-Namen aus `teamlist`-CVAR nicht befüllt |
| `dlls/multiplay_gamerules.cpp` | 852 | TODO | Ausgabe direkt zur Konsole statt umgeleitet |
| `dlls/scripted.cpp` | 286 | UNDONE | Trigger nur temporär deaktiviert |
| `dlls/scripted.cpp` | 849/852 | UNDONE | Ursprung nur verschieben wenn Sequenz tatsächlich gespielt |
| `dlls/spectator.cpp` | 17/137 | UNDONE | Zustand der Spectator-Entität vollständig unklar |
| `dlls/subs.cpp` | 487–488 | UNDONE | Winkel-Vektoren durch Level-Transition-Verhalten unklar |
| `dlls/talkmonster.cpp` | 726–727 | UNDONE | Follow-Liste ohne LRU-Verwaltung und ohne Restore-Prüfung |
| `dlls/talkmonster.cpp` | 1171 | UNDONE | `SetAnswerQuestion`-Aufruf als „EVIL" markiert |
| `dlls/util.cpp` | 711/773 | FIXME | Prüfung `i < MAX_ALIASNAME_LEN` fehlt |
| `dlls/util.cpp` | 952–955 | UNDONE | Screen-Shake: nicht-geerdet, Falloff, Steuerung, und `trigger_camera` unvollständig |
| `dlls/util.cpp` | 1928 | UNDONE | Kein "Ploink"-Sound |
| `dlls/util.cpp` | 2489 | UNDONE | Doppelte Ausführung nötig? |
| `dlls/util.h` | 473 | TODO | `SF_PUSH_USECUSTOMSIZE` noch nicht verwendet |
| `dlls/animation.cpp` | 209 | UNDONE | Callback zum Prüfen ob Sound geprecacht fehlt |
| `dlls/animating.cpp` | 148 | FIXME | Workaround damit Events nicht verloren gehen |
| `dlls/roach.cpp` | 55 | UNDONE | Save/Restore für Roach-Daten unklar |

---

## Client-DLL

| Datei | Zeile | Typ | Beschreibung |
|---|---|---|---|
| `cl_dll/ev_hldm.cpp` | 110 | FIXME | Prüfung ob `playtexture`-Sound MoveVar gesetzt ist |
| `cl_dll/ev_hldm.cpp` | 1266 | TODO | Fliegenden Bolzen vollständig clientseitig vorhersagen |
| `cl_dll/health.cpp` | 102 | TODO | Lokale Gesundheitsdaten aktualisieren |
| `cl_dll/health.cpp` | 304 | TODO | Shift-Wert für Gesundheitsanzeige ermitteln |
| `cl_dll/hud_msg.cpp` | 231 | TODO | Viewangle-Kick und visuelle Schadensanzeige fehlen |
| `cl_dll/StudioModelRenderer.cpp` | 374 | UNDONE | Memory-Leak bei `Mem_Calloc` für Sequenz-Cache |
| `cl_dll/StudioModelRenderer.cpp` | 440–442 | TODO | Knochen-Matrix: cachen, Lookup-Tabelle, lazily pro Entität speichern |
| `cl_dll/StudioModelRenderer.cpp` | 537 | FIXME | Clipping-Fall nicht behandelt |
| `cl_dll/demo.cpp` | 29 | FIXME | Buffer-Helper-Funktionen für typunsicheres Casting fehlen |
| `cl_dll/inputw32.cpp` | 256 | FIXME | Direkt durch Engine routen statt eigene Implementierung |
| `cl_dll/studio_util.cpp` | 141 | FIXME | Eingaben sollten auf halben Winkel skaliert werden |
| `cl_dll/tri.cpp` | 188 | UNDONE | Gouraud-Shading lässt Tracer auf manchen Grafikkarten verschwinden |
| `cl_dll/util.cpp` | 45/84 | FIXME | `sqrt` direkt aufgerufen – möglicherweise Genauigkeitsproblem |
| `cl_dll/view.cpp` | 142 | FIXME | Quaternionen zur Vermeidung von Sprüngen verwenden |
| `cl_dll/view.cpp` | 597 | FIXME | Origin wird mit 1/128 übertragen – Kommentar aktualisieren? |
| `cl_dll/hl/hl_weapons.cpp` | 160 | UNDONE | Nachlad-Sound fehlt |
| `cl_dll/hl/hl_weapons.cpp` | 335 | FIXME | Munitionscode deaktiviert (`#if 0`) – fehlt Ammo-Info auf Client |
| `cl_dll/hl/hl_weapons.cpp` | 706 | FIXME | Waffenauswahl sollte Methode pro Waffe sein |
| `cl_dll/input.cpp` | 10 | TODO | Bob und Pitch-Drift-Code aus `view.cpp` hierher verschieben |
| `cl_dll/input.cpp` | 56 | TODO | Client-DLL-Funktion zum Lesen/Löschen von Impulsen fehlt |

---

## Physik-Shared

| Datei | Zeile | Typ | Beschreibung |
|---|---|---|---|
| `pm_shared/pm_math.c` | 193 | FIXME | Quaternionen statt Euler zur Vermeidung von Gimbal-Lock verwenden |
| `pm_shared/pm_math.c` | 319/336 | FIXME | `sqrt` direkt aufgerufen |
| `pm_shared/pm_shared.c` | 333 | FIXME | `mp_footsteps` muss ein MoveVar werden |
| `pm_shared/pm_shared.c` | 392 | FIXME | Wert sollte in Player-State verschoben werden |
| `pm_shared/pm_shared.c` | 597 | UNDONE | Keine definierten Konstanten für Lauf-, Geh-, Crouch-Geschwindigkeiten |
| `pm_shared/pm_shared.c` | 601 | UNDONE | Gehen sollte serverseitig verwaltet werden |
| `pm_shared/pm_shared.c` | 1468 | FIXME | Steile Schrägen nicht geprüft |
| `pm_shared/pm_shared.c` | 2623 | UNDONE | Treppensteigen basiert auf Geschwindigkeit, nicht Blickwinkel |

---

## Engine-Header

| Datei | Zeile | Typ | Beschreibung |
|---|---|---|---|
| `engine/studio.h` | 32 | TODO | `MAXSTUDIOTRIANGLES` (20000) anpassen/tunen |
| `engine/studio.h` | 33 | TODO | `MAXSTUDIOVERTS` (2048) anpassen/tunen |

---

## VGUI-Utils

Diese TODOs stammen aus dem Valve-eigenen VGUI-Code und betreffen die UI-Bibliothek.

| Datei | Zeile | Beschreibung |
|---|---|---|
| `utils/vgui/include/VGUI.h` | 42–78 | Look-and-Feel, Tooltips, Hotkeys, Background, member-Sichtbarkeit, Benennungskonventionen, Layouts etc. |
| `utils/vgui/include/VGUI_Color.h` | 14–17 | Umbenennung der Farb-Getter/Setter |
| `utils/vgui/include/VGUI_Font.h` | 18 | Fonts/Cursors wie GL-Binds behandeln |
| `utils/vgui/include/VGUI_Button.h` | 23 | `Button` von `AbstractButton` ableiten |
| `utils/vgui/include/VGUI_Label.h` | 15 | `Label` sollte `TextImage` intern nutzen |
| `utils/vgui/include/VGUI_Image.h` | 15 | Insets-Konzept fehlt |
| `utils/vgui/include/VGUI_Border.h` | 14 | Alle Borders sollten betitelt sein |
| `utils/vgui/include/VGUI_App.h` | 35–36 | Public/Private-Zugriffsstruktur unorganisiert |
| `utils/vgui/include/VGUI_ListPanel.h` | 19 | `ScrollPanel` erstellen und für `ListPanel` nutzen |
| `utils/vgui/include/VGUI_TextImage.h` | 16 | Wrapping-Flag statt implizitem Verhalten |
| `utils/vgui/include/VGUI_FileInputStream.h` | 11 | `stdio` / `FILE`-Forward-Deklaration in VC6 defekt |

---

## Sonstige Utils

| Datei | Zeile | Typ | Beschreibung |
|---|---|---|---|
| `utils/common/wadlib.c` | 301 | FIXME | Kompression nicht implementiert |
| `utils/common/cmdlib.c` | 887 | FIXME | Byte-Swap ggf. notwendig |
| `utils/common/mathlib.c` | 31 | FIXME | `sqrt` direkt aufgerufen |
| `utils/common/mathlib.c` | 270 | FIXME | Eingaben sollten auf halben Winkel skaliert werden |

---

## Stub-Funktionen (Client-Prediction)

Die folgenden Stubs in `SpiritSource/cl_dll/com_weapons.cpp` sind absichtlich leere Implementierungen für die Client-Prediction und benötigen **keine** echte Logik, sind aber als potenzielle Erweiterungspunkte dokumentiert:

- `stub_PrecacheModel`
- `stub_PrecacheSound`
- `stub_PrecacheEvent`
- `stub_NameForFunction`
- `stub_SetModel`

---

*Generiert am 2026-04-05 aus dem Quellcode von Spirit of Half-Life 1.5.*
