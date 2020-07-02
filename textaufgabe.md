# MALIS 19.3 WPM_T9.2 / Aufgabe: Shell-Aufgabe

Autorin: Jana Mandrys

Aufgabenstellung:
Erstellen Sie ein Shell-Script, das die Datei "2020-05-23-Article_list_dirty.tsv" bereinigt und eine neue Datei erstellt, die nur noch die zwei Spalten mit ISSNS und den Veröffentlichungsjahren enthält.
Der Name der neuen Datei: "2020-05-23-Dates_and_ISSNs.tsv


## 1. Schritt: Download der Datei "2020-05-23-Article_list_dirty.tsv"

Ich lade die obengenannte Datei, die es zu bearbeiten gilt, herunter und speichere sie im Ordner "foerstner".
Dann erstelle ich qusi eine Kopie mit dem Namen "dirty.tsv", um daran zu arbeiten. Die Originaldatei soll nicht verändert werden.

$ cat 2020-05-23-Article_list_dirty.tsv > dirty.tsv

## 2. Schritt: Arbeiten mit der Datei "dirty.tsv"

Zunächst einmal schaue ich mir mit dem Befehl $ cat dirty.tsv die Zeilen der Datei an.
Mit dem Befehl head -n kann ich eine beliebige Zeilenanzahl eingeben, die ich mir anschauen möchte. Ich entscheide mich für die erste Zeile, also:

$ head -n1 dirty.tsv

So kann ich gut erkennen, wie die Tabelle strukturiert ist, da ich nun in der ersten Zeile die Spaltenüberschriften lesen kann.
Diese lauten:

Creator
Issue
Volume
Journal
ISSN
ID
Citation
Title
Place
Labe
Language
Publisher
Date

Hier kann ich nun gut sehen, dass 
- Informationen zur ISSN an 5. Stelle und
- Informationen zu Date an 12. Stelle stehen.

## 3. Schritt: Die interessanten Informationen aus der Datei "dirty.tsv" separieren

Diese beiden Informationen an 5. und an 12. Stelle möchte ich nun aus der Datei "dirty.tsv" herausschneiden und sie in einer neuen Datei "dirty_better.tsv" ablegen.

$ cat dirty.tsv | cut -f 5,12 > dirty_better.tsv

Mit dem Befehl $ cat dirty_better.tsv lasse ich mir den Inhalt der verschlankten Datei anzeigen, um sehen zu können, welche weiteren Schritte zur Bereinigung nötig sind.

## 4. Schritt: Bereinigen von "Unreinheiten"

Als erstes fällt auf, dass es mehrere Zeilen ohne ISSN, dafür mit irgendwas "eng" gibt. Diese möchte ich entfernen. Die Tabelle ohne diese Zeilen speichere ich in "dirty_better2.tsv".

$ cat dirty_better.tsv | grep -v eng > dirty_better2.tsv

Ich überprüfe wieder mit $ cat dirty_better2.tsv und kann nun sehen, dass die Zeilen mit irgendwas "eng" weg sind.
Als nächstes stören mich die Zeilen, die ebenfalls unnötige Wörter enthalten, wie "ISSN:" oder "issn:" - weg soll auch der Doppelpunkt.

Um sowohl die Wörter in der Klein-, als auch Großschreibung zu erwischen, muss ich zunächst alle Varianten in eine einheitliche Variante verändern. Ich entscheide mich für eine Umwandlung in Großschreibung.

$ tr \[:lower:]\[:upper:] < dirty_better2.tsv > dirty_better3.tsv

 
