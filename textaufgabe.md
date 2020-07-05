# MALIS 19.3 WPM_T9.2 / Aufgabe: Shell-Aufgabe

Autorin: Jana Mandrys

Aufgabenstellung:
Erstellen Sie ein Shell-Script, das die Datei "2020-05-23-Article_list_dirty.tsv" bereinigt und eine neue Datei erstellt, die nur noch die zwei Spalten mit ISSNS und den Veröffentlichungsjahren enthält.
Der Name der neuen Datei: "2020-05-23-Article_List_ISSN_Dates.tsv".


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

- Creator
- Issue
- Volume
- Journal
- **ISSN**
- ID
- Citation
- Title
- Place
- Labe
- Language
- Publisher
- **Date**

Hier kann ich nun gut sehen, dass 
- Informationen zur ISSN an 5. Stelle der Tabelle und
- Informationen zu Date an 12. Stelle der Tabelle stehen.

## 3. Schritt: Die interessanten Informationen aus der Datei "dirty.tsv" separieren

Diese beiden Informationen an 5. und an 12. Stelle möchte ich nun aus der Datei "dirty.tsv" herausschneiden und sie in einer neuen Datei "dirty_better.tsv" ablegen.

$ cat dirty.tsv | cut -f 5,12 > dirty_better.tsv

Mit dem Befehl $ cat dirty_better.tsv lasse ich mir den Inhalt der verschlankten Datei anzeigen, um sehen zu können, welche weiteren Schritte zur Bereinigung nötig sind.

## 4. Schritt: Bereinigen von "Unreinheiten"

### 4.1. "eng" entfernen

Als erstes fällt auf, dass es mehrere Zeilen ohne ISSN, dafür mit irgendwas "eng" gibt. Diese möchte ich entfernen. Die Tabelle ohne diese Zeilen speichere ich in "dirty_better2.tsv".

$ cat dirty_better.tsv | grep -v eng > dirty_better2.tsv

Ich überprüfe wieder mit $ cat dirty_better2.tsv und kann nun sehen, dass die Zeilen mit irgendwas "eng" weg sind.

### 4.2. "ISSN:" und "issn:" entfernen

Als nächstes stören mich die Zeilen, die ebenfalls unnötige Wörter enthalten, wie "ISSN:" oder "issn:" - weg soll auch der Doppelpunkt.

Um sowohl die Wörter in der Klein-, als auch Großschreibung zu erwischen, muss ich zunächst alle Varianten in eine einheitliche Variante verändern. Ich entscheide mich für eine Umwandlung in Großschreibung.
Das Ergebnis dieser Veränderung speichere ich in die Datei "dirty_better3.tsv".

$ tr \[:lower:]\[:upper:] < dirty_better2.tsv > dirty_better3.tsv

Eine Überprüfung mit $ cat dirty_better3.tsv zeigt nun alle Schreibweisen verändert in "ISSN:" - es hat also geklappt.


Da nun alles einheitlich ist, kann ich alle "ISSN:" löschen und die Leerzeichen gleich mit.
Wiederum lasse ich die Ergebnisse dieses Befehls in eine Datei fließen: "dirty_better4.tsv".

$ tr -d 'ISSN:' < dirty_better3.tsv > dirty_better4.tsv

Ich überprüfe das Ergebnis wieder mit $ cat dirty_better4.tsv.

### 4.3. "DATE" entfernen

Jetzt stört mich noch das Wort "DATE", das noch über der Tabelle steht. Dieses entferne ich im nächsten Schritt und speichere das Ergebnis in "dirty_better5.tsv".

$ cat dirty_better4.tsv | grep -v DATE > dirty_better5.tsv
 
Ich schaue mir die Datei wieder an mit $ cat dirty_better5.tsv und habe nur noch Zahlen und Leerzeichen. 

### 4.4. Tabelle sortieren und Leerzeichen entfernen

Mit dem nächsten Befehl sortiere ich die enthaltenen Zahlen und schaue mir das Ergebnis dann an.
Mit dem Befehl sort -u kann ich gleichzeitig die doppelten Zeilen heraussortieren. (u = unique)

$ sort -u dirty_better5 > dirty_better6.tsv

Bei der Überprüfung mit $ cat dirty_better6.tsv sehe ich, dass gleichzeitig auch alle leeren Zeilen entfernt wurden. Das freut mich, denn damit ist die Datei sehr schön sauber geworden! 

### 4.5. 

Die bereinigte Datei "dirty_better6.tsv" bennene ich nun um bzw. erstelle ich eine abgabefertige Datei mit dem richtigen Namen: "2020-05-23-Article_List_ISSN_Date.tsv"

$ cat dirtybetter6.tsv > "2020-05-23-Article_List_ISSN_Date.tsv


Damit ist die Bereinigung der Datei abgeschlossen.
