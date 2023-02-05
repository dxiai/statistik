
# Kausale Operationalisierung mit DAGs

<div class="col-12 alert alert-primary" markdown=1>
Ein DAG ist ein gerichteter Graph, mit dessen Hilfe die theoretischen Kausalzusammenhänge beschrieben werden. 
</div>

Vertiefende Literatur: 

* Pearl, J., & Mackenzie, D. (2018). The book of why: the new science of cause and effect.  Penguin Press. 



Die Beantwortung von Forschungsfragen steht im Zentrum des wissenschaftlichen Arbeitens. Die Statistik ist dafür ein wichtiges Hilfsmittel, um Annahmen mit Hilfe von Hypothesen zu überprüfen. Der schwierigste Teil der Vorbereitung von empirischen Studien ist die Auswahl geeigneter Hypothesen. 

Ein wichtiges Werkzeug zum Operationalisieren von empirischen Studien sind sog. DAG  (Pearl & Mackenzie, 2018). Ein DAG ist ein einfaches und gleichzeitig sehr leistungsstarkes Werkzeug zur Analyse eines Studiendesigns mit Bezug auf die zu erwartenden statistischen Effekte. Wir können also mit Hilfe eines DAGs viele Aussagen über die statistischen Ergebnisse machen bevor wir Daten erhoben haben.   

<p class="alert alert-primary" markdown="1">
**Definition:** Ein **DAG** ist ein *gerichteter azyklischer Graph* (directed acyclic graph), mit welchem die (vermuteten) kausalen Beziehungen zwischen Variablen festgehalten werden. 
</p>

Ein DAG besteht aus sog. *Konten* und *Beziehungen* zwischen den Knoten. Ein Knoten sind immer benannt. Gelegentlich werden Knoten als Kreis dargestellt. Eine Beziehung beschreibt die kausale Wechselwirkung zwischen zwei Knoten und wird als Pfeil dargestellt. Eine Beziehung hat immer genau einen Ausgangsknoten und einen Zielknoten. Die Pfeilspitze zeigt dabei immer auf den Zielknoten. Der Ausgangsknoten bezeichnet eine Ursache für die erwarteten oder beobachteten Werte des Zielknotens (die Wirkung).

![Beispiel für ein DAG](https://github.com/dxiai/statistik/raw/main/bilder/var_op_complex_b.png)

Jeder Knoten kann gleichzeitig Ausgangspunkt und Ziel von Beziehungen mit mehreren Knoten sein. Ein Knoten symbolisiert die Summe der Ursachen. Grundsätzlich gilt dabei, dass alle Werte für alle Knoten **gleichzeitig** vorliegen. Folgt man den Pfeilen eines DAG, so ist es verboten, dass man zu einem Knoten zurückkehren kann. Dadurch ist sichergestellt, dass ein DAG keine Zyklen enthält. Es ist also nicht möglich Rückkopplungen zwischen Knoten mit einem DAG zu modellieren.

<div class="alert alert-secondary" markdown="1">
**Exkurs:** Zyklische Modelle sind in der Realität weit verbreitet. Diese Modelle werden als *Systeme* bezeichnet. Die *Systemtheorie* (Meadows, 2015) beschäftigt sich mit der Modellierung und Untersuchung komplexer Systeme. Diese Systeme sind dabei konzeptionelle Erweiterungen von DAG, wobei mehrere Teilmodelle über "Verzögerungen" verbunden werden. In einem Systemmodell liegen die Werte für alle Knoten ***nicht gleichzeitig*** vor.
</div>

Mit einem DAG wird das sog. *kausale* Modell des Untersuchungsgegenstands dokumentiert. Es handelt sich um die *theoretische Voraussetzung* der geplanten empirischen Untersuchung. Ein kausales Modell beschreibt die erwarteten und bekannten Wechselwirkungen zwischen zu messenden aber auch nicht messbaren Aspekten. Ein DAG repräsentiert *unsere Vorstellung* des Untersuchungsgegenstands, mit der wir die durchzuführenden statistischen Tests begründen und überprüfen. 

Ein DAG kann auf Validität geprüft werden, indem für jeden Knoten geprüft wird, ob über das Verfolgen der Beziehungen ausgehend vom Knoten der Knoten wieder erreicht werden kann. 

Das folgende Beispiel zeigt ein ungültiges DAG. 

![Beispiel für ein ungültiges DAG](https://github.com/dxiai/statistik/raw/main/bilder/invalid_dag.png)

## Ein Beispiel in R 

<div class="alert alert-info" markdown="1">
**Automatisierung:** In R können wir mit der ``dagitty`` Bibliothek beliebig komplexe DAG modellieren und analysieren. Diese Modelle können wir mit Hilfe der Bibliothek ``ggdag`` visualisieren. 
</div>

In R beschreiben wir diese *additive* Beziehung mit Hilfe eines sog. *linearen Modells*. Zum Beispiel nehmen wir an, dass wir das Gewicht von Kindern untersuchen möchten. Wir wissen, dass das Gewicht sowohl vom Alter als auch vom Geschlecht beeinflusst wird. Dieser Einfluss beschreibt die kausale Beziehung von Alter ***und*** Geschlecht auf das Gewicht. Diese Beziehung beschreiben wir in R mit der Modellformel: ``Gewicht ~ Alter + Geschlecht ``. Wir lesen diese Formel wie folgt: "Die Werte in der Variable `Gewicht` folgen den Werten der Variablen `Alter` und `Geschlecht`.  

Wenn wir unser Modell erweitern und die Rechtschreibung der Kinder einbeziehen, dann können wir das mit der folgenden Modellformel erreichen: ``Rechtschreibung ~ Alter + Geschlecht``. 

Der vollständige R-Code lautet wie folgt

```
library(tidyverse)
library(dagitty)
library(ggdag)

dagify(
    Gewicht ~ Alter + Geschlecht,
    Rechtschreibung ~ Alter + Geschlecht
) -> DAGModell

DAGModell %>%
  ggdag(text = F, use_labels = "name") +
      theme_dag()
```

![DAG für die Kausalbeziehungen von Gewicht und Rechtschreibung](https://github.com/dxiai/statistik/raw/main/bilder/DAG_Kausalbeziehungen.png)

Dieses DAG liefert uns bereits viele wichtige Informationen für eine empirische Studie.

1. Die kausale Beziehung  zwischen Alter und Gewicht legt einen *Zusammenhang* zwischen diesem beiden Variablen nahe. Statistisch ist ein Zusammenhang ungerichtet (s. Sitzung 4). Erst durch das DAG wird die Richtung des Zusammenhangs deutlich.
2.  Die kausale Beziehung zwischen Geschlecht und Gewicht legt ebenfalls einen Zusammenhang zwischen diesen beiden Variablen nahe. Dieser Zusammenhang sollte sich über einen *Unterschied* des Gewichts zwischen den Geschlechtern zeigen (s. Sitzung 5). 
1. Die kausale Beziehung  zwischen Alter und Rechtschreibung legt einen *Zusammenhang* zwischen diesem beiden Variablen nahe. Statistisch ist ein Zusammenhang ungerichtet (s. Sitzung 4). Erst durch das DAG wird die Richtung des Zusammenhangs deutlich.
2.  Die kausale Beziehung zwischen Geschlecht und Rechtschreibung legt ebenfalls einen Zusammenhang zwischen diesen beiden Variablen nahe. Dieser Zusammenhang sollte sich über einen *Unterschied* des Gewichts zwischen den Geschlechtern zeigen (s. Sitzung 5).  

Jeder dieser Punkte liefert uns eine empirisch überprüfbare Hypothese. Wichtig ist jedoch ein weiterer Punkt, der implizit in unserem DAG steht: Die Werte der Variablen Rechtschreibung und Gewicht sollten *korrelieren*, weil sie die gleichen Ursachen haben. D.h. wenn wir die Werte der beiden Variablen in einem Punktdiagramm gegenüberstellen sollten die Punkte mehr oder weniger einer Linie folgen. Unser DAG zeigt aber keinen Pfeil, der Rechtschreibung und Gewicht verbindet und eine solche Beziehung begründen würde. Diese Beziehung fehlt, weil wir keinen Grund haben, eine ursächliche Beziehung zwischen Rechtschreibung und Gewicht anzunehmen. 

<p class="alert alert-success" markdown="1">
**Merke:** Zwei Variablen werden korrelieren, wenn sie die gemeinsame kausale Ursachen haben. 
</p>




### Referenzen

* Meadows, D.H. (2015). Thinking in Systems. Chelsea Green Publishing House
* Pearl, J., & Mackenzie, D. (2018). The book of why: the new science of cause and effect.  Penguin Press. 
 


