# Korrelationen und Zusammenhänge


<div class="col-12 alert alert-primary" markdown=1>
<i class="fa fa-lg fa-info-circle"></i>
Dieser Abschnitt ergänzt das Kapitel 10 "Korrelationen" in Bortz und Schuster (2010). 
</div>

Bisher haben wir Unterschiede von Stichproben zum zentralen Lagemass einer gedachten Grundgesamtheit untersucht. Solche Tests arbeiten immer nur mit einer Variable für die Statistik und einer zweiten Variable, die die erste Variable in Teilstichproben gliedert. Oft interessieren uns aber Zusammenhänge, wie z.B.: "Gibt es einen Zusammenhang zwischen der Raumtemperatur und der Arbeitsleistung?" 

In diesem Fall fokussiert die Forschungsfrage auf zwei Variablen. Wir kommen mit den Unterscheidungstests nicht weiter. Stattdessen Fragen wir hier nach dem gemeinsamen Auftreten von Werten in beiden Variablen. Solche Zusammenhänge visualisieren wir oft in Punktdiagrammen. Dabei fällt bei Zusammenhängen auf, dass die Werte entlang der bzw. einer Diagonalen gestreut und nicht über den gesamten Wertebereich der beiden Skalen gestreut sind (Botz & Schuster, 2010, S. 154). 

<div class="alert alert-primary" markdown=1>
Das Mass für Korrelationen sind die sog. Korrelationskoeffizienten. Diese Werten mit *r* gekennzeichnet. 
</div>

### Korrelationen für metrischskalierte und ordinalskalierte Variablen bestimmen

Für Korrelationen bestimmen wir den sog. Korrelationskoeffizienten `r`. In R haben wir dazu eine wichtige Funktion nämlich die Funktion `cor()`. Diese Funktion gibt uns den Korrelationskoeffizienten für zwei Variablen zurück.  

<div class="alert alert-success" markdown=1>
Die `cor()`-Funktion können wir für die Analyse von metrisch- und ordinalskalierten Variablen verwenden. Wir müssen dazu allerdings die korrekte Korrelationsmethode festlegen. 
</div>

* Für metrischskalierte Variablen können wir den *Punkt-Moment-Koeffizienten nach Pearson* (\\( r_{pearson}\\) oder \\( r_p \\)) bestimmen. In diesem Zusammenhang wird oft auch \\( r_\phi \\) verwendet.

Für \\( r_p \\) gilt zusätzlich die Voraussetzung, dass *beide* Variablen *hinreichend normalverteilt* sein müssen.

Falls die Voraussetzungen für \\( r_p \\) nicht erfüllt sind, dann wenden wir  Rangkorrelationsverfahren an. 

**Rangkorrelationsverfahren** dienen analog zu den Wilcoxon- und Mann U-Tests der Identifikation von Zusammenhängen zwischen *ordinalskalierten* Variablen. Analog zum t-Test fallen wir auf diese Verfahren auch bei metrischskalierten Variablen zurück, wenn  die Voraussetzungen für den Punkt-Moment-Koeffizienten ***nicht*** erfüllt sind. 

* Bei hinreichend grossem Stichprobenumfang (n > 50 besser n > 100) verwenden wir die Rangkorrelation nach Spearman ( \\( r_s \\) ) in der Literatur wird diese Korrelation auch als Spearmans \\( \rho \\) bezeichnet.  

* Für kleine Stichproben (n < 50) oder gemischte Vergleiche zwischen metrisch- und ordinalskalierten Variablen nutzen wir die Rangkorrelation nach Kendall  oder Kendalls \\( \tau \\) (\\( r_\tau \\)).

Als Orientierung können wir annehmen, dass Korrelationskoeffizienten < 0.2 auf keine Korrelation hinweisen.

### Korrelationstest

Der Korrelationskoeffizient kann in Grenzfällen nicht aussagekräftig sein. Das ist besonders dann der Fall, wenn wir die Korrelation verwenden um den Effekt einer Variablen auf eine andere zu bestimmen. In solchen Fällen ist eine grobe Abschätzung nicht ausreichen. Besser wäre ein statistischer Kennwert, ob ein erkannter Zusammenhang auch tatsächlich relevant ist. Das ist immer dann von notwendig, wenn die einzelnen Zusammenhänge klein (`r < 0.3`) sind.

<div class="alert alert-success" markdown=1>
Für den Pearson- und den Spearman-Korrelationskoeffizienten `r > 0.3` ist eine statistisch nicht-signifikante Korrelation selten. 
</div>

Nimmt eine unserer Hypothesen einen Zusammenhang an und wir haben nur ein kleinen r-Wert ermittelt (r < 0.3), dann sollten wir sicherheitshalber die Korrelation auf ihre statistische Signifikanz prüfen. Dazu verwenden wir die Funktion `cor.test()`.

<div class="alert alert-success" markdown=1>
Die Nullhypothese des Korrelationstests ist: **Es gibt keine Korrelation zwischen zwei Variablen.**  
</div>

Für den Korrelationstest wählen wir ausnahmsweise ein grosses Konfidenzintervall, weil es sich hier um ein standardisiertes Verfahren handelt. Z.B. 99.9% oder grösser. Bei grossen Stichproben wählen wir für diesen Test besser grosse Konfidenzintervalle, weil mit zunehmender Stichprobengrösse ein tatsächlich existierender Zusammenhang immer deutlicher erkennbar wird.

Beispiel: 

Nehmen wir an, dass für unsere Beispielstichprobe aus den letzten Übungen angenommen wird, dass es einen positiven Zusammenhang zwischen der Nutzung von Messengern (`q18_4`) und der empfundenen Digitalisierung (`q00_1`) gibt. Wir führen den Korrelationstest für die beiden Variablen aus. Weil unsere Hypothese einen positiven Zusammenhang festlegt, handelt es sich um eine gerichtete Hypothese vom Typ "grösser". 

```
daten %>%
    select(q18_4, q00_1) %>%
    drop_na() %>%
    summarise(
        cor= cor.test(q18_4, q00_1, method = "spearman", alternative = "greater") %>% tidy %>% list
    ) %>%
    unnest(cor)
```

| estimate | statistic  | p.value  |
|---|---|---|
| 0.199403 | 4250462 | 0.0001770088 |

`estimate` entspricht hier dem Korrelationskoeffizienten `r`.  Der Wert von `0.199` deutet eine schwache Korrelation an. Der p-Wert von `0.000177` zeigt uns trotzdem eine Signifikanz im 99.9% Konfidenzintervall. Wir können also die Nullhypothese ablehnen und akzeptieren, dass es einen - wenn auch schwachen - Zusammenhang zwischen den beiden Variablen gibt. 


$$ $$