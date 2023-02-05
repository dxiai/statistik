# Rezept: Alle ordinalen und metrischen Variablen beschreiben

Weil wir diese Kennwerte für alle ordinal- und metrischskalierten Variablen bestimen müssen, ist das oben beschriebene Verfahren sehr umständlich, wenn wir viele Variablen haben. Um die Kennwerte für alle Variablen zu bestimmen, wenden wir einen Trick an: Wir wandeln jeden Wert einer Variable in ein *Tupel* (d.h. Wertepaar) aus dem Variablennamen und dem Wert um. Diese Tupel ergeben neue Datensätze. Wir schreiben den Variablennamen in einen neuen Vektor `variable` und die Werte in einen neuen Vektor `werte`. 

Diese Aufgabe übernimmt die Funktion `pivot_longer()` für uns. Dieser Funktion übergeben wir alle Variablen, für die wir die Kennwerte bestimmen müssen. In unserer Beispielstichprobe sind das alle Vektoren, die mit `q10_` beginnen (s. Abschnitt *Variablen und Skalenniveaus*). Im folgenden Beispiel verwende ich die Funktion `starts_with()` aus der Bibliothek [`tidyselect`](https://tidyselect.r-lib.org/articles/syntax.html). Diese Bibliothek wird automatisch mit der `tidyverse`-Bibliothek geladen.

Es ist entscheidend, dass wir nach der Umorganisation der Vektoren die Stichprobe mit `drop_na()` bereinigen. Der vorangestellte Aufruf von `select()`, stellt sicher, dass wir nicht Daten verlieren, nur weil andere nominalskalierte Variablen nicht vorhanden sind. Anschliessend gruppieren wir die Werte für jede Variable und führen die Aggregation von oben aus.

<p class="alert alert-success" markdown="1">
Die folgenden beiden Schritte lassen sich für alle Variablen der gleichen Skalenniveaus vereinfachen, wenn Sie **vor** Ihrer Studie die Skalenniveaus Ihrer Vektoren in einer Tabelle dokumentiert haben. Am Ende dieser Seite finden ein vollständiges Beispiel für die entsprechende SVerallgemeinerung.
</p>

```R
stichprobe %>%
    pivot_longer(starts_with("q10_"), names_to = "variable", values_to = "werte") %>% 
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

        mad = mad(werte, constant = 1)
    ) -> kennwerte

kennwerte
```   

<table border="1">
<thead>
	<tr><th scope=col>variable</th><th scope=col>n</th><th scope=col>min</th><th scope=col>max</th><th scope=col>bw</th><th scope=col>iqr</th><th scope=col>q1</th><th scope=col>md</th><th scope=col>q3</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>q10_1_0</td><td>27</td><td>3</td><td>10</td><td>7</td><td>3.5</td><td>5.5</td><td>7</td><td> 9.0</td></tr>
	<tr><td>q10_1_1</td><td>27</td><td>4</td><td>10</td><td>6</td><td>1.5</td><td>8.5</td><td>9</td><td>10.0</td></tr>
	<tr><td>q10_2_0</td><td>27</td><td>2</td><td>10</td><td>8</td><td>4.0</td><td>3.0</td><td>5</td><td> 7.0</td></tr>
	<tr><td>q10_2_1</td><td>27</td><td>3</td><td>10</td><td>7</td><td>1.0</td><td>8.0</td><td>9</td><td> 9.0</td></tr>
	<tr><td>q10_3_0</td><td>27</td><td>1</td><td>10</td><td>9</td><td>3.0</td><td>3.0</td><td>5</td><td> 6.0</td></tr>
	<tr><td>q10_3_1</td><td>27</td><td>3</td><td>10</td><td>7</td><td>2.5</td><td>7.0</td><td>9</td><td> 9.5</td></tr>
	<tr><td>q10_4_0</td><td>27</td><td>1</td><td>10</td><td>9</td><td>4.0</td><td>1.0</td><td>3</td><td> 5.0</td></tr>
	<tr><td>q10_4_1</td><td>27</td><td>1</td><td>10</td><td>9</td><td>4.0</td><td>6.0</td><td>9</td><td>10.0</td></tr>
	<tr><td>q10_5_0</td><td>27</td><td>1</td><td>10</td><td>9</td><td>3.0</td><td>7.0</td><td>9</td><td>10.0</td></tr>
	<tr><td>q10_5_1</td><td>27</td><td>1</td><td>10</td><td>9</td><td>2.0</td><td>8.0</td><td>9</td><td>10.0</td></tr>
	<tr><td>q10_6_0</td><td>27</td><td>1</td><td>10</td><td>9</td><td>4.0</td><td>3.0</td><td>6</td><td> 7.0</td></tr>
	<tr><td>q10_6_1</td><td>27</td><td>1</td><td>10</td><td>9</td><td>2.0</td><td>7.0</td><td>8</td><td> 9.0</td></tr>
</tbody>
</table>

<br>

Das Resultat ist eine ordentliche Tabelle aller zentralen Kennwerte für ausgewählten Variablen. Die Spalte `n` enthält immer den jeweiligen *Variablenumfang*, da wir fehlende Werte mit `drop_na()` vorher entfernt haben.

### Spezialfall: Ordinalskalierte Variablen

Bei ordinalskalierten Variablen werden alle Kennwerte, die sich auf den Median beziehen *etwas* anders berechnet und gerundet als für metrischskalierte Variablen. R kennt 8 verschiedene Berechnungsvarianten für den Median und die Quantile für metrisch- und ordinalskalierte Variablen. Diese Berechnungsvarianten werden über den Parameter `type` der entsprechenden Funktionen (ausser `mad()`)  angesprochen. In der Praxis verwenden wir für metrischskalierte Variablen den Standardwert für `type` und können diesen Parameter weglassen. 

Für ordinalskalierte Variablen **müssen** wir den Parameter angeben. In Frage kommen dann die Werte `1`, `2` oder `3`.  Wir müssen für alle Berechnungen den **gleichen** Typ verwenden. Da für unsere Zwecke der Typ irrelevant ist, wählen Sie standardmässig den Wert `1`.

Der obige Code für metrischskalierte Variablen ändert sich dann wie folgt. 

```R
stichprobe %>%
    pivot_longer(starts_with("q10_"), names_to = "variable", values_to = "werte") %>% 
    select(variable, werte) %>%
    drop_na() %>%
    group_by(variable) %>%
    summarise(
        n = n(),
        min = min(werte),
        max = max(werte),
        bw = max - min,
        
        iqr = IQR(werte, type = 1),
        
        q1 = quantile(werte, .25, type = 1),
        md = median(werte, type = 1), 
        q3 = quantile(werte, .75, type = 1),

        mad = mad(werte, constant = 1)
    ) -> kennwerte
```

#### Sonderfall `mad()`

Die `mad()` Funktion führt aus Kompatibilitätsgründen eine Skalierung aus. Diese Skalierung dürfen wir in der *deskriptiven Statisktik **nicht** anwenden. 

Daher müssen wir der Funktion `mad()` **immer** den Parameter `constant = 1` übergeben.  

```R
stichprobe %>%
    pivot_longer(starts_with("q10_"), names_to = "variable", values_to = "werte") %>% 
    select(variable, werte) %>%
    drop_na() %>%
    group_by(variable) %>%
    summarise(
        n = n(),
        min = min(werte),
        max = max(werte),
        bw = max - min,
        
        iqr = IQR(werte, type = 1),
        
        q1 = quantile(werte, .25, type = 1),
        md = median(werte, type = 1), 
        q3 = quantile(werte, .75, type = 1),

        mad = mad(werte, constant = 1)
    ) -> kennwerte
```

### Deskriptive Statistik super einfach 

Falls Sie die Skalenniveaus Ihrer Variablen in einer Tabelle dokumentiert haben, dann können Sie die deskriptive Statistik stark vereinfachen. 

Im folgenden Beispiel nehmen wir an, dass in der Variable `infos` Ihre Variableninformationen stehen und Sie im Vektor `varname` den Variablennamen aus Ihrem Stichprobenobjekt festgehalten haben und im Vektor `niveau` das entsprechende Skaleniveau. 

```R
# Deskriptive Statistik für die ordinalskalierten Variablen
ordinaleVariablenNamen = infos %>% filter(niveau == "ordinal") %>% pull(varname)
stichprobe %>%
    select( all_of(ordinameVariablenNamen) ) %>%
    pivot_longer(everything()) %>%
    drop_na() %>% # oder eine andere Operation, um ungültige Werte zu filtern
    group_by(name) %>% # die Variablen einzeln auswerten. WICHTIG!
    summarise(
        n = n(),

        min = min(value),
        max = max(value),
        bw = max - min, 

        iqr = IQR(value, type = 1),
        q1 = quantile(value, .25, type = 1),
        md = median(value, type = 1),
        q3 = quantile(value, .75, type = 1),

        mad = mad(value, constant = 1)
    ) 

# Deskriptive Statistik für die metrischskalierten Variablen

metrischeVariablenNamen = infos %>% filter(niveau == "metrisch") %>% pull(varname)

stichprobe %>%
    select( all_of(metrischeVariablenNamen) ) %>%
    pivot_longer(everything()) %>%
    drop_na() %>% # oder eine andere Operation, um ungültige Werte zu filtern
    group_by(name) %>% # die Variablen einzeln auswerten. WICHTIG!
    summarise(
        n = n(),

        min = min(value),
        max = max(value),
        bw = max - min, 

        mw = mean(value),
        sd = sd(value),

        iqr = IQR(value),
        q1 = quantile(value, .25),
        md = median(value),
        q3 = quantile(value, .75),

        mad = mad(value, constant = 1)
    ) 

```