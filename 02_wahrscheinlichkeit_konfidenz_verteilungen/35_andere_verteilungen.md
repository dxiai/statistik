# Andere wichtige Verteilungen

<div class="alert alert-info" markdown="1">
Für jede Verteilung stellt R vier Funktionen bereit. 

* Die `d`-Funktion für die Dichte der Verteilung an einer bestimmten Stelle der Verteilung. Dieser Wert zeigt für eine Stelle in der Verteilung, wie wahrscheinlich dieser Wert für die Verteilung ist. 
* Die `p`-Funktion für die kummulative Verteilung an einer bestimmten Stelle. Dieser Wert zeigt die Wahrscheinlichkeit für alle Werte grösser oder gleich der angegebenen Stelle in der Verteilung an. 
* Die `q`-Funktion oder Quantilsfunktion, zeigt für eine Wahrscheinlichkeit die jeweilige Quantilsgrenze in der Verteilung an. Es ist die Umkehrfunktion der jeweiligen `p`-Funktion. 
* Die `r`-Funktion zieht aus der jeweiligen Verteilung einen zufälligen Wert. Bei wiederholten Ziehen nähern sich die Werte der Verteilung an. Diese Funktion ist für Simulationen wichtig!
</div>

Für den Zweck von Simulationen in dieser Lehrveranstaltung unterscheiden wir zwischen den folgenden Verteilungen: 

- Gleichverteilung
- Normalverteilung
- Binomialverteilung
- Expoentialverteilung 

Bei der **Gleichverteilung** sind alle Werte in einem Wertebereich gleich wahrscheinlich. Zufallswerte aus dieser Verteilung sind mehr oder weniger gleichmässig verteilt. Die Gleichverteilung wird immer dann verwendet, wenn für eine Variable ***keine*** zentrale Tendenz erwartet wird. Die Zufallsfunktion der Gleichverteilung ist `runif()`.

Bei der **Normalverteilung** sind Werte nah am Mittelwert wahrscheinlicher als Werte, die weiter vom Mittelwert entfernt sind. Die Normalverteilung ist symetrisch um einen Mittelwert mit einer gegebenen Standardabweichung.

Wir verwenden die Normalverteilung immer dann als Ausgangsverteilung, wenn wir metrische Werte simulieren möchten, bei denen ein bekannter Mittelwert das Maximum der Verteilung ist und extreme Abweichungen von diesem Mittelwert immer unwahrscheinlicher sind. Deshalb wird die Normalverteilung auch gerne zur Simulation zufälliger Fehler gewählt, bei denen kleine Abweichungen vom "echten" Wert häufiger Vorkommen als exteme Abweichungen.

Wir steuern die Verteilung über zwei Parameter: Den Mittelwert und die Standardabweichung. Über den Mittelwert legen wir das Zentrum der Verteilung fest und über die Standardabweichung definieren wir, wie breit die Werte um den Mittelwert gestreut sind. Dabei gilt, je grösser die Standardabweichung, je breiter wird die Verteilung. 

<!--
Beispiel für eine Normalverteilung mit \\(\mu = 5 \\) und \\(sd = 3\\).
![Beispiel für eine Normalverteilung mit \\(\mu = 5 \\) und \\(sd = 3\\).]()
-->

Die Zufallsfunktion der Normalverteilung ist `rnorm()`.

Die **Binomialverteilung** ist eine Verteilung, bei der nur diskrete Werte möglich sind. Die Binomialverteilung leitet sich aus der Wiederholung gleichartiger Zufallsexperimente mit genau zwei Ergebniszuständen (0,1). Die Form der Binomialverteilung ergibt sich aus der Anzahl der Wiederholungen (\\( s \\)) und der Wahrscheinlichkeit ( \\( p \\) ), dass bei diesen Experimenten das grössere der beiden Ergebnisse eintritt. 

Weil die Binomialverteilung nur diskrete Werte > 0 annehmen kann, eignet sie sich für die Simulation ordinalskalierter Daten. 

Die Anzahl der Wiederholungen (\\( s \\)) entspricht bei der Simulation dem maximal möglichen Wert einer ordinalskalierten Variable. 

Wird die Wahrscheinlichkeit mit `0.5` gewählt, dann sind beide Ergebnisse sind gleich wahrscheinlich. In diesem Fall ist die Binomialverteilung *symmetrisch*. Ist die Wahrscheinlichkeit < 0.5, dann ist die Verteilung rechtsschief und das Maximum der Verteilung tendiert zum unteren Skalenende. Ist die Wahrscheinlichkeit > 0.5, dann ist die Verteilung linksschief und das Maximum der Verteilung tendiert zum oberen Ende der Skalierung. 

Die sog. **Binärverteilung** ist ein Spezialfall der *Binomialverteilung* mit \\( s = 1 \\).

Die Zufallsfunktion der Binomialverteilung ist `rbinom()`.

Die letzte wichtige Verteilung ist die **Exponentialverteilung**. Die Exponentialverteilung ist eine asymetrische Verteilung mit Ihrem Maximum bei `0` und sie ist nur für Werte > 0 definert. Für grössere Werte läuft diese Verteilung asymptotisch gegen 0, d.h. grössere Werte werden immer unwahrscheinlicher. Die Exponentialverteilung ist über einen *Dämpfungsparameter* definiert. Je kleiner die Dämpfung, desto langsamer geht die Verteilung gegen 0.

Die Zufallsfunktion der Exponentialverteilung ist `rexp()`.

Die Geometrische Verteilung ist die diskrete Version der Exponentialverteilung. Der Dämpfungsparameter wird hier durch einen Wahrscheinlichkeitswert zwichen 0 und 1 ersetzt. Der Effekt auf die Verteilung ist aber der gleiche: Je grösser die Dämpfung, desto schneller läuft die Verteilung gegen 0.

Die Zufallsfunktion der geometrischen Verteilung ist `rgeom()`.

Eine besondere Verteilung, die sich ähnlich wie die geometrische Verteilung verhält ist die sog. negative Binomialverteilung. Während die geometrische Verteilung gleichmässig gegen `0` abfällt, konzentrieren sich die Werte bei der negativen Binomialverteilun bei den niedrigen Werten. Damit fällt die Verteilung schneller ab, läuft dann jedoch etwas langsamer gegen `0`.  Diese Verteilung ist für die Simulation sog. *Longtail* Probleme interessant.

Die Zufallsfunktion der negativen Binomialverteilung ist `rnbinom()`.

$$ $$ 