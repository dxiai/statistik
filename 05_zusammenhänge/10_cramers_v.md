# Nominalskalierte Variablen: Kontingenzkoeffizient und Cramers V


<div class="col-12 alert alert-primary" markdown=1>
<i class="fa fa-lg fa-info-circle"></i>
Dieser Abschnitt ergänzt das Kapitel 10 "Korrelationen" in Bortz und Schuster (2010). 
</div>

### Nominalskalierte Variablen

Nominalskalierte Variablen können wir mit der `cor()`-Funktion nicht analysieren. Damit wir Forschungsfragen für solche Variablen beantworten können, müssen wir auf andere Werkzeuge zurückgreifen. Im Skript aus den vergangenen Semestern wird nur auf den *korrigierten Konfidenzkoeffizienten* hingewiesen. Dieser ist allerdings problematisch (Bortz & Schuster, 2010, S. 180). 

#### Korrigierter Kontingenzkoeffizient

Der *korrigierte Kontingenzkoeffizient*  leitet direkt aus dem \\( \chi^2 \\)-Unabhängigkeitstest ab. Tatsächlich misst dieser Test die *Korrelation* von zwei nominalskalierten Namen. In diesem Fall fällt der Unterscheidungstest mit der Untersuchung von Zusammenhängen zusammen.

Der Kontingenzkoeffizient berechnet sich wie auf Seite 180 in Bortz und Schuster (2010) beschrieben. Diese Formel können wir in R übersetzen. Dazu wenden wir das Konzept der Problemzerlegung an.

Der Wert für \\( \chi^2 \\) entspricht der Vektor `statistic` aus dem Ergebnis der `chisq_test()`-Funktion. Den Stichprobenumfang `n` haben wir bereits bestimmt. Für diesen Fall müssen wir aber unbedingt darauf achten, dass wir `n` immer für die bereinigte Stichprobe bestimmen, die wir auch für den \\( \chi^2 \\)-Test verwendet haben.

Daraus ergibt sich die folgende Logik: 

```
bereinigteDaten %>%
    chisq_stat(response = variable1, explanatory = variable2) -> chi2

bereinigteDaten %>%
    count() %>%
    pull(n) -> n

K_i = sqrt(chi2 / (chi2 - n))
```

### Cramers V

Der Kontingenzkoeffizient erlaubt nur eine ungerichtete Korrelation oder wie in Bortz und Schuster (2010) beschrieben wird (S. 180), die gerichteten Unterschiede einer einzelnen Merkmalsausprägung  festzustellen. Ausserdem lässt sich der Konfidenzkoeffizient nicht mit den Korrelationstests vergleichen. 

In Bortz und Schuster (2021) wird zur Analyse der Korrelation von nominalskalierten Variablen auf den **Cramer Index (CI)** hingewiesen. Dieser Wert wird in den meisten anderen Werken als ***Cramers V*** (\\( r_V \\)) bezeichnet. 

<div class="alert alert-info" markdown=1>
Der Vorteil von Cramers V ist für uns, dass wir Werte erhalten, die wir wie die anderen Korrelationen interpretieren können.
</div>

Wie der Kontingenzkoeffizient basiert Cramers V auf der Teststatistik des \\( \chi^2 \\)-Tests. Zusätzlich hängt Cramers V von der Anzahl der Merkmalsausprägungen ab, die im Test untersucht wurden (Bortz und Schuster, 2010, S. 180). Oft entspricht das der Anzahl der Merkmalsausprägungen der Stichprobe. Wenn wir aber Merkmalsausprägungen mit `fct_lump()` zusammenfassen, dann entspricht diese Anzahl den *resultierenden Merkmalsausprägungen*. Mit anderen Worten den Zeilen und Spalten unserer Kontingenztabelle. Für Cramers V müssen wir den kleineren der beiden Werte bestimmen, um den Wert `R` aus der Formel im Lehrbuch zu erhalten. 

Für den einfachen Fall *ohne* `fct_lump()` kommen wir mit der folgenden Logik auf die Zeilen- und Spaltenzahl:

```
bereinigteDaten %>%
     count(variable1) %>%
     count() %>%
     pull(n) -> n_zeilen

bereinigteDaten %>%
     count(variable2) %>%
     count() %>%
     pull(n) -> n_spalten

R = min(n_zeilen, n_spalten) 
```
Daraus leitet sich Cramers V ab: 

```
r_V = sqrt( chi2/ ( n*(R- 1) ) )
```

Hier beachten wir, dass die kleinste Kontingenztabelle mindestens 2 Spalten und 2 Zeilen hat. R ist also immer mindestens 2. 

<div class="alert alert-success" markdown=1>
Wir bestimmen Cramers V nur für Variablen, für die wir bereits einen dringenden Verdacht auf einen Zusammenhang haben. Dieser Verdacht ist in der Regel ein signifikanter \\( \chi^2 \\)-Unabhängigkeitstest. 
</div>

$$ $$