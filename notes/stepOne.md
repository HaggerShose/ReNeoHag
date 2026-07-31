# Step 1: D-Compiler + Editor-Setup für ReNeoHag

Ziel: Auf diesem Rechner `dub build` ausführen können und optional D-Unterstützung in Cursor.

Branch für eigene Arbeit: `haggars`
Fork: `HaggerShose/ReNeoHag`
Upstream (Original): `Rojetto/ReNeo` (seit ~3 Jahren kaum aktiv)

---

## 1. Was du brauchst
ReNeo baut mit:

- **D-Compiler** (empfohlen zum Start: **DMD**)
- **`dub`** (Package-/Build-Tool, kommt beim DMD-Installer meist mit)

Optional später: **LDC** (LLVM-basiert, oft etwas bessere Release-Optimierung). Für den ersten Win-Modifier-Patch reicht DMD völlig.

---

## 2. Compiler installieren (Windows)
### Variante A: winget (empfohlen)
```powershell
winget search Dlang.DMD
winget install Dlang.DMD
```
Paket-ID: `Dlang.DMD`  
Danach **Terminal komplett schließen und neu öffnen**, damit PATH greift.  
Prüfen:
```powershell
dmd --version
dub --version
```
Beide Befehle sollten eine Versionsnummer zeigen. Fehlt `dub`, siehe unten.  
**LDC:** Bei winget war kein klares `Dlang.LDC`-Paket sichtbar. LDC eher manuell von [Dlang-Download](https://dlang.org/download.html), falls du es später willst. Für den Start reicht DMD.

## 3. Ersten Build von ReNeoHag
Im Repo-Root (Ordner mit `dub.json`):
```powershell
cd C:\ProgrammePortable\ReNeoHag
dub build --build=release
```
Andere Build-Typen (Upstream-README):
```powershell
dub build                          # Debug + Konsole
dub build --build=debug-log        # Debug + reneo_log.txt (kann sensible Tipps loggen)
dub build --build=release          # Alltag: optimiert, keine Konsole
```
Erwartetes Ergebnis im Repo-Root:
- `reneo.exe`
- Cairo-DLLs (laut `dub.json` aus `lib\x86_64\` nach Root kopiert)

Die laufende EXE sucht Configs **neben der EXE** (= Repo-Root):
- `layouts.json`
- `config.json` (oft beim ersten Start erzeugt; in `.gitignore`)
- `compose\`
- `keysymdef.h`

Damit ist das geklonte Repo gleichzeitig Dev- und Alltagsverzeichnis.

### Falls der Build wegen `reneo.res` scheitert
`res\reneo.res` muss existieren. Im Clone ist die Datei meist schon drin. Sonst im Ordner `res\` mit Windows-SDK `rc.exe` (x86-Variante):
```powershell
rc.exe reneo.rc
```
---
## 4. Cursor-Extension für D
|       |                                                                      |
| ----- | -------------------------------------------------------------------- |
| Name  | **D Programming Language (code-d)**                                  |
| ID    | `webfreak.code-d`                                                    |
| Zweck | Autocomplete, Lint, Goto-Symbol, ... via Language Server **serve-d** |

In Cursor: Extensions -> `code-d` oder `D Programming Language` suchen -> installieren.  
Hinweise:  
- Die Extension kann anbieten, serve-d / Tools nachzuinstallieren -- ok.
- Sie **ersetzt nicht** den Compiler. `dmd` + `dub` trotzdem per winget/Installer setzen.
- Für den kleinen Patch (Win in `mods`) reicht auch reines Terminal ohne Extension.
---
## 5. Kurzer Workflow danach
1. Auf Branch `haggars` arbeiten
2. Code unter `source\` patchen (z.B. `mapping.d`, `reneo.d` für Win in `mods`)
3. `dub build --build=release`
4. Alte ReNeo-Instanz beenden, neue `reneo.exe` aus diesem Ordner starten
5. Nur Layout ändern -> meist nur ReNeo neu starten, kein Rebuild

Configs aus dem alten Portable-Ordner hierher übernehmen, damit du nicht das Upstream-Standardlayout nutzt:  
- von `C:\ProgrammePortable\ReNeo\layouts.json` (dein NeoHag)
- von `C:\ProgrammePortable\ReNeo\config.json`
---
## 6. Git-Kurznotiz
- `master` -- Upstream-/Basisstand
- `haggars` -- dein Arbeitsbranch (Patches, Layout, Notes)
- `origin` -- dein Fork (`HaggerShose/ReNeoHag`)
- `upstream` -- `Rojetto/ReNeo`

Branch auf GitHub veröffentlichen (falls noch nicht):
```powershell
git push -u origin haggars
```
---
## 7. Checkliste
- [X] `winget install Dlang.DMD`
- [X] Neues Terminal: `dmd --version` und `dub --version` ok
- [X] Optional: Extension `webfreak.code-d` in Cursor
- [ ] `dub build --build=release` im Repo-Root erfolgreich
- [ ] `reneo.exe` startet; eigene `layouts.json` / `config.json` liegen daneben
- [X] Arbeit läuft auf Branch `haggars`
