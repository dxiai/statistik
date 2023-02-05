# Spezialfall der Normalverteilung: Die Sättigung

Das Konzept der Sättigung ist eng mit der Normalverteilung verbunden. 

<p class="alert alert-primary">
Die Sättigung beschreibt den Anteil eines Merkmals in einer Grundgesamtheit.
</p> 

Das Konzept ist in vielen Bereichen von Bedeutung, weil sich damit sogenannte *Übernahmeprozesse* oder *Phasenübergänge* beschreiben lassen. Der zeitliche Verlauf solcher Prozesse ähnelt oft der *Verteilungsfunktion* der Normalverteilung. 

Die Verteilungsfunktion ist eine stetige Funktion im Ergebnisbereich zwischen 0 und 1. Die Funktion verläuft asymptotisch gegen 0 für kleine Werte und asymptotisch gegen 1 für grosse Werte. Der Wendepunkt der Funktion ist der Mittelwert.

Mathematisch hängt die Verteilungsfunktion mit der bekannten Glockenkurve der Normalverteilung so zusammen. Die Glockenkurve ergibt sich aus der sog. *Dichtefunktion*  der Normalverteilung. Die Dichtefunktion der Normalverteilung ist die *erste Ableitung* der Verteilungsfunktion.

In R rufen wir die Dichtefunktion mit der Funktion `dnorm()` und die Verteilungsfunktion mit der Funktion `pnorm()` auf.  

<p class="alert alert-success">Prägen Sie sich den Verlauf der Verteilungsfunktion einer normalverteilen Variablen ein!</p>

<img src="https://github.com/dxiai/statistik/raw/main/bilder/norm_vis/norm_cumulfunc.png" width="600"> 

*Abbildung: Verteilungsfunktion (rot) und Dichtefunktion (schwarz) der Normalverteilung im Vergleich*

<div class="col-12 text-center">
<iframe width="560" height="315" src="https://www.youtube.com/embed/eSxW-QcAhlU" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>
