## Normalverteilung

Die Normalverteilung ist eine besondere Verteilung für die Statistik. Sie ist die Grundlage für die Lagemasse des arithmetischen Mittelwerts und der Standardabweichung. Diese beiden Werte haben aber nur eine Aussagekraft, wenn die Ausgangsdaten *hinreichend* normalverteilt sind. 

Veranschaulichen wir uns kurz die Normalverteilung. Die Normalverteilung ist eine Funktion mit den folgenden Eigenschaften: 

1. Der Mittelwert ist 0. 
1. Das Maximum der Normalverteilung ist der Mittelwert.
2. Die Funktion ist symmetrisch.
2. Die Funktion ist stetig.
2. Die Normalverteilung nimmt asymptotisch gegen 0 ab. 
3. Die Standardabweichung beträgt 1.
3. Die Wendepunkte der Normalverteilung liegen jeweils an 1 und -1.
4. Die Fläche unter der Verteilungskurve (der sog. *Dichtefunktion*) beträgt 1.

Aus diesen Eigenschaften ergibt sich automatisch, dass die Fläche unter der Verteilungskurve 50% für alle Werte > 0 und 50% für alle Werte < 0 ist.

![Normalverteilung und Teilflächen](https://github.com/dxiai/ct-resourcen/raw/master/statistik/bilder/normalverteilung.png)


Für die Statistik ist die Normalverteilung interessant, weil die *Binomialverteilung* in die Normalverteilung übergeht, wenn die Anzahl der Messungen gegen unendlich geht. Ausserdem ist die Eigenschaft, dass die Fläche unter der Dichtefunktion genau 1 beträgt, sehr praktisch, weil wir so leicht in Prozentwerte umrechnen können.

Dadurch dass die Normalverteilung *normiert* ist, können wir sie durch *Linearverschiebung* an einen beliebigen anderen Mittelwert und eine andere Standardabweichung verschieben. Dabei müssen wir die Kurve nur durch Addition zum neuen Mittelwert verschieben und durch Multiplikation mit der gewünschten Standardabweichung strecken. Diese Verschiebung funktioniert natürlich auch in die andere Richtung indem wir die Operationen umkehren. 

Der jeweilige Höhe der Dichtefunktion entspricht der Wahrscheinlichkeit, dass eine bestimmte Merkmalsausprägung vorkommt. Die Einzelwahrscheinlichkeiten sind recht uninteressant, weil sie relativ klein sind. Da wir aber jetzt wissen, dass die Fläche unter der Dichtefunktion genau 1 ist, steht diese 1 für die Grundgesamtheit. Wir können also für jeden Wert unserer Merkmalsausprägung fragen: Wie viel Prozent der Grundgesamtheit habe eine grössere Wahrscheinlichkeit bzw. liegen näher am Mittelwert? Die Antwort ist "leicht", denn dieser Wert entspricht der Fläche unter der Dichtefunktion für alle Werte, die *näher* am Mittelwert liegen.

Wenn wir eine normalverteile Variable haben, können wir uns also die Frage beantworten, wieviel Prozent unserer Werte liegen in einem Abstand um den Mittelwert. Unser Hauptproblem ist aber, dass wir nur eine *endliche* Anzahl von Messungen haben. Dadurch unterliegt unsere tatsächliche Messung einer zufälligen Abweichung von der tatsächlichen Verteilung. Diese Abweichungen können immer grösser werden, je kleiner unsere Stichprobe ist. Wir fragen uns deshalb: ***Ab wann können wir unsere gemessene Verteilung noch als Normalverteilung akzeptieren?***. 

Wenn wir annehmen, dass wir eine Zufallsstichprobe haben, dann entsprechen unsere gemessenen Merkmalsausprägungen einer zufälligen Stichprobe für die Merkmalsausprägung. Wenn wir mehrere Messungen mit anderen Stichproben durchführen würden wir eine etwas andere Abweichung für die Merkmalsausprägung erhalten. Weil wir ja wissen, dass der Mittelwert genau auf der Kurve der Normalverteilung liegt, können wir die Abweichung bestimmen. Diese Überlegung wiederholen wir für jede Merkmalsausprägung bzw. Intervall von Ausprägungen für eine Variable. In Summe sind all diese Abweichungen selbst wieder normalverteilt, weil sie der Binomialverteilung unterliegen. 

Wir können uns jetzt fragen: Sind die gemessenen Merkmalsausprägungen einer Variable für eine normalverteilte Grundgesamtheit realistisch? 

Für diese Frage können wir die folgende Hypothese formulieren: **Unsere Variable ist normalverteilt.** 

Die Gegenhypothese dazu wäre: **Unsere Variable ist nicht normalverteilt**. 

Wir legen ein sog. *Konfidenzinterval* von 95% fest. Das bedeutet, dass wir unsere Hypothese dann akzeptieren können, wenn unsere Abweichungen weniger als 95% wahrscheinlichere Abweichungen zur Normalverteilung haben. Wir akzeptieren die Gegenhypothese, wenn mehr als 95% wahrscheinlichere Abweichungen existieren. Wir können die Prozentwerte auch umkehren und festlegen: dass mindestens 5% aller Abweichungen unwahrscheinlicher als unsere vorliegenden Abweichungen sein dürfen, damit wir unsere Hypothese akzeptieren können.

![Normalverteilung Konfidenzintervall Beispiel](https://github.com/dxiai/ct-resourcen/raw/master/statistik/bilder/normalverteilung_percent.png)
*Abbildung: Prozent wahrscheinlicherer (blau) und unwahrscheinlicher (weiss) Abweichungen* 


<p class="alert alert-info">Das Konzept des Konfidenzintervalls besprechen wir im folgenden Abschnitt <i>Konfidenz</i>. Für das Erste akzeptieren wir das Konzept.</p>

Diese Überlegungen müssen wir für **jede** Variable durchführen, bevor wir den Mittelwert und die Standardabweichung angeben dürfen. Zum Glück haben sich schlaue Köpfe dazu bereits Gedanken gemacht. Von den verschiedenen Verfahren, die unsere Fragestellung beantworten hat sich der ***Shapiro-Wilk-Text auf Normalität*** als Standard durchgesetzt. Dieser Test liefert uns einen sog. `p-Wert`, der uns mitteilt, wieviel Prozent der Abweichungen von der Normalverteilung noch unwahrscheinlicher sind als unsere gemessenen Daten. Der Shapiro-Wilk-Test hat als Nullhypothese die Annahme, dass eine Variable normalverteilt ist.

In R rufen wir diesen Test mit der Funktion `shapiro.test()` auf. Die Funktion gibt uns mehrere Werte zurück, wobei wir uns nur für den p-Wert (`p.value`) interessieren. Dieser Wert ist Teil unserer *deskriptiven Statistik*, den wir angeben müssen, wenn wir den Mittelwert und die Standardabweichung anführen. Wir können den Shapiro-Wilk-Test in R wie folgt aufrufen, um an unser gewünschtes Ergebnis zu kommen.

```R

beispielstichprobe = tibble(var1 = runif(100, min = 2, max = 4))

beispielstichprobe %>% pull(var1) %>% shapiro.test() %>% pluck("p.value")
```

```
0.000571528901829678
```

Dieser Wert ist nicht grösser als 5% (bzw. `0.05`), womit wir die Nullhypothese "Die Variable ist normalverteilt" ablehnen müssen.

<p class="alert alert-warning">In der Literatur finden Sie gelegentlich die strengere Anforderung, dass mindestens 10% der Abweichungen unwahrscheinlicher sein dürfen.</p>

### Beispiel

Das Illustrieren wir uns mit Hilfe eines Beispiels. Wir verwenden [die Stichprobe `deviceuse`](). 

```R
library(tidyverse)

stichprobe = read_csv("deviceuse.csv" )
```

Diese Stichprobe ist ein Ausschnitt aus der Online Umfrage zum digitalen Habitat, die Sie am Anfang des Studiums ausgefüllt haben.

Die Stichprobe hat vier Vektoren:

* `id` - ein Indikator für die jeweiligen Messungen
* `q00` - eine nominalskalierter Variable mit den Ausprägungen `F`, `M`, `O` und `X`.
* `q01` - eine Likert-Skala im Wertebereich `1`-`7`.
* `q02` - eine metrischskalierte Variable im Wertebereich von `0`-`20`
* `q03` - eine metrischskalierte Variable im Wertebereich von `0`-`20`

Visualisieren wir uns die Variablen `q02` und `q03` aus dieser Stichprobe.

```R
deviceuse %>%
    pivot_longer(c(q02, q03), 
                 names_to = "variable",
                 values_to = "werte") %>%
    ggplot(aes(x = werte)) + 
        geom_histogram(binwidth = 1) +
        facet_grid(  ~ variable) +
        theme_minimal()
```

![Normalverteilung Ja oder Nein](https://github.com/dxiai/ct-resourcen/raw/master/statistik/bilder/normalverteilung_ja_nein.png)

Wenn wir uns die beiden Verteilungen `q02` und `q03` ansehen und nur mit Hilfe des Plots entscheiden müssten, welche der beiden Variablen normalverteilt ist, fällt uns das sehr schwer. 

Stattdessen bestimmen wir die Kennwerte für die beiden Variablen. Diesen Kennwerten fügen wir den Shapiro-Wilk-Test an. Damit wir leichter erkennen, ob eine Normalverteilung gegeben ist, prüfen wir direkt auf unseren Grenzwert. Das Ergebnis dieser Prüfung steht im Ergebnisvektor `normalverteilt`. Den Abschluss der Kennwerte macht der Mittelwert und die Standardabweichung. Diese Werte dürfen wir aber nur dann berichten, wenn im Vektor `normalverteil` an der korrespondierenden Stelle `TRUE` steht. 

```R
deviceuse %>%
    pivot_longer(c(q02, q03), 
                 names_to = "variable",
                 values_to = "werte") %>%
    select(variable, werte) %>%
    drop_na() %>%
    group_by(variable) %>% 
    summarise(
        n = n(),
        
        min = min(werte),
        max = max(werte),
        bw = max - min,

        iqr = IQR(werte),

        q1 = quantile(werte, .25),
        md = median(werte),
        q3 = quantile(werte, .75),

        # Shapiro-Wilk-Test
        shapiro_p = werte %>% shapiro.test() %>% pluck("p.value"),
        # Wir prüfen automatisch auf die Normalverteilung
        normalverteilt = shapiro_p > .05, 
        
        mw = mean(werte),
        sd = sd(werte)
    )
```

<table border="1">
<thead>
	<tr><th scope=col>variable</th><th scope=col>n</th><th scope=col>min</th><th scope=col>max</th><th scope=col>bw</th><th scope=col>iqr</th><th scope=col>q1</th><th scope=col>md</th><th scope=col>q3</th><th scope=col>shapiro_p</th><th scope=col>normalverteilt</th><th scope=col>mw</th><th scope=col>sd</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;lgl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>q02</td><td>126</td><td>1.000000</td><td>19.00000</td><td>18.0000</td><td>4.000000</td><td>5.000000</td><td>7.00000</td><td> 9.00000</td><td>0.0006596695</td><td>FALSE</td><td>7.095238</td><td>3.109800</td></tr>
	<tr><td>q03</td><td>126</td><td>1.292647</td><td>17.53755</td><td>16.2449</td><td>4.140227</td><td>6.402633</td><td>8.41641</td><td>10.54286</td><td>0.2230621254</td><td> TRUE</td><td>8.259517</td><td>2.919789</td></tr>
</tbody>
</table>

Wir sehen, dass die Variable `q02` als *nicht normalverteilt* (p =  .6%) angesehen werden kann. Die Variable `q03` wird hingegen als *normalverteilt* erkannt (p = 22.3%). 
