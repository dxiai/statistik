# Zentrales Grenzwerttheorem oder Mittelwertsatz

<div class="col-12 alert alert-info" markdown=1>
Dieser Abschnitt bezieht sich auf das *zentrale Grenzwerttheorem* (s. Bortz & Schuster, 2010, S. 86)
</div>

Weil unsere Daten aus mehr oder weniger zufälligen Stichproben stammen, müssen wir *hinter* die Daten blicken, um Rückschlüsse aus gemessenen Unterschieden abzuleiten. 

Das *zentrale Grenzwerttheorem* oder *Mittelwertsatz* beschreibt ein besonderes Phänomen, dass für die Untersuchung unserer Daten von Bedeutung ist: 

<p class="alert alert-primary" markdown="1">
Die Mittelwerte von Zufallsstichproben der gleichen Grundgesamtheit sind normalverteilt.
</p>

Dieses Prinzip ist deshalb wichtig, weil wir dadurch Werte auf ihrer Zugehörigkeit zu einer Grundgesamtheit untersuchen können, ohne die Verteilung der Stichprobe berücksichtigen zu müssen. D.h. die Verteilung der Grundgesamtheit und der Stichproben kann unter diesen Voraussetzungen vernachlässigt werden.

Das zentrale Grenzwerttheorem wird durch zwei Parameter beeinflusst.

1. Dem Stichprobemumfang.
2. Der Anzahl der Stichproben.

<p class="alert alert-warning" markdown="1">
In der Praxis ist die Anzahl der Zufallsstichproben oft stark beschränkt. Häufig liegen uns nur zwei oder drei Zufallsstichproben vor. In solchen Fällen kommt dem Stichprobenumfang eine grössere Bedeutung zu. 
</p>

<p class="alert alert-success" markdown="1">
**Merke** Je kleiner der Stichprobenumfang, desto breiter sind die Mittelwerte der Zufallsstichproben gestreut. 
</p>

Das folgende Beispiel *simuliert* jeweils 1000 Zufallsstichproben für die Stichprobenumfänge `10`, `100` und `1000` aus der *gleichverteilten Grundgesamtheit*  zwischen `1` und `10`. Anschliessend werden die Mittelwerte dieser Stichproben als Histogramm gegenübergestellt. 

```R
tibble(nr = 1:1000) %>%
    group_by(nr) %>%
    mutate(
        umfang_10 = runif(10, 1,10) %>% mean(),
        umfang_100 = runif(100, 1,10) %>% mean(),
        umfang_1000 = runif(1000, 1,10) %>% mean(),
    ) %>%
    pivot_longer(starts_with("umfang_10")) %>%
    ggplot(aes(value, fill = name)) +
        geom_histogram(position = "dodge")
```

![Darstellung der Mittelwertverteilungen von 1000 Zufallsstichproben aus der gleichverteilten Grundgesamtheit 1-10 für Stichprobenumfänge 10, 100 und 1000](https://github.com/dxiai/ct-resourcen/raw/master/statistik/bilder/zentraler_grenzwertsatz.png)

<div class="alert alert-secondary" markdown="1">
#### Übungen

1. Simulieren Sie 100 Mittelwerte wie oben für eine *normalverteilte Grundgesamtheit* mit dem Mittelwert von `5` und der Standardabweichung von `2` bei einem Stichprobenumfang von `50`.
2. Simulieren Sie 1000 Mittelwerte wie oben für eine *exponentialverteilte Grundgesamtheit* mit einer Abfallrate von `0.5` bei einem Stichprobenumfang von `250`. 
</div>

$$ $$