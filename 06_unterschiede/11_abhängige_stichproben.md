# Abhängige Stichproben

Neben dem Vergleich nominalskalierter Variablen tauchen in unserem [Entscheidungsbaum](https://moodle.zhaw.ch/mod/page/view.php?id=418609) die beiden Begriffe der ***abhängigen*** und ***unabhängigen Stichprobe*** auf. Die beiden Begriffe sind für die Überprüfung von Unterschieden von ordinalskalierten und metrischskalierten Variablen von Bedeutung. Abhängig von der jeweiligen Art der "Stichprobe" müssen wir andere Tests auswählen, um Unterschiede statistisch zu überprüfen. 

Diese Begriffe sind leicht irritierend, weil wir bisher immer von *Stichproben* als Gesamtheit der Variablen und Sätze gesprochen haben. Ausserdem haben wir *abhängige* und *unabhängige Variablen* kennengelernt. ***Abhängige Stichproben haben weder etwas mit Stichproben noch mit Variablen zu tun.*** 

<div class="alert alert-primary" markdown=1>
**Definition:** Eine Untersuchung von Unterschieden zwischen **abhängigen Stichproben** müssen für die Abhängigkeit zwei Kriterien gelten: 

1. Es werden Zusammenhänge zwischen einer *ordinalskalierten Variable* und einer Variable, die entweder *ordinal* oder *metrisch* skaliert ist, untersucht.
2. Für jede Merkmalsausprägung der unabhängigen Variable existieren Werte in der abhängigen Variable für jede Entität der Ausgangsstichprobe.  
</div>

Aus dieser Definition leitet sich ab, dass es eine Abhängigkeit zwischen den Merkmalsausprägungen der abhängigen Variablen gibt. Diese Abhängigkeit wird über die Ordnung der unabhängigen Variablen festgelegt.

Wie bei [*unabhängigen Stichproben*](https://moodle.zhaw.ch/mod/page/view.php?id=476509) werden bei abhängigen Stichproben die Werte der abhängigen Variablen über die unabhängige Variable *gruppiert*. Für den Unterschied werden hierbei wiederum die Verteilungen der Gruppen verglichen.


<div class="alert alert-success" markdown=1>  
Sie erkennen abhängige Stichproben meistens daran, dass Sie zwei (oder mehr) Variablen mit gleicher Skalierung haben. Oft bilden diese Variablen unterschiedliche Zeitpunkte oder allgemeiner Kontexte ab, die Sie vergleichen wollen. 
Am häufigsten werden Vor- und Nachtests (Pre- und Posttests) in der Qualitätssicherung als abhängige Stichproben umgesetzt.
</div> 

#### Bedingung 1: Ordinalskalierte unabhängige Variable

Die unabhängige Variable *muss* in abhängigen Stichproben ordinalskaliert sein. Es muss möglich sein, die Merkmalsausprägungnen der unabhängigen Variable zu sortieren. Üblich sind dabei Merkmalsausprägungen wie z.B. *vorher*-*nachher* üblich. In konkreten Studien kann die Ordinalskalierung der jeweiligen Variable *verdeckt* sein. Die Reihenfolge muss deshalb in der Dokumentation der Studie im Abschnitt *Methode* festgehalten werden. 

#### Bedingung 2: Alle Entitäten müssen in allen Gruppen abgebildet sein

<div class="alert alert-primary" markdown=1>  
**Abhängige Stichproben** beschreiben den Einfluss von Merkmalsausprägungen einer Variable in einer anderen Gruppe die Ausprägungen der gleichen Variable in einer anderen Gruppe unserer Analyse beeinflussen. 
</div>

Diesen Einfluss können wir nur untersuchen, wenn für jede Entität jeweils eine Merkmalsausprägung in der abhängigen Variable für jede durch die unabhängige Variable gebildete Gruppe vorliegt. Damit liegen die Werte im einfachsten Fall mit zwei Gruppen paarweise vor. Diese Beziehung wird in den Funktionen für die entsprechenden Tests übernommen. 

<p class="alert alert-success" markdown="1"> 
**Merke:** Tests für abhängige Stichproben werden in R mit dem `paired = TRUE` Parameter gekennzeichnet.
</p>

### Besonderheiten bei abhängigen Stichproben

Viele abhängige Stichproben werden in den Rohdaten nicht als abhängige und unabhängige Variablen abgebildet. Stattdessen werden die gepaarten Werte in zwei separaten Variablen erfasst. Für die Analyse muss deshalb die beiden zu untersuchenden Variablen aus den Rohdaten generiert werden. Dazu müssen meistens die Werte aus den Rohdaten in die Langform gebracht werden. 

```
notenspiegel = read_csv2("notenspiegel.csv")
```

Im Beispiel `notenspiegel` liegen die Werte für die Einzelleistungen als separate Variablen  `Luct`, `Lkct` und `Lgct` vor. Die Werte der Variablen `Luct` und `Lkct` beziehen sich auf zeitlich aufeinander folgende Leistungsnachweise aus den Übungen während des Semesters (`Luct`) und der abschliessenden Klausur (`Lkct`). Jeder dieser Werte bezieht sich auf eine teilnehmende Person. Es liegen also die Werte für die Leistungen gepaart aber in der *Breitform* vor. 

Für einen Vergleich kann der Zeitpunkt der Leistungsnachweise als Ordnung für die Skalierung verwendet werden. Damit wir diese Ordnung auch in den Daten abbilden können, müssen die Daten zuerst in die Langform gebracht werden. 

Zur einfacheren Bearbeitung werden aus der Stichprobe nur die Zielvariablen extrahiert. 

<div class="alert alert-warning" markdown=1>
Damit ein paarweiser Vergleich von abhängigen Stichproben funktioniert, müssen uns gleich viele Werte für alle Ausprägungen der erklärenden Variable vorliegen. 
</div>

Damit der Vergleich richtig durchgeführt werden kann, müssen wir ungültige Werte aus der Stichprobe entfernen!

```
notenspiegel %>% 
    select(Luct, Lkct, Lgct) %>% 
    # Ungültige Werte entfernen
    drop_na() %>% 
    filter(Lkct > 1) %>%  
    # Stichprobe in die Langform bringen
    pivot_longer(everything(), names_to = "Zeitpunkt", values_to = "Leistung") -> 
        notenspiegel_lang
```

Das Ergebnis in `notenspiegel_lang` enthält eine Stichprobe mit zwei Variablen: 

- Zeitpunkt (mit `Luct` < `Lkct` < `Lgct`)
- Leistung

Wir können nun die Unterschiede der Leistungen zu den einzelnen Zeitpunkten untersuchen. 

