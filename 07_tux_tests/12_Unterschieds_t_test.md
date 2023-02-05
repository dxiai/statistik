# t-Test für Gruppenvergleiche


<div class="alert alert-info" markdown=1>
Im Skript sowie in Bortz und Schuster (2010) wird ausführlich auf den t-Test für *homogene Varianzen* und anschliessend auf den t-Text für *heterogene Varianzen* eingegangen. In dieser Anmerkung gehe ich auf den Begriff der Homogenität und der Bedeutung der homogenen Varianz für den t-Test ein.
</div>

### Homogenität

In der Statistik wird der Begriff *homogen* immer dann verwendet, wenn Werte eng um einen Referenzwert gestreut sind. Das Gegenteil zu homogen ist *heterogen*. Dieser Bedeutet, dass Werte nicht eng um einen Referenzwert gestreut sind. 

<div class="alert alert-primary" markdown=1>
Wir sprechen von einer *homogenen Stichprobe*, wenn alle Werte einer Variable um eine bestimmte Merkmalsausprägung fallen. 
</div>

Zur Berechnung des t-Tests für unabhängige Stichproben wurde traditionell eine Formel verwendet, die sich wesentlich leichter berechnen lässt. Damit diese einfache Formel verwendet werden darf, muss jedoch sichergestellt sein, dass eine *homogene Varianz* der beiden Stichproben vorliegt. Um diesen Begriff zu verstehen, müssen wir uns daran erinnern, dass die Varianz einer Stichprobe gleich dem Quadrat der Standardabweichung ist. Deshalb wird die Varianz immer mit \\( \sigma^2 \\) für die tatsächliche Standardabweichung der Grundgesamtheit bzw. mit \\( s^2 \\) für die Standardabweichung der Stichprobe bezeichnet. 

<div class="alert alert-primary" markdown=1>
Eine *homogene Varianz* zweier Stichproben ist unter der Annahme gegeben, dass \\( {s_1}^2 \approx {s_2}^2 \\) gilt. 
</div>

### t-Test für homogene Stichproben

Unter diesen Bedingungen lässt sich die Formel zur Berechnung des t-Werts stark vereinfachen und damit leichter berechnen. Der Nachteil dieser Formel ist allerdings, dass wir zusätzlich nachweisen müssen, dass die Annahme der homogenen Varianz für die gegebenen Stichproben auch gültig ist. Entsprechend ist ein vorgelagerter **F-Test** zwingend notwendig. Dieser Test wird im Abschnitt 8.2.1 in Bortz und Schuster (2010, S. 122f) ausführlich diskutiert.

### t-Test für allgemeine Stichproben

In der Literatur wird immer vom *t-Test für heterogene Stichprobenvarianzen* geschrieben. Streng genommen verweist diese Bezeichnung auf eine rechnerische Praxis und nicht auf eine Entweder-Oder-Beziehung. Vielmehr ist der t-Test für homogene Varianzen eine spezielle Variante des t-Tests für heterogene Varianzen. Deshalb ist der Titel *t-Test für allgemeine Stichproben* meiner Meinung nach aussagekräftiger. 

In Statistiklehrbüchern wird der t-Test für homogene Stichproben oft zuerst beschrieben, weil dieser mathematisch einfacher nachzuvollziehen ist als die allgemeinere Fassung nach Welch/Satterthwait. Die allgemeinere Fassung setzt die Varianz der Stichproben nicht gleich, sondern berücksichtigt die Varianzen und Freiheitsgrade der beiden Stichproben beim Test.

### t-Test in der R-Praxis

In der Praxis mit R arbeiten wir in der Regel mit der allgemeinen Variante des t-Tests und vermeiden das Gleichsetzen der Varianzen. R wählt automatisch diese Vorgehensweise. In der Praxis verwenden wir daher meistens den t-Test für heterogene Varianzen, weil dieser ausser der Voraussetzung der hinreichenden Normalverteilung *beider* Stichproben keine weiteren Bedingungen an die Stichproben stellt. 

Trotzdem können wir die Funktion `t_test()` dazu veranlassen, die Variante für homogene Stichproben zu verwenden. Dazu übergeben wir der Funktion den zusätzlichen Parameter `var.equal = TRUE`. Diese Variante verwenden wir, um Beispiele aus Lehrbüchern zu kontrollieren oder falls wir mit einer Methode arbeiten, die explizit nach dem t-Test für homogene Stichprobenvarianzen verlangt. In solchen Fällen dürfen wir nicht vergessen, dass wir die Varianzhomogenität analog zur Normalverteilung sicherstellen müssen, bevor wir diesen Testvariante anwenden. 

$$ $$