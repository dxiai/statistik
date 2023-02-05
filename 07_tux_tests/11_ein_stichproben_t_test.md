# 1-Stichproben t-Test



<div markdown=1 class="alert alert-info">
Dieser Abschnitt bringt Beispiele mit R  für das Kapitel 8.1 in Bortz & Schuster (2010). 
</div> 

Der 1-Stichproben t-Test erlaubt es uns, einen theoretischen Mittelwert mit unseren erhobenen Daten zu vergleichen, obwohl wir die Varianz  der Grundgesamtheit nicht kennen. Während wir für den  [z-Test]()  die Grundgesamtheit kennen müssen, benötigen wir nur Richtwerte. Diese Richtwerte können wir z.B. aus einer vorherigen Messung erhalten haben.

Mit dem 1-Stichproben t-Test überprüfen wir, wie wahrscheinlich sich unsere erhobenen Daten zu einer Grundgesamtheit mit einem bekannten Mittelwert gehören können. Wie der z-Test setzt der t-Test hinreichend **normalverteilte Daten** voraus. Bevor wir also einen t-Test durchführen dürfen, müssen wir unsere Daten **immer** auf ihre Normalverteilung überprüfen. Dabei erinnern wir uns, dass die Normalverteilung nur für *metrischskalierte* Variablen definiert ist. 

Wie der z-Test prüft der t-Test das Verhältnis zum Zentralmass (dem Mittelwert). Unsere Fragestellung kann also ebenfalls die drei Fälle des Verhältnis zur Nullhypothese annehmen: 

- Die Frage kann auf einen ***ungerichteten*** Unterschied ausgerichtet sein.
- Sie kann ***rechtsgerichtet*** sein bzw. ein ***grösser als*** Verhältnis unterscheiden.
- Sie kann ***linksgerichtet*** sein bzw. ein ***kleiner als*** Verhältnis unterscheiden.

Wir müssen also immer unsere Forschungsfrage überprüfen, welchen dieser Unterschiede zum Zentralmass wir testen. 

Der 1-Stichproben t-Test nimmt als Vergleichsgrösse die t-Verteilung mit dem Mittelwert \\( \bar{x} \\). Wir vergleichen also, in wie weit unsere erhobenen Daten zu dieser theoretischen Verteilung passen. Dabei berücksichtigt der t-Test automatisch unseren Stichprobenumfang.

Besonders elegant können wir t-Tests  mit Hilfe der `t_test()`-Funktion aus der Bibliothek `tidymodels` durchführen.

Wir überprüfen die Funktion des 1-Stichproben t-Test  mit einer virtuellen Studierendenkohorte mit einem Stichprobenumfang von 132, für die wir das Alter erhoben haben. 

```R
stichprobe = tibble(alter = rnorm(132, mean = 26, sd = 3))
```

<div class="alert alert-warning" markdown=1> 
**Achtung** Die folgenden Ergebnisse werden mit jedem Durchlauf unterschiedlich sein, weil die Funktion `rnorm()` immer eine neue Stichprobe erzeugt!
</div>

Zuerst stellen wir sicher, dass unsere Stichprobe hinreichend normalverteilt ist. 

```R
stichprobe %>%
    summarise(
        swt = shapiro.test(alter) %>% pluck("p.value")
    )
```

| swt | 
|:---:|
| 0.9617032 | 

Weil `.962` grösser als `.1` ist, akzeptieren wir, dass die Daten hinreichend normalverteilt sind.

Wir prüfen jetzt, ob diese Stichprobe zu Studierenden im 1. Semester passt. Die langjährige Erfahrung lehrt uns dabei, dass die für Studienbeginnende das Durchschnittsalter bei 25 Jahren liegt. 

Wir fragen zuerst einmal ungerichtet: Unterscheidet sich das Durchschnittsalter unserer Stichprobe vom Durchschnitt der Studienbeginnenden. Die Nullhypothese \\(( H_0 \\) lautet in diesem Fall, dass es *keinen* Unterschied zum Durchschnittsalter von 25 Jahren der Studienbeginnenden gibt. 

Diesen Frage beantworten wir wie folgt:

```R
stichprobe %>% 
    t_test(
        response = alter, 
        mu = 25,
        alternative = "two-sided"
    )
```

An den Parameter `mu` übergeben wir den Mittelwert unserer theoretisch angenommenen Stichprobe. Das Ergebnis lautet wie folgt:

| statistic | t_df  | p_value | alternative | lower_ci | upper_ci | 
|:---:|:---:|:---:|:---:|:---:|:---:| 
| 2.445504 | 131 | 0.01579277 | two.sided | 25.12686 | 26.20102 |

Diese Statistik liefert uns die verschiedenen Kennwerte, für die Bortz und Schuster (2010) die manuellen Rechenwege beschreiben. Unser erster Blick geht zum p-Wert, weil dieser uns bei der Entscheidung hilft, ob das Ergebnis im Ablehnungsbereich der Nullhypothese liegt. Der p-Wert ist `0.016`. Dieser Wert ist kleiner als `.025` für das 95% Konfidenzintervall, so dass wir hier sagen können, dass die vorliegende Stichprobe sich *signifikant*  von einer Kohorte im 1./2. Semester unterscheidet. 

Wir können aber auch gerichtet fragen, ob die Stichprobe älter ist, als die Studierenden am Studienbeginn. Weil *älter* auf eine Grösser-Beziehung verweist, haben wir es hier mit einer **rechtsgerichteten** Hypothese zu tun. Diese Richtung übergeben wir dem Parameter `alternative`. Für rechtsgerichtete Hypothesen übergeben wir daher den Wert `greater`, um die Grösser-Beziehung abzubilden.. 

```R
stichprobe %>% 
    t_test(
        response = alter, 
        mu = 25,
        alternative = "greater"
    )
```

| statistic | t_df  | p_value | alternative | lower_ci | upper_ci | 
|:---:|:---:|:---:|:---:|:---:|:---:| 
| 2.445504 | 131 | 0.007896386 | greater | 25.121419 | Inf |

Hier ist der p-Wert mit `.0079` kleiner als `.01`, so dass wir mit 99% Sicherheit sagen können, dass die Stichprobe *signifikant älter* ist, als eine typische Kohorte von Studienbeginnenden. 

Die umgekehrte Frage können wir auch stellen und überprüfen. In diese Fall hätten wir eine linksgerichtete Hypothese. In diesem Fall ist die Alternativhypothese, dass die erwarteten Werte kleiner als der bekannte Mittelwert sein müssen. Konsequenter Weise übergeben wir in diesem Fall der Testfunktion den Parameter `alternative` den Wert  `"less"`.

```R
stichprobe %>% 
    t_test(
        response = alter, 
        mu = 25,
        alternative = "less"
    )
```

| statistic | t_df  | p_value | alternative | lower_ci | upper_ci | 
|:---:|:---:|:---:|:---:|:---:|:---:| 
| 2.445504 | 131 | 0.9921036 | less | -Inf | 26.11369 | 

Dieses Ergebnis zeigt uns einen p-Wert der eindeutig darauf hinweist, dass wir die Nullhypothese nicht verwerfen dürfen. Wir akzeptieren also, dass die Kohorte **nicht** jünger als eine Vergleichskohorte ist.

<div class="alert alert-warning" markdown=1> 
**Achtung**: Nur weil wir eine Nullhypothese nicht ablehnen können, dürfen wir uns nicht dazu verleiten lassen, das Gegenteil anzunehmen. 

Dass eine Kohorte nicht signifikant jünger ist, schliesst also nicht automatisch ein, dass die Kohorte älter als die Vergleichskohorten sind. 
</div>

$$ $$

