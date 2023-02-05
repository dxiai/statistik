## Variablen und Skalenniveaus

<div class="col-12 alert alert-success" markdown=1>
<i class="fa fa-lg fa-info-circle"></i> Dieser Abschnitt gibt eine Kurzübersicht über das Kapitel 1.2 **Skalenniveaus** in *Bortz und Schuster*.</div>

### Variablen

<div class="col-12 alert alert-primary" markdown=1>
Ein *gemessenen* Merkmal einer Entität bezeichnen wir als **Variable**. Variablen unterscheiden sich von *Konstanten* und *Identifikatoren* (engl. *identifier*) durch gemessene bzw. messbare Unterschiede.
</div>

Eine Konstante hat für jede Entität den gleichen Wert. Ein *Identifikator* (engl. *identifier*) erlaubt uns zwei Entitäten in unserer Stichprobe zu unterscheiden. Diese Werte beschreiben aber keine gemessenen Merkmale.

Die Daten einer Stichprobe legen wir in einem *Stichprobenobjekt* ab. Die empirischen Variablen entsprechen den technischen Vektoren. Die *Variable* steht dabei für das jeweilig variierende **Merkmal**. Der zugehörige *Vektor* enthält die tatsächlich **gemessenen Werte**.

<div class="col-12 alert alert-primary" markdown=1>
Die für eine Variable möglichen Werte werden als **Merkmalsausprägung** bezeichnet. 
</div>

### Skalenniveaus

Jede Variable hat ein Skalenniveau. Das Skalenniveau legt fest, welche Werte in unseren Vektoren vorkommen können. Das Skalenniveau wird durch das jeweilige Messinstrument bestimmt und steht schon vor einer Messung fest. 

Durch das Skalenniveau wird vorweg genommen, welche statistischen Tests wir durchführen dürfen. 

<div class="col-12 alert alert-warning" markdown=1>
Wenn wir einen Fragebogen oder eine Beobachtung selbst konzipieren legen wir für jedes zu messende Merkmal das Skalenniveau selbst fest! 

Wir müssen für jede Variable das Skalenniveau dokumentieren!
</div>

Wir unterscheiden in der Praxis werden nicht-metrische und metrische Skalenniveaus unterschieden. Die nicht-metrischen Skalenniveaus sind:

* **Nominalskalierung**
* **Ordinalskalierung**

Die nicht-metrischen Skalenniveaus werden ausschliesslich durch *diskrete* Werte beschrieben. 

In der Literatur werden die metrischen Skalen noch weiter unterteilt. Dabei werden folgenden beiden Skalenniveaus unterschieden. 

* ***Intervallskalierung***
* ***Varianzskalierung***

#### Nominalskalierung

Bei der Nominalskalierung werden Merkmalsausprägungen nur unterschieden, indem die verschiedenen Merkmalsausprägungen *benannt* werden. 

Dabei gilt, dass eine Variable nur einen der möglichen Werte annehmen kann. Das wird in durch den senkrechten Strich im Sinne des *logischen Oder* symbolisiert (A | B).

Ausserdem gilt, dass unterschiedliche Werte auf einem nominalen Skalenniveau durch die *ungleich* Beziehung beschrieben werden (A ≠ B).

#### Ordinalskalen

Die Ordinalskalierung erweitert die Nominalskalierung durch eine zusätzliche *Ordnung* bzw. Reihenfolge der Werte eines Skalenniveaus. 

Auf diesem Niveau sind also zusätzlich Grösser-Kleiner-Beziehungen zwischen den Werten festgelegt. 

<p class="alert alert-warning" markdown=1>
Die Werte von nominalskalierten und ordinalskalierten Variablen beschreiben nur die Beziehungen der Werte zueinander. Es dürfen deshalb nur mathematische Operationen verwendet werden, die diese Beziehungen nicht verändern. Dazu gehören Vergleiche und Logische Operationen. Arithmetische Operationen wie `+`, `-`, `•` oder `/` sind für nicht-metrische Skalenniveaus nicht zulässig!
</p>

#### Intervallskalierung

Die Intervallskalierung beschreibt stetige (nicht-diskrete Werte), bei denen die Abstände zwischen Werten des gleichen Merkmals gleich sind. Die Stetigkeit der Werte legt ausserdem fest, dass graduelle Übergänge zwischen den den Messwerten möglich sind.

Auf dieser Ebene lassen sich Ausprägungen des gleichen Merkmals, die mit unterschiedlichen Skalierungen gemessen wurden vergleichen (z.B. Temperatur in °C oder °F), auf wenn die eigentlichen Werte unterschiedlich sind. 

Hier wird ein **willkürlicher Nullpunkt** für die Skalierung angenommen. 

#### Varianzskalierung

Bei der Varianzskalierung kommt zu den Merkmalen der Intervallskalierung hinzu, dass die nicht nur die Abstände, sondern auch die Verhältnisse zwischen den Merkmalsausprägungen vergleichbar sind. (das ist bei Temperaturmessungen mit °C und °F mit willkürlichen Nullpunkt nicht möglich). 

In der Literatur wir hier ein echter Nullpunkt als Kriterium für die Verhältnisse angegeben. Dieser ergibt sich jedoch als Folge der gleichbleibenden Verhältnisse.

<p class="alert alert-warning" markdown=1>
In der wirtschaftswissenschaftlichen Praxis werden metrische Skalierungen meistens als varianzskalierte Merkmale behandelt.  
</p>

### Verbindungen zwischen den Skalenniveaus

Die Skalenniveaus sind hierarchisch organisiert. Das bedeutet, dass alle Eigenschaften und Operationen des allgemeineren Skalenniveaus auch für die spezielleren Skalenniveaus gelten. 
Das folgende Diagramm zeigt die Abhängigkeiten der Skalenniveaus. 

<img src="https://raw.githubusercontent.com/dxiai/statistik/main/bilder/Skalen_hierarchie.svg" width="800"/>

### Orientierung Skalenniveaus

Das folgende Diagramm bietet eine Orientierung über die Skalenniveaus und ihre Eigenschaften.

<img src="https://raw.githubusercontent.com/dxiai/statistik/main/bilder/Skalen.svg" />

Wir wählen für ein Merkmal das zugehörige Skalenniveau entsprechend der Analysen aus, die wir zur Beantwortung unserer Fragestellung verwenden möchten. D.h. wenn wir eine Normalverteilung nachweisen möchten, dann müssen unsere Werte auf einem metrischen Skalenniveau vorliegen.

### Verteilungen und Skalenniveaus

Das Skalenniveau legt die möglichen Werte für unsere Messungen fest. In einer Stichprobe erhalten wir für jede Entität einen Wert für eine Merkmalsausprägung. Diese Werte sind innerhalb der Stichprobe über das Skalenniveau verteilt. Das Skalenniveau und Verteilungen von Messwerten sind also direkt aneinander gekoppelt. 

In der Statistik interessieren uns die gemessenen Verteilungen. Statistische Fragestellungen beantworten wir über den Vergleich von gemessenen Verteilungen mit statistischen Verteilungen (bzw. Referenzverteilungen).





