### Template-Export

Eine sehr hilfreiche Funktion – der Export der Daten zu einem MARCXML-File.

escape('xml') ersetzt die in XML nicht erlaubten Zeichen durch die jeweiligen XML-Entsprechungen.

Wenn es in einer Spalte leere Zeilen hat, man aber die Werte von OpenRefine in ein MARCXML umwandeln will, braucht man die Funktion "forNonBlank()":

```
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
```

Hier wird bei nicht-leeren Zellen die Variable unter "v" abgespeichert. Anschliessend werden die definierten datafields und subfields ausgegeben. Subfield "a" enthält auch die Variable "v". Falls die Zelle leer ist, wird ein leerer String ausgegeben.

Man muss unbedingt verhindern, dass leere Zellen mit Inhalt "null" vorkommen. Sonst wird das als gültiger Wert übernommen und die Funktion "forNonBlank()" funktioniert nicht zuverlässig. Aber durch eine schnell gesetzte Facette können diese Felder alle auf einmal abgeändert werden.