# Textvergleiche und Zusammenhänge

Neben der Auswertung von Texten auf Zusammenhänge zwischen Variablen, möchten wir Texte untereinander vergleichen. In diesem Fall untersuchen wir Korrelationen und Unterschieden zwischen Texten. Weil wir Worte untersuchen, ergibt sich automatisch, dass wir nominalskalierte Variablen vorliegen haben. Um diese Unterschiede zu untersuchen verwenden deshalb den \\(\chi^2\\)-Test. 

<p class="alert alert-success" markdown="1">
Auf dieser Seite werden die Grundkonzepte auf Basis unkodierten Texten beschrieben. Die Vorgehensweisen lassen sich aber leicht auf die Analyse von kodierten Texten übertragen. 
</p>

### Vorbereitungen

Bereiten wir dafür unsere Arbeitsumgebung vor. 

```
library(tidyverse)
library(tidymodels)
library(tidytext)

stopwords_DE = tibble(word = stopwords::stopwords("de", source = "stopwords-iso"))
```

Wir untersuchen die unkodierten Texte im Verzeichnis `text` unserer [Beispieldaten](https://moodle.zhaw.ch/mod/resource/view.php?id=485346).

```
verzeichnis = "text/"

tibble(
    datei = list.files(verzeichnis)
) -> Dateien

Dateien %>% 
    group_by(datei) %>% 
    mutate(
        rohtext = read_file(str_c(verzeichnis, datei))
    ) %>% 
    ungroup() -> rohtexte 

rohtexte %>% 
    unnest_tokens(word, rohtext) %>% 
    anti_join(stopwords_DE) -> worte
```

### Vergleich über alle Texte

Wir fragen uns, ob sich unsere Texte inhaltlich unterscheiden. Dazu führen wir einen  \\( \chi^2\\)-Test über alle Texte auf Wortebene durch.

```
worte %>% 
    chisq_test(word ~ datei) 
```

| statistic | chisq_df | p_value |
| --- | --- | --- | 
| 2579.955 | 2100 | 2.309982e-12 | 

Wir erkennen nun am p-Wert von ~ `2.31e-12`, dass es einen statistisch signifikanten Unterschied zwischen den Texten gibt. Das hilft uns leider nicht viel weiter, weil wir nur irgendeinen Unterschied gefunden haben. Deshalb müssen wir den Test *paarweise* wiederholen. 

Dazu erstellen wir eine Liste allen Dateipaaren, die wir untersuchen möchten. 

```
Dateien %>% 
    expand( d1 = datei, d2 = datei ) %>% 
    filter( d1 < d2 ) -> Dateipaare
```

Damit haben wir alle eindeutigen Textpaare und stellen so sicher, dass wir keine unnötigen Tests durchführen. Mit dieser Liste können wir nun den Test paarweise wiederholen. 

```
Dateipaare %>%
    group_by(d1, d2) %>% 
    mutate(
        chi2 = worte %>% filter(datei %in% c(d1, d2)) %>% chisq_test( word ~ datei ) %>% list()
    ) %>% 
    ungroup() %>% 
    unnest(chi2) %>% 
    mutate(
        sigP = p_value < .05
    )
```

| d1<br>&lt;chr&gt; | d2<br>&lt;chr&gt; | statistic<br>&lt;dbl&gt; | chisq_df<br>&lt;int&gt; | p_value<br>&lt;dbl&gt; | sigP<br>&lt;lgl&gt; |
|---|---|---|---|---|---|
| marketing_1.txt | marketing_2.txt | 213.6767 | 225 | 6.954802e-01 | FALSE | 
| marketing_1.txt | marketing_3.txt | 604.8302 | 442 | 3.878489e-07 | TRUE |
| marketing_1.txt | marketing_4.txt | 404.1803 | 321 | 1.101188e-03 | TRUE |
| marketing_2.txt | marketing_3.txt | 607.6427 | 447 | 5.894482e-07 | TRUE |
| marketing_2.txt | marketing_4.txt | 403.8190 | 323 | 1.472887e-03 | TRUE |
| marketing_3.txt | marketing_4.txt | 696.1779 | 504 | 2.665061e-08 | TRUE |

Der Vektor `sigP` zeigt uns an, ob der Unterschied im 95% Konfidenzintervall signifikant ist. Das hilft uns, die Daten schneller zu interpretieren. 

Wir erkennen nun leicht, dass sich nur beiden Texte in den Dateien `marketing_1.txt` und `marketing_2.txt` ähneln, weil die Unterschiede nicht signifikant sind. Alle anderen Texte unterscheiden sich in der Wortwahl.

### Inhaltliche Interpretation

Auf der Wortebene unterscheiden sich die Texte also. Wir fragen uns nun, ob die Texte inhaltlich weit auseinander liegen oder nur andere Worte gewählt haben. Für die Analyse von Texten ist es zusätzlich interessant, die Texte zu vergleichen. 

Beginnen wir mit den absoluten Werten und bestimmen die Häufigkeiten der Worte in unseren Dokumenten. Die folgende visualisierte Auswertung zeigt die Häufigkeiten der Worte, die in mindestens 2 Dokumenten und mindestens 3 Mal insgesamt im Korpus auftreten. Damit isolieren wir Worte, die zwar absolut gesehen häufiger vorkommen aber sich nicht für den Vergleich der Texte eignen, weil sie nur in einem Text verwendet werden. 

```
worte %>% 
    mutate(
        word = word %>% factor() %>% fct_infreq() %>% fct_rev()
    ) %>% 
    count(datei, word) %>% 
    group_by(word) %>% 
    mutate(
        total = sum(n),           # finde die gesamtnennung der worte
        dateien = length(datei)   # finde worte, die nur in einem Text vorkommen
    ) %>% 
    ungroup()  %>% 
    # entfernt Worte, die nur in einem Dokument oder sehr selten vorkommen. 
    filter(total > 2, dateien > 1) %>% 
    ggplot(aes(word, n, fill = datei)) + 
        geom_col() + 
        coord_flip()
```
[![Wort Liste nach häufigkeiten](https://github.com/dxiai/statistik/raw/main/bilder/text_analyse/wordcount_fixed.png)](https://github.com/dxiai/statistik/raw/main/bilder/text_analyse/wordcount_fixed.png)

Wir haben nun einen ersten Überblick über die Verteilung der Worte. Das Problem dieser Auswertung ist, dass die Länge der Texte vernachlässigt wird. Dadurch werden die Ergebnisse verzerrt. Verbessern wir die Auswertung so, dass wir die relativen Häufigkeiten verwenden. Um die unterschiedlichen Textlängen auszugleichen, müssen wir die relativen Häufigkeiten verwenden. Weil wir mehrere Texte vergleichen möchten, berechnen wir zwei relative Grössen: 

1. Der relative Anteil eines Worts im jeweiligen Quelltext.
2. Der relative Anteil eines Worts im gesamten Korpus. 

Dadurch wird der Code etwas länger aber kaum komplizierter. 

```
worte %>% 
    # anti_join(selteneWorte) %>% 
    mutate(
        word = word %>% factor() %>% fct_infreq() %>% fct_rev()
    ) %>% 
    count(datei, word) %>% 
    mutate(
            worte_gesamt = sum(n)
    ) %>%

    group_by(datei) %>% 
    mutate(
        total_datei = sum(n)
    ) %>%
    ungroup() %>%

    group_by(word) %>% 
    mutate(
        total = sum(n),           # finde die gesamtnennung der worte
        dateien = length(datei),  # finde worte, die nur in einem Text vorkommen
        anteil_total = total / worte_gesamt,
    ) %>% 

    group_by(datei) %>%
    mutate(
        anteil_datei = n / total_datei, 
    ) %>%
    ungroup()  -> hWorte 
```

Die relativen Werte sind nun in der Variablen `hWorte` gespeichert. 

```
hWorte %>% 
    # entfernt Worte, die nur in einem Dokument oder sehr selten vorkommen.
    filter(total > 2, dateien > 1) %>%
    ggplot(aes(word, anteil_datei, fill = datei)) + 
        geom_col() + 
        coord_flip()
```
[![Wort Liste nach relativen Häufigkeiten](https://github.com/dxiai/statistik/raw/main/bilder/text_analyse/wordcount_relativ.png)](https://github.com/dxiai/statistik/raw/main/bilder/text_analyse/wordcount_relativ.png)

Die neue Visualisierung verwendet die absolute Häufigkeit der Worte zur Sortierung der y-Achse und die relativen Häufigkeiten zur Darstellung der Balken. Wir erkennen nun besser, den tatsächlichen Stellenwert der einzelnen Worte im gesamten Korpus. 

Eine andere interessante Gegenüberstellung der ausgewerteten Texte erreichen wir durch das Gegenüberstellen der gesamten Häufigkeit (globale Häufigkeit) zu den Häufigkeiten in den einzelnen Texten (lokale Häufigkeit). 

```
hWorte %>%
    select(datei, word, anteil_datei, anteil_total) %>% 
    ggplot(aes(x = anteil_total, y = anteil_datei, color = datei, label = word)) +
        geom_jitter() + 
        geom_text(check_overlap = TRUE, vjust = 1.5)
```
 
[![Gegenüberstellung der relativen Häufigkeiten](https://github.com/dxiai/statistik/raw/main/bilder/text_analyse/wort_vergleich_punkte.png)](https://github.com/dxiai/statistik/raw/main/bilder/text_analyse/wort_vergleich_punkte.png)

Diese Gegenüberstellung stellt jedes Wort durch einen Punkt dar. Einzelne dieser Worte lassen wir uns im Plot darstellen, wenn Sie sich nicht zu sehr mit anderen Worten in der gleichen Region des Plots überlappen. Wir können nun erahnen, welche inhaltlichen Abweichungen die Texte haben, die wir im \\( \chi^2 \\)-Test bereits empirisch festgestellt haben. Diese diese Unterschiede können wir besser hervorheben, indem wir die Worte der Texte einzeln darstellen. 

```
hWorte %>%
    select(datei, word, anteil_datei, anteil_total) %>% 
    ggplot(aes(x = anteil_total, y = anteil_datei, color = datei, label = word)) +
        geom_jitter() + 
        geom_text(check_overlap = TRUE, vjust = 1.5) + 
        facet_wrap(vars(datei)) +
        theme(legend.position = "none" )
```

[![Gegenüberstellung der relativen Häufigkeiten](https://github.com/dxiai/statistik/raw/main/bilder/text_analyse/wort_vergleich_punkte_getrennt.png)](https://github.com/dxiai/statistik/raw/main/bilder/text_analyse/wort_vergleich_punkte_getrennt.png)

Wir erkennen nun die Gemeinsamkeiten und Unterschiede der vier Texte gut und können die inhaltliche Bedeutung des \\( \chi^2 \\)-Tests besser interpretieren. 

Leider sind nicht alle Punkte beschriftet. Deshalb können die Visualisierungen nicht für sich stehen bleiben. Es muss zusätzlich eine Tabelle angegeben werden, aus der wir alle Worte in unserem Ergebnis erkennen können. 

```
hWorte %>%
    filter(total > 2, dateien > 1)  %>%
    mutate(
        # Ergebnisse. für die Tabellendarstellung runden
        anteil_total = anteil_total %>% round(3),
        anteil_datei = anteil_datei %>% round(3),
    ) %>% 
    select(datei, word, anteil_datei, anteil_total) %>% 
    pivot_wider(
        names_from = datei, 
        values_from = anteil_datei, 
        values_fill = 0
    ) %>% 
    # Ergebnisse entsprechend der Häugikeiten über alle Texte sortieren
    arrange(desc(across(starts_with("marketing"))))
```

| word<br>&lt;fct&gt; | anteil_total<br>&lt;dbl&gt; | marketing_1.txt<br>&lt;dbl&gt; | marketing_2.txt<br>&lt;dbl&gt; | marketing_3.txt<br>&lt;dbl&gt; | marketing_4.txt<br>&lt;dbl&gt; |
|---|---|---|---|---|---|
| marketing | 0.026 | 0.058 | 0.034 | 0.004 | 0.038 |
| cas | 0.016 | 0.052 | 0.040 | 0.002 | 0.000 | 
| npo | 0.008 | 0.029 | 0.023 | 0.000 | 0.000 | 
| direkt | 0.005 | 0.023 | 0.011 | 0.000 | 0.000 | 
| npos | 0.008 | 0.017 | 0.034 | 0.000 | 0.000 | 
| mitstudierenden | 0.004 | 0.017 | 0.006 | 0.000 | 0.000 | 
| digital | 0.006 | 0.012 | 0.011 | 0.006 | 0.000 | 
| praxisbezug | 0.005 | 0.012 | 0.011 | 0.000 | 0.004 | 
| umsetzen | 0.003 | 0.012 | 0.006 | 0.000 | 0.000 | 
| netzwerk | 0.003 | 0.012 | 0.006 | 0.000 | 0.000 | 
| gelernt | 0.003 | 0.012 | 0.006 | 0.000 | 0.000 | 
| fürs | 0.003 | 0.012 | 0.006 | 0.000 | 0.000 | 
| elemente | 0.003 | 0.012 | 0.006 | 0.000 | 0.000 | 
| austausch | 0.004 | 0.012 | 0.000 | 0.004 | 0.000 | 
| erfahrungen | 0.004 | 0.006 | 0.017 | 0.000 | 0.000 | 
| somit | 0.004 | 0.006 | 0.011 | 0.002 | 0.000 | 
| highlights | 0.003 | 0.006 | 0.011 | 0.000 | 0.000 | 
| gelang | 0.003 | 0.006 | 0.011 | 0.000 | 0.000 | 
| digitale | 0.008 | 0.006 | 0.006 | 0.015 | 0.000 | 
| wichtig | 0.003 | 0.006 | 0.006 | 0.002 | 0.000 | 
| grundsätzlich | 0.003 | 0.006 | 0.006 | 0.000 | 0.004 | 
| digitalisierung | 0.008 | 0.006 | 0.000 | 0.017 | 0.000 | 
| operativen | 0.003 | 0.006 | 0.000 | 0.000 | 0.008 | 
| verschiedene | 0.004 | 0.000 | 0.011 | 0.004 | 0.000 | 
| praxis | 0.004 | 0.000 | 0.011 | 0.000 | 0.008 | 
| digitalen | 0.005 | 0.000 | 0.006 | 0.006 | 0.004 | 
| lassen | 0.004 | 0.000 | 0.000 | 0.006 | 0.004 | 
| prozesse | 0.003 | 0.000 | 0.000 | 0.004 | 0.004 | 
| genau | 0.003 | 0.000 | 0.000 | 0.004 | 0.004 | 
| gehört | 0.003 | 0.000 | 0.000 | 0.004 | 0.004 | 
| herausforderungen | 0.005 | 0.000 | 0.000 | 0.002 | 0.015 |
| wichtige | 0.003 | 0.000 | 0.000 | 0.002 | 0.008 | 

Diese Aufstellung gibt uns nun ein differenziertes Bild, um die Ergebnisse des \\( \chi^2 \\)-Tests inhaltlich zu interpretieren. 

> Welche Rückschlüsse ziehen Sie aus diesen qualitativen Daten?

$$ $$