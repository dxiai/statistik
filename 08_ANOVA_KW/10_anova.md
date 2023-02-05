# ANOVA

<div class="alert alert-info" markdown=1> 
Dieser Abschnitt ergänzt das Kapitel 12 in Bortz und Schuster (2010). Ergänzend wird vorausgesetzt, dass Sie das Kapitel 13 im [Statistik Skript](https://moodle.zhaw.ch/mod/resource/view.php?id=418629) gelesen haben. 
</div>

<div class="alert alert-primary" markdown=1> 
**Definition**: ANOVA steht für *Analysis of Variance*. Mit einer ANOVA testen wir auf Unterschiede zwischen *mehreren* Stichproben. 
</div>

Obwohl die ANOVA so klingt als ob sie als eine Verallgemeinerung des [F-Test]() interpretiert werden kann, handelt es sich um eine Kombination verschiedener Verfahren. Vom Prinzip verwenden wir die ANOVA wie eine Verallgemeinerung des [t-Tests]() mit mehreren unabhängigen Stichproben. **Genau wie beim t-Test dürfen Sie die ANOVA nur einsetzen, wenn Ihre abhängige Variable metrischskaliert ist.** Die zweite Voraussetzung ist die Normalverteilung der Teilstichproben. Bortz und Schuster (2010) verweisen in Kapitel 12 darauf, dass diese Voraussetzung weniger streng gehandhabt werden kann als beim t-Test.  

Die ANOVA kommt also immer dann zum Einsatz, wenn Ihre erklärende bzw. *unabhängige* Variable mehr als zwei Merkmalsausprägungen hat. Zum Beispiel möchten Sie drei oder vier Gruppen anstatt zwei Gruppen vergleichen, dann können Sie nicht mit dem t-Test arbeiten. 

Die ANOVA ist ein statistisches Standardverfahren und muss methodisch nicht weiter erläutert werden. Sie können davon ausgehen, dass ein statistisch interessiertes Gegenüber weiss, was eine ANOVA ist und wie sie eingesetzt wird. 

## Grundprinzip

Das Grundprinzip der ANOVA basiert auf der Annahme, dass die Teilstichproben wahrscheinlich nicht aus der gleichen Grundgesamtheit stammen können, wenn die Werte der Zielvariable oder ***abhängigen Variable*** in den Teilstichproben weniger stark streuen als über alle Stichproben hinweg. Diese Teilstichproben ergeben sich aus den jeweiligen Merkmalsausprägungen der erklärenden bzw. ***unabhängigen Variable***. 

Diese Grundidee finden Sie im [Statistikskript](https://moodle.zhaw.ch/mod/resource/view.php?id=418629) auf Seite 68 visualisiert.

## Nullhypothese

Die Nullhypothese der ANOVA ergibt sich aus dem Vokabular des F-Tests:

<div class="alert alert-primary" markdown=1>
Die Varianzen der Teilstichproben sind *homogen*.
</div>

Umgangssprachlich können wir auch die gleiche Nullhypothese wie beim t-Test annehmen.

Folgerichtig ist die Alternativhypothese der ANOVA: 

Die Varianzen der Teilstichproben sind *heterogen* und gehören deshalb nicht zur gleichen Grundgesamtheit. 

## Eine ANOVA mit R durchführen

Weil die ANOVA ein statistisches Standardverfahren ist, ist es natürlich in R eingebaut. Wir können eine ANOVA mit der `aov()`-Funktion durchführen. Diese Funktion übernimmt die mathematischen Teilschritte dieser Methode für uns. 

Das Ergebnis dieser Funktion müssen wir noch bereinigen, damit wir alle Werte sehen, die uns interessieren.

Die folgenden Arbeitsschritte sind für eine ANOVA notwendig:

Zuerst müssen wir unsere Bibliotheken laden: 

```R
library(tidyverse)
library(tidymodels)
```



Als erstes untersuchen wir die Unterschiede in der Blütenblattlänge (`Petal.Length`) zwischen den drei Spezies in dieser Stichprobe. Weil die nominalskalierte *unabhängige Variable* `Species` drei Merkmalsausprägungen hat, können wir keinen t-Test durchführen. Das ist ein klassischer Fall für eine ANOVA!

In diesem Fall wird die Variable `Species` als *Faktor* bezeichnet. Weil genau eine solche Variable haben, führen wir die sogenannte ***einfaktorielle*** ANOVA durch. 

- Wir übergeben dazu der Funktion `aov()` die abhängige (oder Ziel-) Variable mit der unabhängigen (oder erklärenden) Variable wie wir es schon bei der Funktion `wilcox.test` getan haben. 
- Wir müssen der Funktion zusätzlich den Parameter `data` mit dem Operator `.` übergeben, damit die Variablen richtig zugeordnet werden können. 
- Abschliessend übergeben wir das Ergebnis der `tidy()`-Funktion, damit die Werte schön dargestellt werden. 

```R
iris %>% 
    aov(Petal.Length ~ Species, data = .) %>% 
    tidy()
```

| term | df | sumsq | meansq  | statistic  | p.value | 
|---|---|---|---|---|---| 
| Species | 2 | 437.1028 | 218.5514000 | 1180.161 | 2.856777e-91 | 
| Residuals | 147 | 27.2226 | 0.1851878 | NA | NA | 

Hier fällt uns auf, dass wir zwei Ergebniszeilen erhalten. Die Zeile mit dem Wert `Species` in der Spalte `term` enthält die Werte, die im Skript als *fester Effekt* bezeichnet werden. Die Zeile mit dem Wert `Residuals` enthält die Werte für den *Versuchsfehler*. 

- `df` enthält die Freiheitsgrade für die korrespondierenden F-Tests. 
- `sumsq` entspricht der Anweichungsquadrate zwischen den Faktor-Stufen (im Skript `SQZ`)
- `meansq` entspricht dem Mittelwert der Abweichungsquadrate (im Skript `MQZ`)
- `statistik` ist der Wert über den der p-Wert bestimmt wird. 
- `p.value` zeigt uns wie gewohnt an, ob die ANOVA auf einen signifikanten Unterschied hinweist. 

Weil der p-Wert < 0.01 ist, können wir von einem signifikanten Unterschied bezüglich der Blütenblattlänge zwischen den Sorten ausgehen.

## Die ANOVA liefert ein signifikantes Ergebnis, was nun?

Wenn eine ANOVA einen signifikanten p-Wert liefert, können wir die Nullhypothese der ANOVA verwerfen. Wir können also sagen, dass die Teilstichproben sich stärker voneinander unterscheiden, als die Werte innerhalb der Teilstichproben. 

Diese Aussage ist aber oft nicht ausreichend, um eine Forschungsfrage zufriedenstellend zu beantworten. Oft interessiert uns nun, welche paarweisen Unterschiede zwischen den Gruppen existieren. Weil der p-Wert der ANOVA bereits signifikant ist, können wir nämlich davon ausgehen, dass es mindestens zwischen zwei Merkmalsausprägungen der erklärenden Variable einen signifikaten Unterschied gibt. Dazu wählen wir die Teilstichproben paarweise aus und führen einen t-Test für diese Teilstichproben durch. Das klingt aufwendig und ist es auch. Zum Glück liefert R die Funktion `pairwise.t.test()` mit, die uns diese mühsame Arbeit abnimmt.

```R
iris %>%
    summarise(
        ptt = pairwise.t.test(Petal.Length, Species) %>% tidy %>% list 
        ) %>% 
    unnest(ptt)
```

| group1 | group2 | p.value  | 
|---|---|---| 
| versicolor | setosa | 1.050917e-68 | 
| virginica | setosa | 1.231842e-90 | 
| virginica | versicolor | 1.810597e-31 |

Wir erhalten extrem signifikante Werte für diese paarweisen Vergleiche. Leider hat diese Vorgehensweise das Problem, dass die paarweisen t-Tests die Signifikanz der Wertepaare überschätzen und deutlich von den realistischen p-Werten abweichen. Anstelle der paarweisen t-Tests können wir auf die sog. Tukey-Differenz oder [*Tukey's honestly significant difference test* (Tukey's HSD)](https://link.springer.com/referenceworkentry/10.1007%2F978-1-4419-9863-7_1212) zurückgreifen. Auch dieser Test ist standardmässig in R vorhanden. 

Die Tukey-Differnz ermittelt auf Basis einer ANOVA realistischere p-Werte für die Unterschiede der paarweisen Merkmalsausprägungen unserer unabhängigen Variablen. 

```R
iris %>% 
    aov(Petal.Length ~ Species, data = .) %>% 
    TukeyHSD() %>%
    tidy()
```

| term | contrast  | null.value | estimate | conf.low | conf.high | adj.p.value | 
|---|---|---|---|---|---|---| 
| Species | versicolor-setosa | 0 | 2.798 | 2.59422 | 3.00178 | 2.997602e-15 | 
| Species | virginica-setosa | 0 | 4.090 | 3.88622 | 4.29378 | 2.997602e-15 | 
| Species | virginica-versicolor | 0 | 1.292 | 1.08822 | 1.49578 | 2.997602e-15 |

Die korrigierten p-Werte finden wir im Vektor `adj.p.value`, was für *adjusted p-value* bzw. *korrigierter p-Wert* steht. Wir erkennen hier dass sich alle drei Spezies in der Stichprobe signifikant voneinander unterscheiden. Das können wir uns mit ggplot auch veranschaulichen. 

```R
iris %>% 
    ggplot(aes(Species, Petal.Length)) +
        geom_boxplot()
```

<img src="https://github.com/dxiai/statistik/raw/main/bilder/anova/anova_plot1.png" width="550"/>

Dieses Diagramm zeigt die unterschiedlichen Verteilungen der Spezies deutlich. In diesem Fall können wir die Unterschiede zwischen den Stichproben also nicht nur statistisch zeigen, sondern auch deutlich in der Visualisierung erkennen. 
