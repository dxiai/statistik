# Stichproben simulieren

Ein grosses Problem bei empirischen Studien ist die Verfügbarkeit von Daten. Wir müssen immer abwarten, bis wir die Daten erhoben haben, um ein Gefühl für die Auswertung zu bekommen. Diese Abhängigkeit von den empirischen Daten wirkt sich aus zwei Gründen negativ aus die Datenauswertung aus.  

1. Die Auswertung wird das erste Mal mit den echten Daten ausprobiert. 

2. Es bestehen oft keine Daten, mit denen wir die empirischen Daten vergleichen können.

Diese beiden Gründe lassen sich durch simulierte Daten beheben. 

<p class="alert alert-primary" markdown="1">
**Definition:** Simulierte Daten sind künstlich erzeugte Daten aus gedachten Grundgesamtheiten.
</p>

Der grosse Vorteil von Simulationen ist, dass wir beliebig viele und beliebig grosse Stichproben erheben und vergleichen können. Wir können so den Einfluss der Stichprobengrösse auf unsere Ergebnisse im Vorfeld prüfen. 

Wir unterscheiden mehrere Arten der Simulation: 

1. Die Nullhypothesensimulation
2. Die Simulation der Alternativhypothesen
3. Die Szenariensimulation
4. Bootstrapping

Mit der **Nullhypothesensimulation** erzeugen wir Stichproben aus gedachten Grundgesamtheiten, in denen unsere kausalen Annahmen insgesamt ***nicht*** zutreffen.

Mit der **Simulation der Alternativhypothesen** erzeugen wir Stichproben, in denen alle vermuteten Zusammenhänge zutreffen.

Die **Szenariensimulation** erlaubt uns unterschiedliche Szenarien zu simulieren, indem verschiedene Annahmen und Variationen in eine Simulation einfliessen. 

Das **Bootstrapping** simuliert Stichproben auf der Grundlage empirischer Daten. Das Bootstrapping wird beim statisitschen Modellieren als Variante der **Nullhypothesensimulation** verwendet. 

<p class="alert alert-primary" markdown="1">
**Definition:** *Alle Simulationsverfahren* basieren auf der **zufälligen Auswahl** oder dem **zufälligen Erzeugung** von Werten. 
</p>

In R "ziehen" wir zufällige Werte aus *statistischen Verteilungen*. Die Funktion für solche Werte wird in Bortz und Schuster (2010, S. 61) als **Zufallsvariable** bezeichnet.

<p class="alert alert-primary" markdown="1">
**Definition:** Eine **statistische Verteilung** beschreibt die Wahrscheinlichkeiten, mit der Werte in einem Skalenbereich vorkommen können für die gilt, dass die Summe der Wahrscheinlichkeiten 1 ergibt.
</p> 

## Verteilungen 

<div class="alert alert-info" markdown="1">
Für jede Verteilung stellt R vier Funktionen bereit. 

* Die `d`-Funktion für die Dichte der Verteilung an einer bestimmten Stelle der Verteilung. Dieser Wert zeigt für eine Stelle in der Verteilung, wie wahrscheinlich dieser Wert für die Verteilung ist. 
* Die `p`-Funktion für die kummulative Verteilung an einer bestimmten Stelle. Dieser Wert zeigt die Wahrscheinlichkeit für alle Werte grösser oder gleich der angegebenen Stelle in der Verteilung an. 
* Die `q`-Funktion oder Quantilsfunktion, zeigt für eine Wahrscheinlichkeit die jeweilige Quantilsgrenze in der Verteilung an. Es ist die Umkehrfunktion der jeweiligen `p`-Funktion. 
* Die `r`-Funktion zieht aus der jeweiligen Verteilung einen zufälligen Wert. Bei wiederholten Ziehen nähern sich die Werte der Verteilung an. Diese Funktion ist für Simulationen wichtig!
</div>

Für den Zweck von Simulationen in dieser Lehrveranstaltung unterscheiden wir zwischen den folgenden Verteilungen: 

- Gleichverteilung
- Normalverteilung
- Binomialverteilung
- Expoentialverteilung 

Bei der **Gleichverteilung** sind alle Werte in einem Wertebereich gleich wahrscheinlich. Zufallswerte aus dieser Verteilung sind mehr oder weniger gleichmässig verteilt. Die Gleichverteilung wird immer dann verwendet, wenn für eine Variable ***keine*** zentrale Tendenz erwartet wird. Die Zufallsfunktion der Gleichverteilung ist `runif()`.

Bei der **Normalverteilung** sind Werte nah am Mittelwert wahrscheinlicher als Werte, die weiter vom Mittelwert entfernt sind. Die Normalverteilung ist symetrisch um einen Mittelwert mit einer gegebenen Standardabweichung.

Wir verwenden die Normalverteilung immer dann als Ausgangsverteilung, wenn wir metrische Werte simulieren möchten, bei denen ein bekannter Mittelwert das Maximum der Verteilung ist und extreme Abweichungen von diesem Mittelwert immer unwahrscheinlicher sind. Deshalb wird die Normalverteilung auch gerne zur Simulation zufälliger Fehler gewählt, bei denen kleine Abweichungen vom "echten" Wert häufiger Vorkommen als exteme Abweichungen.

Wir steuern die Verteilung über zwei Parameter: Den Mittelwert und die Standardabweichung. Über den Mittelwert legen wir das Zentrum der Verteilung fest und über die Standardabweichung definieren wir, wie breit die Werte um den Mittelwert gestreut sind. Dabei gilt, je grösser die Standardabweichung, je breiter wird die Verteilung. 

Beispiel für eine Normalverteilung mit \\(\mu = 5 \\) und \\(sd = 3\\).
![Beispiel für eine Normalverteilung mit \\(\mu = 5 \\) und \\(sd = 3\\).]()

Die Zufallsfunktion der Normalverteilung ist `rnorm()`.

Die **Binomialverteilung** ist eine Verteilung, bei der nur diskrete Werte möglich sind. Die Binomialverteilung leitet sich aus der Wiederholung gleichartiger Zufallsexperimente mit genau zwei Ergebniszuständen (0,1). Die Form der Binomialverteilung ergibt sich aus der Anzahl der Wiederholungen (\\( s \\)) und der Wahrscheinlichkeit (\\( p \\)), dass bei diesen Experimenten das grössere der beiden Ergebnisse eintritt. 

Weil die Binomialverteilung nur diskrete Werte > 0 annehmen kann, eignet sie sich für die Simulation ordinalskalierter Daten. 

Die Anzahl der Wiederholungen ((\\( s \\)) entspricht bei der Simulation dem maximal möglichen Wert einer ordinalskalierten Variable. 

Wird die Wahrscheinlichkeit mit `0.5` gewählt, dann sind beide Ergebnisse sind gleich wahrscheinlich. In diesem Fall ist die Binomialverteilung *symmetrisch*. Ist die Wahrscheinlichkeit < 0.5, dann ist die Verteilung rechtsschief und das Maximum der Verteilung tendiert zum unteren Skalenende. Ist die Wahrscheinlichkeit > 0.5, dann ist die Verteilung linksschief und das Maximum der Verteilung tendiert zum oberen Ende der Skalierung. 

Die sog. **Binärverteilung** ist ein Spezialfall der *Binomialverteilung* mit \\( s = 1 \\).

Die Zufallsfunktion der Binomialverteilung ist `rbinom()`.

Die letzte wichtige Verteilung ist die **Exponentialverteilung**. Die Exponentialverteilung ist eine asymetrische Verteilung mit Ihrem Maximum bei `0` und sie ist nur für Werte > 0 definert. Für grössere Werte läuft diese Verteilung asymptotisch gegen 0, d.h. grössere Werte werden immer unwahrscheinlicher. Die Exponentialverteilung ist über einen *Dämpfungsparameter* definiert. Je kleiner die Dämpfung, desto langsamer geht die Verteilung gegen 0.

Die Zufallsfunktion der Exponentialverteilung ist `rexp()`.

Die Geometrische Verteilung ist die diskrete Version der Exponentialverteilung. Der Dämpfungsparameter wird hier durch einen Wahrscheinlichkeitswert zwichen 0 und 1 ersetzt. Der Effekt auf die Verteilung ist aber der gleiche: Je grösser die Dämpfung, desto schneller läuft die Verteilung gegen 0.

Die Zufallsfunktion der geometrischen Verteilung ist `rgeom()`.

Eine besondere Verteilung, die sich ähnlich wie die geometrische Verteilung verhält ist die sog. negative Binomialverteilung. Während die geometrische Verteilung gleichmässig gegen `0` abfällt, konzentrieren sich die Werte bei der negativen Binomialverteilun bei den niedrigen Werten. Damit fällt die Verteilung schneller ab, läuft dann jedoch etwas langsamer gegen `0`.  Diese Verteilung ist für die Simulation sog. *Longtail* Probleme interessant.

Die Zufallsfunktion der negativen Binomialverteilung ist `rnbinom()`.

## Prinzipelle Funktionsweise einer Simulation

Das Grundprinzip einer Simulation ist die Erzeugung von Zufallsstichproben aus gegebenen Verteilungen.

Wir können uns einfach eine Stichprobe mit zufälligen Werten aus unterschiedlichen Verteilungen mit R erzeugen. Im folgenden Beispiel erzeugen wir uns eine Zufallsstichprobe mit `10` Entitäten aus einer Grundgesamtheit für die gilt, dass das Merkmal `V1` gleichmässig zwischen `1` und `10` verteilt ist, das Merkmal `V2` normalverteilt mit einem Mittelwert bei 5 und einer Standardabweichung von 2 ist sowie das Merkmal `V3` exponentialverteilt mit einer Dämpfung von `0.5` ist. 

```R
n = 10

tibble(
    V1 = runif(n, 1, 10),
    V2 = rnorm(n, 5, 2), 
    V3 = rexp(n, .5)
) -> stichprobe
```

Wir legen so die Rahmenbedingungen der Grundgesamtheit fest aus der wir die Zufallsstichproben ziehen. Weil wir die Merkmale entsprechend festgelegt haben, kennen wir die *wahren* Werte der Grundgesamtheit für die Merkmale. Wir können daher überprüfen, wie gut die Stichprobe die wahren Werte abbildet. Damit unterscheidet sich eine Simulation von einer empirischen Untersuchung: Bei einer empirischen Untersuchung kennen wir *nicht* die wahren Werte für die untersuchten Merkmale.

Weil auch die simulierten Werte eine Zufallsstichprobe darstellen, können wir die Methoden der deskripriven Statistik anwenden. Wir werten also diese drei metrisch skalierten Variablen aus: 

```R
stichprobe %>%
    pivot_longer(everything()) -> langform 
    
langform %>%
    group_by(name) %>%
    summarise(
        n = n(),
        mw = mean(value),
        sd = sd(value),

        min = min(value),
        max = max(value),
        bandbreite = max - min,
        
        median = median(value),
        mad = mad(value),
        iqr = IQR(value),

        q1 = quantile(value, .25),
        q3 = quantile(value, .75),
    )
```

Stellen wir uns die Histogramme für diese Variablen dar. 

```
langform %>% 
    ggplot(aes(value)) + 
        geom_histogram() + 
        facet_wrap(name)
```

## Simulationen wiederholen

Neben dem Umstand, dass wir bei einer Simulation die Grundgesamtheit kennen, haben Simulationen einen zweiten entscheidenden Vorteil gegenüber einer empirischen Untersuchung: Simulationen können leicht wiederholt werden.

Dabei nutzen wir in R aus, dass wir Stichproben in andere Stichproben als Listen einbetten können. Erzeugen wir uns 10 Stichproben aus der gleichen Grundgesamtheit: 

```R
n= 10

tibble(
    nr = 1:10
) %>%
    group_by(nr) %>%
    mutate(
        stichprobe = tibble(
            V1 = runif(n, 1, 10),
            V2 = rnorm(n, 5, 2), 
            V3 = rexp(n, .5)
        ) %>% list()
    ) -> simulierteStichproben
```

Wir haben nun eine Stichprobe, die 10 simulierte Stichproben enthält. 

Bestimmen wir für diese Stichproben die Mittelwerte für alle Variablen: 

```R
simulierteStichproben %>% 
    mutate(
        mittelwerte = stichprobe %>% 
                pluck(1) %>% # Stichprobe aus der Liste extrahieren
                pivot_longer(everything()) %>% 
                group_by(name) %>% 
                summarise(mw = mean(value)) %>% 
                list()      # Diese neue Liste wird gebraucht, weil wir jeweils 
                            # mehr als einen Wert erzeugen
    ) -> stichprobenMitMittelwerten
```

Jetzt können wir diese Mittelwerte extrahieren. 

```R
stichprobenMitMittelwerten %>% 
    select(-stichprobe) %>% 
    unnest(mittelwerte) -> variablenMittelwerte
```






## Variablen simulieren

### metrisch skalirte Variablen

### ordinal skalierte Variablen

### Bi-variante Variablen

Eine Bi-variante Variable ist eine Variable mit 2 Merkmalsausprägungen. Solche Variablen lassen sich wie eine Ordinalskalierte Variable simulieren. Wir erzeugen dazu eine Zufallsstichprobe aus einer Binomialverteilung mit genau einem Versuch. Diese Stichprobe hat genau zwei mögliche Ergebnisse: `0` oder `1`. Über die Wahrscheinlichkeit zeigen wir an, welches der beiden Ergebnisse häufiger vorkommen soll. Ist die Wahrscheinlichkeit > 0.5 werden wir mehr `1`-Werte erhalten. Ist die Wahrscheinlichkeit < 0.5, dann erhalten wir mehr `0`-Werte. 

Alternativ können wir eine bivariante Variable aus einer beliebigen anderen statistischen Verteilung ableiten indem wir die Werte der Verteilung so runden, dass wir genau zwei Werte erhalten. 

In der Praxis ist der Weg über die Binomialverteilung am einfachsten und am einfachsten nachzuvollziehen. 

### Nominal skalierte Variablen 

Nominal Skalierte Variablen simulieren wir wie Ordinalskalierte Variablen. Die Ordung der Variablen ergibt sich aus den vermuteten Häufigkeiten. 

Mit den Funktionen `fct_refactor()` weisen wir den Werten den jeweiligen Wert der jeweiligen Skalierung zu. 

Die Idee hinter diesem Ansatz ist, dass wir die Werte nach ihrer Häufigkeit sortieren.

Um keine Unterschiede zwischen den Merkmalen zu simulieren verwenden wir die `runif()`-Funktion. Falls wir Unterschiede zwischen Merkmalsausprägungen vermuten, dann verwenden wir wieder die `rbinom()`-Funktion. Anders als bei ordinal-skalierten Variablen organisieren wir die Wahrscheinlichkeit p immer mit \\( 0 < p < 0.5 \\). Damit stellen wir sicher, dass die am häufigsten auftretende Merkmalsausprägung klein ist, weil wir die Merkmalsausprägungen ja nach Häufigkeit sortiert annehmen. 

Das folgende Beispiel simuliert eine Variable mit 3 Merkmalsausprägungen. 

```R
N = 100

varP = .3
uniP = .7

tibble( x = rbinom(100, 2, .3) %>% as.factor())
```

