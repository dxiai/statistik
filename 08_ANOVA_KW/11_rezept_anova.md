# Rezept: Mehrere ANOVA in einem Schritt durchführen

Jetzt wo wir das Grundprinzip der ANOVA kennen, können wir auch für die anderen Variablen die Unterschiede überprüfen. Allerdings ist es umständlich, für jede Variable die gleichen Schritte zu wiederholen.

Das folgende Rezept führt jeweils eine ANOVA für jede Variable in der `iris`-Stichprobe durch. Der Trick ist hier, dass wir zuerst mit `pivot_longer()` alle Variablen in zwei Variablen verbinden. Diese Variablen ist der `Variablenname` und `Wert`. 

Mit der Funktion `nest_by()` fassen wir nun die Werte eines Variablennamen und die zugehörigen Spezies in einer neuen Unterstichprobe zusammen. `next_by()` funktioniert dabei wie `group_by()` mit dem kleinen Unterschied, dass wir echte Teilstichproben erstellen. Standardmässig legt `next_by()` für diese Stichproben den Vektor `data` an. Mit dem Parameter `.key =` können wir diesem Vektor einen beliebigen Namen zuweisen. In diesem Beispiel habe ich den Vektornamen `Stichprobe` gewählt. 
 
Anschliessend können wir dann mit `mutate` für jede dieser Teilstichproben eine ANOVA wie oben ausführen. Dabei übergeben wir die Stichprobe, die num im Vektor `Stichprobe` gespeichert ist, dem `data`-Parameter der `aov()` Funktion. 

Im gleichen Schritt können wir den nachgereihten Tukey-Differenz ausführen, der uns die paarweisen Signifikanzen liefert. 

Dieses Objekt speichern wir zwischen. 

```R
iris %>% 
    pivot_longer(-Species, names_to = "Variablenname", values_to = "Wert") %>% 
    # Teilstichproben zusammenfassen
    nest_by(Variablenname, .key = "Stichprobe") %>% 
    # Teilstichproben auswerten
    mutate(
        anova_data = aov(Wert ~ Species, data = Stichprobe) %>% list(),
        anova = anova_data %>% tidy() %>% list(),
        tukey = anova_data %>% TukeyHSD() %>% tidy() %>% list()
    )  -> iris_anova
```

Die Präsentation unserer Ergebnisse erfolgt in zwei Schritten. 

1. Wir fassen mit `unnest()` die Ergebnisse der ANOVA Tests in eine schöne Tabelle zusammen und entfernen für die Darstellung unnötigen Vektoren mit `select()`.
2. Wir filtern die Ergebnisse der Tukey-Differenz für alle signifikanten ANOVA-Ergebnisse. Dieser Schritt ist etwas komplizierter, weil hier die Aufbereitung für die tabellarische Darstellung komplizierter ist. 

```R
# Schritt 1
iris_anova %>% 
    select(Variablenname, anova) %>% 
    unnest(anova)
```

| Variablenname | term | df | sumsq  | meansq  | statistic  | p.value  | 
|---|---|---|---|---|---|---| 
| Petal.Length | Species | 2 | 437.10280 | 218.55140000 | 1180.16118 | 2.856777e-91 | 
| Petal.Length | Residuals | 147 | 27.22260 | 0.18518776 | NA | NA | 
| Petal.Width | Species | 2 | 80.41333 | 40.20666667 | 960.00715 | 4.169446e-85 | 
| Petal.Width | Residuals | 147 | 6.15660 | 0.04188163 | NA | NA | 
| Sepal.Length | Species | 2 | 63.21213 | 31.60606667 | 119.26450 | 1.669669e-31 | 
| Sepal.Length | Residuals | 147 | 38.95620 | 0.26500816 | NA | NA | 
| Sepal.Width | Species | 2 | 11.34493 | 5.67246667 | 49.16004 | 4.492017e-17 | 
| Sepal.Width | Residuals | 147 | 16.96200 | 0.11538776 | NA | NA |


```R
# Schritt 2
iris_anova %>% 
    select(Variablenname, tukey, anova) %>% 
    unnest(anova) %>% 
    # Residuals-Zeilen entfernen
    drop_na() %>% 
    # Nur die wichtigen Daten behalten
    select(Variablenname, tukey, anova_p = `p.value`) %>% 
    # Nur signifikante ANOVA Ergebnisse auswerten
    filter(anova_p < 0.05) %>% 
    unnest(tukey) %>% 
    # Nur p-Werte anzeigen
    select(Variablenname, contrast, p_Wert = adj.p.value, anova_p)
```

| Variablenname | contrast | p_Wert  | anova_p | 
|---|---|---|---| 
| Petal.Length | versicolor-setosa | 2.997602e-15 | 2.856777e-91 | 
| Petal.Length | virginica-setosa | 2.997602e-15 | 2.856777e-91 | 
| Petal.Length | virginica-versicolor | 2.997602e-15 | 2.856777e-91 | 
| Petal.Width | versicolor-setosa | 2.997602e-15 | 4.169446e-85 | 
| Petal.Width | virginica-setosa | 2.997602e-15 | 4.169446e-85 | 
| Petal.Width | virginica-versicolor | 2.997602e-15 | 4.169446e-85 | 
| Sepal.Length | versicolor-setosa | 3.386180e-14 | 1.669669e-31 | 
| Sepal.Length | virginica-setosa | 2.997602e-15 | 1.669669e-31 | 
| Sepal.Length | virginica-versicolor | 8.287558e-09 | 1.669669e-31 | 
| Sepal.Width | versicolor-setosa | 3.097522e-14 | 4.492017e-17 | 
| Sepal.Width | virginica-setosa | 1.360737e-09 | 4.492017e-17 | 
| Sepal.Width | virginica-versicolor | 8.780206e-03 | 4.492017e-17 |

In diesem Beispiel zeigt sich, dass sich alle Sorten in allen Merkmalen signifikant unterscheiden.