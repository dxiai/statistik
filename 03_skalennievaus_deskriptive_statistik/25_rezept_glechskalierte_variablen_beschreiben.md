# Rezept: Häufigkeiten von mehreren Variablen bestimmen


In vielen Studien müssen die Häufigkeiten für viele gleichskalierte Variablen bestimment werden. 

Das folgende Rezept zeigt Ihnen, wie Sie die Häufigkeiten mehrerer Variablen gegenüberstellen.

<p class="alert alert-success" markdown="1">
In diesem Rezept wird vorausgesetzt, dass Sie die Variablennamen so organisiert haben, dass zusammengehörende Variablen einen gemeinsamen Teil im Vektornamen haben. Ideal ist es, wenn Sie diesen Teil am Anfang oder am Ende des Vektornamens hinzufügen. Ausserdem hilft es Ihnen bei der schnellen Analyse, wenn Sie definierte Trennzeichen für die Namensteile verwenden. In diesem Beispiel verwende ich den Unterstrich (`_`) als solches Trennzeichen.
</p>

Um die Verteilungen von zwei Variablen gegenüberzustellen, verwenden wir einen ähnlichen Trick wie bei der Bestimmung der Kennwerte für alle Variablen. 

```R
stichprobe %>% 
    select(starts_with("q10_1")) %>%
    pivot_longer(everything(), names_to = "variable", values_to = "werte") %>%
    count(variable, werte)
```

<table border="1">
<thead>
	<tr><th scope=col>variable</th><th scope=col>werte</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>q10_1_0</td><td> 3</td><td> 2</td></tr>
	<tr><td>q10_1_0</td><td> 4</td><td> 2</td></tr>
	<tr><td>q10_1_0</td><td> 5</td><td> 3</td></tr>
	<tr><td>q10_1_0</td><td> 6</td><td> 2</td></tr>
	<tr><td>q10_1_0</td><td> 7</td><td> 5</td></tr>
	<tr><td>q10_1_0</td><td> 8</td><td> 3</td></tr>
	<tr><td>q10_1_0</td><td> 9</td><td> 4</td></tr>
	<tr><td>q10_1_0</td><td>10</td><td> 6</td></tr>
	<tr><td>q10_1_1</td><td> 4</td><td> 1</td></tr>
	<tr><td>q10_1_1</td><td> 5</td><td> 1</td></tr>
	<tr><td>q10_1_1</td><td> 7</td><td> 1</td></tr>
	<tr><td>q10_1_1</td><td> 8</td><td> 4</td></tr>
	<tr><td>q10_1_1</td><td> 9</td><td>11</td></tr>
	<tr><td>q10_1_1</td><td>10</td><td> 9</td></tr>
</tbody>
</table>


<br>

Jetzt stehen die Häufigkeiten der Variablen untereinander. Das ist für Vergleiche nicht sehr hilfreich. Falls unsere Variablen alle den gleichen Wertebereich haben, können wir die Verteilung übersichtlicher darstellen.

```R
stichprobe %>% 
    select(starts_with("q10_1")) %>%
    pivot_longer(everything(), names_to = "variable", values_to = "werte") %>%
    count(variable, werte) %>% 
    pivot_wider(
        names_from = werte, 
        names_sort = TRUE,
        values_from = n, 
        values_fill = 0
    )
```


<table border="1">
<thead>
	<tr><th scope=col>variable</th><th scope=col>3</th><th scope=col>4</th><th scope=col>5</th><th scope=col>6</th><th scope=col>7</th><th scope=col>8</th><th scope=col>9</th><th scope=col>10</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>q10_1_0</td><td>2</td><td>2</td><td>3</td><td>2</td><td>5</td><td>3</td><td> 4</td><td>6</td></tr>
	<tr><td>q10_1_1</td><td>0</td><td>1</td><td>1</td><td>0</td><td>1</td><td>4</td><td>11</td><td>9</td></tr>
</tbody>
</table>

<br>

<p class="alert alert-info">
    Falls Sie die Verteilungen mit <code>ggplot</code> visualisieren möchten, dann dürfen Sie die folgende Variante nicht wählen. Stattdessen übergeben Sie das Ergebnis von <code>count()</code> an ggplot. 
</p>

Beispiel für ein Histogramm über alle Untervariablen von `q10_1` mit ggplot.

```R
stichprobe %>% 
    select(starts_with("q10_1")) %>%
    pivot_longer(everything(), names_to = "variable", values_to = "werte") %>%
    count(variable, werte) %>% 
    ggplot(aes(x = werte, y= n, fill = variable)) + 
        geom_col(position = position_dodge2(preserve = "single"))
```

#### Rezeptvariante: Häufigkeiten aller Variablen einer Variablengruppe bestimmen und gegenüberstellen. 

In unserem Bespiel haben wir für jede Untervariable zwei Verteilungen. Die wollen wir gegenüberstellen. Das abschliessende `pivot_wider()` erzeugt uns diese Gegenüberstellung. 

Erstellen Sie sich eine Hilfsvektor für die Gegenüberstellung. Im folgenden Beispiel nutzen wir aus, dass die Variablenname, diesen Wert am Ende kodieren.


```R
stichprobe %>% 
    select(starts_with("q10")) %>%
    pivot_longer(everything(), names_to = "variable", values_to = "werte") %>%
    # Untervariablen und Aspekte trennen.
    mutate(
        aspekt = variable %>% str_extract("_\\d$"),
        # wir wollen nur die Untervariable behalten
        variable = variable %>% str_extract("q10_\\d+")
    ) %>%
    count(variable, aspekt, werte) %>%
    arrange(werte) %>%
    # Gegenüberstellung  
    pivot_wider(
        names_from = werte, 
        values_from = n,
        values_fill = 0      # nicht vorhandene Werte durch 0 ersetzen
    ) %>%
    arrange(variable, aspekt)
```


<table border="1">
<thead>
	<tr><th scope=col>variable</th><th scope=col>aspekt</th><th scope=col>1</th><th scope=col>2</th><th scope=col>3</th><th scope=col>4</th><th scope=col>5</th><th scope=col>6</th><th scope=col>7</th><th scope=col>8</th><th scope=col>9</th><th scope=col>10</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>q10_1</td><td>_0</td><td>0</td><td>0</td><td>2</td><td>2</td><td>3</td><td>2</td><td>5</td><td>3</td><td> 4</td><td> 6</td></tr>
	<tr><td>q10_1</td><td>_1</td><td>0</td><td>0</td><td>0</td><td>1</td><td>1</td><td>0</td><td>1</td><td>4</td><td>11</td><td> 9</td></tr>
	<tr><td>q10_2</td><td>_0</td><td>0</td><td>2</td><td>6</td><td>3</td><td>3</td><td>4</td><td>5</td><td>2</td><td> 1</td><td> 1</td></tr>
	<tr><td>q10_2</td><td>_1</td><td>0</td><td>0</td><td>1</td><td>1</td><td>0</td><td>1</td><td>2</td><td>6</td><td>11</td><td> 5</td></tr>
	<tr><td>q10_3</td><td>_0</td><td>2</td><td>4</td><td>4</td><td>2</td><td>5</td><td>5</td><td>4</td><td>0</td><td> 0</td><td> 1</td></tr>
	<tr><td>q10_3</td><td>_1</td><td>0</td><td>0</td><td>1</td><td>0</td><td>0</td><td>4</td><td>4</td><td>4</td><td> 7</td><td> 7</td></tr>
	<tr><td>q10_4</td><td>_0</td><td>8</td><td>3</td><td>6</td><td>1</td><td>3</td><td>2</td><td>2</td><td>1</td><td> 0</td><td> 1</td></tr>
	<tr><td>q10_4</td><td>_1</td><td>1</td><td>1</td><td>2</td><td>0</td><td>3</td><td>0</td><td>3</td><td>3</td><td> 6</td><td> 8</td></tr>
	<tr><td>q10_5</td><td>_0</td><td>1</td><td>1</td><td>0</td><td>0</td><td>0</td><td>1</td><td>5</td><td>5</td><td> 5</td><td> 9</td></tr>
	<tr><td>q10_5</td><td>_1</td><td>1</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>4</td><td>4</td><td> 5</td><td>12</td></tr>
	<tr><td>q10_6</td><td>_0</td><td>1</td><td>3</td><td>4</td><td>0</td><td>5</td><td>4</td><td>4</td><td>4</td><td> 1</td><td> 1</td></tr>
	<tr><td>q10_6</td><td>_1</td><td>1</td><td>0</td><td>0</td><td>0</td><td>0</td><td>2</td><td>5</td><td>7</td><td> 8</td><td> 4</td></tr>
</tbody>
</table>
