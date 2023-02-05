# Verteilungen beschreiben

## Verteilung

<div class="alert alert-primary">
Unter einer <b>Verteilung</b> verstehen wir <i>Häufigkeiten von Merkmalsausprägungen in einer Stichprobe oder Grundgesamtheit</i>. 
</div>

Eine Verteilung beschreibt also, wie oft wir eine bestimmte Merkmalsausprägung in unserer Stichprobe gemessen haben. Eine *Merkmalsausprägung* steht dabei für die Werte unserer Variablenskalierung. 

Für diesen Teil benötigen Sie die gleiche [Beispielstichprobe]() wie schon im [Abschnitt *Stichproben beschreiben*]().

```R
library(tidyverse)

stichprobe = read_csv("stichprobe.csv")
``` 

### Verteilungen für nominale Skalenniveaus

Im Abschnitt haben wir die Häufigkeiten einzelner nominalskalierter Variablen gezeigt. Wiederholen wir kurz das Beispiel. 

```R
stichprobe %>% 
    group_by(q3) %>% 
    summarise(n = n())
```

<table border="1">
<thead>
	<tr><th scope=col>q3</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>F</td><td>10</td></tr>
	<tr><td>M</td><td>17</td></tr>
</tbody>
</table>
<br>

```R
stichprobe %>% 
    count(q4)
```

<table border="1">
<thead>
	<tr><th scope=col>q4</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>N</td><td>17</td></tr>
	<tr><td>P</td><td>10</td></tr>
</tbody>
</table>
<br>

Lassen wir die Werte einfach so stehen, kann leicht der Eindruck entstehen, dass die beiden Variablen gleich in der Stichprobe verteilt sind. Deshalb ist es oft notwendig, die Verteilung über zwei oder mehr nominalskalierte Variablen zu bestimmen. Das erreichen wir, indem wir das Auftreten aller **Permutationen** der Merkmalsausprägungen zählen. Wir bestimmen also die Häufigkeiten des *gemeinsamen Auftretens* verschiedener Variablenwerte. Diese Häufigkeiten bestimmen wir über das gruppieren über mehrere Variablen, wie im folgenden Beispiel.

```R
stichprobe %>% 
    count(q3, q4) 
```

<table border="1">
<thead>
	<tr><th scope=col>q3</th><th scope=col>q4</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>F</td><td>N</td><td> 4</td></tr>
	<tr><td>F</td><td>P</td><td> 6</td></tr>
	<tr><td>M</td><td>N</td><td>13</td></tr>
	<tr><td>M</td><td>P</td><td> 4</td></tr>
</tbody>
</table>
<br>


Das Beispiel zeigt, wie häufig welche Merkmalsausprägungen gemeinsam in unserer Stickprobe vorkommen. Auch hier können wir den Modus wie oben bestimmen. 


```R
stichprobe %>% 
    count(q3, q4) %>%
    filter(n == max(n))
```

<table border="1">
<thead>
	<tr><th scope=col>q3</th><th scope=col>q4</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>M</td><td>N</td><td>13</td></tr>
</tbody>
</table>
<br>


Da der Modus einen **absoluten** Wert anzeigt, ist es schwer, die Bedeutung einzuschätzen. Deshalb werden zusätzlich oft auch **relative** Bezugsgrössen mit angegeben. Der relativen Wert ergibt sich aus dem *Verhältnis* des absoluten Werts zum Stichproben- oder Variablenumfang.  


```R
stichprobe %>% 
    count(q3, q4) %>%
    # relativen Wert bestimmen
    mutate(relativ = round(n / stichprobenumfang, 2)) %>%
    filter(n == max(n))
```


<table border="1">
<thead>
	<tr><th scope=col>q3</th><th scope=col>q4</th><th scope=col>n</th><th scope=col>relativ</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>M</td><td>N</td><td>13</td><td>0.48</td></tr>
</tbody>
</table>
<br>

Mit Hilfe des relativen Werts des Modus erkennen wir nun leicht, dass die Permutation (`M`, `N`) der Variablen `q3` und `q4` fast die Hälfte aller Datensätze in unserer Stichprobe ausmacht. Bei kleinen Stichproben, wie in diesem Beispiel, werden Prozentwerte auf den nächsten ganzen Wert gerundet. Das entspricht 2 Nachkommastellen in der Dezimaldarstellung.  


Der Modus fokussiert nur auf eines der möglichen Werte der darunterliegenden Variablenskalierung. Daher ist es üblich die Verteilung der Häufigkeiten über die Skalierung anzugeben. 

<div class="alert alert-primary" markdown=1>
Unter **Verteilung** versteht man in der deskriptiven Statistik die Häufigkeiten für das jeweilige Skalenniveau. Oben haben wir diese Verteilungen bereits gesehen. 
</div>

Zur besseren Lesbarkeit der Häufigkeiten von Permutationen werden diese oft als *Kreuztabelle* dargestellt. Dazu schwenken (engl. *to pivot*) wir eine der Variablen, so dass die Merkmalsausprägungen als Spaltenüberschriften dargestellt werden. Dadurch wird die Darstellung breiter. Entsprechend müssen wir die `pivot_wider()`-Funktion verwenden. Welche Variable wir schwenken, ist eine reine Darstellungsfrage. Es ist normal mehrere Darstellungen auszuprobieren, bevor man sich für eine Variante entscheidet. Dabei ist das Leitprinzip die einfache Lesbarkeit der beschriebenen Werte.  

Im folgenden Beispiel schwenke ich die Ausprägungen der Variable `q4`.


```R
stichprobe %>% 
    count(q3, q4) %>% 
    pivot_wider(names_from = q4, values_from = n)
```

<table border="1">
<thead>
	<tr><th scope=col>q3</th><th scope=col>N</th><th scope=col>P</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>F</td><td> 4</td><td>6</td></tr>
	<tr><td>M</td><td>13</td><td>4</td></tr>
</tbody>
</table>
<br>

### Verteilungen für ordinale Skalenniveaus

Verteilungen für ordinale Skalenniveaus werden genauso bestimmt, wir für nominale Skalenniveaus. Das ist möglich, weil es sich bei ordinalskalierten Variablen ebenfalls um diskrete Daten handelt. 


```R
stichprobe %>% 
    count(q10_1_0)

stichprobe %>%
    count(q10_1_1)
```


<table border="1">
<thead>
	<tr><th scope=col>q10_1_0</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td> 3</td><td>2</td></tr>
	<tr><td> 4</td><td>2</td></tr>
	<tr><td> 5</td><td>3</td></tr>
	<tr><td> 6</td><td>2</td></tr>
	<tr><td> 7</td><td>5</td></tr>
	<tr><td> 8</td><td>3</td></tr>
	<tr><td> 9</td><td>4</td></tr>
	<tr><td>10</td><td>6</td></tr>
</tbody>
</table>
<br>



<table border="1">
<thead>
	<tr><th scope=col>q10_1_1</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td> 4</td><td> 1</td></tr>
	<tr><td> 5</td><td> 1</td></tr>
	<tr><td> 7</td><td> 1</td></tr>
	<tr><td> 8</td><td> 4</td></tr>
	<tr><td> 9</td><td>11</td></tr>
	<tr><td>10</td><td> 9</td></tr>
</tbody>
</table>
<br>


### Verteilungen für metrische Skalenniveaus

Bei metrischskalierten Variablen können wir die Häufigkeiten normalerweise nicht mit `count()` bestimmen, weil die Werte *stetig* über den Wertebereich verteilt sein können. Um die Verteilung bei metrischen Skalenniveaus zu erhalten, müssen wir die Häufigkeiten in Teilintervallen bestimmen. Teilintervalle legen wir mit der `cut_interval()`-Funktion im gemessenen Werte fest. Dazu legen wir die Anzahl der Intervalle mit dem `n`-Parameter fest und zählen die gemessenen Werte in diesen Intervallen. 

Eine ähnliche Technik wird auch bei Histogrammen verwendet. 

Das folgende Beispiel illustriert die Logik, wobei der Wertebereich jeder Variable möglichst in 5 Intervalle geteilt wird. Das Klappt nur, wenn der Wertebereich weit genug gestreckt ist. In unserem Beispiel sind die (diskreten) Werte nicht immer genug gestreckt, so dass für bestimmte Variablen nicht 5 Intervalle gebildet werden können. 



```R
stichprobe %>%
    mutate(
        intervall = q10_1_0 %>% cut_interval(n = 5)
    ) %>% 
    count(intervall)

stichprobe %>%
    mutate(
        intervall = q10_1_1 %>% cut_interval(n = 5)
    ) %>% 
    count(intervall)
```


<table border="1">
<thead>
	<tr><th scope=col>intervall</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>[3,4.4]  </td><td> 4</td></tr>
	<tr><td>(4.4,5.8]</td><td> 3</td></tr>
	<tr><td>(5.8,7.2]</td><td> 7</td></tr>
	<tr><td>(7.2,8.6]</td><td> 3</td></tr>
	<tr><td>(8.6,10] </td><td>10</td></tr>
</tbody>
</table>
<br>

<table border="1">
<thead>
	<tr><th scope=col>intervall</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>[4,5.2]  </td><td> 2</td></tr>
	<tr><td>(6.4,7.6]</td><td> 1</td></tr>
	<tr><td>(7.6,8.8]</td><td> 4</td></tr>
	<tr><td>(8.8,10] </td><td>20</td></tr>
</tbody>
</table>
<br>


Im gegebenen Beispiel gliedere ich die Stichprobe in 5 Intervalle. Alternativ kann ich auch die Intervallbreite mit `cut_width()` festlegen. Diese Alternative erlaubt es uns gleich grosse Intervalle für zwei Variablen mit unterschiedlichen Wertebereichen leichter zu vergleichen. Das ist bei metrischen Skalennivaus möglich, weil die Intervalle sich aus dem gemessenen Wertebereich wiedergeben. Wenn die Bandbreite der gemessenen Werte für zwei Variablen ungleich ist, müssen die Intervallgrenzen für beide Variablen manuell festgelegt werden. 


<p class="alert alert-info"><b>Praxis</b>: Stellen die beiden Intervallauswertungen gegenüber. Was fällt Ihnen auf?</p>

<div class="col-md-12 text-center h4">
    <a href="https://moodle.zhaw.ch/course/view.php?id=7569&section=4"><i class="fa fa-lg fa-arrow-up"></i></a>
</div>