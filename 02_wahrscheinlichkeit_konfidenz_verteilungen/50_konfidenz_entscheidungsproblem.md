# Konfidenz und das Entscheidungsproblem

<div class="col-12 alert alert-primary" markdown=1>
<i class="fa fa-lg fa-info-circle"></i>
Dieser Abschnitt bezieht sich auf das Kapitel 6.2-6.5 in Bortz und Schuster (2010).
</div>

## Das Entscheidungsproblem

Beim Überprüfen von Hypothesen haben wir es immer mit dem gleichen Entscheidungsproblem zu tun: Dürfen wir eine Nullhypothese ablehnen oder nicht?

Unser grosses Problem ist dabei, dass wir bei jeder Messung einen Messfehler berücksichtigen müssen. Unsere Messung kann also eine tatsächliche Merkmalsausprägung gemessen haben oder vom tatsächlichen Wert abweichen. Weil dieser Fehler zufällig erfolgt, können wir nicht wissen, ob wir die tatsächliche Verteilung in einer Stichprobe gemessen haben oder nur einen Spezialfall. 

Beim Testen von Hypothesen wollen wir aber möglichst vermeiden, dass wir unsere Nullhypothese ablehnen, obwohl sie eigentlich gilt (Falsches Negativ), beziehungsweise akzeptieren, obwohl sie nicht gilt (Falsches Positiv). Beim klassischen Hypothesentest konzentrieren wir uns auf den Fall, dass wir vermeiden wollen, dass wir eine Nullhypothese ablehnen, obwohl sie eigentlich gilt. Wir wollen also möglichst sicher sein, dass der tatsächliche Wert einer Verteilung, sich in einem bestimmten Intervall von Merkmalsausprägungen befindet. 

Aus diesen Überlegungen leitet sich die Frage ab, ab wann ist eine Abweichung ausreichend gross, damit wir unsere Nullhypothese ablehnen können. Diese Frage ist nicht ganz einfach zu beantworten und wir werden je nach Fragestellung strengere oder schwächere Kriterien anlegen.

## Konzepte

Um das Entscheidungsproblem zu empirisch zu lösen, brauchen wir zwei Konzepte: Mit dem **Konfidenzintervall** beschreiben wir den Bereich, mit der für unsere Nullhypothese relevante *tatsächliche* Merkmalsausprägung in einem Bereich der *gemessenen* Merkmalsausprägungen wahrscheinlich liegt. Das zweite Konzept ist das ***Signifikanzniveau*** oder auch *Ablehnungsbereich*. Das Signifikanzniveau beschreibt den Bereich ab dem wir unsere Nullhypothese ablehnen würden. 

Dabei berücksichtigen wir, dass wir für unsere Stichprobe Werte gemessen haben, die zufällig vom *tatsächlichen Wert der Grundgesamtheit* abweichen können. Wir wollen uns also möglichst sicher sein, wenn wir unsere Nullhypothese ablehnen. Das Signifikanzniveau ist immer \\( 1 - a_{konfidenz} \\). Bei der Bestimmung des Konfidenzintervalls nutzen wir aus, dass wir die relative Verteilung mit bekannten statistischen Verteilungen vergleichen können. Für die Normalverteilung wissen wir wie viel Prozent der Werte in einem bestimmten Bereich um einen Mittelwert liegen. Wenn wir nun unsere Nullhypothese ablehen, weil die gemessene Abweichung grösser als die Standardabweichung *s* ist, legen wir damit ein Konfidenzintervall von ±*s* fest. Weil wir wissen, dass innerhalb der Standardabweichung etwa 68% der Grundgesamtheit gestreut sind, haben wir eine Wahrscheinlichkeit von 1/3, dass wir unsere Nullhypothese ablehen würden obwohl sie eigentlich gilt. 

Eine Wahrscheinlichkeit von 1/3 ist recht hoch, um auszuschliessen, dass unsere Ergebnisse nicht bloss Zufallstreffer sind. Wir müssen also unser Konfidenzintervall vergrössern. Wir wählen unser Konfidenzintervall, so dass die Wahrscheinlichkeit, dass wir die Nullhypothese fälschlicherweise ablehnen möglichst klein ist. Wenn wir unser Konfidenzintervall so legen, dass der tatsächliche Wert der  Grundgesamtheit mit 90% Wahrscheinlichkeit für unsere Nullhypothese sprechen würde, dann nehmen wir an, dass der tatsächliche Wert entsprechend der Logik der Normalverteilung um unsere gemessenen Merkmalsausprägung gestreut ist. In diesem Fall beträgt die Wahrscheinlichkeit für ein fälschliches Ablehnen nur noch 1 : 10. Legen wir fest, dass wir zu 95% sicher sind, dann beträgt diese Wahrscheinlichkeit schon 1 : 50 und bei 99% 1 : 100. 

<div class="alert alert-success" markdown=1> 
**Merke:** Je grösser unser Konfidenzintervall, desto unwahrscheinlicher ist eine fehlerhafte Ablehnung einer Nullhypothese.
</div>

Neben dem Signifikanzniveau und dem Konfizenzintervall ist der Wahrscheinlichkeit einer Merkmalsausprägung wichtig. Dabei berichten wir nicht die Wahrscheinlichkeit einer bestimmten Ausprägung, sondern immer den Anteil der Merkmalsausprägungen, die höchstens so wahrscheinlich sind wie die berichtete Ausprägung. Dieser Anteil wird als *p-Wert* bezeichnet. In den p-Wert fliesst der prozentuale Anteil der Werte ein, die so wahrscheinlich wie die Merkmalsausprägung oder *noch unwahrscheinlicher* sind.

Wir beurteilen einen p-Wert immer mit Bezug auf ein Signifikanzniveau bzw. ein Konfidenzintervall. Dabei deuten kleine p-Werte auf eine geringe Wahrscheinlichkeit hin, dass die Nullhypothese gültig wäre.

<div class="alert alert-success" markdown=1> 
**Merke:** Liegt der p-Wert innerhalb des Signifikanzniveaus, dann lehnen wir die Nullhypothese ab.
</div>

<img src="https://github.com/dxiai/statistik/raw/main/bilder/norm_vis/percentages_right_confidence.png">

*Abbildung: Visualisierung des Konfidenzintervalls, des Signifikanzniveaus und des p-Werts mit der Normalverteilung*

Zu einem p-Wert gehört immer die Merkmalsausprägung, ab der das Intervall des p-Werts beginnt. In den dargestellten Diagrammen entspricht das dem jeweiligen Wert auf der X-Achse. Dieser Wert wird auch als **z-Wert** bezeichnet.

Wir können den zugehörigen z-Wert für einen p-Wert der Normalverteilung mit Hilfe der `qnorm()`-Funktion bestimmen. Umgekehrt können wir den p-Wert für eine normalverteilte Variable mit der `pnorm()`-Funktion bestimmen. Wir müssen dabei beachten, dass wir für Werte grösser des Mittelwerts, den p-Wert umkehren. 

<p class="alert alert-secondary" markdown="1">
Wir haben p-Werte schon bei der [linearen Regression]() als `Pr()`-Wert sowie beim [Korrelationstest]()  kennengelernt. Die \\( H_0 \\)  der linearen Regression lautet, dass der jeweilige Faktor im linearen Modell **nicht** zutrifft. Beim Korrelationstest lautet die \\( H_0 \\), dass  **keine Korrelation** vorliegt. In beiden Fällen zeigen die p-Werte an, ob sich die Werte verallgemeinern lassen. In diesem beiden Fällen gilt: Je kleiner die p-Werte sind, desto eher lassen sich die Ergebnisse verallgemeinern.
</p>

### Beispiele

1. Bestimmen Sie die z-Werte für die folgenden p-Werte: .25, .63, .75, .90, .98 

```R
tibble(p = c(.25, .63, .75, .90, .98)) %>% 
    mutate(z = qnorm(p))
```

2. Sie haben eine normalverteilte Variable mit dem Mittelwert bei 10 und einer Standardabweichung von 3. Bestimmen Sie die p-Werte für die folgenden Merkmalsausprägungen: 4, 7, 12, 15.

```R
tibble(z = c(4, 7, 12, 15)) %>% 
    mutate(
        p = z %>% pnorm(mean = 10, sd = 3),
        p = ifelse( z > 10, 1 - p, p)
    )
```

### Ungerichtete Hypothesen

Wir sprechen von ungerichteten Hypothesen, wenn wir an der Abweichung von einem Referenzwert interessiert ist. Unsere ***Annahme*** ist also: *Eine Merkmalsausprägung ist ungleich einem Referenzwert.*

<div class="alert alert-success" markdown=1>
**Merke:** Ungerichtete Hypothesen existieren nur für symmetrische Verteilungen, wie der Normalverteilung.
</div>

Dabei interessiert uns nur die Abweichung, nicht die Richtung. Uns interessiert bei diesen Hypothesen, wie wahrscheinlich ein Abstand vom Zentralwert ist. Das ist in aller Regel der Mittelwert einer normalverteilten Variable. In diesem Fall verteilt sich das Signifikanzniveau zu gleichen Teilen auf beiden Seiten des Zentralwerts. Wenn wir zum Beispiel ein Konfidenzintervall von 95% wählen, dann ist unser Siginfikanzniveau 5%. Weil das Signifikanzniveau bei einer ungerichteten Hypothese zu gleichen Teilen auf beiden Seiten des Zentralwerts liegt, finden wir jeweils 2.5% oberhalb und 2.5% unterhalb des Zentralwerts. 

#### Beispiel

Berechnen Sie die Grenzen des 95%-Konfidenzintervall für eine ungerichtete Hypothese. 

```R
Konfidenz = c(qnorm(p = (1 - .95)/2), qnorm(p = .95 + (1 - .95)/2))
Konfidenz
```

* -1.95996398454005
* 1.95996398454005

<div class="alert alert-warning" markdown=1>
Durch die gleichmässige Aufteilung des Signifikanzniveaus auf beide Seiten der Verteilung verschiebt sich die Grenze der p-Werte, ab der wir eine Nullhypothese ablehnen können. 
</div>

<img src="https://github.com/dxiai/statistik/raw/main/bilder/norm_vis/percentages_confidence.png">

In der Praxis werden wir unser Konfidenzintervall variabel wählen und nach unseren gemessenen Werten ausrichten. Wenn der p-Wert bei .04 liegt, dann ist das entsprechende Konfidenzintervall bei 95%. Liegt der p-Wert bei .009, dann ist das Konfidenzintervall bei 99%.

Das folgende Diagramm illustriert die wichtigsten Konfidenzintervalle für ungerichtete Hypothesen.

<img src="https://github.com/dxiai/statistik/raw/main/bilder/norm_vis/percentages_symmetric.png" width="60%">

Bei einer normalverteilten Grundgesamtheit fragen wir uns daher immer, wie viel Prozent einer Merkmalsausprägungen näher am Mittelwert liegen als die gemessene Ausprägung. Diesen Wert können wir mit Hilfe der `pnorm()`-Funktion leicht ermitteln. 

### Wie kommen die Konfidenzintervalle zustande?

In der Literatur wird gelegentlich angegeben, dass die Konfidenzintervall willkürlich festgelegt wurden. Das stimmt nicht ganz. 

Wir haben für die Normalverteilung festgestellt, dass sich die Werte um den Mittelwert häufen und immer weniger werden, je weiter wir uns vom Mittelwert entfernen. Diese Streuung beschreiben wir mit der Standardabweichung. Gleichzeitig gilt, dass die Gesamtwahrscheinlichkeit (d.h. die Fläche unter der Dichtekurve) gleich 1 ist. Jede Teilfläche beschreibt damit den prozentualen Anteil. 

Wir haben gesehen, dass innerhalb der Standardabweichung etwas mehr als 68% aller Werte liegen. Vergrössern wir nun diesen Bereich, dann vergrössern wir auch den Anteil der Werte, die in diesem Bereich liegen. Wie gross müssen wir nun unseren Bereich vergrössern, um auf die typischen Konfidenzintervalle zu kommen? Hier gehen wir nicht willkürlich vor, sondern verwenden ganzzahlige Vielfache der Standardabweichung. 

| Abstand vom Mittelwert | R-code | Prozentualer Anteil | Konfidenzintervall |
| :--- | :--- | :--- | :--- |
| \\(\sigma\\) | `pnorm(1) - pnorm(-1)` | 0.682689492 | 68% |
| \\(2\sigma\\) | `pnorm(2) - pnorm(-2)` | 0.954499736 | 95% |
| \\(3\sigma\\) | `pnorm(3) - pnorm(-3)` | 0.997300203| 99% |
| \\(4\sigma\\) | `pnorm(4) - pnorm(-4)` | 0.999936657 | 99.99% |
| \\(5\sigma\\) | `pnorm(5) - pnorm(-5)` | 0.999999426 | 99.9999% |
| \\(6\sigma\\) | `pnorm(6) - pnorm(-6)` | 0.999999998 | 99.999999% |

Wir erkennen nun, dass die Konfidenzintervalle sich an den ganzzahligen Vielfachen der Standardabweichung orientieren. Die Rundung auf die leicht zu merkende Konfidenzintervalle dient eigentlich nur der leichteren Kommunikation, weil viele Menschen  Bezeichnungen  wie z.B. \\(3\sigma\\) nicht in Prozentwerte übersetzen können.

<p class="alert alert-success" markdown="1">
**Merke:** Die Konfidenzintervalle leiten sich aus der Standardabweichung der Normalverteilung ab.
</p>

Das 90% Konfidenzintervall ist trotzdem speziell, weil es sich nicht auf eine Standardabweichung zurückführen lässt. Dieses Konfidenzintervall wird ausschliesslich wegen der Einfachheit verwendet, weil wir es gewohnt sind uns 10% vorzustellen. Weil das 90% Konfidenzintervall für die statistische Analyse praktisch keine Bedeutung hat, stört uns das nicht. 

<p class="alert alert-danger" markdown="1">
**Wichtig:** Wir dürfen die Konfidenzintervalle nicht beliebig wählen! R zeigt uns zwar die Signifikanz auf verschiedenen Niveaus an. Das für uns relevante Niveau leitet sich aber aus der Genauigkeit des jeweiligen Messinstruments ab. 
</p>

Bei selbst erstellten Skalierungen sollten Sie kein Konfidenzintervall grösser als 95% wählen. Bei soziometrischen Studien, also wenn die Daten durch Menschen erzeugt werden, dann sind Konfidenzintervalle > 99% praktisch nicht möglich. Grössere Konfidenzintervalle ergeben sich nur bei sehr genauen und normierten Messverfahren. Diese Verfahren werden *unverändert* aus der entsprechenden Literatur übernommen, die das jeweilige Konfidenzintervall bestätig hat. Diese Literatur gibt meistens an, wie die Ergebnisse zu interpretieren sind. 

## Gerichtete Hypothesen

Bei gerichteten Hypothesen erwarten wir eine Abweichung der Merkmalsausprägungen in eine bestimmte Richtung im Vergleich zu einem Referenzwert. Bei gerichteten Hypothesen liegt das Signifikanzniveau nur auf einer Seite. 

<p class="alert alert-warning"> Weil das Signifikanzniveau im Verhältnis zur ungerichteten Hypothese für die  jeweilige Richtung grösser ist, müssen wir immer sicherstellen, ob wir  eine gerichtete oder eine ungerichtete Hypothese untersuchen, weil der gleiche p-Wert ausreicht, um die Hypothese im ungerichteten Fall zu verwerfen und im gerichteten Fall dürfen wir die Nullhypothese bei gleichem Signifikanzniveau nicht verwerfen!</p>

<p class="alert alert-success"> 
Um ganz sicher zu sein, dass bei einer gerichteten Hypothese signifikante Abweichungen vorliegen, prüfen wir zuerst die schwächere <i>ungerichtete</i>  Hypothese und anschliessend die gerichtete Hypothese. Damit fangen wir ab, dass unsere Fragestellung nicht den gegenteiligen Effekt hat, als wir erwartet hätten.
</p>

### Rechtsseitige Hypothese

***Annahme***: Merkmalsausprägung ist grösser als Wert `X`. 

Beispiel für eine rechtsseitige Hypothese ist: *Ein neues Verfahren steigert die Produktivität im Vergleich zum herkömmlichen Verfahren.*

In diesem Fall nehmen wir an, dass mit neuen Verfahren die Anzahl der produzierten Einheiten **grösser** ist als beim herkömmlichen Verfahren. 

Das Konfidenzintervall erstreckt sich also von -unendlich bis z-Wert des Konfidenzintervalls.  Das Signifikanzniveau liegt in diesem Fall auf der rechten Seite der Verteilung.

```R
Konfidenz = c(-Inf, qnorm(p = .95))
Konfidenz
```

* `-Inf`
* `1.64485362695147`

<img src="https://github.com/dxiai/statistik/raw/main/bilder/norm_vis/percentages_right.png">

### Linksseitige Hypothese

***Annahme***: Merkmalsausprägung ist kleiner als Wert `X`.

Beispiel für eine linksseitige Hypothese ist: *Ein neues Verfahren beschleunigt die Bearbeitungszeit im Vergleich zum herkömmlichen Verfahren.*

In diesem Fall nehmen wir an, dass die Bearbeitungszeit beim neuen Verfahren **kleiner** sein wird als beim herkömmlichen Verfahren. 

Das Konfidenzintervall erstreckt sich also vom z-Wert für *1-a* bis *unendlich*. Das Signifikanzniveau liegt in diesem Fall auf der linken Seite der Verteilung.

```R
Konfidenz = c(qnorm(p = 1 - .95), Inf)
Konfidenz
```

* `-1.64485362695147`
* `Inf`

<img src="https://github.com/dxiai/statistik/raw/main/bilder/norm_vis/percentages_left.png">

$$ $$
