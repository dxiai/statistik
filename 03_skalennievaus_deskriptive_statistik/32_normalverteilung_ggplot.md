# Normalverteilung mit ggplot darstellen

Gelegentlich wollen wir Normalverteilungen in unseren Daten darstellen. Die Normalverteilung ist eine statistische Funktion, die kein eigenes Stichprobenobjekt erzeugt. Dazu nutzen wir die Möglichkeit von `ggplot`, funktionen direkt darstellen zu können. 

Weil wir ein Stichprobenobjekt benötigen, müssen wir `ggplot` trotzdem einen Platzhalter übergeben. Das ist das `NULL`-Objekt. Damit erkennt `ggplot`, dass wir keine Daten übergeben. 

Zur Darstellung verwenden wir die `dnorm`-Funktion. Diese Funktion steht für *Distribution of Normality* und gibt uns für einen Punkt auf der X-Achse den zugehörigen Wahrscheinlichkeitswert auf der Y-Achse. Können diese Funktion an  `geom_line()` übergeben, damit wir die Verteilungslinie erhalten. 

```
ggplot(NULL, aes(x = c(-4, 4))) +
    xlab("x") +
    geom_line(stat = "function", fun = dnorm, xlim = c(-4, 4))
```

<img src="https://github.com/dxiai/statistik/raw/main/bilder/norm_vis/norm_line.png" width="400">

Wir können die Funktion auch zum Zeichnen von  Flächen verwenden. 

```
ggplot(NULL, aes(x = c(-4, 4))) +
    xlab("x") +
    geom_area(stat = "function", fun = dnorm, xlim = c(-4, 4))
```
<img src="https://github.com/dxiai/statistik/raw/main/bilder/norm_vis/norm_area.png" width="400">

Wenn wir keine Stichprobe haben, weiss `ggplot` nicht, welchen Wertebereich es darstellen soll. Deshalb *müssen* wir den zusätzlichen Parameter `xlim` mitgeben. Dieser Parameter erwartet einen Vektor mit 2 Werten, die die Unter- und Obergrenze des Darstellungsintervalls zeigen. Weil die Normalverteilung symmetrisch ist, wählen wir diese Grenzen im gleichen Abstand von der 0. In meinem Beispiel habe ich -4 und 4 gewählt, weil die Normalverteilung zur 4. Standardabweichung stark abfällt. 

<p class="alert alert-info">
Probieren Sie auch andere Darstellungsbereiche aus. 
</p>

### Teilflächen darstellen

Wir können auch Ausschnitte der Normalverteilung darstellen, um uns zu veranschaulichen wie welchen Umfang einer Normalverteilung ein bestimmter Prozentwert hat. Dazu zeichnen wir als Linie den gesamten Wertebereich unserer Normalverteilung. Anschliessend zeichnen wir den Flächenausschnitt für die Prozentzahl, die wir darstellen möchten. 

```
ggplot(NULL, aes(x = c(-4, 4))) +
    xlab("x") +
    geom_line(stat = "function", fun = dnorm, xlim = c(-4, 4)) +
    geom_area(stat = "function", fun = dnorm, xlim = c(-4, 2), fill = "red")
```

<img src="https://github.com/dxiai/statistik/raw/main/bilder/norm_vis/norm_part.png" width="400">

Ich habe hier den Ausschnitt auf das Intervall -4 bis 2 beschränkt. 

### Prozentwerte als Fläche unter der Normalverteilung darstellen

Wir wissen aber oft nicht welchen Wert auf der X-Achse wir auswählen müssen. Dabei hilft uns die `qnorm()`-Funktion. Die `qnorm()` nimmt Wert für die Fläche unter der Verteilung (den sog. `p`-Wert) und gibt den zugehörigen Wert auf der X-Achse zurück. Um zum Beispiel die Fläche von 90% darzustellen, verwenden wir den folgenden Aufruf. 

```
ggplot(NULL, aes(x = c(-4, 4))) +
    xlab("x") +
    geom_line(stat = "function", fun = dnorm, xlim = c(-4, 4)) +
    geom_area(stat = "function", fun = dnorm, xlim = c(-4, qnorm(p = .90)), fill = "red")
```

<img src="https://github.com/dxiai/statistik/raw/main/bilder/norm_vis/norm_perc.png" width="400">

### Normalverteilung und Histogramme

Wenn wir für metrischskalierte Variablen für ein Histogramm das Verhältnis zur Normalverteilung darstellen möchten, dann müssen wir einen kleinen Kunstgriff verwenden: Anstatt der absoluten Häufigkeit müssen wir beim Histogramm die *Dichte* der Häufigkeit als Grössenordnung für die Balken verwenden. Das erreichen wir, indem wir der Funktion `geom_histogram()` einen Ästhetikhinweis für die Y-Achese mit `aes(y = ..density..)` übergeben. Dadurch *normalisiert* `ggplot` unsere gemessenen Werte automatisch auf die Grössenordnung der Normalverteilung.

Zusätzlich müssen wir die Normalverteilung noch so verschieben, dass die Wertebereiche unserer Variable mit der Normalverteilung zusammenpassen. Dazu berechnen wir den Mittelwert und die Standardabweichung unserer Stichprobe und übergeben diese Werte als zusätzliche Argumente für die `dnorm` Funktion im `args`-Parameter. 

```
du = read_csv("deviceuse-fm20.csv")

mw = du %>% pull(q02) %>% mean()
sd = du %>% pull(q02) %>% sd()

du %>%
    ggplot(aes(x = q02)) + 
        geom_histogram(
            aes(y =..density..), 
            binwidth = 1, 
            fill = "white", 
            color= "darkgrey"
        ) +
        geom_line(
            stat= "function", 
            fun = dnorm, 
            args = c(
                mean = mw , 
                sd = sd
            ), 
            color = "red"
        )
```

<img src="https://github.com/dxiai/statistik/raw/main/bilder/norm_vis/norm_histo.png" width="400">

<div class="col-md-12 text-center h4">
    <a href="https://moodle.zhaw.ch/course/view.php?id=7569&section=4"><i class="fa fa-lg fa-arrow-up"></i></a>
</div>