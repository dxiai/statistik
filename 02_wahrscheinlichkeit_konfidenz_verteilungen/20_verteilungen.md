## Verteilungen und Wahrscheinlichkeit

<div class="col-12 alert alert-info" markdown=1>
<i class="fa fa-lg fa-info-circle"></i><br>
Dieser Abschnitt gibt einen groben Überblick zu den wichtigsten *statistischen* Verteilungen für metrische Skalenniveaus.

Das Thema wird ausführlich im **Kapitel 5** in Bortz und Schuster (2010) behandelt.
</div>

Mit unseren Messungen versuchen wir auf eine Grundgesamtheit zu schliessen. Dabei sind unsere Anhaltspunkte die gemessenen Merkmale unserer Entitäten. 

Unsere Hoffnung ist immer, dass unsere Messung die Verteilung der Grundgesamtheit widerspiegelt. Unsere Messungen sollten also der Verteilung der vermuteten Grundgesamtheit ähneln. Weil wir aber immer mit Ungenauigkeiten kämpfen müssen, vergleichen wir unsere Messungen mit bekannten Verteilungen. 

<div class="alert alert-primary" markdown=1>
**Definition:** Eine *statistische Verteilung* beschreibt eine *Funktion*, aus der wir die Wahrscheinlichkeit ablesen können, mit der eine Merkmalsausprägung einen bestimmten Wert annimmt. 
</div>

Im Kontrast dazu ist bildet eine *empirische Verteilung* die absolute oder relative Häufigkeit ab, mit der eine Merkmalsausprägung in einer Stichprobe gemessen wurde. 

Wir verwenden statistische Verteilungen als Referenzen, um unsere empirischen Verteilungen zu beurteilen. 

Vier statistische Verteilungen sind in dieser Lehrveranstaltung relevant. 

1. Die Normalverteilung
2. Die t-Verteilung
3. Die \\( \chi ^2 \\)-Verteilung
4. Die F-Verteilung

### Normalverteilung

Die [Normalverteilung haben wir bereits kennengelernt](https://moodle.zhaw.ch/mod/page/view.php?id=418588). Die Normalverteilung hilft uns bei der Analyse von Merkmalsausprägungen, die **um einen Zentralwert gestreut** sind.

### t-Verteilung

Die t-Verteilung ähnelt der Normalverteilung. Sie schliesst zusätzlich die Stichprobengrösse in die Verteilung mit ein. Entsprechend hat die t-Verteilung einen zusätzlichen Parameter. Dieser wird mit *Freiheitsgrade* bezeichnet. Wird die Stichprobengrösse sehr gross, dann geht die t-Verteilung in die Normalverteilung über.

### \\( \chi^2 \\)-Verteilung

Die \\( \chi^2 \\)-Verteilung (gesprochen: chi-quadrat oder englisch: chi-squared) ist eine statistische Verteilung, die die Wahrscheinlichkeit eines Ergebnisses aus einer Zufallsstichprobe mit einem gegebenen Umfang bestimmt. Die \\( \chi^2 \\)-Verteilung erlaubt es uns die **Häufigkeitsverteilung** von Stichproben zu beurteilen. 

Die \\( \chi^2 \\)-Verteilung ist ebenfalls abhängig vom Stichprobenumfang und erwartet ebenfalls die Freiheitsgrade der Stichprobe. Mit immer grösserem Stichprobenumfang geht diese Verteilung in eine Normalverteilung über.

### F-Verteilung

Die F-Verteilung ist eine statistische Verteilung die die **Varianz** von **zwei Stichproben** vergleicht. Sie bestimmt die Wahrscheinlichkeit die vorgefundene Varianz unter der Annahme, dass beide Stichproben aus der gleichen Grundgesamtheit stammen. 

Die F-Verteilung hängt von den Freiheitsgraden der beiden Stichproben ab. Wird der Umfang und damit die Freiheitsgrade beider Stichproben sehr gross, geht die F-Verteilung in eine Normalverteilung mit dem Mittelwert 1 über. 

## Freiheitsgrade

Viele statistische Funktionen bilden den Stichprobenumfang als *Freiheitsgrade* ab. Unter Freiheitsgraden verstehen wir die Anzahl der zufälligen Ziehungen, die zu einem bestimmten Ergebnis führen. Ist der Stichprobenumfang gleich 1, dann haben wir immer genau einen Weg, um zu einem Ergebnis zu gelangen. Mit anderen Worten, es gibt *keine* Freiheit, um ein Ergebnis zu erzeugen. Wächst unsere Stichprobe, so erhalten wir immer eine zusätzliche Variationsmöglichkeit mit jeder zusätzlich untersuchten Entität.

Mit den Freiheitsgraden ändert sich die Wahrscheinlichkeitsfunktion. Haben wir keine Freiheiten, dann ist die Wahrscheinlichkeit ein bestimmtes Ergebnis zu erhalten 1. Diese Wahrscheinlichkeiten ändern sich mit dem Umfang unserer Stichproben. Diese Änderungen sind bei wenigen Freiheitsgraden oft sehr gross. Werden unsere Freiheitsgrade immer grösser, dann nähern sich viele statistische Verteilungen immer mehr der Normalverteilung an. Oft haben kleine Unterschiede im Stichprobenumfang bei grossen Stichproben kaum noch Auswirkungen auf die jeweilige statistische Verteilung. Diese grossen Auswirkungen von kleinen Freiheitsgraden auf die jeweilige statistische Verteilung sind der Grund, warum wir ausreichend grosse Stichproben anstreben.

<img src="https://github.com/dxiai/statistik/raw/main/bilder/norm_vis/chisq_changes.png" width="60%">

*Abbildung: Verlauf der \\( \chi^2 \\)-Verteilung bei unterschiedlichen Freiheitsgraden*

$$ $$
