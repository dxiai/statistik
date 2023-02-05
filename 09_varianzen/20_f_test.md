# Der F-Test

<div class="alert alert-info" markdown=1>
Dieser Abschnitt ergänzt das Kapitel 8.6.1 in Bortz und Schuster (2010). 
</div>

Wenn wir Varianzen untersuchen, dann fragen wir uns, ob die von uns beobachteten Varianzunterschiede signifikant sind. Diese Frage unterstützt unsere Entscheidung, ob wir von  heterogenen oder homogenen Stichproben sprechen können.

Zur Untersuchung von Varianzunterschieden verwenden wir den sog. F-Test. Dieser Test prüft, ob die Varianzen zweier Stichproben sich unterscheiden.

<div class="alert alert-primary" markdown=1>
**F-Test Nullhypothese**: Es gibt keine signifikanten Varianzunterschiede. 

Das ist gleichbedeutend mit: Die untersuchten Stichproben sind *homogen*.
</div>

Die Anwendung des F-Tests ist auf metrischskalierte Variablen beschränkt. Allerdings bildet der F-Test auch die Grundlage für nicht-parametrische Tests für ordinale Skalenniveaus, wie z.B. den Wilcoxon-Test und den Mann U-Test. 

Der F-Test verwendet die sog. F-Verteilung zur Bestimmung der p-Werte. Weil diese Verteilung auf der Varianz basiert und die Varianz als das Quadrat der Standardabweichung definiert ist
 ist diese Verteilung nur für Werte > 0 definiert. Der Verlauf der F-Verteilung ähnelt der \\( \chi^2 \\)-Verteilung. Entsprechend müssen wir bei gerichteten Hypothesen auch das Signifikanzniveau vergrössern bzw. um genau zu sein, müssen wir es doppelt so gross wählen. Die dahinter liegende Logik ist im Abschnitt zum \\( \chi^2 \\)-Test erklärt. 

In R wird der F-Test über die Funktion `var.test()` ausgeführt. 

### Beispiel

```r
library(tidyverse)
library(tidymodels)

read_csv("befragung.csv") -> daten
```

Mit diesen Befragungsdaten können wir eine Varianzanalyse durchführen.

```r
daten %>% 
    var.test(q01 ~ kurs, data = .) %>% 
    tidy()
```

| estimate  | num.df | den.df  | statistic  | p.value  | conf.low | conf.high  | method  | alternative  | 
|---|---|---|---|---|---|---|---|---| 
| 1.319318 | 125 | 199 | 1.319318 | 0.08149172 | 0.9659746 | 1.823042 | F test to compare two variances | two.sided | 

<div class="alert alert-success" markdown=1>
In unseren Dokumenten berichten wir dieses Ergebnis wie folgt: \\( F(125, 199) = 1.319318; p = 0.08149172 \\)
</div>

Wir können den p-Wert mit der Funktion `pf()` kontrollieren: 

```r
1 - pf(1.319318, 125, 199)
```

```
0.0407491591432709
```

Das Ergebnis ist nun ungleich dem p-Wert. Weil die F-Verteilung ungerichtet ist, müssen wir bei der manuellen Berechnung eine p-Wert-Anpassung vornehmen und den p-Wert verdoppeln. Diese Anpassung erfolgt analog zu den Überlegungen bei der \\( \chi^2 \\)-Verteilung. Der korrigierte p-Wert ist deshalb:

```
0.0814983182865419
```

<div class="alert alert-warning" markdown=1>
Die beiden p-Werte weichen leicht voneinander ab. Diese Abweichung entsteht durch die Rundung der Werts für die Teststatistik. Weil die Testfunktionen von R immer mit ungerundeten Werten arbeiten, sollten Sie **immer** diese p-Werte übernehmen!
</div> 

Solche Anpassungen nimmt uns die Funktion `var.test()` ab, wenn wir die Richtung unseres Hypothesentests mit angeben. 

## p-Wert des F-Test interpretieren

Im oben gezeigten Beispiel ist der p-Wert `0.0815`. Weil `0.0815` > `0.05` ist, können wir die ungerichtete Nullhypothese nicht verwerfen. Wir nehmen also an, dass unsere beiden Teilstichproben, die durch die Variable `kurs` gebildet werden, **homogene Varianzen** haben. Der p-Wert von 0.0815 deutet aber auch an, dass wir eine signifikantes Ergebnis für eine gerichtete Hypothese erhalten würden. Das erkennen wir daran, dass 0.0815 < 0.1 ist. 

```
daten %>% 
    var.test(q01 ~ kurs, data = ., alternative = "greater") %>%
    tidy()
```

| estimate  | num.df | den.df  | statistic | p.value  | conf.low  | conf.high  | method  | alternative | 
|---|---|---|---|---|---|---|---|---| 
| 1.319318 | 125 | 199 | 1.319318 | 0.04074586 | 1.015588 | Inf | F test to compare two variances | greater |

Die Andeutung bestätigt sich also: Die Varianz in einem der Kurse ist also signifikant grösser als im anderen. Diese Aussage können wir im 95%-Konfidenzintervall machen, weil der p-Wert mit 0.0408 < 0.05 ist. 

## Anwendung des F-Tests

Wir verwenden den F-Test zur Beantwortung von Forschungsfragen, die sich auf die Homogenität oder Heterogenität von Stichproben beziehen. So können wir uns Fragen, ob ein neues Heizungssystem für eine gleichmässigere Raumtemperatur sorgt. Hier erwarten wir für das alte und das neue System die gleichen Mittelwerte (z.B. 21ºC). Wenn das neue System für gleichmässigere Temperaturen sorgt, dann müsste die Varianz der gemessenen Temperaturen homogener sein als für das alte System. Ist dieser Unterschied signifikant, dann können wir diesen jetzt mit einem F-Test feststellen. 

$$ $$
