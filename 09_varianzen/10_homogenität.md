# Homogenität und Heterogenität


<div class="alert alert-info" markdown=1>
Dieser Abschnitt ergänzt das Kapitel 8 in Bortz und Schuster (2010). Insbesondere Abschnitt 8.2  und 8.6.1. 
</div>

Die Homogenität ist über die Varianz einer Stichprobe definiert. Damit kann die Homogenität *ausschliesslich* für metrischskalierte Variablen bestimmt werden. Die Homogenität ist kein absolutes sondern ein relatives Konzept. Es wird deshalb immer eine Referenzgrösse benötigt, um die Homogenität zu beurteilen. 

Eine Stichprobe wird als *homogen* bezeichnet, wenn ihre Varianz klein ist. Eine homogene Stichprobe ist also nicht breit um ihren Mittelwert gestreut. Mit anderen Worten: Die Werte der untersuchten Variable ähneln sich stark. 

Im Gegensatz dazu wird eine Stichprobe als *heterogen* bezeichnet, wenn die Varianz gross ist. D.h. die Stichprobe streut breiter um ihren Mittelwert. Mit anderen Worten: Die Werte der untersuchten Variable ähneln sich kaum. 

Es gibt zwei Fälle der Homogenität, die für uns von besonderer Bedeutung sind. 

1. Die unterschiedlichen Varianzen von Stichproben.
2. Bei gerundeten Werten in sehr homogenen Stichproben.

### Fall 1: Die unterschiedlichen Varianzen von Stichproben.

<div class="alert alert-primary" markdown="1">
**Definition:** Unterscheiden sich die Varianzen von Stichproben nicht, dann sprechen wir von **homogenen Stichproben zur Gesamtstichprobe**.
</div>

In diesem Fall für zwei Stichproben mit den Varianzen \\( \sigma^2_1 \\) und \\( \sigma^2_2\\)  gilt: 

$$ 
\sigma^2_1 = \sigma^2_2 
$$

<div class="alert alert-primary" markdown="1">
**Definition:** Wenn die Varianzen zweier Stichproben voneinander abweichen, dann sprechen wir von **heterogenen Stichproben zur Gesamtstichprobe**. 
</div>

Entsprechend gilt für diesen Fall umgekehrt 

$$ 
\sigma^2_1 \ne \sigma^2_2 
$$

Weil wir die Standardabweichnung der Grundgesamtheit in der Regel nicht kennen, *approximieren* wir diese Unterschiede über die Standardabweichungen \\( s \\) der Stichproben. Um Unterschiede zwischen Stichproben zu bestätigen, müssen wir neben dem Mittelwert zusätzlich die Varianz der Stichproben untersuchen. Dabei kommt der Grenzwertsatz in einer Variation zur Anwendung: Zwei Stichproben stammen wahrscheinlich aus der gleichen Grundgesamtheit, wenn sie homogen zueinander sind. Ob die Homogenität zwischen Stichproben gegeben ist, prüfen wir mit dem [F-Test bzw. Varianztest](https://moodle.zhaw.ch/mod/page/view.php?id=418495).

### Fall 2:  Gerundete Werte in sehr homogenen Stichproben.

Ein ganz anders gelagertes Problem ergibt sich bei der Verwendung von gerundeten Werten in metrischskalierten Variablen. Ist eine Variable einer Stichprobe sehr homogen, dann verlieren die gerundeten Werte bei der statistischen Analyse ihre Eigenschaft des metrischen Skalenniveaus und verhalten sich nur noch wie *ordinalskalierte* Variablen. 

**Wann ist eine Variable sehr homogen?** 

Eine Variable wird als sehr homogen bezeichnet, wenn die Werte sehr eng um den Mittelwert fallen. Dadurch verringert sich der Wertebereich der Variablen. Normalerweise ist das bei metrischskalierten Variablen kein Problem *solange* die Werte **ungerundet** vorliegen. 

Werden die Werte  gerundet, dann fallen Werte in "Kategorien". Ist die Stichprobe nicht zu homogen, dann kann auch dieser Effekt ignoriert werden. Bei sehr homogenen Stichproben entstehen jedoch Probleme, wenn innerhalb der ersten Standardabweichung weniger als 5-7 Werte nach Rundung vorkommen können. In diesen Fällen können die statistischen Effekte überschätzt werden. In solchen Fällen sollte die ansonsten metrischskalierte Variable wie eine ordinalskalierte Variable behandelt werden.

**Warum sollte man sich darum kümmern?**

Bestimmte demographische Werte, wie z.B. das Alter, werden als gerundete Werte erhoben. Gerundete Altersangaben sind in der Regel kein Problem und können wie eine metrischskalierte Variable behandelt werden. Bei sog. Kohortenstudien kann das allerdings nicht zutreffen: Kohortenstudien gruppieren Teilnehmende nach Jahrgängen oder nach Generationen. Dadurch werden die für gerundete Altersangaben übliche Varianz der Gesamtpopulation  stark eingeschränkt. 

Gelegentlich können durch die Stichprobenauswahl ungeplant in einer Studie auftreten. Daher ist es bei gerundeten metrischskalierten Variablen notwendig, eine zu hohe Homogenität auszuschliessen.
