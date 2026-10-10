# PAP-Analysis-Toolbox

Excel → Python → LaTeX in einem Ordner. Fasst die Notebooks `01`–`07` und den Error-Calculator
(`messreihe.py`, `gauss.py`, `serie.py`, `sigma.py`, `paprund.py`) in **einem** Notebook zusammen.

## Benutzung
1. Repo als neuen Versuchsordner klonen oder kopieren (ohne `.git`), z. B. nach `PAP2-Versuch-XX/`.
2. `messwerte.xlsx` ausfüllen (Regeln im Blatt `_Anleitung`, Beispiel im Blatt `Aufgabe1`).
3. `Auswertung.ipynb`: Teil 0 (Einstellungen), Teil 1 (Rechner, einmal ausführen),
   Teil 2 (Auswertung – Beispielzellen kopieren/anpassen), Teil 3 (`export_ergebnisse()`).
4. `main.tex`: `\makecoverpage{...}` ausfüllen, Exporte einbinden, kompilieren.

## Ordner
```
PAP-Analysis-Toolbox/
├─ Auswertung.ipynb        Rechner (R1–R12) + Auswertung
├─ messwerte.xlsx          Messtabellen und Konstanten
├─ main.tex, sty/          LaTeX-Vorlage (PAP1-Template, angepasst)
└─ export/                 ALLES, was das Notebook schreibt (jederzeit löschbar)
   ├─ ergebnisse.tex       Makros \erg{name}, \ergwert, \ergfehler, \ergsig
   ├─ tabellen/<name>.tex  \input{export/tabellen/<name>} in einer table-Umgebung
   ├─ formeln/<name>.tex   Gauß-Herleitung (align, Labels eq:<name>, eq:d<name>)
   ├─ budget/<name>.tex    Fehlerbudget (optional, braucht booktabs)
   └─ diagramme/<name>.png
```

## Excel
- Blatt `Konstanten`: `Name | Wert | Fehler | Einheit | Quelle | Beschreibung`
- Ein Blatt pro Aufgabe: `# name` in Spalte A startet eine Tabelle (rechts daneben Beschreibung),
  nächste Zeile = Kopf, Daten bis zur ersten Leerzeile. Ein Blatt ohne `#` = eine Tabelle (Blattname).
- Kopf `t1 [ms]`, Fehler in `Δt1 [ms]`: nur erste Zeile gefüllt → gilt für alle Zeilen;
  alle gefüllt → je Zeile; teilweise → Fehlermeldung. Blätter mit `_` am Anfang werden ignoriert.

## LaTeX
- Präambel: `siunitx` (v3, `\qty`/`\unit`), `booktabs`, `float`, `\input{export/ergebnisse}`.
- Sprache: `\usepackage{sty/Layout}` (deutsch) oder `\usepackage[english]{sty/Layout}`.
- `\makecoverpage` hat neue Schlüssel `pap` (Standard 2), `author` (Standard Matthes Neumann),
  `partner` (Standard Theo Senoner); Kopfzeile (PAP, Versuch, Gruppe, Partner) kommt automatisch daraus.
- Falscher Name in `\erg{...}` → Kompilierfehler „... fehlt in export/ergebnisse.tex“.

## Methoden (wie bisher)
- PAP-Rundung: Fehler aufrunden, 2 Stellen bei führender 1/2, sonst 1; Wert auf dieselbe Stelle.
- Mittelwert: Δx̄ = sqrt(S_M² + Δ_Gerät²), Gerätefehler aus der Δ-Spalte oder `geraet=`.
- Gauß mit sympy (auch für ganze Spalten), 20-%-Regel nur markiert, nicht angewendet.
- Regression: gewichtet mit 1/Δy_fit², Δy_fit = sqrt(Δy² + (m_grob Δx)²), Fehlergeraden m ± Δm durch den Schwerpunkt.
- σ-Vergleich: t = |a − b| / sqrt(Δa² + Δb²); < 1 sehr gut, < 2 verträglich, < 3 grenzwertig, ≥ 3 signifikant.

## Abhängigkeiten
numpy, sympy, matplotlib, openpyxl, ipython/jupyter
