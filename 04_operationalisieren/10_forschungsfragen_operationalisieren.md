# Forschungsfragen operationalisieren

Um eine Forschungsfrage empirisch zu beantworten, benötigen wir Daten mit einem sinnvollen Zusammenhang zur Fragestellung. Eine der schwierigsten Schritte bei dieser Aufgabe ist die Entscheidung, *welche* Daten erhoben werden sollen. Weil diese Daten Merkmale des Forschungsgegenstands widerspiegeln, ist dieses Problem identisch mit der folgenden Problemstellung. 

<p class="alert alert-info" markdown="1">
**Problem**: Welche Merkmale bzw. *Variablen* des Forschungsgegenstands  beeinflussen die Antwort auf die Fragestellung?
</p>

Die Antwort auf diese Frage beschreibt die Daten, die zur Beantwortung der Forschungsfrage erforderlich sind. Die Beantwortung dieser Frage ist die Aufgabe der **Operationalisierung** der Forschungsfrage. 

Diese Aufgabe lässt sich in zwei Teilprobleme gliedern: 

1. Die Auswahl der Variablen 
2. Die Bestimmung der Zusammenhänge zwischen den Variablen

Weil in dieser Phase des Forschungsprozesses noch keine Daten vorliegen, sind die Ergebnisse dieses Arbeitsschritts immer **Vermutungen** bzw. **Annahmen**. 

<div class="alert alert-secondary" markdown="1">
**Lernziele**

* Variablen einer Forschungsfrage zuordnen
* Zusammenhänge zwischen den Variablen festlegen
* Beobachtbare und nicht beobachtbare Variablen identifizieren
* Variablen und Zusammenhänge in einem DAG organisieren und darstellen
</div>

Als Ausgangslage nehmen wir die folgende Forschungsfrage: 

> Gibt es einen Zusammenhang zwischen der selbst-wahrgenommenen Digitalisierung von Studienanfängern und ihrer digitalen Mediennutzung?

Die Frage bezieht sich auf Studienanfänger, d.h. Studierende im 1. Semester. Die Frage hat zwei offensichtliche Variablen: 

1. Die selbst-wahrgenommene Digitalisierung sowie
2. Die digitale Mediennutzung

Nennen wir diese Variablen `D` für Digitalisierung und `M` für Mediennutzung. Aus der Fragestellung leiten wir ab, dass es eine Verbindung zwischen diesen beiden Variablen gibt. Diese Verbindnung ist ersteinmal *ungerichtet*. Formal können wir diese Beziehung wie folgt aufschreiben. 

$$
D \leftrightarrow M
$$

Wir können aber einen Schritt weiter gehen und uns fragen, welche der beiden Variablen diesen angenommenen Zusammenhang *verursacht*. Hier haben wir zwei Optionen: 

* Die wahrgenommene Digitalisierung beeinflusst die digitale Mediennutzung.
* Die digitale Mediennutzung beeinflusst die wahrgenommene Digitalisierung.

Wenn wir uns zwischen diesen beiden Optionen entscheiden müssen, dann ist die zweite Option *plausibler*, weil aus der Medien*nutzung* ein Bezug zu den digitalen Technologien entsteht, der die wahrgenommene Digitalisierung beeinflusst. Umgekehrt ist eine Digitalisierung *ohne* Nutzung digitaler Medien nicht denkbar.

Wir können den Zusammenhang zwischen den Variablen `D` und `M` auch gerrichtet betrachten: 

$$
D \leftarrow M
$$

Diese Beziehung lesen wir als: *Die Variable D folgt der Variablen M*.

In `R` sind die Pfeile bereits für die Zuweisungsoperation belegt. Der gerichtete Zusammenhang zwischen `D` und `M` wird mit Hilfe des Tilde-Operators ausgedrückt. 

```R
D ~ M
```

Diesen Term liesst man ebenfalls als: *Die Variable D folgt der Variablen M*.

<p class="alert alert-primary" markdown="1">
**Definition**: Eine Variable wird als **abhängige Variable** bezeichnet, wenn sie einer anderen Variablen folgt. 
</p>

<p class="alert alert-primary" markdown="1">
**Definition**: Eine Variable wird als **unabhängige Variable** bezeichnet, wenn ihr andere Variablen folgen. 
</p>

Der Term `D ~ M` ist *keine* R-Operation, die berechnet werden kann, sondern ist ein **Modell** für eine Vermutung.

<p class="alert alert-primary" markdown="1">
**Definition**: Ein **Modell** beschreibt vermutete Beziehungen zwischen Variablen.
</p>

R unterstützt uns bei der Operationalisierung mit Hilfe von Modellen durch die Bibliotheken `dagitty` und `ggdag`.

```R
library(tidyverse)
library(dagitty)
library(ggdag)
```

Jetzt können wir das Modell für unsere Studie operationalisieren. 

```R
Modell = dagify(D ~ M)

Modell
```

```
dag {
D
M
M -> D
}
```

Das Modell zeigt nun alle Variablen und die vermuteten Beziehungen zwischen den Variablen. Schöner sieht es aber mit als Plot aus. 

```R 
Modell %>% 
    ggdag() +
        theme_dag()
```

![Operationalisierte Variablen](https://github.com/dxiai/statistik/raw/main/bilder/var_op_1.png)

Wir haben jetzt die selbst-wahrgenommene Digitalisierung als *abhängige Variable* und die digitale Mediennutzung als *abhängige Variable* festgelegt. Damit können wir die Gestaltung einer empirischen Studie beginnen. 

Die selbst-wahrgenommene Digitalisierung können wir dirket messen, indem wir Studienanfänger fragen, wie stark digitalisiert sie sich selbst wahrnehmen. 

Die digitale Mediennutzung könnten wir analog erfragen: Wie stark werden digitale Medien genutzt? Digitale Mediennutzung ist aber recht kompliziert und umnfasst verschiedene Technologien, Medienformate und Interaktionsformen. Zusätzlich gehören zur digitalen Mediennutzung auf die Gestaltung und das Konsumieren von Medienformaten. Wenn wir die digitale Mediennutzung auf eine Frage reduzieren, dann hängen die Daten von der individuellen Interpretation dieser Facetten durch die Studienteilnehmenden ab.

<p class="alert alert-warning" markdown="1">
Immer wenn Interpretationsspielräume den Studienteilnehmenden geboten werden, wird die empirische Auswertung und Interpretation der erhobenen Daten erschwert. 
</p>

Um die verschiedenen Facetten der digitalen Mediennutzung zu berücksichtigen, erweitern wir unser Modell. Dabei verwenden wir `T` für Technologien, `F` für Medienformate und `I` für Interaktionsformen. Wir berücksichtigen ausserdem, dass die Gestaltung und das Konsumieren für Medienformate und Interaktionsformen gleichermassen separat vorliegen können. Wir bezeichnen daher mit `G_I` die Gestaltung von Interaktionsformen und mit `G_F` die Gestaltung von Medienformaten.

```R
Modell = dagify(
    D ~ M, 
    M ~ T + F + I + G_F + G_I,
    G_F ~ F,
    G_I ~ I
)

ggdag(Modell) + 
    theme_dag()
```

![Komplexe Operationalisierung](https://github.com/dxiai/statistik/raw/main/bilder/var_op_complex_a.png)

Damit haben wir bereits recht viele Variablen, die unseren ursprünglichen Zusammenhang beeinflussen. In unserem Fall sind diese zusätzlichen Variablen *Indikatoren* für die Mediennutzung. Oft werden Indikatoren verwendet, weil die eigentliche Variable nicht direkt beobachtet werden kann. In diesem Fall verändert sich auch unser Modell.


```R
Modell = dagify(
    D ~ T + F + I + G_F + G_I,
    G_F ~ F,
    G_I ~ I
)

ggdag(Modell) + 
    theme_dag()
```

![Komplexe Operationalisierung](https://github.com/dxiai/statistik/raw/main/bilder/var_op_complex_b.png)

<p class="alert alert-success" markdown="1">
Die Operationalisierung ist abgeschlossen, wenn wir Variablen mit messbaren Merkmalsausprägungen auf einem gewünschten Skalenniveau herausgearbeitet haben. 
</p>