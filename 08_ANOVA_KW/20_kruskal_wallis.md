# Kruskal-Wallis-Test: ANOVA-Alternative für ordinale Skalenniveaus

Für die ANOVA gelten die gleichen Voraussetzungen wie für den t-Test. Wir dürfen daher keine ANOVA für ordinalskalierte Daten durchführen.

Für ordinaleskalierte Variablen, bei der die unabhängige Variable mehr als zwei Merkmalsausprägungen hat, verwenden wir den Kruskal-Wallis-Test (`kruskal.test()`). Die Logik dieses Tests entspricht der Logik der ANOVA und wir interpretieren das Ergebnis analog.

Der Aufruf des Kruskal-Wallis-Test ist analog zur `aov()`-Funktion. 

Beispiel:

```R
daten %>% 
    kruskal.test(q16_10 ~ q00_1, data = .) %>% 
    tidy() 
```

Wenn der Kruskal-Wallis-Test signifikant ist, kontrollieren wir wie bei der ANOVA die paarweisen Unterschiede mit dem Wilcoxon- bzw. Mann U-Test. Für diesen Test steht uns für diese Aufgabe die Funktion `pairwise.wilcox.test()` zur Verfügung. Diese Funktion akzeptiert übrigens alle Parameter, wie die normale `wilcox.test()`-Funktion, so dass wir unsere Hypothesen entsprechend formulieren können. 

Beispiel: 

```R
daten %>% 
    summarise(
        pwwilcox = pairwise.wilcox.test(q16_10, q00_1, data = .) %>% tidy() %>% list
    ) %>% 
    unnest(pwwilcox)
```
