# Unterschiede zwischen nominalskalierten Variablen finden - Chi-Quadrat Unabhängigkeitstest


<div class="col-12 alert alert-primary" markdown=1>
<i class="fa fa-lg fa-info-circle"></i>
Dieser Abschnitt fokussiert auf Das Kapitel 9.1 in Bortz und Schuster (2010). 
</div>

Der Chi-Quadrat-Test oder Kurz \\( \chi^2\\)-Test ist neben dem t-Test einer der wichtigsten Tests in der schliessenden Statistik.

Es gibt drei zentrale  Anwendungszenarien:

1. Unterschiede zwischen zwei unabhängigen (nominalskalierten) Variablen überprüfen
2. Unterschiede einer Variable in Bezug auf eine theoretische Verteilung überprüfen
3. Abweichungen einzelner Ausprägungen nominalskalierter Variablen überprüfen.

In diesem Abschnitt diskutieren wir nur das erste Anwendungsszenario.

Die \\( \chi^2 \\)-Verteilung hängt von der Anzahl der Freiheitsgrade ab, die zum gefundenen Ergebnis führen können. Die Verteilung ist also abhängig von der Anzahl der Faktoren, die unser Ergebnis beeinflussen.  

### \\( \chi^2 \\)-Verteilung und die Normalverteilung

Die \\( \chi^2 \\)-Verteilung ist eng mit der Normalverteilung verbunden. Diese Verbinden ist am deutlichsten bei einem Freiheitsgrad von 1 erkennbar. 

<div class="alert alert-primary" markdown=1>
Die \\(\chi^2\\)-Verteilung ist durch die Summe der Quadrate der Mittelwertabweichungen definiert. 
</div>

Die Werte dieser Verteilung sind also strenggenommen ein Mass für die Varianz einer Variablen. Diese Varianzen können wir zum Beispiel dafür verwenden, um die Abweichung eines Werts von einem beliebigen Erwartungswert einzuschätzen. Dadurch können wir sogar normalverteilte Variablen mit Hilfe der \\( \chi^2 \\)-Verteilung vergleichen. Dabei nehmen wir an, dass Abweichungen von einem Erwartungswert normalverteilt, d.h. zufällige Messfehler, sind, falls die Nullhypothese gilt.

Für die \\( \chi^2 \\)-Verteilung mit dem Freiheitsgrad 1 gilt, dass für einen p-Wert der zugehörige Wert der Verteilungsstatistik x gleich dem Quadrat des z-Werts der Standardnormalverteilung für den gleichen p-Wert bei einer *ungerichteten* Hypothese ist. Das veranschaulichen wir uns in R:

```R
tibble( z = seq(from = 0, to = 4, by = .25)) %>%
            mutate(
                p = pnorm(z),
                z2 = z^2, 
                x = qchisq(p - (1 - p), df = 1)
            )
```

| z | p  | z2  | x  | 
|---|---|---|---| 
| 0.00 | 0.5000000 | 0.0000 | 0.0000 | 
| 0.25 | 0.5987063 | 0.0625 | 0.0625 | 
| 0.50 | 0.6914625 | 0.2500 | 0.2500 | 
| 0.75 | 0.7733726 | 0.5625 | 0.5625 | 
| 1.00 | 0.8413447 | 1.0000 | 1.0000 | 
| 1.25 | 0.8943502 | 1.5625 | 1.5625 | 
| 1.50 | 0.9331928 | 2.2500 | 2.2500 | 
| 1.75 | 0.9599408 | 3.0625 | 3.0625 | 
| 2.00 | 0.9772499 | 4.0000 | 4.0000 | 
| 2.25 | 0.9877755 | 5.0625 | 5.0625 | 
| 2.50 | 0.9937903 | 6.2500 | 6.2500 | 
| 2.75 | 0.9970202 | 7.5625 | 7.5625 | 
| 3.00 | 0.9986501 | 9.0000 | 9.0000 | 
| 3.25 | 0.9994230 | 10.5625 | 10.5625 | 
| 3.50 | 0.9997674 | 12.2500 | 12.2500 | 
| 3.75 | 0.9999116 | 14.0625 | 14.0625 | 
| 4.00 | 0.9999683 | 16.0000 | 16.0000 | 

Das Konstrukt in `x = qchisq(p - (1 - p), df = 1)` sieht recht esoterisch aus. Wie wir auf den Term `p - (1 - p)` kommen, erläutere ich im nächsten Abschnitt. 

### Signifikanzniveau für gerichtete Hypothesen ausgleichen

Die Funktion `pnorm()`, die wir für den z-Test verwenden, geht von einer gerichteten Konfidenzintervall aus. Der \\( \chi^2 \\)-Test hat per Definition ein ungerichtetes Konfidenzintervall als Grundlage. Damit wir die Annahme  \\( z^2 = \chi \\) aus dem Lehrbuch (Bortz & Schuster, 2010, S. 149) nachvollziehen können, müssen wir diesen Unterschied ausgleichen. 

Weil die quadrierten z-Werte immer positiv sind, folgt daraus, dass die Hypothesen für ein bestimmtes Signifikanzniveau des \\( \chi^2 \\)-Tests sich immer auf eine ungerichtete Hypothese beziehen. Falls wir gerichtete Hypothesen untersuchen, dann müssen wir die Symmetrie der Normalverteilung wieder ausgleichen. Das erreichen wir, indem wir das Signifikanzniveau durch Verdopplung vergrössern. Die Verdopplung leitet sich aus der Symmetrie der Normalverteilung her: Bei ungerichteten Hypothesen liegt das Signifikanzniveau \\( \alpha \\) zu beiden Seiten des Mittelwerts, so dass jeweils \\( \frac{\alpha}{2} \\) zu beiden Seiten des Mittelwerts liegt. Bei einer gerichteten Hypothese müssen wir das Signifikanzniveau so korrigieren, so dass beide Teile von \\( \alpha \\) zusammengefasst sind. Auch bei dieser Korrektur müssen wir die Symmetrie der Normalverteilung mitdenken, denn sonst korrigieren wir nicht genug. Daraus ergibt sich unsere Korrektur aus der folgenden Logik:

$$ 2\frac{\alpha}{2} = \frac{2\alpha}{2} = \alpha $$

Diese Korrektur gilt übrigens auch für Freiheitsgrade > 1. 

### Kontingenztabellen

Damit wir *nominalskalierte Variablen* vergleichen können, brauchen wir ein Hilfsmittel. Weil bei einer Nominalskalierung keine Reihenfolge der Merkmalsausprägungen existiert, brauchen wir wie bei der Rangsummenanalyse ein Hilfsmittel, um Unterschiede erkennen zu können. Bei nominalskalierten Variablen verwenden wir dazu die Häufigkeiten der Merkmalsausprägungen. Diese Häufigkeiten halten wir in *Kontingenztabellen* fest

<div class="alert alert-primary" markdown=1>
**Kontingenztabellen** zeigen die *Verteilung der Merkmalsausprägungen* über mehrere Variablen. 
</div>

Eine Kontingenztabelle ist also nichts anderes als eine Kreuztabelle, die die Häufigkeiten der gemeinsamen Nennungen der Merkmalsausprägungen festhält. In R können wir eine Kontingeztabelle wie folgt bestimmen:

```R
stichprobe %>% 
    count(variable1, variable2) %>% 
    pivot_wider(names_from = variable2, values_from = n, values_fill = 0)
``` 

### Werte für den \\( \chi^2 \\)-Test zusammenfassen

Eine solche Tabelle brauchen wir, damit wir die Voraussetzungen für den \\( \chi^2 \\)-Test bestimmten können. Hier gelten die Voraussetzungen, dass die Häufigkeiten für eine Merkmalsausprägung der untersuchten Variablen nicht zu klein ist. "Zu klein" bedeutet hier kleiner als 5. In solchen Fällen haben wir zwei Möglichkeiten.

1. Wir fassen mehrere selten vorkommende Merkmalsausprägungen zusammen, oder
2. Wir entfernen eine Merkmalsausprägung, die wir nicht sinnvoll mit einer anderen zusammenfassen können. 

Das veranschaulichen wir uns am folgenden Beispiel mit den Daten der [Digitalen Umfeldumfrage von 2021](https://moodle.zhaw.ch/mod/resource/view.php?id=477490):

```R
library(tidyverse)
library(tidymodels)

daten = read_csv("befragung_du2021.csv")

daten %>% 
    count(q09_1, q00_2) %>%
    pivot_wider(names_from = q00_2, values_from = n, values_fill = 0)
```

| q09_1 | M  | W | K  | 
|---|---|---|---| 
| Andere | 1 | 3 | 0 | 
| Android | 93 | 69 | 0 | 
| iPhone | 70 | 84 | 0 | 
| Mobiltelefon | 3 | 2 | 1 | 

Wir sehen an dieser Tabelle schön, dass die Merkmalsausprägungen `Andere` und `Mobiltelefon` der Variable `q09_1` selten vorkommen. Diese beiden Merkmalsausprägungen können wir zusammenfassen. Das erreichen wir mit der Funktion `fct_lump()`. Ausserdem sehen wir, dass die Merkmalsausprägung `K` der Variable `q00_2` nur ein einziges Mal genannt wurde. Weil die anderen beiden Ausprägungen dieser Variable sehr viel häufiger genannt wurden, können wir hier nicht gut zusammenfassen. Also entfernen wir diese Merkmalsausprägung aus unserer Analyse. 

```R
daten %>%  
    mutate(
        q09_1 = fct_lump(q09_1, 2, other_level= "Andere") #Merkmale zusammenfassen.
    ) %>% 
    filter(q00_2 != "K") -> daten_bereinigt

daten_bereinigt %>%
    count(q09_1, q00_2) %>%
    pivot_wider(names_from = q00_2, values_from = n, values_fill = 0) 
```

| q09_1 | M | W  | 
|---|---|---| 
| Android | 93 | 69 |
| iPhone | 70 | 84 | 
| Andere | 4 | 5 |

Diese Kontingenztabelle zeigt uns nun zwei unabhängige Stichproben für die Variable `q09_1`: Jeweils eine für die Merkmalsausprägung `M` und `W`. 

### Nullhypothese des \\( \chi^2 \\)-Tests

Wenn wir Unterschiede in *unabhängigen* Stichproben einer nominalskalierten Variablen untersuchen, dann wenden wir den sog.  \\( \chi^2 \\)-Unabhängigkeitstest an. Der Name des Tests leitet sich aus der Nullhypothese ab: 

<div class="alert alert-primary" markdown=1>
\\( H_0 \\) lautet für den \\( \chi^2 \\)-Unabhängigkeitstest, dass die Werte der einen Variable unabhängig von den Werten der jeweils anderen Variable sind.
</div>

In diesem Fall dürften sich die Zeilen- bzw. Spaltenverteilungen also nicht signifikant von der Gesamtverteilung unterscheiden. Einfacher ausgedrückt: Fall \\( H_0 \\) gilt, dann kann kein statistischer Unterschied zwischen den Stichproben festgestellt werden. 

Ist der p-Wert des \\( \chi^2 \\)-Tests kleiner als für unser Konfidenzintervall notwendig, dann können wir die Nullhypothese verwerfen. 

### Den \\( \chi^2 \\)-Unabhängigskeitstest in R durchführen

Wir können den \\( \chi^2 \\)-Unabhängigkeitstest am einfachsten mit der Funktion `chisq_test()` aus der Bibliothek `tidymodels` bzw. `infer` ausführen. 

Unsere Hypothese ist, dass es die beiden Stichproben (`M` und `W`) einen signifikanten Unterschied bei der Smartphone-Vorliebe haben. Hierbei handelt es sich um eine ungerichtete Hypothese. Wir müssen deshalb unsere p-Werte nicht anpassen.

```R
daten_bereinigt %>% 
    chisq_test(response = q09_1, explanatory = q00_2)
```

| statistic | chisq_df | p_value  | 
|---|---|---|
| 4.693763 | 2 | 0.09566705 | 
 
Unser p-Wert liegt bei 0.0956. Dieser Wert ist grösser als 0.05 im 95%-Konfidenzintervall. Daher können wir die Nullhypothese nicht verwerfen. Wir stellen also fest, dass wir keinen signifikanten Unterschied bei der Smartphone-Vorliebe feststellen konnten. 

$$ $$