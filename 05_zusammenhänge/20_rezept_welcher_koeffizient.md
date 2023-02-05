# Welchen Koeffizienten muss ich wählen: Cramers V, Kendall, Spearman oder Pearson?

Im [Entscheidungsbaum] sind die Auswahlen ebenfalls festgehalten.

* Bestimmen Sie *Cramers V*, wenn Sie *nominalskalierte Variablen* untersuchen.
* Bestimmen Sie *Kendalls \\( \tau \\)*, wenn Sie *ordinalskalierte Variablen* einer kleinen Stichprobe (n < 50) untersuchen.
* Bestimmen Sie *Spearmans \\( \rho \\)*, wenn Sie *ordinalskalierte Variablen* einer grossen Stichprobe (n >= 50) untersuchen.
* Bestimmen Sie *Pearsons \\(r\\)*, wenn Sie *metrischskalierte Variablen* untersuchen.

<p class="alert alert-info" markdown="1">
Für grosse Stichproben konvergieren die Koeffizienten nach Kendall und Spearman. Die Methode für Spearmans \\( \rho \\) lässt sich schneller berechnen, hat aber bei kleinen Stichproben (n < 50) den Nachteil, dass damit Korrelationen überschätzt werden. Das bedeutet, dass Sie womöglich Korrelationen feststellen, die es eigentlich nicht gibt. Das Verfahren für Kendalls \\( \tau \\) hat dieses Problem nicht, ist aber für grosse Stichproben (etwas) langsamer.
</p>

<p class="alert alert-secondary" markdown="1">
Mit aktuellen Computern werden Sie bei üblichen soziometrischen Untersuchungen mit Stichprobenumfängen mit n < 100000 wahrscheinlich keinen Unterschied in der Berechnungsgeschwindigkeit beim Spearman- und der Kendall-Verfahren spüren.
</p>

$$ $$