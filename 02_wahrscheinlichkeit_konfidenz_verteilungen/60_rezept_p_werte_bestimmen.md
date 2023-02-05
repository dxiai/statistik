# p-Werte mit R bestimmen

<div class="col-12 alert alert-info" markdown=1>
In vielen Lehrbüchern liegen Tabellen für die Wahrscheinlichkeit, den p-Wert und den z-Wert bei unterschiedlichen Freiheitsgraden bei. Es ist zwar immer gut zu wissen, wie man die Ergebnisse komplett analog ermitteln kann. In der Praxis werden wir jedoch das Nachschlagen unserem Computer überlassen. 

In diesen Cheat Sheet werden nur die R-Funktionen erklärt.
</div>

R bietet uns Funktionen für die Arbeit mit den drei wichtigsten Verteilungen (in dieser Lehrveranstaltung). 

- Die Wahrscheinlichkeitsdichte (engl. *density*)
- Den Wahrscheinlichkeitsverteilung (engl. *probability*)
- Den Merkmalsausprägungen oder Quantilen (engl. *quantile*)

Zusätzlich bietet uns R Funktionen um uns zufällige Verteilungen mit bestimmten Merkmalsausprägungen zu erzeugen. Solche Zufallsverteilungen (engl. ***random** distribution*) helfen uns bei der *Simulation* von Auswertungen und Ergebnissen. 

Entsprechend stehen uns für die Normalverteilung, die t-Verteilung, die \\( \chi^2 \\)-Verteilung und die F-Verteilung vier Funktionen zur Verfügung: 

- Die *Density*-Funktion (mit `d` gekennzeichnet)
- Die *Probability*-Funktion (mit `p` gekennzeichnet; liefert *p-Werte*)
- Die *Quantile*-Funktion (mit `q` gekennzeichnet; liefert *z-Werte*)
- Die *Random* -Funktion (mit `r` gekennzeichnet)

Die Kennzeichnungen werden den Kürzeln der gewünschten Verteilung vorangestellt. 

- Die Normalverteilung wird mit `norm` abgekürzt.
- Die t-Verteilung wird mit `t` abgekürzt.
- Die \\( \chi^2 \\)-Verteilung wird mit `chisq` abgekürzt.
- Die F-Verteilung wird mit `f` abgekürzt.

| Ziel |  Normalverteilung | t-Verteilung | \\( \chi^2 \\)-Verteilung | F-Verteilung |
| :---: | :---: | :---: | :---: | :---: | 
| Wahrscheinlichkeit | `dnorm` | `dt` |  `dchisq` |  `df` | 
| p-Werte | `pnorm` | `pt` |  `pchisq` |  `pf` | 
| Quantile/z-Werte | `qnorm` | `qt` |  `qchisq` |  `qf` | 
| Zufallsstichprobe | `rnorm` | `rt` |  `rchisq` |  `rf` | 

Der erste Parameter dieser Funktionen ist der Wert, den wir untersuchen. 

Den Funktionen für die t-Verteilung, die \\( \chi^2 \\)-Verteilung und die F-Verteilung müssen zusätzlich die zugehörigen Freiheitsgrade (*degrees of freedom* oder `df`) übergeben werden. Beachten Sie, dass Die F-Verteilung für die Freiheitsgrade von zwei Variablen definiert ist. 

Alle Funktionen arbeiten die Funktionen von kleinen nach grossen Werten ab. Die Ergebnisse entsprechen also **immer** *Rechtsseitigen* Hypothesentests.

<div class="alert alert-warning" markdown=1>
Für linksseitige oder ungerichtete Hypothesentests müssen Sie die Werte durch Spiegelung  oder Aufteilung umformen.
</div>

<div class="alert alert-success" markdown=1>
Die Funktionen für die Normalverteilung und die t-Verteilung können die Verteilung automatisch auf den konkreten Mittelwert und die Standardabweichung Ihrer Stichprobe transponieren. 
</div>

#### Beispiel

Das folgende Beispiel stellt die Dichte der Normalverteilung der Dichte der t-Verteilung mit 5, 10 und 20 Freiheitsgraden gegenüber.

```
tibble(x = seq(-3, 3, 1)) %>% 
    mutate(
        normal = dnorm(x),
        t05    = dt(x, df = 5),
        t10    = dt(x, df = 10),
        t20    = dt(x, df = 20)
    )
```
<table>
<thead>
	<tr><th scope=col>x</th><th scope=col>normal</th><th scope=col>t05</th><th scope=col>t10</th><th scope=col>t20</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>-3</td><td>0.004431848</td><td>0.01729258</td><td>0.01140055</td><td>0.007963787</td></tr>
	<tr><td>-2</td><td>0.053990967</td><td>0.06509031</td><td>0.06114577</td><td>0.058087215</td></tr>
	<tr><td>-1</td><td>0.241970725</td><td>0.21967980</td><td>0.23036199</td><td>0.236045649</td></tr>
	<tr><td> 0</td><td>0.398942280</td><td>0.37960669</td><td>0.38910838</td><td>0.393988586</td></tr>
	<tr><td> 1</td><td>0.241970725</td><td>0.21967980</td><td>0.23036199</td><td>0.236045649</td></tr>
	<tr><td> 2</td><td>0.053990967</td><td>0.06509031</td><td>0.06114577</td><td>0.058087215</td></tr>
	<tr><td> 3</td><td>0.004431848</td><td>0.01729258</td><td>0.01140055</td><td>0.007963787</td></tr>
</tbody>
</table>

$$ $$
