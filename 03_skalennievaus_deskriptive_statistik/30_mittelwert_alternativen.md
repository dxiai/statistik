# Alternativen zum arithmetischen Mittel

<div class="alert alert-warning" markdown=1>
In diesem Abschnitt wird die [Beispielstichprobe](https://moodle.zhaw.ch/mod/resource/view.php?id=418571) verwendet! Für die Code-Beispiele müssen Sie die Datei zuerst laden.</div>

Sie müssen vorher das folgende Code-Fragment in Ihrem Notebook ausführen.

```R
library(tidyverse)

stichprobe = read_csv("stichprobe.csv")
```

### Geometrischer Mittelwert

Das *arithmetische Mittel* ist zwar das gebräuchlichste aber nicht immer das zuverlässigste Lagemass für die zentrale Tendenz. Neben dem arithmetischen Mittel gibt es noch den *geometrischen Mittelwert* und den *harmonischen Mittelwert*. 

Das **geometrische Mittel** wird eingesetzt, um Verhältnisse zwischen Werten werden zu beschreiben, die sich nicht durch eine Summe ausdrücken lassen. Das geometrische Mittel basiert auf dem Produkt der gemessenen Werte und wird durch die folgende Formel berechnet. 

$$ \sqrt[^n]{\prod_{i = 1}^{n}{n_i} } = \left( \prod_{i = 1}^{n}{n_i} \right)^\frac{1}{n} $$

Diese Formel können wir in R wie folgt schreiben: 

```R
prod(vektorname)^(1/n())
```

Eine typische Anwendung ist das Verfolgen von Änderungen, wie Sie bei der Wachstumsprognose verwendet wird. In diesem Fall liefert das arithmetische Mittel falsche Ergebnisse, weil sich die Folgewerte auf die Vorgängerwerte beziehen. 

Das folgende Beispiel stellt das arithmetische und das geometrische Mittel für die Variable `q10_1_0` gegenüber. 

```R
stichprobe %>%
    summarise(
        mw = mean(q10_1_0),
        gm = prod(q10_1_0)^(1/n())
    )
```
<table border="1">
<thead>
	<tr><th scope=col>mw</th><th scope=col>gm</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>7.259259</td><td>6.849801</td></tr>
</tbody>
</table>
<br>

### Harmonischer Mittelwert

Der **harmonische Mittelwert** beschreibt Daten, deren Werte *zusammengesetzte Masseinheiten* darstellen. Das können Durchschnittsgeschwindigkeiten sein oder auch Durchschnittspreise für Produkte in verschiedenen Mengen und Preisen. Wir merken uns daher, dass wir der Mittelwert für *zusammengesetzte Werte* das *harmonische Mittel* ist. 

Der harmonische Mittelwert berechnet sich aus der folgenden Formel: 

$$ \frac{ \sum^{k}\_{i=1}{ w_i }  }{ \sum^{k}\_{i=1}{ \frac{w_i}{x_i} } } $$ 

Wobei *w* für den Anteil eines Messwerts am Gesamtergebnis steht. Das wird auch als Gewichtung (eng. *weight*) bezeichnet. Für uns typische Stichproben haben ist die Gewichtung eines Werts am Gesamtergebnis 1/n. Damit lässt sich der Zähler der Formel vereinfachen, weil die Summe dieser gleichen Anteile immer \\( n \frac{1}{n} = \frac{n}{n} = 1 \\) entspricht. Das ergibt:

$$ \frac{1}{\sum^{k}_{i=1}{\frac{w_i}{x_i}}} $$ 


**Beispiel:** Wir fahrem mit dem Auto vier Mal die gleiche Strecke. Dabei fährt es jeweils die folgenden Geschwindigkeiten: 25km/h, 40km/h, 100km/h, 80km/h.  Wir fragen uns nach der Durchschnittsgeschwindigkeit. 

Da wir immer die gleiche Strecke fahren, ist der Anteil der einzelnen Fahrten an der Gesamtstrecke immer gleich, nämlich 1/4. Unser *w* ist also immer 1/4. Weil dieser Wert konstant ist, können wir diesen Wert ausklammern, so dass sich die folgende Formel ergibt: 

$$ \frac{4}{\sum^{k}_{i=1}{\frac{1}{x_i}}} $$ 

Berechnen wir den Rest in R, wobei wir beachten, dass die 4 die Anzahl unserer Fahrten ist. Als Vergleich stelle ich das arithmetische Mittel gegenüber. 


```R
tibble(kmh = c(25, 40, 100, 80)) %>%
    summarise(
        mw = mean(kmh),
        geomw = n()/sum(1/kmh)
    )
```


<table border="1">
<thead>
	<tr><th scope=col>mw</th><th scope=col>geomw</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>61.25</td><td>45.71429</td></tr>
</tbody>
</table>

<div class="col-md-12 text-center h4">
    <a href="https://moodle.zhaw.ch/course/view.php?id=7569&section=4"><i class="fa fa-lg fa-arrow-up"></i></a>
</div>