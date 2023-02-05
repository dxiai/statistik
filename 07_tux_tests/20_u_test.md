# Ordinale Skalenniveaus mit dem Wilcoxon und Mann U-Test vergleichen

<div class="col-12 alert alert-primary" markdown=1>
<i class="fa fa-lg fa-info-circle"></i>
Dieser Abschnitt ergänzt das Kapitel 8.7 in Bortz und Schuster (2010, S. 130-135).
</div>

Der t-Test ermöglicht uns zwei hinreichend normalverteilte Stichproben miteinander zu vergleichen. Wir haben auch die Konzepte der abhängigen und unabhängigen Stichproben kennengelernt. Beim Stichprobenvergleich mit dem t-Test müssen wir sicherstellen, dass beide Stichproben hinreichend normalverteilt sind. Das überprüfen wir meistens schon bei der Beschreibung unserer Stichprobe mit dem Shapiro-Wilk-Test. 

Der t-Test hat zwar einige Voraussetzungen aber leider auch einige Vorbedingungen, so dass wir diesen Test für viele Variablen nicht verwenden können. Das sind zum Einen alle metrischskalierten Variablen, die nicht hinreichend normalverteilt sind, und zum Anderen alle ordinalskalierten Variablen, für die Normalverteilung nicht definiert ist. 

Hier stellen sich uns zwei Fragen: 

1. Was machen wir aber, wenn wir mit dem Shapiro-Wilk-Test keine hinreichende Normalverteilung bestätigen können? 
2. Wie können wir Variablen mit ordinalen Skalenniveaus vergleichen?

Hier kommen sog. *nicht-parametrische* bzw. *verteilungsfreie* Verfahren zum Einsatz. 

Wir werden zwei dieser Verfahren im Detail diskutieren: 

1. Rangsummenanalyse
2. \\( \chi^2 \\)-Test

In diesem Abschnitt konzentrieren wir uns *ausschliesslich* auf die beiden wichtigen Tests der Rangsummenanalyse: 

1. U-Test nach Mann-Whitney (oder Mann-Whitney U-Test bzw. kurz Mann U-Test) für unabhängige Stichproben
2. Wilcoxon-Test für abhängige Stichproben

Mit der Rangsummenanalyse beantworten wir die gleichen Fragestellungen wie mit dem t-Test. Deshalb kann man sich wundern, ob man nicht immer mit diesen Verfahren arbeiten sollte. 

<div class="alert alert-success"  markdown=1>
**Merke** *Der t-Test hat eine stärkere Aussagekraft als die Rangsummenanalyse.* Wenn die Voraussetzungen für den t-Test erfüllt sind, wird immer der t-Test verwendet. Wenn nicht, dann verwenden wir den Mann U-Test bzw. den Wilcoxon-Test.
</div>

### Prinzip der Rangsummenanalyse

Bei der Rangsummenanalyse steht die Verteilung der Merkmalsausprägungen im Zentrum der Analyse. Dazu werden die Merkmalsausprägungen hinsichtlich des Abstands zum Median sortiert. In dieser Reihenfolge hat jede Merkmalsausprägung ihren *Rang* vom geringsten Abstand zum grössten Abstand. Wenn wir nun zwei Stichproben vergleichen, sortieren wir Merkmalsausprägungen beider Stichproben nach ihren Rängen. Wir können nun die Stichproben dahingehend vergleichen, ob die Ränge der beiden Stichproben gleich über die Ränge verteilt sind oder ob eine der beiden Stichproben bestimmte Ränge belegt bzw. in wie weit die Ränge von Merkmalsausprägungen voneinander abweichen. 

Damit gibt es zwei mögliche Ergebnisse: 

1. Die Ränge der Merkmalsausprägungen sind gleichmässig zwischen den Stichproben verteilt. Es  gibt also keine Unterschiede zwischen den beiden Stichproben.
2. Die Ränge der Merkmalsausprägungen der Stichproben klumpen zu den Enden der Skalierung. Hier gibt es ein grösser-kleiner Verhältnis.

Diese Verteilungen spiegeln sich in den **Summen der Ränge** wider. Aus diesen Summen leitet sich der Name *Rangsummenanalyse* ab. 

Bilden wir die Differenz der Rangsummen beider Stichproben, dann erhalten wir im ersten Fall als Ergebnis 0, weil sich die Ränge sich gegenseitig aufheben. Im zweiten Fall wird die Differenz entweder grösser oder kleiner 0 sein, je nachdem zu welchen Ende die Stichprobe klumpen. Die Differenz wird als *U* bezeichnet, woher der Mann-Whitney U-Test seinen Namen ableitet. Die möglichen Differenzen sind für die Nullhypothese *normalverteilt* mit dem Mittelwert 0. Dabei gilt wieder die Annahme, dass es keinen Unterschied zwischen den Stichproben gibt.

<div class="alert alert-success" markdown=1>
Rechnen Sie das Alkohol-Beispiel auf Seite 130f in Bortz und Schuster auf Papier nach. Damit lässt sich das Prinzip sehr gut nachvollziehen.  
</div>

Damit können wir die gleichen Nullhypothesen wie beim z-Test für nicht-metrische und nicht-normalverteilte Stichproben beantworten. Ähnlich wie beim t-Test unterscheiden wir auch hier zwischen abhängigen und unabhängigen Stichproben. 

### Mann U-Test oder Wilcoxon-Test?

Obwohl der Wilcoxon-Test und der  Mann-Whitney U-Test mathematisch gleichwertig sind (Bortz & Schuster, 2010), berücksichtigt der Wilcoxon-Test zusätzlich *abhängige Stichproben*. Entsprechend stellt R nur eine einzige Funktion für beide Tests bereit: Die Funktion `wilcox.test()`.

Es hat sich eingebürgert, dass wir vom Wilcoxon-Test sprechen, wenn wir von der Rangsummenanalyse für abhängige Stichproben sprechen, und vom Mann U-Test, wenn unabhängige Stichproben verglichen werden.

<div class="alert alert-success" markdown=1>
**Merke**

- Unabhängige Stichproben: Mann-Whitney U-Test: `wilcox.test(zielgrösse ~ erklaerung, paired = FALSE)`
- Abhängige Stichproben: Wilcoxon-Test: `wilcox.test(varA, varB, paired = TRUE)`
</div>

### Anwendung in der Praxis

In der Praxis verwenden wir beide Tests zur Beantwortung ähnlicher Fragestellungen, für die wir auch den t-Test verwenden würden. Wir können uns diese beiden Tests also als Rettungsring vorstellen, den wir verwenden, wenn wir unsere Variablen nicht mit dem t-Test auswerten dürfen.

Für das folgende Beispiel verwenden wir die [Daten aus der Umfrage zum digitalen Umfeld](https://moodle.zhaw.ch/mod/resource/view.php?id=477490). In dieser Stichprobe gibt es verschiedene ordinalskalierte Variablen. Eine davon ist Nutzung von Smartwatches (`q16_10`). Die Variable hat die folgende Skalierung:

* 1 = keine Nutzung
* 2 = gelegentliche Nutzung
* 3 = min. monatlich
* 4 = min. wöchentlich
* 5 = min. täglich

Weil gelegentlich Werte kleiner 0 und NA vorkommen, müssen wir die Werte zuerst *filtern*, dass wir nur Datensätze vorliegen haben, die in dieser Variable Werte grösser als 0 haben. 

Wir kennen ausserdem das Betriebssystem des Smartphones der Teilnehmenden (`q09_1`).

Wir fragen uns, ob iPhone-Nutzende häufiger eine Smartwatch verwenden als Android-Nutzende. Diese Hypothese ist eine rechtsgerichtet, weil hier vermutet wird, dass iPhone-Nutzende häufiger eine Smartwatch verwenden als Android-Nutzende. 

Betrachten wir zuerst die Verteilungen.

```R
daten %>% 
    filter(q16_10 > 0, q09_1 %in% c("Android", "iPhone")) %>% 
    count(q09_1, q16_10) %>% 
    pivot_wider(names_from = q16_10, values_from = n)
```

| q09_1 | 1  | 2  | 3  | 4  | 5  | 
|---|---|---|---|---|---| 
| Android | 93 | 1 | 3 | 6 | 6 | 
| iPhone | 91 | 3 | 3 | 7 | 11 |

Hier zeigt sich zuerst, dass die meisten Teilnehmenden gar keine Smartwatch verwenden. Die Daten deuten an, dass iPhone-Nutzende tatsächlich häufiger eine Smartwatch verwenden, als die Teilnehmenden, die ein Android verwenden. Wir fragen uns natürlich, ob dieser Eindruck auch statistisch signifikant ist. Das überprüfen wir mit dem Mann-Whitney U-Test, weil es sich bei den Teilstichproben der Besitzenden von Android und iPhone Smartphones um *unabhängige* Stichproben handelt. 

Die Funktion `wilcox.test()` hat zwei Varianten, mit der wir sie aufrufen können. Die eine Variante eignet sich gut für abhängige Stichproben. Diese Variante erwartet zwei Variablen als Parameter. Weil die Variablen abhängiger Stichproben oft in paarweisen Vektoren vorliegen, können wir diese Vektoren direkt übergeben. Testen wir gerichtete Hypothesen, dann stellen wir sicher, dass Referenzstichprobe als erstes (oder `x`-Parameter) und die Vergleichsstichprobe als zweiter (bzw. `y`) Parameter übergeben wird. Ausserdem müssen wir zwingend den Parameter `paired = TRUE` übergeben.

Die Variablen unabhängiger Stichproben liegen oft also zwei Variablen vor. Die erste Variable enthält die gemessenen Werte, die mindestens ordinalskaliert sein müssen.  Die zweite Variable enthält die jeweilige Stichprobenzuordnung und ist entsprechend nominalskaliert. Damit wir unsere Stichprobe nicht zu sehr umformen müssen, können wir die zweite Variante der `wilcox.test()`-Funktion aufrufen. Diese Variante erwartet eine sog. *statistische Formel* als ersten Parameter. Solche Formeln schreiben wir in der Form `x ~ e`, wobei `x` für die Zielvariable und `e` für die *erklärende Variable* steht. Wir kennen diese Variablen schon von der `t_test()`-Funktion: Dort haben wir die Zielvariable an den `response`-Parameter und die erklärende Variable an den `explanatory`-Parameter übergeben. 

Wir können unseren Test also wie folgt schreiben: 

```R
daten %>%
    summarise(
         u = wilcox.test(q16_10 ~ q09_1, data = .) %>% tidy() %>% list()
    ) %>% 
    unnest(u)
```

Wir beachten hier, dass wir ausserdem, dass wir neben der Formel als zweiten Parameter einen `.` übergeben haben. Der Punkt teilt der `summarise()`-Funktion mit, dass es hier das verkettete Objekt hier einfügen soll. Das ist in unserem Fall unser Stichprobenobjekt in der R-Variable `daten`. Der Punkt als zweiter Parameter **muss** bei dieser Variante der `wilcox.test()`-Funktion zwingend übergeben werden! 

Dieser Code wird aber zu einem Fehler führen, weil die Variable `p09_1`  mehr als 2 Werte (im R-Jargon `levels`) hat. 

```R
daten %>%
    summarise(
        q09_1 = unique(q09_1)
    )
```

| q09_1 | 
| --- | 
| Android |
| iPhone |
| Andere |
| Mobiltelefon |

Damit hätten wir nicht zwei sondern vier unabhängige Stichproben. Wie beim t-Test darf die erklärende Variable aber nur zwei Ausprägungen haben. Unsere Hypothese bezieht sich nur auf die Ausprägungen `Android` und `iPhone`. Wir könnten zuerst filtern, was aber ein Problem ergibt, wenn wir mehrere Wilcoxon-Tests auf einmal ausführen wollen. Stattdessen können wir den `subset` Parameter verwenden. Dieser Parameter erwartete einen logischen Ausdruck der auf die erklärende Variable verweist und die Variable auf genau zwei Gruppen reduziert. 

Für unsere Hypothese wollen wir nur Android und iPhone-Besitzende berücksichtigen. Wir können also unseren ersten Ansatz wie folgt umformen:

```R
daten %>%
    filter(q16_10 > 0) %>%
    summarise(
        u_smartphone = wilcox.test(
                            q16_10 ~ q09_1, 
                            data = ., 
                            subset = q09_1 %in% c("Android", "iPhone"),
                            alternative = "greater"
                       ) %>% 
                          tidy() %>% 
                          list()
    ) %>% 
    unnest(u_smartphone)
```

Wir erinnern uns, dass die Nullhypothese in diesem Fall war, dass Teilnehmende mit einem iPhone nicht häufiger eine Smartwatch verwenden als jene mit einem Android-Gerät. Unsere Hypothese ist, dass Eigentümer von iPhones häufiger eine Smartwatch verwenden als die von Android-Smartphones. *Woher weiss nun R welcher Teil der Stichprobe auf welcher Seite des Grösserzeichens stehen muss?* Gar nicht. R verwendet in diesem Fall den Wert der im Vektor `q09_1` als erstes vorkommt. Das ist etwas unzuverlässig. Stattdessen wollen wir die Reihenfolge der Gruppen explizit festlegen. Das machen wir mit der `fct_relevel()`-Funktion. Mit dieser Funktion können wir die Reihenfolge der Werte für unsere Analyse festlegen. Im folgenden Beispiel sortiere ich `iPhone` vor `Android`, so dass R in der Folge das  `greater` im Wilcoxon-Test als *`iPhone` > `Android`* interpretiert. 

```R
daten %>%
    filter(q16_10 > 0) %>%
    mutate(
        q09_1 = q09_1 %>% 
                    fct_relevel(c("iPhone", "Android", "Andere", "Mobiltelefon"))
    ) %>%
    summarise(
        u_smartphone = wilcox.test(
                            q16_10 ~ q09_1, 
                            ., 
                            subset = q09_1 %in% c("Android", "iPhone"),
                            alternative = "greater"
                       ) %>% 
                           pluck("p.value")
    )
``` 

| u_smartphone  | 
|---| 
| 0.1101191 |

Der p-Wert unseres Test liegt mit .11 innerhalb des 95%-Konfidenzintervalls (\\( u_{smartphone} > 0.05\\)).  Wir dürfen also die Nullhypothese nicht verwerfen. Mit anderen Worten: Wir können unseren ersten Eindruck statistisch nicht bestätigen, dass iPhone-Besitzende öfter eine Smartwatch verwenden als Besitzende von Android Smartphones.  


$$ $$