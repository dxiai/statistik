# t-Test zum Stichprobenvergleich


<div class="alert alert-info" markdown=1>
Dieser Abschnitt ergänzt das Kapitel 8.2 und 8.3 in Bortz und Schuster (2010). 
</div>

Der t-Test folgt dem gleichen Prinzip wie der [z-Test](). Es sind also ungerichtete, rechtsgerichtete und linksgerichtete Hypothesentests möglich. Im Gegensatz zum z-Test müssen wir beim t-Test aber nicht die tatsächlich Verteilung der Grundgesamtheit kennen. Dafür "reagiert" der t-Test auf die Grösse unserer Stichproben. 

<div class="alert alert-warning" markdown=1>
Für kleine Stichproben ist es möglich, dass wir für unsere Hypothesen nicht das Signifikanzniveau des  *t*-Tests erreichen können. Das gilt vor Allem dann, wenn die Merkmalsausprägungen breit gestreut sind und die Effektgrösse klein ist (bzw. die Wahrscheinlichkeit eines Fehlers 2. Art gross ist).</div> 

Der t-Test erwartet hinreichend normalverteilte Variablen. Bevor wir also einen t-Test  durchführen müssen wir sicherstellen, dass die Variablen entsprechend normalverteilt sind. Diese Information sollten wir bereits im Schritt der deskriptiven Statistik ermittelt haben. Falls wir unsere Stichprobe in Teilstichproben gliedern, müssen wir den Schritt der deskriptiven Statistik für jede Teilstichprobe durchführen. Damit stellen wir sicher, dass die Variable in allen Stichproben hinreichend normalverteilt ist. 

In der Praxis verwenden wir den t-Test um zwei Stichproben mit einander zu vergleichen. Wenn wir die Daten auswerten liegen die unterschiedlichen Stichproben bereits zusammengefasst in der Form einer Merkmalsvariable und einer erklärenden Variable vor. 

Falls die Daten noch nicht zusammengefasst sind, müssen wir die Daten erst mit `bind_rows()` (s. Datenimport Cheat Sheet) zusammenführen. Dabei beachten wir, dass wir vorher die Zugehörigkeit der Datensätze einfügen. 

Die Nullhypothese des t-Tests ist, dass ***die beiden Stichproben wahrscheinlich aus der gleichen Grundgesamtheit stammen.*** Akzeptieren wir diese Nullhypothese, dann bedeutet das, dass wir keine signifikanten Unterschiede zwischen den Stichproben für die erklärende Variable erkennen konnten. Lehnen wir die Nullhypothese des t-Tests ab, dann bedeutet das einen **signifikanten Unterschied** zwischen den beiden Stichproben. 

Für den t-Test verwenden wir in R die `t_test()` Funktion aus der Bibliothek `tidymodels`. Diese Funktion hat sprechendere Parameter als die alte `t.test()`-Funktion aus der Literatur und dem Internet. Ausserdem liefert uns ein Stichprobenobjekt zurück, dass wir wir gehabt weiterverarbeiten können. Das macht uns das Leben viel einfacher. 

```R
Stichprobe = tibble( 
        id = 1:101,
        a = rnorm(101, mean = 10, sd = 5), 
        b = rnorm(101, mean = 12, sd = 6)
    ) 

Stichprobe %>%
    pivot_longer(c(a, b), names_to = "erklaerend", values_to = "merkmal") %>% 
    drop_na() %>%
    t_test(
        response = merkmal,
        explanatory = erklaerend,
        alternative = "two-sided",
        paired = FALSE
    )
```

Die Beispielstichprobe erzeugen wir uns mit zufällig generierten Daten, wobei wir mit `rnorm()` sicherstellen, dass diese Daten auch normalverteilt sind. Zur Illustration verwende ich hier unterschiedliche Mittelwerte und Standardabweichungen, um die Grundgesamtheit zu simulieren. Nachdem ich die Stichprobe mit `pivot_longer()` in die Form einer erklärenden und einer Merkmalsvariable gebracht habe, rufe ich die `t_test()`-Funktion auf. Dabei  haben die Parameter die folgende Bedeutung: 

- `response` erwartet die Merkmalsvariable. Diese **muss** metrischskaliert und hinreichend normalverteilt sein. 
- `explanatory` erwartet die erklärende Variable. Diese **muss** für den t-Test genau 2 Merkmalsausprägungen haben.
- `alternative` erwartet die Richtung des Tests, wobei `two-sided` für ungerichtete, `less` für linksgerichtete und `greater` für rechtsgerichtete Hypothesen angegeben werden kann. 
- `paired` erwartet einen Wahrheitswert, ob es sich bei den Daten um abhängige (`TRUE`) oder unabhängige (`FALSE`) Stichproben handelt.

Im Beispiel führe ich also einen t-Test für unabhängige Stichproben durch. 
Das Ergebnis für das obige Beispiel ist: 

| statistic | t_df | p_value | alternative  | lower_ci  | upper_ci  | 
|---|---|---|---|---|---| 
| -1.122987 | 193.1518 | 0.2628371 | two.sided | -2.597756 | 0.7128098 |

Bei diesem Ergebnis zeigt uns der p-Wert von `0.2628371`, dass wir ***keinen** signifikanten* Unterschied zwischen den Stichproben erkennen konnten.

### t-Test für gerichtete Hypothesen

Für gerichtete Hypothesen ist es gelegentlich hilfreich, die Reihenfolge der Ausprägungen der erklärenden Variable anzugeben. Das machen wir mit dem `order`-Parameter. Dieser Parameter erwartet einen Vektor mit zwei Werten. Der erste Wert ist die Merkmalsausprägung für die Daten die wir vergleichen möchten und der zweite Wert kennzeichnet die Referenzdaten.

```R
Stichprobe %>%
    drop_na() %>%
    pivot_longer(c(a, b), names_to = "erklaerend", values_to = "merkmal") %>% 
    t_test(
        response = merkmal,
        explanatory = erklaerend,
        alternative = "less",
        paired = TRUE,
        order = c("a", "b") 
    )
```

Dieses Beispiel fragt, ob in einer abhängigen Stichprobe das erklärende Merkmal `b` eine geringere Merkmalsausprägung der `merkmal`-Variable hat als während der Ausgangssituation mit der erklärenden Merkmalsausprägung `b`.

| statistic | t_df | p_value | alternative  | lower_ci  | upper_ci | 
|---|---|---|---|---|---|
| -1.132537 | 100 | 0.1300586 | less | -Inf | 0.4391388 |  

Hier zeigt uns der p-Wert an, dass die Werte mit der Merkmalsausprägung `a` zwar kleiner sind als die mit der Merkmalsausprägung `b`, allerdings ist dieser Unterschied nicht *signifikant* für das 95%-Konfidenzintervall.

<div class="alert alert-warning" markdown=1>
Bei echten Analysen dürfen Sie ***nicht*** zwischen abhängigen und unabhängigen Stichprobenanalysen wechseln!
</div>

<div class="alert alert-success" markdown=1>
Beachten Sie, dass ich im letzten Beispiel das `drop_na()` vor der Umformung der Stichprobe durchführe. Damit stelle ich bei abhängigen Stichproben sicher, dass wir gleich viele Datensätze **und** eine paarweise Bindung für beide Ausprägungen haben.
</div>

<div class="alert alert-success" markdown=1>
In Fragebogenstudien verwenden wir oft die *demographischen Variablen* als erklärende Variablen. Dabei probieren wir mehrere Optionen als erklärende Variable aus. 
</div>
    