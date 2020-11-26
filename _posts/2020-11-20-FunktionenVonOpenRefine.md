# Funktionen von OpenRefine

Auf die Basisfunktionen von OpenRefine gehe ich hier nicht ein. Im folgenden Tutorial von der LibraryCarpentery werden diese alle erläutert:

````
https://librarycarpentry.org/lc-open-refine/
````

In diesem Blogbeitrag behandle ich die beiden äusserst hilfreichen Funktionen "Template-Export" und "Reconciliation".

### Template-Export

Dies ist eine sehr hilfreiche Funktion von OpenRefine – der Export der Daten zu einem MARCXML-File. Die Datenstruktur kann hier selber festgelegt werden.

Über "Export" --> "Templating..." wird das Fenster aufgerufen. Nun kann das Prefix und das Suffix sowie der Separator (meist eine Pipe "|") festgelegt werden. Ausserdem gibt es ein Feld "Row Template" wo die Struktur der jeweiligen Datensätze festgelegt werden kann. Variablen werden in geschweiften Klammern geschrieben  und anschliessend werden die entsprechenden Werte aus den Tabellen abgerufen. Gleich im Templating-Fenster auf der Rechten Seite wird eine Vorschau angezeigt, damit man immer gleich die gemachten Änderungen sehen kann.

#### Good to know

Die Funktion "escape('xml')" ersetzt die in XML nicht erlaubten Zeichen durch die jeweiligen XML-Entsprechungen.

Wenn es in einer Spalte leere Zeilen hat, man aber die Werte von OpenRefine in ein MARCXML umwandeln will, braucht man die Funktion "forNonBlank()":

````
{{
forNonBlank(
    cells['DOI'].value,
    v,
    '<datafield tag="024" ind1="7" ind2=" ">
        <subfield code="a">' + v.escape('xml') + '</subfield>
        <subfield code="2">doi</subfield>        
    </datafield>',
    ''
)
}}
````

Hier wird bei nicht-leeren Zellen die Variable unter "v" abgespeichert. Anschliessend werden die definierten datafields und subfields ausgegeben. Subfield "a" enthält auch die Variable "v". Falls die Zelle leer ist, wird ein leerer String ausgegeben.

Man muss unbedingt verhindern, dass leere Zellen mit Inhalt "null" vorkommen. Je nach OpenRefine-Version wird das als gültiger Wert übernommen und die Funktion "forNonBlank()" funktioniert nicht zuverlässig. Durch eine Facette können diese Felder aber alle auf einmal abgeändert werden.

### "Reconciliation" – Anreicherung von Datensätzen

Mit dieser Funktion können bereits bestehende Datensätze wie z.B. Personen mit Zusatzinformationen (z.B. Geburtsdatum / Beruf / Titel) angereichert werden. OpenRefine bedient sich dafür bei bereits bestehenden Bibliotheken. Beispielsweise kann auf die Gemeinsame Normdatei (GND) zugegriffen werden, somit stehen alle Daten der GND zur Anreicherung zur Verfügung: normierte Einträge für Personen, Körperschaften, Kongresse, Geografika, Sachschlagwörter und Werktitel. Der Zugriff erfolgt über die Rechercheoberfläche "lobid-gnd", welche über die LOD-API abrufbar ist.

Über den Reiter der gewünschten Spalte --> "Reconcile" --> "Start reconciling" kann die Funktion gestartet werden. Über "Add Standard Service..." muss erstmal eine Datenbank verfügbar gemacht werden. Die URL kann nun eingegeben werden, in unserem Fall diese der lobid-gnd:

```
https://lobid.org/gnd/reconcile
```

Nun kommt das Fenster zum Anreichern der Spalten. Links kann ausgewählt werden, mit welcher Entität die Spalteninhalte abgeglichen worden sind. Falls dort der gewünschte Eintrag nicht zu finden ist, kann unten auch eine andere Entität ausgewählt werden. Wenn die Ergebnisse noch exakter sein sollen, können auf der Rechten Seite des Fensters noch weitere Spalten abgeglichen werden. Anschliessend wird der Abgleich über "Start Reconciliation" ausgeführt.

Es werden bei jedem Feld Vorschläge gemacht. Die korrekten Vorschläge können nun ausgewählt werden. Unterstützung kann dabei die "Preview" geben, die erscheint, wenn mit dem Cursor über den jeweiligen Vorschlag gefahren wird. Falls kein passender "Match" erscheint, kann in den einzelnen Zellen auch noch spezifisch danach gesucht werden oder der Abgleich zu dieser Person abgebrochen werden. Bei der Übung die wir gemacht haben, gab es sehr wenige automatische Matches. Dies lässt sich wohl darauf zurückführen, dass die Personen von unserer Übungstabelle (noch) nicht sehr bekannt sind und somit nur sehr wenige davon einen eigenen Eintrag in der GND haben. In diesen Fällen habe ich also die jeweilige Anreicherung abbrechen müssen. Bei den meisten Personen, die einen Match haben, wurde dieser automatisch verbunden. Dies, da ich zuvor den entsprechenden Haken im Reconciliation-Fenster gesetzt habe.