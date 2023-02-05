# Rezept: Korrelationen entdecken


<div markdown=1 class="alert alert-warning">
Sie müssen den Code in diesem Rezept an Ihre konkrete Situation anpassen. 
</div>

### Korrelationen in Variablen entdecken

Mit der Bibliothek ```corrr``` haben wir eine leistungsstarke Bibliothek zur Exploration von Korrelationen, die uns die Arbeit in R leicht macht. Damit können wir in einem Durchgang alle Korrelationskoeffizienten für alle Variablenpaare bestimmen. Wir müssen also nicht jedes Variablenpaar einzeln bestimmen. 

Bei dieser Technik müssen wir vorher alle nominalskalierten Variablen aus unserer Stichprobe entfernen. Ausserdem müssen wir alle ordinalskalierten Variablen, die mit Zeichenketten kodiert sind, in Zahlenwerte konvertieren, so dass die Ordnung dieser Variablen widergespiegelt wird. 

Anschliessend können wir mit der Funktion `correlate()` alle Korrelationskoeffizienten der verbliebenen Variablen bestimmen, wenn unsere bereinigte und konvertierte Stichprobe in der R-Variablen `vergleichbareDaten` gespeichert sind. Im folgenden Beispiel gehe ich davon aus, dass wir nur ordinalskalierte Variablen vergleichen. Deshalb müssen wir die Methode `spearman` auswählen. 

```
library(corrr)

daten = read_csv("befragung.csv")

daten %>%
    select(-c(kurs, q00_2, q09_1, q07_1, q08_1, nr, q01)) -> ordinalskalierteDaten

ordinalskalierteDaten %>% 
    correlate(method = "spearman") -> korrelationen
```

Mit diesen Daten können wir wie gewohnt weiterarbeiten. 
Das Problem ist aber, dass diese Daten als Matrix geliefert werden, so dass wir kaum erkennen können, wo interessante Korrelationen vorliegen. Für die Visualisierung formen wir die Daten um. 

```
korrelationen %>%
   pivot_longer(-rowname, names_to = "y", values_to = "r") %>%
   rename(x = rowname) %>%

   # Fehlende Werte entfernen
   drop_na() %>%

   # Symmetrie der paarweisen Vergleiche entfernen
   filter(x > y) -> korrelationsDreieck
```

Anschliessend können wir ein *Heatmap*-Diagramm erstellen.  Eine Heatmap zeigt uns über die Farbkodierung den Korrelationskoeffizienten für ein Variablenpaar an. Dabei liegen die Variablennamen jeweils auf der X- und auf der Y-Achse und die Färbung wird jeweils am Kreuzungspunkt eingefügt. 

```
korrelationsDreieck %>%
   ggplot(aes(x, y, fill = r)) +
        geom_tile() +

        # Farbcodes definieren
        scale_fill_gradient2(low = "blue", high = "red", mid = "white", 
                             midpoint = 0, limit = c(-1,1), space = "Lab", 
                             name="spearman\nCorrelation") +

        # Bezeichnungen der X-Achse lesbar darstellen
        theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1)) 
```

Die folgende Abbildung zeigt ein Beispiel für eine Heatmap.

<img src="https://github.com/dxiai/statistik/raw/main/bilder/corrr/heatmap.png" width="50%">

